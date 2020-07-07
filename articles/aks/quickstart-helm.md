---
title: Held ile Azure Kubernetes hizmeti (AKS) üzerinde geliştirme
description: Uygulama kapsayıcılarını bir kümede paketlemek ve çalıştırmak için AKS ile Held 'yi kullanın ve Azure Container Registry.
services: container-service
author: zr-msft
ms.topic: article
ms.date: 04/20/2020
ms.author: zarhoads
ms.openlocfilehash: 1f67605918e093e9ab28aa88be777d27acd831ef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82169577"
---
# <a name="quickstart-develop-on-azure-kubernetes-service-aks-with-helm"></a>Hızlı başlangıç: Held ile Azure Kubernetes hizmeti (AKS) üzerinde geliştirme

[Helk][helm] , Kubernetes uygulamalarının yaşam döngüsünü yüklemenize ve yönetmenize yardımcı olan bir açık kaynaklı paketleme aracıdır. *Apt* ve *yum*gibi Linux paket yöneticilerine benzer şekilde, Helm, önceden yapılandırılmış Kubernetes kaynakları paketleri olan Kubernetes grafiklerini yönetmek için kullanılır.

Bu makalede, AKS üzerinde bir uygulamayı paketlemek ve çalıştırmak için helk 'ın nasıl kullanılacağı gösterilmektedir. Held kullanarak mevcut bir uygulamayı yükleme hakkında daha fazla bilgi için bkz. [AKS 'de Held ile mevcut uygulamaları yükleme][helm-existing].

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
* Docker yüklendi ve yapılandırıldı. Docker [Mac][docker-for-mac], [Windows][docker-for-windows] veya [Linux][docker-for-linux] sisteminde Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.
* [Held v3 yüklendi][helm-install].

## <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma
Uygulamanızı AKS kümenizde çalıştırmak için Held 'yi kullanmak için, kapsayıcı görüntülerinizi depolamak için bir Azure Container Registry gerekir. Aşağıdaki örnek, *Myresourcegroup* kaynak grubunda *temel* SKU 'Su Ile *myhelmacr* adlı bir ACR oluşturmak için [az ACR Create][az-acr-create] komutunu kullanır. Kendi benzersiz kayıt defteriniz adını sağlamalısınız. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. *Temel* SKU, geliştirme amaçlı dağıtımlar için uygun maliyetli, depolama ve aktarım hızı açısından dengeli bir giriş noktasıdır.

```azurecli
az group create --name MyResourceGroup --location eastus
az acr create --resource-group MyResourceGroup --name MyHelmACR --sku Basic
```

Çıktı aşağıdaki örneğe benzerdir. Daha sonraki bir adımda kullanılabileceğinizden, ACR 'niz için *Loginserver* değerini bir yere göz önünde koyun. Aşağıdaki örnekte, *Myhelmacr.azurecr.io* *Myhelmacr*için *loginserver* ' dır.

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

ACR örneğini kullanmak için önce oturum açmalısınız. Oturum açmak için [az ACR Login][az-acr-login] komutunu kullanın. Aşağıdaki örnek, *Myhelmacr*adlı bir ACR 'de oturum açacaktır.

```azurecli
az acr login --name MyHelmACR
```

Komut tamamlandığında bir *oturum açma başarılı* iletisi döndürür.

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesi oluşturma

AKS kümesi oluşturma. Aşağıdaki komut, MyAKS adlı bir AKS kümesi oluşturur ve MyHelmACR öğesini ekler.

```azurecli
az aks create -g MyResourceGroup -n MyAKS --location eastus  --attach-acr MyHelmACR --generate-ssh-keys
```

AKS kümenizin, kapsayıcı görüntülerini çekmek ve çalıştırmak için ACR 'nize erişmesi gerekir. Yukarıdaki komut ayrıca *Myhelmacr* ACR 'Nize *myaks* kümesi erişimi verir.

