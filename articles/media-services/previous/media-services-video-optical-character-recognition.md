---
title: Azure Media Analytics OCR ile metni dijitalleştirin | Microsoft Dokümanlar
description: Azure Media Analytics OCR (optik karakter tanıma), video dosyalarındaki metin içeriğini düzenlenebilir, aranabilir dijital metne dönüştürmenizi sağlar.  Bu, medyanızın video sinyalinden anlamlı meta verilerin çıkarılmasını otomatikleştirmenize olanak tanır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 307c196e-3a50-4f4b-b982-51585448ffc6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 11889bd6df0bcc9564c17fdaacc333df1d418660
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77918351"
---
# <a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Video dosyalarındaki metin içeriğini dijital metne dönüştürmek için Azure Media Analytics'i kullanma  

> [!NOTE]
> **Azure Media OCR** ortam işlemcisi kullanımdan kaldırılacak. Emeklilik tarihi [için, eski bileşenler](legacy-components.md) konusuna bakın.

## <a name="overview"></a>Genel Bakış
Video dosyalarınızdan metin içeriği çıkarmanız ve düzenlenebilir, aranabilir bir dijital metin oluşturmanız gerekiyorsa, Azure Media Analytics OCR (optik karakter tanıma) kullanmanız gerekir. Bu Azure Medya İşlemcisi, video dosyalarınızdaki metin içeriğini algılar ve kullanımınız için metin dosyaları oluşturur. OCR, medyanızın video sinyalinden anlamlı meta verilerin çıkarılmasını otomatikleştirmenizi sağlar.

Bir arama motoruyla birlikte kullanıldığında, medyanızı kolayca metin le dizine ekleyebilir ve içeriğinizin bulunabilirliğini artırabilirsiniz. Bu, bir slayt gösterisi sunumunun video kaydı veya ekran yakalama gibi son derece metinsel videoda son derece yararlıdır. Azure OCR Medya İşlemcisi dijital metin için optimize edilebistir.

**Azure Media OCR** medya işlemcisi şu anda Önizleme'de.

Bu makalede, **Azure Media OCR** hakkında ayrıntılı bilgi verilmiştir ve .NET için Medya Hizmetleri SDK ile nasıl kullanılacağını gösterir. Daha fazla bilgi ve örnekler için [bu bloga](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/)bakın.

## <a name="ocr-input-files"></a>OCR giriş dosyaları
Video dosyaları. Şu anda, aşağıdaki biçimleri desteklenir: MP4, MOV ve WMV.

## <a name="task-configuration"></a>Görev yapılandırması
Görev yapılandırması (önceden ayarlanmış). **Azure Media OCR**ile bir görev oluştururken, JSON veya XML kullanarak bir yapılandırma ön ayarbelirtmelisiniz. 

>[!NOTE]
>OCR altyapısı, her iki yükseklik/genişlikte de geçerli bir giriş olarak yalnızca en az 40 piksel ile maksimum 32.000 piksel arasında bir görüntü bölgesi alır.
>

### <a name="attribute-descriptions"></a>Öznitelik açıklamaları
| Öznitelik adı | Açıklama |
| --- | --- |
|Gelişmiş Çıktı| AdvancedOutput'ü doğru ayarlarsanız, JSON çıktısı her bir sözcük için konumsal veriler içerir (tümceciklere ve bölgelere ek olarak). Bu ayrıntıları görmek istemiyorsanız, bayrağı false olarak ayarlayın. Varsayılan değer false'tur. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/azure-media-ocr-simplified-output/)bakın.|
| Dil |(isteğe bağlı) bakmak için metin dilini açıklar. Aşağıdakilerden biri: AutoDetect (varsayılan), Arapça, ÇinceBasitleştirilmiş, ÇinceGeleneksel, Çek Calısa, Felemenkçe, İngilizce, Fince, Fransızca, Almanca, Yunanca, Macarca, İtalyanca, Japonca, Korece, Norveççe, Lehçe, Portekizce, Romence, Rusça, SırpKiril, Sırplatince, Slovakça, İspanyolca, İsveççe, Türkçe. |
| Metin Yönlendirme |(isteğe bağlı) bakmak için metnin yönünü açıklar.  "Sol" tüm harflerin üst sola doğru işaret anlamına gelir.  Varsayılan metin (bir kitapta bulunabilecek şekilde) "Yukarı" yönelimli olarak adlandırılabilir.  Aşağıdakilerden biri: AutoDetect (varsayılan), Yukarı, Sağ, Aşağı, Sol. |
| Zaman Aralığı |(isteğe bağlı) örnekleme hızını açıklar.  Varsayılan her 1/2 saniyede birdir.<br/>JSON formatı - HH:mm:ss. SSS (varsayılan 00:00:00.500)<br/>XML formatı – W3C XSD süresi ilkel (varsayılan PT0.5) |
| Algılama Bölgeleri |(isteğe bağlı) Metni algılamak için video çerçevesi içindeki bölgeleri belirten Bir dizi DetectRegion nesnesi.<br/>DetectRegion nesnesi aşağıdaki dört tamsayı değerden oluşur:<br/>Sol – sol kenar boşluğundan piksel<br/>Üst – üst kenar boşluğundan pikseller<br/>Genişlik – piksellerde bölgenin genişliği<br/>Yükseklik – piksel lerde bölgenin yüksekliği |

