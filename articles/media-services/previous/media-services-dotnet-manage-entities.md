---
title: Medya Hizmetleri .NET SDK ile Varlıkların ve İlgili Varlıkların Yönetimi
description: .NET için Medya Hizmetleri SDK ile varlıkları ve ilgili varlıkları nasıl yöneteceklerini öğrenin.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 1bd8fd42-7306-463d-bfe5-f642802f1906
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: a686465b0006c2e9aac6e06cb4ab12d30921e8c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251147"
---
# <a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Medya Hizmetleri .NET SDK ile Varlıkların ve İlgili Varlıkların Yönetimi
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-manage-entities.md)
> * [Geri kalanı](media-services-rest-manage-entities.md)
> 
> 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Bu konu, .NET ile Azure Medya Hizmetleri varlıklarının nasıl yönetilenini gösterir.

1 Nisan 2017’den itibaren, hesabınızdaki 90 günden eski olan tüm İş kayıtları, toplam kayıt sayısı üst kota sınırının altında olsa bile ilişkili Görev kayıtlarıyla birlikte otomatik olarak silinecektir. Örneğin, 1 Nisan 2017'de, hesabınızdaki 31 Aralık 2016'dan büyük tüm İş kayıtları otomatik olarak silinir. İş/görev bilgilerini arşivlemeniz gerekiyorsa, bu konuda açıklanan kodu kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

## <a name="get-an-asset-reference"></a>Varlık Referansı Alın
Sık kullanılan bir görev, Medya Hizmetleri'ndeki varolan bir varlığa başvuruda bulunulmaktır. Aşağıdaki kod örneği, varlık Kimliği'ni temel alan sunucu bağlam nesnesindeki Varlıklar koleksiyonundan nasıl varlık başvurusu alabileceğinizi gösterir. Aşağıdaki kod örneği, varolan bir IAsset nesnesine başvuru almak için bir Linq sorgusu kullanır.

```csharp
    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();

        return asset;
    }
```

## <a name="list-all-assets"></a>Tüm Varlıkları Listele
Depolama alanınızdaki varlık sayısı arttıkça, varlıklarınızı listelemek yararlı olur. Aşağıdaki kod örneği, sunucu bağlam nesnesindeki Varlıklar koleksiyonunda nasıl yinelenebildiğini gösterir. Her varlıkta, kod örneği de bazı özellik değerlerini konsola yazar. Örneğin, her varlık birçok ortam dosyası içerebilir. Kod örneği, her varlıkla ilişkili tüm dosyaları yazar.

```csharp
    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");

            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }

        // Display output in console.
        Console.Write(builder.ToString());
    }
```

## <a name="get-a-job-reference"></a>İş Başvurusu Alın

Medya Hizmetleri kodundaki görevleri işlemeyle çalışırken, genellikle bir Id'ye dayalı varolan bir işe başvuru almanız gerekir. Aşağıdaki kod örneği, İşler koleksiyonundan bir IJob nesnesine nasıl başvuru alınır gösterilmektedir.

Uzun soluklu bir kodlama işine başlarken bir iş başvurusu almanız ve iş parçacığı üzerindeki iş durumunu denetlemeniz gerekebilir. Bu gibi durumlarda, yöntem iş parçacığından döndüğünde, bir iş için yenilenmiş bir başvuru almanız gerekir.

```csharp
    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();

        return job;
    }
```

## <a name="list-jobs-and-assets"></a>İş ve Varlıkları Listele
İlgili önemli bir görev, Medya Hizmetleri'ndeki ilişkili işleri olan varlıkları listelemektir. Aşağıdaki kod örneği, her IJob nesnesini nasıl listelediğinizi gösterir ve sonra her iş için iş, ilgili tüm görevler, tüm giriş varlıkları ve tüm çıktı varlıklarıyla ilgili özellikleri görüntüler. Bu örnekteki kod, çok sayıda diğer görevler için yararlı olabilir. Örneğin, daha önce çalıştırdığınız bir veya daha fazla kodlama işinden çıktı varlıklarını listelemek istiyorsanız, bu kod çıktı varlıklarına nasıl erişilenleri gösterir. Bir çıktı varlığına başvurunuz olduğunda, içeriği indirerek veya URL sağlayarak diğer kullanıcılara veya uygulamalara teslim edebilirsiniz. 

