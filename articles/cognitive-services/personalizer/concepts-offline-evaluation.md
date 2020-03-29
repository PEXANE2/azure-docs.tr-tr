---
title: Çevrimdışı Değerlendirme yöntemini kullanın - Personalizer
titleSuffix: Azure Cognitive Services
description: Bu makalede, uygulamanızın etkinliğini ölçmek ve öğrenme döngünüzü analiz etmek için çevrimdışı değerlendirmenin nasıl kullanılacağı açıklanacaktır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: diberry
ms.openlocfilehash: f8ceef5e80bf15f0ba52a9c289e617018febfb5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77623600"
---
# <a name="offline-evaluation"></a>Çevrimdışı değerlendirme

Çevrimdışı değerlendirme, kodunuzu değiştirmeden veya kullanıcı deneyimini etkilemeden Kisilikhizmetinin etkinliğini test etmenizi ve değerlendirmenizi sağlayan bir yöntemdir. Çevrimdışı değerlendirme, farklı sıralamaların nasıl performans gösterdiğini karşılaştırmak için uygulamanızdan Rank ve Reward API'lerine gönderilen geçmiş verileri kullanır.

Çevrimdışı değerlendirme bir tarih aralığında gerçekleştirilir. Aralık geçerli saat kadar geç bitirebilir. Aralığın [başlangıcı, veri saklama](how-to-settings.md)için belirtilen gün sayısından fazla olamaz.

Çevrimdışı değerlendirme, aşağıdaki soruları yanıtlamanıza yardımcı olabilir:

* Personalizer başarılı kişiselleştirme için ne kadar etkili?
    * Personalizer online makine öğrenme politikası ile elde edilen ortalama ödüller nelerdir?
    * Personalizer, uygulamanın varsayılan olarak ne yapacağının etkinliğiyle nasıl karşılaştırılır?
    * Ne Kişiselleştirme için rasgele bir seçim karşılaştırmalı etkinliği olurdu?
    * El ile belirlenen farklı öğrenme politikalarının karşılaştırmalı etkinliği ne olurdu?
* Bağlamın hangi özellikleri başarılı kişiselleştirmeye az çok katkıda bulunuyor?
* Eylemlerin hangi özellikleri başarılı kişiselleştirmeye az ya da çok katkıda bulunuyor?

Buna ek olarak, Çevrimdışı Değerlendirme, Personalizer'ın gelecekte sonuçları iyileştirmek için kullanabileceği daha optimize edilmiş öğrenme ilkelerini keşfetmek için kullanılabilir.

Çevrimdışı değerlendirmeler, keşif için kullanılacak olayların yüzdesi konusunda kılavuz sağlamaz.

## <a name="prerequisites-for-offline-evaluation"></a>Çevrimdışı değerlendirme için ön koşullar

Temsilci çevrimdışı değerlendirme için önemli hususlar şunlardır:

* Yeterli veriye sahip. Önerilen minimum en az 50.000 olaydır.
* Temsili kullanıcı davranışı ve trafik içeren dönemlere ait verileri toplayın.

## <a name="discovering-the-optimized-learning-policy"></a>Optimize edilmiş öğrenme politikasını keşfetme

Personalizer otomatik olarak daha optimal bir öğrenme ilkesi keşfetmek için çevrimdışı değerlendirme işlemini kullanabilirsiniz.

Çevrimdışı değerlendirmeyi yaptıktan sonra, Personalizer'ın bu yeni politikayla karşılaştırmalı etkinliğini geçerli çevrimiçi politikayla karşılaştırıldığında görebilirsiniz. Daha sonra bu öğrenme politikasını Personalizer'da hemen etkili hale getirmek için, indirerek ve Modeller ve Politika paneline yükleyerek uygulayabilirsiniz. Ayrıca gelecekteki analiz veya kullanım için indirebilirsiniz.

Değerlendirmede yer alan güncel ilkeler:

