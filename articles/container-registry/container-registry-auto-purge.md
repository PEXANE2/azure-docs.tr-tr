---
title: Temizleme etiketleri ve bildirimleri
description: Yaşa ve etiket filtresine göre azure kapsayıcı kayıt defterinden birden çok etiketi ve bildirimi silmek ve isteğe bağlı olarak temizleme işlemlerini zamanlamak için bir temizleme komutu kullanın.
ms.topic: article
ms.date: 08/14/2019
ms.openlocfilehash: f9d86b628bdd0ce0db3067b02a47517d8aadcba3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087335"
---
# <a name="automatically-purge-images-from-an-azure-container-registry"></a>Azure kapsayıcı kayıt defterinden görüntüleri otomatik olarak temizleme

Bir geliştirme iş akışının parçası olarak bir Azure kapsayıcı kayıt defteri kullandığınızda, kayıt defteri kısa bir süre sonra gerekli olmayan resimler veya diğer yapılarla hızla dolabilir. Belirli bir süreden eski olan tüm etiketleri silmek veya belirtilen ad filtresiyle eşleşmek isteyebilirsiniz. Birden çok yapıyı hızlı bir şekilde `acr purge` silmek için, bu makalede, isteğe bağlı veya zamanlanmış ACR Görevi olarak çalıştırabileceğiniz komut tanıtışlar. [scheduled](container-registry-tasks-scheduled.md) 

Komut `acr purge` şu anda, GitHub'daki`mcr.microsoft.com/acr/acr-cli:0.1` [acr-cli](https://github.com/Azure/acr-cli) repo'daki kaynak kodundan oluşturulmuş ortak kapsayıcı görüntüsünde (), dağıtılır.

Bu makaledeki ACR görev örneklerini çalıştırmak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Yerel olarak kullanmak isterseniz, sürüm 2.0.69 veya daha sonra gereklidir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install]. 

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

> [!WARNING]
> Komutu `acr purge` dikkatli kullanın-silinmiş görüntü verileri KURTARılamaz. Görüntüleri manifest sindirimiile çeken sistemleriniz varsa (görüntü adının aksine), etiketlenmemiş görüntüleri temizlememelisiniz. Etiketlenmemiş görüntülerin silmesi, bu sistemlerin resimleri kayıt defterinizden çekmesini önler. Bunun yerine manifesto tarafından çekerek, *benzersiz* bir etiketleme düzeni, [önerilen en iyi uygulama](container-registry-image-tag-version.md)benimseyerek düşünün.

Azure CLI komutlarını kullanarak tek resim etiketlerini veya manifestolarını silmek istiyorsanız, [Azure Kapsayıcı Kayıt Defteri'ndeki kapsayıcı resimlerini sil'e](container-registry-delete.md)bakın.

## <a name="use-the-purge-command"></a>Temizleme komutunu kullanma