Varlıkları teslim etme seçenekleri hakkında daha fazla bilgi için [,.NET için Medya Hizmetleri SDK ile Varlıkları Teslim Etme'ye](media-services-deliver-streaming-content.md)bakın.

```csharp
    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);

        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();

        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");


            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }

            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {

                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }

            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }

        }

        // Display output in console.
        Console.Write(builder.ToString());
    }
```

## <a name="list-all-access-policies"></a>Tüm Erişim İlkelerini Listele
Medya Hizmetleri'nde, bir varlık veya dosyaları yla ilgili bir erişim ilkesi tanımlayabilirsiniz. Erişim ilkesi, bir dosyanın veya kıymetin izinlerini (ne tür erişim ve süre) tanımlar. Medya Hizmetleri kodunuzda, genellikle bir IAccessPolicy nesnesi oluşturup varolan bir varlıkla ilişkilendirerek bir erişim ilkesi tanımlarsınız. Ardından, Medya Hizmetleri'ndeki varlıklara doğrudan erişim sağlamanızı sağlayan bir ILocator nesnesi oluşturursunuz. Bu dokümantasyon serisine eşlik eden Visual Studio projesi, erişim ilkeleri ve varlıkların yer bulucularının nasıl oluşturulup atanın gerektiğini gösteren çeşitli kod örnekleri içerir.

Aşağıdaki kod örneği, sunucudaki tüm erişim ilkelerinin nasıl listelenebildiğini gösterir ve her biriyle ilişkili izinlerin türünü gösterir. Erişim ilkelerini görüntülemenin başka bir yararlı yolu sunucudaki tüm ILocator nesnelerini listelemektir ve ardından her yer bulucu için AccessPolicy özelliğini kullanarak ilişkili erişim ilkesini listeleyebilirsiniz.

```csharp
    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");

        }
    }
```
    
## <a name="limit-access-policies"></a>Erişim İlkelerini Sınırla 

>[!NOTE]
> Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). 

Örneğin, uygulamanızda yalnızca bir kez çalışacak aşağıdaki koda sahip genel bir ilke kümesi oluşturabilirsiniz. Daha sonra kullanmak üzere bir günlük dosyasına günlük defterine günlük atabilirsiniz:

```csharp
    double year = 365.25;
    double week = 7;
    IAccessPolicy policyYear = _context.AccessPolicies.Create("One Year", TimeSpan.FromDays(year), AccessPermissions.Read);
    IAccessPolicy policy100Year = _context.AccessPolicies.Create("Hundred Years", TimeSpan.FromDays(year * 100), AccessPermissions.Read);
    IAccessPolicy policyWeek = _context.AccessPolicies.Create("One Week", TimeSpan.FromDays(week), AccessPermissions.Read);

    Console.WriteLine("One year policy ID is: " + policyYear.Id);
    Console.WriteLine("100 year policy ID is: " + policy100Year.Id);
    Console.WriteLine("One week policy ID is: " + policyWeek.Id);
```

Ardından, kodunızdaki varolan kodları şu şekilde kullanabilirsiniz:

```csharp
    const string policy1YearId = "nb:pid:UUID:2a4f0104-51a9-4078-ae26-c730f88d35cf";


    // Get the standard policy for 1 year read only
    var tempPolicyId = from b in _context.AccessPolicies
                       where b.Id == policy1YearId
                       select b;
    IAccessPolicy policy1Year = tempPolicyId.FirstOrDefault();

    // Get the existing asset
    var tempAsset = from a in _context.Assets
                where a.Id == assetID
                select a;
    IAsset asset = tempAsset.SingleOrDefault();

    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
        policy1Year,
        DateTime.UtcNow.AddMinutes(-5));
    Console.WriteLine("The locator base path is " + originLocator.BaseUri.ToString());
```

