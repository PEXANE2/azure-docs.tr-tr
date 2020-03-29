---
title: Metin Moderasyon API'sini kullanarak metni orta düzeye göre - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Çevrimiçi konsoldaki Metin Moderasyonu API'sini kullanarak metin moderasyonunu test edin.
services: cognitive-services
author: PatrickFarley
ms.author: pafarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: e0930558f31b27a77fa2cd6b44fcea2fe9091086
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74538819"
---
# <a name="moderate-text-from-the-api-console"></a>API konsolundan orta metin

Metin içeriğinizi küfür için tarayıp özel ve paylaşılan listelerle karşılaştırmak için Azure İçerik Moderatörü'ndeki [Metin Moderasyon API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) kullanın.

## <a name="get-your-api-key"></a>API anahtarınızı alın

API'yi çevrimiçi konsolda test etmeden önce abonelik anahtarınız gerekir. Bu, **Ayarlar** sekmesinde, **Ocp-Apim-Abonelik-Anahtar** kutusunda bulunur. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="navigate-to-the-api-reference"></a>API başvurusuna gidin

[Metin Moderasyon API başvurusuna](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f)gidin. 

  **Metin - Ekran** sayfası açılır.

## <a name="open-the-api-console"></a>API konsolu açma

**API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

  ![Metin - Ekran sayfası bölge seçimi](images/test-drive-region.png)

  **Metin - Ekran** API konsolu açılır.

## <a name="select-the-inputs"></a>Girişleri seçin

### <a name="parameters"></a>Parametreler

Metin ekranınızda kullanmak istediğiniz sorgu parametrelerini seçin. Bu örnekte, **dil**için varsayılan değeri kullanın. İşlem yürütmenin bir parçası olarak olası dili otomatik olarak algılayacaktır, çünkü boş da bırakabilirsiniz.

> [!NOTE]
> **Dil** parametresi için `eng` makine destekli **sınıflandırma** yanıtını (önizleme özelliği) görmek için atayın veya boş bırakın. **Bu özellik yalnızca İngilizce'yi destekler.**
>
> **Küfür terimleri** algılamaiçin, bu makalede listelenen desteklenen dillerin [ISO 639-3 kodunu](http://www-01.sil.org/iso639-3/codes.asp) kullanın veya boş bırakın.

**Otomatik düzeltme**, **KIŞISEL ,** ve **sınıflandırmak (önizleme)** için, **doğru**seçin. **ListId** alanını boş bırakın.

  ![Metin - Ekran konsolu sorgu parametreleri](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>İçerik türü

**İçerik Türü**için, ekrana gelmek istediğiniz içerik türünü seçin. Bu örnekte, varsayılan **metin/düz** içerik türünü kullanın. **Ocp-Apim-Abonelik-Anahtar** kutusuna abonelik anahtarınızı girin.

### <a name="sample-text-to-scan"></a>Tatmak için örnek metin

İstek **gövde** kutusuna, bazı metin girin. Aşağıdaki örnekte metinde kasıtlı bir yazım hatası gösterilmektedir.

```
Is this a grabage or crap email abcdef@abcd.com, phone: 4255550111, IP: 255.255.255.255, 1234 Main Boulevard, Panapolis WA 96555. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

## <a name="analyze-the-response"></a>Yanıtı analiz edin

Aşağıdaki yanıt, API'den gelen çeşitli görüşleri gösterir. Potansiyel küfür, kişisel veriler, sınıflandırma (önizleme) ve otomatik olarak düzeltilmiş sürümü içerir.

> [!NOTE]
> Makine destekli 'Sınıflandırma' özelliği önizlemededir ve yalnızca İngilizce'yi destekler.

```json
{"OriginalText":"Is this a grabage or crap email abcdef@abcd.com, phone: 4255550111, IP: 255.255.255.255, 1234 Main Boulevard, Panapolis WA 96555.\r\nThese are all UK phone numbers: +44 123 456 7890 or 0234 567 8901 or 0456 789 0123.\r\nAlso, 999-99-9999 looks like a social security number (SSN).",
"NormalizedText":"Is this a grabage or crap email abcdef@ abcd. com, phone: 4255550111, IP: 255. 255. 255. 255, 1234 Main Boulevard, Panapolis WA 96555. \r\nThese are all UK phone numbers: +44 123 456 7890 or 0234 567 8901 or 0456 789 0123. \r\nAlso, 999- 99- 9999 looks like a social security number ( SSN) .",
"Misrepresentation":null,
"PII":{  
  "Email":[  
    {  
      "Detected":"abcdef@abcd.com",
      "SubType":"Regular",
      "Text":"abcdef@abcd.com",
      "Index":32
    }
  ],
  "IPA":[  
    {  
      "SubType":"IPV4",
      "Text":"255.255.255.255",
      "Index":72
    }
  ],
  "Phone":[  
    {  
      "CountryCode":"US",
      "Text":"4255550111",
      "Index":56
    },
    {  
      "CountryCode":"US",
      "Text":"425 555 0111",
      "Index":211
    },
    {  
      "CountryCode":"UK",
      "Text":"+44 123 456 7890",
      "Index":207
    },
    {  
      "CountryCode":"UK",
      "Text":"0234 567 8901",
      "Index":227
    },
    {  
      "CountryCode":"UK",
      "Text":"0456 789 0123",
      "Index":244
    }
  ],
  "Address":[  
    {  
      "Text":"1234 Main Boulevard, Panapolis WA 96555",
      "Index":89
    }
  ],
  "SSN":[  
    {  
      "Text":"999999999",
      "Index":56
    },
    {  
      "Text":"999-99-9999",
      "Index":266
    }
  ]
},
"Classification":{  
  "ReviewRecommended":true,
  "Category1":{  
    "Score":1.5113095059859916E-06
  },
  "Category2":{  
    "Score":0.12747249007225037
  },
  "Category3":{  
    "Score":0.98799997568130493
  }
},
"Language":"eng",
"Terms":[  
  {  
    "Index":21,
    "OriginalIndex":21,
    "ListId":0,
    "Term":"crap"
  }
],
"Status":{  
  "Code":3000,
  "Description":"OK",
  "Exception":null
},
"TrackingId":"2eaa012f-1604-4e36-a8d7-cc34b14ebcb4"
}
```

JSON yanıtındaki tüm bölümlerin ayrıntılı bir açıklaması için [Metin Moderasyonu](text-moderation-api.md) kavramsal kılavuzuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API'sini kullanın veya uygulamanızla tümleştirmek için [.NET SDK'yı hızlı bir şekilde izleyin.](dotnet-sdk-quickstart.md)
