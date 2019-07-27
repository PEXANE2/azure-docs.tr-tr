---
title: "Örnek: Yansımayı çözümle API 'sini çağırma-Görüntü İşleme"
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler’de REST’i kullanarak Görüntü İşleme API’sinin nasıl çağrılacağını öğreneceksiniz.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 03/21/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 35e6ad922ab54748165fcf8e273d93ee44bc42cc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68564523"
---
# <a name="example-how-to-call-the-computer-vision-api"></a>Örnek: Görüntü İşleme API'si nasıl çağrılacağını

Bu kılavuzda, REST kullanılarak Görüntü İşleme API’sinin nasıl çağrılacağı gösterilmektedir. Örnekler hem Görüntü İşleme API’si istemci kitaplığı kullanılarak C# dilinde hem de HTTP POST/GET çağrıları olarak yazılır. Odaklanacaklarımız:

- "Etiketler", "Açıklama" ve "Kategoriler" alma.
- "Etki alanına özgü" bilgileri alma (ünlüler)

## <a name="prerequisites"></a>Önkoşullar

- Yerel olarak depolanan görüntünün yolu veya görüntü URL’si.
- Desteklenen giriş yöntemleri: Bir uygulama/sekizli akış veya görüntü URL 'SI biçiminde Ham görüntü ikilisi
- Desteklenen görüntü biçimleri: JPEG, PNG, GIF, BMP
- Görüntü dosyası boyutu: 4 MB 'tan az
- Görüntü boyutu: 50 x 50 pikselden büyük
  
Aşağıdaki örneklerde, aşağıdaki özellikler gösterilmektedir:

1. Bir görüntüyü analiz etme ve döndürülen bir açıklamayı ve etiket dizisini alma.
2. Etki alanına özgü model (özellikle, “ünlüler” modeli) ile bir görüntüyü analiz etme ve JSON döndürmesinde ilgili sonucu alma.

Özellikler şunlara ayrılır:

- **Seçenek bir:** Kapsamlı analiz-yalnızca belirli bir modeli çözümle
- **Iki seçenek:** Gelişmiş analiz- [86 Kategoriler sınıflandırmasında](../Category-Taxonomy.md) ek ayrıntılar sağlamak için çözümleyin
  
## <a name="authorize-the-api-call"></a>API çağrısını yetkilendir

Görüntü İşleme API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtarın bir sorgu dizesi parametresi aracılığıyla geçirilmesi veya istek üst bilgisinde belirtilmesi gerekir.

Ücretsiz deneme anahtarı almak için bkz. bilişsel [Hizmetler 'ı deneme](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Ya da Görüntü İşleme abone olmak ve anahtarınızı almak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin.

1. Sorgu dizesi aracılığıyla abonelik anahtarını geçirme, Görüntü İşleme API’si örneği olarak aşağıdaki örneğe bakın:

```https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>```

1. Abonelik anahtarını geçirme, HTTP isteği üst bilgisinde de belirtilebilir:

```ocp-apim-subscription-key: <Your subscription key>```

1. İstemci kitaplığı kullanılırken, VisionServiceClient sınıfının oluşturucusu aracılığıyla abonelik anahtarı geçirilir:

```var visionClient = new VisionServiceClient("Your subscriptionKey");```

## <a name="upload-an-image-to-the-computer-vision-api-service-and-get-back-tags-descriptions-and-celebrities"></a>Görüntü İşleme API'si hizmetine bir görüntü yükleyin ve geri etiket, açıklama ve ünlüler alın

Görüntü İşleme API’si çağrısını gerçekleştirmenin temel yolu, bir görüntünün doğrudan karşıya yüklenmesiyle gerçekleşir. Görüntüden okunan verilerle uygulama/sekizli akış içerik türü ile bir "POST" isteği gönderilerek bu yapılır. "Etiketler" ve "Açıklama" için bu karşıya yükleme yöntemi, tüm Görüntü İşleme API’si çağrıları için aynı olacaktır. Tek fark, kullanıcının belirttiği sorgu parametreleridir. 

Belirli bir görüntü için "Etiketler" ve "Açıklama" alma işlemi şöyledir:

**Seçenek bir:** "Etiketler" ve bir "Açıklama" listesi alın

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```

```csharp
using Microsoft.ProjectOxford.Vision;
using Microsoft.ProjectOxford.Vision.Contract;
using System.IO;

AnalysisResult analysisResult;
var features = new VisualFeature[] { VisualFeature.Tags, VisualFeature.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.AnalyzeImageAsync(fs, features);
}
```

**İkinci Seçenek** Yalnızca "Etiketler" listesini veya yalnızca "Açıklama" listesini alma:

###### <a name="tags-only"></a>Yalnızca Etiketler:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/tag&subscription-key=<Your subscription key>
var analysisResult = await visionClient.GetTagsAsync("http://contoso.com/example.jpg");
```

###### <a name="description-only"></a>Yalnızca Açıklama:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/describe&subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  analysisResult = await visionClient.DescribeAsync(fs);
}
```

### <a name="get-domain-specific-analysis-celebrities"></a>Etki alanına özgü analiz (ünlüler) Al

**Seçenek bir:** Kapsamlı analiz-yalnızca belirli bir modeli çözümle
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```

Bu seçenek için diğer tüm sorgu parametreleri {visualFeatures, details} geçerli değildir. Desteklenen tüm modelleri görmek istiyorsanız şu seçeneği kullanın:

```
GET https://westus.api.cognitive.microsoft.com/vision/v2.0/models 
var models = await visionClient.ListModelsAsync();
```

