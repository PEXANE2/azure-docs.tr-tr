---
title: Text Analytics REST API ile duyarlılık analizi yapın
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Bilişsel Hizmetler Metin Analizi REST API ile metindeki duyarlılığı nasıl algılayacağınızı gösterecektir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 03/09/2020
ms.author: aahi
ms.openlocfilehash: b3c112876bfd2578e6ebaa95c6902aa9b8f832d9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79203466"
---
# <a name="how-to-detect-sentiment-using-the-text-analytics-api"></a>Nasıl yapilir: Metin Analizi API'sini kullanarak duyarlılığı algılama

Metin Analizi API'nin Duygu Analizi özelliği metni değerlendirir ve her cümle için duyarlılık puanları ve etiketleri döndürür. Bu sosyal medya, müşteri değerlendirmeleri, tartışma forumları ve daha olumlu ve olumsuz duyguları tespit etmek için yararlıdır. API tarafından kullanılan AI modelleri hizmet tarafından sağlanır, sadece analiz için içerik göndermek zorunda.

> [!TIP]
> Text Analytics ayrıca dil algılama için Linux tabanlı docker kapsayıcı görüntüsü sağlar, böylece [Text Analytics kapsayıcısını](text-analytics-how-to-install-containers.md) verilerinize yakın olarak yükleyebilir ve çalıştırabilirsiniz.

Sentiment Analysis, önizlemede daha fazla bilgi içeren çok çeşitli dilleri destekler. Daha fazla bilgi için bkz. [Desteklenen diller](../text-analytics-supported-languages.md).

## <a name="concepts"></a>Kavramlar

Text Analytics API 0 ve 1 arasında bir duyarlılık puanı oluşturmak için bir makine öğrenme sınıflandırma algoritması kullanır. 1’e yakın puanlar pozitif yaklaşımı, 0’a yakın puanlar ise negatif yaklaşımı gösterir. Metindeki tek tek varlıklar yerine belgenin tamamında duygu analizi yapılır. Bu, duyarlılık puanlarının bir belge veya cümle düzeyinde döndürüldi anlamına gelir. 

Kullanılan model metin ve duygu dernekleri geniş bir korpus ile önceden eğitilir. Metin işleme, konuşmanın bir parçası çözümlemesi, sözcük yerleştirme ve sözcük çağrışımları gibi analiz tekniklerinin bir birleşimini kullanır. Algoritma hakkında daha fazla bilgi için bkz. [Metin Analizi Tanıtımı](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/). Şu anda, kendi eğitim verilerinizi sağlamak mümkün değildir. 

Belgeler büyük bir metin bloğu yerine daha az cümle içerdiğinde, doğruluk puanlama eğilimi vardır. Nesnellik değerlendirmesi aşamasında model, belgenin tamamının nesnel mi olduğunu yoksa yaklaşım mı içerdiğini belirler. Çoğunlukla objektif olan bir belge, daha fazla işleme gerektirilmeden 0,50 puanla sonuçlanan duygu algılama aşamasına ilerlemez. Ardışık alanda devam eden belgeler için, bir sonraki aşama 0,50'nin üzerinde veya altında bir puan oluşturur. Puan, belgede algılanan duyarlılık derecesine bağlıdır.

## <a name="sentiment-analysis-versions-and-features"></a>Duygu Analizi sürümleri ve özellikleri

Text Analytics API, Sentiment Analysis'in v2 ve v3 olmak gibi iki versiyonunu sunar. Sentiment Analysis v3 (Genel önizleme), API'nin metin kategorizasyonu ve puanlamasının doğruluğu nda ve ayrıntılarında önemli iyileştirmeler sağlar.

