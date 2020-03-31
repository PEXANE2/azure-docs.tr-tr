---
title: Azure Medya Analitiği ile Hareketleri Algılama | Microsoft Dokümanlar
description: Azure Medya Hareket Dedektörü ortam işlemcisi (MP), uzun ve olaysız bir video içinde ilgi çekici bölümleri verimli bir şekilde belirlemenize olanak tanır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.reviewer: milanga
ms.openlocfilehash: f4c021531a4d04bf16e5dbee4172952433f675d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77913013"
---
# <a name="detect-motions-with-azure-media-analytics"></a>Azure Medya Analitiği ile Hareketleri Algılama

> [!NOTE]
> **Azure Media Motion Detector** ortam işlemcisi kullanımdan kaldırılacak. Emeklilik tarihi [için, eski bileşenler](legacy-components.md) konusuna bakın.
 
## <a name="overview"></a>Genel Bakış

**Azure Medya Hareket Dedektörü** ortam işlemcisi (MP), uzun ve olaysız bir video içinde ilgi çekici bölümleri verimli bir şekilde belirlemenize olanak tanır. Hareket algılama, hareketin gerçekleştiği video bölümlerini belirlemek için statik kamera görüntülerinde kullanılabilir. Zaman damgaları ve olayın oluştuğu sınırlayıcı bölge içeren bir meta veri içeren bir JSON dosyası oluşturur.

Güvenlik video beslemelerine yönelik olan bu teknoloji, hareketi ilgili olaylara ve gölgeler ve Aydınlatma değişiklikleri gibi yanlış pozitif lere göre kategorize edebilir. Bu, kamera akışlarından sonsuz alakasız olaylarla spam edilmeden güvenlik uyarıları oluşturmanıza olanak sağlarken, uzun güvenlik videolarından ilgi çekici anları ayıklayabilmenizi sağlar.

**Azure Medya Hareket Dedektörü** MP şu anda Önizleme'de.

Bu **makalede, Azure Media Motion Detector** hakkında ayrıntılı bilgi verir ve .NET için Medya Hizmetleri SDK ile nasıl kullanılacağını gösterir

## <a name="motion-detector-input-files"></a>Hareket Dedektörü giriş dosyaları
Video dosyaları. Şu anda, aşağıdaki biçimleri desteklenir: MP4, MOV ve WMV.

## <a name="task-configuration-preset"></a>Görev yapılandırması (önceden ayarlanmış)
**Azure Media Motion Detector**ile bir görev oluştururken, bir yapılandırma ön ayarı belirtmeniz gerekir. 

### <a name="parameters"></a>Parametreler
Aşağıdaki parametreleri kullanabilirsiniz:

| Adı | Seçenekler | Açıklama | Varsayılan |
| --- | --- | --- | --- |
| sensitivitySeviye |String:'düşük', 'orta', 'yüksek' |Hareketlerin raporlandığı duyarlılık düzeyini ayarlar. Yanlış pozitif sayısını ayarlamak için bunu ayarlayın. |'orta' |
| çerçeveSamplingValue |Pozitif tamsayı |Algoritmanın çalışma sıklığını ayarlar. 1 her kare eşittir, 2 her saniye kare anlamına gelir, ve benzeri. |1 |
| detectLightChange |Boolean:'true', 'false' |Sonuçlarda ışık değişikliklerinin bildirilip bildirilmediğini ayarlar |'Yanlış' |
| mergeTimeThreshold |Xs-time: Hh:mm:ss<br/>Örnek: 00:00:03 |2 olayın birleştirildiği ve 1 olarak raporlandığı hareket olayları arasındaki zaman penceresini belirtir. |00:00:00 |
| algılamaBölgeler |Bir dizi algılama bölgesi:<br/>- Algılama Bölgesi 3 veya daha fazla noktadan oluşan bir dizidir<br/>- Nokta 0'dan 1'e kadar bir x ve y koordinatıdır. |Kullanılacak çokgen algılama bölgelerinin listesini açıklar.<br/>Sonuçlar, ilki 'id':0 olmak üzere, bölgelerle birlikte kimlik olarak bildirilir |Tüm çerçeveyi kapsayan tek bölge. |

