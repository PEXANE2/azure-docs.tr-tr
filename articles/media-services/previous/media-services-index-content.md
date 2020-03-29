---
title: Azure Media Indexer ile Medya Dosyalarını Dizine Ekleme
description: Azure Media Indexer, medya dosyalarınızın içeriğini aranabilir hale getirmenize ve kapalı altyazı ve anahtar kelimeler için tam metin transkript oluşturmanıza olanak tanır. Bu konu, Media Indexer'ın nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Asolanki
manager: femila
editor: ''
ms.assetid: 827a56b2-58a5-4044-8d5c-3e5356488271
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/22/2019
ms.author: juliako
ms.reviewer: johndeu
ms.openlocfilehash: 7ccc2d5956b44a8cd85f19e0905539c32f58bc5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164007"
---
# <a name="indexing-media-files-with-azure-media-indexer"></a>Azure Media Indexer ile Medya Dosyalarını Dizine Ekleme

> [!NOTE]
> **Azure Media Indexer** medya işlemcisi kullanımdan kaldırılacak. Emeklilik tarihleri için bu [eski bileşenler](legacy-components.md) konusuna bakın. [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) bu eski medya işlemcisinin yerini alır. Daha fazla bilgi için azure [media indexer ve Azure Media Indexer 2'den Azure Media Services Video Indexer'a geçir'e](migrate-indexer-v1-v2.md)bakın.

Azure Media Indexer, medya dosyalarınızın içeriğini aranabilir hale getirmenize ve kapalı altyazı ve anahtar kelimeler için tam metin transkript oluşturmanıza olanak tanır. Bir ortam dosyasini veya birden çok medya dosyalarını toplu olarak işleyebilirsiniz.  

İçeriği dizine ekrirken, net konuşma (arka plan müziği, gürültü, efektler veya mikrofon okuması olmadan) ortam dosyalarını kullandığınızdan emin olun. Uygun içeriğe örnek olarak şunlar verilebilir: kaydedilmiş toplantılar, konferanslar veya sunular. Aşağıdaki içerik dizin oluşturma için uygun olmayabilir: filmler, TV şovları, karışık ses ve ses efektleri olan her şey, arka plan gürültüsü (tıs) ile kötü kaydedilmiş içerik.

Dizin oluşturma işi aşağıdaki çıktıları oluşturabilir:

* Aşağıdaki biçimlerde kapalı başlık dosyaları: **TTML**ve **WebVTT**.
  
    Kapalı altyazı dosyaları, kaynak videodaki konuşmanın ne kadar tanınabilir olduğuna bağlı olarak bir dizin oluşturma işini puanlayan Tanınabilirlik adlı bir etiket içerir.  Kullanılabilirlik için çıktı dosyalarını taramak için Tanınabilirlik değerini kullanabilirsiniz. Düşük puan, ses kalitesi nedeniyle kötü dizin oluşturma sonuçları anlamına gelir.
* Anahtar kelime dosyası (XML).

Bu makalede, **bir varlığı dizine dizin** ve **Dizin birden çok dosya**için dizin işleri oluşturmak için nasıl gösterir.

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Dizin oluşturma görevleri için yapılandırma ve bildirim dosyalarını kullanma
Bir görev yapılandırması kullanarak dizin oluşturma görevleriniz için daha fazla ayrıntı belirtebilirsiniz. Örneğin, medya dosyanız için hangi meta verileri kullanacağınızı belirtebilirsiniz. Bu meta veriler dil altyapısı tarafından kelime dağarcığını genişletmek için kullanılır ve konuşma tanıma doğruluğunu büyük ölçüde artırır.  Ayrıca istediğiniz çıktı dosyalarını belirtebilirsiniz.

Ayrıca, bir bildirim dosyası kullanarak aynı anda birden çok ortam dosyalarını işleyebilirsiniz.

Daha fazla bilgi için [Azure Media Indexer için Görev ÖnKümesi'ne](https://msdn.microsoft.com/library/dn783454.aspx)bakın.

## <a name="index-an-asset"></a>Bir varlığı dizine dizine
Aşağıdaki yöntem, bir ortam dosyasını varlık olarak yükler ve varlığı dizine ekecek bir iş oluşturur.

Yapılandırma dosyası belirtilmemişse, ortam dosyası tüm varsayılan ayarlarla birlikte dizine işlenir.

```csharp
    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
```

<!-- __ -->
### <a name="output-files"></a><a id="output_files"></a>Çıktı dosyaları
Varsayılan olarak, dizin oluşturma işi aşağıdaki çıktı dosyalarını oluşturur. Dosyalar ilk çıktı varlığında depolanır.

Birden fazla giriş ortamı dosyası olduğunda, Indexer iş çıkışları için 'JobResult.txt' adlı bir bildirim dosyası oluşturur. Her giriş ortamı dosyası için, ortaya çıkan TTML, WebVTT ve anahtar kelime dosyaları sırayla numaralandırılır ve "Diğer Ad" kullanılarak adlandırılır.

| Dosya adı | Açıklama |
| --- | --- |
| **InputFileName.ttml**<br/>**InputFileName.vtt** |TTML ve WebVTT formatlarında Kapalı Resim Yazısı (CC) dosyaları.<br/><br/>İşitme engelli kişiler için ses ve video dosyalarını erişilebilir hale getirmek için kullanılabilirler.<br/><br/>Kapalı Resim Yazısı dosyaları, kaynak videodaki konuşmanın ne kadar tanınabilir olduğuna bağlı olarak bir dizin oluşturma işini puanlayan <b>Tanıma</b> adlı bir etiket içerir.  Kullanılabilirlik için çıktı dosyalarını taramak için <b>Tanınabilirlik</b> değerini kullanabilirsiniz. Düşük puan, ses kalitesi nedeniyle kötü dizin oluşturma sonuçları anlamına gelir. |
| **InputFileName.kw.xml<br/>InputFileName.info** |Anahtar kelime ve bilgi dosyaları. <br/><br/>Anahtar kelime dosyası, sıklık ve mahsup bilgileriyle konuşma içeriğinden çıkarılan anahtar kelimeleri içeren bir XML dosyasıdır. <br/><br/>Bilgi dosyası, tanınan her terim hakkında ayrıntılı bilgiler içeren düz metin dosyasıdır. İlk satır özeldir ve Tanınabilirlik puanını içerir. Sonraki her satır aşağıdaki verilerin sekmeyle ayrılmış bir listesidir: başlangıç zamanı, bitiş saati, sözcük/tümcecik, güven. Süresaniye cinsinden verilir ve güven 0-1'den bir sayı olarak verilir. <br/><br/>Örnek satır: "1,20 1,45 kelime 0,67" <br/><br/>Bu dosyalar, konuşma analizi yapmak veya medya dosyalarını daha bulunabilir hale getirmek ve hatta daha alakalı reklamlar sunmak için kullanılmak üzere Bing, Google veya Microsoft SharePoint gibi arama motorlarına maruz kalmak gibi çeşitli amaçlariçin kullanılabilir. |
| **İş Sonucu.txt** |Çıktı bildirimi, yalnızca aşağıdaki bilgileri içeren birden çok dosyayı dizine ekrirken mevcut:<br/><br/><table border="1"><tr><th>InputFile</th><th>Diğer ad</th><th>Ortam Uzunluğu</th><th>Hata</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/> |

Tüm giriş ortam dosyaları başarıyla dizine eklenmez, dizin oluşturma işi hata kodu 4000 ile başarısız olur. Daha fazla bilgi için [Hata kodlarına](#error_codes)bakın.

## <a name="index-multiple-files"></a>Birden çok dosyayı dizine ekleme
Aşağıdaki yöntem, birden çok ortam dosyasını varlık olarak yükler ve tüm bu dosyaları toplu olarak dizine dizine ekecek bir iş oluşturur.

".lst" uzantılı bir manifesto dosyası oluşturulur ve varlığa yüklenir. Bildirim dosyası, tüm varlık dosyalarının listesini içerir. Daha fazla bilgi için [Azure Media Indexer için Görev ÖnKümesi'ne](https://msdn.microsoft.com/library/dn783454.aspx)bakın.

```csharp
    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }
```

### <a name="partially-succeeded-job"></a>Kısmen Başarılı İş
Tüm giriş ortam dosyaları başarıyla dizine eklenmez, dizin oluşturma işi hata kodu 4000 ile başarısız olur. Daha fazla bilgi için [Hata kodlarına](#error_codes)bakın.

Aynı çıktılar (başarılı işler gibi) oluşturulur. Hata sütundeğerlerine göre hangi giriş dosyalarının başarısız olduğunu bulmak için çıktı bildirimi dosyasına başvurabilirsiniz. Başarısız olan giriş dosyaları için, ortaya çıkan TTML, WebVTT ve anahtar kelime dosyaları oluşturulmayacak.

### <a name="task-preset-for-azure-media-indexer"></a><a id="preset"></a>Azure Media Indexer için Görev Ön Kümesi
Azure Media Indexer'ın işlemesi, görevin yanında isteğe bağlı bir görev önceden ayarlanarak özelleştirilebilir.  Aşağıdaki bu yapılandırma xml biçimini açıklar.

| Adı | Gerektirme | Açıklama |
| --- | --- | --- |
| **Giriş** |yanlış |Dizinlemek istediğiniz varlık dosyası(lar).</p><p>Azure Media Indexer aşağıdaki ortam dosya biçimlerini destekler: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>**Giriş** öğesinin **adı** veya **liste** özniteliği (aşağıda gösterildiği gibi) dosya adını (ler) belirtebilirsiniz. Hangi varlık dosyasının dizin ekinine belirtmezseniz, birincil dosya seçilir. Birincil kıymet dosyası ayarlanmazsa, giriş kıymetindeki ilk dosya dizine işlenir.</p><p>Varlık dosya adını açıkça belirtmek için şunları yapın:<br/>`<input name="TestFile.wmv">`<br/><br/>Aynı anda birden çok varlık dosyasını (en fazla 10 dosya) dizine ekebilirsiniz. Bunu yapmak için:<br/><br/><ol class="ordered"><li><p>Bir metin dosyası (bildirim dosyası) oluşturun ve bir .lst uzantısı verin. </p></li><li><p>Bu bildirim dosyasına giriş kıymetinizdeki tüm varlık dosya adlarının listesini ekleyin. </p></li><li><p>Bildirim dosyasını kıymete ekleyin (yükleyin).  </p></li><li><p>Girişin liste özniteliğinde bildirim dosyasının adını belirtin.<br/>`<input list="input.lst">`</li></ol><br/><br/>Not: Bildirim dosyasına 10'dan fazla dosya eklerseniz, dizin oluşturma işi 2006 hata koduyla başarısız olur. |
| **Meta veri** |yanlış |Kelime Uyarlaması için kullanılan belirtilen varlık dosyası(lar) için meta veriler.  Uygun adlar gibi standart olmayan sözcük sözcükleri tanımak için Indexer hazırlamak yararlıdır.<br/>`<metadata key="..." value="..."/>` <br/><br/>Önceden tanımlanmış **anahtarlar**için **değerler** sağlayabilirsiniz. Şu anda aşağıdaki tuşlar desteklenir:<br/><br/>"başlık" ve "açıklama" - işiniz için dil modeli çimdik ve konuşma tanıma doğruluğunu artırmak için kelime uyarlaması için kullanılır.  Değerler tohumu Internet, Dizin oluşturma göreviniz süresince iç sözlüğü genişletmek için içeriğini kullanarak bağlamsal olarak alakalı metin belgelerini bulmak için arama lar yapmaktadır.<br/>`<metadata key="title" value="[Title of the media file]" />`<br/>`<metadata key="description" value="[Description of the media file] />"` |
| **Özellik** <br/><br/> Sürüm 1.2'de eklendi. Şu anda desteklenen tek özellik konuşma tanıma ("ASR"). |yanlış |Konuşma Tanıma özelliği aşağıdaki ayarlar ayarı tuşlarına sahiptir:<table><tr><th><p>Anahtar</p></th>        <th><p>Açıklama</p></th><th><p>Örnek değer</p></th></tr><tr><td><p>Dil</p></td><td><p>Multimedya dosyasında tanınacak doğal dil.</p></td><td><p>İngilizce, İspanyolca</p></td></tr><tr><td><p>Resim YazısıFormatlar</p></td><td><p>istenilen çıktı altyazı biçimlerinin yarı sütunlu ayrılmış listesi (varsa)</p></td><td><p>ttml;webvtt</p></td></tr><tr><td><p></p></td><td><p> </p></td><td><p>Doğru; False</p></td></tr><tr><td><p>Anahtar Kelimeler Oluşturma</p></td><td><p>Bir anahtar kelime XML dosyası gerekli olup olmadığını belirten bir boolean bayrağı.</p></td><td><p>Doğru; False. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Tam altyazıları zorlayıp zorlamayacağına (güven düzeyine bakılmaksızın) belirten bir boolean bayrağı.  </p><p>Varsayılan değer yanlıştır, bu durumda %50'den az güven düzeyine sahip sözcük ve tümcecikler son altyazı çıktılarından çıkarılır ve elipslerle değiştirilir ("...).  Elipsler resim yazısı kalite kontrolü ve denetimi için yararlıdır.</p></td><td><p>Doğru; False. </p></td></tr></table> |

### <a name="error-codes"></a><a id="error_codes"></a>Hata kodları
Bir hata durumunda, Azure Media Indexer aşağıdaki hata kodlarından birini geri bildirmelidir:

| Kod | Adı | Olası Nedenler |
| --- | --- | --- |
| 2000 |Geçersiz yapılandırma |Geçersiz yapılandırma |
| 2001 |Geçersiz giriş varlıkları |Eksik giriş varlıkları veya boş varlık. |
| 2002 |Geçersiz bildirim |Manifest boş veya manifesto geçersiz öğeleri içerir. |
| 2003 |Ortam dosyasını karşıdan yükleyemedi |Bildirim dosyasındaki geçersiz URL. |
| 2004 |Desteklenmeyen iletişim kuralı |Ortam URL protokolü desteklenmez. |
| 2005 |Desteklenmeyen dosya türü |Giriş ortamı dosya türü desteklenmez. |
| 2006 |Çok fazla giriş dosyası |Giriş bildiriminde 10'dan fazla dosya vardır. |
| 3000 |Ortam dosyasının şifresini çözemedi |Desteklenmeyen ortam codec <br/>or<br/> Bozuk ortam dosyası <br/>or<br/> Giriş ortamlarında ses akışı yok. |
| 4000 |Toplu iş dizini oluşturma kısmen başarılı |Bazı giriş ortam dosyaları dizine alınamadı. Daha fazla bilgi için <a href="#output_files">Çıktı dosyalarına</a>bakın. |
| diğer |İç hatalar |Lütfen destek ekibiyle iletişime geçin. indexer@microsoft.com |

## <a name="supported-languages"></a><a id="supported_languages"></a>Desteklenen Diller
Şu anda İngilizce ve İspanyolca dilleri desteklenir.  

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Medya Hizmetleri Analizine Genel Bakış](media-services-analytics-overview.md)

[Azure Media Indexer 2 Önizlemesi ile Medya Dosyalarını Dizine Ekleme](media-services-process-content-with-indexer2.md)

