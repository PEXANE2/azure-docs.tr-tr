---
title: Content Moderator içindeki C# özel bir terim listesine karşı metin denetleme
titleSuffix: Azure Cognitive Services
description: İçin C#Content moderator SDK kullanarak özel terim listeleriyle nasıl bir metin vardır.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: sajagtap
ms.openlocfilehash: d1c2f8b06d333be23f25a2d150c23269bf84cd2e
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72242837"
---
# <a name="check-text-against-a-custom-term-list-in-c"></a>İçindeki özel bir terim listesine karşı metin denetleC#

Azure Content Moderator 'deki varsayılan genel liste, içerik denetleme gereksinimlerinin çoğu için yeterlidir. Ancak, kuruluşunuza özgü terimler için ekran yapmanız gerekebilir. Örneğin, daha fazla inceleme için rakip adlarını etiketlemek isteyebilirsiniz. 

Metin denetleme API 'siyle birlikte kullanılacak özel terim listeleri oluşturmak için [.NET için Content moderator SDK 'sını](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) kullanabilirsiniz.

Bu makalede, .NET için Content Moderator SDK 'yı kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri sunulmaktadır:
- Bir liste oluşturun.
- Koşulları bir listeye ekleyin.
- Bir listedeki koşullara göre ekran terimleri.
- Bir listeden koşulları silin.
- Bir listeyi silin.
- Liste bilgilerini düzenleyin.
- Listedeki değişikliklerin yeni bir taramaya dahil edilmesini sağlamak için dizini yenileyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-up-for-content-moderator-services"></a>Content Moderator hizmetlere kaydolun

REST API veya SDK aracılığıyla Content Moderator Hizmetleri kullanabilmeniz için önce bir abonelik anahtarı gerekir. Bir [Azure portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) Content moderator hizmetine abone olun.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturma

1. Çözümünüze yeni bir **konsol uygulaması (.NET Framework)** projesi ekleyin.

1. Projeyi **Termlists**olarak adlandırın. Bu projeyi çözüm için tek bir başlangıç projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükler

TermLists projesi için aşağıdaki NuGet paketlerini yükler:

- Microsoft. Azure. Biliveservices. Contentmoderatör
- Microsoft. Rest. ClientRuntime
- Microsoft. Rest. ClientRuntime. Azure
- Newtonsoft. JSON

### <a name="update-the-programs-using-statements"></a>Programın using deyimlerini güncelleştirme

Aşağıdaki `using` deyimlerini ekleyin.

```csharp
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.Azure.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
```

### <a name="create-the-content-moderator-client"></a>Content Moderator istemcisini oluşturma

Aboneliğiniz için bir Content Moderator istemcisi oluşturmak üzere aşağıdaki kodu ekleyin.

> [!IMPORTANT]
> **Azureregion** ve **cmsubscriptionkey** alanlarını bölge tanımlayıcı ve abonelik anahtarlarınızın değerleriyle güncelleştirin.

```csharp
/// <summary>
/// Wraps the creation and configuration of a Content Moderator client.
/// </summary>
/// <remarks>This class library contains insecure code. If you adapt this 
/// code for use in production, use a secure method of storing and using
/// your Content Moderator subscription key.</remarks>
public static class Clients
{
    /// <summary>
    /// The region/location for your Content Moderator account, 
    /// for example, westus.
    /// </summary>
    private static readonly string AzureRegion = "YOUR API REGION";

    /// <summary>
    /// The base URL fragment for Content Moderator calls.
    /// </summary>
    private static readonly string AzureBaseURL =
        $"https://{AzureRegion}.api.cognitive.microsoft.com";

    /// <summary>
    /// Your Content Moderator subscription key.
    /// </summary>
    private static readonly string CMSubscriptionKey = "YOUR API KEY";

    /// <summary>
    /// Returns a new Content Moderator client for your subscription.
    /// </summary>
    /// <returns>The new client.</returns>
    /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
    /// When you have finished using the client,
    /// you should dispose of it either directly or indirectly. </remarks>
    public static ContentModeratorClient NewClient()
    {
        // Create and initialize an instance of the Content Moderator API wrapper.
        ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

        client.Endpoint = AzureBaseURL;
        return client;
    }
}
```

### <a name="add-private-properties"></a>Özel özellikler ekleme

Aşağıdaki özel özellikleri TermLists, Class program ad alanına ekleyin.

```csharp
/// <summary>
/// The language of the terms in the term lists.
/// </summary>
private const string lang = "eng";

/// <summary>
/// The minimum amount of time, in milliseconds, to wait between calls
/// to the Content Moderator APIs.
/// </summary>
private const int throttleRate = 3000;

/// <summary>
/// The number of minutes to delay after updating the search index before
/// performing image match operations against the list.
/// </summary>
private const double latencyDelay = 0.5;
```

## <a name="create-a-term-list"></a>Terim listesi oluşturma

