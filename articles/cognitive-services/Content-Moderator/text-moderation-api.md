---
title: Metin Moderasyonu - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Olası istenmeyen metin, kişisel veriler ve özel terimler listeleri için metin moderasyonunu kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 5a07f0749b59efc96b67df3ad5ed2fbf353be614
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74538836"
---
# <a name="learn-text-moderation-concepts"></a>Metin moderasyon kavramlarını öğrenin

Metin içeriğini ortalamak için İçerik Moderatör'ün makine destekli metin moderasyonunu ve [insan inceleme](Review-Tool-User-Guide/human-in-the-loop.md) özelliklerini kullanın.

İlke ve eşiklerinize göre içerikleri engelleyebilir, onaylayabilir veya gözden geçirebilirsiniz. İş ortaklarının, çalışanların ve tüketicilerin metin içeriği ürettiği ortamların insanmoderasyonunu artırmak için kullanın. Bu kapsama sohbet odaları, tartışma panoları, sohbet botları, e-ticaret katalogları ve belgeler dahildir. 

Hizmet yanıtı aşağıdaki bilgileri içerir:

- Küfür: çeşitli dillerde küfür terimleri yerleşik listesi ile terim tabanlı eşleştirme
- Sınıflandırma: makine destekli sınıflandırma üç kategoriye ayrılmıştır
- Kişisel veriler
- Otomatik düzeltilmiş metin
- Orijinal metin
- Dil

## <a name="profanity"></a>Küfür

API [desteklenen dillerin](Text-Moderation-API-Languages.md)herhangi birinde herhangi bir saygısız terim algılarsa, bu terimler yanıta dahil edilir. Yanıt, özgün metinde`Index`konumlarını da içerir. Aşağıdaki `ListId` örnekte JSON, varsa [özel terim listelerinde](try-terms-list-api.md) bulunan terimleri ifade eder.

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 0,
        "Term": "crap"
    }

> [!NOTE]
> **Dil** parametresi için `eng` makine destekli **sınıflandırma** yanıtını (önizleme özelliği) görmek için atayın veya boş bırakın. **Bu özellik yalnızca İngilizce'yi destekler.**
>
> **Küfür terimleri** algılamaiçin, bu makalede listelenen desteklenen dillerin [ISO 639-3 kodunu](http://www-01.sil.org/iso639-3/codes.asp) kullanın veya boş bırakın.

## <a name="classification"></a>Sınıflandırma

İçerik Moderatör'ün makine destekli **metin sınıflandırma özelliği** yalnızca **İngilizce'yi**destekler ve istenmeyen içeriğin algılenmesine yardımcı olur. İşaretlenen içerik, içeriğe bağlı olarak uygunsuz olarak değerlendirilebilir. Her kategorinin olasılığını iletir ve bir insan incelemesi önerebilir. Özellik, olası kötü amaçlı, aşağılayıcı veya ayrımcı dili tanımlamak için eğitilmiş bir model kullanır. Bu argo, kısaltılmış kelimeler, saldırgan ve kasıtlı olarak gözden geçirilmesi için yanlış yazılmış sözcükleri içerir. 

JSON ekstresindeki aşağıdaki ekstre örnek bir çıktı gösterir:

    "Classification": {
        "ReviewRecommended": true,
        "Category1": {
            "Score": 1.5113095059859916E-06
            },
        "Category2": {
            "Score": 0.12747249007225037
            },
        "Category3": {
            "Score": 0.98799997568130493
        }
    }

### <a name="explanation"></a>Açıklama

- `Category1`bazı durumlarda müstehcen veya yetişkin olarak kabul edilebilecek dilin potansiyel varlığı anlamına gelir.
- `Category2`bazı durumlarda cinsel açıdan müstehcen veya olgun olarak kabul edilebilecek dilin potansiyel varlığı anlamına gelir.
- `Category3`belirli durumlarda rahatsız edici sayılabilecek dilin olası varlığı anlamına gelir.
- `Score`0 ile 1 arasındadır. Puan ne kadar yüksekse, model kategorinin uygulanabilir olabileceğini tahmin ediyor. Bu özellik, el ile kodlanmış sonuçlar yerine istatistiksel bir modele dayanır. Her kategorinin gereksinimlerinize nasıl uygun olduğunu belirlemek için kendi içeriğinizle test yapmanızı öneririz.
- `ReviewRecommended`iç puan eşiklerine bağlı olarak doğru veya yanlıştır. Müşteriler bu değeri kullanıp kullanmayacağını veya içerik ilkelerine göre özel eşiklere karar verip vermeyeceğine karar vermelidir.

## <a name="personal-data"></a>Kişisel veriler

Kişisel veri özelliği bu bilgilerin olası varlığını algılar:

- E-posta adresi
- ABD Posta adresi
- IP adresi
- ABD Telefon numarası
- İngiltere Telefon numarası
- Sosyal Güvenlik Numarası (SSN)

Aşağıdaki örnek yanıt ı gösterir:

```json
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
      "Index":212
    },
    { 
      "CountryCode":"UK",
      "Text":"+123 456 7890",
      "Index":208
    },
    { 
      "CountryCode":"UK",
      "Text":"0234 567 8901",
      "Index":228
    },
    { 
      "CountryCode":"UK",
      "Text":"0456 789 0123",
      "Index":245
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
      "Index":267
    }
  ]
}
```

## <a name="auto-correction"></a>Otomatik düzeltme

Giriş metninin ('lzay' ve 'f0x' kasıtlı olduğunu varsayalım):

    The qu!ck brown f0x jumps over the lzay dog.

Otomatik düzeltme isterseniz, yanıt metnin düzeltilmiş sürümünü içerir:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Özel terimler listelerinizi oluşturma ve yönetme

Varsayılan, terimlerin genel listesi çoğu durumda harika çalışsa da, işletme gereksinimlerinize özgü terimlere karşı ekran alabilirsiniz. Örneğin, kullanıcıların gönderilerinden herhangi bir rekabetçi marka adlarını filtrelemek isteyebilirsiniz.

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

Aşağıdaki örnekte eşleşen Liste Kimliği gösterilmektedir:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

İçerik Moderatör özel dönem listeleri yönetmek için işlemleri ile bir [Dönem Listesi API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) sağlar. [Dönem Listeleri API Konsolu](try-terms-list-api.md) ile başlayın ve REST API kod örneklerini kullanın. Visual Studio ve C#'a aşinaysanız [Terim Listeleri .NET quickstart'a](term-lists-quickstart-dotnet.md) da göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Metin moderasyonu API konsolunu](try-text-api.md) test edin ve REST API kod örneklerini kullanın. Visual Studio ve C#'a aşinaysanız [.NET SDK quickstart'ın](dotnet-sdk-quickstart.md) Metin Moderasyon bölümüne de göz atın.
