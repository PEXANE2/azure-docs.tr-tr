---
title: İstek limitleri-Translator Metin Çevirisi API'si
titleSuffix: Azure Cognitive Services
description: Bu makalede Translator Metin Çevirisi API'si için istek sınırları listelenmektedir. Ücretler, istek başına 5.000 karakter sınırlaması olan istek sıklığıyla değil, karakter sayısına göre ücretlendirilir. Karakter sınırları, abonelik tabanlıdır ve F0 ile 2.000.000 karakter arasında sınırlanır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: swmachan
ms.openlocfilehash: 8d26efec2783d6f121c319e46b1b505b6e1b1e09
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79498939"
---
# <a name="request-limits-for-translator-text"></a>Translator Metin Çevirisi için istek sınırları

Bu makale Translator Metin Çevirisi API'si için azaltma sınırları sağlar. Hizmetler çeviri, alfabelik, cümle uzunluğu algılama, dil algılama ve alternatif çeviriler içerir.

## <a name="character-and-array-limits-per-request"></a>İstek başına karakter ve dizi sınırları

Her çeviri isteği, çeviri yaptığınız tüm hedef dillerde 5.000 karakterle sınırlıdır. Örneğin, 3 farklı dile çevirmek için 1.500 karakterlik bir çeviri isteği gönderilmesi, istek sınırını karşılayan 1, 500 x3 = 4.500 karakterlik bir istek boyutuyla sonuçlanır. İstek sayısına göre değil, karakter başına ücretlendirilirsiniz. Daha kısa istekler göndermeniz önerilir.

Aşağıdaki tabloda Translator Metin Çevirisi API'si her bir işlemi için dizi öğesi ve karakter sınırları listelenmektedir.

| İşlem | Dizi öğesinin en büyük boyutu |   En fazla dizi öğesi sayısı |  En fazla Istek boyutu (karakter) |
|:----|:----|:----|:----|
| Çevirme | 5.000 | 100   | 5.000 |
| Karakter Dönüştürme | 5.000 | 10    | 5.000 |
| Algılama | 10,000 | 100 |   50.000 |
| BreakSentence | 10,000    | 100 | 50.000 |
| Sözlük Arama| 100 |  10  | 1000 |
| Sözlük Örnekleri | "metin için 100 ve çeviri için 100 (200 toplam)| 10|   2.000 |

## <a name="character-limits-per-hour"></a>Saat başına karakter sınırları

Saat başına karakter sınırınız Translator Metin Çevirisi abonelik katmanınızı temel alır. 

Saatlik kota, saat boyunca eşit olarak tüketilmelidir. Örneğin, saat başına 2.000.000 karakterlik F0 katman sınırında, karakterler dakikada kabaca 33.300 karakterden daha hızlı bir şekilde tüketilmelidir (2.000.000 karakter, 60 dakikaya bölünür).

Bu sınırlara ulaşan veya daha fazla bilgi alırsanız veya kısa bir süre içinde kotanın bir kısmını çok büyük bir süre gönderirseniz, muhtemelen kota dışı bir yanıt alacaksınız. Eşzamanlı isteklerde sınır yoktur.

| Katman | Karakter sınırı |
|------|-----------------|
| F0 | 2.000.000 karakter/saat |
| S1 | 40.000.000 karakter/saat |
| S2/C2 | 40.000.000 karakter/saat |
| S3/C3 | 120.000.000 karakter/saat |
| S4/C4 | 200.000.000 karakter/saat |

[Çok hizmet abonelikleri](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) limitleri S1 katmanıyla aynı.

Bu sınırlar, Microsoft 'un standart çeviri modelleriyle kısıtlıdır. Özel çevirici kullanan özel çeviri modelleri saniyede 1.800 karakter ile sınırlıdır.

## <a name="latency"></a>Gecikme süresi

Translator Metin Çevirisi API'si, özel modeller kullanılırken Standart modeller ve 120 saniye kullanarak en fazla 15 saniyelik gecikme süresine sahiptir. Genellikle, *100 karakter içindeki metin* yanıtları 150 milisaniyelik ile 300 milisaniyeye döndürülür. Özel çevirmen modelleri sürekli istek hızında benzer gecikme özelliklerine sahiptir ve istek hızlarınız aralıklı olduğunda daha yüksek bir gecikme süresine sahip olabilir. Yanıt süreleri, istek ve dil çiftinin boyutuna bağlı olarak değişir. Bu zaman diliminde bir çeviri veya [hata yanıtı](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) almazsanız lütfen kodunuzu, ağ bağlantınızı kontrol edin ve yeniden deneyin. 

## <a name="sentence-length-limits"></a>Tümce uzunluğu sınırları

[Breakcümlesi](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence) işlevini kullanırken, cümle uzunluğu 275 karakterle sınırlıdır. Bu diller için özel durumlar mevcuttur:

| Dil | Kod | Karakter sınırı |
|----------|------|-----------------|
| Çince | zh | 132 |
| Almanca | seçimini | 290 |
| İtalyanca | içerdiği | 280 |
| Japonca | Sofya | 150 |
| Portekizce | pt | 290 |
| İspanyolca | es | 280 |
| İtalyanca | içerdiği | 280 |
| Tayca | 11 | 258 |

> [!NOTE]
> Bu sınır, çeviriler için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

* [Fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Bölgesel kullanılabilirlik](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [v3 Translator Metin Çevirisi API’si başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
