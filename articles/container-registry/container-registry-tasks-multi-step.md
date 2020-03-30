---
title: Oluşturmak için çok aşamalı görev, test & yama görüntü
description: Azure Kapsayıcı Kayıt Defteri'ndeki ACR Görevleri'nin bulutta kapsayıcı görüntüleri oluşturmak, test etmek ve yamalamak için görev tabanlı iş akışları sağlayan bir özelliği olan çok aşamalı görevlere giriş.
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: 0dcd38559d3f50715f982de4c9c80bfe9c6c8433
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399704"
---
# <a name="run-multi-step-build-test-and-patch-tasks-in-acr-tasks"></a>ACR Görevlerinde çok aşamalı oluşturma, test etme ve düzeltme eki görevler çalıştırma

Çok aşamalı görevler, çok adımlı, çok kapsayıcı tabanlı iş akışlarıyla ACR Görevlerinin tek görüntü oluşturma ve itme yeteneğini genişletir. Birkaç görüntüyü seri veya paralel olarak oluşturmak ve itmek için çok aşamalı görevleri kullanın. Ardından bu görüntüleri tek bir görev çalışması içinde komut olarak çalıştırın. Her adım bir kapsayıcı görüntü oluşturma veya itme işlemini tanımlar ve bir kapsayıcının yürütülmesini de tanımlayabilir. Çok adımlı görevdeki her adım, yürütme ortamı olarak bir kapsayıcı kullanır.

> [!IMPORTANT]
> Önizlemede daha önce `az acr build-task` komutuyla görev oluşturduysanız [az acr task][az-acr-task] komutuyla bu görevleri yeniden oluşturmanız gerekebilir.

Örneğin, aşağıdaki mantığı otomatikleştiren adımlarla bir görev çalıştırabilirsiniz:

1. Web uygulaması görüntüsü oluşturma
1. Web uygulama kapsayıcısını çalıştırma
1. Web uygulaması test görüntüsü oluşturma
1. Çalışan uygulama kapsayıcısına karşı testler gerçekleştiren web uygulama test konteynerini çalıştırın
1. Testler geçerse, bir Miğfer grafiği arşiv paketi oluşturun
1. Yeni `helm upgrade` Helm grafik arşiv paketini kullanarak gerçekleştirme

Tüm adımlar Azure içinde gerçekleştirilir, bu da çalışmayı Azure'un işlem kaynaklarına boşaltır ve sizi altyapı yönetiminden kurtarır. Azure konteyner kayıt defterinizin yanı sıra, yalnızca kullandığınız kaynaklar için ödeme yapıyorsun. Fiyatlandırma hakkında daha fazla bilgi için [Azure Konteyner Kayıt Defteri fiyatlandırmasındaki][pricing]Kapsayıcı **Yapısı** bölümüne bakın.


## <a name="common-task-scenarios"></a>Sık karşılaşılan görev senaryoları

Çok adımlı görevler aşağıdaki mantık gibi senaryoları etkinleştirin:

* Bir veya daha fazla kapsayıcı görüntüsünü seri veya paralel olarak oluşturun, etiketleyin ve itin.
* Birim testini ve kod kapsamı sonuçlarını çalıştırın ve yakalayın.
* İşlevsel testleri çalıştırın ve yakalayın. ACR Görevleri, aralarında bir dizi istek yürüterek birden fazla kapsayıcı çalıştırmayı destekler.
* Kapsayıcı görüntü oluşturmanın ön/posta adımları da dahil olmak üzere görev tabanlı yürütme gerçekleştirin.
* En sevdiğiniz dağıtım motoruyla hedef ortamınıza bir veya daha fazla kapsayıcı dağıtın.

## <a name="multi-step-task-definition"></a>Çok adımlı görev tanımı

ACR Görevleri'nde çok adımlı bir görev, YAML dosyasındaki bir dizi adım olarak tanımlanır. Her adım, bir veya daha fazla önceki adımın başarıyla tamamlanmasına ilişkin bağımlılıkları belirtebilir. Aşağıdaki görev adımı türleri kullanılabilir:

