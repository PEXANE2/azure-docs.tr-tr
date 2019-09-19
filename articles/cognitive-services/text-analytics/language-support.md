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
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 953699793d81485e3828b9fb46de8523d2b7674e
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129993"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Metin Analizi API'si için dil ve bölge desteği

Bu makalede her bir işlem için hangi dillerin desteklendiği açıklanmaktadır: yaklaşım analizi, anahtar ifade ayıklama, dil algılama ve adlandırılmış varlık tanıma.

## <a name="language-detection"></a>Dil Algılama

Metin Analizi API'si, çok çeşitli diller, çeşitler, diapacts ve bazı bölgesel/kültürel dillerini algılayabilir.  Dil Algılama, bir dilin "betiğini" döndürür. Örneğin, "bir köpek var" `en` ifadesi için `en-US`yerine döndürülür. Tek özel durum, dil algılama yeteneğinin döndürdüğü `zh_CHS` veya `zh_CHT` bir komut dosyasını verilen metin olarak belirleyebileceği yalnızca Çince 'dir. Belirli bir betiğin bir Çince belge için belirlenemediği durumlarda, yalnızca `zh`döndürülür.

Bu özellik için dillerin tam listesini yayımlamadık, ancak çeşitli diller, çeşitler, diapacts ve bazı bölgesel/kültürel dillerini algılayabilir. 

Daha az sıklıkta kullanılan bir dilde ifade ettiğiniz bir içeriğiniz varsa, bir kodu döndürüp döndürdüğünü görmek için Dil Algılama deneyebilirsiniz. Tespit edilemez dillerin yanıtı `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Yaklaşım Analizi, Anahtar İfade Ayıklama ve adlandırılmış varlık tanıma

Yaklaşım analizi, anahtar ifade ayıklama ve varlık tanıma için, desteklenen dillerin listesi, çözümleyiciler ek dillerin dil kurallarına uyum sağlayacak şekilde iyileştirilmişse daha seçmeli olur. [Varlık türlerinin](how-tos/text-analytics-how-to-entity-linking.md#supported-types-for-named-entity-recognition) tam kümesi için şu anda aşağıdaki dillerle sınırlı destek verilmiştir: 
* Türkçe
* Çince-Basitleştirilmiş
* Fransızca
* Almanca
* İspanyolca

Diğer diller için yalnızca `Organization` ve adlı varlıklar döndürülür. `Location` `Person`

## <a name="language-list-and-status"></a>Dil listesi ve durumu

Dil desteği başlangıçta, genel kullanıma açık (GA) duruma, her biri birbirinden bağımsız olarak ve Metin Analizi hizmetten bağımsız olarak kullanıma sunulmuştur. Dillerin, genel kullanıma açık olan Metin Analizi API'si geçiş sırasında bile önizlemede kalması mümkündür.

| Dil    | Dil kodu | Yaklaşım | Anahtar ifadeler | Adlandırılmış Varlık Tanıma |   Notlar  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Arapça      | `ar`          |           |             | ✔\*                     | |
| Çekçe       | `cs`          |           |             | ✔\*                     | |
| Çince-Basitleştirilmiş | `zh-hans`| ✔\***     |             | ✔         |    |
| Çince-Geleneksel | `zh-hant`| ✔\***     |             |          |    |
| Danca      | `da`          | ✔\*     | ✔           | ✔\*            |     |
| Felemenkçe       | `nl`          | ✔\*     | ✔          |  ✔\*           |     |
| Türkçe     | `en`          | ✔\***       | ✔           |  ✔\*\*     |      |
| Fince     | `fi`          | ✔\*     | ✔           |  ✔\*           |     |
| Fransızca      | `fr`          | ✔\***       | ✔           |  ✔            |     |
| Almanca      | `de`          | ✔\*     | ✔           |  ✔           |     |
| Yunanca       | `el`          | ✔\*     |             |            |     |
| Macarca   | `hu`          |           |             |  ✔\*          |     | 
| İtalyanca     | `it`          | ✔\***     | ✔           |  ✔\*           |     |
| Japonca    | `ja`          | ✔\***         | ✔           |  ✔\*          |     |
| Korece      | `ko`          |          | ✔           |  ✔\*          |     |
| Norveççe (Bokmål) | `no`  | ✔\*     |  ✔          | ✔\*            |     |
| Lehçe      | `pl`          | ✔\*     |  ✔          |  ✔\*           |     |
| Portekizce (Portekiz) | `pt-PT`| ✔        |  ✔          | ✔\*      |`pt`Ayrıca kabul edildi|
| Portekizce (Brezilya)   | `pt-BR`|          |  ✔   |  ✔\*       |     |
| Rusça     | `ru`          | ✔\*     | ✔           |  ✔\*           |     |
| İspanyolca     | `es`          | ✔        | ✔           |   ✔\*\*      |     | 
| İsveççe     | `sv`          | ✔\*     | ✔           |   ✔\*          |     |
| Türkçe     | `tr`          | ✔\*     |             |   ✔\*          |  |

\*Dil desteği önizlemede

\*\*[Adlandırılmış varlık tanıma](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-ner) ve [varlık bağlama](how-tos/text-analytics-how-to-entity-linking.md#entity-linking) , bu dilin her ikisi için de kullanılabilir.  

\** * [Yaklaşım Analizi v3 genel önizlemede](https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis#sentiment-analysis-v3-public-preview) kullanılabilir

## <a name="see-also"></a>Ayrıca bkz.

[Bilişsel hizmetler belgeleri sayfası](https://docs.microsoft.com/azure/cognitive-services/)   
[Bilişsel Hizmetler Ürün sayfası](https://azure.microsoft.com/services/cognitive-services/)