**Iki seçenek:** Gelişmiş analiz- [86 Kategoriler sınıflandırmasında](../Category-Taxonomy.md) ek ayrıntılar sağlamak için çözümleyin

Bir veya daha fazla etki alanına özgü modelde yer alan ayrıntılara ek olarak genel görüntü analizi almak istediğiniz uygulamalar için, modeller sorgu parametresi ile v1 API’sini genişletiriz.

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.0/analyze?details=celebrities
```

Bu yöntem çağrıldığında önce 86 kategorisi sınıflandırıcısını çağırırız. Herhangi bir kategori, bilinen/eşleşen bir modelin kategorisiyle eşleşiyorsa, ikinci bir sınıflandırıcı çağrısı geçirme işlemi gerçekleşir. Örneğin, "details=all" veya "details" olursa ‘celebrities’ öğesini ekleyin; böylece 86 kategorisi sınıflandırıcısı çağrıldıktan sonra ünlüler modelini çağırırız ve sonuç, kişi kategorisini içerir. Bu, ünlülerle ilgilenen kullanıcılar için Birinci Seçeneğe kıyasla gecikme süresini artırır.

Tüm v1 sorgu parametreleri, bu durumda aynı şekilde davranır.  visualFeatures=categories belirtilmezse, örtük olarak etkinleştirilir.

## <a name="retrieve-and-understand-the-json-output-for-analysis"></a>Analiz için JSON çıkışını alma ve anlama

Bir örneği aşağıda verilmiştir:

```json
{  
  "tags":[  
    {  
      "name":"outdoor",
      "score":0.976
    },
    {  
      "name":"bird",
      "score":0.95
    }
  ],
  "description":{  
    "tags":[  
      "outdoor",
      "bird"
    ],
    "captions":[  
      {  
        "text":"partridge in a pear tree",
        "confidence":0.96
      }
    ]
  }
}
```

Alan | Type | İçerik
------|------|------|
Tags  | `object` | Etiket dizisi için üst düzey nesnedir
tags[].Name | `string`  | Etiketler sınıflandırıcısındaki anahtar sözcüktür
tags[].Score    | `number`  | 0 ile 1 arasında güven puanıdır
description  | `object` | Açıklama için üst düzey nesnedir.
description.tags[] |    `string`    | Etiketlerin listesidir.  Açıklamalı alt yazı üretme özelliği yeterince güvenilir değilse, çağıranın kullanımına sunulan tek bilgi etiketler olabilir.
description.captions[].text | `string`  | Görüntüyü açıklayan bir ifadedir.
description.captions[].confidence   | `number`  | İfade için güven düzeyidir.

## <a name="retrieve-and-understand-the-json-output-of-domain-specific-models"></a>Etki alanına özgü modellerin JSON çıkışını alma ve anlama

**Seçenek bir:** Kapsamlı analiz-yalnızca belirli bir modeli çözümle

Çıkış bir etiket dizisi olacaktır; örnek şuna benzer:

```json
{  
  "result":[  
    {  
      "name":"golden retriever",
      "score":0.98
    },
    {  
      "name":"Labrador retriever",
      "score":0.78
    }
  ]
}
```

**Iki seçenek:** Gelişmiş analiz-86 Kategoriler sınıflandırmasında ek ayrıntılar sağlamak için çözümleyin

İkinci Seçeneği (Gelişmiş Analiz) kullanan etki alanına özgü modeller için kategoriler döndürme türü genişletilmiştir. Aşağıda bir örnek verilmiştir:

```json
{  
  "requestId":"87e44580-925a-49c8-b661-d1c54d1b83b5",
  "metadata":{  
    "width":640,
    "height":430,
    "format":"Jpeg"
  },
  "result":{  
    "celebrities":[  
      {  
        "name":"Richard Nixon",
        "faceRectangle":{  
          "left":107,
          "top":98,
          "width":165,
          "height":165
        },
        "confidence":0.9999827
      }
    ]
  }
}
```

Kategoriler alanı, özgün taksonomideki [86 kategorisinden](../Category-Taxonomy.md) birinin veya daha fazlasının listesidir. Alt çizgiyle biten kategorilerin, bu kategori ve alt öğeleri ile eşleşeceğini de unutmayın (örneğin, ünlüler modeli için people_ ve people_group).

Alan   | Type  | İçerik
------|------|------|
categories | `object`   | Üst düzey nesne
categories[].name    | `string` | 86 kategorisi sınıflandırmasındaki ad
categories[].score  | `number`  | 0 ile 1 arasında güven puanı
categories[].detail  | `object?`      | İsteğe bağlı ayrıntı nesnesi

Birden fazla kategori eşleşiyorsa (örneğin, model=ünlüler olduğunda 86 kategorisi sınıflandırıcısı, hem people_ hem de people_young için bir puan döndürür), ayrıntılar en genel düzey eşleşmesine (bu örnekte people_) eklenir.

## <a name="errors-responses"></a>Hata yanıtları

Bunlar, hem Option One hem de Option Two senaryosunda döndürülebilecek ek NotSupportedModel hatası (HTTP 400) ile vision.analyze öğesine benzer. İkinci Seçenek (Gelişmiş Analiz) için, ayrıntılarda belirtilen modellerden herhangi biri tanınmıyorsa, bunlardan biri veya daha fazlası geçerli olsa da API bir NotSupportedModel döndürür.  Kullanıcılar, hangi modellerin desteklendiğini öğrenmek için listModels öğesini çağırabilir.

## <a name="next-steps"></a>Sonraki adımlar

REST API’yi kullanmak için [Görüntü İşleme API’si Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44) bölümüne gidin.