**Contentmoderatorclient. Listmanagemen, mLists. Create**ile bir terim listesi oluşturun. **Oluşturulacak** ilk parametre, "Application/JSON" olması gereken bir MIME türü içeren bir dizedir. Daha fazla bilgi için bkz. [API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). İkinci parametre, yeni terim listesi için bir ad ve açıklama içeren bir **gövde** nesnesidir.

> [!NOTE]
> Her liste **10.000 terimi aşmamak**için en fazla **5 dönemli** liste vardır.

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

> [!NOTE]
> Content Moderator hizmet anahtarınız, saniye başına istek (RPS) oranı sınırına sahiptir ve limiti aşarsanız, SDK 429 hata koduyla bir özel durum oluşturur. Ücretsiz katman anahtarında tek RPS hız sınırı vardır.

```csharp
/// <summary>
/// Creates a new term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <returns>The term list ID.</returns>
static string CreateTermList (ContentModeratorClient client)
{
    Console.WriteLine("Creating term list.");

    Body body = new Body("Term list name", "Term list description");
    TermList list = client.ListManagementTermLists.Create("application/json", body);
    if (false == list.Id.HasValue)
    {
        throw new Exception("TermList.Id value missing.");
    }
    else
    {
        string list_id = list.Id.Value.ToString();
        Console.WriteLine("Term list created. ID: {0}.", list_id);
        Thread.Sleep(throttleRate);
        return list_id;
    }
}
```

## <a name="update-term-list-name-and-description"></a>Güncelleştirme terimi liste adı ve açıklaması

Bu terim listesi bilgilerini **Contentmoderatorclient. Listmanagemenlermlists. Update**ile güncelleştirin. **Güncelleştirilecek** ilk parametre, liste kimliğidir. İkinci parametre, "Application/JSON" olması gereken bir MIME türüdür. Daha fazla bilgi için bkz. [API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). Üçüncü parametre, yeni adı ve açıklamayı içeren bir **gövde** nesnesidir.

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

```csharp
/// <summary>
/// Update the information for the indicated term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="list_id">The ID of the term list to update.</param>
/// <param name="name">The new name for the term list.</param>
/// <param name="description">The new description for the term list.</param>
static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
{
    Console.WriteLine("Updating information for term list with ID {0}.", list_id);
    Body body = new Body(name, description);
    client.ListManagementTermLists.Update(list_id, "application/json", body);
    Thread.Sleep(throttleRate);
}
```

## <a name="add-a-term-to-a-term-list"></a>Terim listesine bir terim ekleyin

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

```csharp
/// <summary>
/// Add a term to the indicated term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="list_id">The ID of the term list to update.</param>
/// <param name="term">The term to add to the term list.</param>
static void AddTerm (ContentModeratorClient client, string list_id, string term)
{
    Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
    client.ListManagementTerm.AddTerm(list_id, term, lang);
    Thread.Sleep(throttleRate);
}
```

## <a name="get-all-terms-in-a-term-list"></a>Terim listesindeki tüm terimleri al

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

```csharp
/// <summary>
/// Get all terms in the indicated term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="list_id">The ID of the term list from which to get all terms.</param>
static void GetAllTerms(ContentModeratorClient client, string list_id)
{
    Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
    Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
    TermsData data = terms.Data;
    foreach (TermsInList term in data.Terms)
    {
        Console.WriteLine(term.Term);
    }
    Thread.Sleep(throttleRate);
}
```

## <a name="add-code-to-refresh-the-search-index"></a>Arama dizinini yenilemek için kod ekleme

Bir terim listesinde değişiklik yaptıktan sonra, bir dahaki sefer için terim listesini bir sonraki açışınızda görüntülenecek değişiklikler için arama dizinini yenileyin. Bu, masaüstünüzdeki bir arama altyapısının (etkinse) veya bir Web araması altyapısının, yeni dosyaları veya sayfaları içerecek şekilde dizinini sürekli olarak yenilediğine benzer.

Bir terim listesi arama dizinini **Contentmoderatorclient. Listmanagemen, mLists. Refreshındexmethod**ile yenileyin.

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

```csharp
/// <summary>
/// Refresh the search index for the indicated term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="list_id">The ID of the term list to refresh.</param>
static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
{
    Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
    client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
    Thread.Sleep((int)(latencyDelay * 60 * 1000));
}
```

## <a name="screen-text-using-a-term-list"></a>Bir terim listesi kullanan ekran metni

Aşağıdaki parametreleri alan **Contentmoderatorclient. Textdenetlemesi. screentext**ile bir terim listesi kullanarak metin görürsünüz.

- Terim listesindeki koşulların dili.
- "Metin/html", "metin/xml", "metin/markaşağı" veya "metin/düz" olabilen bir MIME türü.
- Ekrandaki metin.
- Boolean değeri. Metni filtrelemeden önce otomatik hale eklemek için bu alanı **true** olarak ayarlayın.
- Boolean değeri. Metinde kişisel olarak tanımlanabilen bilgileri (PII) algılamak için bu alanı **doğru** olarak ayarlayın.
- Terim listesi KIMLIĞI.

