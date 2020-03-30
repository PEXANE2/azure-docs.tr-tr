---
title: Temel görüntü güncellemeleri - Görevler
description: Uygulama kapsayıcısı resimleri için temel görüntüler ve temel resim güncelleştirmenin Azure Kapsayıcı Kayıt Defteri görevini nasıl tetikleyebilir hakkında bilgi edinin.
ms.topic: article
ms.date: 01/22/2019
ms.openlocfilehash: 017c8f8a3a15896bd6e14a54136ba713e9f9c499
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617937"
---
# <a name="about-base-image-updates-for-acr-tasks"></a>ACR Görevleri için temel görüntü güncelleştirmeleri hakkında

Bu makalede, bir uygulamanın temel resmindeki güncelleştirmeler ve bu güncelleştirmelerin bir Azure Kapsayıcı Kayıt Defteri görevini nasıl tetiklediği hakkında arka plan bilgileri verilmektedir.

## <a name="what-are-base-images"></a>Temel görüntüler nedir?

Çoğu kapsayıcı görüntüsünü tanımlayan Dockerfiles, görüntünün temel görüntüsü olarak adlandırılan bir üst görüntü *belirtir.* Normalde temel görüntüler, [Alpine Linux][base-alpine] veya [Windows Nano Server][base-windows] gibi bir işletim sistemi içerir ve kapsayıcının kalan katmanları bu işletim sistemi üzerine uygulanır. Bunlar ayrıca [Node.js][base-node] veya [.NET Core][base-dotnet] gibi uygulama çerçeveleri de içerir. Bu temel görüntüler genellikle genel yukarı görüntülere dayanır. Uygulama görsellerinizin birkaçı ortak bir temel görüntü paylaşabilir.

Temel görüntü çoğunlukla, görüntüdeki işletim sistemi veya çerçevenin yeni özelliklerini veya geliştirmelerini içermesi için görüntü bakımcısı tarafından güncelleştirilir. Temel görüntüyü güncelleştirmenin bir diğer yaygın nedeni de güvenlik yamalarıdır. Bu akış yukarı güncelleştirmeleri oluştuğunda, temel resimlerinizi kritik düzeltmeyi içerecek şekilde de güncelleştirmeniz gerekir. Her uygulama görüntüsü, temel resminize şimdi eklenen bu yukarı akım düzeltmelerini de içerecek şekilde yeniden oluşturulmalıdır.

Özel geliştirme ekibi gibi bazı durumlarda, temel görüntü işletim sistemi veya çerçeveden daha fazlasını belirtebilir. Örneğin, temel görüntü izlenmesi gereken paylaşılan bir hizmet bileşeni görüntüsü olabilir. Bir takımın üyelerinin sınama için bu temel görüntüyü izlemeleri veya uygulama görüntüleri geliştirirken görüntüyü düzenli olarak güncelleştirmeleri gerekebilir.

## <a name="track-base-image-updates"></a>Temel görüntü güncelleştirmelerini izleme

ACR Görevleri, kapsayıcının temel görüntüsü güncelleştirildiğinde sizin için görüntüleri otomatik olarak oluşturma özelliğine sahiptir.

ACR Görevler, kapsayıcı görüntüsü oluşturduğunda temel görüntü bağımlılıklarını dinamik olarak keşfeder. Sonuç olarak, bir uygulama görüntüsünün temel görüntüsünün ne zaman güncelleştirilmede algılayabilirsiniz. Önceden yapılandırılmış bir yapı göreviyle, ACR Görevleri temel görüntüye başvuran her uygulama görüntüsünü otomatik olarak yeniden oluşturabilir. Bu otomatik algılama ve yeniden oluşturma yla, ACR Görevleri, güncelleştirilmiş temel görüntünüze başvuran her uygulama görüntüsünü el ile izlemek ve güncelleştirmek için normalde gereken zaman ve çabadan tasarruf etmenizi sağlar.

## <a name="base-image-locations"></a>Temel görüntü konumları

