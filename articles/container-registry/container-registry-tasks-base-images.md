---
title: Temel görüntü güncelleştirmeleri-görevler
description: Uygulama kapsayıcısı görüntülerinin temel görüntüleri ve temel görüntü güncelleştirmesinin Azure Container Registry görevi nasıl tetikleyebileceğinizi öğrenin.
ms.topic: article
ms.date: 01/22/2019
ms.openlocfilehash: 017c8f8a3a15896bd6e14a54136ba713e9f9c499
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77617937"
---
# <a name="about-base-image-updates-for-acr-tasks"></a>ACR görevleri için temel görüntü güncelleştirmeleri hakkında

Bu makale, bir uygulamanın temel görüntüsüne yönelik güncelleştirmeler ve bu güncelleştirmelerin Azure Container Registry bir görevi nasıl tetikleyebilmesi hakkında arka plan bilgileri sağlar.

## <a name="what-are-base-images"></a>Temel görüntüler nelerdir?

Çoğu kapsayıcı görüntüsünü tanımlayan dockerfiles, görüntünün dayandığı ve genellikle *temel görüntüsü*olarak adlandırılan bir üst görüntü belirler. Temel görüntüler genellikle, kapsayıcı katmanlarının geri kalanının uygulandığı [alp Linux][base-alpine] veya [Windows nano sunucu][base-windows]gibi işletim sistemini içerir. Ayrıca, [Node. js][base-node] veya [.NET Core][base-dotnet]gibi uygulama çerçeveleri de içerebilir. Bu temel görüntüler genellikle genel yukarı akış görüntülerini temel alır. Uygulama görüntülerinizin birkaçı ortak bir temel görüntü paylaşabilir.

Temel görüntü çoğunlukla, görüntüdeki işletim sistemi veya çerçevenin yeni özelliklerini veya geliştirmelerini içermesi için görüntü bakımcısı tarafından güncelleştirilir. Temel görüntüyü güncelleştirmenin bir diğer yaygın nedeni de güvenlik yamalarıdır. Bu yukarı akış güncelleştirmeleri gerçekleştiğinde, temel görüntülerinizi de kritik bir çözüm içerecek şekilde güncelleştirmeniz gerekir. Ayrıca, bu yukarı akış düzeltmelerini temel görüntıza dahil etmek için her bir uygulama görüntüsünün yeniden oluşturulması gerekir.

Özel bir geliştirme ekibi gibi bazı durumlarda, temel görüntü, işletim sistemi veya Framework 'ten daha fazla belirtebilir. Örneğin, bir temel görüntü izlenmesi gereken bir paylaşılan hizmet bileşen görüntüsü olabilir. Bir ekibin üyelerinin test için bu temel görüntüyü izlemesi veya uygulama görüntülerini geliştirirken düzenli olarak görüntüyü güncelleştirmesi gerekebilir.

## <a name="track-base-image-updates"></a>Temel görüntü güncelleştirmelerini izleme

ACR Görevleri, kapsayıcının temel görüntüsü güncelleştirildiğinde sizin için görüntüleri otomatik olarak oluşturma özelliğine sahiptir.

ACR görevleri bir kapsayıcı görüntüsü oluşturduğunda temel görüntü bağımlılıklarını dinamik olarak bulur. Sonuç olarak, bir uygulama görüntüsünün temel görüntüsünün ne zaman güncelleştirileceğini algılayabilir. Önceden yapılandırılmış bir derleme göreviyle, ACR görevleri temel görüntüye başvuran her uygulama görüntüsünü otomatik olarak yeniden oluşturabilir. Bu otomatik algılama ve yeniden oluşturma ile, ACR görevleri, güncelleştirilmiş temel görüntenize başvuran her bir uygulama görüntüsünü el ile izlemek ve güncelleştirmek için normalde gereken zaman ve çabayı kaydeder.

## <a name="base-image-locations"></a>Taban görüntü konumları

