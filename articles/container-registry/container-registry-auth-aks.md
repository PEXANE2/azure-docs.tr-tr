---
title: Azure Kubernetes hizmetinden Azure Container Registry kimlik doğrulama
description: Azure Active Directory hizmet sorumlusu kullanarak Azure Kubernetes hizmetinden özel kapsayıcı kayıt defterinizde görüntülere erişim sağlamayı öğrenin.
services: container-service
author: dlepow
manager: gwallace
ms.service: container-service
ms.topic: article
ms.date: 08/27/2019
ms.author: danlep
ms.openlocfilehash: dc5276227913d2da6e52ee3c0fb493b98e86688a
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827774"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Azure Kubernetes hizmetinden Azure Container Registry kimlik doğrulama

Azure Kubernetes Service (AKS) ile Azure Container Registry (ACR) kullanırken, bir kimlik doğrulama mekanizmasının kurulması gerekir. Bu makalede, bu iki Azure hizmeti arasında kimlik doğrulaması için önerilen yapılandırmaların ayrıntıları yer aldığı açıklanır.

Yalnızca bu kimlik doğrulama yöntemlerinden birini yapılandırmanız yeterlidir. En yaygın yaklaşım, [AKS hizmet sorumlusunu kullanarak erişim verkullanmaktır](#grant-aks-access-to-acr). Belirli gereksinimleriniz varsa, isteğe bağlı olarak [Kubernetes gizli dizileri kullanarak erişim izni](#access-with-kubernetes-secret)verebilirsiniz.

Bu makalede, zaten bir AKS kümesi oluşturduğunuz ve kümeye `kubectl` komut satırı istemcisiyle erişebildiğinizi varsaymış olursunuz. Küme oluşturma zamanında bir küme oluşturmak ve bir kapsayıcı kayıt defterine erişimi yapılandırmak istiyorsanız bkz. [öğretici: AKS kümesi dağıtma](../aks/tutorial-kubernetes-deploy-cluster.md) veya [Azure Kubernetes hizmetinden Azure Container Registry kimlik doğrulama](../aks/cluster-container-registry-integration.md).

## <a name="grant-aks-access-to-acr"></a>ACR 'ye AKS erişimi verme

Bir AKS kümesi oluşturduğunuzda, Azure aynı zamanda diğer Azure kaynaklarıyla küme çalışabilirliği desteklemek için bir hizmet sorumlusu oluşturur. Bir ACR kayıt defteri ile kimlik doğrulaması için otomatik olarak oluşturulan bu hizmet sorumlusunu kullanabilirsiniz. Bunu yapmak için, kümenin hizmet sorumlusu kapsayıcısını kapsayıcı kayıt defterine veren bir Azure AD [rol ataması](../role-based-access-control/overview.md#role-assignments) oluşturmanız gerekir.

Azure Container Registry 'ye yönelik AKS tarafından oluşturulan hizmet sorumlusu çekme erişimine izin vermek için aşağıdaki betiği kullanın. Betiği çalıştırmadan önce ortamınız için `AKS_*` ve `ACR_*` değişkenlerini değiştirin.

```bash
#!/bin/bash

AKS_RESOURCE_GROUP=myAKSResourceGroup
AKS_CLUSTER_NAME=myAKSCluster
ACR_RESOURCE_GROUP=myACRResourceGroup
ACR_NAME=myACRRegistry

# Get the id of the service principal configured for AKS
CLIENT_ID=$(az aks show --resource-group $AKS_RESOURCE_GROUP --name $AKS_CLUSTER_NAME --query "servicePrincipalProfile.clientId" --output tsv)

# Get the ACR registry resource id
ACR_ID=$(az acr show --name $ACR_NAME --resource-group $ACR_RESOURCE_GROUP --query "id" --output tsv)

# Create role assignment
az role assignment create --assignee $CLIENT_ID --role acrpull --scope $ACR_ID
```

## <a name="access-with-kubernetes-secret"></a>Kubernetes gizli anahtarı ile erişim

Bazı örneklerde, ACR 'ye erişim izni veren otomatik oluşturulan AKS hizmet sorumlusuna gerekli rolü atamayamayabilir. Örneğin, kuruluşunuzun güvenlik modeli nedeniyle, AKS tarafından oluşturulan hizmet sorumlusuna bir rol atamak için Azure Active Directory kiracınızda yeterli izinlere sahip olmayabilirsiniz. Bir hizmet sorumlusuna rol atamak için Azure AD hesabınızın Azure AD kiracınızda yazma izni olması gerekir. İzniniz yoksa, yeni bir hizmet sorumlusu oluşturabilir ve ardından bir Kubernetes görüntü çekme gizli anahtarı kullanarak kapsayıcı kayıt defterine erişim izni verebilirsiniz.

Yeni bir hizmet sorumlusu oluşturmak için aşağıdaki betiği kullanın (Kubernetes görüntü çekme sırrı için kimlik bilgilerini kullanacaksınız). Betiği çalıştırmadan önce ortamınızın `ACR_NAME` değişkenini değiştirin.

```bash
#!/bin/bash

ACR_NAME=myacrinstance
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate the ACR login server and resource id.
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create acrpull role assignment with a scope of the ACR resource.
SP_PASSWD=$(az ad sp create-for-rbac --name http://$SERVICE_PRINCIPAL_NAME --role acrpull --scopes $ACR_REGISTRY_ID --query password --output tsv)

# Get the service principal client id.
CLIENT_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output used when creating Kubernetes secret.
echo "Service principal ID: $CLIENT_ID"
echo "Service principal password: $SP_PASSWD"
```

Artık hizmet sorumlusunun kimlik bilgilerini bir Kubernetes [resim çekme gizli][image-pull-secret]dizisi içinde depolayacaksınız. Bu, kapsayıcıları çalıştırırken aks kümenizin başvurtukları olabilir.

Kubernetes gizli anahtarını oluşturmak için aşağıdaki **kubectl** komutunu kullanın. @No__t-0 ' yı Azure Container Registry 'nizin tam adıyla değiştirin ("acrname.azurecr.io" biçimindedir). @No__t-0 ve `<service-principal-password>` ' i önceki betiği çalıştırarak elde ettiğiniz değerlerle değiştirin. @No__t-0 ' i iyi biçimlendirilmiş bir e-posta adresiyle değiştirin.

```bash
kubectl create secret docker-registry acr-auth --docker-server <acr-login-server> --docker-username <service-principal-ID> --docker-password <service-principal-password> --docker-email <email-address>
```

Artık `imagePullSecrets` parametresinde adını (Bu örnekte "ACR-Auth") belirterek Pod dağıtımlarında Kubernetes gizliliğini kullanabilirsiniz:

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: acr-auth-example
spec:
  template:
    metadata:
      labels:
        app: acr-auth-example
    spec:
      containers:
      - name: acr-auth-example
        image: myacrregistry.azurecr.io/acr-auth-example
      imagePullSecrets:
      - name: acr-auth
```

<!-- LINKS - external -->
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[image-pull-secret]: https://kubernetes.io/docs/concepts/configuration/secret/#using-imagepullsecrets
