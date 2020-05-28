---
title: Azure Container Registry Azure Kubernetes hizmeti ile tümleştirme
description: Azure Kubernetes Service 'i (AKS) Azure Container Registry (ACR) ile tümleştirmeyi öğrenin
services: container-service
manager: gwallace
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: 70c36f9a18a85b90bb3a66d4083a71a00f61f14e
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84016381"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Azure Kubernetes Service'ten Azure Container Registry ile Kimlik Doğrulaması Yapma

Azure Kubernetes Service (AKS) ile Azure Container Registry (ACR) kullanırken, bir kimlik doğrulama mekanizmasının kurulması gerekir. Bu makalede, bu iki Azure hizmeti arasında kimlik doğrulaması yapılandırmak için örnekler sağlanmaktadır. 

Azure CLı ile birkaç basit komutlarda ACR tümleştirmesi için AKS tümleştirmesini ayarlayabilirsiniz. Bu tümleştirme, AKS kümesiyle ilişkili hizmet sorumlusuna AcrPull rolünü atar.

## <a name="before-you-begin"></a>Başlamadan önce

Bu örneklerde şunlar gerekir:

* **Azure aboneliğinde** **sahip** veya **Azure Hesap Yöneticisi** rolü
* Azure CLı sürüm 2.0.73 veya üzeri

Bir **sahip** veya **Azure Hesap Yöneticisi** rolüne gerek duymamak için bir hizmet sorumlusunu el ile yapılandırabilir veya mevcut bir hizmet sorumlusunu kullanarak aks 'ten ACR 'nin kimliğini doğrulayabilirsiniz. Daha fazla bilgi için bkz. [hizmet sorumluları Ile ACR kimlik doğrulaması](../container-registry/container-registry-auth-service-principal.md) veya [çekme gizli anahtarı Ile Kubernetes kimlik doğrulaması](../container-registry/container-registry-auth-kubernetes.md).

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>ACR tümleştirmesi ile yeni bir AKS kümesi oluşturma

AKS kümenizi ilk oluşturma sırasında AKS ve ACR tümleştirmesini ayarlayabilirsiniz.  AKS kümesinin ACR ile etkileşime geçmesini sağlamak için bir Azure Active Directory **hizmet sorumlusu** kullanılır. Aşağıdaki CLı komutu aboneliğinizdeki mevcut bir ACR 'ye yetki vermenize ve hizmet sorumlusu için uygun **Acrpull** rolünü yapılandırmanıza olanak tanır. Aşağıdaki parametreleriniz için geçerli değerler sağlayın.

```azurecli
# set this to the name of your Azure Container Registry.  It must be globally unique
MYACR=myContainerRegistry

# Run the following line to create an Azure Container Registry if you do not already have one
az acr create -n $MYACR -g myContainerRegistryResourceGroup --sku basic

# Create an AKS cluster with ACR integration
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr $MYACR
```

Alternatif olarak, aşağıdaki biçime sahip bir ACR kaynak KIMLIĞI kullanarak ACR adını belirtebilirsiniz:

`/subscriptions/\<subscription-id\>/resourceGroups/\<resource-group-name\>/providers/Microsoft.ContainerRegistry/registries/\<name\>` 

```azurecli
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr /subscriptions/<subscription-id>/resourceGroups/myContainerRegistryResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry
```

Bu adımın tamamlanması birkaç dakika sürebilir.

## <a name="configure-acr-integration-for-existing-aks-clusters"></a>Mevcut AKS kümeleri için ACR tümleştirmesini yapılandırma

Aşağıdaki gibi **ACR-Name** veya **ACR-Resource-id** için geçerli değerler sağlayarak mevcut bir ACR 'yi mevcut aks kümeleriyle tümleştirin.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
```

veya

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

Bir ACR ve AKS kümesi arasındaki tümleştirmeyi aşağıdakiler ile de kaldırabilirsiniz

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acrName>
```

veya

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```

## <a name="working-with-acr--aks"></a>ACR & AKS ile çalışma

### <a name="import-an-image-into-your-acr"></a>ACR 'nize görüntü aktarma

Aşağıdaki çalıştırarak, Docker Hub 'ından bir görüntüyü ACR 'nize aktarın:


```azurecli
az acr import  -n <myContainerRegistry> --source docker.io/library/nginx:latest --image nginx:v1
```

### <a name="deploy-the-sample-image-from-acr-to-aks"></a>Örnek görüntüyü ACR 'den AKS 'e dağıtma

Uygun AKS kimlik bilgilerine sahip olduğunuzdan emin olun

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

Aşağıdakileri içeren **ACR-NGINX. YAML** adlı bir dosya oluşturun:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx0-deployment
  labels:
    app: nginx0-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx0
  template:
    metadata:
      labels:
        app: nginx0
    spec:
      containers:
      - name: nginx
        image: <replace this image property with you acr login server, image and tag>
        ports:
        - containerPort: 80
```

Ardından, bu dağıtımı AKS kümenizde çalıştırın:

```console
kubectl apply -f acr-nginx.yaml
```

Aşağıdakileri çalıştırarak dağıtımı izleyebilirsiniz:

```console
kubectl get pods
```

İki çalışan bir pod olmalıdır.

```output
NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

<!-- LINKS - external -->
[AKS AKS CLI]:  https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
