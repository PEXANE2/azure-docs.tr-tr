---
title: Dil desteği - Text Analytics API
titleSuffix: Azure Cognitive Services
description: 'Text Analytics API tarafından desteklenen doğal dillerin listesi. Bu makalede, her işlem için hangi dillerin desteklendiği açıklanmaktadır: duyarlılık analizi, anahtar tümcecik çıkarma, dil algılama ve varlık tanıma.'
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: aahi
ms.openlocfilehash: c5a413a4fe8d9ac9b7aac59ca78cedc6d5a7a313
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219285"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Metin Analizi API'si için dil ve bölge desteği

Bu makalede, her işlem için hangi dillerin desteklendiği açıklanmaktadır: duyarlılık analizi, anahtar tümcecik çıkarma, dil algılama ve varlık tanıma adı.

## <a name="language-detection"></a>Dil Algılama

Metin Analizi API'si çok çeşitli dilleri, varyantları, lehçeleri ve bazı bölgesel/kültürel dilleri algılayabilir.  Dil Algılama, bir dilin "komut dosyasını" döndürür. Örneğin, ifade için "Ben bir köpek var" yerine dönecektir `en` `en-US`. Tek özel durum Çince, dil algılama yeteneği `zh_CHS` `zh_CHT` dönecek veya sağlanan metin verilen komut dosyası belirleyebilirsiniz. Belirli bir komut dosyasının Çince bir belge için tanımlanamadığı durumlarda, yalnızca `zh`.

Bu özellik için dillerin tam listesini yayımlayabiliyoruz, ancak çok çeşitli dilleri, varyantları, lehçeleri ve bazı bölgesel/kültürel dilleri algılayabilir. 

Daha az kullanılan bir dilde ifade edilen içeriğiniz varsa, kodu döndürp döndürtmediğinizi görmek için Dil Algılama'yı deneyebilirsiniz. Algılanamayan dilleriçin yanıt `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Duyarlılık Analizi, Anahtar İfade Çıkarma ve İsimli Varlık Tanıma

Duyarlılık analizi, anahtar tümcecik çıkarma ve varlık tanıma için, çözümleyiciler ek dillerin dilkurallarına uyacak şekilde geliştirildikçe desteklenen dillerin listesi daha seçicidir. Adlandırılmış Varlık Tanıma v2'de, [varlık türlerinin](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) tam kümesi için destek şu anda aşağıdaki dillerle sınırlıdır: 
* Türkçe
* Basitleştirilmiş Çince
* Fransızca
* Almanca
* İspanyolca

Yalnızca `Person`, `Location` `Organization` ve adlandırılmış varlıklar diğer diller için döndürülür.

## <a name="language-list-and-status"></a>Dil listesi ve durumu

Dil desteği başlangıçta önizlemede kullanıma sunularak, birbirinden bağımsız olarak genel olarak kullanılabilir (GA) statüsüne ve genel olarak Metin Analizi hizmetinden mezun olur. Text Analytics API'si genel kullanıma geçse bile dillerin önizlemede kalması mümkündür.

> [!NOTE]
> Adlandırılmış Varlık Tanıma (NER) v3 genel önizlemesi için ayrıntılı dil desteği için [bkz.](named-entity-types.md)

| Dil              | Dil kodu | Yaklaşım | Anahtar tümcecikler | Adlandırılmış Varlık Tanıma | Varlık bağlama |       Notlar        |
|:----------------------|:-------------:|:---------:|:-----------:|:------------------------:|:--------------:|:------------------:|
| Arapça                |     `ar`      |           |             |           ✔\*           |                |                    |
| Çekçe                 |     `cs`      |           |             |           ✔\*           |                |                    |
| Basitleştirilmiş Çince    |   `zh-hans`   |  ✔\*\*   |             |            ✔             |                | `zh`ayrıca kabul                   |
| Geleneksel Çince   |   `zh-hant`   |  ✔\*\*   |             |                          |                |                    |
| Danca                |     `da`      |   ✔\*    |      ✔      |           ✔\*           |                |                    |
| Felemenkçe                 |     `nl`      |   ✔\**   |      ✔      |           ✔\*           |                |                    |
| Türkçe               |     `en`      |   ✔\**   |      ✔      |          ✔\*\*          |     ✔\**      |                    |
| Fince               |     `fi`      |   ✔\*    |      ✔      |           ✔\*           |                |                    |
| Fransızca                |     `fr`      |   ✔\**   |      ✔      |            ✔             |                |                    |
| Almanca                |     `de`      |   ✔\**   |      ✔      |            ✔             |                |                    |
| Yunanca                 |     `el`      |   ✔\*    |             |                          |                |                    |
| Macarca             |     `hu`      |           |             |           ✔\*           |                |                    |
| İtalyanca               |     `it`      |   ✔\**   |      ✔      |           ✔\*           |                |                    |
| Japonca              |     `ja`      |   ✔\**   |      ✔      |           ✔\*           |                |                    |
| Korece                |     `ko`      |   ✔\*\*  |      ✔      |           ✔\*           |                |                    |
| Norveççe (Bokmål)   |     `no`      |   ✔\*    |      ✔      |           ✔\*           |                | `nb`ayrıca kabul                   |
| Lehçe                |     `pl`      |   ✔\*    |      ✔      |           ✔\*           |                |                    |
| Portekizce (Portekiz) |    `pt-PT`    |   ✔\**    |      ✔      |           ✔\*           |                | `pt`ayrıca kabul |
| Portekizce (Brezilya)   |    `pt-BR`    |           |      ✔      |           ✔\*           |                |                    |
| Rusça               |     `ru`      |   ✔\*    |      ✔      |           ✔\*           |                |                    |
| İspanyolca               |     `es`      |   ✔\**    |      ✔      |           ✔\*           |     ✔\**      |                    |
| İsveççe               |     `sv`      |   ✔\*    |      ✔      |           ✔\*           |                |                    |
| Türkçe               |     `tr`      |   ✔\*    |             |           ✔\*           |                |                    |

\*Dil desteği önizlemede

\** Ayrıca [Sentiment Analysis v3](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis#sentiment-analysis-versions-and-features) ve / veya [Named Entity Recognition v3](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) kamu önizlemeleri mevcuttur.

## <a name="see-also"></a>Ayrıca bkz.

[Bilişsel Hizmetler Dokümantasyon sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
