---
title: .NET-Content Moderator kullanarak incelemeler oluşturma
titleSuffix: Azure Cognitive Services
description: .NET için Azure Content Moderator SDK kullanarak incelemeler oluşturma.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: sajagtap
ms.openlocfilehash: e3b36eae4f6dc4343828a38fa4ffe3920a83f589
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72242862"
---
# <a name="create-human-reviews-net"></a>İnsan incelemeleri oluşturma (.NET)

, İnsan moderatör 'nin değerlendirilmesine yönelik depolamayı ve görüntüleme içeriğini gözden geçirir. Bir Kullanıcı bir gözden geçirmeyi tamamladığında, sonuçlar belirtilen geri çağırma uç noktasına gönderilir. Bu kılavuzda, [.NET için Content moderator SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) 'yı kullanmaya başlamanıza yardımcı olacak bilgiler ve kod örnekleri sunulmaktadır:

- İnsan moderatör için bir inceleme kümesi oluşturma
- İnsan moderatör için mevcut incelemelerinin durumunu alın

## <a name="prerequisites"></a>Prerequisites

- Content Moderator [İnceleme aracı](https://contentmoderator.cognitive.microsoft.com/) sitesinde oturum açın veya hesap oluşturun.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>API anahtarınızın İnceleme oluşturma için gözden geçirme API 'sini çağıraerişebildiğinden emin olun

Önceki adımları tamamladıktan sonra, Azure portal başlatıldığında iki Content Moderator anahtarı ile karşılaşabilirsiniz.

SDK örneğinizdeki Azure tarafından sağlanmış API anahtarını kullanmayı planlıyorsanız, uygulamanızın gözden geçirme API 'sini çağırmasını ve incelemeleri oluşturmasını sağlamak için [Azure 'DA API 'yi gözden geçir](./review-tool-user-guide/configure.md#use-your-azure-account-with-the-review-apis) bölümünde bahsedilen adımları izleyin.

İnceleme aracı tarafından oluşturulan ücretsiz deneme anahtarını kullanırsanız, gözden geçirme aracı hesabınız anahtarı daha önce bilir ve bu nedenle ek bir adım gerekmez.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturma

1. Çözümünüze yeni bir **konsol uygulaması (.NET Framework)** projesi ekleyin.

   Örnek kodda, projeyi **Createreviews**olarak adlandırın.

1. Bu projeyi çözüm için tek bir başlangıç projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükler

Aşağıdaki NuGet paketlerini yükler:

- Microsoft. Azure. Biliveservices. Contentmoderatör
- Microsoft. Rest. ClientRuntime
- Newtonsoft. JSON

### <a name="update-the-programs-using-statements"></a>Programın using deyimlerini güncelleştirme

Programın using deyimlerini değiştirin.


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

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>İç içerik bilgilerini bir gözden geçirme KIMLIĞIYLE ilişkilendirmek için bir sınıf oluşturun

Aşağıdaki sınıfı **Program** sınıfına ekleyin. İnceleme KIMLIĞINI öğe için iç içerik KIMLIĞINIZLE ilişkilendirmek için bu sınıfı kullanın.

```csharp
/// <summary>
    /// Associates the review ID (assigned by the service) to the internal
    /// content ID of the item.
    /// </summary>
    public class ReviewItem
    {
        /// <summary>
        /// The media type for the item to review.
        /// </summary>
        public string Type;

        /// <summary>
        /// The URL of the item to review.
        /// </summary>
        public string Url;

        /// <summary>
        /// The internal content ID for the item to review.
        /// </summary>
        public string ContentId;

        /// <summary>
        /// The ID that the service assigned to the review.
        /// </summary>
        public string ReviewId;
    }
```

### <a name="initialize-application-specific-settings"></a>Uygulamaya özgü ayarları başlatma

> [!NOTE]
> Content Moderator hizmet anahtarınız saniyede bir istek (RPS) hız sınırına sahiptir ve sınırı aşarsanız SDK, 429 hata koduyla bir özel durum oluşturur.
>
> Ücretsiz katman anahtarının bir RPS hız sınırı vardır.

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Program.cs içindeki **Program** sınıfına aşağıdaki sabitleri ekleyin

```csharp
/// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const double latencyDelay = 45;

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";
```

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Program.cs içindeki **Program** sınıfına aşağıdaki sabitleri ve statik alanları ekleyin

Bu değerleri, aboneliğinize ve ekibinize özgü bilgileri içerecek şekilde güncelleştirin.

> [!NOTE]
> TeamName sabitini [Content moderator gözden geçirme araç](https://contentmoderator.cognitive.microsoft.com/) aboneliğinizi oluştururken kullandığınız ada ayarlarsınız. Ekip adını **Ayarlar** (dişli) menüsündeki **kimlik bilgileri** bölümünden alırsınız.
>
> Ekip adınız, **API** bölümündeki **ID** alanının değeridir.

```csharp
/// <summary>
/// The name of the team to assign the review to.
/// </summary>
/// <remarks>This must be the team name you used to create your
/// Content Moderator account. You can retrieve your team name from
/// the Content Moderator web site. Your team name is the Id associated
/// with your subscription.</remarks>
private const string TeamName = "YOUR REVIEW TEAM ID";

/// <summary>
/// The optional name of the subteam to assign the review to.
/// </summary>
private const string Subteam = null;

/// <summary>
/// The callback endpoint for completed reviews.
/// </summary>
/// <remarks>Reviews show up for reviewers on your team. 
/// As reviewers complete reviews, results are sent to the
/// callback endpoint using an HTTP POST request.</remarks>
private const string CallbackEndpoint = "YOUR API ENDPOINT";

/// <summary>
/// The media type for the item to review.
/// </summary>
/// <remarks>Valid values are "image", "text", and "video".</remarks>
private const string MediaType = "image";

/// <summary>
/// The URLs of the images to create review jobs for.
/// </summary>
private static readonly string[] ImageUrls = new string[] {
    "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg"
    // add more if you want
};

/// <summary>
/// The metadata key to initially add to each review item.
/// </summary>
private const string MetadataKey = "sc";

/// <summary>
/// The metadata value to initially add to each review item.
/// </summary>
private const string MetadataValue = "true";
```

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Program.cs içindeki **Program** sınıfına aşağıdaki statik alanları ekleyin

Uygulama durumunu izlemek için bu alanları kullanın.

```csharp
/// <summary>
/// A static reference to the text writer to use for logging.
/// </summary>
private static TextWriter writer;

/// <summary>
/// The cached review information, associating a local content ID
/// to the created review ID for each item.
/// </summary>
private static List<ReviewItem> reviewItems =
    new List<ReviewItem>();
```

## <a name="create-a-method-to-write-messages-to-the-log-file"></a>Günlük dosyasına ileti yazmak için bir yöntem oluşturma

**Program** sınıfına aşağıdaki yöntemi ekleyin.

```csharp
/// <summary>
/// Writes a message to the log file, and optionally to the console.
/// </summary>
/// <param name="message">The message.</param>
/// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
private static void WriteLine(string message = null, bool echo = false)
{
    writer.WriteLine(message ?? String.Empty);

    if (echo)
    {
        Console.WriteLine(message ?? String.Empty);
    }
}
```

## <a name="create-a-method-to-create-a-set-of-reviews"></a>İnceleme kümesi oluşturmak için bir yöntem oluşturma

Normalde, gözden geçirilmesi gereken gelen görüntüleri, metinleri veya videoları belirlemek için bir iş mantığınızı kullanabilirsiniz. Ancak, burada yalnızca sabit bir görüntü listesi kullanılır.

**Program** sınıfına aşağıdaki yöntemi ekleyin.

```csharp
/// <summary>
/// Create the reviews using the fixed list of images.
/// </summary>
/// <param name="client">The Content Moderator client.</param>
private static void CreateReviews(ContentModeratorClient client)
{
    WriteLine(null, true);
    WriteLine("Creating reviews for the following images:", true);

    // Create the structure to hold the request body information.
    List<CreateReviewBodyItem> requestInfo =
        new List<CreateReviewBodyItem>();

    // Create some standard metadata to add to each item.
    List<CreateReviewBodyItemMetadataItem> metadata =
        new List<CreateReviewBodyItemMetadataItem>(
        new CreateReviewBodyItemMetadataItem[] {
            new CreateReviewBodyItemMetadataItem(
                MetadataKey, MetadataValue)
        });

    // Populate the request body information and the initial cached review information.
    for (int i = 0; i < ImageUrls.Length; i++)
    {
        // Cache the local information with which to create the review.
        var itemInfo = new ReviewItem()
        {
            Type = MediaType,
            ContentId = i.ToString(),
            Url = ImageUrls[i],
            ReviewId = null
        };

        WriteLine($" - {itemInfo.Url}; with id = {itemInfo.ContentId}.", true);

        // Add the item informaton to the request information.
        requestInfo.Add(new CreateReviewBodyItem(
            itemInfo.Type, itemInfo.Url, itemInfo.ContentId,
            CallbackEndpoint, metadata));

        // Cache the review creation information.
        reviewItems.Add(itemInfo);
    }

    var reviewResponse = client.Reviews.CreateReviewsWithHttpMessagesAsync(
        "application/json", TeamName, requestInfo);

    // Update the local cache to associate the created review IDs with
    // the associated content.
    var reviewIds = reviewResponse.Result.Body;
    for (int i = 0; i < reviewIds.Count; i++)
    {
        Program.reviewItems[i].ReviewId = reviewIds[i];
    }

    WriteLine(JsonConvert.SerializeObject(
    reviewIds, Formatting.Indented));

    Thread.Sleep(throttleRate);
}
```

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Mevcut incelemelerde durum almak için bir yöntem oluşturma

**Program** sınıfına aşağıdaki yöntemi ekleyin.

> [!Note]
> Uygulamada, geri çağırma URL 'sini `CallbackEndpoint` ' ı el ile incelemenin sonuçlarını alacak URL 'ye ayarlarsınız (bir HTTP POST isteği aracılığıyla). Bekleyen incelemelerinin durumunu denetlemek için bu yöntemi değiştirebilirsiniz.

```csharp
/// <summary>
    /// Gets the review details from the server.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void GetReviewDetails(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Getting review details:", true);
        foreach (var item in reviewItems)
        {
            var reviewDetail = client.Reviews.GetReviewWithHttpMessagesAsync(
                TeamName, item.ReviewId);

            WriteLine(
                $"Review {item.ReviewId} for item ID {item.ContentId} is " +
                $"{reviewDetail.Result.Body.Status}.", true);
            WriteLine(JsonConvert.SerializeObject(
                reviewDetail.Result.Body, Formatting.Indented));

            Thread.Sleep(throttleRate);
        }
    }
```

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>İnceleme kümesi oluşturmak ve durumunu denetlemek için kod ekleyin

Aşağıdaki kodu **Main** yöntemine ekleyin.

Bu kod, listeyi tanımlama ve yönetme konusunda gerçekleştirdiğiniz birçok işlemin yanı sıra görüntüleri ekran görüntülerini da kullanarak benzetir. Günlüğe kaydetme özellikleri, Içerik Mmoderatör hizmetine SDK çağrıları tarafından oluşturulan yanıt nesnelerini görmenizi sağlar.

```csharp
using (TextWriter outputWriter = new StreamWriter(OutputFile, false))
{
    writer = outputWriter;
    using (var client = Clients.NewClient())
    {
        CreateReviews(client);
        GetReviewDetails(client);

        Console.WriteLine();
        Console.WriteLine("Perform manual reviews on the Content Moderator site.");
        Console.WriteLine("Then, press any key to continue.");
        Console.ReadKey();

        Console.WriteLine();
        Console.WriteLine($"Waiting {latencyDelay} seconds for results to propigate.");
        Thread.Sleep(latencyDelay * 1000);

        GetReviewDetails(client);
    }

    writer = null;
    outputWriter.Flush();
    outputWriter.Close();
}

Console.WriteLine();
Console.WriteLine("Press any key to exit...");
Console.ReadKey();
```

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırın ve çıktıyı gözden geçirin

Aşağıdaki örnek çıktıyı görürsünüz:

```console
Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
```

**Sc** etiketinin **doğru**olarak ayarlandığı bekleyen resim incelemesini görmek için Content moderator İnceleme aracında oturum açın. Ayrıca, gözden geçirme aracında tanımladığınız varsayılan **a** ve **r** etiketlerini ve özel etiketleri de görürsünüz.

Göndermek için **İleri** düğmesini kullanın.

![İnsan moderatör için görüntü incelemesi](images/moderation-reviews-quickstart-dotnet.PNG)

Sonra, devam etmek için herhangi bir tuşa basın.

```console
Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...
```

## <a name="check-out-the-following-output-in-the-log-file"></a>Günlük dosyasında aşağıdaki çıktıyı inceleyin.

> [!NOTE]
> Çıkış dosyanızda, "\{teamname}" ve "\{callbackUrl}" dizeleri sırasıyla `TeamName` ve `CallbackEndpoint` alanları için değerleri yansıtır.

İnceleme kimlikleri ve görüntü içeriği URL 'Leri, uygulamayı her çalıştırdığınızda farklı olur ve bir gözden geçirme tamamlandığında `reviewerResultTags` alanı gözden geçirenin öğeyi nasıl etiketlemesinin olduğunu yansıtır.

```json
Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.
    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Pending",
        "reviewerResultTags": [],
        "createdBy": "{teamname}",
        "metadata": [
        {
              "key": "sc",
              "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
              "key": "a",
              "value": "False"
        },
        {
              "key": "r",
              "value": "True"
        },
        {
              "key": "sc",
              "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
              "key": "sc",
              "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }
```

## <a name="your-callback-url-if-provided-receives-this-response"></a>Sağlanmışsa geri çağırma URL 'niz bu yanıtı alır

Aşağıdaki örnekte olduğu gibi bir yanıt görürsünüz:

```json
{
    "ReviewId": "201801i48a2937e679a41c7966e838c92f5e649",
    "ModifiedOn": "2018-01-06T05:04:40.5525865Z",
    "ModifiedBy": "yourusername",
    "CallBackType": "Review",
    "ContentId": "0",
    "ContentType": "Image",
    "Metadata": {
        "sc": "true"
        },
    "ReviewerResultTags": {
        "a": "False",
        "r": "False",
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

[.NET SDK Content moderator](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) alın ve bu ve diğer Content moderator hızlı başlangıç Için [Visual Studio çözümünü](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) indirin ve tümleştirmenizi kullanmaya başlayın.