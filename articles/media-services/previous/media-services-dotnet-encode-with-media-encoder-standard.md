---
title: .NET kullanarak Media Encoder Standardı ile bir varlığı kodlayın | Microsoft Dokümanlar
description: Bu makalede, Media Encoder Standard kullanarak bir varlığı kodlamak için .NET'in nasıl kullanılacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 03431b64-5518-478a-a1c2-1de345999274
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: 259e32d55f25c4a146b7ff358eb503763dd5fab2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69016580"
---
# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>.NET'i kullanarak Media Encoder Standard ile bir varlığı kodlama  

Kodlama işleri, Medya Hizmetleri'nde en yaygın işleme işlemlerinden biridir. Medya dosyalarını bir kodlamadan diğerine dönüştürmek için kodlama işleri oluşturursunuz. Kodladiğinizde, yerleşik Medya Encoder'ını kullanabilirsiniz. Medya Hizmetleri ortağı tarafından sağlanan bir kodlayıcıyı da kullanabilirsiniz; üçüncü taraf kodlayıcılar Azure Marketi'nden edinilebilir. 

Bu makalede, .NET'in varlıklarınızı Media Encoder Standard (MES) ile kodlamak için nasıl kullanılacağı gösterilmektedir. Media Encoder [Standard, burada](https://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)açıklanan kodlayıcı presetlerinden biri kullanılarak yapılandırılır.

Kaynak dosyalarınızı her zaman uyarlanabilir bir bitrate MP4 setine kodlamanız ve ardından [Dinamik Ambalaj'ı](media-services-dynamic-packaging-overview.md)kullanarak seti istenilen biçime dönüştürmeniz önerilir. 

Çıktı varlığınız depolama şifreliyse, varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için [bkz.](media-services-dotnet-configure-asset-delivery-policy.md)

> [!NOTE]
> MES, giriş dosyası adının ilk 32 karakterini içeren bir ad içeren bir çıktı dosyası üretir. Ad, önceden ayarlanmış dosyada belirtilene dayanır. Örneğin, "FileName": "{Basename}_{Index}{Extension}". {Basename} giriş dosyası adının ilk 32 karakteriyle değiştirilir.
> 
> 

### <a name="mes-formats"></a>MES Biçimleri
[Biçimler ve codec'ler](media-services-media-encoder-standard-formats.md)

### <a name="mes-presets"></a>MES Ön ayarları
Media Encoder [Standard, burada](https://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)açıklanan kodlayıcı presetlerinden biri kullanılarak yapılandırılır.

### <a name="input-and-output-metadata"></a>Giriş ve çıktı meta verileri
MES kullanarak bir giriş kıymetini (veya varlıkları) kodlarken, bu kodlama görevinin başarıyla tamamlanmasından sonra bir çıktı kıymeti elde edersiniz. Çıktı varlığı, kullandığınız kodlama ön ayarını temel alan video, ses, küçük resimler, manifesto vb. içerir.

Çıktı varlığı, giriş kıymeti hakkında meta verileri içeren bir dosya da içerir. Meta data XML dosyasının adı aşağıdaki biçime sahiptir: <asset_id>_metadata.xml (örneğin, 41114ad3-eb5e-4c57-8d92-5354e2b7d4a4_metadata.xml), <asset_id> giriş varlığının AssetId değeridir. Bu giriş meta veri XML şema [burada](media-services-input-metadata-schema.md)açıklanmıştır.

Çıktı varlığı, çıktı varlığı yla ilgili meta verileri içeren bir dosya da içerir. Meta data XML dosyasının adı aşağıdaki biçimi vardır: <source_file_name>_manifest.xml (örneğin, BigBuckBunny_manifest.xml). Bu çıktı meta veri XML şema [burada](media-services-output-metadata-schema.md)açıklanmıştır.

İki meta veri dosyasından birini incelemek istiyorsanız, bir SAS bulucu oluşturup dosyayı yerel bilgisayarınıza indirebilirsiniz. Medya Hizmetleri .NET SDK Uzantılarını Kullanarak Bir SAS bulucu su nasıl oluşturulup dosyayı indirebileceğinize ilişkin bir örnek bulabilirsiniz.

## <a name="download-sample"></a>Örnek indirme
[Buradan](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/)MES ile nasıl kodlanabileceğinizi gösteren bir örnek alıp çalıştırabilirsiniz.

## <a name="net-sample-code"></a>.NET örnek kodu

Aşağıdaki kod örneği, aşağıdaki görevleri gerçekleştirmek için Medya Hizmetleri .NET SDK'yı kullanır:

* Kodlama işi oluşturun.
* Media Encoder Standart kodlayıcısına başvurun.
* [Uyarlanabilir Akış](media-services-autogen-bitrate-ladder-with-mes.md) ön ayarını kullanmayı belirtin. 
* İşe tek bir kodlama görevi ekleyin. 
* Kodlanacak giriş kıymetini belirtin.
* Kodlanmış kıymeti içeren bir çıktı kıymeti oluşturun.
* İş ilerlemesini denetlemek için bir olay işleyicisi ekleyin.
* İşi gönderin.

#### <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio projesi oluşturup yapılandırma

Geliştirme ortamınızı kurun ve app.config dosyanızı [.NET ile Media Services geliştirme](media-services-dotnet-how-to-use.md) bölümünde açıklandığı gibi bağlantı bilgileriyle doldurun. 

#### <a name="example"></a>Örnek 

```csharp
using System;
using System.Linq;
using System.Configuration;
using System.IO;
using System.Threading;
using Microsoft.WindowsAzure.MediaServices.Client;

namespace MediaEncoderStandardSample
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

        private static readonly string _supportFiles =
            Path.GetFullPath(@"../..\Media");

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

            // Create a task with the encoding details, using a string preset.
            // In this case "Adaptive Streaming" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
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

## <a name="advanced-encoding-features-to-explore"></a>Keşfedilecek Gelişmiş Kodlama Özellikleri
* [Küçük resimler nasıl üretilir?](media-services-dotnet-generate-thumbnail-with-mes.md)
* [Kodlama sırasında küçük resim oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md#example-of-generating-a-thumbnail-while-encoding)
* [Kodlama sırasında videoları kırpma](media-services-crop-video.md)
* [Kodlama ön ayarlarını özelleştirme](media-services-custom-mes-presets-with-dotnet.md)
* [Görüntüyü içeren bir videoyu yerle bir veya filigranla kaplama](media-services-advanced-encoding-with-mes.md#overlay)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
[.NET](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services Kodlama Genel Bakış](media-services-encode-asset.md) ile Media Encoder Standard kullanarak küçük resim oluşturma