> [!NOTE]
> * Sentiment Analysis v3 istek biçimi ve [veri sınırları](../overview.md#data-limits) önceki sürümle aynıdır.
> * Duyarlılık Analizi v3 aşağıdaki bölgelerde `Australia East`mevcuttur: `East Asia`, `East US` `East US 2`, `North Europe` `Southeast Asia` `South Central US` `UK South` `Central Canada` `Central US`, , `West Europe`, `West US 2`, , , , ve .

| Özellik                                   | Duygu Analizi v2 | Duygu Analizi v3 |
|-------------------------------------------|-----------------------|-----------------------|
| Tek ve toplu iş istekleri için yöntemler    | X                     | X                     |
| Belgenin tamamı için duyarlılık puanları  | X                     | X                     |
| Bireysel cümleler için duygu puanları |                       | X                     |
| Duygu etiketleme                        |                       | X                     |
| Model sürümü                   |                       | X                     |

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

### <a name="sentiment-scoring"></a>Duygu puanlama

Sentiment Analysis v3, metni duygu etiketleri ile sınıfa verir (aşağıda açıklanmıştır). Döndürülen puanlar, modelin metnin pozitif, negatif veya nötr olduğuna olan güvenini temsil eder. Daha yüksek değerler daha yüksek güven anlamına da tır. 

### <a name="sentiment-labeling"></a>Duygu etiketleme

Duygu Analizi v3 puanları ve etiketleri bir cümle ve belge düzeyinde döndürebilir. Skorlar ve `positive`etiketler `negative`, `neutral`, ve . Belge düzeyinde, `mixed` duygu etiketi de puan olmadan iade edilebilir. Belgenin duyarlılığı aşağıda belirlenmiştir:

| Cümle duyarlılığı                                                                            | Döndürülen belge etiketi |
|-----------------------------------------------------------------------------------------------|-------------------------|
| Belgede en `positive` az bir cümle var. Cümlelerin geri kalanı `neutral`. | `positive`              |
| Belgede en `negative` az bir cümle var. Cümlelerin geri kalanı `neutral`. | `negative`              |
| Belgede en `negative` az bir `positive` cümle ve en az bir cümle bulunmaktadır.    | `mixed`                 |
| Belgedeki tüm cümleler `neutral`.                                                  | `neutral`               |

### <a name="model-versioning"></a>Model sürümü

> [!NOTE]
> Duygu analizi için model sürümü sürümünde `v3.0-preview.1`başlayan mevcuttur.

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

### <a name="example-c-code"></a>Örnek C# kodu

[GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/TextAnalyticsSentiment.cs)Sentiment Analysis'in bu sürümünü çağıran örnek bir C# uygulaması bulabilirsiniz.


#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

### <a name="sentiment-scoring"></a>Duygu puanlama

Duygu çözümleyicisi metni ağırlıklı olarak olumlu veya olumsuz olarak sınıflar. 0 ile 1 aralığında bir puan atar. 0,5’e yakın değerler nötr veya belirsizdir. 0,5 puanı, nötr olma durumunu belirtir. Bir dize duygusallık için analiz edilenemediğinde veya duyguları olmadığında, skor her zaman tam olarak 0,5'tir. Örneğin, İngilizce dil koduyla İspanyolca bir dize geçirirseniz puan 0,5 olur.

---

## <a name="sending-a-rest-api-request"></a>REST API isteği gönderme 

### <a name="preparation"></a>Hazırlık

Duyarlılık analizi üzerinde çalışmak için metin daha küçük miktarlarda verdiğinizde daha yüksek kaliteli bir sonuç üretir. Bu, büyük metin öbekleri üzerinde daha iyi performans gösteren anahtar ifade ayıklamasının tersidir. Her iki işlemden de en iyi sonuçları elde etmek için girişleri uygun şekilde yeniden yapılandırın.

Bu biçimde JSON belgeleriniz olmalıdır: kimlik, metin ve dil.

Belge boyutu belge başına 5.120 karakterin altında olmalıdır. Koleksiyon başına en fazla 1.000 öğe (d) öğeniz olabilir. Koleksiyon, istek gövdesinde gönderilir.

## <a name="structure-the-request"></a>İsteği yapılandırma

Bir POST isteği oluşturun. Postacı'yı veya **API test** konsolu'nu aşağıdaki başvuru bağlantılarında [kullanarak](text-analytics-how-to-call-api.md) hızlı bir şekilde bir tane sini yapılandırabilir ve gönderebilirsiniz. 

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

[Duygu Analizi v3 referans](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment)

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

[Duygu Analizi v2 referans](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)

---

Azure'da bir Text Analytics kaynağı veya anında metin [analizi kapsayıcısı](text-analytics-how-to-install-containers.md)kullanarak duygu analizi için HTTPS bitiş noktasını ayarlayın. Kullanmak istediğiniz sürüm için doğru URL'yi eklemeniz gerekir. Örnek:

> [!NOTE]
> Metin Analizi kaynağınızın anahtarını ve bitiş noktanızı azure portalında bulabilirsiniz. Bunlar, **kaynak yönetimi**altında kaynağın **Hızlı başlangıç** sayfasında yer alır. 

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/sentiment`

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/sentiment`

---

Metin Analizi API anahtarınızı içerecek şekilde bir istek üstbilgisi ayarlayın. İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

### <a name="example-sentiment-analysis-request"></a>Örnek Duygu Analizi talebi 

Aşağıda, yaklaşım analizi için gönderebileceğiniz bir içerik örneği verilmiştir. İstek biçimi, API'nin her iki sürümü için de aynıdır.
    
```json
{
    "documents": [
    {
        "language": "en",
        "id": "1",
        "text": "Hello world. This is some input text that I love."
    },
    {
        "language": "en",
        "id": "2",
        "text": "It's incredibly sunny outside! I'm so happy."
    }
    ],
}
```

### <a name="post-the-request"></a>İsteğin ivetini gönderme

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye gönderebileceğiniz isteklerin boyutu ve sayısı hakkında bilgi için genel bakışta [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Text Analytics API'si devletsizdir. Hesabınızda hiçbir veri depolanır ve sonuçlar yanıtta hemen döndürülür.


### <a name="view-the-results"></a>Sonuçları görüntüleme

Duygu çözümleyicisi metni ağırlıklı olarak olumlu veya olumsuz olarak sınıflar. 0 ile 1 aralığında bir puan atar. 0,5’e yakın değerler nötr veya belirsizdir. 0,5 puanı, nötr olma durumunu belirtir. Bir dize duygusallık için analiz edilenemediğinde veya duyguları olmadığında, skor her zaman tam olarak 0,5'tir. Örneğin, İngilizce dil koduyla İspanyolca bir dize geçirirseniz puan 0,5 olur.

Hemen çıktı döndürülür. Sonuçları JSON'u kabul eden bir uygulamaya aktarabilir veya çıktıyı yerel sistemdeki bir dosyaya kaydedebilirsiniz. Ardından, çıktıyı verileri sıralamak, aramak ve işlemek için kullanabileceğiniz bir uygulamaya aktarın. Çok dilli ve emoji desteği nedeniyle, yanıt metin uzaklıkları içerebilir. Daha fazla bilgi için [uzaklıkları nasıl işleyeceğinize](../concepts/text-offsets.md) bakın.

#### <a name="version-30-preview"></a>[Sürüm 3.0-önizleme](#tab/version-3)

### <a name="sentiment-analysis-v3-example-response"></a>Duygu Analizi v3 örnek yanıt

Sentiment Analysis v3'ten gelen yanıtlar, analiz edilen her cümle ve belge için duygu etiketleri ve puanları içerir. `documentScores`belge duyarlılığı etiketi `mixed`.

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.98570585250854492,
                "neutral": 0.0001625834556762,
                "negative": 0.0141316400840878
            },
            "sentences": [
                {
                    "sentiment": "neutral",
                    "sentenceScores": {
                        "positive": 0.0785155147314072,
                        "neutral": 0.89702343940734863,
                        "negative": 0.0244610067456961
                    },
                    "offset": 0,
                    "length": 12
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.98570585250854492,
                        "neutral": 0.0001625834556762,
                        "negative": 0.0141316400840878
                    },
                    "offset": 13,
                    "length": 36
                }
            ]
        },
        {
            "id": "2",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.89198976755142212,
                "neutral": 0.103382371366024,
                "negative": 0.0046278294175863
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.78401315212249756,
                        "neutral": 0.2067587077617645,
                        "negative": 0.0092281140387058
                    },
                    "offset": 0,
                    "length": 30
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.99996638298034668,
                        "neutral": 0.0000060341349126,
                        "negative": 0.0000275444017461
                    },
                    "offset": 31,
                    "length": 13
                }
            ]
        }
    ],
    "errors": []
}
```

#### <a name="version-21"></a>[Sürüm 2.1](#tab/version-2)

### <a name="sentiment-analysis-v2-example-response"></a>Duygu Analizi v2 örnek yanıt

Sentiment Analysis v2'den gelen yanıtlar, gönderilen her belge için duyarlılık puanları içerir.

```json
{
  "documents": [{
    "id": "1",
    "score": 0.98690706491470337
  }, {
    "id": "2",
    "score": 0.95202046632766724
  }],
  "errors": []
}
```

---

## <a name="summary"></a>Özet

Bu makalede, Metin Analizi API'sini kullanarak duyarlılık analizi için kavramları ve iş akışını öğrendiniz. Özet:

+ Duygu Analizi seçilen diller için iki sürümde kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir kimlik, metin ve dil kodu içerir.
+ POST isteği, kişiselleştirilmiş bir erişim anahtarı ve aboneliğiniz için geçerli bir bitiş noktası kullanarak bir `/sentiment` bitiş [noktasıdır.](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)
+ Her belge kimliği için duyarlılık puanından oluşan yanıt çıktısı, JSON'u kabul eden herhangi bir uygulamaya aktarılabilir. Örneğin, Excel ve Power BI.

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizine genel bakış](../overview.md)
* [Text Analytics istemci kitaplığını kullanma](../quickstarts/text-analytics-sdk.md)
* [Yenilikler](../whats-new.md)
