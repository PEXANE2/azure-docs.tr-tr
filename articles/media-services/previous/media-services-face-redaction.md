---
title: Azure Media Analytics ile yüzleri redact | Microsoft Dokümanlar
description: Azure Media Redactor, bulutta ölçeklenebilir yüz redaksiyonu sunan bir Azure Media Analytics medya işlemcisidir. Bu makalede, Azure medya analitiği ile yüzlerin nasıl redact gösteriş.
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
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 6a1b7a76ef1efda51f09ac733b3d434235ff40ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900309"
---
# <a name="redact-faces-with-azure-media-analytics"></a>Azure Media Analytics ile yüzleri redact 
## <a name="overview"></a>Genel Bakış
**Azure Media Redactor,** bulutta ölçeklenebilir yüz redaksiyonu sunan bir [Azure Media Analytics](media-services-analytics-overview.md) medya işlemcisidir (MP). Yüz redaksiyonu, seçilen kişilerin yüzlerini bulanıklaştıracak şekilde videonuzu değiştirmenize olanak tanır. Yüz redaksiyon hizmetini kamu güvenliği ve haber medyası senaryolarında kullanmak isteyebilirsiniz. Birden çok yüz içeren görüntülerin birkaç dakika el ile redact saat sürebilir, ancak bu hizmet ile yüz redaksiyon işlemi sadece birkaç basit adım gerektirir. Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/azure-media-redactor/) bloga bakın.

Bu makalede, **Azure Media Redactor** hakkında ayrıntılı bilgi verir ve .NET için Media Services SDK ile nasıl kullanılacağını gösterir.

## <a name="face-redaction-modes"></a>Yüz redaksiyon modları
Yüz redaksiyonu, videonun her karesindeki yüzleri algılayarak ve yüz nesnesini zaman içinde hem ileri hem de geri izleyerek çalışır, böylece aynı kişi diğer açılardan da bulanıklaşabilir. Otomatik redaksiyon işlemi karmaşıktır ve her zaman istenen çıktının %100'ünü üretmez, bu nedenle Media Analytics size son çıktıyı değiştirmenin birkaç yolunu sunar.

Tam otomatik moduna ek olarak, bulunan yüzlerin bir listesi üzerinden seçilmesini/seçimini yapmasına olanak tanıyan iki geçişli bir iş akışı vardır. Ayrıca, MP JSON biçiminde bir meta veri dosyası kullanır kare ayarlamaları başına rasgele yapmak için. Bu iş akışı **Çözümle** ve **Redact** modlarına bölünür. İki modu, her iki görevi de tek bir işte çalıştıran tek bir geçişte birleştirebilirsiniz; bu mod **Kombine**olarak adlandırılır.

### <a name="combined-mode"></a>Birleşik mod
Bu herhangi bir manuel giriş olmadan otomatik olarak redacted mp4 üretir.

| Aşama | Dosya Adı | Notlar |
| --- | --- | --- |
| Giriş varlığı |foo.bar |WMV, MOV veya MP4 formatında video |
| Giriş config |İş yapılandırması ön kümesi |{'version':'1.0', 'options': {'mode':'combined'}} |
| Çıkış varlığı |foo_redacted.mp4 |Bulanıklık uygulanan video |

