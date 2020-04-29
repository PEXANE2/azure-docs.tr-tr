---
title: Tahmin puanları-LUO
description: Tahmin puanı, LUSıS API hizmeti 'nin, Kullanıcı utanına bağlı olarak tahmin sonuçları için sahip olduğunu gösterir.
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: 709a34f0a278d8a17267c7544583798d54167dad
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81382372"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Tahmin puanları, amaç ve varlıklar için tahmin doğruluğunu belirtir

Bir tahmin puanı, bir Kullanıcı aracının tahmin sonuçlarına güvenirlik derecesi olduğunu gösterir.

Tahmin puanı sıfır (0) ile bir (1) arasındadır. Son derece iyi bir Lune puanı 0,99 ' dir. Düşük güvenilirliğe sahip bir örnek 0,01 ' dir.

|Puan değeri|Güvenilirlik|
|--|--|
|1|kesin eşleşme|
|0.99|yüksek güvenirlik|
|0,01|düşük güvenilirlik|
|0|eşleşmeyen kesin hata|

## <a name="top-scoring-intent"></a>En iyi Puanlama hedefi

Her söylenişi tahmini, bir en yüksek puan hedefi döndürür. Bu tahmin, tahmin puanlarının sayısal bir karşılaştırmasına sahiptir.

## <a name="proximity-of-scores-to-each-other"></a>Puanlarının birbirlerine yakınlığı

İlk 2 puan aralarında çok küçük bir fark olabilir. LUO, en üst puanı döndürmekten başka bir yakınlık göstermez.

## <a name="return-prediction-score-for-all-intents"></a>Tüm amaçlar için dönüş tahmini puanı

Bir test veya uç nokta sonucu, tüm amaçları içerebilir. Bu yapılandırma, doğru QueryString ad/değer çiftini kullanarak bitiş noktasında ayarlanır.

|Tahmin API'si|QueryString adı|
|--|--|
|Yüklemesinde|`show-all-intents=true`|
|V2|`verbose=true`|

## <a name="review-intents-with-similar-scores"></a>Benzer puanları olan amaçları gözden geçirme

Tüm amaçlar için puanı gözden geçirmek, yalnızca doğru bir amaç olduğunu doğrulamak için iyi bir yoldur, ancak bir sonraki tanımlanan amaç puanı, önemli ölçüde ve sürekli olarak daha düşüktür.

Birden çok amaç, bir utterance içeriğine göre kapalı tahmin puanlarına sahip ise, LUYA amaçları arasında geçiş gösterebilir. Bu durumu onarmak için, her bir amaca göre her bir amaca uygun olarak daha fazla bağlamsal farklılıklar eklemeye devam edin veya bir sohbet bot gibi istemci uygulamasına, 2 üst amaçların nasıl işleneceği hakkında programlama seçenekleri elde edebilirsiniz.

Çok yakın puanlanmış 2 amaç, **belirleyici olmayan eğitim**nedeniyle ters çıkabilir. En üstteki puan ikinci üst ve ikinci en üst puanı ilk en iyi puan haline gelebilir. Bu durumu engellemek için, 2 amaçlarını kapsayan sözcük seçimi ve bağlamı ile bu söylik için en üst iki amaç için her birine örnek ekleyin. İki amaç aynı sayıda örnek ile aynı olmalıdır. Bir parmak izi, eğitim nedeniyle insürüme engel olmak için bir Thumb kuralı, puanlar %15 farkındır.

**Belirleyici olmayan eğitimi** [tüm verilerle birlikte eğitime](luis-how-to-train.md#train-with-all-data)dönüştürebilirsiniz.

## <a name="differences-with-predictions-between-different-training-sessions"></a>Farklı eğitim oturumları arasındaki tahminlerle ilgili farklılıklar

Aynı modeli farklı bir uygulamada eğitedığınızda ve puanlar aynı değilse, bu fark, **belirleyici olmayan bir eğitim** (rastgele bir öğe) olmasından kaynaklanır. İkinci olarak, bir aradan daha fazla hedefe kadar tüm çakışmalar, eğitime göre değişiklik gösterir.

Sohbet botunuz, bir amaç için güvenilirliği belirtmek üzere belirli bir LUO puanı gerektiriyorsa, en üstteki iki amaç arasındaki puan farkını kullanmanız gerekir. Bu durum, eğitiminde Çeşitlemeler için esneklik sağlar.

**Belirleyici olmayan eğitimi** [tüm verilerle birlikte eğitime](luis-how-to-train.md#train-with-all-data)dönüştürebilirsiniz.

## <a name="e-exponent-notation"></a>E (üs) gösterimi

Tahmin puanları, gibi 0-1 aralığının üstünde _görünen_ üs gösterimini kullanabilir `9.910309E-07`. Bu puan, çok **küçük** bir sayının göstergesidir.

|E Gösterim puanı |Gerçek puan|
|--|--|
|9.910309 e-07|.0000009910309|

<a name="punctuation"></a>

## <a name="application-settings"></a>Uygulama ayarları

Aksanların ve noktalama işaretlerinin tahmin puanlarını denetlemek için [uygulama ayarlarını](luis-reference-application-settings.md) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

LUSıS uygulamanıza varlık ekleme hakkında daha fazla bilgi edinmek için bkz. [varlık ekleme](luis-how-to-add-entities.md) .
