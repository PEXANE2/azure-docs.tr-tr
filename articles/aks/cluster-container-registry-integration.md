---
title: Azure Konteyner Kayıt Defterini Azure Kubernetes Hizmetiyle Tümleştir
description: Azure Kubernetes Hizmeti'ni (AKS) Azure Kapsayıcı Kayıt Defteri (ACR) ile nasıl entegre edebilirsiniz öğrenin
services: container-service
manager: gwallace
ms.topic: article
ms.date: 02/25/2020
ms.openlocfilehash: 514cc25e1959145c65fe60cd3054cec4ed28f44d
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80617415"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Azure Kubernetes Service'ten Azure Container Registry ile Kimlik Doğrulaması Yapma

Azure Kubernetes Hizmeti (AKS) ile Azure Kapsayıcı Kayıt Defteri'ni (ACR) kullanırken, bir kimlik doğrulama mekanizmasının oluşturulması gerekir. Bu makalede, bu iki Azure hizmeti arasında kimlik doğrulamayapılandırmak için örnekler verilmektedir. 

AKS'den ACR tümleştirmesine Azure CLI ile birkaç basit komutta ayarlayabilirsiniz. Bu tümleştirme, AkS Kümesi'yle ilişkili hizmet ilkesine AcrPull rolünü atar.

## <a name="before-you-begin"></a>Başlamadan önce

Bu örnekler şunları gerektirir:

* **Azure aboneliğinde** **sahip** veya Azure **hesap yöneticisi** rolü
* Azure CLI sürüm 2.0.73 veya sonrası

**Sahibi** veya **Azure hesap yöneticisi** rolüne ihtiyaç duymamak için bir hizmet ilkesini el ile yapılandırabilir veya AKS'den ACR'nin kimliğini doğrulamak için varolan bir hizmet ilkesini kullanabilirsiniz. Daha fazla bilgi için, [hizmet ilkeleriyle ACR kimlik doğrulaması](../container-registry/container-registry-auth-service-principal.md) veya [Kubernetes'ten bir çekme sırrıyla kimlik doğrulaması'na](../container-registry/container-registry-auth-kubernetes.md)bakın.

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>ACR tümleştirmesi ile yeni bir AKS kümesi oluşturma

AKS kümenizin ilk oluşturulması sırasında AKS ve ACR tümleştirmesini ayarlayabilirsiniz.  Bir AKS kümesinin ACR ile etkileşim kurabilmesi için bir Azure Etkin Dizin **hizmet ilkesi** kullanılır. Aşağıdaki CLI komutu, aboneliğinizde varolan bir ACR'yi yetkilendirmenize olanak tanır ve hizmet sorumlusu için uygun **ACRPull** rolünü yapılandırır. Parametreleriniz için aşağıdaki geçerli değerleri sağlar.

```azurecli
# set this to the name of your Azure Container Registry.  It must be globally unique
$MYACR=myContainerRegistry

# Run the following line to create an Azure Container Registry if you do not already have one
az acr create -n $MYACR -g myContainerRegistryResourceGroup --sku basic

# Create an AKS cluster with ACR integration
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr $MYACR
```

Alternatif olarak, aşağıdaki biçime sahip bir ACR kaynak kimliği kullanarak ACR adını belirtebilirsiniz:

`/subscriptions/\<subscription-id\>/resourceGroups/\<resource-group-name\>/providers/Microsoft.ContainerRegistry/registries/\<name\>` 

```azurecli
az aks create -n myAKSCluster -g myResourceGroup --generate-ssh-keys --attach-acr /subscriptions/<subscription-id>/resourceGroups/myContainerRegistryResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry
```

Bu adımın tamamlanması birkaç dakika sürebilir.

## <a name="configure-acr-integration-for-existing-aks-clusters"></a>Varolan AKS kümeleri için ACR tümleştirmeyi yapılandırma

Aşağıdaki gibi **acr-name** veya **acr-resource-id** için geçerli değerler sağlayarak varolan bir ACR'yi varolan AKS kümeleriyle tümleştirin.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
```

Veya

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

Ayrıca, bir ACR ve aks kümesi arasındaki tümleştirmeyi aşağıdaki

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acrName>
```

or

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```

## <a name="working-with-acr--aks"></a>ACR & AKS ile çalışma

### <a name="import-an-image-into-your-acr"></a>ACR'nize görüntü alma

Aşağıdakileri çalıştırarak docker hub'ından ACR'nize bir görüntü aktarın:


```azurecli
az acr import  -n <myContainerRegistry> --source docker.io/library/nginx:latest --image nginx:v1
```

### <a name="deploy-the-sample-image-from-acr-to-aks"></a>Örnek görüntüyü ACR'den AKS'ye dağıtma

Uygun AKS kimlik bilgilerine sahip olduğundan emin olun

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

**Acr-nginx.yaml** adlı ve aşağıdakileri içeren bir dosya oluşturun:

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

İki çalışan kapsülün olmalı.

```output
NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

<!-- LINKS - external -->
[AKS AKS CLI]:  https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
