---
title: Medya Hizmetleri ile Azure İşlevleri Geliştirme
description: Bu konu, Azure portalını kullanarak Medya Hizmetleri ile Azure İşlevlerini geliştirmeye nasıl başlayacağımı gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 51bdcb01-1846-4e1f-bd90-70020ab471b0
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 618acae10b874eb5ebd5b6da7fe081368528dbd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251173"
---
# <a name="develop-azure-functions-with-media-services"></a>Medya Hizmetleri ile Azure İşlevleri Geliştirme

Bu makalede, Medya Hizmetleri'ni kullanan Azure İşlevleri oluşturmaya nasıl başalacağınızı gösterilmektedir. Bu makalede tanımlanan Azure İşlevi, yeni MP4 dosyaları için **giriş** adlı bir depolama hesabı kapsayıcısını izler. Bir dosya depolama kabına bırakıldığında, blob tetikleyici işlevi yürütür. Azure işlevlerini gözden geçirmek için Azure **işlevleri** bölümünde [genel bakış](../../azure-functions/functions-overview.md) ve diğer konulara bakın.

Azure Medya Hizmetleri'ni kullanan mevcut Azure Işlevlerini keşfetmek ve dağıtmak istiyorsanız, [Medya Hizmetleri Azure İşlevlerini](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)göz atın. Bu depo, içeriği doğrudan blob depolamadan, kodlamadan ve içeriği blob depolamaya geri yazmakla ilgili iş akışlarını göstermek için Medya Hizmetleri'ni kullanan örnekler içerir. Ayrıca, WebHooks ve Azure Kuyrukları aracılığıyla iş bildirimlerinin nasıl izlendiğine ilgili örnekler de içerir. İşlevlerinizi, [Medya Hizmetleri Azure Fonksiyonları](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) deposundaki örneklere göre de geliştirebilirsiniz. İşlevleri dağıtmak **için Azure'a Dağıt düğmesine** basın.

## <a name="prerequisites"></a>Ön koşullar

