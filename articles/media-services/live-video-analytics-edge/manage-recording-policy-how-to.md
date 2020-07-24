---
title: Kayıt ilkesini yönetme-Azure
description: Bu konuda, kayıt ilkesinin nasıl yönetileceği açıklanmaktadır.
ms.topic: how-to
ms.date: 04/27/2020
ms.openlocfilehash: d3a1be915dc1cc8714e49cc7b2fe68bbe9cad161
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87011490"
---
# <a name="manage-recording-policy"></a>Kayıt ilkesini yönetme

[Sürekli video kaydı](continuous-video-recording-concept.md)için IoT Edge, canlı video analizi 'ni kullanarak haftalık veya aylık bir videoyu buluta kaydedebilirsiniz. Azure depolama 'da yerleşik olan [yaşam döngüsü yönetimi araçlarını](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal) kullanarak bu Bulut arşivinin uzunluğunu (gün cinsinden) yönetebilirsiniz.  

Medya hizmeti hesabınız bir Azure depolama hesabına bağlı ve buluta video kaydettiğinizde içerik bir medya hizmeti varlığına [yazılır.](../latest/assets-concept.md) Her varlık, depolama hesabındaki bir kapsayıcıya eşlenir. yaşam döngüsü yönetimi, aşağıdaki gibi bir [kural](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#rules) belirtebileceğiniz bir depolama hesabı için bir [ilke](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#policy) tanımlamanızı sağlar.

```
{
  "rules": [
    {
      "name": "NinetyDayRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [ "blockBlob" ]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "delete": { "daysAfterModificationGreaterThan": 90 }
          }
        }
      }
    }
  ]
}
```

Yukarıdaki kural:

* Depolama hesabındaki tüm blok Blobları için geçerlidir.
* Blobların 30 günden fazla olduğu durumlarda [sık erişimli erişim katmanından seyrek erişimli 'e](../../storage/blobs/storage-blob-storage-tiers.md?tabs=azure-portal)taşındığını belirtir.
* Blob 90 günden daha fazla yaşlandıklarında bunlar silinir.

Canlı video analizi, videonuzu belirtilen süre içinde arşivlemediğinden, varlığınız her segment için bir blob, bir blob serisi içerecektir. Yaşam döngüsü yönetim ilkesi, eski Blobları içinde açılır ve bunları sildiğinde, medya hizmeti API 'Leri aracılığıyla kalan bloblara erişebilmeye ve bunları kayıttan yürütmeye devam edersiniz. Daha fazla bilgi için bkz. [kayıtları kayıttan yürütme](playback-recordings-how-to.md). 

## <a name="limitations"></a>Sınırlamalar

Yaşam döngüsü yönetimiyle ilgili bazı bilinen sınırlamalar aşağıda verilmiştir:

* İlkede en fazla 100 kural bulunabilir ve her kural 10 ' a kadar kapsayıcı belirtebilir. Bu nedenle, farklı kayıt ilkelerine (örneğin, Park lotu için 3 günlük arşiv, yük gözündeki kamera için 30 gün ve kullanıma alma sayacının arkasındaki kamera için 180 gün) sahip olmanız gerekiyorsa, en az 1000 kamera için kuralları özelleştirebilirsiniz.
* Yaşam döngüsü yönetim ilkesi güncelleştirmeleri anında değil. Daha fazla bilgi için [Bu SSS bölümüne](../../storage/blobs/storage-lifecycle-management-concepts.md?tabs=azure-portal#faq) bakın.
* Blobların seyrek erişimli katmana taşındığı bir ilke uygulamayı seçerseniz, arşivin o bölümünün yürütülmesi etkilenebilir. Daha fazla gecikme süresi veya tek tek tek tek bir hata olabilir. Media Services arşiv katmanındaki içeriğin kayıttan yürütülmesini desteklemez.

## <a name="next-steps"></a>Sonraki adımlar

[Kayıtların kayıttan yürütülmesi](playback-recordings-how-to.md)