Bir Dockerfile dosyasındaki görüntü yapıları için ACR görevi aşağıdaki konumlarda temel görüntülerde bağımlılıkları algılar:

* Görevin çalıştığı aynı Azure Container kayıt defteri
* Aynı veya farklı bir bölgedeki başka bir özel Azure Kapsayıcı kayıt defteri 
* Docker Hub 'da ortak depo 
* Microsoft Container Registry genel depo

`FROM` bildiriminde belirtilen temel görüntü bu konumlardan birinde yer alıyorsa, ACR görevi görüntünün tabanı güncelleştirildikten sonra yeniden oluşturulmasını sağlamak için bir kanca ekler.

## <a name="additional-considerations"></a>Diğer konular

* **Uygulama görüntüleri Için temel görüntüler** -Şu anda ACR görevi yalnızca uygulama (*çalışma zamanı*) görüntüleri için temel görüntü güncelleştirmelerini izler. Çok aşamalı Dockerfiles 'da kullanılan ara (*buildtime*) görüntüleri için temel görüntü güncelleştirmelerini izlemez.  

* **Varsayılan olarak etkindir** - [az ACR Task Create][az-acr-task-create] komutuyla bir ACR görevi oluşturduğunuzda, varsayılan olarak görev bir temel görüntü güncelleştirmesi *tarafından tetiklenir.* Diğer bir deyişle, `base-image-trigger-enabled` özelliği true olarak ayarlanır. Bir görevde bu davranışı devre dışı bırakmak istiyorsanız, özelliği false olarak güncelleştirin. Örneğin, şu [az ACR Task Update][az-acr-task-update] komutunu çalıştırın:

  ```azurecli
  az acr task update --myregistry --name mytask --base-image-trigger-enabled False
  ```

* **Bağımlılıkları izlemek Için Tetikle** -BIR ACR görevinin, kendi temel görüntüsünü içeren bir kapsayıcı görüntüsünün bağımlılıklarını belirlemesine ve izlemesine olanak tanımak için, önce görevi görüntüyü **en az bir kez**oluşturmak üzere tetiklemeniz gerekir. Örneğin, [az ACR Task Run][az-acr-task-run] komutunu kullanarak görevi el ile tetikleyin.

* **Temel görüntü Için kararlı etiket** -temel görüntü güncelleştirmesinde bir görevi tetiklemek için, temel görüntünün `node:9-alpine`gibi *kararlı* bir etiketi olmalıdır. Bu etiketleme, işletim sistemi ve çerçeve düzeltme ekleriyle en son kararlı sürüme güncelleştirilmiş bir temel görüntü için tipik bir noktadır. Temel görüntü yeni bir sürüm etiketiyle güncelleştirilirse bir görevi tetiklemez. Görüntü etiketleme hakkında daha fazla bilgi için [en iyi yöntemler Kılavuzu](container-registry-image-tag-version.md)' na bakın. 

* **Diğer görev Tetikleyicileri** -temel görüntü güncelleştirmeleri tarafından tetiklenen bir görevde, Tetikleyicileri [kaynak kodu işlemeye](container-registry-tutorial-build-task.md) veya [bir zamanlamaya](container-registry-tasks-scheduled.md)göre de etkinleştirebilirsiniz. Bir temel görüntü güncelleştirme, [çok adımlı bir görevi](container-registry-tasks-multi-step.md)de tetikleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bir temel görüntü güncelleştirildikten sonra uygulama görüntüsü derlemelerini otomatik hale getirmeye yönelik senaryolar için aşağıdaki öğreticilere bakın:

* [Aynı kayıt defterinde bir temel görüntü güncelleştirildiği zaman kapsayıcı görüntüsü derlemelerini otomatikleştirin](container-registry-tutorial-base-image-update.md)

* [Farklı bir kayıt defterinde temel görüntü güncelleştirildiği zaman kapsayıcı görüntüsü derlemelerini otomatikleştirin](container-registry-tutorial-base-image-update.md)


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