Kapsayıcı `acr purge` komutu, ad filtresiyle eşleşen ve belirli bir süreden daha eski bir depodaki etiketlere göre görüntüleri siler. Varsayılan olarak, yalnızca etiket başvuruları silinir, temel [bildirimler](container-registry-concepts.md#manifest) ve katman verileri değil. Komutun manifestoları silme seçeneği de vardır. 

> [!NOTE]
> `acr purge`özniteliğin `write-enabled` ayarlandığı bir resim etiketini veya deposunu `false`silmez. Daha fazla bilgi için, [bir Azure kapsayıcı kayıt defterinde kapsayıcı görüntüsünü kilitle'ye](container-registry-image-lock.md)bakın.

`acr purge`[ACR Görevi'nde](container-registry-tasks-overview.md)kapsayıcı komutu olarak çalışacak şekilde tasarlanmıştır, böylece görevin çalıştığı ve eylemleri gerçekleştirdiği kayıt defteriyle otomatik olarak doğrulanır. Bu makaledeki görev örnekleri, tam nitelikli kapsayıcı görüntü komutu yerine `acr purge` komut diğer [adını](container-registry-tasks-reference-yaml.md#aliases) kullanır.

En azından çalıştırdığınızda `acr purge`aşağıdakileri belirtin:

* `--filter`- Depodaki etiketleri filtrelemek için bir depo ve düzenli bir *ifade.* Örnekler: `--filter "hello-world:.*"` depodaki `hello-world` tüm etiketleri eşler `--filter "hello-world:^1.*"` ve `1`'' ile başlayan etiketleri eşler. Birden `--filter` çok depoları temizlemek için birden çok parametre geçirin.
* `--ago`- Görüntülerin silindiği sürenin ötesinde bir süreyi belirtmek için Go stili [süre dizesi.](https://golang.org/pkg/time/) Süre, her biri birim soneki olan bir veya daha fazla ondalık sayı dizisinden oluşur. Geçerli zaman birimleri günler için "d", saat için "h", dakika lar için "m" içerir. Örneğin, `--ago 2d3h6m` değiştirilen tüm filtrelenmiş görüntüleri 2 günden fazla, 3 saat ve `--ago 1.5h` 6 dakika önce seçer ve 1,5 saatten daha uzun süre önce değiştirilen görüntüleri seçer.

`acr purge`birkaç isteğe bağlı parametreyi destekler. Aşağıdaki iki bu makalede örneklerde kullanılır:

* `--untagged`- İlişkili etiketleri *(etiketlenmemiş bildirimler)* olmayan bildirimlerin silindiğini belirtir.
* `--dry-run`- Hiçbir verinin silindiğini belirtir, ancak çıktı, komut bu bayrak olmadan çalıştırılır gibi aynıdır. Bu parametre, korumak istediğiniz verileri yanlışlıkla silmediğinden emin olmak için bir temizleme komutunu sınamak için yararlıdır.

Ek parametreler için `acr purge --help`çalıştırın. 

`acr purge`akışlı ve daha sonra almak için kaydedilen [çalışma değişkenleri](container-registry-tasks-reference-yaml.md#run-variables) ve [görev çalıştırma günlükleri](container-registry-tasks-logs.md) de dahil olmak üzere ACR Görevleri komutlarının diğer özelliklerini destekler.

### <a name="run-in-an-on-demand-task"></a>İsteğe bağlı görevde çalıştırma

Aşağıdaki örnekte, isteğe bağlı komutu `acr purge` çalıştırmak için [az acr çalıştır][az-acr-run] komutu kullanır. Bu örnek, 1 günden daha uzun `hello-world` bir süre önce değiştirilen *nüfus defterimdeki* depodaki tüm resim etiketlerini ve bildirimlerini siler. Kapsayıcı komutu bir ortam değişkeni kullanılarak geçirilir. Görev kaynak bağlamı olmadan çalışır.

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge --filter 'hello-world:.*' \
  --untagged --ago 1d"

az acr run \
  --cmd "$PURGE_CMD" \
  --registry myregistry \
  /dev/null
```

### <a name="run-in-a-scheduled-task"></a>Zamanlanmış bir görevde çalıştır

Aşağıdaki örnekte, günlük [zamanlanmış ACR görevi](container-registry-tasks-scheduled.md)oluşturmak için [az acr görev oluşturma][az-acr-task-create] komutu kullanır. Görev tasfiye etiketleri depoda `hello-world` 7 günden fazla önce değiştirildi. Kapsayıcı komutu bir ortam değişkeni kullanılarak geçirilir. Görev kaynak bağlamı olmadan çalışır.

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge --filter 'hello-world:.*' \
  --ago 7d"

az acr task create --name purgeTask \
  --cmd "$PURGE_CMD" \
  --schedule "0 0 * * *" \
  --registry myregistry \
  --context /dev/null
```

Zamanlayıcı tetikleyicisinin yapılandırıldığını görmek için [az acr görev göster][az-acr-task-show] komutunu çalıştırın.

### <a name="purge-large-numbers-of-tags-and-manifests"></a>Çok sayıda etiket ve manifeste temizleme

Çok sayıda etiket ve bildirimin temizlenmesi birkaç dakika veya daha uzun sürebilir. Binlerce etiket ve bildirimi temizlemek için komutun isteğe bağlı bir görev için varsayılan zaman aşımı süresinden 600 saniye veya zamanlanmış bir görev için 3600 saniyeden daha uzun çalışması gerekebilir. Zaman aşımı süresi aşılırsa, yalnızca etiket ve bildirim alt kümesi silinir. Büyük ölçekli bir temizlemenin tamamlandığından emin `--timeout` olmak için, değeri artırmak için parametreyi geçirin. 

Örneğin, aşağıdaki isteğe bağlı görev 3600 saniye (1 saat) bir zaman zaman dilimi ayarlar:

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge --filter 'hello-world:.*' \
  --ago 1d --untagged"

az acr run \
  --cmd "$PURGE_CMD" \
  --registry myregistry \
  --timeout 3600 \
  /dev/null
```

## <a name="example-scheduled-purge-of-multiple-repositories-in-a-registry"></a>Örnek: Bir kayıt defterinde birden çok deponun zamanlanmış tasfiyesi

Bu örnek, `acr purge` bir kayıt defterindeki birden çok depoyı düzenli olarak temizlemek için kullanarak geçer. Örneğin, görüntüleri `samples/devimage1` ve `samples/devimage2` depoları iten bir geliştirme ardışık hattınız olabilir. Geliştirme görüntülerini düzenli aralıklarla dağıtımlarınız için bir üretim deposuna aktarırsınız, böylece artık geliştirme görüntülerine ihtiyacınız olmaz. Haftalık bazda, önümüzdeki hafta `samples/devimage1` nın `samples/devimage2` çalışmasına hazırlık olarak, depoları ve depoları tasfiye emzebilirsiniz.

### <a name="preview-the-purge"></a>Temizlemeyi önizleyin

Verileri silmeden önce, parametreyi `--dry-run` kullanarak isteğe bağlı temizleme görevi çalıştırmanızı öneririz. Bu seçenek, herhangi bir veri kaldırmadan komutun temizleyeceği etiketleri ve bildirimleri görmenizi sağlar. 

Aşağıdaki örnekte, her depodaki filtre tüm etiketleri seçer. Parametre, `--ago 0d` filtrelerle eşleşen depolarda her yaştan görüntülerle eşleşir. Senaryonuz için gereken seçim ölçütlerini değiştirin. `--untagged` Parametre etiketlere ek olarak bildirimleri silmek için gösterir. Kapsayıcı komutu bir ortam değişkeni kullanılarak [az acr çalıştır][az-acr-run] komutuna geçirilir.

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge \
  --filter 'samples/devimage1:.*' --filter 'samples/devimage2:.*' \
  --ago 0d --untagged --dry-run"

az acr run \
  --cmd "$PURGE_CMD" \
  --registry myregistry \
  /dev/null
```

Seçim parametreleri ile eşleşen etiketleri ve bildirimleri görmek için komut çıktısını gözden geçirin. Komut ile çalıştırıldığı `--dry-run`için, hiçbir veri silinmez.

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

### <a name="schedule-the-purge"></a>Temizlemeyi zamanlama

Kuru çalıştırmayı doğruladıktan sonra, temizlemeyi otomatikleştirmek için zamanlanmış bir görev oluşturun. Aşağıdaki örnek, önceki temizleme komutunu çalıştırmak için Pazar günü saat 1:00 UTC'de haftalık bir görev zamanlar:

```azurecli
# Environment variable for container command line
PURGE_CMD="acr purge \
  --filter 'samples/devimage1:.*' --filter 'samples/devimage2:.*' \
  --ago 0d --untagged"

az acr task create --name weeklyPurgeTask \
  --cmd "$PURGE_CMD" \
  --schedule "0 1 * * Sun" \
  --registry myregistry \
  --context /dev/null
```

Zamanlayıcı tetikleyicisinin yapılandırıldığını görmek için [az acr görev göster][az-acr-task-show] komutunu çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Kapsayıcı Kayıt Defteri'nde [resim verilerini silmek](container-registry-delete.md) için diğer seçenekler hakkında bilgi edinin.

Görüntü depolama hakkında daha fazla bilgi için [Azure Kapsayıcı Kayıt Defteri'ndeki Kapsayıcı resim depolama](container-registry-storage.md)alanına bakın.

<!-- LINKS - External -->

[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show