* [`build`](container-registry-tasks-reference-yaml.md#build): Tanıdık `docker build` sözdizimini kullanarak, seri veya paralel olarak bir veya daha fazla kapsayıcı görüntüsü oluşturun.
* [`push`](container-registry-tasks-reference-yaml.md#push): Yapılı görüntüleri konteyner kayıt defterine itin. Azure Konteyner Kayıt Defteri gibi özel kayıtlar ve ortak Docker Hub desteklenir.
* [`cmd`](container-registry-tasks-reference-yaml.md#cmd): Çalışan görev bağlamında bir işlev olarak çalışabilecek bir kapsayıcı çalıştırın. Parametreleri kapsayıcının ,'s'una `[ENTRYPOINT]`geçirebilir ve env, ayırma ve `docker run` diğer tanıdık parametreler gibi özellikleri belirtebilirsiniz. Adım `cmd` türü, eşzamanlı kapsayıcı yürütme ile birim ve işlevsel test sağlar.

Aşağıdaki parçacıklar, bu görev adımı türlerinin nasıl birleştirilen gösteriş gösterir. Çok aşamalı görevler, Dockerfile'dan tek bir görüntü oluşturmak ve kayıt defterinize itmek kadar basit olabilir, benzer bir YAML dosyası yla:

```yml
version: v1.1.0
steps:
  - build: -t $Registry/hello-world:$ID .
  - push: ["$Registry/hello-world:$ID"]
```

Veya daha karmaşık, yapı, test, dümen paketi ve dümen dağıtımı (konteyner kayıt defteri ve Helm deposu yapılandırma gösterilmez) için adımları içeren bu hayali çok adımlı tanımı gibi:

```yml
version: v1.1.0
steps:
  - id: build-web
    build: -t $Registry/hello-world:$ID .
    when: ["-"]
  - id: build-tests
    build -t $Registry/hello-world-tests ./funcTests
    when: ["-"]
  - id: push
    push: ["$Registry/helloworld:$ID"]
    when: ["build-web", "build-tests"]
  - id: hello-world-web
    cmd: $Registry/helloworld:$ID
  - id: funcTests
    cmd: $Registry/helloworld:$ID
    env: ["host=helloworld:80"]
  - cmd: $Registry/functions/helm package --app-version $ID -d ./helm ./helm/helloworld/
  - cmd: $Registry/functions/helm upgrade helloworld ./helm/helloworld/ --reuse-values --set helloworld.image=$Registry/helloworld:$ID
```

Çok adımlı görev YAML dosyaları ve çeşitli senaryolar için Dockerfiles için [görev örneklerine](container-registry-tasks-samples.md) bakın.

## <a name="run-a-sample-task"></a>Örnek bir görev çalıştırma

Görevler, "hızlı çalıştırma" adı verilen el ile yürütmeyi ve Git commit veya temel görüntü güncelleştirmesi üzerinde otomatik yürütmeyi destekler.

Bir görevi çalıştırmak için önce görevin adımlarını YAML dosyasında tanımlarsınız, sonra Azure CLI komut [az kısaltma çalıştırırsınız.][az-acr-run]

Aşağıda, örnek bir görev YAML dosyasını kullanarak bir görevi çalıştıran bir örnek Azure CLI komutu verilmiştir. Adımları oluşturmak ve sonra bir görüntü itin. Komutu çalıştırmadan önce kendi Azure kapsayıcı kayıt defterinizin adıyla güncelleştirin. `\<acrName\>`

```azurecli
az acr run --registry <acrName> -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

Görevi çalıştırdığınızda, çıktı YAML dosyasında tanımlanan her adımın ilerlemesini göstermelidir. Aşağıdaki çıktıda, adımlar ve `acb_step_0` `acb_step_1`.

```azurecli
az acr run --registry myregistry -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

```output
Sending context to registry: myregistry...
Queued a run with ID: yd14
Waiting for an agent...
2018/09/12 20:08:44 Using acb_vol_0467fe58-f6ab-4dbd-a022-1bb487366941 as the home volume
2018/09/12 20:08:44 Creating Docker network: acb_default_network
2018/09/12 20:08:44 Successfully set up Docker network: acb_default_network
2018/09/12 20:08:44 Setting up Docker configuration...
2018/09/12 20:08:45 Successfully set up Docker configuration
2018/09/12 20:08:45 Logging in to registry: myregistry.azurecr-test.io
2018/09/12 20:08:46 Successfully logged in
2018/09/12 20:08:46 Executing step: acb_step_0
2018/09/12 20:08:46 Obtaining source code and scanning for dependencies...
2018/09/12 20:08:47 Successfully obtained source code and scanned for dependencies
Sending build context to Docker daemon  109.6kB
Step 1/1 : FROM hello-world
 ---> 4ab4c602aa5e
Successfully built 4ab4c602aa5e
Successfully tagged myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:48 Executing step: acb_step_1
2018/09/12 20:08:48 Pushing image: myregistry.azurecr-test.io/hello-world:yd14, attempt 1
The push refers to repository [myregistry.azurecr-test.io/hello-world]
428c97da766c: Preparing
428c97da766c: Layer already exists
yd14: digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812 size: 524
2018/09/12 20:08:55 Successfully pushed image: myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:55 Step id: acb_step_0 marked as successful (elapsed time in seconds: 2.035049)
2018/09/12 20:08:55 Populating digests for step id: acb_step_0...
2018/09/12 20:08:57 Successfully populated digests for step id: acb_step_0
2018/09/12 20:08:57 Step id: acb_step_1 marked as successful (elapsed time in seconds: 6.832391)
The following dependencies were found:
- image:
    registry: myregistry.azurecr-test.io
    repository: hello-world
    tag: yd14
    digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
  git: {}


Run ID: yd14 was successful after 19s
```

Git commit veya temel görüntü güncelleştirmesi üzerindeki otomatik yapılar hakkında daha fazla bilgi için, [Bkz. Otomatikleştir görüntü yapılışı](container-registry-tutorial-build-task.md) ve [Temel görüntü güncelleştirmesi](container-registry-tutorial-base-image-update.md) öğretici makaleler oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Çok aşamalı görev başvurusu ve örneklerini burada bulabilirsiniz:

* [Görev başvurusu](container-registry-tasks-reference-yaml.md) - Görev adımı türleri, özellikleri ve kullanımı.
* [Görev örnekleri](container-registry-tasks-samples.md) `task.yaml` - Karmaşıkbasitten birkaç senaryo için örnek ve Docker dosyaları.
* [Cmd repo](https://github.com/AzureCR/cmd) - ACR görevleri için komut olarak kapsayıcıların bir koleksiyonu.

<!-- IMAGES -->

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[task-examples]: https://github.com/Azure-Samples/acr-tasks
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task]: /cli/azure/acr/task