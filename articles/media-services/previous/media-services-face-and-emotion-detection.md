---
title: Azure Medya Analitiği ile Yüz ve Duyguları Algılama | Microsoft Dokümanlar
description: Bu konu, Azure Media Analytics ile yüzlerin ve duyguların nasıl algılanış edilebildiğini gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 5ca4692c-23f1-451d-9d82-cbc8bf0fd707
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.reviewer: milanga
ms.openlocfilehash: 2d746167f993438e5fce467365844df2078c08a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919320"
---
# <a name="detect-face-and-emotion-with-azure-media-analytics"></a>Azure Media Analytics ile Yüz ve Duyguları Algılama

> [!NOTE]
> **Azure Media Face Detector** ortam işlemcisi kullanımdan kaldırılacak. Emeklilik tarihi [için, eski bileşenler](legacy-components.md) konusuna bakın.

## <a name="overview"></a>Genel Bakış

**Azure Media Face Detector** ortam işlemcisi (MP), yüz ifadeleri aracılığıyla izleyici katılımını ve tepkisini saymanızı, hareketleri izlemenizi ve hatta ölçmenizi sağlar. Bu hizmet iki özellik içerir: 

* **Yüz algılama**
  
    Yüz algılama, video daki insan yüzlerini bulur ve izler. Birden çok yüz algılanabilir ve daha sonra hareket ederken izlenebilir ve zaman ve konum meta verileri JSON dosyasında döndürülür. İzleme sırasında, kişi ekranda hareket ederken, engellenmiş olsa lar veya kısa bir süre için çerçeveden ayrılsalar bile aynı yüze tutarlı bir kimlik vermeye çalışır.
  
  > [!NOTE]
  > Bu hizmet yüz tanıma gerçekleştirmez. Çerçeveyi terk eden veya çok uzun süre engellenen bir kişiye geri döndüklerinde yeni bir kimlik verilir.
  > 
  > 
* **Duygu algılama**
  
    Duygu Algılama, mutluluk, üzüntü, korku, öfke ve daha fazlası dahil olmak üzere tespit edilen yüzlerden alınan birden fazla duygusal özellik üzerinde analiz sağlayan Yüz Algılama Ortam İşlemcisi'nin isteğe bağlı bir bileşenidir. 

**Azure Media Yüz Dedektörü** MP şu anda Önizleme'de.

Bu makalede, **Azure Media Yüz Dedektörü** hakkında ayrıntılı bilgi verilmiştir ve .NET için Medya Hizmetleri SDK ile nasıl kullanılacağını gösterir.

## <a name="face-detector-input-files"></a>Yüz Dedektörü giriş dosyaları
Video dosyaları. Şu anda, aşağıdaki biçimleri desteklenir: MP4, MOV ve WMV.

## <a name="face-detector-output-files"></a>Yüz Dedektörü çıkış dosyaları
Yüz algılama ve izleme API'si, bir videoda 64'e kadar insan yüzü algılayabilen yüksek hassasiyetli yüz konumu algılama ve izleme sağlar. Frontal yüzler en iyi sonuçları sağlarken, yan yüzler ve küçük yüzler (24x24 pikselden daha az veya eşit) bu kadar doğru olmayabilir.

Algılanan ve izlenen yüzler, görüntüdeki yüzlerin pikseller halindeki konumunu gösteren koordinatlarla (sol, üst, genişlik ve yükseklik) ve o kişinin izini gösteren bir yüz kimliği numarasıyla döndürülür. Face ID numaraları, ön yüzün kaybolduğu veya çerçevede çakıştıldığında olduğu durumlarda sıfırlanma yatkınlığı yla karşı karşıya dır ve bu da bazı kişilerin birden fazla kimlik atanmasına neden olur.

## <a name="elements-of-the-output-json-file"></a><a id="output_elements"></a>Çıktı JSON dosyasının öğeleri

[!INCLUDE [media-services-analytics-output-json](../../../includes/media-services-analytics-output-json.md)]

Yüz Dedektörü parçalanma tekniklerini kullanır (meta verilerin zaman tabanlı parçalar halinde bölünebildiği ve yalnızca ihtiyacınız olanı karşıdan yükleyebileceğiniz) ve bölümlendirme (olayların çok büyük olması durumunda parçalandığı) Bazı basit hesaplamalar, verileri dönüştürmenize yardımcı olabilir. Örneğin, bir olay 6300 'de (kene) olarak, 2997 (keneler/sn) ve 29,97 karelik (kare/sn) kare hızıyla başladıysa, o zaman:

* Başlangıç/Ölçek = 2,1 saniye
* Saniye x Framerate = 63 kare

