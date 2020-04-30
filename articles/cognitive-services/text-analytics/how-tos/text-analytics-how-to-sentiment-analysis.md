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
ms.date: 04/27/2020
ms.author: aahi
ms.openlocfilehash: 99a62daf6dced88efd9bda591a0ca44a8b259a75
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82195647"
---
# <a name="how-to-detect-sentiment-using-the-text-analytics-api"></a>Nasıl yapılır: Metin Analizi API'si kullanarak yaklaşımı algılama

Metin Analizi API'si Yaklaşım Analizi özelliği metni değerlendirir ve her tümce için yaklaşım puanlarını ve etiketlerini döndürür. Bu, sosyal medya, müşteri incelemeleri, tartışma forumları ve daha fazlası için olumlu ve olumsuz yaklaşımı saptamak için yararlıdır. API tarafından kullanılan AI modelleri hizmet tarafından sağlanır, yalnızca analiz için içerik göndermeniz gerekir.

> [!TIP]
> Metin Analizi Ayrıca, dil algılama için Linux tabanlı bir Docker kapsayıcı görüntüsü sağlar, bu sayede Metin Analizi kapsayıcısını verilerinize yakın şekilde [yükleyip çalıştırabilirsiniz](text-analytics-how-to-install-containers.md) .

Yaklaşım Analizi, önizleme aşamasında çok çeşitli dilleri destekler. Daha fazla bilgi için bkz. [Desteklenen diller](../text-analytics-supported-languages.md).

## <a name="concepts"></a>Kavramlar

Metin Analizi API'si, 0 ile 1 arasında bir yaklaşım puanı oluşturmak için bir makine öğrenimi sınıflandırma algoritması kullanır. 1’e yakın puanlar pozitif yaklaşımı, 0’a yakın puanlar ise negatif yaklaşımı gösterir. Yaklaşım analizi, metinde tek tek varlıklar yerine tüm belgenin tamamında gerçekleştirilir. Bu, yaklaşım puanlarının bir belge veya tümce düzeyinde döndürüldüğü anlamına gelir. 

Kullanılan model, çok sayıda metin ve yaklaşım ilişkilendirmeleriyle önceden eğitilmiş. Metin işleme, konuşma bölümü analizi, sözcük yerleşimi ve Word ilişkilendirmeleri gibi analize yönelik tekniklerin bir birleşimini kullanır. Algoritma hakkında daha fazla bilgi için bkz. [Metin Analizi Tanıtımı](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/). Şu anda kendi eğitim verilerinizi sağlamak mümkün değildir. 

Belgeler, büyük bir metin bloğu yerine daha az sayıda tümce içerdiğinde iyileştirebilecek Puanlama doğruluğu için bir kullanım eğilimi vardır. Nesnellik değerlendirmesi aşamasında model, belgenin tamamının nesnel mi olduğunu yoksa yaklaşım mı içerdiğini belirler. Genellikle hedefi olan bir belge, daha fazla işlem olmadan 0,50 puanı ile sonuçlanan yaklaşım algılama aşamasına ileretmez. Ardışık düzende devam eden belgeler için, sonraki aşama 0,50 veya üzeri bir puan üretir. Puan, belgede algılanan yaklaşım derecesine bağlıdır.

## <a name="sentiment-analysis-versions-and-features"></a>Sürümler ve Özellikler Yaklaşım Analizi

Metin Analizi API'si, Yaklaşım Analizi-v2 ve v3 'nin iki sürümünü sunmaktadır. Yaklaşım Analizi v3 (Genel Önizleme), API 'nin metin kategorisi ve Puanlama hakkında doğruluk ve ayrıntı açısından önemli geliştirmeler sağlar.

