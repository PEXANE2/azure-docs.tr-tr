---
title: Olası yetişkinlere uygun ve müstehcen içeriği algılamak için Azure Medya İçerik Moderatörü'ne (Azure Medya İçerik Moderatörü) kullanın | Microsoft Dokümanlar
description: Azure Media Content Moderator medya işlemcisi, videolardaki olası yetişkinlere uygun ve müstehcen içeriğin algılanmasına yardımcı olur.
services: media-services
documentationcenter: ''
author: sanjeev3
manager: mikemcca
editor: ''
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: 83fe7867a3128ac82597c028452863a1ad681ace
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77914341"
---
# <a name="use-azure-media-content-moderator-to-detect-possible-adult-and-racy-content"></a>Olası yetişkinlere uygun ve müstehcen içeriği algılamak için Azure Medya İçerik Moderatörü'ne kullanın 

> [!NOTE]
> **Azure Medya İçeriği Moderatör medya** işlemcisi kullanımdan kaldırılacak. Emeklilik tarihi [için, eski bileşenler](legacy-components.md) konusuna bakın.

## <a name="overview"></a>Genel Bakış
**Azure Media Content Moderator** medya işlemcisi (MP), videolarınız için makine destekli ılımlılık kullanmanıza olanak tanır. Örneğin videolardaki yetişkinlere yönelik veya müstehcen içerikleri tespit edip belirlenen içeriklerin moderasyon ekibiniz tarafından gözden geçirilmesini isteyebilirsiniz.

**Azure Medya İçerik Moderatörü** MP şu anda Önizleme'de.

Bu makalede, **Azure Medya İçerik Moderatörü** hakkında ayrıntılı bilgi verilmiştir ve .NET için Medya Hizmetleri SDK ile nasıl kullanılacağı nı gösterir.

## <a name="content-moderator-input-files"></a>İçerik Moderatör giriş dosyaları
Video dosyaları. Şu anda, aşağıdaki biçimleri desteklenir: MP4, MOV ve WMV.

## <a name="content-moderator-output-files"></a>İçerik Moderatör çıktı dosyaları
JSON formatında ılımlı çıkış otomatik olarak algılanan çekim ve anahtar çerçeveleri içerir. Anahtar kareler, olası yetişkinlere uygun veya müstehcen içerik için güven puanlarıyla döndürülür. Ayrıca, incelemenin önerilip önerilmediğini belirten bir boolean bayrağı da içerir. İnceleme önerisi bayrağı, yetişkinlere uygun ve müstehcen puanların iç eşiklerine göre değerler atanır.

## <a name="elements-of-the-output-json-file"></a>Çıktı JSON dosyasının öğeleri

İş, algılanan çekimler ve anahtar kareler ve bunlar yetişkin veya müstehcen içerik içerip içermediği hakkında meta veriler içeren bir JSON çıktı dosyası üretir.

JSON çıktısı aşağıdaki öğeleri içerir:

### <a name="root-json-elements"></a>Kök JSON elemanları