#### <a name="json-preset-example"></a>JSON önceden ayarlanmış örnek

```json
    {
        "Version":1.0, 
        "Options": 
        {
            "AdvancedOutput":"true",
            "Language":"English", 
            "TimeInterval":"00:00:01.5",
            "TextOrientation":"Up",
            "DetectRegions": [
                    {
                       "Left": 10,
                       "Top": 10,
                       "Width": 100,
                       "Height": 50
                    }
             ]
        }
    }
```

#### <a name="xml-preset-example"></a>XML önceden ayarlanmış örnek

```xml
    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""https://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""https://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""https://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <AdvancedOutput>true</AdvancedOutput>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>10</Left>
                   <Top>10</Top>
                   <Width>100</Width>
                   <Height>50</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>
```

## <a name="ocr-output-files"></a>OCR çıktı dosyaları
OCR ortam işlemcisinin çıktısı bir JSON dosyasıdır.

### <a name="elements-of-the-output-json-file"></a>Çıktı JSON dosyasının öğeleri
Video OCR çıkışı, videonuzda bulunan karakterler hakkında zaman segmentli veriler sağlar.  Dil veya oryantasyon gibi öznitelikleri tam olarak çözümlemek le ilgilendiğiniz sözcükleri öğrenmek için kullanabilirsiniz. 

Çıktı aşağıdaki öznitelikleri içerir:

| Öğe | Açıklama |
| --- | --- |
| Timescale |Videonun saniyesi için "kene" |
| Uzaklık |zaman damgaları için zaman mahsup. Video API'lerinin sürüm 1.0'ında bu her zaman 0 olacaktır. |
| Framerate |Videonun saniye kareleri |
| genişlik |piksellerde video genişliği |
| yükseklik |piksel lerde videoyüksekliği |
| Fragments |meta verilerin parçalandığı zaman tabanlı video parçaları dizisi |
| start |"keneler" içinde bir parçanın başlangıç saati |
| süre |"kene" parçasının uzunluğu |
| interval |verilen parça içindeki her olayın aralığı |
| etkinlikler |bölgeleri içeren dizi |
| region |algılanan sözcükleri veya tümcecikleri temsil eden nesne |
| language |bir bölge içinde algılanan metnin dili |
| Yönlendirme |bir bölge içinde algılanan metnin yönlendirmesi |
| satırlar |bir bölge içinde algılanan metin satırları dizisi |
| metin |gerçek metin |

### <a name="json-output-example"></a>JSON çıktı örneği
Aşağıdaki çıktı örneği genel video bilgilerini ve birkaç video parçasını içerir. Her video parçasında, OCR MP tarafından dil ve metin yönlendirmesi ile algılanan her bölgeyi içerir. Bölge ayrıca bu bölgedeki her sözcük satırını satır metni, satırın konumu ve bu satırdaki her sözcük bilgisi (sözcük içeriği, konum ve güven) içerir. Aşağıdaki bir örnektir ve ben satır satırbazı yorumlar koymak.

```json
    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }
```

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program nasıl yapılacağını gösterir:

1. Bir varlık oluşturun ve bir medya dosyasını varlığa yükleyin.
2. OCR yapılandırma/önceden ayarlanmış dosya içeren bir iş oluşturun.
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

namespace OCR
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

            // Run the OCR job.
            var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                        @"C:\supportFiles\OCR\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
        }

        static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My OCR Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My OCR Job");

            // Get a reference to Azure Media OCR.
            string MediaProcessorName = "Azure Media OCR";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My OCR Task",
                processor,
                configuration,
                TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);

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

