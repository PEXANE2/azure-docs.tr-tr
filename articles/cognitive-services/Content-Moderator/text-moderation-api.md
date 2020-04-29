---
title: Metin denetleme-Content Moderator
titleSuffix: Azure Cognitive Services
description: Olası istenmeyen metinler, kişisel veriler ve hüküm özel listeleri için metin denetlemeyi kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 41e88dd5a08de485f770559959843ba3b54e590f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81274019"
---
# <a name="learn-text-moderation-concepts"></a>Metin denetleme kavramlarını öğrenin

Content Moderator makine yardımlı metin denetimi ve [insan incelemesi](Review-Tool-User-Guide/human-in-the-loop.md) yeteneklerini, orta düzey metin içeriğine göre kullanın.

İlke ve eşiklerinize göre içerikleri engelleyebilir, onaylayabilir veya gözden geçirebilirsiniz. İş ortaklarının, çalışanların ve tüketicilerin metin içeriği oluşturduğu ortamların insan yönetimini artırmak için bu uygulamayı kullanın. Bu kapsama sohbet odaları, tartışma panoları, sohbet botları, e-ticaret katalogları ve belgeler dahildir. 

Hizmet yanıtı aşağıdaki bilgileri içerir:

- Küfür: çeşitli dillerde yerleşik küfürlü terimleri yerleşik listesiyle terim tabanlı eşleştirme
- Sınıflandırma: üç kategoriye makine yardımlı sınıflandırma
- Kişisel veriler
- Otomatik düzeltilen metin
- Özgün metin
- Dil

## <a name="profanity"></a>Küfür

API, [desteklenen dillerin](Text-Moderation-API-Languages.md)herhangi birinde herhangi bir küfürlü terimi algılarsa, bu terimler yanıta dahil edilir. Yanıt, özgün metindeki konumunu (`Index`) da içerir. `ListId` AŞAĞıDAKI örnek JSON, varsa [özel terim listelerinde](try-terms-list-api.md) bulunan terimleri ifade eder.

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 0,
        "Term": "crap"
    }

> [!NOTE]
> **Dil** parametresi için, makine yardımlı `eng` **Sınıflandırma** yanıtını (Önizleme özelliği) görmek üzere boş bırakın veya boş bırakın. **Bu özellik yalnızca İngilizce 'yi destekler**.
>
> **Küfür terimleri** algılama için, bu makalede listelenen desteklenen dillerin [ISO 639-3 kodunu](http://www-01.sil.org/iso639-3/codes.asp) kullanın veya boş bırakın.

## <a name="classification"></a>Sınıflandırma

Content Moderator makine yardımlı **metin sınıflandırma özelliği** **yalnızca İngilizce**'yi destekler ve istenmeyebilecek içerikleri algılamaya yardımcı olur. Bayrak eklenmiş içerik, bağlama bağlı olarak uygun şekilde değerlendirilemeyebilir. Her kategorinin oluşma olasılığını artırırken, bir insan incelemesi önermeyebilir. Özelliği, olası rahatsız edici, negatif veya Discriminatory dilini belirlemek için eğitilen bir model kullanır. Bu, gözden geçirilmek üzere Slang, kısaltılmış sözcükler, rahatsız edici ve bilerek yanlış yazılmış sözcükler içerir. 

JSON ayıklamada aşağıdaki ayıklama örnek bir çıktı gösterir:

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

- `Category1`belirli durumlarda cinsel açık veya yetişkin olarak değerlendirilen dilin potansiyel olma durumunu ifade eder.
- `Category2`, belirli durumlarda cinsel veya kötü bir şekilde düşünülmeyen dilin potansiyel olma durumunu gösterir.
- `Category3`belirli durumlarda rahatsız edici olarak değerlendirilen dilin potansiyel olma durumunu ifade eder.
- `Score`0 ile 1 arasındadır. Puan arttıkça, modelin daha yüksek olması kategorinin uygulanabilir olabileceğini tahmin edilir. Bu özellik el ile kodlanmış sonuçlar yerine istatistiksel bir model kullanır. Her kategorinin gereksinimlerinize göre nasıl hizalanacağını öğrenmek için kendi içeriklerinizi test etmenizi öneririz.
- `ReviewRecommended`, iç puan eşiklerine bağlı olarak doğru ya da yanlış şeklindedir. Müşteriler, bu değerin kullanılıp kullanılmayacağını ya da içerik ilkelerine bağlı olarak özel eşiklere karar vermesini değerlendirmelidir.

## <a name="personal-data"></a>Kişisel veriler

Kişisel veriler özelliği, bu bilgilerin olası varlığını algılar:

- E-posta adresi
- ABD posta adresi
- IP adresi
- ABD telefon numarası

Aşağıdaki örnekte bir örnek yanıt gösterilmektedir:

```json
"pii":{
  "email":[
      {
        "detected":"abcdef@abcd.com",
        "sub_type":"Regular",
        "text":"abcdef@abcd.com",
        "index":32
      }
  ],
  "ssn":[

  ],
  "ipa":[
      {
        "sub_type":"IPV4",
        "text":"255.255.255.255",
        "index":72
      }
  ],
  "phone":[
      {
        "country_code":"US",
        "text":"6657789887",
        "index":56
      }
  ],
  "address":[
      {
        "text":"1 Microsoft Way, Redmond, WA 98052",
        "index":89
      }
  ]
}
```

## <a name="auto-correction"></a>Otomatik Düzeltme

Giriş metninin (' lzay ' ve ' f0x ' kasıtlı olarak) olduğunu varsayalım:

    The qu!ck brown f0x jumps over the lzay dog.

Otomatik düzeltme için sorun yaparsanız, yanıt metnin düzeltilmiş sürümünü içerir:

    The quick brown fox jumps over the lazy dog.

## <a name="creating-and-managing-your-custom-lists-of-terms"></a>Özel terim listelerinizi oluşturma ve yönetme

Varsayılan, koşulların genel listesi çoğu durumda harika olduğundan, iş gereksinimlerinize özgü koşullara göre ekran yapmak isteyebilirsiniz. Örneğin, kullanıcılar tarafından gönderilen tüm rekabetçi marka adlarını filtrelemek isteyebilirsiniz.

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

Aşağıdaki örnek eşleşen liste KIMLIĞINI gösterir:

    "Terms": [
    {
        "Index": 118,
        "OriginalIndex": 118,
        "ListId": 231.
        "Term": "crap"
    }

Content Moderator, özel terim listelerini yönetmeye yönelik işlemler içeren bir [terim listesi API 'si](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) sağlar. [LISTE API konsolu](try-terms-list-api.md) ' nu başlatın ve REST API kod örneklerini kullanın. Ayrıca, Visual Studio ve C# hakkında bilginiz varsa [.net hızlı başlangıç terimini](term-lists-quickstart-dotnet.md) inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

[Metin denetleme API konsolunun](try-text-api.md) sürücüsünü test edin ve REST API kod örneklerini kullanın. Ayrıca, Visual Studio ve C# hakkında bilginiz varsa [.NET SDK hızlı başlangıç](dotnet-sdk-quickstart.md) 'nin metin denetleme bölümüne göz atın.
