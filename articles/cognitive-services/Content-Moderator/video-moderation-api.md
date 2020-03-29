---
title: C# içinde sakıncalı materyaller için video içeriğini analiz edin - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: .NET için İçerik Moderatör SDK kullanarak çeşitli sakıncalı malzeme için video içeriği analiz etmek için nasıl
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 71858755fe31823d4d7ef8623b915db851530116
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72755230"
---
# <a name="analyze-video-content-for-objectionable-material-in-c"></a>C'deki sakıncalı materyaller için video içeriğini analiz edin #

Bu makale, yetişkinlere uygun veya müstehcen içerik için video içeriğini taramaya [.NET için İçerik Moderatör SDK'sını](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) kullanmaya başlamanıza yardımcı olacak bilgi ve kod örnekleri sağlar.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/) herhangi bir sürümü

## <a name="set-up-azure-resources"></a>Azure kaynakları ayarlama

İçerik Moderatör'ün video moderasyon özelliği, Azure Medya Hizmetleri'nde (AMS) ücretsiz bir genel önizleme **medya işlemcisi** olarak kullanılabilir. Azure Medya Hizmetleri, video içeriğini depolamak ve aktarmak için özel bir Azure hizmetidir. 

### <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

AMS'ye abone olmak ve ilişkili bir Azure depolama hesabı oluşturmak için [Azure Medya Hizmetleri hesabı oluştur'daki](https://docs.microsoft.com/azure/media-services/media-services-portal-create-account) yönergeleri izleyin. Bu depolama hesabında yeni bir Blob depolama kapsayıcısı oluşturun.

### <a name="create-an-azure-active-directory-application"></a>Azure Etkin Dizin uygulaması oluşturma

Azure portalındaki yeni AMS aboneliğinize gidin ve yan menüden **API erişimini** seçin. **Hizmet sorumlusuyla Azure Medya Hizmetlerine Bağlan'ı**seçin. **REST API uç nokta** alanındaki değeri not edin; Buna daha sonra ihtiyacın olacak.

Azure **AD uygulaması** bölümünde **Yeni Oluştur'u** seçin ve yeni Azure AD uygulama kaydınızı adlandırın (örneğin, "VideoModADApp"). **Kaydet'i** tıklatın ve uygulama yapılandırılırken birkaç dakika bekleyin. Ardından, yeni uygulama kaydınızı sayfanın **Azure AD uygulaması** bölümünde görmeniz gerekir.

Uygulama kaydınızı seçin ve altındaki **uygulamayı yönet** düğmesine tıklayın. **Uygulama Kimliği** alanındaki değeri not edin; Buna daha sonra ihtiyacın olacak. **Ayarlar** > **Tuşlarını**seçin ve yeni bir anahtar için açıklama girin ("VideoModKey" gibi). **Kaydet'i**tıklatın ve ardından yeni anahtar değerini fark edin. Bu dizekopyalayın ve güvenli bir yere kaydedin.

Yukarıdaki işlemin daha ayrıntılı bir şekilde gözden geçirilmesi için Bkz. [Azure AD kimlik doğrulaması ile başlayın.](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad)

Bunu yaptıktan sonra, video moderasyonlu ortam işlemcisini iki farklı şekilde kullanabilirsiniz.

## <a name="use-azure-media-services-explorer"></a>Azure Medya Hizmetleri Gezgini'ni kullanma

Azure Media Services Explorer, AMS için kullanıcı dostu bir ön yüzdür. AMS hesabınıza göz atmak, video yüklemek ve İçerik Moderator medya işlemcisiyle içeriği taramak için kullanın. [GitHub'dan](https://github.com/Azure/Azure-Media-Services-Explorer/releases)indirin ve yükleyin veya daha fazla bilgi için [Azure Media Services Explorer blog gönderisini](https://azure.microsoft.com/blog/managing-media-workflows-with-the-new-azure-media-services-explorer-tool/) görün.

![İçerik Moderatörü ile Azure Medya Hizmetleri gezgini](images/ams-explorer-content-moderator.PNG)

## <a name="create-the-visual-studio-project"></a>Visual Studio projesini oluşturma

1. Visual Studio'da, yeni bir **Konsol uygulaması (.NET Framework)** projesi oluşturun ve **videoModeration**adını alın. 
1. Çözümünüzde başka projeler de varsa, tek başlangıç projesi olarak bunu seçin.
1. Gereken NuGet paketlerini alın. Çözüm Gezgini'nde projenize sağ tıklayın ve **NuGet Paketlerini Yönet**'i seçin; ardından aşağıdaki projeleri bulun ve yükleyin:
    - windowsazure.mediaservices
    - windowsazure.mediaservices.extensions

## <a name="add-video-moderation-code"></a>Video moderasyon kodu ekleme

Ardından, temel bir içerik moderasyonu senaryosu uygulamak için kodu bu kılavuzdan kopyalayıp projenize yapıştıracaksınız.

### <a name="update-the-programs-using-statements"></a>Programı deyimler kullanarak güncelleştirme

Aşağıdaki `using` deyimlerini _Program.cs_ dosyanızın en üstüne ekleyin.

```csharp
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.IO;
using System.Threading;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using System.Collections.Generic;
```

### <a name="set-up-resource-references"></a>Kaynak başvuruları ayarlama

_Program.cs'da_ **Program** sınıfına aşağıdaki statik alanları ekleyin. Bu alanlar, AMS aboneliğinize bağlanmak için gerekli bilgileri tutar. Yukarıdaki adımlarda sahip olduğunuz değerlerle doldurun. Azure AD uygulamanızın **Uygulama Kimliği** değeri olduğunu `CLIENT_SECRET` ve bu uygulama için oluşturduğunuz "VideoModKey"in değeri olduğunu unutmayın. `CLIENT_ID`

```csharp
// declare constants and globals
private static CloudMediaContext _context = null;
private static CloudStorageAccount _StorageAccount = null;

// Azure Media Services (AMS) associated Storage Account, Key, and the Container that has 
// a list of Blobs to be processed.
static string STORAGE_NAME = "YOUR AMS ASSOCIATED BLOB STORAGE NAME";
static string STORAGE_KEY = "YOUR AMS ASSOCIATED BLOB STORAGE KEY";
static string STORAGE_CONTAINER_NAME = "YOUR BLOB CONTAINER FOR VIDEO FILES";

private static StorageCredentials _StorageCredentials = null;

// Azure Media Services authentication. 
private const string AZURE_AD_TENANT_NAME = "microsoft.onmicrosoft.com";
private const string CLIENT_ID = "YOUR CLIENT ID";
private const string CLIENT_SECRET = "YOUR CLIENT SECRET";

// REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
private const string REST_API_ENDPOINT = "YOUR API ENDPOINT";

// Content Moderator Media Processor Nam
private const string MEDIA_PROCESSOR = "Azure Media Content Moderator";

// Input and Output files in the current directory of the executable
private const string INPUT_FILE = "VIDEO FILE NAME";
private const string OUTPUT_FOLDER = "";

// JSON settings file
private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";

```

Yerel bir video dosyası (en basit durum) kullanmak istiyorsanız, projeye ekleyin `INPUT_FILE` ve değerini (göreli yollar yürütme dizinine göredir) olarak yolunu girin.

Ayrıca geçerli dizinde _preset.json_ dosyasını oluşturmanız ve bir sürüm numarası belirtmek için kullanmanız gerekir. Örnek:

```JSON
{
    "version": "2.0"
}
```

### <a name="load-the-input-videos"></a>Giriş videosunu(lar) yükleyin

**Program** sınıfının **ana** yöntemi, bir Azure Ortam Bağlamı ve ardından Bir Azure Depolama Bağlamı oluşturur (videolarınızın blob depolama alanında olması durumunda). Kalan kod, Azure depolama kapsayıcısı içindeki yerel bir klasörden, blob'dan veya birden çok blob'dan bir videoyu tarar. Diğer kod satırlarını yorumlayarak tüm seçenekleri deneyebilirsiniz.

```csharp
// Create Azure Media Context
CreateMediaContext();

// Create Storage Context
CreateStorageContext();

// Use a file as the input.
IAsset asset = CreateAssetfromFile();

// -- OR ---

// Or a blob as the input
// IAsset asset = CreateAssetfromBlob((CloudBlockBlob)GetBlobsList().First());

// Then submit the asset to Content Moderator
RunContentModeratorJob(asset);

//-- OR ----

// Just run the content moderator on all blobs in a list (from a Blob Container)
// RunContentModeratorJobOnBlobs();
```

### <a name="create-an-azure-media-context"></a>Azure Ortam Bağlamı Oluşturma

**Program** sınıfına aşağıdaki yöntemi ekleyin. Bu, AMS ile iletişime izin vermek için AMS kimlik bilgilerinizi kullanır.

```csharp
// Creates a media context from azure credentials
static void CreateMediaContext()
{
    // Get Azure AD credentials
    var tokenCredentials = new AzureAdTokenCredentials(AZURE_AD_TENANT_NAME,
        new AzureAdClientSymmetricKey(CLIENT_ID, CLIENT_SECRET),
        AzureEnvironments.AzureCloudEnvironment);

    // Initialize an Azure AD token
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

    // Create a media context
    _context = new CloudMediaContext(new Uri(REST_API_ENDPOINT), tokenProvider);
}
```

### <a name="add-the-code-to-create-an-azure-storage-context"></a>Azure Depolama Bağlamı oluşturmak için kodu ekleme

**Program** sınıfına aşağıdaki yöntemi ekleyin. Blob depolama alanınıza erişmek için depolama kimlik bilgilerinizden oluşturulan Depolama Bağlamını kullanırsınız.

```csharp
// Creates a storage context from the AMS associated storage name and key
static void CreateStorageContext()
{
    // Get a reference to the storage account associated with a Media Services account. 
    if (_StorageCredentials == null)
    {
        _StorageCredentials = new StorageCredentials(STORAGE_NAME, STORAGE_KEY);
    }
    _StorageAccount = new CloudStorageAccount(_StorageCredentials, false);
}
```

### <a name="add-the-code-to-create-azure-media-assets-from-local-file-and-blob"></a>Yerel dosya ve blob'dan Azure Medya Varlıkları oluşturmak için kodu ekleyin

İçerik Moderatörü medya işlemcisi, Azure Medya Hizmetleri platformundaki **Varlıklar'da** işleri yürütür.
Bu yöntemler, varlıkları yerel bir dosyadan veya ilişkili bir blob'dan oluşturur.

```csharp
// Creates an Azure Media Services Asset from the video file
static IAsset CreateAssetfromFile()
{
    return _context.Assets.CreateFromFile(INPUT_FILE, AssetCreationOptions.None); ;
}

// Creates an Azure Media Services asset from your blog storage
static IAsset CreateAssetfromBlob(CloudBlockBlob Blob)
{
    // Create asset from the FIRST blob in the list and return it
    return _context.Assets.CreateFromBlob(Blob, _StorageCredentials, AssetCreationOptions.None);
}
```

### <a name="add-the-code-to-scan-a-collection-of-videos-as-blobs-within-a-container"></a>Bir kapsayıcı içinde video koleksiyonu (blobs olarak) tatmak için kodu ekleyin

```csharp
// Runs the Content Moderator Job on all Blobs in a given container name
static void RunContentModeratorJobOnBlobs()
{
    // Get the reference to the list of Blobs. See the following method.
    var blobList = GetBlobsList();

    // Iterate over the Blob list items or work on specific ones as needed
    foreach (var sourceBlob in blobList)
    {
        // Create an Asset
        IAsset asset = _context.Assets.CreateFromBlob((CloudBlockBlob)sourceBlob,
                            _StorageCredentials, AssetCreationOptions.None);
        asset.Update();

        // Submit to Content Moderator
        RunContentModeratorJob(asset);
    }
}

// Get all blobs in your container
static IEnumerable<IListBlobItem> GetBlobsList()
{
    // Get a reference to the Container within the Storage Account
    // that has the files (blobs) for moderation
    CloudBlobClient CloudBlobClient = _StorageAccount.CreateCloudBlobClient();
    CloudBlobContainer MediaBlobContainer = CloudBlobClient.GetContainerReference(STORAGE_CONTAINER_NAME);

    // Get the reference to the list of Blobs 
    var blobList = MediaBlobContainer.ListBlobs();
    return blobList;
}
```

### <a name="add-the-method-to-run-the-content-moderator-job"></a>İçerik Moderatör İşçalıştırmak için yöntem ekleyin

```csharp
// Run the Content Moderator job on the designated Asset from local file or blob storage
static void RunContentModeratorJob(IAsset asset)
{
    // Grab the presets
    string configuration = File.ReadAllText(CONTENT_MODERATOR_PRESET_FILE);

    // grab instance of Azure Media Content Moderator MP
    IMediaProcessor mp = _context.MediaProcessors.GetLatestMediaProcessorByName(MEDIA_PROCESSOR);

    // create Job with Content Moderator task
    IJob job = _context.Jobs.Create(String.Format("Content Moderator {0}",
            asset.AssetFiles.First() + "_" + Guid.NewGuid()));

    ITask contentModeratorTask = job.Tasks.AddNew("Adult and racy classifier task",
            mp, configuration,
            TaskOptions.None);
    contentModeratorTask.InputAssets.Add(asset);
    contentModeratorTask.OutputAssets.AddNew("Adult and racy classifier output",
        AssetCreationOptions.None);

    job.Submit();


    // Create progress printing and querying tasks
    Task progressPrintTask = new Task(() =>
    {
        IJob jobQuery = null;
        do
        {
            var progressContext = _context;
            jobQuery = progressContext.Jobs
            .Where(j => j.Id == job.Id)
                .First();
                Console.WriteLine(string.Format("{0}\t{1}",
                DateTime.Now,
                jobQuery.State));
                Thread.Sleep(10000);
            }
            while (jobQuery.State != JobState.Finished &&
            jobQuery.State != JobState.Error &&
            jobQuery.State != JobState.Canceled);
    });
    progressPrintTask.Start();

    Task progressJobTask = job.GetExecutionProgressTask(
    CancellationToken.None);
    progressJobTask.Wait();

    // If job state is Error, the event handling 
    // method for job progress should log errors.  Here we check 
    // for error state and exit if needed.
    if (job.State == JobState.Error)
    {
        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
        Console.WriteLine(string.Format("Error: {0}. {1}",
        error.Code,
        error.Message));
    }

    DownloadAsset(job.OutputMediaAssets.First(), OUTPUT_FOLDER);
}
```

### <a name="add-helper-functions"></a>Yardımcı işlevleri ekleme

Bu yöntemler, Azure Medya Hizmetleri varlığından İçerik Moderatörü çıktı dosyasını (JSON) indirir ve programın konsola çalışan bir durumu günlüğe kaydedebilmeleri için denetleme işinin durumunu izlemenize yardımcı olur.

```csharp
static void DownloadAsset(IAsset asset, string outputDirectory)
{
    foreach (IAssetFile file in asset.AssetFiles)
    {
        file.Download(Path.Combine(outputDirectory, file.Name));
    }
}

// event handler for Job State
static void StateChanged(object sender, JobStateChangedEventArgs e)
{
    Console.WriteLine("Job state changed event:");
    Console.WriteLine("  Previous state: " + e.PreviousState);
    Console.WriteLine("  Current state: " + e.CurrentState);
    switch (e.CurrentState)
    {
        case JobState.Finished:
            Console.WriteLine();
            Console.WriteLine("Job finished.");
            break;
        case JobState.Canceling:
        case JobState.Queued:
        case JobState.Scheduled:
        case JobState.Processing:
            Console.WriteLine("Please wait...\n");
            break;
        case JobState.Canceled:
            Console.WriteLine("Job is canceled.\n");
            break;
        case JobState.Error:
            Console.WriteLine("Job failed.\n");
            break;
        default:
            break;
    }
}
```

### <a name="run-the-program-and-review-the-output"></a>Programı çalıştırma ve çıktıyı gözden geçirme

İçerik Moderasyonu işi tamamlandıktan sonra JSON yanıtını analiz edin. Bu öğelerden oluşur:

- Video bilgi özeti
- **"** **Parçalar**" olarak çekimler
- **Yetişkin** ve **Racy** puanlarına dayalı "**events**" with a **reviewRecommended" (= true or false)"** bayrağı olarak **anahtar kareler**
- **başlangıç**, **süre**, **totalSüre**ve **zaman damgası** "keneler" içindedir. Saniye cinsinden sayı almak için **zaman ölçeğine** bölün.
 
> [!NOTE]
> - `adultScore`belirli durumlarda müstehcen veya yetişkin olarak kabul edilebilecek içeriğin potansiyel varlığını ve tahmin puanını temsil eder.
> - `racyScore`bazı durumlarda müstehcen veya olgun olarak kabul edilebilecek içeriğin potansiyel varlığını ve tahmin puanını temsil eder.
> - `adultScore`ve `racyScore` 0 ile 1 arasındadır. Puan ne kadar yüksekse, model kategorinin uygulanabilir olabileceğini tahmin ediyor. Bu önizleme, el ile kodlanmış sonuçlar yerine istatistiksel bir modele dayanır. Her kategorinin gereksinimlerinize nasıl uygun olduğunu belirlemek için kendi içeriğinizle test yapmanızı öneririz.
> - `reviewRecommended`iç puan eşiklerine bağlı olarak doğru veya yanlıştır. Müşteriler bu değeri kullanıp kullanmayacağını veya içerik ilkelerine göre özel eşiklere karar verip vermeyeceğine karar vermelidir.

```json
{
"version": 2,
"timescale": 90000,
"offset": 0,
"framerate": 50,
"width": 1280,
"height": 720,
"totalDuration": 18696321,
"fragments": [
{
    "start": 0,
    "duration": 18000
},
{
    "start": 18000,
    "duration": 3600,
    "interval": 3600,
    "events": [
    [
        {
        "reviewRecommended": false,
        "adultScore": 0.00001,
        "racyScore": 0.03077,
        "index": 5,
        "timestamp": 18000,
        "shotIndex": 0
        }
    ]
    ]
},
{
    "start": 18386372,
    "duration": 119149,
    "interval": 119149,
    "events": [
    [
        {
        "reviewRecommended": true,
        "adultScore": 0.00000,
        "racyScore": 0.91902,
        "index": 5085,
        "timestamp": 18386372,
        "shotIndex": 62
        }
    ]
    ]
}
]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Denetleme çıktınızdan [nasıl video incelemeleri](video-reviews-quickstart-dotnet.md) oluşturacağınızı öğrenin.

Video incelemelerinize [transkript moderasyonu](video-transcript-moderation-review-tutorial-dotnet.md) ekleyin.

Tam bir video ve transkript [ılımlılık çözümü](video-transcript-moderation-review-tutorial-dotnet.md)oluşturmak için nasıl ayrıntılı öğretici göz atın.

Bunun için [Visual Studio çözümlerini indirin](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) ve diğer İçerik Moderatörleri .NET için hızlı bir şekilde başlatın.