| Öğrenme ayarları | Amaç|
|--|--|
|**Online Politika**| Personalizer'da kullanılan güncel Öğrenme Politikası |
|**Taban çizgisi**|Uygulamanın varsayılanı (Rank aramalarında gönderilen ilk Eylem tarafından belirlendiği gibi)|
|**Rasgele İlke**|Her zaman sağlanan eylemlerrastgele seçim döndürür hayali bir Sıralama davranışı.|
|**Özel İlkeler**|Değerlendirmeye başlarken ek Öğrenme İlkeleri yüklenir.|
|**Optimize İlke**|Değerlendirme, optimize edilmiş bir ilkeyi bulma seçeneğiyle başlatıldıysa, aynı zamanda karşılaştırılır ve geçerli politikanın yerine, onu karşılayabilir veya çevrimiçi öğrenme ilkesi haline getirebilirsiniz.|

## <a name="understanding-the-relevance-of-offline-evaluation-results"></a>Çevrimdışı değerlendirme sonuçlarının alaka düzeyini anlama

Çevrimdışı bir değerlendirme çalıştırdığınızda, sonuçların _güven sınırlarını_ çözümlemek çok önemlidir. Bunlar genişse, uygulamanızın ödül tahminlerinin kesin veya önemli olması için yeterli veri almadığı anlamına gelir. Sistem daha fazla veri biriktirdikçe ve daha uzun dönemlerde çevrimdışı değerlendirmeler çalıştırdığınızda, güven aralıkları daralır.

## <a name="how-offline-evaluations-are-done"></a>Çevrimdışı değerlendirmeler nasıl yapılır?

Çevrimdışı **Değerlendirmeler, Counterfactual Evaluation**adı verilen bir yöntem kullanılarak yapılır.

Personalizer, kullanıcıların davranışlarının (ve dolayısıyla ödüllerin) geriye dönük olarak tahmin edilmesinin imkansız olduğu varsayımıüzerine kuruludur (Kullanıcıya gördüklerinden farklı bir şey gösterilmiş olsaydı ne olurdu bilemeyiz) ve sadece ölçülen ödüller.

Bu değerlendirmeler için kullanılan kavramsal süreçtir:

```
[For a given _learning policy), such as the online learning policy, uploaded learning policies, or optimized candidate policies]:
{
    Initialize a virtual instance of Personalizer with that policy and a blank model;

    [For every chronological event in the logs]
    {
        - Perform a Rank call

        - Compare the reward of the results against the logged user behavior.
            - If they match, train the model on the observed reward in the logs.
            - If they don't match, then what the user would have done is unknown, so the event is discarded and not used for training or measurement.

    }

    Add up the rewards and statistics that were predicted, do some aggregation to aid visualizations, and save the results.
}
```

Çevrimdışı değerlendirme yalnızca gözlenen kullanıcı davranışını kullanır. Bu işlem, özellikle uygulamanız çok sayıda eylemle sıralama çağrıları yapıyorsa, büyük hacimli verileri atar.


## <a name="evaluation-of-features"></a>Özelliklerin değerlendirilmesi

Çevrimdışı değerlendirmeler, eylemler veya bağlam için belirli özelliklerin ne kadarının daha yüksek ödüller için tartArttığı hakkında bilgi sağlayabilir. Bilgiler, verilen süre ve verilere göre değerlendirme kullanılarak hesaplanır ve zamana göre değişebilir.

Özellik değerlendirmelerine bakmanızı ve şunları sormanızı öneririz:

* Uygulamanız veya sisteminiz daha etkili olanların çizgisinde başka hangi, ek özellikler sağlayabilir?
* Düşük etkinlik nedeniyle hangi özellikler kaldırılabilir? Düşük etkinlik özellikleri makine öğrenimine _gürültü_ katar.
* Yanlışlıkla dahil olan herhangi bir özellik var mı? Bunlara örnek olarak şunlar verilebilir: kullanıcı tanımlayıcı bilgiler, yinelenen kimlikler, vb.
* Düzenleyici veya sorumlu kullanım konuları nedeniyle kişiselleştirmek için kullanılmaması gereken istenmeyen özellikler var mı? İstenmeyen özellikler proxy olabilir (yani, yakından ayna veya ilişkili) özellikleri var mı?


## <a name="next-steps"></a>Sonraki adımlar

[Yapılandırın Personalizer](how-to-settings.md)
[Çalıştır Çevrimdışı Değerlendirmeler](how-to-offline-evaluation.md) Nasıl [Personalizer Çalışır](how-personalizer-works.md) anlayın
