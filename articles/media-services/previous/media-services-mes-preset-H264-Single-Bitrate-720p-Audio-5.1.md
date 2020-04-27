---
title: H264 tekli bit hızı 720p ses 5,1 | Microsoft Docs
description: Bu konu, **H264 tekli bit hızı 720p ses 5,1** görevi önayarıyla genel bir bakış sunar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: ba2bfa57-3c87-4b1b-b91b-58f9108f4e85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: aad0a5f0e521d0608c1797ea8c1225c48240b391
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "61129568"
---
# <a name="h264-single-bitrate-720p-audio-51"></a>H264 Single Bitrate 720p Audio 5.1
`Media Encoder Standard`kodlama işleri oluştururken kullanabileceğiniz bir kodlama önayarları kümesi tanımlar. Medya dosyanızı kodlamak istediğiniz biçimi `preset name` belirtmek için bir kullanabilirsiniz. Ya da kendi JSON veya XML tabanlı önayarlarınızı oluşturabilirsiniz (UTF-8 veya UTF-16 kodlaması kullanarak). Daha sonra özel önayar kodlayıcıya geçirebilirsiniz. Bu `Media Encoder Standard` kodlayıcı tarafından desteklenen tüm önceden ayarlanmış adların listesi için bkz. [Media Encoder Standard Için görev önayarları](media-services-mes-presets-overview.md).  
  
 Bu konu, `H264 Single Bitrate 720p Audio 5.1` XML ve JSON biçimindeki ön ayarı gösterir.  
  
 Bu önceden tanımlı, 4500 kbps ve AAC 5,1 ses hızında tek bir MP4 dosyası üretir. Bu önayar için profil, bit hızı, örnekleme oranı vb. hakkında ayrıntılı bilgi için, aşağıda tanımlanan XML veya JSON 'u inceleyin. Her öğenin anlamı ve her öğe için geçerli değerler hakkında açıklamalar için [Media Encoder Standard şemasına](media-services-mes-schema.md)bakın.  
  
 XML  
  
```  
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
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>6</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>384</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:02",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 4500,  
          "MaxBitrate": 4500,  
          "BufferWindow": "00:00:05",  
          "Width": 1280,  
          "Height": 720,  
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
      "Channels": 6,  
      "SamplingRate": 48000,  
      "Bitrate": 384,  
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
```
