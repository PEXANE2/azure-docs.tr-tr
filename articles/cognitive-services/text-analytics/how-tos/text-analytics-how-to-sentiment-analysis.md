---
title: Metin Analizi REST API ile yaklaşım analizini gerçekleştirme
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure bilişsel hizmetler Metin Analizi REST API metinle ilgili yaklaşım hakkında nasıl algılanacağı gösterilir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 05/18/2020
ms.author: aahi
ms.openlocfilehash: acd8fae81baa7ad65b8d9c321c55a6311cbf4c72
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "84141254"
---
# <a name="how-to-detect-sentiment-using-the-text-analytics-api"></a>Nasıl yapılır: Metin Analizi API'si kullanarak yaklaşımı algılama

Metin Analizi API'si Yaklaşım Analizi özelliği metni değerlendirir ve her tümce için yaklaşım puanlarını ve etiketlerini döndürür. Bu, sosyal medya, müşteri incelemeleri, tartışma forumları ve daha fazlası için olumlu ve olumsuz yaklaşımı saptamak için yararlıdır. API tarafından kullanılan AI modelleri hizmet tarafından sağlanır, yalnızca analiz için içerik göndermeniz gerekir.

Bir yaklaşım analiz isteği gönderdikten sonra, API, tümce ve belge düzeyindeki yaklaşım etiketlerini ("negatif", "nötr" ve "pozitif") ve güvenirlik puanlarını döndürür.

Yaklaşım Analizi, önizleme aşamasında çok çeşitli dilleri destekler. Daha fazla bilgi için bkz. [desteklenen diller](../text-analytics-supported-languages.md).

## <a name="sentiment-analysis-versions-and-features"></a>Sürümler ve Özellikler Yaklaşım Analizi

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

| Özellik                                   | Yaklaşım Analizi v3 | Yaklaşım Analizi v 3.1 (Önizleme) |
|-------------------------------------------|-----------------------|-----------------------------------|
| Tek ve toplu istekler için Yöntemler    | X                     | X                                 |
| Yaklaşım puanları ve etiketleme             | X                     | X                                 |
| Linux tabanlı [Docker kapsayıcısı](text-analytics-how-to-install-containers.md) | X  |  |
| Görüşün madenciliği                            |                       | X                                 |

### <a name="sentiment-scoring-and-labeling"></a>Yaklaşım Puanlama ve etiketleme

V3 'teki Yaklaşım Analizi, bir tümce ve belge düzeyinde döndürülen ve her biri için bir güven puanı bulunan yaklaşım etiketlerini metne uygular. 

Etiketler `positive` , ve ' dir `negative` `neutral` . Belge düzeyinde yaklaşım `mixed` etiketi de döndürülebilir. Belgenin yaklaşımı aşağıda belirlenir:

| Tümce yaklaşımı                                                                            | Döndürülen belge etiketi |
|-----------------------------------------------------------------------------------------------|-------------------------|
| Belgede en az bir `positive` cümle vardır. Tümcelerin geri kalanı şunlardır `neutral` . | `positive`              |
| Belgede en az bir `negative` cümle vardır. Tümcelerin geri kalanı şunlardır `neutral` . | `negative`              |
| Belgede en az bir `negative` cümle ve en az bir `positive` cümle vardır.    | `mixed`                 |
| Belgedeki tüm tümceler `neutral` .                                                  | `neutral`               |

Güvenirlik puanları 1 ile 0 arasındadır. 1 ' e yakın puan, etiketin sınıflandırmasında daha yüksek bir güven gösterir, ancak daha düşük puanlar daha düşük güvenilirliğe işaret ediyor. Her belge veya tümce içindeki güven puanları 1 ' e kadar bir ekler.

### <a name="opinion-mining"></a>Görüşün madenciliği

Fikrinizi araştırma, 3,1-Preview. 1 ' den başlayarak Yaklaşım Analizi bir özelliktir. Doğal dil Işlemede (NLP) en boy tabanlı Yaklaşım Analizi olarak da bilinen bu özellik, metinle ilgili, özelliklerle (ürünlerin veya hizmetlerin öznitelikleri gibi) ilgili daha ayrıntılı bilgiler sağlar.

Örneğin, bir müşteri bir otel hakkında "Oda harika, ancak personel daha kolay değildi" gibi bir otel hakkında geri bildirim bırakırsa, feedback araştırma, metindeki yönleri ve bunlarla ilişkili ek ve duyguları bulur:

