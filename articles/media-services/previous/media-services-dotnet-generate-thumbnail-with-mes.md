---
title: Media Encoder Standard ve .NET kullanarak küçük resim oluşturma
description: Bu konu, Bir varlığı kodlamak ve media encoder Standard'ı kullanarak aynı anda küçük resimler oluşturmak için .NET'in nasıl kullanılacağını gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: b8dab73a-1d91-4b6d-9741-a92ad39fc3f7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 6bc29c098bcf7ef1d1a2e2532a00c95f0ec7e927
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61244238"
---
# <a name="how-to-generate-thumbnails-using-media-encoder-standard-with-net"></a>Media Encoder Standard ve .NET kullanarak küçük resim oluşturma 

[JPEG,](https://en.wikipedia.org/wiki/JPEG) [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics)veya [BMP](https://en.wikipedia.org/wiki/BMP_file_format) görüntü dosyası biçimlerindeki giriş videonuzdan bir veya daha fazla küçük resim oluşturmak için Media Encoder Standard'ı kullanabilirsiniz. Yalnızca resim üreten Görevler gönderebilir veya küçük resim oluşturmayı kodlamayla birleştirebilirsiniz. Bu makalede, bu tür senaryolar için birkaç örnek XML ve JSON küçük resim hazır ayarlar sağlar. Makalenin sonunda, kodlama görevini gerçekleştirmek için Medya Hizmetleri .NET SDK'nın nasıl kullanılacağını gösteren bir [örnek kod](#code_sample) vardır.

Örnek hazır ayarlarda kullanılan öğeler hakkında daha fazla bilgi için [Media Encoder Standard şeasını](media-services-mes-schema.md)gözden geçirmelisiniz.

[Dikkat Edilmesi Gerekenler](media-services-dotnet-generate-thumbnail-with-mes.md#considerations) bölümünü gözden geçirin.
    
## <a name="example-of-a-single-png-file-preset"></a>"Tek Bir PNG dosyası" ön ayar örneği

Aşağıdaki JSON ve XML ön ayarı, kodlayıcının "ilginç" bir çerçeve bulmak için en iyi çabayı sarf ettiği giriş videosunun ilk birkaç saniyesinden itibaren tek bir çıkış PNG dosyası üretmek için kullanılabilir. Çıktı görüntü boyutlarının %100 olarak ayarlandığını, yani bunların giriş videosunun boyutlarıyla eşleştin. "Çıktılar" bölümündeki "Biçim" ayarının "Codec" bölümündeki "PngLayers" kullanımını eşleştirmek için nasıl gerekli olduğuna da dikkat edin. 

### <a name="json-preset"></a>JSON hazır

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "PngImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        }
      ]
    }
```
    
### <a name="xml-preset"></a>XML ön ayar

```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <PngImage Start="{Best}">
          <PngLayers>
            <PngLayer>
              <Width>100%</Width>
              <Height>100%</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>
```

## <a name="example-of-a-series-of-jpeg-images-preset"></a>"JPEG görüntüleri serisi" önceden ayarlanmış bir örnek

Aşağıdaki JSON ve XML ön ayı, görüntü boyutunun giriş videosunun dörtte biri olarak belirtildiği giriş zaman çizelgesinin %95'i olan %5, %15, ..., giriş zaman damgalarında 10 görüntüden oluşan bir set üretmek için kullanılabilir.

### <a name="json-preset"></a>JSON hazır

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "5%",
          "Step": "10%",
          "Range": "96%",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }
```

### <a name="xml-preset"></a>XML ön ayar
    
```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="5%" Step="10%" Range="96%">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>
```

## <a name="example-of-a-one-image-at-a-specific-timestamp-preset"></a>"Belirli bir zaman damgası olan tek görüntü" ön kümesi örneği

Aşağıdaki JSON ve XML ön ayar, giriş videosunun 30 saniyelik işaretinde tek bir JPEG görüntüsü oluşturmak için kullanılabilir. Bu ön ayar, giriş videosunun süresi 30 saniyeden fazla olmasını bekler (aksi takdirde iş başarısız olur).