## <a name="list-all-locators"></a>Tüm Locators listele
Yer bulucu, bir varlığa erişmek için doğrudan bir yol ve bulucunun ilişkili erişim ilkesinde tanımlandığı şekilde varlığa yönelik izinler sağlayan bir URL'dir. Her varlığın, Locators özelliğinde onunla ilişkili ILocator nesnelerinden oluşan bir koleksiyonu olabilir. Sunucu bağlamında ayrıca tüm yer bulucuları içeren bir Konumbelirleyiciler koleksiyonu vardır.

Aşağıdaki kod örneği, sunucudaki tüm konum belirleyicileri listeler. Her yer bulucu için, ilgili varlık ve erişim ilkesi için Id gösterir. Ayrıca izinlerin türünü, son kullanma tarihini ve varlığa giden tam yolu görüntüler.

Bir varlığa giden bulucu yolunun kıymetin yalnızca temel URL'si olduğunu unutmayın. Bir kullanıcının veya uygulamanın göz atabileceği tek tek dosyalara doğrudan bir yol oluşturmak için, kodunuzun bulucu yoluna belirli bir dosya yolunu eklemesi gerekir. Bunun nasıl yapılacılayacıyla ilgili daha fazla bilgi için, .NET için [Medya Hizmetleri SDK ile Varlıkları Teslim](media-services-deliver-streaming-content.md)etme konusuna bakın.

```csharp
    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }
```

## <a name="enumerating-through-large-collections-of-entities"></a>Varlıkların büyük koleksiyonları aracılığıyla sayısal
Varlıkları sorgularken, public REST v2 sorgu sonuçlarını 1000 sonuçla sınırladığı için, aynı anda döndürülen 1000 varlık sınırı vardır. Büyük varlık koleksiyonları arasında sayısallaştırırken Atla ve Al'ı kullanmanız gerekir. 

Aşağıdaki işlev, sağlanan Medya Hizmetleri Hesabı'ndaki tüm işlerde döngüler. Medya Hizmetleri, Jobs Collection'da 1000 iş döndürür. İşlev, tüm işlerin numaralandırıldığından emin olmak için Atla ve Al'ı kullanır (hesabınızda 1000'den fazla iş olması durumunda).

```csharp
    static void ProcessJobs()
    {
        try
        {

            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;

            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }

                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }
```

## <a name="delete-an-asset"></a>Bir Varlığı Silme
Aşağıdaki örnek bir varlığı siler.

```csharp
    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();

        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");

    }
```

## <a name="delete-a-job"></a>İşi Silme
Bir işi silmek için, Devlet mülkünde belirtildiği gibi işin durumunu denetlemeniz gerekir. Tamamlanan veya iptal edilen işler silinebilirken, sıraya, zamanlanan veya işleme gibi belirli diğer durumlarda ki işlerin önce iptal edilmesi ve ardından silinmesi gerekir.

Aşağıdaki kod örneği, iş durumlarını denetleyerek ve durum bittiğinde veya iptal edildiğinde silme yöntemini gösterir. Bu kod, bir işe başvuru almak için bu konudaki önceki bölüme bağlıdır: Bir iş başvurusu alın.

```csharp
    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;

        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);

            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }

        }
    }
```


## <a name="delete-an-access-policy"></a>Erişim İlkesi Silme
Aşağıdaki kod örneği, bir ilke Kimliği'ne dayalı bir erişim ilkesine nasıl başvuru alınıp sonra ilkeyi nasıl silerken gösterilmektedir.

```csharp
    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();

        policy.Delete();

    }
```


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

