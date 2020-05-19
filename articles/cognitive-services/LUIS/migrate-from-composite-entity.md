---
title: Karma varlık yükseltme-LUSıS
description: Karma varlığı, HALSıS portalındaki yükseltme işlemi ile makine tarafından öğrenilen varlığa yükseltin.
ms.topic: how-to
ms.date: 05/04/2020
ms.openlocfilehash: 9bbb03a2009bdcb6e2fa5d20aefadd4c7c99f025
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83599430"
---
# <a name="upgrade-composite-entity-to-machine-learned-entity"></a>Bileşik varlığı makineye öğrenilen varlığa yükselt

Varlıkta hata ayıklama için daha iyi bir kullanılabilirlik elde eden daha fazla tahmine sahip bir varlık oluşturmak amacıyla, bileşik varlığı makine tarafından öğrenilen varlığa yükseltin.

## <a name="current-version-model-restrictions"></a>Geçerli sürüm modeli kısıtlamaları

Yükseltme işlemi, uygulamanızda bulunan mevcut bileşik varlıkların uygulamanızın yeni bir sürümüne göre, makine tarafından öğrenilen varlıklar oluşturur. Buna Bileşik varlık alt öğeleri ve rolleri dahildir. Bu işlem, Ayrıca, yeni varlığı kullanmak için de etiketleri örnek olarak değiştirir.

## <a name="upgrade-process"></a>Yükseltme işlemi

Yükseltme işlemi:
* Her bileşik varlık için makine tarafından öğrenilen yeni varlık oluşturur.
* Alt varlıklar:
    * Alt varlık yalnızca bileşik varlıkta kullanılıyorsa, yalnızca makine tarafından öğrenilen varlığa eklenir.
    * Alt varlık bileşik içinde _ve_ ayrı bir varlık (örneğin, örnek olarak etiketlenir) olarak kullanılırsa, bir varlık olarak sürüme ve yeni makine tarafından öğrenilen varlığa bir alt varlık olarak eklenir.
    * Alt varlık bir rol kullanıyorsa, her rol aynı ada sahip bir alt varlığa dönüştürülür.
    * Alt varlık makine tarafından öğrenilen bir varlık (normal ifade, liste varlığı veya önceden oluşturulmuş varlık) ise, aynı adla yeni bir alt varlık oluşturulur ve yeni alt varlık, gerekli özelliği eklenen makine tarafından öğrenilen varlığı kullanan bir özelliğe sahiptir.
* Adlar korunur, ancak aynı alt varlık/eşdüzey düzeyde benzersiz olmalıdır. [Benzersiz adlandırma sınırlarına](luis-boundaries.md#name-uniqueness)bakın.
* Örnek söyleylerdeki Etiketler, alt varlıklara sahip yeni makine tarafından öğrenilen varlığa geçiş yaptı.

Modelinizin değişikliğini anlamak için aşağıdaki grafiği kullanın:

|Eski nesne|Yeni nesne|Notlar|
|--|--|--|
|Bileşik varlık|Yapıda makine tarafından öğrenilen varlık|Her iki nesne de üst nesnelerdir.|
|Kompozit 'ın alt varlığı **basit bir varlıktır**|alt varlık|Her iki nesne de alt nesnelerdir.|
|Kompozit 'ın alt varlığı, sayı gibi **önceden oluşturulmuş bir varlıktır**|Sayı gibi önceden oluşturulmuş varlık adına sahip alt varlık ve alt varlık, kısıtlama seçeneği _true_olarak ayarlanan önceden oluşturulmuş sayı varlığı _özelliğine_ sahiptir.|alt varlık, alt varlık düzeyinde kısıtlaması olan özelliği içerir.|
|Kompozit 'ın alt varlığı sayı gibi **önceden oluşturulmuş varlıktır** ve önceden oluşturulmuş varlık bir **role** sahiptir|rol adına sahip alt varlık ve alt varlık, kısıtlama seçeneği true olarak ayarlanan önceden oluşturulmuş sayı varlığı özelliğine sahiptir.|alt varlık, alt varlık düzeyinde kısıtlaması olan özelliği içerir.|
|Rol|alt varlık|Rol adı, alt varlık adı olur. Alt varlık, makine tarafından öğrenilen varlığın doğrudan bir alt öğesi.|

## <a name="begin-upgrade-process"></a>Yükseltme işlemini Başlat

Güncelleştirme yapmadan önce şunları yaptığınızdan emin olun:

* Yükseltmek için doğru sürümde değilseniz sürümü değiştirin


1. Bildirimden yükseltme işlemini başlatın veya bir sonraki zamanlanmış komut istemine kadar bekleyebilirsiniz.

    > [!div class="mx-imgBorder"]
    > ![Bildirimleri yükseltmeye başla](./media/update-composite-entity/notification-begin-update.png)

1. Açılır pencerede **Şimdi Yükselt**' i seçin.

1. Bilgileri **yükselttiğinizde neler olduğunu** gözden geçirin ve ardından **devam**' ı seçin.

1. Yükseltmek için listeden bileşik varlıkları seçin ve ardından **devam**' ı seçin.

1. Etkin olmayan tüm değişiklikleri, onay kutusunu seçerek geçerli sürümden yükseltilen sürüme taşıyabilirsiniz.

1. Yükseltme işlemine başlamak için **devam** ' ı seçin.

1. İlerleme çubuğu, yükseltme işleminin durumunu gösterir.

1. İşlem tamamlandığında, yeni makine tarafından öğrenilen varlıkların yeni bir eğitilen sürümü bulunmaktadır. Yeni varlığı görmek için **Yeni varlıklarınızı deneyin** ' i seçin.

    Yeni varlık için yükseltme veya eğitim başarısız olursa, bir bildirim daha fazla bilgi sağlar.

1. Varlık listesi sayfasında, yeni varlıklar tür adının yanında **Yeni** ile işaretlenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Yazarlar ve ortak çalışanlar](luis-how-to-collaborate.md)