---
title: Metin Analizi API'si v2 için geçiş kılavuzu
titleSuffix: Azure Cognitive Services
description: Uygulamalarınızı Metin Analizi API'si sürüm 3 ' ü kullanacak şekilde taşımayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 05/13/2020
ms.author: aahi
ms.openlocfilehash: 2fd6c908aa95ef22cbf7ea0b62b925d84d66cd28
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84143276"
---
# <a name="migrate-to-version-3x-of-the-text-analytics-api"></a>Metin Analizi API'si sürüm 3. x ' e geçirin

[!INCLUDE [v3 region availability](includes/v3-region-availability.md)]

Metin Analizi API'si sürüm 2,1 kullanıyorsanız, bu makale uygulamanızı 3. x sürümünü kullanacak şekilde yükseltmenize yardımcı olur. Sürüm 3,0 genel kullanıma sunulmuştur ve genişletilmiş [adlandırılmış varlık tanıma (ner)](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) ve [model sürümü oluşturma](concepts/model-versioning.md)gibi yeni özellikler sunar. Bir v 3.1 (v 3.1-Preview. x) önizleme sürümü de bulunur ve bu da, [fikrinizi araştırma](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)gibi özellikler ekler. V2 'de kullanılan modeller gelecekteki güncelleştirmeleri almaz. 

#### <a name="sentiment-analysis"></a>[Yaklaşım Analizi](#tab/sentiment-analysis)

## <a name="feature-changes"></a>Özellik değişiklikleri 

2,1 sürümündeki Yaklaşım Analizi, API 'ye gönderilen her belge için 0 ile 1 arasındaki yaklaşım puanlarını döndürür ve daha fazla pozitif yaklaşım gösteren 1 ' e yaklaşır. Sürüm 3 Bunun yerine yaklaşım etiketlerini ("pozitif" veya "negatif" gibi), hem tümceler hem de belge için bir bütün olarak ve bunlarla ilişkili güven puanlarını döndürür. 

## <a name="steps-to-migrate"></a>Geçiş adımları

### <a name="rest-api"></a>REST API

Uygulamanız REST API kullanıyorsa, istek bitiş noktasını, yaklaşım analizi için v3 uç noktasına güncelleştirin. Örneğin: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/sentiment` . Ayrıca, [JSON yanıtında](how-tos/text-analytics-how-to-sentiment-analysis.md#view-the-results)döndürülen yaklaşım etiketlerini kullanmak için uygulamayı güncelleştirmeniz gerekir. 

### <a name="client-libraries"></a>İstemci kitaplıkları

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

#### <a name="ner-and-entity-linking"></a>[NER ve varlık bağlama](#tab/named-entity-recognition)

## <a name="feature-changes"></a>Özellik değişiklikleri

> [!NOTE] 
> Şu anda [v3 varlık kategorileri](named-entity-types.md) yalnızca İngilizce metinde döndürülür. API, sürüm 2,1 ' de desteklendiklerinden, diğer dillerdeki istekler için sürüm 2,1 sonuçları döndürür.

Sürüm 2,1 ' de Metin Analizi API'si, adlandırılmış varlık tanıma (NER) ve varlık bağlama için bir uç nokta kullanır. Sürüm 3, genişletilmiş adlandırılmış varlık algılaması sağlar ve NER ve varlık bağlama istekleri için ayrı uç noktalar kullanır. V 3.1-Preview. 1 ' den başlayarak, NER kişisel `pii` ve sistem durumu bilgilerini de algılayabilir `phi` . 

## <a name="steps-to-migrate"></a>Geçiş adımları

### <a name="rest-api"></a>REST API

Uygulamanız REST API kullanıyorsa, istek bitiş noktasını, NER ve/veya varlık bağlama için v3 uç noktalarına güncelleştirin.

Varlık Bağlama
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/linking`

HI
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`

Ayrıca, [JSON yanıtında](how-tos/text-analytics-how-to-entity-linking.md#view-results)döndürülen [varlık kategorilerini](named-entity-types.md) kullanmak için uygulamanızı güncelleştirmeniz gerekir.

### <a name="client-libraries"></a>İstemci kitaplıkları

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]


#### <a name="language-detection"></a>[Dil algılama](#tab/language-detection)

## <a name="feature-changes"></a>Özellik değişiklikleri 

Dil algılama özelliği, uç nokta sürümünün dışında v3 'de değişmemiştir, ancak JSON yanıtı `ConfidenceScore` yerine içerir `score` . V3 Ayrıca çıktıda yalnızca tek bir dili döndürür. 

## <a name="steps-to-migrate"></a>Geçiş adımları

### <a name="rest-api"></a>REST API

Uygulamanızda REST API kullanılıyorsa, dil algılama için istek uç noktasını v3 uç noktasına güncelleştirin. Örneğin: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/languages` . Ayrıca, `ConfidenceScore` `score` [JSON yanıtında](how-tos/text-analytics-how-to-language-detection.md#step-3-view-the-results)yerine kullanmak üzere uygulamayı güncelleştirmeniz gerekecektir. 

### <a name="client-libraries"></a>İstemci kitaplıkları

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]


#### <a name="key-phrase-extraction"></a>[Anahtar tümceciği ayıklama](#tab/key-phrase-extraction)

## <a name="feature-changes"></a>Özellik değişiklikleri 

Anahtar tümceciği ayıklama özelliği, uç nokta sürümü dışındaki v3 'de değişmemiştir.

## <a name="steps-to-migrate"></a>Geçiş adımları

### <a name="rest-api"></a>REST API

Uygulamanız REST API kullanıyorsa, anahtar tümceciği ayıklama için istek uç noktasını v3 uç noktasına güncelleştirin. Örneğin, `https://<your-custom-subdomain>.api.cognitiveservices.azure.com/text/analytics/v3.0/keyPhrases`

### <a name="client-libraries"></a>İstemci kitaplıkları

[!INCLUDE [Client library migration information](includes/client-library-migration-section.md)]

---


## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizi API'si v2 başvurusu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/)
* [Metin Analizi API'si nedir?](overview.md)
* [Dil desteği](language-support.md)
* [Model sürümü oluşturma](concepts/model-versioning.md)