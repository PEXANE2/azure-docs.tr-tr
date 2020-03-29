---
title: İstek sınırları - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Bu makalede, Çevirmen Metin API'si için istek sınırları listelenmiştir. Ücretler karakter sayısına göre tahakkuk eder, istek başına 5.000 karakter sınırı olan sıklık isteğine bağlı olarak tahakkuk eder. Karakter sınırları abonelik tabanlıdır ve F0 saatte 2 milyon karakterle sınırlıdır.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: swmachan
ms.openlocfilehash: 8d26efec2783d6f121c319e46b1b505b6e1b1e09
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79498939"
---
# <a name="request-limits-for-translator-text"></a>Çevirmen Metni için istek sınırları

Bu makalede, Çevirmen Metin API'si için azaltma sınırları sağlanmaz. Hizmetler arasında çeviri, çeviri, cümle uzunluğu algılama, dil algılama ve alternatif çeviriler yer almaktadır.

## <a name="character-and-array-limits-per-request"></a>İstek başına karakter ve dizi sınırları

Her çeviri isteği, çevirdiğiniz tüm hedef dillerde 5.000 karakterle sınırlıdır. Örneğin, 3 farklı dile çevirmek için 1.500 karakterlik bir çeviri isteği göndermek, istek sınırını karşılayan 1.500x3 = 4.500 karakterlik bir istek boyutuyla sonuçlanır. Karakter başına ücretlendirilirsiniz, istek sayısına göre değil. Daha kısa istekler gönderilmesi önerilir.

Aşağıdaki tabloda, Çevirmen Metin API'sinin her çalışması için dizi öğesi ve karakter sınırları listelenmiştir.

| İşlem | Dizi Elemanının Maksimum Boyutu |   Maksimum Dizi Öğesi Sayısı |  Maksimum İstek Boyutu (karakterler) |
|:----|:----|:----|:----|
| Çevirme | 5.000 | 100   | 5.000 |
| Karakter Dönüştürme | 5.000 | 10    | 5.000 |
| Algılama | 10,000 | 100 |   50.000 |
| BreakSentence | 10,000    | 100 | 50.000 |
| Sözlük Arama| 100 |  10  | 1000 |
| Sözlük Örnekleri | Metin için 100 ve çeviri için 100 (200 toplam)| 10|   2.000 |

## <a name="character-limits-per-hour"></a>Saat başına karakter sınırları

Saat başına karakter sınırınız Çevirmen Metni abonelik katmanınızı temel almaktadır. 

Saatlik kota saat boyunca eşit olarak tüketilmelidir. Örneğin, saatte 2 milyon karakterlik F0 katman sınırında, karakterler dakikada yaklaşık 33.300 karakterden daha hızlı tüketilmemelidir (2 milyon karakter 60 dakikaya bölünür).

Bu sınırlara ulaşır veya aşarak ulaşırsanız veya kotanın büyük bir kısmını kısa sürede gönderirseniz, büyük olasılıkla kota dışı bir yanıt alırsınız. Eşzamanlı isteklerde sınır yoktur.

| Katman | Karakter sınırı |
|------|-----------------|
| F0 | Saatte 2 milyon karakter |
| S1 | Saatte 40 milyon karakter |
| S2 / C2 | Saatte 40 milyon karakter |
| S3 / C3 | Saatte 120 milyon karakter |
| S4 / C4 | Saatte 200 milyon karakter |

Çoklu [hizmet abonelikleri](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication) için sınırlar S1 katmanı ile aynıdır.

Bu sınırlar Microsoft'un standart çeviri modelleri ile sınırlıdır. Özel Çevirmen kullanan özel çeviri modelleri saniyede 1.800 karakterle sınırlıdır.

## <a name="latency"></a>Gecikme süresi

Çevirmen Metin API'si, standart modelleri kullanarak maksimum 15 saniye, özel modelleri kullanırken ise 120 saniye gecikme süreye sahiptir. Genellikle, *100 karakter içindeki metin yanıtları* 150 milisaniye ile 300 milisaniye arasında döndürülür. Özel çevirmen modelleri, sürekli istek oranı yla ilgili benzer gecikme süreözelliklerine sahiptir ve istek oranınız aralıklı olduğunda daha yüksek bir gecikme gecikmesi olabilir. Yanıt süreleri, isteğin ve dil çiftinin boyutuna bağlı olarak değişir. Bu süre içinde bir çeviri veya [hata yanıtı](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#errors) almazsanız, lütfen kodunuzu, ağ bağlantınızı ve yeniden deneyini kontrol edin. 

## <a name="sentence-length-limits"></a>Cümle uzunluğu sınırları

[Kesme Cümlesi](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-break-sentence) işlevini kullanırken, cümle uzunluğu 275 karakterle sınırlıdır. Bu diller için istisnalar vardır:

| Dil | Kod | Karakter sınırı |
|----------|------|-----------------|
| Çince | Zh | 132 |
| Almanca | de | 290 |
| İtalyanca | bu | 280 |
| Japonca | Ja | 150 |
| Portekizce | pt | 290 |
| İspanyolca | es | 280 |
| İtalyanca | bu | 280 |
| Tayca | inci | 258 |

> [!NOTE]
> Bu sınır çeviriler için geçerli değildir.

## <a name="next-steps"></a>Sonraki adımlar

* [Fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/translator-text-api/)
* [Bölgesel kullanılabilirlik](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
* [v3 Translator Metin Çevirisi API’si başvurusu](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference)