## <a name="face-detection-input-and-output-example"></a>Yüz algılama giriş ve çıkış örneği
### <a name="input-video"></a>Giriş videosu
[Giriş Videosu](https://ampdemo.azureedge.net/azuremediaplayer.html?url=httpss%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Görev yapılandırması (önceden ayarlanmış)
**Azure Media Face Detector**ile bir görev oluştururken, bir yapılandırma ön ayarı belirtmeniz gerekir. Aşağıdaki yapılandırma ön kümesi sadece yüz algılama içindir.

```json
    {
      "version":"1.0",
      "options":{
          "TrackingMode": "Fast"
      }
    }
```

#### <a name="attribute-descriptions"></a>Öznitelik açıklamaları
| Öznitelik adı | Açıklama |
| --- | --- |
| Mod |Hızlı - hızlı işlem hızı, ancak daha az doğru (varsayılan).|

### <a name="json-output"></a>JSON çıktı
JSON çıktısının aşağıdaki örneği kesildi.

```json
    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],
```


## <a name="emotion-detection-input-and-output-example"></a>Duygu algılama giriş ve çıkış örneği
### <a name="input-video"></a>Giriş videosu
[Giriş Videosu](https://ampdemo.azureedge.net/azuremediaplayer.html?url=httpss%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

### <a name="task-configuration-preset"></a>Görev yapılandırması (önceden ayarlanmış)
**Azure Media Face Detector**ile bir görev oluştururken, bir yapılandırma ön ayarı belirtmeniz gerekir. Aşağıdaki yapılandırma önceden ayarlanmış duygu algılama dayalı JSON oluşturmak için belirtir.

```json
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }
```


#### <a name="attribute-descriptions"></a>Öznitelik açıklamaları
| Öznitelik adı | Açıklama |
| --- | --- |
| Mod |Yüzler: Yalnızca yüz algılama.<br/>PerFaceEmotion: Her yüz algılama için duyguları bağımsız olarak döndürün.<br/>AggregateEmotion: Çerçevedeki tüm yüzler için ortalama duygu değerlerini döndürün. |
| AggregateEmotionWindowMs |AggregateEmotion modu seçiliyse kullanın. Her toplam sonucu milisaniye cinsinden üretmek için kullanılan videonun uzunluğunu belirtir. |
| AggregateEmotionIntervalMs |AggregateEmotion modu seçiliyse kullanın. Toplu sonuçlar üretmek için hangi frekansla belirtir. |

#### <a name="aggregate-defaults"></a>Toplam varsayılanlar
Aşağıda toplam pencere ve aralık ayarları için önerilen değerler verilmiştir. AggregateEmotionWindowMs AggregateEmotionIntervalMs daha uzun olmalıdır.

|| Varsayılanlar(lar) | Maks(lar) | Min(ler) |
|--- | --- | --- | --- |
| AggregateEmotionWindowMs |0,5 |2 |0,25|
| AggregateEmotionIntervalMs |0,5 |1 |0,25|

### <a name="json-output"></a>JSON çıktı
Toplam duygu (kesilmiş) için JSON çıkışı:

```json
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
```

## <a name="limitations"></a>Sınırlamalar
* Desteklenen giriş video biçimleri MP4, MOV ve WMV içerir.
* Algılanabilir yüz boyutu aralığı 24x24 ile 2048x2048 piksel arasındadır. Bu aralıktaki yüzler algılanmaz.
* Her video için döndürülen maksimum yüz sayısı 64'tür.
* Bazı yüzler teknik zorluklar nedeniyle tespit edilemeyebilir; örneğin, çok büyük yüz açıları (baş-poz) ve büyük tıkanıklık. Frontal ve yakın ön yüzler en iyi sonuçları verir.

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program nasıl yapılacağını gösterir:

1. Bir varlık oluşturun ve bir medya dosyasını varlığa yükleyin.
2. Aşağıdaki json önceden ayarlanmış içeren bir yapılandırma dosyasıdayalı bir yüz algılama görevi ile bir iş oluşturun: 

    ```json
            {
                "version": "1.0"
            }
    ```
3. Çıktı JSON dosyalarını indirin. 

#### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

#### <a name="example"></a>Örnek

```csharp
using System;
using System.Configuration;
using System.IO;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;
using System.Threading.Tasks;

namespace FaceDetection
{
    class Program
    {
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AMSAADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["AMSRESTAPIEndpoint"];
        private static readonly string _AMSClientId =
            ConfigurationManager.AppSettings["AMSClientId"];
        private static readonly string _AMSClientSecret =
            ConfigurationManager.AppSettings["AMSClientSecret"];

        // Field for service context.
        private static CloudMediaContext _context = null;

        static void Main(string[] args)
        {
            AzureAdTokenCredentials tokenCredentials =
                new AzureAdTokenCredentials(_AADTenantDomain,
                    new AzureAdClientSymmetricKey(_AMSClientId, _AMSClientSecret),
                    AzureEnvironments.AzureCloudEnvironment);

            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);

            // Run the FaceDetection job.
            var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                        @"C:\supportFiles\FaceDetection\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
        }

        static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Face Detection Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Detection Job");

            // Get a reference to Azure Media Face Detector.
            string MediaProcessorName = "Azure Media Face Detector";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Detection Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Detection Output Asset", AssetCreationOptions.None);

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
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));
                return null;
            }

            return job.OutputMediaAssets[0];
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

        static private void StateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);

            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished.");
                    Console.WriteLine();
                    break;
                case JobState.Canceling:
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    Console.WriteLine("Please wait...\n");
                    break;
                case JobState.Canceled:
                case JobState.Error:
                    // Cast sender as a job.
                    IJob job = (IJob)sender;
                    // Display or log error details as needed.
                    // LogJobStop(job.Id);
                    break;
                default:
                    break;
            }
        }
    }
}
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Medya Hizmetleri Analizine Genel Bakış](media-services-analytics-overview.md)

[Azure Medya Analizi demoları](https://amslabs.azurewebsites.net/demos/Analytics.html)

