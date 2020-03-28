---
title: Görüntü İşleme API’sini çağırma
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler'de REST API'sini kullanarak BilgisayarLı Vizyon API'sini nasıl arayacağınızı öğrenin.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: sample
ms.date: 09/09/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 298228eedb73298f00654f4f72c201d9ed671090
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72177056"
---
# <a name="call-the-computer-vision-api"></a>Görüntü İşleme API’sini çağırma

Bu makalede, REST API kullanarak Bilgisayar Görme API nasıl çağrılabilir gösterir. Örnekler, Computer Vision API istemci kitaplığı kullanılarak ve HTTP POST veya GET çağrıları kullanılarak C# olarak yazılır. Makale üzerinde duruluyor:

- Etiketleri, açıklamayı ve kategorileri alma
- Etki alanına özgü bilgiler veya "ünlüler" alma

## <a name="prerequisites"></a>Ön koşullar

- Bir resim URL'si veya yerel olarak depolanan bir resme giden yol
- Desteklenen giriş yöntemleri: uygulama/sekizli akış veya görüntü URL'si şeklinde ham görüntü ikilisi
- Desteklenen görüntü dosyası biçimleri: JPEG, PNG, GIF ve BMP
- Resim dosyası boyutu: 4 MB veya daha az
- Görüntü boyutları: &times; 50 50 piksel veya daha büyük
  
Bu makaledeki örnekler aşağıdaki özellikleri göstermektedir:

* Bir dizi etiket ve açıklama döndürmek için görüntüyü çözümleme
* JSON'da karşılık gelen sonucu döndürmek için bir görüntüyü etki alanına özgü bir modelle (özellikle "ünlüler" modeli) analiz etme

Özellikler aşağıdaki seçenekleri sunar:

- **Seçenek 1**: Kapsamlı Çözümleme - Yalnızca belirli bir modeli analiz
- **Seçenek 2**: Gelişmiş Analiz - [86 kategorili taksonomi](../Category-Taxonomy.md) kullanarak ek ayrıntılar sağlamak için analiz
  
## <a name="authorize-the-api-call"></a>API çağrısını yetkilendirme

Görüntü İşleme API’sine yapılan her çağrı için bir abonelik anahtarı gerekir. Bu anahtar, sorgu dize parametresinden geçirilmeli veya istek üstbilgisinde belirtilmelidir.

Ücretsiz deneme anahtarı almak için aşağıdakilerden birini yapın:
* [Bilişsel Hizmetleri Deneyin](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) sayfasına gidin. 
* Computer Vision'a abone olmak için [Bilişsel Hizmetler hesabı oluştur](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) sayfasına gidin.

Aşağıdakilerden birini yaparak abonelik anahtarını geçebilirsiniz:

* Bu Bilgisayar Lı Vizyon API örneğinde olduğu gibi, bir sorgu dizesinde geçirin:

  ```
  https://westus.api.cognitive.microsoft.com/vision/v2.1/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
  ```

* HTTP istek üstbilgisinde belirtin:

  ```
  ocp-apim-subscription-key: <Your subscription key>
  ```

* İstemci kitaplığını kullandığınızda, anahtarı ComputerVisionClient'ın oluşturucusundan geçirin ve istemcinin özelliğinde bölgeyi belirtin:

    ```
    var visionClient = new ComputerVisionClient(new ApiKeyServiceClientCredentials("Your subscriptionKey"))
    {
        Endpoint = "https://westus.api.cognitive.microsoft.com"
    }
    ```

## <a name="upload-an-image-to-the-computer-vision-api-service"></a>Görüntübilgisayar Görme API hizmetine yükleme

Computer Vision API aramasını gerçekleştirmenin temel yolu, etiketleri, açıklamayı ve ünlüleri doğrudan döndürmek için bir resim yüklemektir. Bunu, görüntüden okunan verilerle birlikte HTTP gövdesindeki ikili görüntüyle birlikte bir "POST" isteği göndererek yaparsınız. Yükleme yöntemi tüm Computer Vision API aramaları için aynıdır. Tek fark, belirttiğiniz sorgu parametreleridir. 

Belirli bir resim için, aşağıdaki seçeneklerden birini kullanarak etiketleri ve açıklamayı alın:

### <a name="option-1-get-a-list-of-tags-and-a-description"></a>Seçenek 1: Etiketlerin listesini ve açıklamayı alma

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/analyze?visualFeatures=Description,Tags&subscription-key=<Your subscription key>
```

```csharp
using System.IO;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

ImageAnalysis imageAnalysis;
var features = new VisualFeatureTypes[] { VisualFeatureTypes.Tags, VisualFeatureTypes.Description };

using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  imageAnalysis = await visionClient.AnalyzeImageInStreamAsync(fs, features);
}
```

### <a name="option-2-get-a-list-of-tags-only-or-a-description-only"></a>Seçenek 2: Yalnızca etiketlerin listesini veya yalnızca bir açıklama alma

Yalnızca etiketler için çalıştırın:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/tag?subscription-key=<Your subscription key>
var tagResults = await visionClient.TagImageAsync("http://contoso.com/example.jpg");
```

