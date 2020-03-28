---
title: Text Analytics REST API ile dili algılama
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler'deki Text Analytics REST API'sini kullanarak dili algılayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 07/30/2019
ms.author: aahi
ms.openlocfilehash: d34f3a03e1bcd35c270d13c4dda57d0394a36e4b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70387800"
---
# <a name="example-detect-language-with-text-analytics"></a>Örnek: Metin Analizi ile dili algılama

Azure Metin Analizi REST API'nin [Dil Algılama](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) özelliği, her belge için metin girişini değerlendirir ve dil tanımlayıcılarını analizin gücünü gösteren bir skorla döndürür.

Bu özellik, dilin bilinmediği rastgele metni toplayan içerik depoları için kullanışlıdır. Giriş belgesinde hangi dilin kullanıldığını belirlemek için bu analizin sonuçlarını ayrıştırabilirsiniz. Yanıt, modelin güvenini yansıtan bir puan da döndürür. Puan değeri 0 ile 1 arasındadır.

Dil Algılama özelliği çok çeşitli dilleri, varyantları, lehçeleri ve bazı bölgesel veya kültürel dilleri algılayabilir. Bu özelliğin tam dil listesi yayımlanmamaktadır.

Daha az kullanılan bir dilde ifade edilen içeriğiniz varsa, bir kodu döndürür mü görmek için Dil Algılama özelliğini deneyebilirsiniz. Algılanemeyen dillerin `unknown`yanıtı.

> [!TIP]
> Text Analytics ayrıca dil algılama için Linux tabanlı docker kapsayıcı görüntüsü sağlar, böylece [Text Analytics kapsayıcısını](text-analytics-how-to-install-containers.md) verilerinize yakın olarak yükleyebilir ve çalıştırabilirsiniz.

## <a name="preparation"></a>Hazırlık

Bu formatta JSON belgeleriniz olmalıdır: kimlik ve metin.

Belge boyutu, belge başına 5.120 karakterin altında olmalıdır. Koleksiyon başına en fazla 1.000 öğe (d) öğeniz olabilir. Koleksiyon, istek gövdesinde gönderilir. Aşağıdaki örnek, dil algılama için gönderebilecek içerik örneğidir:

```json
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

## <a name="step-1-structure-the-request"></a>1. Adım: İsteği yapılandırma

İstek tanımı hakkında daha fazla bilgi için [bkz.](text-analytics-how-to-call-api.md) Kolaylık olması için aşağıdaki noktalar yeniden belirtilmektedir:

+ Bir POST isteği oluşturun. Bu isteğe ilişkin API belgelerini gözden geçirmek için [Dil Algılama API'sine](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7)bakın.

+ Dil algılama için HTTP uç noktasını ayarlayın. Azure'da metin analizi kaynağını veya anında metin [analizi kapsayıcısı](text-analytics-how-to-install-containers.md)kullanın. URL'ye `/text/analytics/v2.1/languages` eklemeniz gerekir. Örneğin: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/languages`.

+ Metin Analizi işlemleri için [erişim anahtarını](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) içerecek şekilde bir istek üstbilgisini ayarlayın.

+ İstek gövdesinde, bu analiz için hazırladığınız JSON belgeleri koleksiyonunu sağlayın.

> [!Tip]
> İsteği yapılandırmak ve hizmete GÖNDERMEK için [Postman](text-analytics-how-to-call-api.md) kullanın veya [belgelerdeki](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7)**API testi konsolu**’nu açın.

## <a name="step-2-post-the-request"></a>Adım 2: Post istek