| Görünüş | İzi    | Yaklaşım |
|--------|------------|-----------|
| Oda   | alanları      | pozitif  |
| personeli  | değişikliklerinden | negatif  |

Sonuçlarınızda dikkatli araştırma elde etmek için, `opinionMining=true` bayrağı bir yaklaşım Analizi isteğine dahil etmeniz gerekir. Fikrinizi araştırma sonuçları, yaklaşım Analizi yanıtına dahil edilir.

## <a name="sending-a-rest-api-request"></a>REST API isteği gönderiliyor 

### <a name="preparation"></a>Hazırlık

Yaklaşım analizi, üzerinde çalışmak üzere daha az miktarda metin verdiğiniz zaman daha yüksek kalitede bir sonuç üretir. Bu, büyük metin öbekleri üzerinde daha iyi performans gösteren anahtar ifade ayıklamasının tersidir. Her iki işlemden de en iyi sonuçları almak için, girişleri buna göre yeniden yapılandırmayı göz önünde bulundurun.

Bu biçimde JSON belgelerinize sahip olmanız gerekir: KIMLIK, metin ve dil.

Belge boyutunun belge başına 5.120 karakter altında olması gerekir. Koleksiyon başına en fazla 1.000 öğe (kimlik) kullanabilirsiniz. Koleksiyon, istek gövdesinde gönderilir.

## <a name="structure-the-request"></a>İsteği yapısı

Bir POST isteği oluşturun. Hızlı bir şekilde yapısına ve gönderebilmeniz için aşağıdaki başvuru bağlantılarında [Postman](text-analytics-how-to-call-api.md) veya **API test konsolunu** kullanabilirsiniz. 

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

[Yaklaşım Analizi v3 başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/Sentiment)

#### <a name="version-31-preview1"></a>[Sürüm 3,1-Önizleme. 1](#tab/version-3-1)

[Yaklaşım Analizi v 3.1 başvurusu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-1/operations/Sentiment)

---

### <a name="request-endpoints"></a>İstek uç noktaları

Azure 'da bir Metin Analizi kaynağı veya bir örneklenmiş [metin analizi kapsayıcısı](text-analytics-how-to-install-containers.md)kullanarak yaklaşım ANALIZI için HTTPS uç noktasını ayarlayın. Kullanmak istediğiniz sürüm için doğru URL 'YI dahil etmeniz gerekir. Örneğin:

> [!NOTE]
> Azure portalında Metin Analizi kaynağınız için anahtarınızı ve uç noktanızı bulabilirsiniz. Kaynak **yönetimi**altında kaynağın **hızlı başlangıç** sayfasında yer alır. 

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/sentiment`

#### <a name="version-31-preview1"></a>[Sürüm 3,1-Önizleme. 1](#tab/version-3-1)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/sentiment`