Dockerfile'den görüntü oluşturmada, ACR görevi aşağıdaki konumlardaki temel görüntülerdeki bağımlılıkları algılar:

* Görevin çalıştığı aynı Azure kapsayıcı kayıt defteri
* Aynı veya farklı bir bölgede başka bir özel Azure kapsayıcı kayıt defteri 
* Docker Hub'da genel repo 
* Microsoft Kapsayıcı Kayıt Defteri'nde genel repo

`FROM` İfadede belirtilen temel görüntü bu konumlardan birinde bulunursa, ACR görevi, tabanı güncelleştirilmeye her zaman görüntünün yeniden oluşturulmasını sağlamak için bir kanca ekler.

## <a name="additional-considerations"></a>Diğer konular

* **Uygulama görüntüleri için temel görüntüler** - Şu anda, bir ACR görevi yalnızca uygulama *(çalışma zamanı)* görüntüleri için temel görüntü güncelleştirmelerini izler. Çok aşamalı Dockerfiles'da kullanılan ara *(buildtime)* görüntüler için temel görüntü güncelleştirmelerini izlemez.  

* **Varsayılan olarak etkin** - [Az acr görev oluşturma][az-acr-task-create] komutu ile bir ACR görevi oluşturduğunuzda, varsayılan olarak görev bir temel görüntü güncelleştirmesi tarafından tetikleyici için *etkinleştirilir.* Diğer bir `base-image-trigger-enabled` de, özellik True olarak ayarlanır. Bir görevde bu davranışı devre dışı kılmış olmak istiyorsanız, özelliği False olarak güncelleştirin. Örneğin, aşağıdaki [az acr görev güncelleştirme][az-acr-task-update] komutunu çalıştırın:

  ```azurecli
  az acr task update --myregistry --name mytask --base-image-trigger-enabled False
  ```

* **Bağımlılıkları izlemek için tetikleyici** - Bir kapsayıcı görüntüsünün temel görüntüsünü içeren bağımlılıklarını belirlemek ve izlemek için bir ACR görevini etkinleştirmek için önce görüntüyü oluşturmak için görevi **en az bir kez**tetiklemeniz gerekir. Örneğin, [az acr görev çalıştırma][az-acr-task-run] komutunu kullanarak görevi el ile tetikle.

* Temel görüntü için kararlı etiket - Temel görüntü güncelleştirmesi üzerinde bir görevi `node:9-alpine`tetiklemek **için,** temel görüntü gibi *kararlı* bir etikete sahip olmalıdır. Bu etiketleme, işletim sistemi ve çerçeve yamaları ile güncelleştirilen bir temel görüntü için en son kararlı sürümiçin tipiktir. Temel görüntü yeni bir sürüm etiketiyle güncelleştirilirse, bir görevi tetiklemez. Resim etiketleme hakkında daha fazla bilgi için [en iyi uygulamalar kılavuzuna](container-registry-image-tag-version.md)bakın. 

* **Diğer görev tetikleyicileri** - Temel görüntü güncelleştirmeleri tarafından tetiklenen bir görevde, [kaynak kodu işlemeye](container-registry-tutorial-build-task.md) veya [zamanlamaya](container-registry-tasks-scheduled.md)dayalı tetikleyicileri de etkinleştirebilirsiniz. Temel görüntü güncelleştirmesi [çok adımlı](container-registry-tasks-multi-step.md)bir görevi de tetikleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

Temel görüntü güncelleştirildikten sonra uygulama görüntüsü yapılarını otomatikleştirmek için senaryolar için aşağıdaki öğreticilere bakın:

* [Temel görüntü aynı kayıt defterinde güncelleştirildiğinde kapsayıcı görüntüsünü otomatikleştirin](container-registry-tutorial-base-image-update.md)

* [Temel görüntü farklı bir kayıt defterinde güncelleştirildiğinde kapsayıcı görüntüsünü otomatikleştirin](container-registry-tutorial-base-image-update.md)


<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-update]: /cli/azure/acr/task#az-acr-task-update
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
