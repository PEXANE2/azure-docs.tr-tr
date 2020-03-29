---
title: Bitrate merdiveni otomatik olarak oluşturmak için Media Encoder Standard'ı kullanın - Azure | Microsoft Dokümanlar
description: Bu konu, giriş çözünürlüğüne ve bitrate'ye dayalı bir bitrate merdiveni otomatik olarak oluşturmak için Media Encoder Standard'ın (MES) nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: b7f0b77ba11a0c9c1670ec240caf45fcf61a934d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74896013"
---
#  <a name="use-media-encoder-standard-to-auto-generate-a-bitrate-ladder"></a>Bitrate merdiveni otomatik olarak oluşturmak için Media Encoder Standard'ı kullanın  

## <a name="overview"></a>Genel Bakış

Bu makalede, giriş çözünürlüğü ve bitrate dayalı bir bitrate merdiveni (bitrate çözünürlüğü çiftleri) otomatik oluşturmak için Media Encoder Standard (MES) nasıl kullanılacağını gösterir. Otomatik olarak oluşturulan önceden ayar, giriş çözünürlüğünü ve bithızını asla aşmaz. Örneğin, giriş 3 Mbps'de 720p ise, çıkış en iyi ihtimalle 720p kalır ve 3 Mbps'den düşük hızlarda başlar.

### <a name="encoding-for-streaming-only"></a>Yalnızca Akış için Kodlama

Amacınız kaynak videonuzu yalnızca akış için kodlamaksa, kodlama görevi oluştururken "Uyarlanabilir Akış" ön ayarını kullanmanız gerekir. **Adaptive Streaming** ön ayarını kullanırken, MES kodlayıcısı bir bitrate merdiveni akıllıca kaplar. Ancak, hizmet kaç katmanın kullanılacağını ve hangi çözünürlükte kullanılacağını belirlediğinden, kodlama maliyetlerini denetleyemeyeceksiniz. Bu makalenin sonunda **Uyarlanabilir Akış** ön kümesi ile kodlama sonucunda MES tarafından üretilen çıktı katmanlarının örneklerini görebilirsiniz. Çıkış Varlık ses ve video interleaved değil MP4 dosyaları içerir.

### <a name="encoding-for-streaming-and-progressive-download"></a>Akış ve Aşamalı İndirme için Kodlama

Amacınız akış için kaynak video kodlamak yanı sıra aşamalı indirmek için MP4 dosyaları üretmek için ise, o zaman bir kodlama görevi oluştururken "İçerik Uyarlanabilir Çoklu Bitrate MP4" önceden ayarını kullanmanız gerekir. **İçerik Uyarlamalı Çoklu Bitrate MP4** ön ayarını kullanırken, MES kodlayıcısı yukarıdaki yle aynı kodlama mantığını uygular, ancak şimdi çıkış varlığı ses ve videonun ara ayrıldığı MP4 dosyaları içerir. Bu MP4 dosyalarından birini (örneğin, en yüksek bit hızı sürümü) aşamalı bir indirme dosyası olarak kullanabilirsiniz.

## <a name="encoding-with-media-services-net-sdk"></a><a id="encoding_with_dotnet"></a>Medya Hizmetleri ile Kodlama .NET SDK

Aşağıdaki kod örneği, aşağıdaki görevleri gerçekleştirmek için Medya Hizmetleri .NET SDK'yı kullanır:

- Kodlama işi oluşturun.
- Media Encoder Standart kodlayıcısına başvurun.
- İşe bir kodlama görevi ekleyin ve **Uyarlanabilir Akış** hazır kümesini kullanmayı belirtin. 
- Kodlanmış kıymeti içeren bir çıktı kıymeti oluşturun.
- İş ilerlemesini denetlemek için bir olay işleyicisi ekleyin.
- İşi gönderin.

#### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

#### <a name="example"></a>Örnek

```
using System;
using System.Configuration;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;

namespace AdaptiveStreamingMESPresest
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

            // Get an uploaded asset.
            var asset = _context.Assets.FirstOrDefault();

            // Encode and generate the output using the "Adaptive Streaming" preset.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");

            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            "Adaptive Streaming",
            TaskOptions.None);

            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();

            return job.OutputMediaAssets[0];
        }
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                    break;
                default:
                    break;
            }
        }
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();

            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));

            return processor;
        }
    }
}
```

## <a name="output"></a><a id="output"></a>Çıktı

Bu bölümde, **Uyarlamalı Akış** ön kümesi ile kodlama sonucunda MES tarafından üretilen çıkış katmanlarının üç örneği gösterilmektedir. 

### <a name="example-1"></a>Örnek 1
Yükseklik "1080" ve kare hızı "29.970" ile kaynak 6 video katmanları üretir:

|Katman|Height|Genişlik|Bithızı(kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Örnek 2
Yüksekliği "720" ve kare hızı "23.970" olan kaynak 5 video katmanı üretir:

|Katman|Height|Genişlik|Bithızı(kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Örnek 3
Yükseklik "360" ve kare hızı "29.970" ile kaynak 3 video katmanları üretir:

|Katman|Height|Genişlik|Bithızı(kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Medya Hizmetleri Kodlama Genel Bakış](media-services-encode-asset.md)

