---
title: Metin Analizi API'sındaki yenilikler
titleSuffix: Text Analytics - Azure Cognitive Services
description: Bu makale, Azure Bilişsel Hizmetler Metin Analizi'nin yeni sürümleri ve özellikleri hakkında bilgi sağlar.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: 162e60ac8d33dc5d1951a58b0a9643b668608d7b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77188794"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Metin Analizi API’sindeki yenilikler

Text Analytics API sürekli olarak güncelleştirilir. Son gelişmelerden haberdar olmak için bu makalede, yeni sürümler ve özellikler hakkında bilgi verilmektedir.

## <a name="february-2020"></a>Şubat 2020

### <a name="sdk-support-for-text-analytics-api-v3-public-preview"></a>Text Analytics API v3 Public Preview için SDK desteği

[Birleştirilmiş Azure SDK sürümü](https://techcommunity.microsoft.com/t5/azure-sdk/january-2020-unified-azure-sdk-release/ba-p/1097290)kapsamında, Text Analytics API v3 SDK artık aşağıdaki programlama dilleri için genel önizleme olarak kullanılabilir:
   * [C #](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-csharp)
   * [Python](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-python)
   * [JavaScript (Düğüm.js)](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-javascript)
   * [Java](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-java)

> [!div class="nextstepaction"]
> [Text Analytics API v3 SDK hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3)

### <a name="named-entity-recognition-v3-public-preview"></a>Adlandırılmış Varlık Tanıma v3 genel önizleme

Metinde bulunan genel ve kişisel bilgi varlıklarının algılanmasını genişletirken, Adlandırılmış Varlık Tanıma (NER) v3 genel önizleme hizmetinde artık ek varlık türleri mevcuttur. Bu güncelleştirme, [aşağıdakileri](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) `2020-02-01`içeren model sürümünü sunar:

* Aşağıdaki genel varlık türlerinin tanınması (yalnızca İngilizce):
    * Kişi Tipi
    * Ürün
    * Olay
    * Konum altında bir alt tip olarak Jeopolitik Varlık (GPE)
    * Beceri

* Aşağıdaki kişisel bilgi varlık türlerinin tanınması (yalnızca İngilizce):
    * Kişi
    * Kuruluş
    * Miktar altında alt tip olarak yaş
    * DateTime altında alt tür olarak tarih
    * Email 
    * Telefon Numarası (yalnızca ABD)
    * URL'si
    * IP Adresi

> [!div class="nextstepaction"]
> [Adlandırılmış Varlık Tanıma v3 hakkında daha fazla bilgi edinin](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)

### <a name="october-2019"></a>Ekim 2019

#### <a name="named-entity-recognition-ner"></a>Adlandırılmış Varlık Tanıma (NER)

* Kişisel bilgi varlık türlerini tanımak için yeni bir [bitiş noktası](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionPii) (yalnızca İngilizce)

* [Varlık tanıma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral) ve [varlık bağlama](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesLinking)için ayrı uç noktaları.

* [Model sürümü,](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) `2019-10-01`hangi içerir:
    * Metinde bulunan varlıkların genişletilmiş algılama ve kategorilere ayırması. 
    * Aşağıdaki yeni varlık türlerinin tanınması:
        * Telefon numarası
        * IP adresi

Entity linking İngilizce ve İspanyolca'yı destekler. NER dil desteği varlık türüne göre değişir.

#### <a name="sentiment-analysis-v3-public-preview"></a>Duygu Analizi v3 genel önizleme

* Duyguları analiz etmek için [yeni bir son nokta.](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment)
* [Model sürümü,](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) `2019-10-01`hangi içerir:

    * API metin kategorizasyonu ve puanlama doğruluğu ve ayrıntılı önemli iyileştirmeler.
    * Metindeki farklı duygular için otomatik etiketleme.
    * Bir belge ve cümle düzeyinde duygu analizi ve çıktı. 

İngilizce (),`en`Japonca`ja`(), Çince`zh-Hans`Basitleştirilmiş (`zh-Hant`),`fr`Fransızca (`it`),`es`İtalyanca (`nl`),`pt`İspanyolca (`de`), Felemenkçe ( `Australia East`), `Central Canada` `Central US`Portekizce (), Almanca (), `West US 2`ve Almanca (), aşağıdaki bölgelerde mevcuttur: , , `East Asia` `East US` `East US 2` `North Europe` `Southeast Asia` `South Central US`, , , `UK South`, , `West Europe`, 

> [!div class="nextstepaction"]
> [Sentiment Analysis v3 hakkında daha fazla bilgi edinin](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)

## <a name="next-steps"></a>Sonraki adımlar

* [Metin Analizi API'si nedir?](overview.md)  
* [Örnek kullanıcı senaryoları](text-analytics-user-scenarios.md)
* [Yaklaşım analizi](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Dil algılama](how-tos/text-analytics-how-to-language-detection.md)
* [Varlık tanıma](how-tos/text-analytics-how-to-entity-linking.md)
* [Anahtar tümcecik çıkarma](how-tos/text-analytics-how-to-keyword-extraction.md)
