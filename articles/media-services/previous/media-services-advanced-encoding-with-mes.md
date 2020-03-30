---
title: MES hazır ayarlarını özelleştirerek gelişmiş kodlama gerçekleştirin | Microsoft Dokümanlar
description: Bu konu, Media Encoder Standart görev hazır ayarlarını özelleştirerek gelişmiş kodlamanın nasıl gerçekleştirilini gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 2a4ade25-e600-4bce-a66e-e29cf4a38369
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: 5f7611fd9df207df51fa0e51218d8a234583b1f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529792"
---
# <a name="perform-advanced-encoding-by-customizing-mes-presets"></a>MES hazır ayarlarını özelleştirerek gelişmiş kodlama gerçekleştirin 

## <a name="overview"></a>Genel Bakış

Bu konu, Media Encoder Standart ön ayarlarını nasıl özelleştiriştirilir gösterir. Özel hazır ayarlar konusunu [kullanarak Media Encoder Standard ile kodlama,](media-services-custom-mes-presets-with-dotnet.md) bir kodlama görevi ve bu görevi yürüten bir iş oluşturmak için .NET'in nasıl kullanılacağını gösterir. Önceden ayarlanmış bir özelliği özelleştirdikten sonra, kodlama görevine özel hazır ayarları tedarik edin. 

