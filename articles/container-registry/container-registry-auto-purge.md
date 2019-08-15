---
title: Azure Container Registry resim kaynaklarını otomatik olarak kaldır
description: Bir Azure Container Registry 'den yaş ve etiket filtresine göre birden çok etiketi ve bildirimi silmek için bir temizleme komutu kullanın ve isteğe bağlı olarak temizleme işlemlerini zamanlayın.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 08/14/2019
ms.author: danlep
ms.openlocfilehash: cd0860d5f830607d6514dbc7edab71f33c27255b
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69036923"
---
# <a name="automatically-purge-images-from-an-azure-container-registry"></a>Azure Container Registry 'den görüntüleri otomatik olarak Temizleme

Bir Azure Container Registry 'yi bir geliştirme iş akışının parçası olarak kullandığınızda, kayıt defteri kısa bir süre sonra gerek duyulmayan görüntülerle veya diğer yapıtlarla hızlıca doldurabilir. Belirli bir süreden eski olan veya belirtilen bir ad filtresiyle eşleşen tüm etiketleri silmek isteyebilirsiniz. Birden çok yapıyı hızlıca silmek için bu makalede, isteğe `acr purge` bağlı veya [Zamanlanmış](container-registry-tasks-scheduled.md) bir ACR görevi olarak çalıştırabileceğiniz komut tanıtılmaktadır. 

`acr purge` Komut şu anda Microsoft Container Registry tarafından kullanılabilen bir ortak kapsayıcı görüntüsüne dağıtılır.

Bu makalede ACR görev örneklerini çalıştırmak için Azure Cloud Shell veya yerel bir Azure CLı yüklemesi kullanabilirsiniz. Yerel olarak kullanmak isterseniz, 2.0.69 veya üzeri sürümü gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install]. 

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

> [!WARNING]
> `acr purge` Komutu dikkatli bir şekilde kullanın--silinen görüntü verileri kurtarılamaz. Bildirim özetine (görüntü adından farklı olarak) görüntüleri çeken sistemleriniz varsa etiketlenmemiş görüntüleri temizleyemezsiniz. Etiketlenmemiş görüntüleri silmek, bu sistemlerin Kayıt defterinizden görüntüleri çekmesini engeller. Bildirime göre çekmek yerine, [Önerilen en iyi yöntem](container-registry-image-tag-version.md)olan *benzersiz etiketleme* düzenini benimsede düşünün.

Azure CLı komutlarını kullanarak tek resim etiketlerini veya bildirimleri silmek istiyorsanız, bkz. [Azure Container Registry kapsayıcı görüntülerini silme](container-registry-delete.md).

## <a name="use-the-purge-command"></a>Temizle komutunu kullanma

