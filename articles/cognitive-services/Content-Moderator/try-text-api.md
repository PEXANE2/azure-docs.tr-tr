---
title: Metin denetleme API-Content Moderator kullanarak orta metin
titleSuffix: Azure Cognitive Services
description: Çevrimiçi konsolundaki metin denetleme API 'sini kullanarak test-sürücü metin denetimi.
services: cognitive-services
author: sanjeev3
ms.author: sajagtap
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: 49047c21f86530aac6ed3e0138a643fd08c35af8
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561190"
---
# <a name="moderate-text-from-the-api-console"></a>API konsolundan orta metin

Metin içeriğini küfür için taramak ve özel ve paylaşılan listelerle karşılaştırmak için Azure Content Moderator 'de [metin denetleme API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f) 'sini kullanın.

## <a name="get-your-api-key"></a>API anahtarınızı alın

Çevrimiçi konsolda API 'YI test etmeden önce, abonelik anahtarınız olmalıdır. Bu, **Ayarlar** sekmesinde, **OCP-apim-Subscription-Key** kutusunda bulunur. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="navigate-to-the-api-reference"></a>API başvurusuna git

[Metin denetleme API başvurusuna](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f)gidin. 

  **Metin ekranı** sayfası açılır.

## <a name="open-the-api-console"></a>API konsolunu açın

**Open API test konsolu**için, konumunuzu en iyi şekilde açıklayan bölgeyi seçin. 

  ![Metin-ekran sayfa bölgesi seçimi](images/test-drive-region.png)

  **Metin ekranı** API konsolu açılır.

## <a name="select-the-inputs"></a>Girişleri seçin

### <a name="parameters"></a>Parametreler

Metin ekranınızda kullanmak istediğiniz sorgu parametrelerini seçin. Bu örnek için **dil**için varsayılan değeri kullanın. Ayrıca işlem, yürütmenin bir parçası olarak olası dili otomatik olarak algılayacağından boş bırakabilirsiniz.

> [!NOTE]
> **Dil** parametresi için, `eng` makine yardımlı **Sınıflandırma** yanıtını (Önizleme özelliği) görmek üzere boş bırakın veya boş bırakın. **Bu özellik yalnızca İngilizce 'yi destekler**.
>
> **Küfür terimleri** algılama için, bu makalede listelenen desteklenen dillerin [ISO 639-3 kodunu](http://www-01.sil.org/iso639-3/codes.asp) kullanın veya boş bırakın.

**Otomatik Düzelt**, **PII**ve **sınıflandır (Önizleme)** için **doğru**öğesini seçin. **ListId** alanını boş bırakın.

  ![Metin-ekran konsolu sorgu parametreleri](images/text-api-console-inputs.PNG)

### <a name="content-type"></a>İçerik türü

**Içerik türü**için, ekranın istediğiniz içerik türünü seçin. Bu örnek için varsayılan **metin/düz** içerik türünü kullanın. **OCP-apim-Subscription-Key** kutusuna abonelik anahtarınızı girin.

### <a name="sample-text-to-scan"></a>Taranacak örnek metin

**İstek gövdesi** kutusuna bir metin girin. Aşağıdaki örnek, metinde isteyerek bir yazım hatası gösterir.

> [!NOTE]
> Aşağıdaki örnek metinde geçersiz sosyal güvenlik numarası bilerek yapılır. Amaç, örnek giriş ve çıkış biçimini iletsağlamaktır.

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.
Also, 999-99-9999 looks like a social security number (SSN).
```

## <a name="analyze-the-response"></a>Yanıtı çözümle

Aşağıdaki yanıt, API 'deki çeşitli öngörüleri gösterir. Olası bir küfür, kişisel veriler, sınıflandırma (Önizleme) ve otomatik düzeltilmiş sürüm içerir.

> [!NOTE]
> Makine destekli ' sınıflandırma ' özelliği önizlemededir ve yalnızca Ingilizce 'yi destekler.

```json
{"OriginalText":"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.\r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300.\r\nAlso, 544-56-7788 looks like a social security number (SSN).",
"NormalizedText":"Is this a grabage or crap email abcdef@ abcd. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. \r\nThese are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. \r\nAlso, 544- 56- 7788 looks like a social security number ( SSN) .",
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
      "Text":"6657789887",
      "Index":56
    },
    {  
      "CountryCode":"US",
      "Text":"870 608 4000",
      "Index":211
    },
    {  
      "CountryCode":"UK",
      "Text":"+44 870 608 4000",
      "Index":207
    },
    {  
      "CountryCode":"UK",
      "Text":"0344 800 2400",
      "Index":227
    },
    {  
      "CountryCode":"UK",
      "Text":"0800 820 3300",
      "Index":244
    }
  ],
  "Address":[  
    {  
      "Text":"1 Microsoft Way, Redmond, WA 98052",
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

JSON yanıtındaki tüm bölümlerin ayrıntılı bir açıklaması için, [metin denetleme](text-moderation-api.md) kavramsal kılavuzuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API kullanın veya uygulamanızla tümleştirmek üzere [metin denetleme .net hızlı](text-moderation-quickstart-dotnet.md) başlangıcı ile başlayın.
