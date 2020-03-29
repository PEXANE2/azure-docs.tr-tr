---
title: .NET kullanarak video incelemeleri oluşturma - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Bu makale, video incelemeleri oluşturmak için C# ile İçerik Moderatör SDK'yı kullanmaya hızla başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: pafarley
ms.openlocfilehash: 7130ed43183d64b00f8f5ef1697b9a3b456ad396
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72931668"
---
# <a name="create-video-reviews-using-net"></a>.NET'i kullanarak video incelemeleri oluşturma

Bu makalede, [C# ile İçerik Moderatör SDK'yı](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) kullanmaya hızla başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlanmaktadır:

- İnsan moderatörler için video incelemesi oluşturma
- Gözden geçirmeye çerçeve ekleme
- İnceleme için çerçeveleri alın
- İncelemenin durumunu ve ayrıntılarını alın
- İncelemeyi yayımla

## <a name="prerequisites"></a>Ön koşullar

- İçerik Moderatör [İnceleme araç](https://contentmoderator.cognitive.microsoft.com/) sitesinde oturum açın veya bir hesap oluşturun.
- Bu makalede, videoyu [denetlediğinizi varsayar (bkz. hızlı başlangıç)](video-moderation-api.md) ve yanıt verilerine sahipsiniz. İnsan moderatörler için çerçeve tabanlı incelemeler oluşturmak için ihtiyacınız var.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>API anahtarınızın inceleme oluşturma amacıyla inceleme API'sini çağırabildiğinden emin olun

Önceki adımları tamamladıktan sonra, başlangıcı Azure portaldan yaptıysanız şu anda iki Content Moderator anahtarınız olmalıdır.

SDK örneğinizde Azure tarafından sağlanan API anahtarını kullanmayı planlıyorsanız, uygulamanızın inceleme API’sini çağırmasına ve incelemeler oluşturmasına izin vermek için [inceleme API'siyle Azure anahtarını kullanma](review-tool-user-guide/configure.md#use-your-azure-account-with-the-review-apis) bölümünde anlatılan adımları izleyin.

İnceleme aracı tarafından oluşturulan ücretsiz deneme anahtarını kullanırsanız, inceleme aracı hesabınız anahtarı zaten tanıyordur ve bu nedenle ek bir adım gerekli değildir.

### <a name="prepare-your-video-and-the-video-frames-for-review"></a>Videonuzu ve video çerçevelerinizi incelemeye hazırlayın

İncelenecek video ve örnek video çerçeveleri, URL'lerine ihtiyacınız olduğundan çevrimiçi olarak yayınlanmalıdır.

> [!NOTE]
> Program, inceleme API'sinin kullanımını göstermek için rastgele yetişkinlere uygun/müstehcen puanlarla videodan el ile kaydedilmiş ekran görüntülerini kullanır. Gerçek dünyada, görüntü oluşturmak ve puanlar atamak için [video denetleme çıktısını](video-moderation-api.md#run-the-program-and-review-the-output) kullanırsınız. 

Video için, inceleme aracının videoyu oynatıcı görünümünde oynatıcı görünümünde oynatıcı da oynatabilmesi için bir akış bitiş noktasına ihtiyacınız vardır.

![Video demo küçük resim](images/ams-video-demo-view.PNG)

- Bildirim URL'si için bu [Azure Medya Hizmetleri demo](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) sayfasındaki **URL'yi** kopyalayın.

Video çerçeveleri (görüntüler) için aşağıdaki görüntüleri kullanın:

![Video kare küçük resim 1](images/ams-video-frame-thumbnails-1.PNG) | ![Video kare küçük resim 2](images/ams-video-frame-thumbnails-2.PNG) | ![Video kare küçük resim 3](images/ams-video-frame-thumbnails-3.PNG) |
| :---: | :---: | :---: |
[Çerçeve 1](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG) | [Çerçeve 2](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG) | [Çerçeve 3](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG) |

## <a name="create-your-visual-studio-project"></a>Visual Studio projenizi oluşturun

1. Çözümünüze yeni bir **Konsol uygulaması (.NET Framework)** projesi ekleyin.

1. Proje **VideoReviews**adı .

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

Namespace **VideoReviews,** sınıf **Programı**için aşağıdaki özel özellikleri ekleyin. Bitiş `AzureEndpoint` noktası `CMSubscriptionKey` URL'niz ve abonelik anahtarınızın değerleriyle alanları ve alanları güncelleştirin. Bunları Azure portalında kaynağınızın **Hızlı başlangıç** sekmesinde bulabilirsiniz.


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

Namespace **VideoReviews**, sınıf **Programı**için aşağıdaki yöntem tanımı ekleyin.

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
> Content Moderator hizmet anahtarınızın saniye başına istek (RPS) hız limiti vardır ve sınırı aşarsanız SDK 429 hata kodu ile bir özel durum oluşturur.
>
> Ücretsiz katmanı anahtarı bir RPS’lik hız sınırına sahiptir.

## <a name="add-video-frames-to-the-video-review"></a>Video incelemesine video çerçeveleri ekleme

**ContentModeratorClient.Reviews.AddVideoFrameUrl** (video çerçeveleriniz çevrimiçi barındırılıyorsa) veya **ContentModeratorClient.Reviews.AddVideoFrameStream** (video çerçeveleriniz yerel olarak barındırılıyorsa) ile bir video incelemesine video çerçeveleri eklersiniz. Bu hızlı başlatma, video karelerinizin çevrimiçi olarak barındırılan bir yer olduğunu varsayar ve bu nedenle **AddVideoFrameUrl'yi**kullanır. Daha fazla bilgi için bkz. [API başvurusu](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd).

**AddVideoFrameUrl** aşağıdaki gerekli parametrelere sahiptir:
1. MIME türü içeren ve "uygulama/json" olması gereken bir dize.
1. İçerik Moderatör takım adınız.
1. Video inceleme kimliği **CreateVideoReviews**tarafından döndürülür.
1. **Bir IList\<VideoFrameBodyItem>** nesne. Her **VideoFrameBodyItem** nesnesi bir video çerçeveyi temsil eder.

**VideoFrameBodyItem** aşağıdaki özelliklere sahiptir:
- **Zaman damgası.** Saniyeler içinde video çerçevesinin çekildiği videoyu içeren bir dize.
- **FrameImage**. Video çerçevesinin URL'si.
- **Meta veriler**. Bir IList\<VideoFrameBodyItemMetadataItem>. **VideoFrameBodyItemMetadataItem** sadece bir anahtar/değer çiftidir. Geçerli anahtarlar şunlardır:
- **yorumTavsiye edilir**. Video çerçevesinin insan tarafından incelenmesi önerilirse doğrudur.
- **adultScore**. Video çerçevesindeki yetişkinlere uygun içeriğin önem derecesini belirten 0'dan 1'e kadar bir değer.
- **bir**. Video yetişkinlere uygun içerik içeriyorsa doğrudur.
- **racyScore**. Video çerçevesindeki müstehcen içeriğin şiddetini belirten 0'dan 1'e kadar olan bir değer.
- **r**. Video çerçevesi müstehcen içerik içeriyorsa doğrudur.
- **İncelemeSonucuEtiketler**. Bir IList\<VideoFrameBodyItemReviewerResultTagsItem>. **VideoFrameBodyItemReviewerResultTagsItem** sadece bir anahtar / değer çifti. Bir uygulama video çerçeveleri düzenlemek için bu etiketleri kullanabilirsiniz.

> [!NOTE]
> Bu hızlı başlatma **adultScore** ve **racyScore** özellikleri için rasgele değerler oluşturur. Bir üretim uygulamasında, bu değerleri Azure Medya Hizmeti olarak dağıtılan [video denetleme hizmetinden](video-moderation-api.md)elde elabilirsiniz.

Ad alanı VideoReviews, sınıf Programı için aşağıdaki yöntem tanımları ekleyin.

```csharp
<summary>
/// Create a video frame to add to a video review after the video review is created.
/// </summary>
/// <param name="url">The URL of the video frame image.</param>
/// <returns>The video frame.</returns>
private static VideoFrameBodyItem CreateFrameToAddToReview(string url, string timestamp_seconds)
{
    // We generate random "adult" and "racy" scores for the video frame.
    Random rand = new Random();

    var frame = new VideoFrameBodyItem
    {
        // The timestamp is measured in milliseconds. Convert from seconds.
        Timestamp = (int.Parse(timestamp_seconds) * 1000).ToString(),
        FrameImage = url,

        Metadata = new List<VideoFrameBodyItemMetadataItem>
        {
            new VideoFrameBodyItemMetadataItem("reviewRecommended", "true"),
            new VideoFrameBodyItemMetadataItem("adultScore", rand.NextDouble().ToString()),
            new VideoFrameBodyItemMetadataItem("a", "false"),
            new VideoFrameBodyItemMetadataItem("racyScore", rand.NextDouble().ToString()),
            new VideoFrameBodyItemMetadataItem("r", "false")
        },

        ReviewerResultTags = new List<VideoFrameBodyItemReviewerResultTagsItem>()
        {
            new VideoFrameBodyItemReviewerResultTagsItem("tag1", "value1")
        }
    };

    return frame;
}
```

```csharp
/// <summary>
/// Add a video frame to the indicated video review. For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
/// <param name="url">The URL of the video frame image.</param>
static void AddFrame(ContentModeratorClient client, string review_id, string url, string timestamp_seconds)
{
    Console.WriteLine("Adding a frame to the review with ID {0}.", review_id);

    var frames = new List<VideoFrameBodyItem>()
    {
        CreateFrameToAddToReview(url, timestamp_seconds)
    };
        
    client.Reviews.AddVideoFrameUrl("application/json", TeamName, review_id, frames);

    Thread.Sleep(throttleRate);
```

## <a name="get-video-frames-for-video-review"></a>Video incelemesi için video çerçeveleri alın

**ContentModeratorClient.Reviews.GetVideoFrames**ile video incelemesi için video çerçevelerini alabilirsiniz. **GetVideoFrames** aşağıdaki gerekli parametrelere sahiptir:
1. İçerik Moderatör takım adınız.
1. Video inceleme kimliği **CreateVideoReviews**tarafından döndürülür.
1. Almak için ilk video çerçevesinin sıfır tabanlı dizin.
1. Elde etmek için video kare sayısı.

Ad alanı VideoReviews, sınıf Programı için aşağıdaki yöntem tanımı ekleyin.

```csharp
/// <summary>
/// Get the video frames assigned to the indicated video review.  For more information, see the API reference:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7ba43e7151f0b10d45200
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
static void GetFrames(ContentModeratorClient client, string review_id)
{
    Console.WriteLine("Getting frames for the review with ID {0}.", review_id);

    Frames result = client.Reviews.GetVideoFrames(TeamName, review_id, 0);
    Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

    Thread.Sleep(throttleRate);
}
```

## <a name="get-video-review-information"></a>Video inceleme bilgileri alın

**ContentModeratorClient.Reviews.GetReview**ile bir video incelemesi için bilgi almak. **GetReview** aşağıdaki gerekli parametrelere sahiptir:
1. İçerik Moderatör takım adınız.
1. Video inceleme kimliği **CreateVideoReviews**tarafından döndürülür.

Ad alanı VideoReviews, sınıf Programı için aşağıdaki yöntem tanımı ekleyin.

```csharp
/// <summary>
/// Get the information for the indicated video review. For more information, see the reference API:
/// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2
/// </summary>
/// <param name="client">The Content Moderator client.</param>
/// <param name="review_id">The video review ID.</param>
private static void GetReview(ContentModeratorClient client, string review_id)
{
    Console.WriteLine("Getting the status for the review with ID {0}.", review_id);

    var result = client.Reviews.GetReview(ModeratorHelper.Clients.TeamName, review_id);
    Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

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

Ad alanı VideoReviews, sınıf Programı ana **yöntem** tanımı ekleyin. Son olarak, Program sınıfını ve VideoReviews ad alanını kapatın.

```csharp
static void Main(string[] args)
{
    using (ContentModeratorClient client = NewClient())
    {
        // Create a review with the content pointing to a streaming endpoint (manifest)
        var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
        string review_id = CreateReview(client, "review1", streamingcontent);

        var frame1_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG";
        var frame2_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG";
        var frame3_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG";

        // Add the frames from 17, 64, and 144 seconds.
        AddFrame(client, review_id, frame1_url, "17");
        AddFrame(client, review_id, frame2_url, "64");
        AddFrame(client, review_id, frame3_url, "144");

        // Get frames information and show
        GetFrames(client, review_id);
        GetReview(client, review_id);

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

```json
Creating a video review.
Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
Getting frames for the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
{
    "ReviewId": "201801v3212bda70ced4928b2cd7459c290c7dc",
    "VideoFrames": [
    {
        "Timestamp": "17000",
        "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_17000.PNG",
        "Metadata": [
        {
            "Key": "reviewRecommended",
            "Value": "true"
        },
        {
            "Key": "adultScore",
            "Value": "0.808312381528463"
        },
        {
            "Key": "a",
            "Value": "false"
        },
        {
            "Key": "racyScore",
            "Value": "0.846378884206702"
        },
        {
            "Key": "r",
            "Value": "false"
        }
        ],
        "ReviewerResultTags": [
        {
            "Key": "tag1",
            "Value": "value1"
        }
    ]
    },
    {
        "Timestamp": "64000",
        "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_64000.PNG",
        "Metadata": [
        {
            "Key": "reviewRecommended",
            "Value": "true"
        },
        {
            "Key": "adultScore",
            "Value": "0.576078300166912"
        },
        {
            "Key": "a",
            "Value": "false"
        },
        {
            "Key": "racyScore",
            "Value": "0.244768953064815"
        },
        {
            "Key": "r",
            "Value": "false"
        }
        ],
        "ReviewerResultTags": [
        {
            "Key": "tag1",
            "Value": "value1"
        }
    ]
    },
    {
        "Timestamp": "144000",
        "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_144000.PNG",
        "Metadata": [
        {
            "Key": "reviewRecommended",
            "Value": "true"
        },
        {
            "Key": "adultScore",
            "Value": "0.664480847150311"
        },
        {
            "Key": "a",
            "Value": "false"
        },
        {
            "Key": "racyScore",
            "Value": "0.933817870418456"
        },
        {
            "Key": "r",
            "Value": "false"
        }
        ],
        "ReviewerResultTags": [
        {
            "Key": "tag1",
            "Value": "value1"
        }
        ]
    }
    ]
}

Getting the status for the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
{
    "ReviewId": "201801v3212bda70ced4928b2cd7459c290c7dc",
    "SubTeam": "public",
    "Status": "UnPublished",
    "ReviewerResultTags": [],
    "CreatedBy": "testreview6",
    "Metadata": [
    {
        "Key": "FrameCount",
        "Value": "3"
    }
    ],
    "Type": "Video",
    "Content": "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
    "ContentId": "review1",
    "CallbackEndpoint": null
}

Publishing the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
Open your Content Moderator Dashboard and select Review > Video to see the review.
Press any key to close the application.
```

## <a name="check-out-your-video-review"></a>Video incelemenize göz atın

Son olarak, **Video**>İnceleme**ekranında** İçerik Moderatörü inceleme aracı hesabınızda video incelemesini görürsünüz.

![İnsan moderatörler için video incelemesi](images/ams-video-review.PNG)

## <a name="next-steps"></a>Sonraki adımlar

İçerik [Moderatör .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) ve [Visual Studio çözüm](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) bu ve diğer İçerik Moderatör hızlı .NET için başlar alın.

Video incelemesine [transkript moderasyonu](video-transcript-moderation-review-tutorial-dotnet.md) nasıl ekleyeceğinizi öğrenin. 

Tam bir [video ılımlılık çözümü](video-transcript-moderation-review-tutorial-dotnet.md)geliştirmek için nasıl ayrıntılı öğretici göz atın.
