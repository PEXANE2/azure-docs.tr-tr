---
title: Miğfer ile Azure Kubernetes Hizmetinde (AKS) geliştirin
description: Bir kümedeki uygulama kapsayıcılarını paketlemek ve çalıştırmak için AKS ve Azure Kapsayıcı Kayıt Defteri ile Miğfer'i kullanın.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 04/20/2020
ms.author: zarhoads
ms.openlocfilehash: 77627ab846999ea5ba42fde7a9c49b9cc7559fba
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81873439"
---
# <a name="quickstart-develop-on-azure-kubernetes-service-aks-with-helm"></a>Quickstart: Helm ile Azure Kubernetes Hizmetinde (AKS) geliştirin

[Helm,][helm] Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. *APT* ve *Yum*gibi Linux paket yöneticilerine benzer şekilde Helm, önceden yapılandırılmış Kubernetes kaynaklarının paketleri olan Kubernetes grafiklerini yönetmek için kullanılır.

Bu makalede, AKS'de bir uygulamayı paketlemek ve çalıştırmak için Helm'i nasıl kullanacağınızı gösterilmektedir. Helm kullanarak varolan bir uygulamayı yükleme hakkında daha fazla bilgi için, [aks'ta Helm ile varolan uygulamaları yükleyin'e][helm-existing]bakın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
* Docker yüklendi ve yapılandırıldı. Docker [Mac][docker-for-mac], [Windows][docker-for-windows] veya [Linux][docker-for-linux] sisteminde Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.
* [Helm v3 yüklü][helm-install].

## <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma
Uygulamanızı AKS kümenizde çalıştırmak için Miğfer'i kullanmak için kapsayıcı resimlerinizi depolamak için bir Azure Kapsayıcı Kayıt Defteri gerekir. Aşağıdaki örnekte, *Temel* SKU ile *MyResourceGroup* kaynak grubunda *MyHelmACR* adlı bir ACR oluşturmak için [az acr oluşturma][az-acr-create] kullanır. Kendi benzersiz kayıt defteri adınızı sağlamanız gerekir. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. *Temel* SKU, geliştirme amaçlı dağıtımlar için uygun maliyetli, depolama ve aktarım hızı açısından dengeli bir giriş noktasıdır.

```azurecli
az group create --name MyResourceGroup --location eastus
az acr create --resource-group MyResourceGroup --name MyHelmACR --sku Basic
```

Çıktı aşağıdaki örneğe benzerdir. Daha sonraki bir adımda kullanılacağı için ACR'niz için *girişServer* değerine dikkat edin. Aşağıdaki örnekte, *myhelmacr.azurecr.io* *MyHelmACR*için *loginServer* olduğunu.

```console
{
  "adminUserEnabled": false,
  "creationDate": "2019-06-11T13:35:17.998425+00:00",
  "id": "/subscriptions/<ID>/resourceGroups/MyResourceGroup/providers/Microsoft.ContainerRegistry/registries/MyHelmACR",
  "location": "eastus",
  "loginServer": "myhelmacr.azurecr.io",
  "name": "MyHelmACR",
  "networkRuleSet": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "MyResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

ACR örneğini kullanmak için önce oturum açmanız gerekir. Oturum açmak için [az acr giriş][az-acr-login] komutunu kullanın. Aşağıdaki örnek, *MyHelmACR*adlı bir ACR'de oturum açacaktır.

```azurecli
az acr login --name MyHelmACR
```

Komut tamamlandıktan sonra *Oturum Açma Başarılı* iletisini döndürür.

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesi oluşturma

Bir AKS kümesi oluşturun. Aşağıdaki komut MyAKS adında bir AKS kümesi oluşturur ve MyHelmACR'ı bağlar.

```azurecli
az aks create -g MyResourceGroup -n MyAKS --location eastus  --attach-acr MyHelmACR --generate-ssh-keys
```

AKS kümenizin kapsayıcı görüntülerini çekmek ve çalıştırmak için ACR'nize erişmesi gerekir. Yukarıdaki komut ayrıca *MyHelmACR ACR'nize* *MyAKS* küme erişimi verir.

## <a name="connect-to-your-aks-cluster"></a>AKS kümenize bağlanın

Yerel bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([kubectl][kubectl]) kullanmanız gerekir.

Azure Cloud Shell'i kullanıyorsanız `kubectl` zaten yüklüdür. [az aks install-cli][] komutunu kullanarak da yerel ortama yükleyebilirsiniz:

```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][] komutunu kullanın. Aşağıdaki örnek, *MyResourceGroup'ta* *MyAKS* adlı AKS kümesi için kimlik bilgilerini alır:

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlatma, [Azure Dev Spaces örnek deposundan örnek bir Düğüm.js uygulaması][example-nodejs]kullanır. Uygulamayı GitHub'dan klonla ve `dev-spaces/samples/nodejs/getting-started/webfrontend` dizine gidin.

