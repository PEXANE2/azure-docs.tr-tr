---
title: Uygulamadan Azure Container Registry görüntüsü oluşturma
description: Bir uygulamadan kapsayıcı görüntüsü oluşturmak için az ACR Pack Build komutunu kullanın ve Dockerfile kullanmadan Azure Container Registry gönderin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 07/22/2019
ms.author: danlep
ms.openlocfilehash: 5100418651e24d74ad747e8c436ffce53c899a92
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68500903"
---
# <a name="build-and-push-an-image-from-an-app-using-a-cloud-native-buildpack"></a>Cloud Native Buildpack kullanarak bir uygulamadan görüntü oluşturma ve gönderme

Azure CLI komutu `az acr pack build` , [buildpacks](https://buildpacks.io/)'ten bir uygulama oluşturmak ve görüntüsünü bir Azure Container Registry 'ye göndermek için [`pack`](https://github.com/buildpack/pack) CLI aracını kullanır. Bu özellik, bir Dockerfile tanımlamak zorunda kalmadan Node. js, Java ve diğer dillerdeki uygulama kaynak kodunuzun hızlı bir kapsayıcı görüntüsünü oluşturmak için bir seçenek sunar.

Bu makaledeki örnekleri çalıştırmak için Azure CLı 'nın Azure Cloud Shell veya yerel bir yüklemesini kullanabilirsiniz. Yerel olarak kullanmak isterseniz, 2.0.70 veya üzeri sürümü gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="use-the-build-command"></a>Build komutunu kullanma

Cloud Native Buildpacks kullanarak bir kapsayıcı görüntüsü derlemek ve göndermek için [az ACR Pack Build][az-acr-pack-build] komutunu çalıştırın. [Az ACR Build][az-acr-build] komutu, bir dockerfile kaynağından ve ilgili koddan `az acr pack build` doğrudan bir uygulama kaynak ağacı belirttiğinizde bir görüntü oluşturur ve gönderir.

En azından, çalıştırdığınızda `az acr pack build`şunları belirtin:

* Komutu çalıştırdığınız bir Azure Container kayıt defteri
* Elde edilen görüntü için bir görüntü adı ve etiketi
* Bir yerel dizin, GitHub deposu veya uzak bir tarbol gibi ACR görevleri için [desteklenen bağlam konumlarından](container-registry-tasks-overview.md#quick-task) biri
* Bir Buildpack Oluşturucu görüntüsünün adı, örneğin `cloudfoundry/cnb:bionic`.  

`az acr pack build`, akan ve ayrıca daha sonra alınabilmeleri için [çalışan değişkenleri](container-registry-tasks-reference-yaml.md#run-variables) ve [görev çalıştırma günlüklerini](container-registry-tasks-overview.md#view-task-logs) içeren ACR görevleri komutlarının diğer özelliklerini destekler.

## <a name="example-build-nodejs-image-with-cloud-foundry-builder"></a>Örnek: Cloud Foundry Builder ile Node. js görüntüsü oluşturma

Aşağıdaki örnek, `cloudfoundry/cnb:bionic` oluşturucuyu kullanarak [Azure-Samples/NodeJS-docs-Hello-World](https://github.com/Azure-Samples/nodejs-docs-hello-world) depoındaki Node. js uygulamasından bir kapsayıcı görüntüsü oluşturur:

```azurecli
az acr pack build \
    --registry myregistry \
    --image {{.Run.Registry}}/node-app:1.0 \
    --builder cloudfoundry/cnb:bionic \
    https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

Bu örnek, `node-app` görüntüsünü `1.0` etiketiyle oluşturur ve *myregistry* kapsayıcı kayıt defterine gönderir. Burada, hedef kayıt defteri adı, görüntü adına açıkça sona erer. Belirtilmezse, kayıt defteri URL 'SI otomatik olarak görüntü adına eklenir.

Komut çıktısı, görüntüyü oluşturma ve gönderme sürecini gösterir. 

Görüntü başarıyla oluşturulduktan sonra, yüklüyse Docker ile çalıştırabilirsiniz. İlk olarak kayıt defterinizde oturum açın:

```azurecli
az acr login --name myregistry
```

Görüntüyü çalıştırın:

```console
docker run --rm -p 1337:1337 myregistry.azurecr.io/node-app:1.0
```

`localhost:1337` Örnek Web uygulamasını görmek için en sevdiğiniz tarayıcınıza gidin. Kapsayıcıyı `[Ctrl]+[C]` durdurmak için tuşuna basın.

## <a name="example-build-java-image-with-heroku-builder"></a>Örnek: Heroku Builder ile Java görüntüsü oluşturma

Aşağıdaki örnek, `heroku/buildpacks:18` oluşturucuyu kullanarak [buildpack/Sample-Java-App](https://github.com/buildpack/sample-java-app) deposunda Java uygulamasından bir kapsayıcı görüntüsü oluşturur:

```azurecli
az acr pack build \
    --registry myregistry \
    --image java-app:{{.Run.ID}} \
    --pull --builder heroku/buildpacks:18 \
    https://github.com/buildpack/sample-java-app.git
```

Bu örnekte, komutun `java-app` çalıştırma kimliğiyle etiketlenmiş görüntü oluşturulur ve bunu *myregistry* kapsayıcı kayıt defterine iter.

`--pull` Parametresi, Heroku Builder görüntüsü ACR görevleri tarafından önbelleğe alınmadığı için, komutun gerekli en son Oluşturucu görüntüsünü çektiğinden emin olur.

Komut çıktısı, görüntüyü oluşturma ve gönderme sürecini gösterir. 

Görüntü başarıyla oluşturulduktan sonra, yüklüyse Docker ile çalıştırabilirsiniz. İlk olarak kayıt defterinizde oturum açın:

```azurecli
az acr login --name myregistry
```

Çalışma *kimliği*için görüntü etiketinizi değiştirerek görüntüyü çalıştırın:

```console
docker run --rm -p 8080:8080 myregistry.azurecr.io/java-app:runid
```

`localhost:8080` Örnek Web uygulamasını görmek için en sevdiğiniz tarayıcınıza gidin. Kapsayıcıyı `[Ctrl]+[C]` durdurmak için tuşuna basın.


## <a name="next-steps"></a>Sonraki adımlar

Bir kapsayıcı görüntüsünü `az acr pack build`derleyip gönderdikten sonra, istediğiniz bir hedefe herhangi bir görüntü gibi dağıtabilirsiniz. Azure dağıtım seçenekleri, [App Service](../app-service/containers/tutorial-custom-docker-image.md) veya [Azure Kubernetes hizmetinde](../aks/tutorial-kubernetes-deploy-cluster.md), diğerleri arasında çalıştırmayı içerir.

ACR görevleri özellikleri hakkında daha fazla bilgi için bkz. [ACR görevleriyle kapsayıcı görüntüsü derlemelerini ve bakımını otomatikleştirme](container-registry-tasks-overview.md).


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr/task
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
