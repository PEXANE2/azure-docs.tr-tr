---
title: Dil desteği-Metin Analizi API'si
titleSuffix: Azure Cognitive Services
description: "Metin Analizi API'si tarafından desteklenen doğal dillerin bir listesi. Bu makalede her bir işlem için hangi dillerin desteklendiği açıklanmaktadır: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve varlık tanıma."
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: aahi
ms.openlocfilehash: c5a413a4fe8d9ac9b7aac59ca78cedc6d5a7a313
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78899445"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Metin Analizi API'si için dil ve bölge desteği

Bu makalede her bir işlem için hangi dillerin desteklendiği açıklanmaktadır: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve adlandırılmış varlık tanıma.

## <a name="language-detection"></a>Dil Algılama

Metin Analizi API'si, çok çeşitli diller, çeşitler, diapacts ve bazı bölgesel/kültürel dillerini algılayabilir.  Dil Algılama, bir dilin "betiğini" döndürür. Örneğin, "bir köpek var" ifadesi için `en-US`yerine `en` döndürülür. Tek özel durum, dil algılama yeteneğinin `zh_CHS` veya `zh_CHT` verilen metni tespit edebilir. Belirli bir betiğin bir Çince belge için belirlenemediği durumlarda yalnızca `zh`döndürür.

Bu özellik için dillerin tam listesini yayımlamadık, ancak çeşitli diller, çeşitler, diapacts ve bazı bölgesel/kültürel dillerini algılayabilir. 

Daha az sıklıkta kullanılan bir dilde ifade ettiğiniz bir içeriğiniz varsa, bir kodu döndürüp döndürdüğünü görmek için Dil Algılama deneyebilirsiniz. Tespit edilemez dillerin yanıtı `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Yaklaşım Analizi, Anahtar İfade Ayıklama ve adlandırılmış varlık tanıma

Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanıma için, desteklenen dillerin listesi, çözümleyiciler ek dillerin dil kurallarına uyum sağlayacak şekilde iyileştirilmişse daha seçmeli olur. Adlandırılmış varlık tanıma v2 'de, [varlık türlerinin](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) tam kümesi için destek şu anda aşağıdaki dillerle sınırlıdır: 
* Türkçe
* Çince-Basitleştirilmiş
* Fransızca
* Almanca
* İspanyolca

Diğer diller için yalnızca `Person`, `Location` ve `Organization` adlandırılmış varlıklar döndürülür.

## <a name="language-list-and-status"></a>Dil listesi ve durumu

Dil desteği başlangıçta, genel kullanıma açık (GA) duruma, her biri birbirinden bağımsız olarak ve Metin Analizi hizmetten bağımsız olarak kullanıma sunulmuştur. Dillerin, genel kullanıma açık olan Metin Analizi API'si geçiş sırasında bile önizlemede kalması mümkündür.

> [!NOTE]
> Adlandırılmış varlık tanıma (NER) v3 genel önizlemesi için ayrıntılı dil desteği için bkz. [adlandırılmış varlık türleri](named-entity-types.md).

| Dil              | Dil kodu | Yaklaşım | Anahtar tümceleri | Adlandırılmış Varlık Tanıma | Varlık bağlama |       Notlar        |
|:----------------------|:-------------:|:---------:|:-----------:|:------------------------:|:--------------:|:------------------:|
| Arapça                |     `ar`      |           |             |           ✔ \*           |                |                    |
| Çekçe                 |     `cs`      |           |             |           ✔ \*           |                |                    |
| Çince-Basitleştirilmiş    |   `zh-hans`   |  ✔ \*\*   |             |            ✔             |                | `zh` de kabul edildi                   |
| Çince-Geleneksel   |   `zh-hant`   |  ✔ \*\*   |             |                          |                |                    |
| Danca                |     `da`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Felemenkçe                 |     `nl`      |   ✔ \**   |      ✔      |           ✔ \*           |                |                    |
| Türkçe               |     `en`      |   ✔ \**   |      ✔      |          ✔ \*\*          |     ✔ \**      |                    |
| Fince               |     `fi`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Fransızca                |     `fr`      |   ✔ \**   |      ✔      |            ✔             |                |                    |
| Almanca                |     `de`      |   ✔ \**   |      ✔      |            ✔             |                |                    |
| Yunanca                 |     `el`      |   ✔ \*    |             |                          |                |                    |
| Macarca             |     `hu`      |           |             |           ✔ \*           |                |                    |
| İtalyanca               |     `it`      |   ✔ \**   |      ✔      |           ✔ \*           |                |                    |
| Japonca              |     `ja`      |   ✔ \**   |      ✔      |           ✔ \*           |                |                    |
| Korece                |     `ko`      |   ✔ \*\*  |      ✔      |           ✔ \*           |                |                    |
| Norveççe (Bokmål)   |     `no`      |   ✔ \*    |      ✔      |           ✔ \*           |                | `nb` de kabul edildi                   |
| Lehçe                |     `pl`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Portekizce (Portekiz) |    `pt-PT`    |   ✔\**    |      ✔      |           ✔ \*           |                | `pt` de kabul edildi |
| Portekizce (Brezilya)   |    `pt-BR`    |           |      ✔      |           ✔ \*           |                |                    |
| Rusça               |     `ru`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| İspanyolca               |     `es`      |   ✔\**    |      ✔      |           ✔ \*           |     ✔ \**      |                    |
| İsveççe               |     `sv`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Türkçe               |     `tr`      |   ✔ \*    |             |           ✔ \*           |                |                    |

\* dil desteği önizlemede

\** [yaklaşım Analizi v3](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis#sentiment-analysis-versions-and-features) ve/veya [adlandırılmış varlık tanıma v3](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) Genel önizlemelerinde de mevcuttur.

## <a name="see-also"></a>Ayrıca bkz.

Bilişsel [Hizmetler belge sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
