---
title: Azure Container Registry Azure Kubernetes hizmeti ile tümleştirme
description: Azure Kubernetes Service 'i (AKS) Azure Container Registry (ACR) ile tümleştirmeyi öğrenin
services: container-service
author: mlearned
manager: gwallace
ms.service: container-service
ms.topic: article
ms.date: 09/17/2018
ms.author: mlearned
ms.openlocfilehash: ab744efd205d826cb7ae2c3eda7bba28f4a9bee0
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71097797"
---
# <a name="authenticate-with-azure-container-registry-from-azure-kubernetes-service"></a>Azure Kubernetes hizmetinden Azure Container Registry kimlik doğrulama

Azure Kubernetes Service (AKS) ile Azure Container Registry (ACR) kullanırken, bir kimlik doğrulama mekanizmasının kurulması gerekir. Bu makalede, bu iki Azure hizmeti arasında kimlik doğrulaması için önerilen yapılandırmaların ayrıntıları yer aldığı açıklanır.

Azure CLı ile birkaç basit komutlarda ACR tümleştirmesi için AKS tümleştirmesini ayarlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdakilere sahip olmanız gerekir:

* **Azure aboneliğinde** **sahip** veya **Azure Hesap Yöneticisi** rolü
* Ayrıca Azure CLı sürüm 2.0.73 veya üzeri bir sürüme de ihtiyacınız vardır
* İstemcinizdeki [Docker yüklü](https://docs.docker.com/install/) olmalıdır ve [Docker Hub](https://hub.docker.com/) 'a erişmeniz gerekir

## <a name="create-a-new-aks-cluster-with-acr-integration"></a>ACR tümleştirmesi ile yeni bir AKS kümesi oluşturma

AKS kümenizi ilk oluşturma sırasında AKS ve ACR tümleştirmesini ayarlayabilirsiniz.  AKS kümesinin ACR ile etkileşime geçmesini sağlamak için bir Azure Active Directory **hizmet sorumlusu** kullanılır. Aşağıdaki CLı komutu aboneliğinizdeki mevcut bir ACR 'ye yetki vermenize ve hizmet sorumlusu için uygun **Acrpull** rolünü yapılandırmanıza olanak tanır. Aşağıdaki parametreleriniz için geçerli değerler sağlayın.  Köşeli ayraçlar içindeki parametreler isteğe bağlıdır.
```azurecli
az login
az acr create -n myContainerRegistry -g myContainerRegistryResourceGroup --sku basic [in case you do not have an existing ACR]
az aks create -n myAKSCluster -g myResourceGroup --attach-acr <acr-name-or-resource-id>
```
**ACR kaynak KIMLIĞI aşağıdaki biçimdedir:** 

/Subscriptions/< abonelik-d >/resourceGroups/< Kaynak-Grup-adı >/providers/Microsoft.ContainerRegistry/registries/{name} 
  
Bu adımın tamamlanması birkaç dakika sürebilir.

## <a name="configure-acr-integration-for-existing-aks-clusters"></a>Mevcut AKS kümeleri için ACR tümleştirmesini yapılandırma

Aşağıdaki gibi **ACR-Name** veya **ACR-Resource-id** için geçerli değerler sağlayarak mevcut bir ACR 'yi mevcut aks kümeleriyle tümleştirin.

```azurecli
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acr-resource-id>
```

Bir ACR ve AKS kümesi arasındaki tümleştirmeyi aşağıdakiler ile de kaldırabilirsiniz
```azurecli
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acrName>
az aks update -n myAKSCluster -g myResourceGroup --detach-acr <acr-resource-id>
```


## <a name="log-in-to-your-acr"></a>ACR 'nize oturum açma

ACR 'nize oturum açmak için aşağıdaki komutu kullanın.  <acrname> Parametresini ACR adınızla değiştirin.  Örneğin, varsayılan değer **aks-Resource-group > ACR <** .

```azurecli
az acr login -n <acrName>
```

## <a name="pull-an-image-from-docker-hub-and-push-to-your-acr"></a>Docker Hub 'dan bir görüntü çekin ve ACR 'nize gönderin

Docker Hub 'dan bir görüntü çekin, resmi etiketleyerek ACR 'nize gönderin.

```console
acrloginservername=$(az acr show -n <acrname> -g <myResourceGroup> --query loginServer -o tsv)
docker pull nginx
```

```
$ docker tag nginx $acrloginservername/nginx:v1
$ docker push $acrloginservername/nginx:v1

The push refers to repository [someacr1.azurecr.io/nginx]
fe6a7a3b3f27: Pushed
d0673244f7d4: Pushed
d8a33133e477: Pushed
v1: digest: sha256:dc85890ba9763fe38b178b337d4ccc802874afe3c02e6c98c304f65b08af958f size: 948
```

## <a name="update-the-state-and-verify-pods"></a>Durumu güncelleştirin ve pod 'yi doğrulayın

Dağıtımınızı doğrulamak için aşağıdaki adımları gerçekleştirin.

```azurecli
az aks get-credentials -g myResourceGroup -n myAKSCluster
```

YAML dosyasını görüntüleyin ve değeri ACR oturum açma sunucusu, görüntünüz ve etiketinizle değiştirerek Image özelliğini düzenleyin.

```
$ cat acr-nginx.yaml

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

$ kubectl apply -f acr-nginx.yaml
$ kubectl get pods

You should have two running pods.

NAME                                 READY   STATUS    RESTARTS   AGE
nginx0-deployment-669dfc4d4b-x74kr   1/1     Running   0          20s
nginx0-deployment-669dfc4d4b-xdpd6   1/1     Running   0          20s
```

<!-- LINKS - external -->
[AKS AKS CLI]:  https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create