### <a name="json-example"></a>JSON örneği

```json
    {
      "version": "1.0",
      "options": {
        "sensitivityLevel": "medium",
        "frameSamplingValue": 1,
        "detectLightChange": "False",
        "mergeTimeThreshold":
        "00:00:02",
        "detectionZones": [
          [
            {"x": 0, "y": 0},
            {"x": 0.5, "y": 0},
            {"x": 0, "y": 1}
           ],
          [
            {"x": 0.3, "y": 0.3},
            {"x": 0.55, "y": 0.3},
            {"x": 0.8, "y": 0.3},
            {"x": 0.8, "y": 0.55},
            {"x": 0.8, "y": 0.8},
            {"x": 0.55, "y": 0.8},
            {"x": 0.3, "y": 0.8},
            {"x": 0.3, "y": 0.55}
          ]
        ]
      }
    }
```

## <a name="motion-detector-output-files"></a>Hareket Dedektörü çıkış dosyaları
Bir hareket algılama işi, video içindeki hareket uyarılarını ve kategorilerini açıklayan çıktı varlığındaki bir JSON dosyasını döndürür. Dosya, videoda algılanan hareket süresi ve süresi hakkında bilgi içerir.

Hareket Dedektörü API sabit bir arka plan video (örneğin, bir gözetim video hareket nesneleri olduğunda göstergeler sağlar). Hareket Dedektörü, aydınlatma ve gölge değişiklikleri gibi yanlış alarmları azaltmak için eğitilir. Algoritmaların geçerli sınırlamaları gece görüş videoları, yarı saydam nesneler ve küçük nesneleri içerir.

### <a name="elements-of-the-output-json-file"></a><a id="output_elements"></a>Çıktı JSON dosyasının öğeleri
> [!NOTE]
> En son sürümde, Output JSON biçimi değişti ve bazı müşteriler için bir kırılma değişikliği temsil edebilir.
> 
> 

Aşağıdaki tabloda çıktı JSON dosyasının öğeleri açıklanmaktadır.

| Öğe | Açıklama |
| --- | --- |
| version |Bu, Video API sürümüanlamına gelir. Geçerli sürüm 2'dir. |
| Zaman ölçeği |Videonun saniyesinde "Keneler". |
| uzaklık |"Keneler"deki zaman damgaları için zaman mahsupları. Video API'lerinin sürüm 1.0'ında bu her zaman 0 olacaktır. Desteklediğimiz gelecek senaryolarda bu değer değişebilir. |
| Framerate |Videodaki saniye başına kare hızı. |
| genişlik, yükseklik |Piksellerde videonun genişliğini ve yüksekliğini ifade eder. |
| start |"Keneler"deki başlangıç zaman damgası. |
| süre |Olayın uzunluğu, "keneler"de. |
| interval |Olaydaki her giriş aralığı, "kene"de. |
| etkinlikler |Her olay parçası, bu süre içinde algılanan hareketi içerir. |
| type |Geçerli sürümde, bu her zaman genel hareket için '2' dir. Bu etiket, Video API'lerine gelecekteki sürümlerde hareketi kategorilere ayırma esnekliği sağlar. |
| regionId |Yukarıda açıklandığı gibi, bu her zaman bu sürümde 0 olacaktır. Bu etiket, Video API'ye gelecekteki sürümlerde çeşitli bölgelerde hareket bulma esnekliği sağlar. |
| Bölge |Videonuzdaki hareketi önemsediğiniz alanı ifade eder. <br/><br/>-"id" bölge alanını temsil eder – bu sürümde sadece bir tane vardır, ID 0. <br/>-"type" hareket için önemsediğiniz bölgenin şeklini temsil eder. Şu anda, "dikdörtgen" ve "çokgen" desteklenir.<br/> "Dikdörtgen" olarak belirttiyseniz, bölgenin X, Y, Genişlik ve Yükseklik boyutları vardır. X ve Y koordinatları, 0,0 ile 1,0 arasında normalleştirilmiş bir ölçekte bölgenin sol üst XY koordinatlarını temsil eder. Genişlik ve yükseklik, 0,0 ile 1,0 arasında normalleştirilmiş bir ölçekte bölgenin boyutunu temsil ediyor. Geçerli sürümde, X, Y, Genişlik ve Yükseklik her zaman 0, 0 ve 1, 1 olarak sabitlenir. <br/>"Çokgen" olarak belirtirseniz, bölgenin noktalarda boyutları vardır. <br/> |
| Parça |Meta veriler, parça adı verilen farklı bölümlere ayrılır. Her parçada başlangıç, süre, aralık sayısı ve olaylar vardır. Olay olmayan bir parça, bu başlangıç zamanı ve süresi boyunca hiçbir hareketin algılanmadı anlamına gelir. |
| parantez [] |Her parantez olaydaki bir aralığı temsil eder. Bu aralık için boş köşeli ayraçlar hiçbir hareket algılanmadı anlamına gelir. |
| Konum |Olaylar altındaki bu yeni giriş, hareketin oluştuğu konumu listeler. Bu, algılama bölgelerinden daha spesifiktir. |

