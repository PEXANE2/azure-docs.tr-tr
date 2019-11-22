---
title: Tahmin puanları-LUO
titleSuffix: Azure Cognitive Services
description: Tahmin puanı, LUSıS API hizmeti 'nin, Kullanıcı utanına bağlı olarak tahmin sonuçları için sahip olduğunu gösterir.
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
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280817"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Tahmin puanları, amaç ve varlıklar için tahmin doğruluğunu belirtir

Bir tahmin puanı, bir Kullanıcı aracının tahmin sonuçlarına güvenirlik derecesi olduğunu gösterir.

Tahmin puanı, sıfır (0) ve bir (1) ' dir. Bir yüksek oranda başarılara LUIS puan 0.99 örneğidir. Düşük güvenilirlik puanı 0,01 örneğidir. 

|Puanı değeri|Confidence|
|--|--|
|1|kesin eşleştir|
|0.99|yüksek güvenilirlik|
|0.01|düşük güven|
|0|eşleştirilecek kesin hatası|

## <a name="top-scoring-intent"></a>Üst Puanlama hedefi

Bir üst Puanlama amacı her utterance tahmin döndürür. Bu tahmin, tahmin puanlarının sayısal bir karşılaştırmasına sahiptir. 

## <a name="proximity-of-scores-to-each-other"></a>Puanlarının birbirlerine yakınlığı

İlk 2 puan aralarında çok küçük bir fark olabilir. LUO, en üst puanı döndürmekten başka bir yakınlık göstermez.  

## <a name="return-prediction-score-for-all-intents"></a>Tüm amaçlar için tahmin puanı döndürür

Bir test veya uç nokta sonuç, tüm hedefleri içerebilir. Bu yapılandırma, doğru QueryString ad/değer çiftini kullanarak bitiş noktasında ayarlanır.

|Tahmin API'si|QueryString adı|
|--|--|
|V3|`show-all-intents=true`|
|V2|`verbose=true`|

## <a name="review-intents-with-similar-scores"></a>Benzer puanları amaçlarıyla gözden geçirin

Tüm amaçlar için puanı gözden geçirmek, yalnızca doğru bir amaç olduğunu doğrulamak için iyi bir yoldur, ancak bir sonraki tanımlanan amaç puanı, önemli ölçüde ve sürekli olarak daha düşüktür.

LUIS, birden çok ıntents Kapat tahmin puanları, bir utterance içeriğine göre varsa amaçları arasında geçiş yapabilirsiniz. Bu durumu onarmak için, her bir amaca göre her bir amaca uygun olarak daha fazla bağlamsal farklılıklar eklemeye devam edin veya bir sohbet bot gibi istemci uygulamasına, 2 üst amaçların nasıl işleneceği hakkında programlama seçenekleri elde edebilirsiniz.

Çok yakın puanlanmış 2 amaç, **belirleyici olmayan eğitim**nedeniyle ters çıkabilir. En çok puan ikinci üst ve ilk üst puanı ikinci en çok puan haline gelir. Bu durumu engellemek için, 2 amaçlarını kapsayan sözcük seçimi ve bağlamı ile bu söylik için en üst iki amaç için her birine örnek ekleyin. İki amacı hakkında örnek konuşma aynı sayıda olmalıdır. Bir eğitim, nedeniyle tersine çevirme önlemek ayırma için udur puanları % 15 fark.

**Belirleyici olmayan eğitimi** [tüm verilerle birlikte eğitime](luis-how-to-train.md#train-with-all-data)dönüştürebilirsiniz.

## <a name="differences-with-predictions-between-different-training-sessions"></a>Farklı eğitim oturumları arasındaki tahminlerle ilgili farklılıklar

Aynı modeli farklı bir uygulamada eğitedığınızda ve puanlar aynı değilse, bu fark, **belirleyici olmayan bir eğitim** (rastgele bir öğe) olmasından kaynaklanır. İkincisi, birden fazla hedefi için bir utterance herhangi bir çakışma üst hedefi aynı utterance için eğitim göre değiştirebilirsiniz anlamına gelir.

Sohbet botunuz, bir amaç için güvenilirliği belirtmek üzere belirli bir LUO puanı gerektiriyorsa, en üstteki iki amaç arasındaki puan farkını kullanmanız gerekir. Bu durum, eğitiminde Çeşitlemeler için esneklik sağlar.

**Belirleyici olmayan eğitimi** [tüm verilerle birlikte eğitime](luis-how-to-train.md#train-with-all-data)dönüştürebilirsiniz.

## <a name="e-exponent-notation"></a>E (üstel) gösterimde

Tahmin puanları, üstel gösterim kullanabilir _görünen_ 0-1 yukarıda gibi aralığı `9.910309E-07`. Bu puanı göstergesidir bir çok **küçük** sayı.

|E gösterimi puanı |Gerçek puanı|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="punctuation"></a>Noktalama işaretleri

Noktalama işaretlerini kullanma veya yoksayma hakkında [daha fazla bilgi edinin](luis-concept-utterance.md#punctuation-marks) . 

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [varlık Ekle](luis-how-to-add-entities.md) LUIS uygulamanızı varlıklar ekleme hakkında daha fazla bilgi için.