- İlk işlevinizin oluşturmadan önce etkin bir Azure hesabınız olması gerekir. Bir Azure hesabınız yoksa [ücretsiz hesaplar kullanılabilir](https://azure.microsoft.com/free/).
- Azure Medya Hizmetleri (AMS) hesabınızda eylem gerçekleştiren veya Medya Hizmetleri tarafından gönderilen olayları dinleyen Azure Fonksiyonları oluşturacaksanız, [burada](media-services-portal-create-account.md)açıklandığı gibi bir AMS hesabı oluşturmanız gerekir.
    
## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

1. [Azure portalına](https://portal.azure.com) gidip Azure hesabınızla oturum açın.
2. [Burada](../../azure-functions/functions-create-function-app-portal.md)açıklandığı gibi bir işlev uygulaması oluşturun.

>[!NOTE]
> **StorageConnection** ortamı değişkeninde belirttiğiniz bir depolama hesabı (bir sonraki adıma bakın) uygulamanızla aynı bölgede olmalıdır.

## <a name="configure-function-app-settings"></a>İşlev uygulama ayarlarını yapılandırma

Medya Hizmetleri işlevlerini geliştirirken, işlevleriniz boyunca kullanılacak ortam değişkenleri eklemek kullanışlıdır. Uygulama ayarlarını yapılandırmak için Uygulama Ayarlarını Yapılandır bağlantısını tıklatın. Daha fazla bilgi için Azure [İşlevi uygulama ayarlarını nasıl yapılandırılatır'](../../azure-functions/functions-how-to-use-azure-function-app-settings.md)a bakın. 

Bu makalede tanımlanan işlev, uygulama ayarlarınızda aşağıdaki ortam değişkenleri olduğunu varsayar:

**AMSAADTenantDomain**: Azure AD kiracı bitiş noktası. AMS API'sine bağlanma hakkında daha fazla bilgi için [bu](media-services-use-aad-auth-to-access-ams-api.md) makaleye bakın.

**AMSRESTAPIEndpoint**: REST API bitiş noktasını temsil eden URI. 

**AMSClientId**: Azure AD uygulaması istemci kimliği.

**AMSClientSecret**: Azure AD uygulaması istemcisi gizli.

**StorageConnection**: Medya Hizmetleri hesabıyla ilişkili hesabın depolama bağlantısı. Bu değer **function.json** dosyası ve **run.csx** dosyasında (aşağıda açıklanmıştır) kullanılır.

## <a name="create-a-function"></a>İşlev oluşturma

İşlev uygulamanız dağıtıldıktan sonra, **uygulamayı Uygulama Hizmetleri** Azure İşlevleri arasında bulabilirsiniz.

1. İşlev uygulamanızı seçin ve **Yeni İşlev'i**tıklatın.
2. **C#** dili ve **Veri İşleme** senaryosunu seçin.
3. **BlobTrigger** şablonu seçin. Bu **işlev, giriş** kabına bir blob yüklendiğinde tetiklenir. **Giriş** **adı,** bir sonraki adımda Yol'da belirtilir.

    ![files](./media/media-services-azure-functions/media-services-azure-functions004.png)

4. **BlobTrigger'ı**seçtikten sonra sayfada biraz daha denetim görünür.

    ![files](./media/media-services-azure-functions/media-services-azure-functions005.png)

4. **Oluştur'u**tıklatın. 

## <a name="files"></a>Dosyalar

Azure işleviniz, bu bölümde açıklanan kod dosyaları ve diğer dosyalarla ilişkilidir. Bir işlev oluşturmak için Azure portalını kullandığınızda, **function.json** ve **run.csx** sizin için oluşturulur. Bir **project.json** dosyası eklemeniz veya yüklemeniz gerekir. Bu bölümün geri kalanı her dosyanın kısa bir açıklamasını verir ve tanımlarını gösterir.

![files](./media/media-services-azure-functions/media-services-azure-functions003.png)

### <a name="functionjson"></a>function.json

function.json dosyası işlev bağlamalarını ve diğer yapılandırma ayarlarını tanımlar. Çalışma zamanı, izlenecek olayları ve işlevlerin yürütülmesinden veri aktarılmayı ve verileri nasıl döndüreceklerini belirlemek için bu dosyayı kullanır. Daha fazla bilgi için [Azure işlevleri HTTP ve webhook bağlamalarına](../../azure-functions/functions-reference.md#function-code)bakın.

>[!NOTE]
>İşlevin yürütülmesini önlemek için **devre dışı bırakılan** özelliği **doğru** olarak ayarlayın. 

Varolan function.json dosyasının içeriğini aşağıdaki kodla değiştirin:

```json
{
  "bindings": [
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "input/{filename}.mp4",
      "connection": "ConnectionString"
    }
  ],
  "disabled": false
}
```

### <a name="projectjson"></a>project.json

Project.json dosyası bağımlılıkları içerir. Burada Nuget gerekli .NET Azure Medya Hizmetleri paketleri içeren **project.json** dosyasının bir örneği. Sürüm numaralarının paketlerdeki en son güncelleştirmelerle değiştiğini unutmayın, bu nedenle en son sürümleri onaylamanız gerekir. 

Project.json'a aşağıdaki tanımı ekleyin. 

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "windowsazure.mediaservices": "4.0.0.4",
        "windowsazure.mediaservices.extensions": "4.0.0.4",
        "Microsoft.IdentityModel.Clients.ActiveDirectory": "3.13.1",
        "Microsoft.IdentityModel.Protocol.Extensions": "1.0.2.206221351"
      }
    }
   }
}

```
    
### <a name="runcsx"></a>run.csx

Bu, işlevinizin C# kodudur.  Aşağıda tanımlanan işlev, yeni MP4 dosyaları için **giriş** adlı bir depolama hesabı kapsayıcısını (yolda belirtilen ler) izler. Bir dosya depolama kabına bırakıldığında, blob tetikleyici işlevi yürütür.
    
Bu bölümde tanımlanan örnek, 

1. bir varlığın Medya Hizmetleri hesabına nasıl sindirilen (bir blob'u bir AMS varlığına sokarak) ve 
2. Media Encoder Standard'ın "Uyarlanabilir Akış" ön ayarlarını kullanan bir kodlama işi nasıl gönderilemez.

Gerçek hayattaki senaryoda, büyük olasılıkla iş ilerlemesini izlemek ve ardından kodlanmış varlığınızı yayımlamak istiyorsunuz. Daha fazla bilgi için Medya [Hizmetleri iş bildirimlerini izlemek için Azure WebHooks'u kullanın'a](media-services-dotnet-check-job-progress-with-webhooks.md)bakın. Daha fazla örnek için Bkz. [Medya Hizmetleri Azure İşlevleri.](https://github.com/Azure-Samples/media-services-dotnet-functions-integration)  

Varolan run.csx dosyasının içeriğini aşağıdaki kodla değiştirin: İşlevinizi tanımlamayı bitirdikten sonra **Kaydet ve Çalıştır'ı**tıklatın.

```csharp
#r "Microsoft.WindowsAzure.Storage"
#r "Newtonsoft.Json"
#r "System.Web"

