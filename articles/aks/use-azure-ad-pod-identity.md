---
title: Azure Kubernetes hizmetinde Azure Active Directory Pod tarafından yönetilen kimlikler kullanma (Önizleme)
description: Azure Kubernetes Service 'te (AKS) AAD Pod tarafından yönetilen Yönetilen kimlikler kullanmayı öğrenin
services: container-service
ms.topic: article
ms.date: 12/01/2020
ms.openlocfilehash: e7c8a96ad012afdcd724a4a242c27018563f3a10
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102176323"
---
# <a name="use-azure-active-directory-pod-managed-identities-in-azure-kubernetes-service-preview"></a>Azure Kubernetes hizmetinde Azure Active Directory Pod tarafından yönetilen kimlikler kullanma (Önizleme)

Pod tarafından yönetilen kimlik Azure Active Directory, Kubernetes temel öğelerini kullanarak Azure Active Directory (AAD) [Azure kaynakları ve kimlikleri için yönetilen kimlikleri][az-managed-identities] pods ile ilişkilendirin. Yöneticiler, bir kimlik sağlayıcısı olarak AAD 'yi kullanan Azure kaynaklarına erişmesine izin veren Kubernetes temel olarak kimlikler ve bağlamalar oluşturur.

> [!NOTE]
> Zaten bir AADPODIDENTITY yüklemesine sahipseniz, mevcut yüklemeyi kaldırmanız gerekir. Bu özelliği etkinleştirmek, MıC bileşeninin gerekli olmadığı anlamına gelir.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki kaynağın yüklü olması gerekir:

* Azure CLı, sürüm 2.8.0 veya üzeri
* `azure-preview`Uzantı sürümü 0.4.68 veya üzeri

### <a name="limitations"></a>Sınırlamalar

* Küme için en fazla 50 Pod kimliği kullanılabilir.
* Küme için en fazla 50 Pod kimlik özel durumu kullanılabilir.
* Pod tarafından yönetilen kimlikler yalnızca Linux düğüm havuzlarında kullanılabilir.

### <a name="register-the-enablepodidentitypreview"></a>Kayıt defteri `EnablePodIdentityPreview`

Özelliği kaydedin `EnablePodIdentityPreview` :

```azurecli
az feature register --name EnablePodIdentityPreview --namespace Microsoft.ContainerService
```

### <a name="install-the-aks-preview-azure-cli"></a>`aks-preview`Azure CLI 'yı yükler

Ayrıca, *aks-Preview* Azure CLI uzantısı sürüm 0.4.64 veya sonraki bir sürüme de ihtiyacınız vardır. [Az Extension Add][az-extension-add] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükler. Veya [az Extension Update][az-extension-update] komutunu kullanarak kullanılabilir tüm güncelleştirmeleri yükler.

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="create-an-aks-cluster-with-managed-identities"></a>Yönetilen kimliklerle bir AKS kümesi oluşturma

