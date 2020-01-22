---
title: Azure'da Kubernetes öğreticisi - Kapsayıcı kayıt defteri oluşturma
description: Bu Azure Kubernetes Service (AKS) öğreticisinde bir Azure Container Registry örneği oluşturacak ve kapsayıcı görüntüsüne örnek uygulama yükleyeceksiniz.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: 04fbea9714224f0ecbac0e14618caaf39fa3cedf
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76291150"
---
# <a name="tutorial-deploy-and-use-azure-container-registry"></a>Öğretici: Azure Container Registry’yi dağıtma ve kullanma

Azure Container Registry (ACR), kapsayıcı görüntüleri için özel bir kayıt defteridir. Özel kapsayıcı kayıt defteri, uygulamalarınızı ve özel kodlarınızı güvenli bir şekilde derlemenizi ve dağıtmanızı sağlar. Yedi öğreticiden oluşan bu serinin ikinci kısmında, bir ACR örneği dağıtacak ve ona bir kapsayıcı görüntüsü göndereceksiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Azure Container Registry (ACR) örneği oluşturma
> * ACR için kapsayıcı görüntüsü etiketleme
> * Görüntüyü ACR’ye yükleme
> * Kayıt defterinizdeki görüntüleri görüntüleme

Ek öğreticilerde, bu ACR örneği AKS 'teki bir Kubernetes kümesiyle tümleşiktir ve görüntüden bir uygulama dağıtılır.

## <a name="before-you-begin"></a>Başlamadan önce

[Önceki öğreticide][aks-tutorial-prepare-app], basit bir Azure Voting uygulaması için kapsayıcı görüntüsü oluşturulacaktır. Azure Voting uygulaması görüntüsünü oluşturmadıysanız [Öğretici 1 - Kapsayıcı görüntüleri oluştur][aks-tutorial-prepare-app]’a dönün.

Bu öğreticide, Azure CLı sürüm 2.0.53 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma

Bir Azure Container Registry oluşturmak için önce bir kaynak grubuna ihtiyaç duyarsınız. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnekte, *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturulur:

```azurecli
az group create --name myResourceGroup --location eastus
```

[Az ACR Create][az-acr-create] komutuyla bir Azure Container Registry örneği oluşturun ve kendi kayıt defteriniz adını sağlayın. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Bu öğreticinin geri kalan aşamalarında, kapsayıcı kayıt defteri adı için yer tutucu olarak `<acrName>` kullanılacaktır. Kendi benzersiz kayıt defteriniz adını sağlayın. *Temel* SKU, geliştirme amaçlı dağıtımlar için uygun maliyetli, depolama ve aktarım hızı açısından dengeli bir giriş noktasıdır.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

## <a name="log-in-to-the-container-registry"></a>Kapsayıcı kayıt defterinde oturum açma

ACR örneğini kullanmak için oturum açmanız gerekir. [Az ACR Login][az-acr-login] komutunu kullanın ve önceki adımda kapsayıcı kayıt defterine verilen benzersiz adı belirtin.

```azurecli
az acr login --name <acrName>
```

Komut tamamlandığında bir *Oturum Başarıyla Açıldı* iletisi döndürür.

## <a name="tag-a-container-image"></a>Kapsayıcı görüntüsünü etiketleme

Geçerli yerel görüntülerinizin listesini görmek için [Docker görüntüleri][docker-images] komutunu kullanın:

```
$ docker images

REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

*azure-vote-front* kapsayıcı görüntüsünü ACR ile birlikte kullanmak için görüntünün, kayıt defterinizin oturum açma sunucusunun adresiyle etiketlenmesi gerekir. Bu etiket, görüntü kayıt defterine kapsayıcı görüntüleri gönderilirken kullanılır.

Oturum açma sunucusu adresini almak için, şu şekilde *Loginserver* için [az ACR List][az-acr-list] komutunu ve sorguyu kullanın:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Şimdi, yerel *Azure-oyön* görüntünüzü kapsayıcı kayıt defterinin *Acrloginserver* adresiyle etiketleyin. Görüntü sürümünü belirtmek için görüntü adının sonuna *:v1* ekleyin:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Etiketlerin uygulandığını doğrulamak için [Docker görüntülerini][docker-images] yeniden çalıştırın. Böylece görüntü, ACR örneği adresi ve sürüm numarasıyla etiketlenmiş olur.

```
$ docker images

REPOSITORY                                           TAG           IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest        eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry.azurecr.io/azure-vote-front      v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest        a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask         788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Kayıt defterine görüntü gönderme

Görüntünüz oluşturulup etiketledikten sonra, *Azure-oy ön* görüntüsünü ACR örneğinizle gönderin. [Docker Push][docker-push] kullanın ve görüntü adı Için kendi *Acrloginserver* adresinizi şu şekilde sağlayın:

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

Görüntünün ACR'ye gönderilmesi birkaç dakika sürebilir.

## <a name="list-images-in-registry"></a>Kayıt defterindeki görüntüleri listeleme

ACR örneğinize gönderilen görüntülerin listesini döndürmek için [az ACR Repository List][az-acr-repository-list] komutunu kullanın. Aşağıda gösterilen şekilde kendi `<acrName>` değerinizi belirtin:

```azurecli
az acr repository list --name <acrName> --output table
```

Aşağıdaki örnek çıktıda *azure-vote-front* görüntüsünün kayıt defterinde kullanılabilir durumda olduğu gösterilmektedir:

```
Result
----------------
azure-vote-front
```

Belirli bir görüntünün etiketlerini görmek için [az ACR Repository Show-Tags][az-acr-repository-show-tags] komutunu aşağıdaki gibi kullanın:

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Aşağıdaki örnekte önceki adımda etiketlenen *v1* görüntüsü gösterilmektedir:

```
Result
--------
v1
```

Artık özel bir Azure Container Registry örneğinde depolanmış olan bir kapsayıcı görüntünüz var. Bir sonraki öğreticide bu görüntüyü, ACR’den bir Kubernetes kümesine dağıtacaksınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure Container Service oluşturduğunuz ve AKS kümesinde kullanılmak üzere bir görüntü gönderdiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Azure Container Registry (ACR) örneği oluşturma
> * ACR için kapsayıcı görüntüsü etiketleme
> * Görüntüyü ACR’ye yükleme
> * Kayıt defterinizdeki görüntüleri görüntüleme

Azure’da Kubernetes kümesi dağıtmayla ilgili daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Kubernetes kümesi dağıtma][aks-tutorial-deploy-cluster]

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr
[az-acr-list]: /cli/azure/acr
[az-acr-login]: https://docs.microsoft.com/cli/azure/acr#az-acr-login
[az-acr-list]: https://docs.microsoft.com/cli/azure/acr#az-acr-list
[az-acr-repository-list]: /cli/azure/acr/repository
[az-acr-repository-show-tags]: /cli/azure/acr/repository
[az-group-create]: /cli/azure/group#az-group-create
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-tutorial-deploy-cluster]: ./tutorial-kubernetes-deploy-cluster.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