```console
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/nodejs/getting-started/webfrontend
```

## <a name="create-a-dockerfile"></a>Dockerfile Oluşturma

Aşağıdakileri kullanarak yeni bir *Dockerfile dosyası* oluşturun:

```dockerfile
FROM node:latest

WORKDIR /webfrontend

COPY package.json ./

RUN npm install

COPY . .

EXPOSE 80
CMD ["node","server.js"]
```

## <a name="build-and-push-the-sample-application-to-the-acr"></a>Örnek uygulamayı Oluşturun ve ACR'ye itin

[Az acr listesi][az-acr-list] komutunu kullanarak ve *giriş Sunucusu*için sorgulama kullanarak giriş sunucusu adresini alın:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Örnek uygulama kapsayıcınızı Oluşturmak, etiketlemek ve ACR'ye itmek için Docker'ı kullanın:

```console
docker build -t webfrontend:latest .
docker tag webfrontend <acrLoginServer>/webfrontend:v1
docker push <acrLoginServer>/webfrontend:v1
```

## <a name="create-your-helm-chart"></a>Miğfer grafiğinizi oluşturun

Komutu kullanarak Miğfer grafiğinizi `helm create` oluşturun.

```console
helm create webfrontend
```

*webfrontend/values.yaml*için aşağıdaki güncellemeleri yapın:

* Değiştir `image.repository``<acrLoginServer>/webfrontend`
* Değiştir `service.type``LoadBalancer`

Örneğin:

```yml
# Default values for webfrontend.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

replicaCount: 1

image:
  repository: <acrLoginServer>/webfrontend
  pullPolicy: IfNotPresent
...
service:
  type: LoadBalancer
  port: 80
...
```

`appVersion` `v1` *Webfrontend/Chart.yaml'de*güncelleştirin. Örneğin:

```yml
apiVersion: v2
name: webfrontend
...
# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application.
appVersion: v1
```

## <a name="run-your-helm-chart"></a>Miğfer grafiğinizi çalıştırın

Miğfer `helm create` grafiğinizi kullanarak uygulamanızı yüklemek için komutu kullanın.

```console
helm install webfrontend webfrontend/
```

Hizmetin genel bir IP adresini döndürmesi birkaç dakika sürer. İlerlemeyi izlemek için `kubectl get service` *saat* parametresi ile komutu kullanın:

```console
$ kubectl get service --watch

NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
webfrontend         LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
...
webfrontend         LoadBalancer  10.0.141.72   <EXTERNAL-IP> 80:32150/TCP   7m
```

Örnek uygulamayı görmek `<EXTERNAL-IP>` için bir tarayıcıda uygulamanızın yük dengeleyicisine gidin.

## <a name="delete-the-cluster"></a>Küme silme

Kümeye artık gerek kalmadığında, kaynak grubunu, AKS kümesini, kapsayıcı kayıt defterini, orada depolanan kapsayıcı görüntülerini ve ilgili tüm kaynakları kaldırmak için [az grubu silme][az-group-delete] komutunu kullanın.

```azurecli-interactive
az group delete --name MyResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve kaldırılmasını gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar

Helm kullanma hakkında daha fazla bilgi için Helm belgelerine bakın.

> [!div class="nextstepaction"]
> [Miğfer belgeleri][helm-documentation]

[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-list]: /cli/azure/acr#az-acr-list
[az-group-delete]: /cli/azure/group#az-group-delete
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli

[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[example-nodejs]: https://github.com/Azure/dev-spaces/tree/master/samples/nodejs/getting-started/webfrontend
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[helm]: https://helm.sh/
[helm-documentation]: https://helm.sh/docs/
[helm-existing]: kubernetes-helm.md
[helm-install]: https://helm.sh/docs/intro/install/
[sp-delete]: kubernetes-service-principal.md#additional-considerations
