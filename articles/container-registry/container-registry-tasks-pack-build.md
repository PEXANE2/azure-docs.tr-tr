---
title: Cloud Native Buildpack ile görüntü oluşturun
description: Bir uygulamadan bir kapsayıcı görüntüsü oluşturmak için az acr paketi oluşturma komutunu kullanın ve Dockerfile kullanmadan Azure Konteyner Kayıt Defteri'ne ilerleyin.
ms.topic: article
ms.date: 10/24/2019
ms.openlocfilehash: c42bde6bbab5973094302a2d41f004d7600bdf9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087080"
---
# <a name="build-and-push-an-image-from-an-app-using-a-cloud-native-buildpack"></a>Cloud Native Buildpack kullanarak bir uygulamadan görüntü oluşturma ve itme

Azure CLI `az acr pack build` komutu, bir uygulama oluşturmak ve görüntüsünü bir Azure kapsayıcı kayıt defterine itmek için [`pack`](https://github.com/buildpack/pack) [Buildpacks'ten](https://buildpacks.io/)cli aracını kullanır. Bu özellik, Bir Dockerfile tanımlamak zorunda kalmadan Node.js, Java ve diğer dillerde uygulama kaynak kodundan hızlı bir şekilde bir kapsayıcı görüntü oluşturmak için bir seçenek sağlar.

Bu makaledeki örnekleri çalıştırmak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.0.70 veya daha sonra gereklidir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="use-the-build-command"></a>Yapı komutunu kullanma

Cloud Native Buildpacks kullanarak bir kapsayıcı görüntüsü oluşturmak ve itmek için [az acr paketi oluşturma][az-acr-pack-build] komutunu çalıştırın. [Az acr build][az-acr-build] komutu bir Dockerfile kaynağından ve ilgili koddan `az acr pack build` bir görüntü oluşturur ve iter, doğrudan bir uygulama kaynak ağacı belirtin.

En azından çalıştırdığınızda `az acr pack build`aşağıdakileri belirtin:

* Komutu çalıştırdığınız bir Azure kapsayıcı kayıt defteri
* Ortaya çıkan görüntü için bir resim adı ve etiketi
* Yerel dizin, GitHub repo'su veya uzak bir tarball gibi ACR Görevleri için [desteklenen bağlam konumlarından](container-registry-tasks-overview.md#context-locations) biri
* Uygulamanız için uygun bir Buildpack oluşturucu görüntüsünün adı. Azure Konteyner Kayıt Defteri, daha `cloudfoundry/cnb:0.0.34-cflinuxfs3` hızlı yapılar gibi oluşturucu görüntüleri önbelleğe almaz.  

`az acr pack build`akışlı ve daha sonra almak için kaydedilen [çalışma değişkenleri](container-registry-tasks-reference-yaml.md#run-variables) ve [görev çalıştırma günlükleri](container-registry-tasks-logs.md) de dahil olmak üzere ACR Görevleri komutlarının diğer özelliklerini destekler.

## <a name="example-build-nodejs-image-with-cloud-foundry-builder"></a>Örnek: Bulut Döküm oluşturucusu ile Düğüm.js görüntü oluşturma

Aşağıdaki örnek, `cloudfoundry/cnb:0.0.34-cflinuxfs3` oluşturucuyu kullanarak Azure [Örnekleri/nodejs-docs-hello-world](https://github.com/Azure-Samples/nodejs-docs-hello-world) repo'daki Bir Düğüm.js uygulamasından bir kapsayıcı görüntüsü oluşturur. Bu oluşturucu Azure Kapsayıcı Kayıt Defteri tarafından `--pull` önbelleğe alınmış olduğundan bir parametre gerekmez:

```azurecli
az acr pack build \
    --registry myregistry \
    --image {{.Run.Registry}}/node-app:1.0 \
    --builder cloudfoundry/cnb:0.0.34-cflinuxfs3 \
    https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

Bu örnek, `node-app` görüntüyü `1.0` etiketle oluşturur ve *nüfus defteri* kapsayıcı kayıt defterine iter. Bu örnekte, hedef kayıt defteri adı açıkça görüntü adına hazırlanır. Belirtilmemişse, kayıt defteri giriş sunucusu adı otomatik olarak görüntü adına hazırlanır.

Komut çıktısı, görüntü oluşturma ve itme ilerlemesini gösterir. 

Görüntü başarıyla oluşturulmuş sonra, eğer yüklü varsa, Docker ile çalıştırabilirsiniz. Kayıt defterinizde ilk oturum:

```azurecli
az acr login --name myregistry
```

Görüntüyü çalıştırın:

```console
docker run --rm -p 1337:1337 myregistry.azurecr.io/node-app:1.0
```

Örnek web `localhost:1337` uygulamasını görmek için en sevdiğiniz tarayıcıya göz atın. Konteynırı durdurmak için basın. `[Ctrl]+[C]`

## <a name="example-build-java-image-with-heroku-builder"></a>Örnek: Heroku oluşturucu ile Java görüntüsü oluşturun

Aşağıdaki örnek, `heroku/buildpacks:18` oluşturucuyu kullanarak [buildpack/sample-java-app](https://github.com/buildpack/sample-java-app) repo'daki Java uygulamasından bir kapsayıcı görüntüsü oluşturur. Parametre, `--pull` komutun en son oluşturucu görüntüyü çekmesi gerektiğini belirtir. 

```azurecli
az acr pack build \
    --registry myregistry \
    --image java-app:{{.Run.ID}} \
    --pull --builder heroku/buildpacks:18 \
    https://github.com/buildpack/sample-java-app.git
```

Bu örnek, `java-app` komutun çalıştırılatan kimliğiyle etiketlenmiş görüntüyü oluşturur ve *nüfus defteri* kapsayıcı kayıt defterine iter.

Komut çıktısı, görüntü oluşturma ve itme ilerlemesini gösterir. 

Görüntü başarıyla oluşturulmuş sonra, eğer yüklü varsa, Docker ile çalıştırabilirsiniz. Kayıt defterinizde ilk oturum:

```azurecli
az acr login --name myregistry
```

*Runid*için görüntü etiketiyerine, görüntü çalıştırın:

```console
docker run --rm -p 8080:8080 myregistry.azurecr.io/java-app:runid
```

Örnek web `localhost:8080` uygulamasını görmek için en sevdiğiniz tarayıcıya göz atın. Konteynırı durdurmak için basın. `[Ctrl]+[C]`


## <a name="next-steps"></a>Sonraki adımlar

Bir kapsayıcı görüntüsünü oluşturup `az acr pack build`itmeden sonra, istediğiniz görüntü gibi istediğiniz hedefe dağıtabilirsiniz. Azure dağıtım seçenekleri arasında [Uygulama Hizmeti'nde](../app-service/containers/tutorial-custom-docker-image.md) veya [Azure Kubernetes Hizmeti'nde](../aks/tutorial-kubernetes-deploy-cluster.md)çalıştırılanın diğerleri de yer alıyor.

ACR Görevleri özellikleri hakkında daha fazla bilgi için, [ACR Görevleri ile kapsayıcı görüntü oluşturma ve bakım otomatikleştir'e](container-registry-tasks-overview.md)bakın.


<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr/task
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