> [!NOTE]
> * Yaklaşım Analizi v3 istek biçimi ve [veri sınırları](../overview.md#data-limits) , önceki sürümle aynıdır.
> * Yaklaşım Analizi `Australia East`v3 şu bölgelerde kullanılabilir:, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe`ve. `West US 2`

| Özellik                                   | Yaklaşım Analizi v2 | Yaklaşım Analizi v3 |
|-------------------------------------------|-----------------------|-----------------------|
| Tek ve toplu istekler için Yöntemler    | X                     | X                     |
| Tüm belge için yaklaşım puanları  | X                     | X                     |
| Ayrı cümleler için yaklaşım puanları |                       | X                     |
| Yaklaşım etiketleme                        |                       | X                     |
| Model sürümü oluşturma                   |                       | X                     |

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

### <a name="sentiment-scoring"></a>Yaklaşım Puanlama

Yaklaşım Analizi v3, metni yaklaşım etiketleriyle sınıflandırır (aşağıda açıklanmıştır). Döndürülen puanlar, metnin pozitif, negatif veya nötr olduğu, modelin güvenini temsil eder. Daha yüksek değerler daha yüksek güvenilirliğe işaret eder. 

### <a name="sentiment-labeling"></a>Yaklaşım etiketleme

Yaklaşım Analizi v3, bir tümce ve belge düzeyinde (`positive`, `negative`, ve `neutral`) güven puanlarıyla birlikte yaklaşım etiketleri getirir. `mixed` Yaklaşım etiketi belge düzeyinde de döndürülebilir. 

Belgenin yaklaşımı aşağıda belirlenir:

| Tümce yaklaşımı                                                                            | Döndürülen belge etiketi |
|-----------------------------------------------------------------------------------------------|-------------------------|
| Belgede en az `positive` bir cümle vardır. Tümcelerin geri kalanı şunlardır `neutral`. | `positive`              |
| Belgede en az `negative` bir cümle vardır. Tümcelerin geri kalanı şunlardır `neutral`. | `negative`              |
| Belgede en az `negative` bir cümle ve en az `positive` bir cümle vardır.    | `mixed`                 |
| Belgedeki tüm tümceler `neutral`.                                                  | `neutral`               |

### <a name="model-versioning"></a>Model sürümü oluşturma

> [!NOTE]
> Yaklaşım analizi için model sürümü oluşturma, sürümden `v3.0-preview.1`itibaren kullanılabilir.

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

### <a name="example-c-code"></a>Örnek C# kodu

[GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/TextAnalyticsSentiment.cs)üzerinde yaklaşım analizi bu sürümünü çağıran örnek bir C# uygulaması bulabilirsiniz.


#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

### <a name="sentiment-scoring"></a>Yaklaşım Puanlama

Yaklaşım Çözümleyicisi, metni ağırlıklı pozitif veya negatif olarak sınıflandırır. 0 ile 1 arasında bir puan atar. 0,5’e yakın değerler nötr veya belirsizdir. 0,5 puanı, nötr olma durumunu belirtir. Bir dize yaklaşım için çözümlenememesi veya yaklaşım yoksa, puan her zaman 0,5 ' dir. Örneğin, İngilizce dil koduyla İspanyolca bir dize geçirirseniz puan 0,5 olur.

---

## <a name="sending-a-rest-api-request"></a>REST API isteği gönderiliyor 

### <a name="preparation"></a>Hazırlık

Yaklaşım analizi, üzerinde çalışmak üzere daha az miktarda metin verdiğiniz zaman daha yüksek kalitede bir sonuç üretir. Bu, büyük metin öbekleri üzerinde daha iyi performans gösteren anahtar ifade ayıklamasının tersidir. Her iki işlemden de en iyi sonuçları elde etmek için girişleri uygun şekilde yeniden yapılandırın.

Bu biçimde JSON belgelerinize sahip olmanız gerekir: KIMLIK, metin ve dil.

Belge boyutunun belge başına 5.120 karakter altında olması gerekir. Koleksiyon başına en fazla 1.000 öğe (kimlik) kullanabilirsiniz. Koleksiyon, istek gövdesinde gönderilir.

## <a name="structure-the-request"></a>İsteği yapısı

Bir POST isteği oluşturun. Hızlı bir şekilde yapısına ve gönderebilmeniz için aşağıdaki başvuru bağlantılarında [Postman](text-analytics-how-to-call-api.md) veya **API test konsolunu** kullanabilirsiniz. 

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

[Yaklaşım Analizi v3 başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment)

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

[Yaklaşım Analizi v2 başvurusu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)

---

Azure 'da bir Metin Analizi kaynağı veya bir örneklenmiş [metin analizi kapsayıcısı](text-analytics-how-to-install-containers.md)kullanarak yaklaşım ANALIZI için HTTPS uç noktasını ayarlayın. Kullanmak istediğiniz sürüm için doğru URL 'YI dahil etmeniz gerekir. Örneğin:

> [!NOTE]
> Azure portalında Metin Analizi kaynağınız için anahtarınızı ve uç noktanızı bulabilirsiniz. Kaynak **yönetimi**altında kaynağın **hızlı başlangıç** sayfasında yer alır. 

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/sentiment`

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/sentiment`

---

Metin Analizi API'si anahtarınızı dahil etmek için bir istek üst bilgisi ayarlayın. İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

### <a name="example-sentiment-analysis-request"></a>Örnek Yaklaşım Analizi isteği 

Aşağıda, yaklaşım analizi için gönderebileceğiniz bir içerik örneği verilmiştir. İstek biçimi, her iki API sürümü için de aynıdır.
    
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

### <a name="post-the-request"></a>İsteği gönder

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye başına gönderebilmeniz için istek boyutu ve sayısı hakkında bilgi için genel bakış konusunun [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Metin Analizi API'si durum bilgisiz. Hesabınızda hiç veri depolanmaz ve sonuçlar yanıt içinde hemen döndürülür.


### <a name="view-the-results"></a>Sonuçları görüntüleme

Yaklaşım Çözümleyicisi, metni ağırlıklı pozitif veya negatif olarak sınıflandırır. 0 ile 1 arasında bir puan atar. 0,5’e yakın değerler nötr veya belirsizdir. 0,5 puanı, nötr olma durumunu belirtir. Bir dize yaklaşım için çözümlenememesi veya yaklaşım yoksa, puan her zaman 0,5 ' dir. Örneğin, İngilizce dil koduyla İspanyolca bir dize geçirirseniz puan 0,5 olur.

Hemen çıktı döndürülür. Sonuçları JSON kabul eden bir uygulamaya veya çıktıyı yerel sistemdeki bir dosyaya kaydedebilirsiniz. Sonra çıktıyı, verileri sıralamak, aramak ve işlemek için kullanabileceğiniz bir uygulamaya içeri aktarın. Çok dilli ve Emoji desteği nedeniyle, yanıt metin uzaklıkları içerebilir. Daha fazla bilgi için bkz. [uzaklıkları işleme](../concepts/text-offsets.md) .

#### <a name="version-30-preview"></a>[Sürüm 3,0-Önizleme](#tab/version-3)

### <a name="sentiment-analysis-v3-example-response"></a>Yaklaşım Analizi v3 örnek yanıtı

Yaklaşım Analizi v3 'ten gelen yanıtlar, çözümlenen her tümce ve belge için yaklaşım etiketleri ve puanları içerir. `documentScores`Belge yaklaşım etiketi ise `mixed`döndürülmez.

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

#### <a name="version-21"></a>[Sürüm 2,1](#tab/version-2)

### <a name="sentiment-analysis-v2-example-response"></a>Yaklaşım Analizi v2 örnek yanıtı

Yaklaşım Analizi v2 'nin yanıtları gönderilen her belge için yaklaşım puanlarını içerir.

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

Bu makalede, Metin Analizi API'si kullanarak yaklaşım analizi için kavramları ve iş akışını öğrendiniz. Özet:

+ Yaklaşım Analizi, iki sürümde seçili diller için kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir KIMLIK, metin ve dil kodu içerir.
+ POST isteği, kişiselleştirilmiş bir `/sentiment` [erişim anahtarı ve](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) aboneliğiniz için geçerli olan bir uç nokta kullanarak bir uç noktaya gönderilir.
+ Her belge KIMLIĞI için bir yaklaşım puanından oluşan yanıt çıkışı, JSON kabul eden herhangi bir uygulamaya akışla eklenebilir. Örneğin, Excel ve Power BI.

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizine genel bakış](../overview.md)
* [Metin Analizi istemci kitaplığını kullanma](../quickstarts/text-analytics-sdk.md)
* [Yenilikler](../whats-new.md)