| Öğe | Açıklama |
| --- | --- |
| version |İçerik Moderatör sürümü. |
| Zaman ölçeği |Videonun saniyesinde "Keneler". |
| uzaklık |Zaman damgası için saat farkıdır. Video API'lerinin sürüm 1.0'ında bu değer her zaman 0 olacaktır. Bu değer gelecekte değişebilir. |
| Framerate |Videodaki saniye başına kare hızı. |
| genişlik |Çıkış video çerçevesinin piksel genişliği.|
| yükseklik |Çıkış video çerçevesinin piksel yüksekliği.|
| toplamSüre |Giriş videosunun süresi, "keneler" olarak adlandırılır. |
| [Parça](#fragments-json-elements) |Meta veriler, parça adı verilen farklı bölümlere ayrılır. Her parça, başlangıç, süre, aralık numarası ve olay(lar) içeren otomatik olarak algılanan bir çekimdir. |

### <a name="fragments-json-elements"></a>Parçaları JSON elemanları

|Öğe|Açıklama|
|---|---|
| start |"Keneler"deki ilk olayın başlangıç saati. |
| süre |Parçanın uzunluğu, "keneler"de. |
| interval |Parça içindeki her olay girişinin aralığı , "kene" olarak adlandırılır. |
| [Olay](#events-json-elements) |Her olay bir klibi temsil eder ve her klip, bu süre içinde algılanan ve izlenen anahtar kareleri içerir. Bu bir dizi olay. Dış dizi bir zaman aralığını temsil eder. İç dizi belirtilen noktada gerçekleşen 0 veya daha fazla olaydan oluşur.|

### <a name="events-json-elements"></a>Olaylar JSON elemanları

|Öğe|Açıklama|
|---|---|
| yorumTavsiye edilen | `true`veya `false` **adultScore** veya **racyScore'un** iç eşikleri aşıp aşmadığına bağlı olarak. |
| adultPuan | 0,00 ile 0,99 arasında bir ölçekte olası yetişkinlere uygun içerik için güven puanı. |
| racyScore | 0,00 ile 0,99 arasında bir ölçekte olası müstehcen içerik için güven puanı. |
| dizin | ilk kare dizini son kare dizini için bir ölçekte çerçeve dizin. |
| timestamp | "Keneler"deki çerçevenin konumu. |
| shotIndex | Üst atışin dizini. |


## <a name="content-moderation-quickstart-and-sample-output"></a>İçerik Moderasyonu hızlı başlatma ve örnek çıktı

### <a name="task-configuration-preset"></a>Görev yapılandırması (önceden ayarlanmış)
**Azure Medya İçerik Yöneticisi**ile bir görev oluştururken, yapılandırma ön ayarlarını belirtmeniz gerekir. Aşağıdaki yapılandırma ön kümesi yalnızca içerik moderasyonu içindir.

    {
      "version":"2.0"
    }

### <a name="net-code-sample"></a>.NET kod örneği

Aşağıdaki .NET kod örneği, İçerik Moderatörü işini çalıştırmak için Medya Hizmetleri .NET SDK'yı kullanır. Bir medya hizmetleri Varlık'ı, videoyu içeren giriş olarak denetlenecek şekilde alır.
Tam kaynak kodu ve Visual Studio projesi için [İçerik Moderatör video hızlı başlatış](../../cognitive-services/Content-Moderator/video-moderation-api.md) bakın.


```csharp
    /// <summary>
    /// Run the Content Moderator job on the designated Asset from local file or blob storage
    /// </summary>
    /// <param name="asset"></param>
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

For the full source code and the Visual Studio project, check out the [Content Moderator video quickstart](../../cognitive-services/Content-Moderator/video-moderation-api.md).

### JSON output

The following example of a Content Moderator JSON output was truncated.

> [!NOTE]
> Location of a keyframe in seconds = timestamp/timescale

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

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Medya Hizmetleri Analizine Genel Bakış](media-services-analytics-overview.md)

[Azure Medya Analizi demoları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

## <a name="next-steps"></a>Sonraki adımlar

İçerik Moderatör'ün [video moderatörü ve inceleme çözümü](../../cognitive-services/Content-Moderator/video-moderation-human-review.md)hakkında daha fazla bilgi edinin.

[Video moderasyon quickstart](../../cognitive-services/Content-Moderator/video-moderation-api.md)tam kaynak kodu ve Visual Studio proje alın. 

.NET'teki orta [düzeydeki transkriptleri](../../cognitive-services/Content-Moderator/video-transcript-reviews-quickstart-dotnet.md) ve orta düzeydeki çıktınızdan [video incelemelerini](../../cognitive-services/Content-Moderator/video-reviews-quickstart-dotnet.md) nasıl oluşturacağınızı öğrenin.

Ayrıntılı .NET [video moderasyonu ve inceleme öğretici](../../cognitive-services/Content-Moderator/video-transcript-moderation-review-tutorial-dotnet.md)göz atın. 
