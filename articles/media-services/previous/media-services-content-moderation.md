---
title: Olası yetişkin ve kcy içeriğini algılamak için Azure Medya Content Moderator kullanma | Microsoft Docs
description: Azure Media Content Moderator medya işlemcisi, videolardaki olası yetişkin ve yoğun içeriğin algılanmasına yardımcı olur.
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
ms.custom: devx-track-csharp
ms.openlocfilehash: 395d2239970e7efbf6973d6262df0e049306584b
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89266706"
---
# <a name="use-azure-media-content-moderator-to-detect-possible-adult-and-racy-content"></a>Olası yetişkin ve kcy içeriğini algılamak için Azure Medya Content Moderator kullanma

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!NOTE]
> **Azure media Content moderator** medya işlemcisi kullanımdan kaldırılacak. Kullanımdan kaldırma tarihi için, [eski bileşenler](legacy-components.md) konusuna bakın.

## <a name="overview"></a>Genel Bakış
**Azure media Content moderator** medya IŞLEMCISI (MP), videolarınız için makine destekli denetleme kullanmanıza olanak sağlar. Örneğin videolardaki yetişkinlere yönelik veya müstehcen içerikleri tespit edip belirlenen içeriklerin moderasyon ekibiniz tarafından gözden geçirilmesini isteyebilirsiniz.

**Azure Media Content moderator** MP Şu anda önizleme aşamasındadır.

Bu makale,  **Azure medya Content moderator** ilgili ayrıntıları verir ve .net IÇIN Media Services SDK ile nasıl kullanacağınızı gösterir.

## <a name="content-moderator-input-files"></a>Content Moderator giriş dosyaları
Video dosyaları. Şu anda şu biçimler desteklenir: MP4, MOV ve WMV.

## <a name="content-moderator-output-files"></a>Content Moderator çıktı dosyaları
JSON biçimindeki aracılı çıkış, otomatik olarak algılanan görüntüleri ve ana kareleri içerir. Ana kareler, olası yetişkin veya kcy içeriği için güven puanları ile döndürülür. Ayrıca, bir gözden geçirmeyi önerip önerilmeyeceğini belirten bir Boole bayrağı da içerir. Öneriyi gözden geçirme işareti, yetişkinlere ve kcy puanlarını iç eşiklere göre atanır.

## <a name="elements-of-the-output-json-file"></a>Çıktı JSON dosyasının öğeleri

İş, algılanan görüntüleri ve ana kareleri ve yetişkinlere yönelik içerik içerip içermediğini içeren bir JSON çıkış dosyası oluşturur.

JSON çıktısı aşağıdaki öğeleri içerir:

### <a name="root-json-elements"></a>Kök JSON öğeleri

| Öğe | Açıklama |
| --- | --- |
| sürüm |Content Moderator sürümü. |
| zaman |Videonun saniye başına "ticks" değeri. |
| uzaklık |Zaman damgası için saat farkıdır. Video API 'Lerinin 1,0 sürümünde bu değer her zaman 0 olur. Bu değer gelecekte değişebilir. |
| kare hızı değerini |Videodaki saniye başına kare hızı. |
| genişlik |Çıkış video çerçevesinin piksel cinsinden genişliği.|
| boy |Çıkış video çerçevesinin piksel cinsinden yüksekliği.|
| Toplam süre |Giriş videosunun süresi "ticks" cinsinden. |
| [Eşleştir](#fragments-json-elements) |Meta veriler parçalar adlı farklı kesimlere bölünmüştür. Her parça, başlatma, süre, Aralık numarası ve olay (ler) içeren otomatik olarak algılanan bir çekidir. |

### <a name="fragments-json-elements"></a>Parçacık JSON öğeleri

|Öğe|Açıklama|
|---|---|
| start |"Ticks" içindeki ilk olayın başlangıç saati. |
| süre |Parçanın uzunluğu, "ticks." |
| interval |"Ticks" içinde, parça içindeki her bir olay girişinin aralığı. |
| [olayları](#events-json-elements) |Her olay bir klibi temsil eder ve her klip, algılanan ve bu süre içinde izlenen ana kareleri içerir. Bu, bir dizi olaydır. Dış dizi bir zaman aralığını temsil eder. İç dizi belirtilen noktada gerçekleşen 0 veya daha fazla olaydan oluşur.|

### <a name="events-json-elements"></a>Olaylar JSON öğeleri

|Öğe|Açıklama|
|---|---|
| Gözden geçirmeyi öneririz | `true` ya da `false` **AdultScore** veya **oycyscore** 'ın iç eşikleri aşdığına bağlı olarak. |
| adultScore | 0,00 ile 0,99 arasında bir ölçekte olası yetişkinlere yönelik içerik için güvenirlik puanı. |
| Oycyscore | 0,00 ile 0,99 arasında bir ölçekte olabilecek olası bir içerik için güvenirlik puanı. |
| dizin | İlk kare dizininden son kare dizinine kadar bir ölçekte çerçevenin dizini. |
| timestamp | "Ticks" içindeki çerçevenin konumu. |
| Shotındex | Üst görüntü dizini. |


## <a name="content-moderation-quickstart-and-sample-output"></a>İçerik denetleme hızlı başlangıç ve örnek çıkış

### <a name="task-configuration-preset"></a>Görev yapılandırması (önceden ayarlanmış)
**Azure medya Content moderator**bir görev oluştururken, bir yapılandırma ön ayarı belirtmeniz gerekir. Aşağıdaki yapılandırma ön ayarı yalnızca içerik denetlemesi içindir.

```json
{
    "version":"2.0"
}
```

### <a name="net-code-sample"></a>.NET kod örneği

Aşağıdaki .NET kod örneği, bir Content Moderator işini çalıştırmak için .NET SDK Media Services kullanır. Bir medya Hizmetleri varlığını, aracılık yapılacak videoyu içeren giriş olarak alır.
Tam kaynak kodu ve Visual Studio projesi için [Content moderator video hızlı](../../cognitive-services/Content-Moderator/video-moderation-api.md) başlangıcı ' na bakın.


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
```

Tam kaynak kodu ve Visual Studio projesi için [Content moderator video hızlı](../../cognitive-services/Content-Moderator/video-moderation-api.md)başlangıcını inceleyin.

### <a name="json-output"></a>JSON çıkışı

Content Moderator JSON çıkışının aşağıdaki örneği kesildi.

> [!NOTE]
> Saniye cinsinden bir ana karenin konumu = zaman damgası/zaman ölçeği

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

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Media Services Analytics genel bakışı](media-services-analytics-overview.md)

[Azure Media Analytics gösterileri](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

## <a name="next-steps"></a>Sonraki adımlar

Content Moderator [video denetleme ve İnceleme çözümü](../../cognitive-services/Content-Moderator/video-moderation-human-review.md)hakkında daha fazla bilgi edinin.

[Video denetleme hızlı](../../cognitive-services/Content-Moderator/video-moderation-api.md)başlangıcı ' ndan tam kaynak kodunu ve Visual Studio projesini alın. 

Aracılı çıktıınızdan [video İncelemeleri](../../cognitive-services/Content-Moderator/video-reviews-quickstart-dotnet.md) ve .net 'teki [Orta yazı betikleri](../../cognitive-services/Content-Moderator/video-transcript-reviews-quickstart-dotnet.md) oluşturmayı öğrenin.

Ayrıntılı .NET [video denetlemesi ve gözden geçirme öğreticisine](../../cognitive-services/Content-Moderator/video-transcript-moderation-review-tutorial-dotnet.md)göz atın. 
