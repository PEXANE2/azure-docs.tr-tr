---
title: Metin analizi REST API ile dil algılama | Microsoft Docs
description: Azure Bilişsel Hizmetler'in sunduğu metin analizi REST API'sini kullanarak, dili algılayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: e1adeb34cf999f471bb183e4d7de9c65427252bb
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67986520"
---
# <a name="example-detect-language-with-text-analytics"></a>Örnek: Metin analiziyle dili algılayın

[Dil algılama](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) Özelliği Azure metin analizi REST API'si, her belge için metin girişi değerlendirir ve Dil tanımlayıcıları ile analiz gücünü gösteren bir puan döndürür.

Bu özellik, dilin bilinmediği rastgele metni toplayan içerik depoları için kullanışlıdır. Giriş belgesinde hangi dilin kullanıldığını belirlemek için bu analizin sonuçlarını ayrıştırabilirsiniz. Yanıt, ayrıca modelin güvenle yansıtan bir puan döndürür. Puan 0 ile 1 arasında değerdir.

Dil algılama özelliği, çok çeşitli diller, çeşitleri, diyalektler ve bazı bölgesel veya kültürel dil algılayabilir. Bu özellik için dillerin tam listesini yayımlanan değil.

Daha az sık kullanılan bir dille ifade içeriğiniz varsa, bir kod döndürür görmek için dil algılama özelliğini deneyebilirsiniz. Yanıt algılanamayan diller için `unknown`.

> [!TIP]
> Metin analizi de Linux tabanlı bir Docker kapsayıcı görüntüsü dil için algılama sağlar, böylece [yükleyin ve metin analizi kapsayıcı çalıştırın](text-analytics-how-to-install-containers.md) verilerinizi yakın.

## <a name="preparation"></a>Hazırlık

JSON belgelerini şu biçimde olmalıdır: KODU ve metin.

Belge boyutunu, belgeyi başına altında 5.120 karakter olmalıdır. En fazla 1.000 sahip olduğunuz koleksiyon başına öğe sayısı (Kimlikler). Koleksiyon, istek gövdesinde gönderilir. Aşağıdaki örnek, dil algılama için gönderdiğiniz içeriğin örneğidir:

   ```
    {
        "documents": [
            {
                "id": "1",
                "text": "This document is in English."
            },
            {
                "id": "2",
                "text": "Este documento está en inglés."
            },
            {
                "id": "3",
                "text": "Ce document est en anglais."
            },
            {
                "id": "4",
                "text": "本文件为英文"
            },                
            {
                "id": "5",
                "text": "Этот документ на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>1\. adım: Yapı isteği

İstek tanımı hakkında daha fazla bilgi için bkz. [metin analizi API'sini çağırmak](text-analytics-how-to-call-api.md). Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir POST isteği oluşturun. Bu istek için API belgeleri gözden geçirmek için bkz: [dil algılama API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7).

+ Dil algılama için HTTP uç noktasını ayarlayın. Azure veya bir örneklenmiş bir metin analizi kaynağı kullanma [metin analizi kapsayıcı](text-analytics-how-to-install-containers.md). Dahil etmelisiniz `/languages` kaynak: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`.

+ Metin Analizi işlemlerine yönelik erişim anahtarını dahil etmek için bir istek üst bilgisi ayarlayın. Daha fazla bilgi için [bulma uç noktaları ve erişim anahtarları](text-analytics-how-to-access-key.md).

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) **API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>2\. adım: POST isteği