Yönetilen bir kimlik ve pod ile yönetilen kimlik özellikli bir AKS kümesi oluşturun. Aşağıdaki komutlar *myresourcegroup adlı* bir kaynak grubu oluşturmak için [az Group Create][az-group-create] komutunu ve *Myresourcegroup* kaynak grubunda *myakscluster* adlı bir aks kümesi oluşturmak için [az aks Create][az-aks-create] komutunu kullanır.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
az aks create -g myResourceGroup -n myAKSCluster --enable-managed-identity --enable-pod-identity --network-plugin azure
```

AKS kümenizde oturum açmak için [az aks Get-Credentials][az-aks-get-credentials] kullanın. Bu komut ayrıca `kubectl` geliştirme bilgisayarınızda istemci sertifikasını indirir ve yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="create-an-identity"></a>Kimlik oluşturma

[Az Identity Create][az-identity-create] kullanarak bir kimlik oluşturun ve *IDENTITY_CLIENT_ID* ve *IDENTITY_RESOURCE_ID* değişkenlerini ayarlayın.

```azurecli-interactive
az group create --name myIdentityResourceGroup --location eastus
export IDENTITY_RESOURCE_GROUP="myIdentityResourceGroup"
export IDENTITY_NAME="application-identity"
az identity create --resource-group ${IDENTITY_RESOURCE_GROUP} --name ${IDENTITY_NAME}
export IDENTITY_CLIENT_ID="$(az identity show -g ${IDENTITY_RESOURCE_GROUP} -n ${IDENTITY_NAME} --query clientId -otsv)"
export IDENTITY_RESOURCE_ID="$(az identity show -g ${IDENTITY_RESOURCE_GROUP} -n ${IDENTITY_NAME} --query id -otsv)"
```

## <a name="assign-permissions-for-the-managed-identity"></a>Yönetilen kimlik için izin atama

*IDENTITY_CLIENT_ID* yönetilen kimliğin, aks kümenizin sanal makine ölçek kümesini içeren kaynak grubunda okuyucu izinlerine sahip olması gerekir.

```azurecli-interactive
NODE_GROUP=$(az aks show -g myResourceGroup -n myAKSCluster --query nodeResourceGroup -o tsv)
NODES_RESOURCE_ID=$(az group show -n $NODE_GROUP -o tsv --query "id")
az role assignment create --role "Reader" --assignee "$IDENTITY_CLIENT_ID" --scope $NODES_RESOURCE_ID
```

## <a name="create-a-pod-identity"></a>Pod kimliği oluşturma

Kullanarak küme için bir pod kimliği oluşturun `az aks pod-identity add` .

> [!IMPORTANT]
> `Owner`Kimliği ve rol bağlamayı oluşturmak için aboneliğinizde, gibi uygun izinlere sahip olmanız gerekir.

```azurecli-interactive
export POD_IDENTITY_NAME="my-pod-identity"
export POD_IDENTITY_NAMESPACE="my-app"
az aks pod-identity add --resource-group myResourceGroup --cluster-name myAKSCluster --namespace ${POD_IDENTITY_NAMESPACE}  --name ${POD_IDENTITY_NAME} --identity-resource-id ${IDENTITY_RESOURCE_ID}
```

> [!NOTE]
> AKS kümenizde Pod tarafından yönetilen kimliği etkinleştirdiğinizde, KUIN- *addon-Exception* adlı bir AzurePodIdentityException, *Kuto-System* ad alanına eklenir. Bir AzurePodIdentityException, bazı etiketlerin, düğüm tarafından yönetilen kimlik (NMI) sunucusu tarafından yakalanmadan Azure Instance Metadata Service (IMDS) uç noktasına erişmesine izin verir. *Aks-addon-Exception* , AAD Pod tarafından yönetilen kimlik gibi aks 'Nin bir AzurePodIdentityException el ile yapılandırılması gerekmeden çalışmasına izin verir. İsteğe bağlı olarak,,, veya kullanarak AzurePodIdentityException ekleyebilir, kaldırabilir ve `az aks pod-identity exception add` güncelleştirebilirsiniz `az aks pod-identity exception delete` `az aks pod-identity exception update` `kubectl` .

## <a name="run-a-sample-application"></a>Örnek uygulama çalıştırma

Pod 'un AAD Pod tarafından yönetilen kimliği kullanabilmesi için pod 'ın bir *AzureIdentityBinding* ile eşleşen bir değer içeren bir *aadpodidbinding* etiketi olması gerekir. AAD Pod tarafından yönetilen kimliği kullanarak örnek bir uygulama çalıştırmak için, `demo.yaml` aşağıdaki içeriğe sahip bir dosya oluşturun. *POD_IDENTITY_NAME*, *IDENTITY_CLIENT_ID* ve *IDENTITY_RESOURCE_GROUP* önceki adımlardan alınan değerlerle değiştirin. *SUBSCRIPTION_ID* , abonelik Kimliğinizle değiştirin.

> [!NOTE]
> Önceki adımlarda *POD_IDENTITY_NAME*, *IDENTITY_CLIENT_ID* ve *IDENTITY_RESOURCE_GROUP* değişkenlerini oluşturdunuz. `echo`Örneğin, değişkenler için ayarladığınız değeri göstermek için gibi bir komut kullanabilirsiniz `echo $IDENTITY_NAME` .

```yml
apiVersion: v1
kind: Pod
metadata:
  name: demo
  labels:
    aadpodidbinding: POD_IDENTITY_NAME
spec:
  containers:
  - name: demo
    image: mcr.microsoft.com/oss/azure/aad-pod-identity/demo:v1.6.3
    args:
      - --subscriptionid=SUBSCRIPTION_ID
      - --clientid=IDENTITY_CLIENT_ID
      - --resourcegroup=IDENTITY_RESOURCE_GROUP
    env:
      - name: MY_POD_NAME
        valueFrom:
          fieldRef:
            fieldPath: metadata.name
      - name: MY_POD_NAMESPACE
        valueFrom:
          fieldRef:
            fieldPath: metadata.namespace
      - name: MY_POD_IP
        valueFrom:
          fieldRef:
            fieldPath: status.podIP
  nodeSelector:
    kubernetes.io/os: linux
```

Pod tanımında bir *aadpodidbinding* etiketinin, önceki adımda çalıştırdığınız Pod Identity adı ile eşleşen bir değere sahip olduğuna dikkat edin `az aks pod-identity add` .

Öğesini `demo.yaml` kullanarak Pod Kimliğiniz ile aynı ad alanına dağıtın `kubectl apply` :

```azurecli-interactive
kubectl apply -f demo.yaml --namespace $POD_IDENTITY_NAMESPACE
```

Örnek uygulamanın kullanarak başarıyla çalıştığını doğrulayın `kubectl logs` .

```azurecli-interactive
kubectl logs demo --follow --namespace $POD_IDENTITY_NAMESPACE
```

Günlüklerde bir belirtecin başarıyla alındığından ve *Get* işleminin başarılı olduğundan emin olun.
 
```output
...
successfully doARMOperations vm count 0
successfully acquired a token using the MSI, msiEndpoint(http://169.254.169.254/metadata/identity/oauth2/token)
successfully acquired a token, userAssignedID MSI, msiEndpoint(http://169.254.169.254/metadata/identity/oauth2/token) clientID(xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
successfully made GET on instance metadata
...
```

## <a name="clean-up"></a>Temizleme

AAD Pod tarafından yönetilen kimliği kümeinizden kaldırmak için, kümeden örnek uygulamayı ve pod kimliğini kaldırın. Ardından kimliği kaldırın.

```azurecli-interactive
kubectl delete pod demo --namespace $POD_IDENTITY_NAMESPACE
az aks pod-identity delete --name ${POD_IDENTITY_NAME} --namespace ${POD_IDENTITY_NAMESPACE} --resource-group myResourceGroup --cluster-name myAKSCluster
az identity delete -g ${IDENTITY_RESOURCE_GROUP} -n ${IDENTITY_NAME}
```

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler][az-managed-identities].

<!-- LINKS - external -->
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-group-create]: /cli/azure/group#az-group-create
[az-identity-create]: /cli/azure/identity#az_identity_create
[az-managed-identities]: ../active-directory/managed-identities-azure-resources/overview.md
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