#### <a name="input-example"></a>Giriş örneği:
[bu videoyu izleyin](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

#### <a name="output-example"></a>Çıktı örneği:
[bu videoyu izleyin](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

### <a name="analyze-mode"></a>Analiz modu
İki geçişli iş akışının **analiz** geçişi bir video girişi alır ve yüz konumlarının bir JSON dosyası ve algılanan her yüzün jpg görüntüleri üretir.

| Aşama | Dosya Adı | Notlar |
| --- | --- | --- |
| Giriş varlığı |foo.bar |WMV, MPV veya MP4 formatında video |
| Giriş config |İş yapılandırması ön kümesi |{'version':'1.0', 'options': {'mode':'analyze'}} |
| Çıkış varlığı |foo_annotations.json |JSON formatında yüz konumlarının ek açıklama verileri. Bu, bulanıklaştırma sınırlayıcı kutularını değiştirmek için kullanıcı tarafından düzenlenebilir. Aşağıdaki örneğe bakın. |
| Çıkış varlığı |foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg] |Numaranın yüzün labelId'ini gösterdiği her tespit edilen yüzün kırpılmış bir jpg'si |

#### <a name="output-example"></a>Çıktı örneği:

```json
    {
      "version": 1,
      "timescale": 24000,
      "offset": 0,
      "framerate": 23.976,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 48048,
          "interval": 1001,
          "events": [
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [],
            [
              {
                "index": 13,
                "id": 1138,
                "x": 0.29537,
                "y": -0.18987,
                "width": 0.36239,
                "height": 0.80335
              },
              {
                "index": 13,
                "id": 2028,
                "x": 0.60427,
                "y": 0.16098,
                "width": 0.26958,
                "height": 0.57943
              }
            ],

    … truncated
```

### <a name="redact-mode"></a>Redact modu
İş akışının ikinci geçişi, tek bir varlığa birleştirilmesi gereken daha fazla sayıda girdi alır.

Bu, bulanıklaştırılaması gereken bir iyelik listesi, özgün video ve JSON ek açıklamalarını içerir. Bu mod, giriş videosuna bulanıklaştırma uygulamak için ek açıklamaları kullanır.

Analyze pass'inden çıkan çıktı orijinal videoyu içermez. Videonun Redact modu görevi için giriş varlığına yüklenmesi ve birincil dosya olarak seçilmesi gerekir.

| Aşama | Dosya Adı | Notlar |
| --- | --- | --- |
| Giriş varlığı |foo.bar |WMV, MPV veya MP4 formatında video. Adım 1'deki yle aynı video. |
| Giriş varlığı |foo_annotations.json |isteğe bağlı değişiklikler ile birinci aşamadan gelen ek açıklamalar meta veri dosyası. |
| Giriş varlığı |foo_IDList.txt (İsteğe Bağlı) |İsteğe bağlı yeni satır, redact için yüz iliklerinin listesini ayırdı. Boş bırakılırsa, bu tüm yüzleri bulanıklaştırır. |
| Giriş config |İş yapılandırması ön kümesi |{'version':'1.0', 'options': {'mode':'redact'}} |
| Çıkış varlığı |foo_redacted.mp4 |Ek açıklamalara göre uygulanan bulanıklık içeren video |

#### <a name="example-output"></a>Örnek çıktı
Bu, bir kimlik seçili bir IDList'in çıktısI.

[bu videoyu izleyin](https://ampdemo.azureedge.net/?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

Örnek foo_IDList.txt
 
     1
     2
     3

## <a name="blur-types"></a>Bulanıklaştırma türleri

**Kombine** veya **Redact** modunda, JSON giriş yapılandırması üzerinden seçebileceğiniz 5 farklı bulanıklık modu vardır: **Düşük**, **Med**, **Yüksek**, **Kutu**, ve **Siyah**. Varsayılan **olarak Med** kullanılır.

Bulanıklık türlerinden örnekler aşağıda bulabilirsiniz.

### <a name="example-json"></a>Örnek JSON:

```json
    {'version':'1.0', 'options': {'Mode': 'Combined', 'BlurType': 'High'}}
```

#### <a name="low"></a>Düşük

![Düşük](./media/media-services-face-redaction/blur1.png)
 
#### <a name="med"></a>Med

![Med](./media/media-services-face-redaction/blur2.png)

#### <a name="high"></a>Yüksek

![Yüksek](./media/media-services-face-redaction/blur3.png)

#### <a name="box"></a>Box

![Box](./media/media-services-face-redaction/blur4.png)

#### <a name="black"></a>Siyah

![Siyah](./media/media-services-face-redaction/blur5.png)

## <a name="elements-of-the-output-json-file"></a>Çıktı JSON dosyasının öğeleri

Redaction MP, bir video karesinde en fazla 64 insan yüzü algılayabilen yüksek hassasiyetli yüz konumu algılama ve izleme sağlar. Ön yüzler en iyi sonuçları sağlarken, yan yüzler ve küçük yüzler (24x24 pikselden daha az veya eşit) zorludur.

[!INCLUDE [media-services-analytics-output-json](../../../includes/media-services-analytics-output-json.md)]

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki program nasıl yapılacağını gösterir:

1. Bir varlık oluşturun ve bir medya dosyasını varlığa yükleyin.
2. Aşağıdaki json önceden ayarını içeren bir yapılandırma dosyasına dayalı bir yüz redaksiyon görevi olan bir iş oluşturun: 

    ```json
            {
                'version':'1.0',
                'options': {
                    'mode':'combined'
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

namespace FaceRedaction
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

            // Run the FaceRedaction job.
            var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                        @"C:\supportFiles\FaceRedaction\config.json");

            // Download the job output asset.
            DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
        }

        static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
        {
            // Create an asset and upload the input media file to storage.
            IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Face Redaction Input Asset",
            AssetCreationOptions.None);

            // Declare a new job.
            IJob job = _context.Jobs.Create("My Face Redaction Job");

            // Get a reference to Azure Media Redactor.
            string MediaProcessorName = "Azure Media Redactor";

            var processor = GetLatestMediaProcessorByName(MediaProcessorName);

            // Read configuration from the specified file.
            string configuration = File.ReadAllText(configurationFile);

            // Create a task with the encoding details, using a string preset.
            ITask task = job.Tasks.AddNew("My Face Redaction Task",
            processor,
            configuration,
            TaskOptions.None);

            // Specify the input asset.
            task.InputAssets.Add(asset);

            // Add an output asset to contain the results of the job.
            task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);

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

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>İlgili bağlantılar
[Azure Medya Hizmetleri Analizine Genel Bakış](media-services-analytics-overview.md)

[Azure Medya Analizi demoları](https://azuremedialabs.azurewebsites.net/demos/Analytics.html)