## <a name="connect-to-your-aks-cluster"></a>AKS kümenize bağlanma

Yerel bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([kubectl][kubectl]) kullanmanız gerekir.

Azure Cloud Shell'i kullanıyorsanız `kubectl` zaten yüklüdür. [az aks install-cli][] komutunu kullanarak da yerel ortama yükleyebilirsiniz:

```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][] komutunu kullanın. Aşağıdaki örnek, *Myresourcegroup*Içinde *myaks* adlı aks kümesi için kimlik bilgilerini alır:

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıç [, Azure dev Spaces örnek deposundan bir örnek Node.js uygulaması][example-nodejs]kullanır. Uygulamayı GitHub 'dan kopyalayın ve `dev-spaces/samples/nodejs/getting-started/webfrontend` dizine gidin.

```console
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/nodejs/getting-started/webfrontend
```

## <a name="create-a-dockerfile"></a>Dockerfile oluşturma

Aşağıdakileri kullanarak yeni bir *Dockerfile* dosyası oluşturun:

```dockerfile
FROM node:latest

WORKDIR /webfrontend

COPY package.json ./

RUN npm install

COPY . .

EXPOSE 80
CMD ["node","server.js"]
```

## <a name="build-and-push-the-sample-application-to-the-acr"></a>Örnek uygulamayı derleyin ve ACR 'ye gönderin

[Az ACR List][az-acr-list] komutunu kullanarak oturum açma sunucusu adresini alın ve *loginserver*için sorgulama yapın:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Örnek uygulama kapsayıcınızı oluşturmak, etiketlemek ve ACR 'ye göndermek için Docker 'ı kullanın:

```console
docker build -t webfrontend:latest .
docker tag webfrontend <acrLoginServer>/webfrontend:v1
docker push <acrLoginServer>/webfrontend:v1
```

## <a name="create-your-helm-chart"></a>Held grafiğinizi oluşturma

Komutunu kullanarak Held grafiğinizi oluşturun `helm create` .

```console
helm create webfrontend
```

*Web ön ucu/değerleri. YAML*için aşağıdaki güncelleştirmeleri yapın:

* Değiştir `image.repository``<acrLoginServer>/webfrontend`
* Değiştir `service.type``LoadBalancer`

Örnek:

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

`appVersion` `v1` *Web ön ucu/grafik. YAML*'de olarak güncelleştirin. Örneğin:

```yml
apiVersion: v2
name: webfrontend
...
# This is the version number of the application being deployed. This version number should be
# incremented each time you make changes to the application.
appVersion: v1
```

## <a name="run-your-helm-chart"></a>Held grafiğinizi çalıştırma

`helm install`Hele grafiğinizi kullanarak uygulamanızı yüklemek için komutunu kullanın.

```console
helm install webfrontend webfrontend/
```

Hizmetin genel bir IP adresi döndürmesi birkaç dakika sürer. İlerlemeyi izlemek için, `kubectl get service` *Gözcü* parametresiyle komutunu kullanın:

```console
$ kubectl get service --watch

NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
webfrontend         LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
...
webfrontend         LoadBalancer  10.0.141.72   <EXTERNAL-IP> 80:32150/TCP   7m
```

Örnek uygulamayı görmek için kullanarak bir tarayıcıda uygulamanızın yük dengeleyicisine gidin `<EXTERNAL-IP>` .

## <a name="delete-the-cluster"></a>Küme silme

Küme artık gerekli olmadığında, [az Group Delete][az-group-delete] komutunu kullanarak kaynak grubunu, aks kümesini, kapsayıcı kayıt defterini, orada depolanan kapsayıcı görüntülerini ve tüm ilgili kaynakları kaldırın.

```azurecli-interactive
az group delete --name MyResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve kaldırma gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar

Held kullanma hakkında daha fazla bilgi için HELI belgelerine bakın.

> [!div class="nextstepaction"]
> [Hela belgeleri][helm-documentation]

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
