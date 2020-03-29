---
title: Tahmin puanları - LUIS
titleSuffix: Azure Cognitive Services
description: Tahmin puanı, luis API hizmetinin kullanıcı nın söyleyiş lerine dayalı tahmin sonuçları için sahip olduğu güven derecesini gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: diberry
ms.openlocfilehash: b360bc82b80e834492b524acc5c4535b0409eda1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74280817"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Tahmin puanları, amaç ve varlıklar için tahmin doğruluğunu gösterir

Tahmin puanı, LUIS'in kullanıcı söyleyiş lerinin tahmin sonuçları için sahip olduğu güven derecesini gösterir.

Tahmin puanı sıfır (0) ile bir (1) arasındadır. Luis skoruna bir örnek 0.99'dur. Düşük güven puanına örnek olarak 0,01'dir. 

|Puan değeri|Güvenilirlik|
|--|--|
|1|kesin maç|
|0.99|yüksek güven|
|0.01|düşük güven|
|0|eşleşmek için kesin başarısızlık|

## <a name="top-scoring-intent"></a>En çok puan lama niyeti

Her söyleyiş tahmini bir üst puanlama niyet döndürür. Bu tahmin, tahmin puanlarının sayısal bir karşılaştırmasIdır. 

## <a name="proximity-of-scores-to-each-other"></a>Puanların birbirine yakınlığı

En iyi 2 puan aralarında çok küçük bir fark olabilir. LUIS, en yüksek skoru geri almaktan başka bir yakınlık göstermiyor.  

## <a name="return-prediction-score-for-all-intents"></a>Tüm niyetler için tahmin puanını iade etme

Bir test veya bitiş noktası sonucu tüm amaçları içerebilir. Bu yapılandırma, doğru querystring adı/değer çifti kullanılarak bitiş noktasına ayarlanır.

|Tahmin API'si|Querystring adı|
|--|--|
|V3|`show-all-intents=true`|
|V2|`verbose=true`|

## <a name="review-intents-with-similar-scores"></a>Benzer puanlarla niyetleri gözden geçirme

Tüm niyetler için puanı gözden geçirmek, yalnızca doğru niyetin tanımlanmasının değil, aynı zamanda bir sonraki tanımlanan niyetin puanının, sözcük için önemli ölçüde ve tutarlı bir şekilde daha düşük olduğunu doğrulamak için iyi bir yoldur.

Birden çok niyet, bir söyleyiş bağlamına bağlı olarak yakın tahmin puanlarına sahipse, LUIS niyetler arasında geçiş yapabilir. Bu durumu düzeltmek için, her niyete daha geniş bir bağlamsal farklılık yelpazesiyle söz eklemeye devam edin veya sohbet botu gibi istemci uygulamasının 2 üst amacın nasıl işleyeceğiniz hakkında programlı seçimler yapması nasıI olabilir.

Çok yakından puanlanan 2 niyet, **non-deterministic eğitim**nedeniyle ters olabilir. En yüksek puan ikinci üst, ikinci üst skor ise ilk en yüksek puan olabilir. Bu durumu önlemek için, 2 niyeti farklılaştıran kelime seçimi ve bağlam ile bu söyleyiş için en iyi iki niyetin her birine örnek ifadeler ekleyin. İki niyet, aynı sayıda örnek söze sahip olmalıdır. Eğitim nedeniyle ters liği önlemek için ayrılık için başparmak kuralı, puanlar% 15 fark.

[Tüm verilerle eğitim](luis-how-to-train.md#train-with-all-data)vererek **deterministik olmayan eğitimi** kapatabilirsiniz.

## <a name="differences-with-predictions-between-different-training-sessions"></a>Farklı eğitim oturumları arasındaki öngörülerle arasındaki farklar

Aynı modeli farklı bir uygulamada eğittiyseniz ve puanlar aynı olmadığında, bu fark **deterministik olmayan bir eğitim** (rastgelelik öğesi) olmasıdır. İkinci olarak, birden fazla niyet için bir söyleyiş herhangi bir örtüşme aynı söyleyiş için üst niyet eğitime dayalı olarak değişebilir anlamına gelir.

Sohbet botunuzun bir niyete olan güvenini göstermek için belirli bir LUIS puanı gerekiyorsa, en iyi iki amaç arasındaki puan farkını kullanmalısınız. Bu durum eğitim varyasyonları için esneklik sağlar.

[Tüm verilerle eğitim](luis-how-to-train.md#train-with-all-data)vererek **deterministik olmayan eğitimi** kapatabilirsiniz.

## <a name="e-exponent-notation"></a>E (üs) gösterimi

Tahmin puanları, 0-1 aralığının üzerinde _görünarak_ üslü gösterimi `9.910309E-07`kullanabilir. Bu skor çok **az** bir sayının göstergesidir.

|E gösterim puanı |Gerçek puan|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="punctuation"></a>Noktalama işaretleri

Noktalama işaretlerini kullanma veya yok sayma hakkında [daha fazla bilgi edinin.](luis-concept-utterance.md#punctuation-marks) 

## <a name="next-steps"></a>Sonraki adımlar

Bkz. LUIS uygulamanıza nasıl varlık ekleyeceğiniz hakkında daha fazla bilgi edinmek için [taraf ekle'](luis-how-to-add-entities.md) ye bakın.