İstek alındığında analiz gerçekleştirilir. Dakika ve saniye gönderebileceğiniz isteklerin boyutu ve sayısı hakkında bilgi için genel bakışta [veri sınırları](../overview.md#data-limits) bölümüne bakın.

Hizmetin durum bilgisi olmadığını unutmayın. Hesabınızda bir veri depolanmaz. Sonuçlar hemen yanıtta döndürülür.


## <a name="step-3-view-the-results"></a>Adım 3: Sonuçları görüntüleme

Tüm POST istekleri, d'ler ve algılanan özelliklerle JSON biçimli bir yanıt döndürer.

Hemen çıktı döndürülür. Sonuçları JSON'u kabul eden bir uygulamaya aktarabilir veya çıktıyı yerel sistemdeki bir dosyaya kaydedebilirsiniz. Ardından, çıktıyı verileri sıralamak, aramak ve işlemek için kullanabileceğiniz bir uygulamaya aktarın.

Örnek istek için sonuçlar, aşağıdaki JSON gibi görünmelidir. Birden çok öğeiçeren bir belge olduğuna dikkat edin. Çıktı İngilizce dilindedir. Dil tanımlayıcıları bir kolay ad ve [ISO 639-1](https://www.iso.org/standard/22109.html) biçiminde dil kodu içerir.

1,0 pozitif puanı, analizin mümkün olan en yüksek güvenilirlik düzeyini ifade eder.

```json
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
        "errors": []
    }
```

### <a name="ambiguous-content"></a>Belirsiz içerik

Bazı durumlarda girişe göre dilleri ayrıştırmak zor olabilir. Parametreyi `countryHint` 2 harfli ülke kodu belirtmek için kullanabilirsiniz. Varsayılan olarak API varsayılan countryHint olarak "ABD" kullanıyor, bu davranışı kaldırmak için bu değeri boş `countryHint = ""` dize için ayarlayarak bu parametreyi sıfırlayabilirsiniz.

Örneğin, "Impossible" hem İngilizce hem de Fransızca için yaygındır ve sınırlı bir bağlamda verilirse yanıt "ABD" ülke ipucuna dayalı olacaktır. Metnin kökeninin Fransa’dan geldiği biliniyorsa bu bir ipucu olarak verilebilir.

**Giriş**

```json
    {
        "documents": [
            {
                "id": "1",
                "text": "impossible"
            },
            {
                "id": "2",
                "text": "impossible",
                "countryHint": "fr"
            }
        ]
    }
```

Hizmet artık daha iyi bir karar vermek için ek bağlam vardır: 

**Çıktı**

```json
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
                        "name": "French",
                        "iso6391Name": "fr",
                        "score": 1
                    }
                ]
            }
        ],
        "errors": []
    }
```

Çözümleyici girişi ayrışdıramazsa, döndürür. `(Unknown)` Bir örnek, yalnızca Arapça sayılardan oluşan bir metin bloğu gönderirseniz.

```json
    {
        "id": "5",
        "detectedLanguages": [
            {
                "name": "(Unknown)",
                "iso6391Name": "(Unknown)",
                "score": "NaN"
            }
        ]
    }
```

### <a name="mixed-language-content"></a>Karışık dil içeriği

Aynı belgedeki karışık dil içeriği, içerikteki en büyük gösterime sahip, ancak daha düşük pozitif derecelendirmeye sahip dili döndürür. Derecelendirme, değerlendirmenin marjinal gücünü yansıtır. Aşağıdaki örnekte giriş, İngilizce, İspanyolca ve Fransızca dillerinin birleşimidir. Çözümleyici, hakim dili belirlemek için her bir kesimdeki karakterleri sayar.

**Giriş**

```json
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

Elde edilen çıktı, 1,0'ın üzerinde bir puana sahip olan baskın dilden oluşur ve bu da güven düzeyinin daha zayıf olduğunu gösterir.

```json
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

Bu makalede, Azure Bilişsel Hizmetler'de Metin Analizi'ni kullanarak dil algılama için kavramları ve iş akışını öğrendiniz. Aşağıdaki noktalar açıklanmış ve gösterilmiştir:

+ [Dil algılama,](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) çok çeşitli diller, varyantlar, lehçeler ve bazı bölgesel veya kültürel diller için kullanılabilir.
+ İstek gövdesindeki JSON belgeleri bir kimlik ve metin içerir.
+ POST isteği, kişiselleştirilmiş bir erişim anahtarı ve aboneliğiniz için geçerli bir bitiş noktası kullanarak bir `/languages` bitiş [noktasıdır.](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)
+ Yanıt çıktısı, her belge kimliği için dil tanımlayıcılarından oluşur. Çıktı JSON kabul eden herhangi bir uygulamaya akış olabilir. Örnek uygulamalar excel ve Power BI, birkaç isim içerir.

## <a name="see-also"></a>Ayrıca bkz.

 [Metin Analizine genel bakış](../overview.md) [Sık sorulan sorular (SSS)](../text-analytics-resource-faq.md)</br>
 [Metin Analizi ürün sayfası](//go.microsoft.com/fwlink/?LinkID=759712)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yaklaşımı analiz etme](text-analytics-how-to-sentiment-analysis.md)
