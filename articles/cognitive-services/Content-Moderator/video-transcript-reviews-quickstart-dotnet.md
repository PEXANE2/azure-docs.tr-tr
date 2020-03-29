---
title: .NET - İçerik Moderatörünü kullanarak video transkript incelemeleri oluşturma
titleSuffix: Azure Cognitive Services
description: .NET için Azure Bilişsel Hizmetler İçerik ModeratörÜ SDK'yı kullanarak video transkript incelemeleri oluşturmayı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: pafarley
ms.openlocfilehash: b2d763454b86570b57a16fb9ae2107a2a2bcd23d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73744383"
---
# <a name="create-video-transcript-reviews-using-net"></a>.NET'i kullanarak video transkript incelemeleri oluşturma

Bu makalede, [C# ile İçerik Moderatör SDK'yı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) kullanmaya hızla başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanmaktadır:

- İnsan moderatörler için video incelemesi oluşturma
- İncelemeye denetlenmiş bir transkript ekleme
- İncelemeyi yayımla

## <a name="prerequisites"></a>Ön koşullar

- Daha önce yapmadıysanız İçerik Moderatör [İnceleme araç](https://contentmoderator.cognitive.microsoft.com/) sitesinde oturum açın veya bir hesap oluşturun.
- Bu makalede, video [nun moderatörlüğünü](video-moderation-api.md) yaptığınızı ve insan karar verme için inceleme aracında [video incelemesini oluşturduğunuzu](video-reviews-quickstart-dotnet.md) varsayar. Şimdi inceleme aracına denetitilen video transkriptleri eklemek istiyorsunuz.

## <a name="ensure-your-api-key-can-call-the-review-api-job-creation"></a>API anahtarınızın inceleme API'sini (İş oluşturma) çağırabilmesini sağlayın

Önceki adımları tamamladıktan sonra, başlangıcı Azure portaldan yaptıysanız şu anda iki Content Moderator anahtarınız olmalıdır.

SDK örneğinizde Azure tarafından sağlanan API anahtarını kullanmayı planlıyorsanız, uygulamanızın inceleme API’sini çağırmasına ve incelemeler oluşturmasına izin vermek için [inceleme API'siyle Azure anahtarını kullanma](./review-tool-user-guide/configure.md#use-your-azure-account-with-the-review-apis) bölümünde anlatılan adımları izleyin.

İnceleme aracı tarafından oluşturulan ücretsiz deneme anahtarını kullanırsanız, inceleme aracı hesabınız anahtarı zaten tanıyordur ve bu nedenle ek bir adım gerekli değildir.

## <a name="prepare-your-video-for-review"></a>Videonuzu incelemeye hazırlayın

Transkripti video incelemesine ekleyin. Video çevrimiçi olarak yayınlanmalıdır. Akış bitiş noktasına ihtiyacınız var. Akış bitiş noktası, inceleme aracı video oynatıcısının videoyu oynatmasını sağlar.

![Video demo küçük resim](images/ams-video-demo-view.PNG)

- Bildirim URL'si için bu [Azure Medya Hizmetleri demo](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) sayfasındaki **URL'yi** kopyalayın.

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturun

1. Çözümünüze yeni bir **Konsol uygulaması (.NET Framework)** projesi ekleyin.

1. Proje **VideoTranscriptReviews**adı .

1. Bu projeyi çözümün tekil başlangıç projesi olarak seçin.

### <a name="install-required-packages"></a>Gerekli paketleri yükleme

TermLists projesi için aşağıdaki NuGet paketlerini yükleyin.

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Programı deyimler kullanarak güncelleştirme

Programın aşağıdaki ifadeleri kullanarak değiştirin.


```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.Azure.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
```

### <a name="add-private-properties"></a>Özel özellikler ekleme

Namespace **VideoTranscriptReviews,** sınıf **Programı**için aşağıdaki özel özellikleri ekleyin. Bitiş `AzureEndpoint` noktası `CMSubscriptionKey` URL'niz ve abonelik anahtarınızın değerleriyle alanları ve alanları güncelleştirin. Bunları Azure portalında kaynağınızın **Hızlı başlangıç** sekmesinde bulabilirsiniz.

```csharp
namespace VideoReviews
{
    class Program
    {
        // NOTE: Enter a valid endpoint URL
        /// <summary>
        /// The endpoint URL of your subscription
        /// </summary>
        private static readonly string AzureEndpoint = "YOUR ENDPOINT URL";

        // NOTE: Enter a valid subscription key.
        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR CONTENT MODERATOR KEY";

        // NOTE: Replace this example team name with your Content Moderator team name.
        /// <summary>
        /// The name of the team to assign the job to.
        /// </summary>
        /// <remarks>This must be the team name you used to create your 
        /// Content Moderator account. You can retrieve your team name from
        /// the Content Moderator web site. Your team name is the Id associated 
        /// with your subscription.</remarks>
        private const string TeamName = "YOUR CONTENT MODERATOR TEAM ID";

        /// <summary>
        /// The minimum amount of time, in milliseconds, to wait between calls
        /// to the Content Moderator APIs.
        /// </summary>
        private const int throttleRate = 2000;
```

### <a name="create-content-moderator-client-object"></a>İçerik Moderatör Üden nesnesi oluşturma

Ad alanı VideoTranscriptReviews, sınıf Programı için aşağıdaki yöntem tanımı ekleyin.

```csharp
/// <summary>
/// Returns a new Content Moderator client for your subscription.
/// </summary>
/// <returns>The new client.</returns>
/// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
/// When you have finished using the client,
/// you should dispose of it either directly or indirectly. </remarks>
public static ContentModeratorClient NewClient()
{
    return new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey))
    {
        Endpoint = AzureEndpoint
    };
}
```

## <a name="create-a-video-review"></a>Video incelemesi oluşturma

**ContentModeratorClient.Reviews.CreateVideoReviews**ile bir video incelemesi oluşturun. Daha fazla bilgi için bkz. [API başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

**CreateVideoReviews** aşağıdaki gerekli parametrelere sahiptir:
1. MIME türü içeren ve "uygulama/json" olması gereken bir dize. 
1. İçerik Moderatör takım adınız.
1. Bir **IList\<CreateVideoReviewsBodyItem>** nesne. Her **CreateVideoReviewsBodyItem** nesnesi bir video incelemesini temsil eder. Bu hızlı başlatma, aynı anda bir gözden geçirme oluşturur.

**CreateVideoReviewsBodyItem'in** çeşitli özellikleri vardır. En azından aşağıdaki özellikleri ayarlarsınız:
- **İçerik**. Gözden geçirilecek videonun URL'si.
- **ContentId**. Video incelemesine atamak için bir kimlik.
- **Durum**. Değeri "Yayımlanmamış" olarak ayarlayın. Ayarlamazsanız, varsayılan olarak "Beklemede", bu da video incelemesinin yayınlandığı ve insan incelemesinin beklemede olduğu anlamına gelir. Bir video incelemesi yayınlandıktan sonra, artık video çerçeveleri, bir transkript veya transkript ılımlılık sonucu ekleyebilirsiniz.

> [!NOTE]
> **CreateVideoReviews** bir IList\<dize> döndürür. Bu dizelerin her biri bir video incelemesi için bir kimlik içerir. Bu kimlikler GUID'dir ve **ContentId** özelliğinin değeriyle aynı değildir.

Ad alanı VideoReviews, sınıf Programı için aşağıdaki yöntem tanımı ekleyin.

```csharp
/// <summary>
/// Create a video review. For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="id">The ID to assign to the video review.</param>
/// <param name="content">The URL of the video to review.</param>
/// <returns>The ID of the video review.</returns>
private static string CreateReview(ContentModeratorClient client, string id, string content)
{
    Console.WriteLine("Creating a video review.");

    List<CreateVideoReviewsBodyItem> body = new List<CreateVideoReviewsBodyItem>() {
        new CreateVideoReviewsBodyItem
        {
            Content = content,
            ContentId = id,
            /* Note: to create a published review, set the Status to "Pending".
            However, you cannot add video frames or a transcript to a published review. */
            Status = "Unpublished",
        }
    };

    var result = client.Reviews.CreateVideoReviews("application/json", TeamName, body);

    Thread.Sleep(throttleRate);

    // We created only one review.
    return result[0];
}
```

> [!NOTE]
> Content Moderator hizmet anahtarınızın saniyede istek sayısı (RPS) hız sınırı vardır. Sınırı aşarsanız, SDK 429 hata koduyla bir özel durum oluşturulur.
>
> Ücretsiz katmanı anahtarı bir RPS’lik hız sınırına sahiptir.

## <a name="add-transcript-to-video-review"></a>Video incelemesine transkript ekleme

**ContentModeratorClient.Reviews.AddVideoTranscript**ile bir video incelemesine bir transkript eklersiniz. **AddVideoTranskript** aşağıdaki gerekli parametrelere sahiptir:
1. İçerik Moderatör takım kimliğiniz.
1. Video inceleme kimliği **CreateVideoReviews**tarafından döndürülür.
1. Transkriptiçeren bir **Akış** nesnesi.

Transkript WebVTT biçiminde olmalıdır. Daha fazla bilgi için [Bkz. WebVTT: Web Video Metin İzleri Biçimi.](https://www.w3.org/TR/webvtt1/)

> [!NOTE]
> Program VTT formatında bir örnek transkript kullanır. Gerçek bir çözümde, bir videodan [bir transkript oluşturmak](https://docs.microsoft.com/azure/media-services/media-services-index-content) için Azure Media Indexer hizmetini kullanırsınız.

Ad alanı VideotranscriptReviews, sınıf Programı için aşağıdaki yöntem tanımı ekleyin.

```csharp
/// <summary>
/// Add a transcript to the indicated video review.
/// The transcript must be in the WebVTT format.
/// For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b8b2e7151f0b10d451fe
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
/// <param name="transcript">The video transcript.</param>
static void AddTranscript(ContentModeratorClient client, string review_id, string transcript)
{
    Console.WriteLine("Adding a transcript to the review with ID {0}.", review_id);
    client.Reviews.AddVideoTranscript(TeamName, review_id, new MemoryStream(System.Text.Encoding.UTF8.GetBytes(transcript)));
    Thread.Sleep(throttleRate);
}
```

## <a name="add-a-transcript-moderation-result-to-video-review"></a>Video incelemesine transkript denetleme sonucu ekleme

Bir video incelemesine transkript eklemenin yanı sıra, bu transkriptin moderating sonucunu da eklersiniz. Bunu **ContentModeratorClient.Reviews.AddVideoTranscriptModerationResult**ile yaparsınız. Daha fazla bilgi için bkz. [API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff).

**AddVideoTranskriptModerationResult** aşağıdaki gerekli parametrelere sahiptir:
1. MIME türü içeren ve "uygulama/json" olması gereken bir dize. 
1. İçerik Moderatör takım adınız.
1. Video inceleme kimliği **CreateVideoReviews**tarafından döndürülür.
1. Bir IList\<TranskriptModerationBodyItem>. **Bir TranskriptModerationBodyItem** aşağıdaki özelliklere sahiptir:
1. **Şartlar**. Bir IList\<TranskriptModerationBodyItemTermsItem>. **Bir TranskriptModerationBodyItemTermsItem** aşağıdaki özelliklere sahiptir:
1. **Dizin**. Terimin sıfır tabanlı indeksi.
1. **Dönem**. Terimi içeren bir dize.
1. **Zaman damgası.** Terimlerin bulunduğu transkriptteki zamanı saniyeler içinde içeren bir dize.

Transkript WebVTT biçiminde olmalıdır. Daha fazla bilgi için [Bkz. WebVTT: Web Video Metin İzleri Biçimi.](https://www.w3.org/TR/webvtt1/)

Ad alanı VideoTranscriptReviews, sınıf Programı için aşağıdaki yöntem tanımı ekleyin. Bu yöntem **ContentModeratorClient.TextModeration.ScreenText** yöntemine bir transkript gönderir. Ayrıca bir IList\<TranskriptModerationBodyItem> içine sonucu çevirir ve **AddVideoTranscriptModerationResult**gönderir.

```csharp
/// <summary>
/// Add the results of moderating a video transcript to the indicated video review.
/// For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
/// <param name="transcript">The video transcript.</param>
static void AddTranscriptModerationResult(ContentModeratorClient client, string review_id, string transcript)
{
    Console.WriteLine("Adding a transcript moderation result to the review with ID {0}.", review_id);

    // Screen the transcript using the Text Moderation API. For more information, see:
    // https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f
    Screen screen = client.TextModeration.ScreenText("eng", "text/plain", transcript);

    // Map the term list returned by ScreenText into a term list we can pass to AddVideoTranscriptModerationResult.
    List<TranscriptModerationBodyItemTermsItem> terms = new List<TranscriptModerationBodyItemTermsItem>();
    if (null != screen.Terms)
    {
        foreach (var term in screen.Terms)
        {
            if (term.Index.HasValue)
            {
                terms.Add(new TranscriptModerationBodyItemTermsItem(term.Index.Value, term.Term));
            }
        }
    }

    List<TranscriptModerationBodyItem> body = new List<TranscriptModerationBodyItem>()
    {
        new TranscriptModerationBodyItem()
        {
            Timestamp = "0",
            Terms = terms
        }
    };

    client.Reviews.AddVideoTranscriptModerationResult("application/json", TeamName, review_id, body);

    Thread.Sleep(throttleRate);
}
```

## <a name="publish-video-review"></a>Video incelemesi yayınlama

**ContentModeratorClient.Reviews.PublishVideoReview**ile bir video incelemesi yayınlamak . **PublishVideoReview** aşağıdaki gerekli parametrelere sahiptir:
1. İçerik Moderatör takım adınız.
1. Video inceleme kimliği **CreateVideoReviews**tarafından döndürülür.

Ad alanı VideoReviews, sınıf Programı için aşağıdaki yöntem tanımı ekleyin.

```csharp
/// <summary>
/// Publish the indicated video review. For more information, see the reference API:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7bb29e7151f0b10d45201
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
private static void PublishReview(ContentModeratorClient client, string review_id)
{
    Console.WriteLine("Publishing the review with ID {0}.", review_id);
    client.Reviews.PublishVideoReview(TeamName, review_id);
    Thread.Sleep(throttleRate);
}
```

## <a name="putting-it-all-together"></a>Hepsini bir araya getirme

Ad alanı VideoTranscriptReviews, sınıf Programı **ana** yöntem tanımı ekleyin. Son olarak, Program sınıfını ve VideoTranskriptReviews ad alanını kapatın.

> [!NOTE]
> Program VTT formatında bir örnek transkript kullanır. Gerçek bir çözümde, bir videodan [bir transkript oluşturmak](https://docs.microsoft.com/azure/media-services/media-services-index-content) için Azure Media Indexer hizmetini kullanırsınız.

```csharp
static void Main(string[] args)
{
    using (ContentModeratorClient client = NewClient())
    {
        // Create a review with the content pointing to a streaming endpoint (manifest)
        var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
        string review_id = CreateReview(client, "review1", streamingcontent);

        var transcript = @"WEBVTT

        01:01.000 --> 02:02.000
        First line with a negative word in a transcript.

        02:03.000 --> 02:25.000
        This is another line in the transcript.
        ";

        AddTranscript(client, review_id, transcript);

        AddTranscriptModerationResult(client, review_id, transcript);

        // Publish the review
        PublishReview(client, review_id);

        Console.WriteLine("Open your Content Moderator Dashboard and select Review > Video to see the review.");
        Console.WriteLine("Press any key to close the application.");
        Console.ReadKey();
    }
}
```

## <a name="run-the-program-and-review-the-output"></a>Programı çalıştırma ve çıktıyı gözden geçirme

Uygulamayı çalıştırdığınızda, aşağıdaki satırlarda bir çıktı görürsünüz:

```console
Creating a video review.
Adding a transcript to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Adding a transcript moderation result to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Publishing the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
Open your Content Moderator Dashboard and select Review > Video to see the review.
Press any key to close the application.
```

## <a name="navigate-to-your-video-transcript-review"></a>Video transkript incelemenize gidin

**Videoyu İncele**>**Video**>**ekranındaki** İçerik Moderatörü inceleme aracınızdaki video transkript incelemesine gidin.

Aşağıdaki özellikleri görürsünüz:
- Eklediğiniz iki transkript satırı
- Metin moderasyon hizmeti tarafından bulunan ve vurgulanan küfür terimi
- Transkripsiyon metnini seçmek videoyu o zaman damgasından başlatır

![İnsan moderatörler için video transkript incelemesi](images/ams-video-transcript-review.PNG)

## <a name="next-steps"></a>Sonraki adımlar

İçerik [Moderatör .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözüm](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer İçerik Moderatör hızlı .NET için başlar alın.

İnceleme aracında [video incelemelerini](video-reviews-quickstart-dotnet.md) nasıl oluşturacağınızı öğrenin.

Tam bir [video ılımlılık çözümü](video-transcript-moderation-review-tutorial-dotnet.md)geliştirmek için nasıl ayrıntılı öğretici göz atın.