using System;
using System.Net;
using System.Net.Http;
using Newtonsoft.Json;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
using System.IO;
using System.Web;
using Microsoft.Azure;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.Azure.WebJobs;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
  
// Read values from the App.config file.

static readonly string _AADTenantDomain = Environment.GetEnvironmentVariable("AMSAADTenantDomain");
static readonly string _RESTAPIEndpoint = Environment.GetEnvironmentVariable("AMSRESTAPIEndpoint");
 
static readonly string _mediaservicesClientId = Environment.GetEnvironmentVariable("AMSClientId");
static readonly string _mediaservicesClientSecret = Environment.GetEnvironmentVariable("AMSClientSecret");

static readonly string _connectionString = Environment.GetEnvironmentVariable("ConnectionString");  

private static CloudMediaContext _context = null;
private static CloudStorageAccount _destinationStorageAccount = null;

public static void Run(CloudBlockBlob myBlob, string fileName, TraceWriter log)
{
    // NOTE that the variables {fileName} here come from the path setting in function.json
    // and are passed into the  Run method signature above. We can use this to make decisions on what type of file
    // was dropped into the input container for the function. 

    // No need to do any Retry strategy in this function, By default, the SDK calls a function up to 5 times for a 
    // given blob. If the fifth try fails, the SDK adds a message to a queue named webjobs-blobtrigger-poison.

    log.Info($"C# Blob trigger function processed: {fileName}.mp4");
    log.Info($"Media Services REST endpoint : {_RESTAPIEndpoint}");

    try
    {
        AzureAdTokenCredentials tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain,
                            new AzureAdClientSymmetricKey(_mediaservicesClientId, _mediaservicesClientSecret),
                            AzureEnvironments.AzureCloudEnvironment);
 
        AzureAdTokenProvider tokenProvider = new AzureAdTokenProvider(tokenCredentials);
 
        _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

        IAsset newAsset = CreateAssetFromBlob(myBlob, fileName, log).GetAwaiter().GetResult();

        // Step 2: Create an Encoding Job

        // Declare a new encoding job with the Standard encoder
        IJob job = _context.Jobs.Create("Azure Function - MES Job");

        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Create a task with the encoding details, using a custom preset
        ITask task = job.Tasks.AddNew("Encode with Adaptive Streaming",
            processor,
            "Adaptive Streaming",
            TaskOptions.None); 

        // Specify the input asset to be encoded.
        task.InputAssets.Add(newAsset);

        // Add an output asset to contain the results of the job. 
        // This output is specified as AssetCreationOptions.None, which 
        // means the output asset is not encrypted. 
        task.OutputAssets.AddNew(fileName, AssetCreationOptions.None);

        job.Submit();
        log.Info("Job Submitted");

    }
    catch (Exception ex)
    {
        log.Error("ERROR: failed.");
        log.Info($"StackTrace : {ex.StackTrace}");
        throw ex;
    }
}

private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
{
    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

    if (processor == null)
    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

    return processor;
}

public static async Task<IAsset> CreateAssetFromBlob(CloudBlockBlob blob, string assetName, TraceWriter log){
    IAsset newAsset = null;

    try{
        Task<IAsset> copyAssetTask = CreateAssetFromBlobAsync(blob, assetName, log);
        newAsset = await copyAssetTask;
        log.Info($"Asset Copied : {newAsset.Id}");
    }
    catch(Exception ex){
        log.Info("Copy Failed");
        log.Info($"ERROR : {ex.Message}");
        throw ex;
    }

    return newAsset;
}

