---
title: Azure Bilişsel Hizmetler'in sunduğu metin analizi REST API'yi kullanarak duygu analizi | Microsoft Docs
description: Metin analizi REST API'sini kullanarak duyarlılığı Algıla öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: c3004dd3910dd5fdafc933efa213c9f097310e87
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68001701"
---
# <a name="example-detect-sentiment-with-text-analytics"></a>Örnek: Metin analiziyle duyguları algılayın

[Azure yaklaşım analizi API'sini](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) metin girişi değerlendirir ve her belge için bir yaklaşım puanı döndürür. 1 (pozitif) puanları aralık, 0 (negatif).

Bu yetenek; sosyal medya, müşteri incelemeleri ve tartışma forumlarında pozitif ve negatif yaklaşımları algılamak için kullanışlıdır. İçerik, sizin tarafınızdan sağlanır. Modelleriniz ve eğitim verileriniz hizmeti tarafından sağlanır.

Şu anda İngilizce, Almanca, İspanyolca ve Fransızca yaklaşım analizi API'sini destekler. Diğer diller önizleme aşamasındadır. Daha fazla bilgi için bkz. [Desteklenen diller](../text-analytics-supported-languages.md).

> [!TIP]
> Yapabilecekleriniz Azure metin analizi API'si de Linux tabanlı bir Docker kapsayıcı görüntüsü yaklaşım analizi için sağlar. böylece [yükleyin ve metin analizi kapsayıcı çalıştırın](text-analytics-how-to-install-containers.md) verilerinizi yakın.

## <a name="concepts"></a>Kavramlar

Metin Analizi, 0 ile 1 arasında bir yaklaşım puanı oluşturmak için makine öğrenmesi sınıflandırma algoritması kullanır. 1’e yakın puanlar pozitif yaklaşımı, 0’a yakın puanlar ise negatif yaklaşımı gösterir. Model, yaklaşım ilişkilendirmeleri ile kapsamlı bir metin gövdesi kullanılarak önceden eğitilir. Şu anda, kendi eğitim verilerini sağlamak mümkün değildir. Metin analizi sırasında teknikler birleşimini modeli kullanır. Teknikleri, metin işleme, konuşma bölümü analizi, sözcük yerleştirme ve sözcük ilişkileri içerir. Algoritma hakkında daha fazla bilgi için bkz. [Metin Analizi Tanıtımı](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

Metindeki belirli bir varlık için yaklaşımı ayıklamanın tersine yaklaşım analizi, belgenin tamamında gerçekleştirilir. Uygulamada, belgeler, büyük bir metin bloğu yerine bir veya iki cümle içerdiğinde artırmak için Puanlama doğruluğu için eğilimi yoktur. Nesnellik değerlendirmesi aşamasında model, belgenin tamamının nesnel mi olduğunu yoksa yaklaşım mı içerdiğini belirler. Çoğunlukla olan bir belgeyi hedefi ayrıntılı işlem ile 0,50 puan sonuçlanır duygu algılama aşaması için ilerleme durumu değil. İşlem hattında devam belgeler için sonraki aşamaya bir puan 0,50 altına veya üstüne oluşturur. Puan belgede algılanan yaklaşımı derecesini bağlıdır.

## <a name="preparation"></a>Hazırlık

Metin üzerinde çalışmak için daha küçük öbeklere verdiğinizde yaklaşım analizi daha yüksek kaliteli bir sonuç üretir. Bu, büyük metin öbekleri üzerinde daha iyi performans gösteren anahtar ifade ayıklamasının tersidir. Her iki işlemden de en iyi sonuçları elde etmek için girişleri uygun şekilde yeniden yapılandırın.

JSON belgelerini şu biçimde olmalıdır: Kimliği, metin ve dili.

Belge boyutuna, belge başına altında 5.120 karakter olmalıdır. En fazla 1.000 sahip olduğunuz koleksiyon başına öğe sayısı (Kimlikler). Koleksiyon, istek gövdesinde gönderilir. Aşağıdaki örnek, yaklaşım analizi için gönderdiğiniz içeriğin örneğidir:

```
    {
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },                
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>1\. adım: Yapı isteği

İstek tanımı hakkında daha fazla bilgi için bkz. [metin analizi API'sini çağırmak](text-analytics-how-to-call-api.md). Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir POST isteği oluşturun. Bu istek için API belgeleri gözden geçirmek için bkz: [yaklaşım analizi API'sini](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9).

+ Yaklaşım analizi için HTTP uç noktası kullanarak metin analizi kaynak Azure veya örneklenmiş bir ayarlamak [metin analizi kapsayıcı](text-analytics-how-to-install-containers.md). Dahil etmelisiniz `/sentiment` kaynak: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`.

+ Metin Analizi işlemlerine yönelik erişim anahtarını dahil etmek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için [bulma uç noktaları ve erişim anahtarları](text-analytics-how-to-access-key.md).

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

> [!Tip]
> Kullanım [Postman](text-analytics-how-to-call-api.md) veya **API sınama Konsolu** içinde [belgeleri](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) istek yapısı ve hizmete gönderin.

## <a name="step-2-post-the-request"></a>2\. adım: POST isteği

İstek alındığında analiz gerçekleştirilir. Dakika başına gönderin ve ikinci olarak, bkz: istek sayısı ve boyutu hakkında bilgi için [veri sınırları](../overview.md#data-limits) genel bakış bölümünde.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.


## <a name="step-3-view-the-results"></a>3\. adım: Sonuçları görüntüleme

Yaklaşım Çözümleyicisi pozitif veya negatif metin ağırlıklı olarak sınıflandırır. 0 ile 1 aralığında bir puan atar. 0,5’e yakın değerler nötr veya belirsizdir. 0,5 puanı, nötr olma durumunu belirtir. Bir dize yaklaşım için analiz edilemez veya hiçbir yaklaşım vardır, puan her zaman olduğu 0,5 tam olarak. Örneğin, İngilizce dil koduyla İspanyolca bir dize geçirirseniz puan 0,5 olur.

Hemen çıktı döndürülür. JSON kabul eden bir uygulama sonuçları akış veya çıkış yerel sistemin bir dosyaya kaydedin. Ardından, çıkış sıralayın, arayın ve verileri işlemek için kullanabileceğiniz bir uygulamayı içeri aktarın.

Aşağıdaki örnek, bu makaledeki belge koleksiyonu yanıtı gösterir:

```json
{
    "documents": [
        {
            "score": 0.9999237060546875,
            "id": "1"
        },
        {
            "score": 0.0000540316104888916,
            "id": "2"
        },
        {
            "score": 0.99990355968475342,
            "id": "3"
        },
        {
            "score": 0.980544924736023,
            "id": "4"
        },
        {
            "score": 0.99996328353881836,
            "id": "5"
        }
    ],
    "errors": []
}
```

## <a name="sentiment-analysis-v3-public-preview"></a>Yaklaşım analizi v3 genel önizlemeye sunuldu

[Yaklaşım analizi'nın sonraki sürümü](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-preview/operations/56f30ceeeda5650db055a3c9) genel Önizleme için kullanıma sunulmuştur. Doğruluk ve ayrıntılarını API'nin metin kategorilere ayırma ve puanlama önemli geliştirmeler sağlar. 

> [!NOTE]
> * Yaklaşım analizi v3 istek biçimi ve [veri sınırları](../overview.md#data-limits) önceki sürümü ile aynıdır.
> * Şu anda, yaklaşım analizi v3: 
>    * Şu anda yalnızca İngilizce dilini desteklemektedir.  
>    * Aşağıdaki bölgelerde kullanılabilir: `Central US`, `Central Canada`, ve `East Asia`.

|Özellik |Açıklama  |
|---------|---------|
|Geliştirilmiş doğruluğuna     | Metin belgelerinde olumlu, nötr, olumsuz ve karışık yaklaşımların algılanması konusunda önceki sürümlere göre ciddi bir iyileştirme sağlandı.           |
|Belge ve cümle düzeyinde yaklaşım puanı     | Hem belgenin hem belgedeki tek tek cümlelerin yaklaşımını algılayabilirsiniz. Belgede birden fazla cümle varsa her cümleye de ayrı bir yaklaşım puanı verilir.         |
|Yaklaşım kategorisi ve puanı     | API, artık bir yaklaşım puanına ek olarak bir metin için yaklaşım kategorileri döndürür. Kategoriler `positive`, `negative`, `neutral`, ve `mixed`.       |
| Gelişmiş çıktı | Yaklaşım analizi, artık tüm metin belgesi hem kendi cümleleri tek tek bilgi döndürür. |

### <a name="sentiment-labeling"></a>Yaklaşım etiketleme

Yaklaşım analizi v3 puanlarını ve etiketleri bir cümle ve belge düzeyinde döndürebilir. Puanları ve etiketlerin `positive`, `negative`, ve `neutral`. Belge düzeyinde `mixed` yaklaşım etiketinin (puan değil) da döndürülebilir. Cümleleri puanları toplayarak belgenin yaklaşımı belirlenir.

| Tümce yaklaşım                                                        | Belge etiketi döndürdü |
|---------------------------------------------------------------------------|----------------|
| En az bir pozitif cümle ve cümleleri geri kalanı bağımsız. | `positive`     |
| En az bir negatif cümle ve cümleleri geri kalanı bağımsız.  | `negative`     |
| En az bir negatif cümle ve en az bir pozitif cümle.         | `mixed`        |
| Tüm cümleleri belirsiz.                                                 | `neutral`      |

### <a name="sentiment-analysis-v3-example-request"></a>Yaklaşım analizi v3 örnek istek

Aşağıdaki JSON, yaklaşım analizi yeni sürümüne yapılan bir istek örneğidir. Biçimlendirme isteği önceki sürümü aynıdır:

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
  ]
}
```

### <a name="sentiment-analysis-v3-example-response"></a>Yaklaşım analizi v3 örnek yanıt

İstek biçimini önceki sürümüyle aynı olsa da, yanıt biçimi değişmiştir. Yeni API sürümüne ait bir örnek yanıt aşağıdaki JSON şöyledir:

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

### <a name="example-c-code"></a>Örnek C# kod

Bir örnek bulabilirsiniz C# üzerinde yaklaşım analizi bu sürümü çağıran uygulama [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/SentimentV3.cs).

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve iş akışı yaklaşım analizi için Azure Bilişsel hizmetler metin analizi kullanarak öğrendiniz. Özet:

+ [Yaklaşım analizi API'sini](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) seçili diller için kullanılabilir.
+ İstek gövdesindeki JSON belgelerini bir kimliği, metin ve dil kodu içerir.
+ POST isteğiyse bir `/sentiment` kişiselleştirilmiş kullanarak uç nokta [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) aboneliğiniz için geçerli.
+ Her belge kimliği için bir yaklaşım puanı oluşan yanıt çıkış JSON kabul eden herhangi bir uygulama için aktarılabilir. Örnek uygulamaları, Excel ve Power BI, dizileri içerir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Anahtar ifadeleri ayıklama](text-analytics-how-to-keyword-extraction.md)