### <a name="json-preset"></a>JSON hazır

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "25%",
              "Height": "25%"
            }
          ],
          "Start": "00:00:30",
          "Step": "1",
          "Range": "1",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
      ]
    }
```

### <a name="xml-preset"></a>XML ön ayar
```xml
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <JpgImage Start="00:00:30" Step="00:00:01" Range="00:00:01">
          <JpgLayers>
            <JpgLayer>
              <Width>25%</Width>
              <Height>25%</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>
```

## <a name="example-of-a-thumbnails-at-different-resolutions-preset"></a>"Farklı çözünürlüklerde küçük resimler" ön ayar örneği

Aşağıdaki önceden ayar, tek bir görevde farklı çözünürlüklerde küçük resimler oluşturmak için kullanılabilir. Örnekte, giriş zaman çizelgesinin %5, %15, ..., %95'inde, kodlayıcı iki görüntü oluşturur – biri giriş video çözünürlüğünün %100'ü, diğeri %50'dir.

Dosya Adı'nda {Çözünürlük} makrosu kullanımına dikkat edin; çıktı görüntülerinin dosya adını oluştururken önakümenin Kodlama bölümünde belirttiğiniz genişliği ve yüksekliği kullanmak için kodlayıcıya işaret gösterir. Bu aynı zamanda farklı görüntüler arasında kolayca ayırt yardımcı olur

### <a name="json-preset"></a>JSON hazır

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "JpgLayers": [
        {
          "Quality": 90,
          "Type": "JpgLayer",
          "Width": "100%",
          "Height": "100%"
        },
        {
          "Quality": 90,
          "Type": "JpgLayer",
          "Width": "50%",
          "Height": "50%"
        }

          ],
          "Start": "5%",
          "Step": "10%",
          "Range": "96%",
          "Type": "JpgImage"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Resolution}_{Index}{Extension}",
          "Format": {
        "Type": "JpgFormat"
          }
        }
      ]
    }
```

### <a name="xml-preset"></a>XML ön ayar
```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
    <Encoding>
    <JpgImage Start="5%" Step="10%" Range="96%"><JpgImage Start="00:00:01" Step="00:00:15">
      <JpgLayers>
       <JpgLayer>
        <Width>100%</Width>
        <Height>100%</Height>
        <Quality>90</Quality>
       </JpgLayer>
       <JpgLayer>
        <Width>50%</Width>
        <Height>50%</Height>
        <Quality>90</Quality>
       </JpgLayer>
      </JpgLayers>
    </JpgImage>
    </Encoding>
    <Outputs>
      <Output FileName="{Basename}_{Resolution}_{Index}{Extension}">
        <JpgFormat/>
      </Output>
    </Outputs>
    </Preset>
```

## <a name="example-of-generating-a-thumbnail-while-encoding"></a>Kodlama sırasında küçük resim oluşturma örneği

Yukarıdaki örneklerin tümü yalnızca görüntü üreten bir kodlama görevini nasıl gönderebileceğinizi tartışmış olsa da, video/ses kodlamasını küçük resim oluşturmayla birleştirebilirsiniz. Aşağıdaki JSON ve XML ön ayar, kodlama sırasında bir küçük resim oluşturmak için **Media Encoder Standard'ı** söyler.

