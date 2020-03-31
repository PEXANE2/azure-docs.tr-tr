---
title: Media Encoder Standard ile video kırpma - Azure | Microsoft Dokümanlar
description: Kırpma, video çerçevesi içinde dikdörtgen bir pencere seçme ve yalnızca bu pencereiçindeki pikselleri kodlama işlemidir. Bu makalede, Media Encoder Standard ile video kırpma nasıl gösterilmektedir.
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2019
ms.author: anilmur
ms.reviewer: juliako
ms.openlocfilehash: 059816284e39c65bb772bd02f066d73da624722f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74887773"
---
# <a name="crop-videos-with-media-encoder-standard"></a>Media Encoder Standard ile videoları kırpma  

Giriş videonuzu kırpmak için Media Encoder Standard 'ı (MES) kullanabilirsiniz. Kırpma, video çerçevesi içinde dikdörtgen bir pencere seçme ve yalnızca bu pencereiçindeki pikselleri kodlama işlemidir. Aşağıdaki diyagram işlemi göstermeye yardımcı olur.

![Bir video kırpma](./media/media-services-crop-video/media-services-crop-video01.png)

1920x1080 piksel çözünürlüğe (16:9 en boy oranı) sahip, ancak solda ve sağda siyah çubuklara (sütun kutuları) sahip olan bir videoyu girdiniz, böylece yalnızca 4:3 pencere veya 1440x1080 piksel etkin video içerdiğini varsayalım. Mes'i kullanarak siyah çubukları kırpabilir veya dinleyebilirsiniz ve 1440x1080 bölgesini kodlayabilirsiniz.

MES'te kırpma bir ön işleme aşamasıdır, bu nedenle kodlama ön ayarındaki kırpma parametreleri orijinal giriş videosuna uygulanır. Kodlama sonraki bir aşamadır ve genişlik/yükseklik ayarları orijinal video için değil, *önceden işlenmiş* video için geçerlidir. Önceden ayarlarınızı tasarlarken aşağıdakileri yapmanız gerekir: (a) kırpma parametrelerini orijinal giriş videosuna göre seçin ve (b) kırpılan videoya göre kod ayarlarınızı seçin. Kod ayarlarınızı kırpılmış videoyla eşleştirmezseniz, çıktı beklediğiniz gibi olmayacaktır.

[Aşağıdaki](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) konu, MES ile kodlama işinin nasıl oluşturulacağını ve kodlama görevi için özel bir ön anın nasıl belirtilen gibi olduğunu gösterir. 

## <a name="creating-a-custom-preset"></a>Özel bir ön ayar oluşturma
Diyagramda gösterilen örnekte:

1. Orijinal giriş 1920x1080
2. Giriş çerçevesi merkezli 1440x1080 çıktısına kırpılması gerekir
3. Bu, (1920 – 1440)/2 = 240 x ofset ve sıfır ın Y mahsupu anlamına gelir
4. Kırpma dikdörtgeninin Genişliği ve Yüksekliği sırasıyla 1440 ve 1080'dir
5. Kod aşamasında, sormak üç katman üretmektir, çözünürlükleri 1440x1080, 960x720 ve 480x360, sırasıyla

### <a name="json-preset"></a>JSON hazır
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
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
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
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
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
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


## <a name="restrictions-on-cropping"></a>Kırpma ile ilgili kısıtlamalar
Kırpma özelliği manuel olması içindir. Giriş videonuzu, ilgili kareleri seçmenize, kırpma dikdörtgeni için uzaklıkları belirlemek için imleci konumlandırmanıza ve söz konusu video için ayarlanan kodlama ön ayarını belirlemenize vb. olanak tanıyan uygun bir düzenleme aracına yüklemeniz gerekir. Bu özellik, giriş videonuzdaki kara posta kutusu/sütun sınırlarının otomatik olarak algılanması ve kaldırılması gibi şeyleri etkinleştirmek için değildir.

Aşağıdaki kısıtlamalar kırpma özelliği için geçerlidir. Bunlar karşılanmazsa, Kod Görevi başarısız olabilir veya beklenmeyen bir çıktı üretebilir.

1. Kırpma dikdörtgeninin koordinatları ve boyutu giriş videosuna sığmalı
2. Yukarıda belirtildiği gibi, kod ayarlarındaki Genişlik & Yüksekliği kırpılmış videoya karşılık
3. Kırpma, yatay modda çekilen videolar için geçerlidir (örn. dikey olarak veya dikey olarak tutulan bir akıllı telefonla kaydedilen videolar için geçerli değildir)
4. Kare piksellerle yakalanan aşamalı video ile en iyi şekilde çalışır

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Sonraki adım
AMS tarafından sunulan harika özellikler hakkında bilgi edinmenize yardımcı olmak için Azure Medya Hizmetleri öğrenme yollarına bakın.  

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
