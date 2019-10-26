---
title: 'Hızlı başlangıç: Content Moderator metin içeriğini C# analiz etme'
titleSuffix: Azure Cognitive Services
description: .NET için Content Moderator SDK kullanarak çeşitli sakıncalı malzemeler için metin içeriğini çözümleme
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: quickstart
ms.date: 10/24/2019
ms.author: pafarley
ms.openlocfilehash: 51202597aaee2940a764d0385d4d93c7409f4905
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72935917"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-c"></a>Hızlı Başlangıç: C# dilinde uygunsuz malzemeler için metin içeriğini analiz etme

Bu makalede, [.NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) için Content Moderator SDK'sını kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanır. Potansiyel olarak sakıncalı olan malzemelere yönelik olarak, metin içeriğinin terim tabanlı filtrelemesini ve sınıflandırmasını nasıl çalıştıracağınızı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Önkoşullar
- Content Moderator abonelik anahtarı. Content Moderator abone olmak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin. Ardından, sırasıyla `CONTENT_MODERATOR_SUBSCRIPTION_KEY` ve `CONTENT_MODERATOR_ENDPOINT`adlı anahtar ve uç nokta URL 'SI için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü

> [!NOTE]
> Bu kılavuzda ücretsiz katmanı Content Moderator aboneliği kullanılır. Her abonelik katmanında nelerin sağlandığı hakkında bilgi için [Fiyatlandırma ve sınırlar](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/) sayfasına bakın.

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

1. Visual Studio'da yeni bir **Konsol uygulaması (.NET Framework)** projesi oluşturun ve projeyi **TextModeration** olarak adlandırın. 
1. Çözümünüzde başka projeler de varsa, tek başlangıç projesi olarak bunu seçin.
1. Gerekli NuGet paketini alın. Çözüm Gezgini ' de projenize sağ tıklayın ve **NuGet Paketlerini Yönet**' i seçin. Sonra **`Microsoft.Azure.CognitiveServices.ContentModerator` paketini**bulup yükler. Alternatif olarak, çözüm dizininden aşağıdaki komutu çalıştırabilirsiniz:

```console
dotnet add package Microsoft.Azure.CognitiveServices.ContentModerator
```

## <a name="add-text-moderation-code"></a>Metin moderasyon kodu ekleme

Ardından, temel bir içerik moderasyonu senaryosu uygulamak için kodu bu kılavuzdan kopyalayıp projenize yapıştıracaksınız.

### <a name="include-namespaces"></a>Ad alanlarını ekleme

Aşağıdaki `using` deyimlerini *Program.cs* dosyanızın en üstüne ekleyin.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?name=snippet_using)]

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisini oluşturma

Aboneliğinize bir Content Moderator istemci sağlayıcısı oluşturmak için aşağıdaki kodu *Program.cs* dosyanıza ekleyin. Aynı ad alanında **Program** sınıfının yanına sınıfı ekleyin. **AzureBaseURL** ve **cmsubscriptionkey** alanlarını Endpoint URL 'nizin ve abonelik anahtarınızın değerleriyle güncelleştirmeniz gerekir. Bunları, Azure portal kaynağınızın **hızlı başlangıç** sekmesinde bulabilirsiniz.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?name=snippet_client)]

### <a name="set-up-input-and-output-targets"></a>Giriş ve çıkış hedeflerini ayarlama

Aşağıdaki statik alanları _Program.cs_ dosyasındaki **Program** sınıfına ekleyin. Bu alanlar, giriş metni içeriği ve çıkış JSON içeriği için dosyaları belirtir.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?name=snippet_fields)]

*Textfile. txt* giriş dosyasını oluşturmanız ve yolunu güncelleştirmeniz gerekir (yollar yürütme dizinine göre değişir). _TextFile.txt_ dosyasını açın ve yönetilecek metni ekleyin. Bu hızlı başlangıçta aşağıdaki örnek metin kullanılır:

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="load-the-input-text"></a>Giriş metnini yükleme

Aşağıdaki kodu **Main** yöntemine ekleyin. Temel işlem, **ScreenText** yöntemidir. Yöntemin parametreleri hangi moderasyon işlemlerinin yapılacağını belirtir. Bu örnekte, yöntem şu işlemler için yapılandırılır:
- Metindeki olası küfürleri algılama.
- Metni normalleştirme ve yazım hatalarını otomatik olarak düzeltme.
- ABD ve UK telefon numaraları, e-posta adresleri ve ABD posta adresleri gibi kişisel verileri tespit edin.
- Makine öğrenme tabanlı modelleri kullanarak metni üç kategoride sınıflandırma.

Bu işlemlerin ne yaptığı hakkında daha fazla bilgi edinmek istiyorsanız, [Sonraki adımlar](#next-steps) bölümündeki bağlantıyı izleyin.

[!code-csharp[](~/cognitive-services-content-moderator-samples/documentation-samples/csharp/text-moderation-quickstart-dotnet.cs?name=snippet_main)]

## <a name="run-the-program"></a>Programı çalıştırma

Program JSON dize verilerini _TextModerationOutput.txt_ dosyasına yazacaktır. Bu hızlı başlangıçta kullanılan örnek metin aşağıdaki çıkışı verir:

```json
Autocorrect typos, check for matching terms, check for personal data, and classify.
{
"OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
"NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"Misrepresentation": null,
    
"Classification": {
        "Category1": {
        "Score": 1.5113095059859916E-06
        },
        "Category2": {
        "Score": 0.12747249007225037
        },
        "Category3": {
        "Score": 0.98799997568130493
        },
        "ReviewRecommended": true
  },
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "PII": {
        "Email": [
            {
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 33
            }
            ],
        "IPA": [
            {
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 73
            }
            ],
        "Phone": [
            {
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 57
            },
            {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 211
            },
            {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 207
            },
            {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 227
            },
            {
        "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 244
            }
            ],
         "Address": [{
             "Text": "1 Microsoft Way, Redmond, WA 98052",
             "Index": 89
                }]
    },
  "Language": "eng",
  "Terms": [
    {
        "Index": 22,
        "OriginalIndex": 22,
        "ListId": 0,
        "Term": "crap"
    }
  ],
  "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Content Moderator hizmetini kullanarak belirli bir metin örneği hakkındaki ilgili bilgileri döndüren basit bir .NET uygulaması geliştirdiniz. Bundan sonra, hangi verilere ihtiyacınız olduğuna ve uygulamanızın bunları nasıl işleyeceğine karar verebilmek için farklı bayrakların ve sınıflandırmaların anlamları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Metin moderasyonu kılavuzu](text-moderation-api.md)