### <a name="json-preset"></a><a id="json"></a>JSON hazır
Şema hakkında bilgi için [bu](https://msdn.microsoft.com/library/mt269962.aspx) makaleye bakın.

```json
    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
    
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": "100%",
              "Height": "100%"
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Resolution}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }
```

### <a name="xml-preset"></a><a id="xml"></a>XML ön ayar
Şema hakkında bilgi için [bu](https://msdn.microsoft.com/library/mt269962.aspx) makaleye bakın.

```csharp
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>100%</Width>
              <Height>100%/Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Resolution}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
      </Outputs>
    </Preset>   
```

## <a name="encode-video-and-generate-thumbnail-with-net"></a><a id="code_sample"></a>Videoyu kodlayın ve .NET ile küçük resim oluşturun

Aşağıdaki kod örneği, aşağıdaki görevleri gerçekleştirmek için Medya Hizmetleri .NET SDK'yı kullanır:

* Kodlama işi oluşturun.
* Media Encoder Standart kodlayıcısına başvurun.
* Kodlama ön ayarını içeren önceden ayarlanmış [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) veya [JSON'u](media-services-dotnet-generate-thumbnail-with-mes.md#json) ve küçük resimler oluşturmak için gereken bilgileri yükleyin. Bu [XML](media-services-dotnet-generate-thumbnail-with-mes.md#xml) veya [JSON'u](media-services-dotnet-generate-thumbnail-with-mes.md#json) bir dosyaya kaydedebilir ve dosyayı yüklemek için aşağıdaki kodu kullanabilirsiniz.
  
        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
* İşe tek bir kodlama görevi ekleyin. 
* Kodlanacak giriş kıymetini belirtin.
* Kodlanmış kıymeti içeren bir çıktı kıymeti oluşturun.
* İş ilerlemesini denetlemek için bir olay işleyicisi ekleyin.
* İşi gönderin.

Geliştirme ortamınızı nasıl ayarlayabileceğinize ilişkin yol tarifleri için [.NET](media-services-dotnet-how-to-use.md) makalesi ile Medya Hizmetleri geliştirme sine bakın.

```csharp
using System;
using System.Configuration;
using System.IO;
using System.Linq;
using Microsoft.WindowsAzure.MediaServices.Client;
using System.Threading;

namespace EncodeAndGenerateThumbnails
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

        private static CloudMediaContext _context = null;

        private static readonly string _mediaFiles =
        Path.GetFullPath(@"../..\Media");

        private static readonly string _singleMP4File =
            Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");

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

            // Encode and generate the thumbnails.
            EncodeToAdaptiveBitrateMP4Set(asset);

            Console.ReadLine();
        }

        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Thumbnail Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

            // Load the XML (or JSON) from the local file.
            string configuration = File.ReadAllText("ThumbnailPreset_JSON.json");

            // Create a task
            ITask task = job.Tasks.AddNew("Media Encoder Standard Thumbnail task",
                    processor,
                    configuration,
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

## <a name="considerations"></a>Dikkat edilmesi gerekenler
Aşağıdaki noktalara dikkat edilmelidir:

* Başlangıç/Adım/Aralık için açık zaman damgalarının kullanılması, giriş kaynağının en az 1 dakika uzunluğunda olduğunu varsayar.
* Jpg/Png/BmpImage öğeleri Başlangıç, Adım ve Aralık dize özelliklerine sahiptir – bunlar şu şekilde yorumlanabilir:
  
  * Çerçeve Numarası negatif olmayan tümsalar, örneğin "Başlangıç": "120",
  * %-suffixed olarak ifade edilirse kaynak süresine göre, örneğin "Başlangıç": "%15", OR
  * HH:MM:SS olarak ifade edilirse zaman damgası... Biçim. Örneğin "Başlat" : "00:01:00"
    
    Notaları istediğiniz gibi karıştırıp eşleştirebilirsiniz.
    
    Ayrıca, Başlat ayrıca özel bir Makro destekler:{Best}, içerik NOT ilk "ilginç" çerçevebelirlemek için çalışır: (Başlat {Best} olarak ayarlandığında Adım ve Aralık yoksayılır)
  * Varsayılanlar: Başlat:{En Iyi}
* Çıkış biçiminin her Görüntü biçimi için açıkça sağlanması gerekir: Jpg/Png/BmpFormat. Mevcut olduğunda, MES JpgFormat ve benzeri JpgVideo eşleşir. OutputFormat, görüntü çıktıbiçimleri için (bir ve yalnızca bir kez) bulunması gereken yeni bir görüntü kodlayıcıya özgü Makro: {Index}'i sunar.

## <a name="next-steps"></a>Sonraki adımlar

Kodlama işi beklemedeyken [iş ilerlemesini](media-services-check-job-progress.md) denetleyebilirsiniz.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Medya Hizmetleri Kodlama Genel Bakış](media-services-encode-asset.md)