/// <summary>
/// Creates a new asset and copies blobs from the specifed storage account.
/// </summary>
/// <param name="blob">The specified blob.</param>
/// <returns>The new asset.</returns>
public static async Task<IAsset> CreateAssetFromBlobAsync(CloudBlockBlob blob, string assetName, TraceWriter log)
{
     //Get a reference to the storage account that is associated with the Media Services account. 
    _destinationStorageAccount = CloudStorageAccount.Parse(_connectionString);

    // Create a new asset. 
    var asset = _context.Assets.Create(blob.Name, AssetCreationOptions.None);
    log.Info($"Created new asset {asset.Name}");

    IAccessPolicy writePolicy = _context.AccessPolicies.Create("writePolicy",
    TimeSpan.FromHours(4), AccessPermissions.Write);
    ILocator destinationLocator = _context.Locators.CreateLocator(LocatorType.Sas, asset, writePolicy);
    CloudBlobClient destBlobStorage = _destinationStorageAccount.CreateCloudBlobClient();

    // Get the destination asset container reference
    string destinationContainerName = (new Uri(destinationLocator.Path)).Segments[1];
    CloudBlobContainer assetContainer = destBlobStorage.GetContainerReference(destinationContainerName);

    try{
    assetContainer.CreateIfNotExists();
    }
    catch (Exception ex)
    {
    log.Error ("ERROR:" + ex.Message);
    }

    log.Info("Created asset.");

    // Get hold of the destination blob
    CloudBlockBlob destinationBlob = assetContainer.GetBlockBlobReference(blob.Name);

    // Copy Blob
    try
    {
    using (var stream = await blob.OpenReadAsync()) 
    {            
        await destinationBlob.UploadFromStreamAsync(stream);          
    }

    log.Info("Copy Complete.");

    var assetFile = asset.AssetFiles.Create(blob.Name);
    assetFile.ContentFileSize = blob.Properties.Length;
    assetFile.IsPrimary = true;
    assetFile.Update();
    asset.Update();
    }
    catch (Exception ex)
    {
    log.Error(ex.Message);
    log.Info (ex.StackTrace);
    log.Info ("Copy Failed.");
    throw;
    }

    destinationLocator.Delete();
    writePolicy.Delete();

    return asset;
}
```

## <a name="test-your-function"></a>İşlevinizi sınama

İşlevinizi sınamak için, bağlantı dizesinde belirttiğiniz depolama hesabının **giriş** kapsayıcısına bir MP4 dosyası yüklemeniz gerekir.  

1. **StorageConnection** ortamı değişkeninde belirttiğiniz depolama hesabını seçin.
2. **Blobs'ı**tıklatın.
3. **+ Kapsayıcı**'ya tıklayın. Kapsayıcı **girişine**isim verem.
4. **Upload tuşuna** basın ve yüklemek istediğiniz bir .mp4 dosyasına göz atın.

>[!NOTE]
> Tüketim planında bir blob tetikleyicisi kullanıyorsanız, bir işlev uygulaması boşta kaldıktan sonra yeni lekeleri işlemekte 10 dakikaya kadar gecikme olabilir. Fonksiyon uygulaması çalıştırıldıktan sonra, lekeler hemen işlenir. Daha fazla bilgi için [Blob depolama tetikleyicileri ve bağlamaları'na](https://docs.microsoft.com/azure/azure-functions/functions-bindings-storage-blob)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu noktada, bir Medya Hizmetleri uygulaması geliştirmeye başlamaya hazırsınız. 
 
Azure İşlerini ve Mantıksal Uygulamaları Azure Medya Hizmetleri ile kullanarak özel içerik oluşturma iş akışları oluşturmak için daha fazla ayrıntı ve eksiksiz örnekler/çözümler için [GitHub'daki Medya Hizmetleri .NET İştümleme Örneği'ne](https://github.com/Azure-Samples/media-services-dotnet-functions-integration) bakın

Ayrıca, [medya hizmetleri iş bildirimlerini .NET ile izlemek için Azure WebHooks'u kullanın'a](media-services-dotnet-check-job-progress-with-webhooks.md)bakın. 

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