İstek alındığında analiz gerçekleştirilir. Dakika başına gönderin ve ikinci olarak, bkz: istek sayısı ve boyutu hakkında bilgi için [veri sınırları](../overview.md#data-limits) genel bakış bölümünde.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.


## <a name="step-3-view-the-results"></a>3\. adım: Sonuçları görüntüleme

Tüm POST isteklerinden kimlikleri JSON biçimli bir yanıt döndürür ve özellikleri algılandı.

Hemen çıktı döndürülür. JSON kabul eden bir uygulama sonuçları akış veya çıkış yerel sistemin bir dosyaya kaydedin. Ardından, çıkış sıralayın, arayın ve verileri işlemek için kullanabileceğiniz bir uygulamayı içeri aktarın.

Örnek istek için sonuçlar, aşağıdaki JSON gibi görünmelidir. Birden çok öğe içeren bir belge olduğuna dikkat edin. Çıktı İngilizce dilindedir. Dil tanımlayıcıları bir kolay ad ve [ISO 639-1](https://www.iso.org/standard/22109.html) biçiminde dil kodu içerir.

1,0 pozitif puanı, analizin mümkün olan en yüksek güvenilirlik düzeyini ifade eder.



```
{
    "documents": [
        {
            "id": "1",
            "detectedLanguages": [
                {
                    "name": "English",
                    "iso6391Name": "en",
                    "score": 1
                }
            ]
        },
        {
            "id": "2",
            "detectedLanguages": [
                {
                    "name": "Spanish",
                    "iso6391Name": "es",
                    "score": 1
                }
            ]
        },
        {
            "id": "3",
            "detectedLanguages": [
                {
                    "name": "French",
                    "iso6391Name": "fr",
                    "score": 1
                }
            ]
        },
        {
            "id": "4",
            "detectedLanguages": [
                {
                    "name": "Chinese_Simplified",
                    "iso6391Name": "zh_chs",
                    "score": 1
                }
            ]
        },
        {
            "id": "5",
            "detectedLanguages": [
                {
                    "name": "Russian",
                    "iso6391Name": "ru",
                    "score": 1
                }
            ]
        }
    ],
```

### <a name="ambiguous-content"></a>Belirsiz içerik

Çözümleyici giriş varsa ayrıştırılamıyor, döndürür `(Unknown)`. Yalnızca Arapça sayılar içeren bir metin bloğu gönderirseniz bir örnektir.

```
    {
      "id": "5",
      "detectedLanguages": [
        {
          "name": "(Unknown)",
          "iso6391Name": "(Unknown)",
          "score": "NaN"
        }
      ]
```
### <a name="mixed-language-content"></a>Karma dil içeriği

Aynı belgede karma dil içeriği, içeriğin büyük gösteriminde, ancak daha düşük bir pozitif derecelendirme dilini döndürür. Derecelendirmesi, değerlendirmeyi Marjinal gücünü yansıtır. Aşağıdaki örnekte giriş, İngilizce, İspanyolca ve Fransızca dillerinin birleşimidir. Çözümleyici, hakim dili belirlemek için her bir kesimdeki karakterleri sayar.

**Girdi**

```
{
  "documents": [
    {
      "id": "1",
      "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
    }
  ]
}
```

**Çıktı**

Sonuçta elde edilen çıktı daha zayıf bir güven düzeyini gösteren hakim dili, küçüktür 1.0 puanı oluşur.

```
{
  "documents": [
    {
      "id": "1",
      "detectedLanguages": [
        {
          "name": "Spanish",
          "iso6391Name": "es",
          "score": 0.9375
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="summary"></a>Özet

Bu makalede, kavramlar ve iş akışı dil algılama için Azure Bilişsel hizmetler metin analizi kullanarak öğrendiniz. Aşağıdaki noktaları açıklandığı ve gösterildiği:

+ [Dil algılama](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) çok çeşitli diller, çeşitleri, diyalektler ve bölgesel veya kültürel bazı diller için kullanılabilir.
+ İstek gövdesindeki JSON belgelerini bir kimliği ve metin ekleyin.
+ POST isteğiyse bir `/languages` kişiselleştirilmiş kullanarak uç nokta [erişim anahtarı ve uç nokta](text-analytics-how-to-access-key.md) aboneliğiniz için geçerli.
+ Yanıt çıktısı Dil tanımlayıcıları için her bir belge kimliğinin oluşur. Çıkış JSON kabul eden herhangi bir uygulama için aktarılabilir. Örnek uygulamaları, Excel ve Power BI, dizileri içerir.

## <a name="see-also"></a>Ayrıca bkz. 

 [Metin Analizine genel bakış](../overview.md)  
 [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yaklaşımı analiz etme](text-analytics-how-to-sentiment-analysis.md)