Aşağıdaki JSON örneği çıktıyı gösterir:

```json
    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
```

## <a name="limitations"></a>Sınırlamalar
* Desteklenen giriş video biçimleri MP4, MOV ve WMV içerir.
* Hareket Algılama sabit arka plan videoları için optimize edin. Algoritma, aydınlatma değişiklikleri ve gölgeler gibi yanlış alarmları azaltmaya odaklanır.
* Bazı hareketler teknik zorluklar nedeniyle tespit edilemeyebilir; örneğin, gece görüş videoları, yarı saydam nesneler ve küçük nesneler.

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program nasıl yapılacağını gösterir:

1. Bir varlık oluşturun ve bir medya dosyasını varlığa yükleyin.
2. Aşağıdaki json önceden ayarını içeren bir yapılandırma dosyasına dayalı bir video hareket algılama görevi içeren bir iş oluşturun: 
   
    ```json
            {
            "Version": "1.0",
            "Options": {
                "SensitivityLevel": "medium",
                "FrameSamplingValue": 1,
                "DetectLightChange": "False",
                "MergeTimeThreshold":
                "00:00:02",
                "DetectionZones": [
                [
                    {"x": 0, "y": 0},
                    {"x": 0.5, "y": 0},
                    {"x": 0, "y": 1}
                ],
                [
                    {"x": 0.3, "y": 0.3},
                    {"x": 0.55, "y": 0.3},
                    {"x": 0.8, "y": 0.3},
                    {"x": 0.8, "y": 0.55},
                    {"x": 0.8, "y": 0.8},
                    {"x": 0.55, "y": 0.8},
                    {"x": 0.3, "y": 0.8},
                    {"x": 0.3, "y": 0.55}
                ]
                ]
            }
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

namespace VideoMotionDetection
{
    class Program
    {
        // Read values from the App.config file.
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

            // Run the VideoMotionDetection job.
            var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                        @"C:\supportFiles\VideoMotionDetection\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
        }

        static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Video Motion Detection Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Video Motion Detection Job");

            // Get a reference to Azure Media Motion Detector.
            string MediaProcessorName = "Azure Media Motion Detector";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Video Motion Detection Output Asset", AssetCreationOptions.None);

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
[Azure Medya Hizmetleri Hareket Dedektörü blogu](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Medya Hizmetleri Analizine Genel Bakış](media-services-analytics-overview.md)

[Azure Medya Analizi demoları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

