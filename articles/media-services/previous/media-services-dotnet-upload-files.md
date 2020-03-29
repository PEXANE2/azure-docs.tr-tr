---
title: .NET | Microsoft Dokümanlar
description: Varlıklar oluşturup yükleyerek medya içeriğini Medya Hizmetleri'ne nasıl dahil ediniz öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: c9c86380-9395-4db8-acea-507c52066f73
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 03b9995eab503ac1fcd4615882419dde31d4f8bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64869485"
---
# <a name="upload-files-into-a-media-services-account-using-net"></a>.NET kullanarak Media Services hesabına dosya yükleme 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Media Services’de, dijital dosyalar bir varlığa yüklenir (veya alınır). **Varlık** varlığı video, ses, görüntü, küçük resim koleksiyonları, metin parçaları ve kapalı altyazı dosyaları (ve bu dosyalarla ilgili meta veriler) içerebilir.  Dosyalar yüklendikten sonra, içeriğiniz daha fazla işleme ve akış için bulutta güvenli bir şekilde saklanır.

Varlık içindeki dosyalara **Varlık Dosyaları** adı verilir. **AssetFile** örneği ve gerçek ortam dosyası iki farklı nesnedir. AssetFile örneği medya dosyası yla ilgili meta verileri içerirken, ortam dosyası gerçek medya içeriğini içerir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki noktalara dikkat edilmelidir:
 
 * Medya Hizmetleri, akış içeriği için URL'ler inşa ederken IAssetFile.Name özelliğinin değerini kullanır (örneğin, http://{AMSAccount}.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Bu nedenle, yüzde kodlama izin verilmez. **Ad** özelliğinin değeri aşağıdaki [yüzde kodlama-ayrılmış karakterlerden](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)hiçbirine sahip olamaz : !*'();:@&=+$,/?%#[]". Ayrıca, dosya adı uzantısı için yalnızca bir '.' olabilir.