XML önceden ayarlanmış kullanıyorsanız, aşağıdaki XML örneklerinde gösterildiği gibi öğelerin sırasını koruduğumdan emin olun (örneğin, KeyFrameInterval SceneChangeDetection'dan önce gelmelidir).

> [!NOTE] 
> Media Encoder Standard'ın gelişmiş Media Services v2 özelliklerinin çoğu şu anda v3'te bulunmamaktadır. Daha fazla bilgi için [özellik boşluklarına](https://docs.microsoft.com/azure/media-services/latest/media-services-v2-vs-v3#feature-gaps-with-respect-to-v2-apis)bakın.

## <a name="support-for-relative-sizes"></a>Göreli boyutlar için destek

Küçük resimler oluştururken, her zaman piksellerde çıkış genişliği ve yüksekliği belirtmeniz gerekmez. Bunları yüzdeolarak, aralıkta belirtebilirsiniz [1%, ..., %100].

### <a name="json-preset"></a>JSON hazır
    "Width": "100%",
    "Height": "100%"

### <a name="xml-preset"></a>XML ön ayar
    <Width>100%</Width>
    <Height>100%</Height>

## <a name="generate-thumbnails"></a><a id="thumbnails"></a>Küçük resim oluşturma

Bu bölümde, küçük resimler oluşturan bir önceden ayarnasıl özelleştirilen gösterilmektedir. Aşağıda tanımlanan önceden ayarlanmış, dosyanızı nasıl kodlamak istediğinize ilişkin bilgilerin yanı sıra küçük resimler oluşturmak için gereken bilgileri de içerir. [Bu](media-services-mes-presets-overview.md) bölümü belgeleyen MES önceden ayarlarından herhangi birini alabilir ve küçük resimler oluşturan kod ekleyebilirsiniz.  

> [!NOTE]
> Aşağıdaki önceden ayarlanmış **SceneChangeDetection** ayarı yalnızca tek bir bit hızıvideo kodlama iseniz doğru ayarlanabilir. Çok bit hızında bir videoya kodlar ve **SceneChangeDetection'ı** doğru ayarlarsanız, kodlayıcı bir hata döndürür.  
>
>

Şema hakkında bilgi için [bu](media-services-mes-schema.md) konuya bakın.

[Dikkat Edilmesi Gerekenler](#considerations) bölümünü gözden geçirin.

### <a name="json-preset"></a><a id="json"></a>JSON hazır
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
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
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
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a name="xml-preset"></a><a id="xml"></a>XML ön ayar
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
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

### <a name="considerations"></a>Dikkat edilmesi gerekenler

Aşağıdaki noktalara dikkat edilmelidir:

* Başlangıç/Adım/Aralık için açık zaman damgalarının kullanılması, giriş kaynağının en az 1 dakika uzunluğunda olduğunu varsayar.
* Jpg/Png/BmpImage öğeleri Başlangıç, Adım ve Aralık dize özelliklerine sahiptir – bunlar şu şekilde yorumlanabilir:

  * Çerçeve Numarası negatif olmayan tümsalar, örneğin "Başlangıç": "120",
  * %-suffixed olarak ifade edilirse kaynak süresine göre, örneğin "Başlangıç": "%15", OR
  * HH:MM:SS olarak ifade edilirse zaman damgası... biçimi, örneğin "Başlat" : "00:01:00"

    Notaları istediğiniz gibi karıştırıp eşleştirebilirsiniz.

    Ayrıca, Başlat ayrıca özel bir Makro destekler:{Best}, içerik NOT ilk "ilginç" çerçevebelirlemek için çalışır: (Başlat {Best} olarak ayarlandığında Adım ve Aralık yoksayılır)
  * Varsayılanlar: Başlat:{En Iyi}
* Çıkış biçiminin her Görüntü biçimi için açıkça sağlanması gerekir: Jpg/Png/BmpFormat. Mevcut olduğunda, MES JpgFormat ve benzeri JpgVideo eşleşir. OutputFormat, görüntü çıktıbiçimleri için (bir ve yalnızca bir kez) bulunması gereken yeni bir görüntü kodlayıcıya özgü Makro: {Index}'i sunar.

## <a name="trim-a-video-clipping"></a><a id="trim_video"></a>Videoyu kırpma (kırpma)
Bu bölümde, girişin asma dosya veya isteğe bağlı dosya olarak adlandırılan giriş videosunu kırpmak veya kırpmak için kodlayıcı hazır ayarlarını değiştirme hakkında dır. Kodlayıcı, canlı bir akıştan yakalanan veya arşivlenmiş bir varlığı kesmek veya kesmek için de kullanılabilir – bunun ayrıntıları [bu blogda](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/)mevcuttur.

Videolarınızı kırpmak [için, bu](media-services-mes-presets-overview.md) bölümde belgelenen MES hazır ayarlarından herhangi birini alabilir ve **Kaynaklar** öğesini değiştirebilirsiniz (aşağıda gösterildiği gibi). Başlangıç Zamanı'nın değerinin giriş videosunun mutlak zaman damgaları ile eşleşmesi gerekir. Örneğin, giriş videosunun ilk karesinde 12:00:10.000'lik bir zaman damgası varsa, Başlangıç Saati en az 12:00:10.000 ve daha büyük olmalıdır. Aşağıdaki örnekte, giriş videosunun sıfır başlangıç zaman damgası olduğunu varsayıyoruz. **Kaynaklar** önceden ayarlanmış başında yerleştirilmelidir.

### <a name="json-preset"></a><a id="json"></a>JSON hazır
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="xml-preset"></a>XML ön ayar
Videolarınızı kırpmak [için, burada](media-services-mes-presets-overview.md) belgelenen MES hazır ayarlarından herhangi birini alabilir ve **Kaynaklar** öğesini değiştirebilirsiniz (aşağıda gösterildiği gibi).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
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
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

## <a name="create-an-overlay"></a><a id="overlay"></a>Bindirme oluşturma

Media Encoder Standardı, bir görüntüyü varolan bir videonun üzerine bindirmenize olanak tanır. Şu anda, aşağıdaki biçimleri desteklenir: png, jpg, gif ve bmp. Aşağıda tanımlanan önceden ayarlanmış, video bindirmesinin temel bir örneğidir.

Önceden ayarlanmış bir dosya tanımlamaya ek olarak, Medya Hizmetleri'ne varlıktaki hangi dosyanın bindirme görüntüsü olduğunu ve hangi dosyanın görüntüyü üzerine koymak istediğiniz kaynak video olduğunu da bilmeniz gerekir. Video dosyası **birincil** dosya olmalıdır.

.NET kullanıyorsanız, [bu](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) konuda tanımlanan .NET örneğine aşağıdaki iki işlevi ekleyin. **UploadMediaFilesFromFolder** işlevi dosyaları bir klasörden yükler (örneğin, BigBuckBunny.mp4 ve Image001.png) ve mp4 dosyasını kıymetin birincil dosyası olarak ayarlar. **EncodeWithOverlay** işlevi, kodlama görevini oluşturmak için ona geçirilen özel önceden ayarlanmış dosyayı (örneğin, aşağıdaki ön ayar) kullanır.


    static public IAsset UploadMediaFilesFromFolder(string folderPath)
    {
        IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
    
        foreach (var af in asset.AssetFiles)
        {
            // The following code assumes 
            // you have an input folder with one MP4 and one overlay image file.
            if (af.Name.Contains(".mp4"))
                af.IsPrimary = true;
            else
                af.IsPrimary = false;
    
            af.Update();
        }
    
        return asset;
    }

    static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
    {
        // Declare a new job.
        IJob job = _context.Jobs.Create("Media Encoder Standard Job");
        // Get a media processor reference, and pass to it the name of the 
        // processor to use for the specific task.
        IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(customPresetFileName);

        // Create a task
        ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input assets to be encoded.
        // This asset contains a source file and an overlay file.
        task.InputAssets.Add(assetSource);

        // Add an output asset to contain the results of the job. 
        task.OutputAssets.AddNew("Output asset",
            AssetCreationOptions.None);

        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
        job.Submit();
        job.GetExecutionProgressTask(CancellationToken.None).Wait();

        return job.OutputMediaAssets[0];
    }


> [!NOTE]
> Geçerli sınırlamalar:
>
> Bindirme opaklık ayarı desteklenmez.
>
> Kaynak video dosyanız ve bindirme görüntü dosyanızın aynı varlıkta olması gerekir ve video dosyasının bu kıymetteki birincil dosya olarak ayarlanması gerekir.
>
>

### <a name="json-preset"></a>JSON hazır
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


### <a name="xml-preset"></a>XML ön ayar
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


## <a name="insert-a-silent-audio-track-when-input-has-no-audio"></a><a id="silent_audio"></a>Girişte ses yoksa sessiz bir ses parçası ekleme
Varsayılan olarak, kodlayıcıya yalnızca video içeren ve ses içermeyen bir giriş gönderirseniz, çıkış kıymeti yalnızca video verileri içeren dosyalar içerir. Bazı oyuncular bu tür çıkış akışlarını işleyebilir. Bu ayarı, kodlayıcıyı bu senaryoda çıktıya sessiz bir ses parçası eklemeye zorlamak için kullanabilirsiniz.

Girişte ses olmadığında sessiz ses parçası içeren bir varlık üretmeye kodlayıcıyı zorlamak için "InsertSilenceIfNoAudio" değerini belirtin.

[Bu](media-services-mes-presets-overview.md) bölümde belgelenen MES hazır ayarlarından herhangi birini alabilir ve aşağıdaki değişiklikleri yapabilirsiniz:

### <a name="json-preset"></a>JSON hazır
    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

### <a name="xml-preset"></a>XML ön ayar
    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

## <a name="disable-auto-de-interlacing"></a><a id="deinterlacing"></a>Otomatik bağlantıyı devre dışı açma
Müşteriler, interlace içeriğinin otomatik olarak silinmesini istiyorlarsa hiçbir şey yapmak zorunda değildir. Otomatik de-interlacing olduğunda (varsayılan) MES interlaced çerçeveler ve sadece de-interlaces çerçeveler interlaced olarak işaretlenmiş otomatik algılama yapar.

Otomatik bağlantı yıkıntma kapatabilirsiniz. Bu seçenek önerilmez.

### <a name="json-preset"></a>JSON hazır
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

### <a name="xml-preset"></a>XML ön ayar
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


## <a name="audio-only-presets"></a><a id="audio_only"></a>Yalnızca ses ekia ayarı
Bu bölümde yalnızca sese özel iki MES ön ayarı gösteriş vardır: AAC Audio ve AAC Kaliteli Ses.

### <a name="aac-audio"></a>AAC Ses
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

### <a name="aac-good-quality-audio"></a>AAC Kaliteli Ses
    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a name="concatenate-two-or-more-video-files"></a><a id="concatenate"></a>İki veya daha fazla video dosyalarını birleştirir

Aşağıdaki örnek, iki veya daha fazla video dosyasını birleştirmek için nasıl bir ön ayar oluşturabileceğinizi göstermektedir. En yaygın senaryo, ana videoya bir üstbilgi veya römork eklemek istediğinizde. Amaçlanan kullanım, birlikte düzenlenen video dosyalarının özellikleri (video çözünürlüğü, kare hızı, ses parça sayısı, vb.) paylaşmasıdır. Farklı kare hızlarında veya farklı ses parçalarıyla video karıştırmamaya dikkat edin.

>[!NOTE]
>Konsültasyon özelliğinin mevcut tasarımı, giriş video kliplerinin çözünürlük, kare hızı vb. açısından tutarlı olmasını bekler. 

### <a name="requirements-and-considerations"></a>Gereksinimler ve hususlar

* Giriş videolarının yalnızca bir ses parçası olmalıdır.
* Giriş videolarının tümü aynı kare hızına sahip olmalıdır.
* Videolarınızı ayrı varlıklara yüklemeniz ve videoları her varlığın birincil dosyası olarak ayarlamanız gerekir.
* Videolarınızın süresini bilmeniz gerekir.
* Aşağıda önceden ayarlanmış örnekler, tüm giriş videolarının sıfır lık bir zaman damgası ile başladığını varsayar. Videoların başlangıç zaman damgası farklısa, genellikle canlı arşivlerde olduğu gibi Başlangıç Zamanı değerlerini değiştirmeniz gerekir.
* JSON önceden ayarlanmış giriş varlıklarının AssetID değerlerine açık başvurular yapar.
* Örnek kod, JSON önceden ayarlandığını varsayar, örneğin "C:\supportFiles\preset.json". Ayrıca, iki video dosyası yükleyerek iki varlığın oluşturulduğunu ve ortaya çıkan AssetID değerlerini bildiğinizi varsayar.
* Kod snippet ve JSON ön ayar iki video dosyası concatenating bir örnek gösterir. İkiden fazla videoya şu şekilde uzatabilirsiniz:

  1. Arama görevi. InputAssets.Add() sırayla, daha fazla video eklemek için tekrar tekrar.
  2. Aynı sırada daha fazla giriş ekleyerek JSON'daki "Kaynaklar" öğesine karşılık gelen düzenlemeyapma.

### <a name="net-code"></a>.NET kodu

    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();

    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");

    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");

    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);

    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job.
    // This output is specified as AssetCreationOptions.None, which
    // means the output asset is not encrypted.
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);

    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

### <a name="json-preset"></a>JSON hazır

Özel ön ayarınızı, biraraya getirmek istediğiniz varlıkların kimlikleri ve her video için uygun zaman dilimiyle güncelleştirin.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
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
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

## <a name="crop-videos-with-media-encoder-standard"></a><a id="crop"></a>Videoları Media Encoder Standard ile kırpma
Media [Encoder Standart](media-services-crop-video.md) konu ile Kırpma videoları bakın.

## <a name="insert-a-video-track-when-input-has-no-video"></a><a id="no_video"></a>Girişte video yokken video parçası ekleme

Varsayılan olarak, kodlayıcıya yalnızca ses içeren ve video içermeyen bir giriş gönderirseniz, çıkış kıymeti yalnızca ses verileri içeren dosyalar içerir. Azure Media Player da dahil olmak üzere bazı oyuncular [(buna](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)bakın) bu tür akışları işleyebilir. Bu ayarı, kodlayıcıyı bu senaryodaki çıktıya tek renkli bir video parçası eklemeye zorlamak için kullanabilirsiniz.

> [!NOTE]
> Kodlayıcıyı bir çıktı video parçası eklemeye zorlamak, çıktı Varlığının boyutunu ve bu nedenle kodlama Görevi için tahakkuk eden maliyeti artırır. Bu artış aylık ücretleri üzerinde sadece mütevazı bir etkisi olduğunu doğrulamak için testler çalıştırmalısınız.
>

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Yalnızca en düşük bit hızında video ekleme

Video dosyaları ve yalnızca ses dosyalarının bir karışımını içeren akış için giriş kataloğunuzun tamamını kodlamak için ["H264 Multiple Bitrate 720p"](media-services-mes-preset-h264-multiple-bitrate-720p.md) gibi birden çok bit hızı kodlama ön kümesi kullandığınızı varsayalım. Bu senaryoda, girişte video yoksa, her çıktı bit hızına video eklemek yerine kodlayıcıyı yalnızca en düşük bit hızında tek renkli bir video parçası eklemeye zorlamak isteyebilirsiniz. Bunu başarmak için **InsertBlackIfNoVideoBottomLayerOnly** bayrağını kullanmanız gerekir.

[Bu](media-services-mes-presets-overview.md) bölümde belgelenen MES hazır ayarlarından herhangi birini alabilir ve aşağıdaki değişiklikleri yapabilirsiniz:

#### <a name="json-preset"></a>JSON hazır
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML ön ayar

XML kullanırken, **AACAudio**bir öznitelik olarak **H264Video** öğesi ve Condition ="InsertSilenceIfNoAudio" bir öznitelik olarak Condition="InsertBlackIfNoVideoOnly" kullanın.

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideoBottomLayerOnly">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .
```

### <a name="inserting-video-at-all-output-bitrates"></a>Tüm çıkış bit hızlarında video ekleme
Video dosyaları ve yalnızca ses dosyalarının bir karışımını içeren akış için giriş kataloğunuzun tamamını kodlamak için ["H264 Multiple Bitrate 720p"](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) gibi birden çok bit hızı kodlama sıyrık kullandığınızı varsayalım. Bu senaryoda, girişte video yoksa, kodlayıcıyı tüm çıkış bit hızlarına tek renkli bir video parçası eklemeye zorlamak isteyebilirsiniz. Bu, çıktı Varlıklarınızın video parçaları ve ses parçaları sayısına göre homojen olmasını sağlar. Bunu başarmak için "InsertBlackIfNoVideo" bayrağını belirtmeniz gerekir.

[Bu](media-services-mes-presets-overview.md) bölümde belgelenen MES hazır ayarlarından herhangi birini alabilir ve aşağıdaki değişiklikleri yapabilirsiniz:

#### <a name="json-preset"></a>JSON hazır
    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML ön ayar

XML kullanırken, **AACAudio'ya**öznitelik olarak Durum="InsertBlackIfNoVideo" kullanın **H264Video** öğesine ve Condition="InsertSilenceIfNoAudio"ya öznitelik olarak .

```
. . .
<Encoding>
  <H264Video Condition="InsertBlackIfNoVideo">
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <SceneChangeDetection>true</SceneChangeDetection>
    <StretchMode>AutoSize</StretchMode>
    <H264Layers>
      <H264Layer>
        . . .
      </H264Layer>
    </H264Layers>
    <Chapters />
  </H264Video>
  <AACAudio Condition="InsertSilenceIfNoAudio">
    <Profile>AACLC</Profile>
    <Channels>2</Channels>
    <SamplingRate>48000</SamplingRate>
    <Bitrate>128</Bitrate>
  </AACAudio>
</Encoding>
. . .  
```

## <a name="rotate-a-video"></a><a id="rotate_video"></a>Videoyu döndürme
[Media Encoder Standardı](media-services-dotnet-encode-with-media-encoder-standard.md) 0/90/180/270 açıları ile döndürmeyi destekler. Varsayılan davranış, gelen video dosyasındaki döndürme meta verilerini algılamaya ve bunu telafi etmeye çalıştığı "Otomatik" dir. [Bu](media-services-mes-presets-overview.md) bölümde tanımlanan hazır ayarlardan birine aşağıdaki **Kaynaklar** öğesini ekleyin:

### <a name="json-preset"></a>JSON hazır
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [

    ...
### <a name="xml-preset"></a>XML ön ayar
    <Sources>
           <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

Ayrıca, döndürme telafisi tetiklendiğinde, kodlayıcının ön ayardaki Genişlik ve Yükseklik ayarlarını nasıl yorumladığı hakkında daha fazla bilgi için [bu](media-services-mes-schema.md#PreserveResolutionAfterRotation) konuya bakın.

Giriş videosunda döndürme meta verilerini yok saymak için kodlayıcıya "0" değerini gösterebilirsiniz.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Medya Hizmetleri Kodlama Genel Bakış](media-services-encode-asset.md)
