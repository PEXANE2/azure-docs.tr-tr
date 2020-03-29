---
title: Android için H264 Tek Bitrate Yüksek Kaliteli SD | Microsoft Dokümanlar
description: Konu Android görev önceden ayarlanmış **için H264 Tek Bitrate Yüksek Kaliteli SD** genel bir bakış verir.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 4a190f24-ec6a-47e7-8bca-6294e064b15b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 217c4874f0375aeb4d80162af1b8453a3f7f625f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61463523"
---
# <a name="h264-single-bitrate-high-quality-sd-for-android"></a>Android için H264 Single Bitrate Yüksek Kaliteli SD
`Media Encoder Standard`kodlama işleri oluştururken kullanabileceğiniz bir kodlama ön ayarları kümesini tanımlar. A'yı, `preset name` medya dosyanızı kodlamak istediğiniz biçimi belirtmek için de kullanabilirsiniz. Veya kendi JSON veya XML tabanlı hazır ayarlarınızı (UTF-8 veya UTF-16 kodlaması kullanarak) oluşturabilirsiniz. Daha sonra özel ön ayarını kodlayıcıya geçirirsiniz. Bu `Media Encoder Standard` kodlayıcı tarafından desteklenen tüm önceden ayarlanmış adların listesi [için, Ortam Kodlayıcı Standardı için Görev Hazır Ayarları'na](media-services-mes-presets-overview.md)bakın.  
  
 Bu `H264 Single Bitrate High Quality SD for Android` konu, XML ve JSON biçiminde önceden ayarlanmışı gösterir.  
  
 Bu ön ayar, 500 kbps bithızı na ve stereo AAC sese sahip tek bir MP4 dosyası üretir. Bu önceden ayarlanmış profil, bitrate, örnekleme hızı vb. hakkında ayrıntılı bilgi için aşağıda tanımlanan XML veya JSON'u inceleyin. Bu hazır ayarlardaki her öğenin ne anlama geldiğini ve her öğe için geçerli değerleri açıklamaları için [Media Encoder Standart şeması](media-services-mes-schema.md) konusuna bakın.  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="https://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:05</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>500</Bitrate>  
          <Width>480</Width>  
          <Height>360</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Baseline</Profile>  
          <Level>3</Level>  
          <BFrames>0</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>false</AdaptiveBFrame>  
          <EntropyMode>Cavlc</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>500</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
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
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:05",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Baseline",  
          "Level": "3",  
          "Bitrate": 500,  
          "MaxBitrate": 500,  
          "BufferWindow": "00:00:05",  
          "Width": 480,  
          "Height": 360,  
          "ReferenceFrames": 3,  
          "EntropyMode": "Cavlc",  
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
```
