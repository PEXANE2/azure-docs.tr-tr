---
title: Azure Media Indexer 2 Önizlemesi ile Medya Dosyalarını Dizine Ekleme | Microsoft Dokümanlar
description: Azure Media Indexer, medya dosyalarınızın içeriğini aranabilir hale getirmenize ve kapalı altyazı ve anahtar kelimeler için tam metin transkript oluşturmanıza olanak tanır. Bu konu, Media Indexer 2 Preview'un nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/22/2019
ms.author: juliako
ms.reviewer: adsolank
ms.openlocfilehash: c24218dc116803ca0e0a1f166b7b54b24fc4d5ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78163803"
---
# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Azure Media Indexer 2 Önizlemesi ile Medya Dosyalarını Dizine Ekleme

> [!NOTE]
> **Azure Media Indexer 2** medya işlemcisi kullanımdan kaldırılacak. Emeklilik tarihleri için bu [eski bileşenler](legacy-components.md) konusuna bakın. [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) bu eski medya işlemcisinin yerini alır. Daha fazla bilgi için azure [media indexer ve Azure Media Indexer 2'den Azure Media Services Video Indexer'a geçir'e](migrate-indexer-v1-v2.md)bakın.

**Azure Media Indexer 2 Önizleme** ortam işlemcisi (MP), medya dosyalarını ve içeriğini aranabilir hale getirmenin yanı sıra kapalı altyazı parçaları oluşturmanıza olanak tanır. [Azure Media Indexer'ın](media-services-index-content.md)önceki sürümüyle karşılaştırıldığında, **Azure Media Indexer 2 Preview** daha hızlı dizin oluşturma gerçekleştirir ve daha geniş dil desteği sunar. Desteklenen diller Arasında İngilizce, İspanyolca, Fransızca, Almanca, İtalyanca, Çince (Mandarin, Basitleştirilmiş), Portekizce, Arapça, Rusça ve Japonca bulunmaktadır.

**Azure Media Indexer 2 Preview** MP şu anda Önizleme'de.

Bu makalede, **Azure Media Indexer 2 Preview**ile dizin oluşturma işlerinin nasıl oluşturulacak olduğu gösterilmektedir.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki noktalara dikkat edilmelidir:
 
* Indexer 2, Azure China 21Vianet ve Azure Kamu'da desteklenmez.
* İçeriği dizine ekrirken, çok net bir konuşma olan medya dosyalarını kullandığınızdan emin olun (arka plan müziği, gürültü, efektler veya mikrofon okumaları olmadan). Uygun içeriğe örnek olarak şunlar verilebilir: kaydedilmiş toplantılar, konferanslar veya sunular. Aşağıdaki içerik dizin oluşturma için uygun olmayabilir: filmler, TV şovları, karışık ses ve ses efektleri olan her şey, arka plan gürültüsü (tıs) ile kötü kaydedilmiş içerik.
 
## <a name="input-and-output-files"></a>Giriş ve çıktı dosyaları
### <a name="input-files"></a>Dosya girişi
Ses veya video dosyaları

### <a name="output-files"></a>Çıkış dosyaları
Dizin oluşturma işi aşağıdaki biçimlerde kapalı altyazı dosyaları oluşturabilir:  

* **TTML**
* **WebVTT**

Bu biçimlerde kapalı altyazı (CC) dosyaları işitme engelli kişiler için ses ve video dosyalarını erişilebilir hale getirmek için kullanılabilir.

## <a name="task-configuration-preset"></a>Görev yapılandırması (önceden ayarlanmış)
**Azure Media Indexer 2 Preview**ile bir dizin oluşturma görevi oluştururken, bir yapılandırma ön kümesi belirtmeniz gerekir.

Aşağıdaki JSON kullanılabilir parametreleri ayarlar.

```json
    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }
```

## <a name="supported-languages"></a>Desteklenen diller
Azure Media Indexer 2 Preview, aşağıdaki diller için konuşma metnini destekler (görev yapılandırmasında dil adını belirtirken, parantez içinde aşağıda gösterildiği gibi 4 karakterli kod kullanın):

* İngilizce [Enus]
* İspanyolca [Eses]
* Çince (Mandalina, Basitleştirilmiş) [ZhCn]
* Fransızca [FrFr]
* Almanca [Dede]
* İtalyanca [Itit]
* Portekizce [PtBr]
* Arapça (Mısırlı) [ArEg]
* Japonca [JaJp]
* Rusça [RuRu]
* İngiliz İngilizcesi [Engb]
* İspanyolca (Meksika) [EsMx] 

## <a name="supported-file-types"></a>Desteklenen dosya türleri

Desteklenen dosya türleri hakkında bilgi için [desteklenen codec/formatlar](media-services-media-encoder-standard-formats.md#input-containerfile-formats) bölümüne bakın.

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program nasıl yapılacağını gösterir:

1. Bir varlık oluşturun ve bir medya dosyasını varlığa yükleyin.
2. Aşağıdaki json ön ayarını içeren bir yapılandırma dosyasını temel alan bir dizin oluşturma görevi içeren bir iş oluşturun:

    ```json
            {
            "version":"1.0",
            "Features":
                [
                {
                "Options": {
                        "Formats":["WebVtt","ttml"],
                        "Language":"enUs",
                        "Type":"RecoOptions"
                },
                "Type":"SpReco"
                }]
            }
    ```
    
3. Çıktı dosyalarını indirin. 
   
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

namespace IndexContent
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

            // Run indexing job.
            var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                        @"C:\supportFiles\Indexer\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
        }

        static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                "My Indexing Input Asset",
                AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Indexing Job");

            // Get a reference to Azure Media Indexer 2 Preview.
            string MediaProcessorName = "Azure Media Indexer 2 Preview";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

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

[Azure Medya Analizi demoları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