Yalnızca bir açıklama için çalıştırın:

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/describe?subscription-key=<Your subscription key>
using (var fs = new FileStream(@"C:\Vision\Sample.jpg", FileMode.Open))
{
  imageDescription = await visionClient.DescribeImageInStreamAsync(fs);
}
```

## <a name="get-domain-specific-analysis-celebrities"></a>Etki alanına özel analiz (ünlüler) alın

### <a name="option-1-scoped-analysis---analyze-only-a-specified-model"></a>Seçenek 1: Kapsamlı çözümleme - Yalnızca belirli bir modeli analiz
```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/models/celebrities/analyze
var celebritiesResult = await visionClient.AnalyzeImageInDomainAsync(url, "celebrities");
```

Bu seçenek için diğer tüm sorgu parametreleri {visualFeatures, details} geçerli değildir. Desteklenen tüm modelleri görmek istiyorsanız şu seçeneği kullanın:

```
GET https://westus.api.cognitive.microsoft.com/vision/v2.1/models 
var models = await visionClient.ListModelsAsync();
```

### <a name="option-2-enhanced-analysis---analyze-to-provide-additional-details-by-using-86-categories-taxonomy"></a>Seçenek 2: Gelişmiş analiz - 86 kategorili taksonomi kullanarak ek ayrıntılar sağlamak için analiz

Bir veya daha fazla etki alanına özgü modelden ayrıntılara ek olarak genel bir görüntü analizi almak istediğiniz uygulamalarda, modeller sorgu parametresini kullanarak v1 API'sini genişletin.

```
POST https://westus.api.cognitive.microsoft.com/vision/v2.1/analyze?details=celebrities
```

Bu yöntemi çağırdığınızda, önce [86 kategorili](../Category-Taxonomy.md) sınıflandırıcıyı çağırırsınız. Kategorilerden herhangi biri bilinen veya eşleşen bir modelle eşleşirse, ikinci bir sınıflandırıcı çağrıları oluşur. Örneğin, "ayrıntılar=tümü" veya "ayrıntılar" "ünlüler" içeriyorsa, 86 kategorili sınıflandırıcıyı aradıktan sonra ünlüler modelini ararsınız. Sonuç kategori kişi içerir. Seçenek 1'in aksine, bu yöntem ünlülerle ilgilenen kullanıcılar için gecikme sürelerini artırır.

Bu durumda, tüm v1 sorgu parametreleri aynı şekilde olur. visualFeatures=kategoriler belirtmezseniz, dolaylı olarak etkinleştirilir.

## <a name="retrieve-and-understand-the-json-output-for-analysis"></a>Analiz için JSON çıktısını alın ve anlayın

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

Alan | Tür | İçerik
------|------|------|
Etiketler  | `object` | Bir dizi etiket için üst düzey nesne.
tags[].Name | `string`  | Etiketler sınıflandırıcıdan gelen anahtar kelime.
tags[].Score    | `number`  | Güven puanı, 0 ile 1 arasında.
açıklama  | `object` | Açıklama için üst düzey nesne.
description.tags[] |    `string`    | Etiket listesi.  Resim yazısı oluşturma özelliğine yeterince güven yoksa, etiketler arayan kişi için kullanılabilen tek bilgi olabilir.
description.captions[].text | `string`  | Görüntüyü açıklayan bir ifadedir.
description.captions[].confidence   | `number`  | İfadenin güven puanı.

## <a name="retrieve-and-understand-the-json-output-of-domain-specific-models"></a>Etki alanına özgü modellerin JSON çıktısını alın ve anlayın

### <a name="option-1-scoped-analysis---analyze-only-a-specified-model"></a>Seçenek 1: Kapsamlı çözümleme - Yalnızca belirli bir modeli analiz

Çıktı, aşağıdaki örnekte gösterildiği gibi bir etiket dizisidir:

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

### <a name="option-2-enhanced-analysis---analyze-to-provide-additional-details-by-using-the-86-categories-taxonomy"></a>Seçenek 2: Gelişmiş analiz - "86 kategorili" taksonomi kullanarak ek ayrıntılar sağlamak için analiz

Seçenek 2'yi (gelişmiş çözümleme) kullanan etki alanına özgü modellerde, aşağıdaki örnekte gösterildiği gibi kategoriler inttürü genişletilir:

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

Kategoriler alanı, özgün taksonomideki [86 kategoriden](../Category-Taxonomy.md) birinin veya daha fazlasının listesidir. Alt puanla biten kategoriler bu kategoriyle ve alt oyunlarıyla eşleşir (örneğin, "people_" veya "people_group" ünlüler modeli için).

Alan   | Tür  | İçerik
------|------|------|
kategoriler | `object`   | Üst düzey nesne.
categories[].name    | `string` | 86 kategorili taksonomi listesindeki ad.
categories[].score  | `number`  | Güven puanı, 0 ile 1 arasında.
categories[].detail  | `object?`      | (İsteğe bağlı) Ayrıntı nesnesi.

Birden çok kategori eşleşirse (örneğin, 86 kategorili sınıflandırıcı, model=ünlüler olduğunda hem "people_" hem de "people_young" için bir puan döndürür), ayrıntılar en genel düzey eşleşmesine eklenir ("people_," bu örnekte).

## <a name="error-responses"></a>Hata yanıtları

Bu hatalar, hem Seçenek 1 hem de Seçenek 2 senaryolarında döndürülebilecek ek NotSupportedModel hatasıyla (HTTP 400) vision.analyze'dakilerle aynıdır. Seçenek 2 (gelişmiş çözümleme) için, ayrıntılarda belirtilen modellerden herhangi biri tanınmıyorsa, api bir veya daha fazlası geçerli olsa bile bir NotSupportedModel döndürür. Hangi modellerin desteklenebileceğini öğrenmek için listModels'i arayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

REST API’yi kullanmak için [Görüntü İşleme API’si Başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44) bölümüne gidin.