Kapsayıcı `acr purge` komutu, bir ad filtresiyle eşleşen ve belirtilen süreden eski olan bir depodaki resimleri etikete göre siler. Varsayılan olarak, temel alınan [Bildirimler](container-registry-concepts.md#manifest) ve katman verileri değil yalnızca etiket başvuruları silinir. Komutun Ayrıca bildirimleri silme seçeneği vardır. 

> [!NOTE]
> `acr purge``write-enabled` özniteliğin ayarlandığıbirresimetiketini`false`veya depoyu silmez. Bilgi için bkz. [Azure Container Registry 'de kapsayıcı görüntüsünü kilitleme](container-registry-image-lock.md).

`acr purge`, bir [ACR görevinde](container-registry-tasks-overview.md)kapsayıcı komutu olarak çalışacak şekilde tasarlanmıştır, böylece görevin çalıştığı kayıt defteriyle otomatik olarak kimliğini doğrular. 

En azından, çalıştırdığınızda `acr purge`şunları belirtin:

* `--registry`-Komutu çalıştırdığınız Azure Container kayıt defteri. 
* `--filter`-Depodaki etiketleri filtrelemek için bir depo ve *normal bir ifade* . Örnekler: `--filter "hello-world:.*"` `hello-world` depodaki tüm etiketlerle eşleşir ve `--filter "hello-world:^1.*"` ile `1`başlayan etiketlerle eşleşir. Birden çok `--filter` depo temizlemek için birden çok parametre geçirin.
* `--ago`-Bir go stili [Duration dize](https://golang.org/pkg/time/) , resimlerin silindiği süreyi belirtir. Süre, her biri birim sonekine sahip bir veya daha fazla ondalık sayı dizisinden oluşur. Geçerli zaman birimleri günler için "d", saat için "h", dakika için "m" içerir. Örneğin, `--ago 2d3h6m` en son 2 günden daha fazla, 3 saat ve 6 dakika önce değiştirilen tüm filtrelenmiş resimleri seçer ve `--ago 1.5h` en son değiştirilen görüntüleri 1,5 saat önce seçer.

`acr purge`isteğe bağlı birkaç parametreyi destekler. Aşağıdaki ikisi bu makaledeki örneklerde kullanılmıştır:

* `--untagged`-İlişkili etiketleri (*etiketlenmemiş bildirimler*) olmayan bildirimlerin silineceğini belirtir.
* `--dry-run`-Hiçbir veri silinmediğini belirtir, ancak çıktı komutun bu bayrak olmadan çalıştırıldığı ile aynı olur. Bu parametre, korumak istediğiniz verileri yanlışlıkla silmediğinden emin olmak için bir temizleme komutunun test edilmesi için yararlıdır.

Ek parametreler için, öğesini `acr purge --help`çalıştırın. 

`acr purge`, akan ve ayrıca daha sonra alınabilmeleri için [çalışan değişkenleri](container-registry-tasks-reference-yaml.md#run-variables) ve [görev çalıştırma günlüklerini](container-registry-tasks-overview.md#view-task-logs) içeren ACR görevleri komutlarının diğer özelliklerini destekler.

### <a name="run-in-an-on-demand-task"></a>İsteğe bağlı bir görevde Çalıştır

Aşağıdaki örnek, `purge` komutu isteğe bağlı olarak çalıştırmak için [az ACR Run][az-acr-run] komutunu kullanır. Bu örnek, `hello-world` *myregistry* içindeki depodaki tüm resim etiketlerini ve bildirimlerini 1 günden daha önce değiştirilmiş olarak siler. Kapsayıcı komutu bir ortam değişkeni kullanılarak geçirilir. Görev, kaynak bağlamı olmadan çalışır.

```azurecli
# Environment variable for container command line
PURGE_CMD="mcr.microsoft.com/acr/acr-cli:0.1 purge \
  --registry {{.Run.Registry}} --filter 'hello-world:.*' --untagged --ago 1d"

az acr run \
  --cmd "$PURGE_CMD" \
  --registry myregistry \
  /dev/null
```

### <a name="run-in-a-scheduled-task"></a>Zamanlanmış bir görevde Çalıştır

Aşağıdaki örnek, günlük [Zamanlanmış ACR görevi](container-registry-tasks-scheduled.md)oluşturmak için [az ACR Task Create][az-acr-task-create] komutunu kullanır. Görev, `hello-world` havuzda en fazla 7 gün önce değiştirilen etiketleri temizler. Kapsayıcı komutu bir ortam değişkeni kullanılarak geçirilir. Görev, kaynak bağlamı olmadan çalışır.

```azurecli
# Environment variable for container command line
PURGE_CMD="mcr.microsoft.com/acr/acr-cli:0.1 purge \
  --registry {{.Run.Registry}} --filter 'hello-world:.*' --ago 7d"

az acr task create --name purgeTask \
  --cmd "$PURGE_CMD" \
  --schedule "0 0 * * *" \
  --registry myregistry \
  --context /dev/null
```

Süreölçer tetikleyicisinin yapılandırıldığını görmek için [az ACR Task Show][az-acr-task-show] komutunu çalıştırın.

### <a name="purge-large-numbers-of-tags-and-manifests"></a>Büyük sayıda etiketi ve bildirimi Temizleme

Çok sayıda etiket ve bildirim temizleme birkaç dakika veya daha uzun sürebilir. Binlerce etiket ve bildirimi temizlemek için, komutun isteğe bağlı bir görev için varsayılan zaman aşımı süresi olan 600 saniye veya zamanlanan bir görevde 3600 saniye daha uzun süre çalışması gerekebilir. Zaman aşımı süresi aşılırsa, etiketlerin ve bildirimlerin yalnızca bir alt kümesi silinir. Büyük ölçekli bir temizleme işleminin tamamlandığından emin olmak için, değeri artırmak için `--timeout` parametresini geçirin. 

Örneğin, aşağıdaki isteğe bağlı görev, 3600 saniyelik bir zaman aşımı süresi (1 saat) ayarlar:

```azurecli
# Environment variable for container command line
PURGE_CMD="mcr.microsoft.com/acr/acr-cli:0.1 purge \
  --registry {{.Run.Registry}} --filter 'hello-world:.*' --ago 1d --untagged"

az acr run \
  --cmd "$PURGE_CMD" \
  --registry myregistry \
  --timeout 3600 \
  /dev/null
```

## <a name="example-scheduled-purge-of-multiple-repositories-in-a-registry"></a>Örnek: Bir kayıt defterinde birden çok depo için zamanlanmış temizleme

Bu örnekte, bir kayıt `acr purge` defterinde düzenli olarak birden çok depo temizlemek için kullanımı gösterilmektedir. Örneğin, görüntüleri `samples/devimage1` ve `samples/devimage2` depolarına gönderen bir geliştirme işlem hattına sahip olabilirsiniz. Geliştirme görüntülerini düzenli aralıklarla dağıtımlarınız için bir üretim deposuna aktarırsınız, böylece artık geliştirme görüntülerine gerek kalmaz. Haftalık olarak, gelecek haftaki işe yönelik hazırlık `samples/devimage1` bölümünde `samples/devimage2` ve depoları temizleyemezsiniz.

### <a name="preview-the-purge"></a>Temizlemeyi önizleyin

Verileri silmeden önce, `--dry-run` parametresini kullanarak isteğe bağlı bir temizleme görevi çalıştırmayı öneririz. Bu seçenek, herhangi bir veriyi kaldırmadan komutun temizlendirilecektir etiketleri ve bildirimleri görmenizi sağlar. 

Aşağıdaki örnekte, her bir depodaki filtre tüm etiketleri seçer. `--ago 0d` Parametresi, filtrelerle eşleşen depolardaki tüm yaştaki görüntülerle eşleşir. Seçim ölçütlerini senaryonuz için gereken şekilde değiştirin. Parametresi `--untagged` , etiketlerin yanı sıra bildirimlerin silineceğini gösterir. Kapsayıcı komutu, bir ortam değişkeni kullanılarak [az ACR Run][az-acr-run] komutuna geçirilir.

```azurecli
# Environment variable for container command line
PURGE_CMD="mcr.microsoft.com/acr/acr-cli:0.1 purge \
  --registry {{.Run.Registry}} \
  --filter 'samples/devimage1:.*' --filter 'samples/devimage2:.*' \
  --ago 0d --untagged --dry-run"

az acr run \
  --cmd  "$PURGE_CMD" \
  --registry myregistry \
  /dev/null
```

Seçim parametreleriyle eşleşen etiketleri ve bildirimleri görmek için komut çıkışını gözden geçirin. Komutu ile `--dry-run`çalıştırıldığı için hiçbir veri silinmez.

Örnek çıktı:

```console
[...]
Deleting tags for repository: samples/devimage1
myregistry.azurecr.io/samples/devimage1:232889b
myregistry.azurecr.io/samples/devimage1:a21776a
Deleting manifests for repository: samples/devimage1
myregistry.azurecr.io/samples/devimage1@sha256:81b6f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e788b
myregistry.azurecr.io/samples/devimage1@sha256:3ded859790e68bd02791a972ab0bae727231dc8746f233a7949e40f8ea90c8b3
Deleting tags for repository: samples/devimage2
myregistry.azurecr.io/samples/devimage2:5e788ba
myregistry.azurecr.io/samples/devimage2:f336b7c
Deleting manifests for repository: samples/devimage2
myregistry.azurecr.io/samples/devimage2@sha256:8d2527cde610e1715ad095cb12bc7ed169b60c495e5428eefdf336b7cb7c0371
myregistry.azurecr.io/samples/devimage2@sha256:ca86b078f89607bc03ded859790e68bd02791a972ab0bae727231dc8746f233a

Number of deleted tags: 4
Number of deleted manifests: 4
[...]
```

### <a name="schedule-the-purge"></a>Temizlemeyi zamanla

Kuru çalıştırmayı doğruladıktan sonra, temizleme işlemini otomatikleştirmek için zamanlanmış bir görev oluşturun. Aşağıdaki örnek, önceki temizleme komutunu çalıştırmak için 1:00 UTC 'de Pazar günü bir haftalık görevi zamanlar:

```azurecli
# Environment variable for container command line
PURGE_CMD="mcr.microsoft.com/acr/acr-cli:0.1 purge \
  --registry {{.Run.Registry}} \
  --filter 'samples/devimage1:.*' --filter 'samples/devimage2:.*' \
  --ago 0d --untagged --dry-run"

az acr task create --name weeklyPurgeTask \
  --cmd "$PURGE_CMD" \
  --schedule "0 1 * * Sun" \
  --registry myregistry \
  --context /dev/null
```

Süreölçer tetikleyicisinin yapılandırıldığını görmek için [az ACR Task Show][az-acr-task-show] komutunu çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Container Registry [resim verilerini silmeye](container-registry-delete.md) yönelik diğer seçenekler hakkında bilgi edinin.

Görüntü depolama hakkında daha fazla bilgi için bkz. [Azure Container Registry Container Image Storage](container-registry-storage.md).

<!-- LINKS - External -->

[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show

