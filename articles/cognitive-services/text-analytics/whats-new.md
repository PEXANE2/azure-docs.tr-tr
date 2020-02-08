---
title: Metin Analizi API'si yenilikler
titleSuffix: Text Analytics - Azure Cognitive Services
description: Bu makalede, Azure bilişsel hizmetler Metin Analizi yeni sürümler ve özellikler hakkında bilgi verilmektedir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: e3fbff7a5919d4d43656f3112cb24f2017191348
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77086794"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Metin Analizi API’sindeki yenilikler

Metin Analizi API'si, sürekli olarak güncelleştirilir. Son geliştirmelerin güncel kalması için, bu makalede yeni yayınlar ve özellikler hakkında bilgi verilmektedir.

## <a name="named-entity-recognition-v3-public-preview---february-2020"></a>Adlandırılmış varlık tanıma v3 genel önizlemesi-Şubat 2020

Ek varlık türleri artık, metinde bulunan genel ve kişisel bilgi varlıklarının algılanmasını genişlettiğimiz için adlandırılmış varlık tanıma (NER) v3 genel önizleme hizmetinde kullanılabilir. Bu güncelleştirme, aşağıdakileri içeren [model sürümü](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) `2020-02-01`tanıtır:

* Aşağıdaki genel varlık türlerini tanıma (Yalnızca Ingilizce):
    * Kişilik türü
    * Ürün
    * Olay
    * Konum altında alt tür olarak geopolitik varlık (GPE)
    * İmde

* Aşağıdaki kişisel bilgi varlığı türlerini tanıma (Yalnızca Ingilizce):
    * Kişi
    * Kuruluş
    * Miktar altında alt tür olarak yaş
    * DateTime altındaki alt tür olarak Tarih
    * E-posta 
    * Telefon numarası (yalnızca ABD)
    * URL'si
    * IP Adresi

> [!div class="nextstepaction"]
> [Adlandırılmış varlık tanıma v3 hakkında daha fazla bilgi edinin](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)

### <a name="october-2019"></a>Ekim 2019

#### <a name="named-entity-recognition-ner"></a>Adlandırılmış varlık tanıma (NER)

* Kişisel bilgi varlık türlerini (Yalnızca Ingilizce) tanıma için [Yeni bir uç nokta](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionPii)

* [Varlık tanıma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral) ve [varlık bağlama](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesLinking)için ayrı uç noktalar.

* [Model sürümü](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) `2019-10-01`şunları içerir:
    * Metinde bulunan varlıkların genişletilmiş algılanması ve kategorilere ayrılması. 
    * Aşağıdaki yeni varlık türlerinin tanınması:
        * Telefon numarası
        * IP adresi

Varlık bağlama, Ingilizce ve Ispanyolca 'yi destekler. NER dil desteği varlık türüne göre farklılık gösterir.

#### <a name="sentiment-analysis-v3-public-preview"></a>Yaklaşım Analizi v3 genel önizleme

* Yaklaşımı çözümlemek için [Yeni bir uç nokta](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment) .
* [Model sürümü](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) `2019-10-01`şunları içerir:

    * API 'nin metin kategorisi ve Puanlama açısından doğruluk ve ayrıntılarda önemli geliştirmeler.
    * Metindeki farklı sentiler için otomatik etiketleme.
    * Bir belge ve tümce düzeyinde yaklaşım Analizi ve çıktısı. 

Ingilizce (`en`), Japonca (`ja`) destekler. Çince Basitleştirilmiş (`zh-Hans`), geleneksel Çince (`zh-Hant`), Fransızca (`fr`), Italyanca (`it`), Ispanyolca (`es`), Felemenkçe (`nl`), Portekizce (`pt`) ve Almanca (`de`) ve şu bölgelerde kullanılabilir: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe`ve `West US 2`. 

> [!div class="nextstepaction"]
> [Yaklaşım Analizi v3 hakkında daha fazla bilgi edinin](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizi API'si nedir?](overview.md)  
* [Örnek kullanıcı senaryoları](text-analytics-user-scenarios.md)
* [Yaklaşım Analizi](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Dil algılama](how-tos/text-analytics-how-to-language-detection.md)
* [Varlık tanıma](how-tos/text-analytics-how-to-entity-linking.md)
* [Anahtar tümceciği ayıklama](how-tos/text-analytics-how-to-keyword-extraction.md)