Daha fazla bilgi için bkz. [API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

Ekran **metni** , filtreleme içinde Content moderator algılanan terimleri listeleyen bir **terms** özelliği olan bir **ekran** nesnesi döndürür. Content Moderator, filtreleme sırasında herhangi bir terim algılamamışsa, **terms** özelliğinin **null**değerine sahip olduğunu unutmayın.

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

```csharp
/// <summary>
/// Screen the indicated text for terms in the indicated term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="list_id">The ID of the term list to use to screen the text.</param>
/// <param name="text">The text to screen.</param>
static void ScreenText (ContentModeratorClient client, string list_id, string text)
{
    Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
    Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
    if (null == screen.Terms)
    {
        Console.WriteLine("No terms from the term list were detected in the text.");
    }
    else
    {
        foreach (DetectedTerms term in screen.Terms)
        {
            Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
        }
    }
    read.Sleep(throttleRate);
}
```

## <a name="delete-terms-and-lists"></a>Terimleri ve listeleri sil

Bir terimi veya listeyi silme işlemi basittir. Aşağıdaki görevleri yapmak için SDK 'Yı kullanın:

- Bir terimi silin. (**Contentmoderatorclient. Listmanagemenbir. deleteterm**)
- Listeyi silmeden bir listedeki tüm terimleri silin. (**Contentmoderatorclient. Listmanagemen, d. DeleteAllTerms**)
- Bir listeyi ve tüm içeriğini silin. (**Contentmoderatorclient. Listmanagemen, mLists. Delete**)

### <a name="delete-a-term"></a>Terimi silme

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

```csharp
/// <summary>
/// Delete a term from the indicated term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="list_id">The ID of the term list from which to delete the term.</param>
/// <param name="term">The term to delete.</param>
static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
{
    Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
    client.ListManagementTerm.DeleteTerm(list_id, term, lang);
    Thread.Sleep(throttleRate);
}
```

### <a name="delete-all-terms-in-a-term-list"></a>Bir terim listesindeki tüm terimleri silme

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

```csharp
/// <summary>
/// Delete all terms from the indicated term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="list_id">The ID of the term list from which to delete all terms.</param>
static void DeleteAllTerms (ContentModeratorClient client, string list_id)
{
    Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
    client.ListManagementTerm.DeleteAllTerms(list_id, lang);
    Thread.Sleep(throttleRate);
}
```

### <a name="delete-a-term-list"></a>Terim listesini silme

Aşağıdaki yöntem tanımını ad alanı TermLists, Class program öğesine ekleyin.

```csharp
/// <summary>
/// Delete the indicated term list.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="list_id">The ID of the term list to delete.</param>
static void DeleteTermList (ContentModeratorClient client, string list_id)
{
    Console.WriteLine("Deleting term list with ID {0}.", list_id);
    client.ListManagementTermLists.Delete(list_id);
    Thread.Sleep(throttleRate);
}
```

## <a name="compose-the-main-method"></a>Main metodunu oluşturma

**Ana** yöntem tanımını ad alanı **termlists**, Class **Program**öğesine ekleyin. Son olarak, **Program** sınıfını ve **termlists** ad alanını kapatın.

```csharp
static void Main(string[] args)
{
    using (var client = Clients.NewClient())
    {
        string list_id = CreateTermList(client);

        UpdateTermList(client, list_id, "name", "description");
        AddTerm(client, list_id, "term1");
        AddTerm(client, list_id, "term2");

        GetAllTerms(client, list_id);

        // Always remember to refresh the search index of your list
        RefreshSearchIndex(client, list_id);

        string text = "This text contains the terms \"term1\" and \"term2\".";
        ScreenText(client, list_id, text);

        DeleteTerm(client, list_id, "term1");

        // Always remember to refresh the search index of your list
        RefreshSearchIndex(client, list_id);

        text = "This text contains the terms \"term1\" and \"term2\".";
        ScreenText(client, list_id, text);

        DeleteAllTerms(client, list_id);
        DeleteTermList(client, list_id);

        Console.WriteLine("Press ENTER to close the application.");
        Console.ReadLine();
    }
}
```

## <a name="run-the-application-to-see-the-output"></a>Çıktıyı görmek için uygulamayı çalıştırın

Konsol çıktılarınız aşağıdakine benzer şekilde görünür:

```console
Creating term list.
Term list created. ID: 252.
Updating information for term list with ID 252.

Adding term "term1" to term list with ID 252.
Adding term "term2" to term list with ID 252.

Getting terms in term list with ID 252.
term1
term2

Refreshing search index for term list with ID 252.

Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
Found term: "term1" from list ID 252 at index 32.
Found term: "term2" from list ID 252 at index 46.

Removed term "term1" from term list with ID 252.

Refreshing search index for term list with ID 252.

Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
Found term: "term2" from list ID 252 at index 46.

Removing all terms from term list with ID 252.
Deleting term list with ID 252.
Press ENTER to close the application.
```

## <a name="next-steps"></a>Sonraki adımlar

.NET için bu ve diğer Content Moderator hızlı başlangıçlara yönelik [Content moderator .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözümü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) alın ve tümleştirmenizi kullanmaya başlayın.