* Adın uzunluğu 260 karakterden büyük olmamalıdır.
* Media Services ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. Dosya boyutu sınırlaması hakkında ayrıntılı bilgi için [bu](media-services-quotas-and-limitations.md) makaleye bakın.
* Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu makaleye](media-services-dotnet-manage-entities.md#limit-access-policies) bakın.

Varlıkları oluşturduğunuzda, aşağıdaki şifreleme seçeneklerini belirtebilirsiniz:

* **Hiçbiri**: Şifreleme kullanılmaz. Varsayılan değer budur. Bu seçeneği kullanırken içeriğiniz taşıma sırasında veya depolama alanında korumalı değildir.
  Aşamalı karşıdan yüklemeyi kullanarak bir MP4 sunmayı planlıyorsanız, şu seçeneği kullanın: 
* **CommonEncryption** - Daha önce şifrelenmiş ve Ortak Şifreleme veya PlayReady DRM ile korunmuş içerik yüklüyorsanız bu seçeneği kullanın (örneğin, PlayReady DRM ile korunan Sorunsuz Akış).
* **EnvelopeEncrypted** – AES ile şifrelenmiş HLS yüklüyorsanız bu seçeneği kullanın. Dosyaların Transform Manager tarafından kodlanmış ve şifrelenmiş olması gerektiğini unutmayın.
* **StorageEncrypted** - AES-256 bit şifrelemekullanarak net içeriğinizi yerel olarak şifreler ve ardından güvenli bir şekilde şifrelendiği Azure Depolama'ya yükler. Depolama Şifrelemesi ile korunan varlıklar, kodlamadan önce otomatik olarak şifrelenerek şifrelenmiş bir dosya sistemine yerleştirilir ve yeni bir çıktı varlığı şeklinde geri yüklenmeden önce isteğe bağlı olarak yeniden şifrelenir. Depolama Şifrelemesinin birincil kullanım nedeni, yüksek kaliteli girdi medya dosyalarınızın güvenliğini güçlü şifrelemeyle diskte bekleyen konumda sağlamak istediğiniz durumdur.
  
    Medya Hizmetleri, Dijital Haklar Yöneticisi (DRM) gibi kablolu olarak değil, varlıklarınız için disk te sağlýk depolama şifrelemesi sağlar.
  
    Varlığınıza depolama şifrelemesi uygulanmışsa varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için [bkz.](media-services-dotnet-configure-asset-delivery-policy.md)

Kıymetinizin **Ortak Şifrelenmiş** bir seçenek veya **EnvelopeEncrypted** seçeneği yle şifreleneceğini belirtirseniz, varlığınızı bir **ContentKey**ile ilişkilendirmeniz gerekir. Daha fazla bilgi için [ContentKey nasıl oluşturulur.](media-services-dotnet-create-contentkey.md) 

Kıymetinizin **StorageEncrypted** seçeneği yle şifreleneceğini belirtirseniz, .NET için Medya Hizmetleri SDK, varlığınız için bir **StorageEncrypted** **ContentKey** oluşturur.

Bu makalede, Medya Hizmetleri .NET SDK'nın yanı sıra Medya Hizmetleri .NET SDK uzantılarının bir Medya Hizmetleri varlığına dosya yüklemek için nasıl kullanılacağı gösterilmektedir.

## <a name="upload-a-single-file-with-media-services-net-sdk"></a>Medya Hizmetleri .NET SDK ile tek bir dosya yükleme

Aşağıdaki kod tek bir dosyayı yüklemek için .NET'i kullanır. AccessPolicy ve Locator, Upload işlevi tarafından oluşturulur ve yok edilir. 

```csharp
        static public IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
        {
            if (!File.Exists(singleFilePath))
            {
                Console.WriteLine("File does not exist.");
                return null;
            }

            var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
            IAsset inputAsset = _context.Assets.Create(assetName, assetCreationOptions);

            var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));

            Console.WriteLine("Upload {0}", assetFile.Name);

            assetFile.Upload(singleFilePath);
            Console.WriteLine("Done uploading {0}", assetFile.Name);

            return inputAsset;
        }
```


## <a name="upload-multiple-files-with-media-services-net-sdk"></a>Medya Hizmetleri .NET SDK ile birden fazla dosya yükleyin
Aşağıdaki kod, bir varlığın nasıl oluşturulup birden çok dosya yüklenirken gösterilmektedir.

Kod aşağıdakileri yapar:

* Önceki adımda tanımlanan CreateEmptyAsset yöntemini kullanarak boş bir kıymet oluşturur.
* Varlığa erişim izinlerini ve süresini tanımlayan bir **AccessPolicy** örneği oluşturur.
* Varlığa erişim sağlayan bir **Konum belirleyici** örneği oluşturur.
* **BlobTransferClient** örneği oluşturur. Bu tür, Azure lekeleri üzerinde çalışan bir istemciyi temsil eder. Bu örnekte, istemci yükleme ilerlemesini izler. 
* Belirtilen dizindeki dosyaları oyalar ve her dosya için bir **AssetFile** örneği oluşturur.
* **UploadAsync** yöntemini kullanarak dosyaları Medya Hizmetleri'ne yükler. 

> [!NOTE]
> Aramaların engellenmediğinden ve dosyaların paralel olarak yüklendiğinden emin olmak için UploadAsync yöntemini kullanın.
> 
> 

```csharp
        static public IAsset CreateAssetAndUploadMultipleFiles(AssetCreationOptions assetCreationOptions, string folderPath)
        {
            var assetName = "UploadMultipleFiles_" + DateTime.UtcNow.ToString();

            IAsset asset = _context.Assets.Create(assetName, assetCreationOptions);

            var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                AccessPermissions.Write | AccessPermissions.List);

            var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);

            var blobTransferClient = new BlobTransferClient();
            blobTransferClient.NumberOfConcurrentTransfers = 20;
            blobTransferClient.ParallelTransferThreadCount = 20;

            blobTransferClient.TransferProgressChanged += blobTransferClient_TransferProgressChanged;

            var filePaths = Directory.EnumerateFiles(folderPath);

            Console.WriteLine("There are {0} files in {1}", filePaths.Count(), folderPath);

            if (!filePaths.Any())
            {
                throw new FileNotFoundException(String.Format("No files in directory, check folderPath: {0}", folderPath));
            }

            var uploadTasks = new List<Task>();
            foreach (var filePath in filePaths)
            {
                var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                Console.WriteLine("Created assetFile {0}", assetFile.Name);

                // It is recommended to validate AssetFiles before upload. 
                Console.WriteLine("Start uploading of {0}", assetFile.Name);
                uploadTasks.Add(assetFile.UploadAsync(filePath, blobTransferClient, locator, CancellationToken.None));
            }

            Task.WaitAll(uploadTasks.ToArray());
            Console.WriteLine("Done uploading the files");

            blobTransferClient.TransferProgressChanged -= blobTransferClient_TransferProgressChanged;

            locator.Delete();
            accessPolicy.Delete();

            return asset;
        }

    static void  blobTransferClient_TransferProgressChanged(object sender, BlobTransferProgressChangedEventArgs e)
    {
        if (e.ProgressPercentage > 4) // Avoid startup jitter, as the upload tasks are added.
        {
            Console.WriteLine("{0}% upload competed for {1}.", e.ProgressPercentage, e.LocalFile);
        }
    }
```


Çok sayıda varlık yüklerken aşağıdakileri göz önünde bulundurun:

* İş parçacığı başına yeni bir **CloudMediaContext** nesnesi oluşturun. **CloudMediaContext** sınıfı iş parçacığı için güvenli değildir.
* SayıEşzamanlıAkları 2'nin varsayılan değerinden 5 gibi daha yüksek bir değere yükseltin. Bu özelliğin ayarlanması **CloudMediaContext'ın**tüm örneklerini etkiler. 
* ParallelTransferThreadCount'ı varsayılan değerde 10 olarak tutun.

## <a name="ingesting-assets-in-bulk-using-media-services-net-sdk"></a><a id="ingest_in_bulk"></a>Medya Hizmetleri .NET SDK kullanarak Varlıkların Toplu Olarak Yutulma
Büyük varlık dosyaları yüklemek, varlık oluşturma sırasında bir darboğaz olabilir. Varlıkları Toplu olarak veya "Toplu Alma"da yutmak, varlık oluşturmayı yükleme işleminden ayırmayı içerir. Toplu sindirici bir yaklaşım kullanmak için, varlığı ve ilişkili dosyalarını açıklayan bir bildirim (IngestManifest) oluşturun. Ardından, ilişkili dosyaları manifestonun blob kapsayıcısına yüklemek için seçtiğiniz yükleme yöntemini kullanın. Microsoft Azure Media Services, bildirimle ilişkili blob kapsayıcısını izler. Bir dosya blob kapsayıcısına yüklendikten sonra, Microsoft Azure Media Services, bildirimdeki varlığın yapılandırmasına (IngestManifestAsset) bağlı olarak varlık oluşturmayı tamamlar.

Yeni bir IngestManifest oluşturmak için, CloudMediaContext'taki IngestManifests koleksiyonunun maruz kaçtığı Oluşturma yöntemini arayın. Bu yöntem, sağladığınız bildirim adı ile yeni bir IngestManifest oluşturur.

```csharp
    IIngestManifest manifest = context.IngestManifests.Create(name);
```

Toplu IngestManifest ile ilişkili varlıkları oluşturun. Toplu sindirme için varlık üzerinde istenen şifreleme seçeneklerini yapılandırın.

```csharp
    // Create the assets that will be associated with this bulk ingest manifest
    IAsset destAsset1 = _context.Assets.Create(name + "_asset_1", AssetCreationOptions.None);
    IAsset destAsset2 = _context.Assets.Create(name + "_asset_2", AssetCreationOptions.None);
```

Bir IngestManifestAsset toplu yutma için bir toplu IngestManifest ile bir Varlık ilişkilendirer. Ayrıca, her Varlığı oluşturan Varlık Dosyalarını da ilişkilendirer. Bir IngestManifestAsset oluşturmak için sunucu bağlamında Oluştur yöntemini kullanın.

Aşağıdaki örnek, daha önce oluşturulan iki varlığı toplu yutma bildirimine ilişkilendiren iki yeni IngestManifest Varlığı eklemeyi gösterir. Her IngestManifestAsset, toplu sindirme sırasında her varlık için yüklenen bir dizi dosyayı da ilişkilendirer.  

```csharp
    string filename1 = _singleInputMp4Path;
    string filename2 = _primaryFilePath;
    string filename3 = _singleInputFilePath;

    IIngestManifestAsset bulkAsset1 =  manifest.IngestManifestAssets.Create(destAsset1, new[] { filename1 });
    IIngestManifestAsset bulkAsset2 =  manifest.IngestManifestAssets.Create(destAsset2, new[] { filename2, filename3 });
```

Eğer **IngestManifest IIngestManifest.BlobStorageUriForUpload** özelliği tarafından sağlanan blob depolama konteyner URI varlık dosyalarını yükleme yeteneğine sahip herhangi bir yüksek hızlı istemci uygulaması kullanabilirsiniz. 

Aşağıdaki kod, varlık dosyalarını yüklemek için .NET SDK'nın nasıl kullanılacağını gösterir.

```csharp
    static void UploadBlobFile(string containerName, string filename)
    {
        Task copytask = new Task(() =>
        {
            var storageaccount = new CloudStorageAccount(new StorageCredentials(_storageAccountName, _storageAccountKey), true);
            CloudBlobClient blobClient = storageaccount.CreateCloudBlobClient();
            CloudBlobContainer blobContainer = blobClient.GetContainerReference(containerName);

            string[] splitfilename = filename.Split('\\');
            var blob = blobContainer.GetBlockBlobReference(splitfilename[splitfilename.Length - 1]);

            using (var stream = System.IO.File.OpenRead(filename))
                blob.UploadFromStream(stream);

            lock (consoleWriteLock)
            {
                Console.WriteLine("Upload for {0} completed.", filename);
            }
        });

        copytask.Start();
    }
```

Bu makalede kullanılan örnek için varlık dosyaları yükleme kodu aşağıdaki kod örneğinde gösterilmiştir:

```csharp
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename1);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename2);
    UploadBlobFile(manifest.BlobStorageUriForUpload, filename3);
```

**IngestManifest'in**İstatistik özelliğini yoklayarak **IngestManifest** ile ilişkili tüm varlıklar için toplu sindiricinin ilerlemesini belirleyebilirsiniz. İlerleme bilgilerini güncelleştirmek için, İstatistik ler özelliğini her yoklağınızda yeni bir **CloudMediaContext** kullanmanız gerekir.

Aşağıdaki örnek, bir IngestManifest'in **id'ine**göre yoklamayı gösterir.

```csharp
    static void MonitorBulkManifest(string manifestID)
    {
       bool bContinue = true;
       while (bContinue)
       {
          CloudMediaContext context = GetContext();
          IIngestManifest manifest = context.IngestManifests.Where(m => m.Id == manifestID).FirstOrDefault();

          if (manifest != null)
          {
             lock(consoleWriteLock)
             {
                Console.WriteLine("\nWaiting on all file uploads.");
                Console.WriteLine("PendingFilesCount  : {0}", manifest.Statistics.PendingFilesCount);
                Console.WriteLine("FinishedFilesCount : {0}", manifest.Statistics.FinishedFilesCount);
                Console.WriteLine("{0}% complete.\n", (float)manifest.Statistics.FinishedFilesCount / (float)(manifest.Statistics.FinishedFilesCount + manifest.Statistics.PendingFilesCount) * 100);

                if (manifest.Statistics.PendingFilesCount == 0)
                {
                   Console.WriteLine("Completed\n");
                   bContinue = false;
                }
             }

             if (manifest.Statistics.FinishedFilesCount < manifest.Statistics.PendingFilesCount)
                Thread.Sleep(60000);
          }
          else // Manifest is null
             bContinue = false;
       }
    }
```


## <a name="upload-files-using-net-sdk-extensions"></a>.NET SDK Uzantılarını kullanarak dosya yükleme
Aşağıdaki örnekte, .NET SDK Uzantılarını kullanarak tek bir dosyanın nasıl yüklenirgibi olduğu gösterilmektedir. Bu durumda **CreateFromFile** yöntemi kullanılır, ancak asynchronous sürümü de kullanılabilir (**CreateFromFileAsync**). **CreateFromFile** yöntemi, dosyanın yükleme ilerlemesini bildirmek için dosya adını, şifreleme seçeneğini ve geri aramayı belirtmenizi sağlar.

```csharp
    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }
```

Aşağıdaki örnek, UploadFile işlevini çağırır ve depolama şifrelemesini varlık oluşturma seçeneği olarak belirtir.  

```csharp
    var asset = UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.StorageEncrypted);
```

## <a name="next-steps"></a>Sonraki adımlar

Karşıya yüklenen varlıklarınızı artık kodlayabilirsiniz. Daha fazla bilgi için bkz. [Varlıkları kodlama](media-services-portal-encode.md).

Yapılandırılmış kapsayıcıya gelen dosyaya göre bir kodlama işi tetiklemek için Azure İşlevleri’ni de kullanabilirsiniz. Daha fazla bilgi için [bu örneğe](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/ ) bakın.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Sonraki adım
Artık bir varlığı Medya Hizmetleri'ne yüklediğinize göre, [Medya İşlemci][How to Get a Media Processor] makalesini Nasıl Elde Edilir'e gidin.

[How to Get a Media Processor]: media-services-get-media-processor.md