Fikrinizi araştırma sonuçları almak için parametresini eklemeniz gerekir `opinionMining=true` . Örneğin:

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.1/sentiment?opinionMining=true`

Bu parametre `false` Varsayılan olarak olarak ayarlanır. 

---

Metin Analizi API'si anahtarınızı dahil etmek için bir istek üst bilgisi ayarlayın. İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

### <a name="example-sentiment-analysis-request"></a>Örnek Yaklaşım Analizi isteği 

Aşağıda, yaklaşım analizi için gönderebileceğiniz bir içerik örneği verilmiştir. İstek biçimi her iki sürüm için de aynıdır.
    
```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "The restaurant had great food and our waiter was friendly."
    }
  ]
}
```

### <a name="post-the-request"></a>İsteği gönder

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye başına gönderebilmeniz için istek boyutu ve sayısı hakkında bilgi için genel bakış konusunun [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Metin Analizi API'si durum bilgisiz. Hesabınızda hiç veri depolanmaz ve sonuçlar yanıt içinde hemen döndürülür.


### <a name="view-the-results"></a>Sonuçları görüntüleme

Yaklaşım analizi, tüm belge ve içindeki her cümle için bir yaklaşım etiketi ve Güvenirlik puanı döndürür. 1 ' e yakın puan, etiketin sınıflandırmasında daha yüksek bir güven gösterir, ancak daha düşük puanlar daha düşük güvenilirliğe işaret ediyor. Bir belgede birden fazla tümce olabilir ve her belge veya tümce içindeki güven puanları 1 ' e kadar eklenebilir.

Hemen çıktı döndürülür. Sonuçları JSON kabul eden bir uygulamaya veya çıktıyı yerel sistemdeki bir dosyaya kaydedebilirsiniz. Sonra çıktıyı, verileri sıralamak, aramak ve işlemek için kullanabileceğiniz bir uygulamaya içeri aktarın. Çok dilli ve Emoji desteği nedeniyle, yanıt metin uzaklıkları içerebilir. Daha fazla bilgi için bkz. [uzaklıkları işleme](../concepts/text-offsets.md) .

#### <a name="version-30"></a>[Sürüm 3,0](#tab/version-3)

### <a name="sentiment-analysis-v30-example-response"></a>Yaklaşım Analizi v 3.0 örnek yanıtı

Yaklaşım Analizi v3 'ten gelen yanıtlar, çözümlenen her tümce ve belge için yaklaşım etiketleri ve puanları içerir.

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "confidenceScores": {
                "positive": 1.0,
                "neutral": 0.0,
                "negative": 0.0
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "confidenceScores": {
                        "positive": 1.0,
                        "neutral": 0.0,
                        "negative": 0.0
                    },
                    "offset": 0,
                    "length": 58,
                    "text": "The restaurant had great food and our waiter was friendly."
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

#### <a name="version-31-preview1"></a>[Sürüm 3,1-Önizleme. 1](#tab/version-3-1)

### <a name="sentiment-analysis-v31-example-response"></a>Yaklaşım Analizi v 3.1 örnek yanıtı

Yaklaşım Analizi v 3.1, **sürüm 3,0** sekmesindeki yanıt nesnesine ek olarak görüşme madenciliği sağlar. Aşağıdaki yanıtta, *restorana tümce harika yiyecek içeriyordu ve garson kullanımı kolay* iki yöne sahiptir: *yiyecek* ve *garson*. Her bir en boy `relations` özelliği, `ref` ilişkili `documents` , `sentences` ve nesnelerine URI başvurusu olan bir değer içerir `opinions` .

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "confidenceScores": {
                "positive": 1.0,
                "neutral": 0.0,
                "negative": 0.0
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "confidenceScores": {
                        "positive": 1.0,
                        "neutral": 0.0,
                        "negative": 0.0
                    },
                    "offset": 0,
                    "length": 58,
                    "text": "The restaurant had great food and our waiter was friendly.",
                    "aspects": [
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1.0,
                                "negative": 0.0
                            },
                            "offset": 25,
                            "length": 4,
                            "text": "food",
                            "relations": [
                                {
                                    "relationType": "opinion",
                                    "ref": "#/documents/0/sentences/0/opinions/0"
                                }
                            ]
                        },
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1.0,
                                "negative": 0.0
                            },
                            "offset": 38,
                            "length": 6,
                            "text": "waiter",
                            "relations": [
                                {
                                    "relationType": "opinion",
                                    "ref": "#/documents/0/sentences/0/opinions/1"
                                }
                            ]
                        }
                    ],
                    "opinions": [
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1.0,
                                "negative": 0.0
                            },
                            "offset": 19,
                            "length": 5,
                            "text": "great",
                            "isNegated": false
                        },
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1.0,
                                "negative": 0.0
                            },
                            "offset": 49,
                            "length": 8,
                            "text": "friendly",
                            "isNegated": false
                        }
                    ]
                }
            ],
            "warnings": []
        }
    ],
    "errors": [],
    "modelVersion": "2020-04-01"
}
```

---

## <a name="summary"></a>Özet

Bu makalede, Metin Analizi API'si kullanarak yaklaşım analizi için kavramları ve iş akışını öğrendiniz. Özet:

+ Yaklaşım Analizi seçili diller için kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir KIMLIK, metin ve dil kodu içerir.
+ POST isteği, `/sentiment` kişiselleştirilmiş bir [erişim anahtarı ve](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) aboneliğiniz için geçerli olan bir uç nokta kullanarak bir uç noktaya gönderilir.
+ Her belge KIMLIĞI için bir yaklaşım puanından oluşan yanıt çıkışı, JSON kabul eden herhangi bir uygulamaya akışla eklenebilir. Örneğin, Excel ve Power BI.

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizine genel bakış](../overview.md)
* [Metin Analizi istemci kitaplığını kullanma](../quickstarts/text-analytics-sdk.md)
* [Yenilikler](../whats-new.md)
