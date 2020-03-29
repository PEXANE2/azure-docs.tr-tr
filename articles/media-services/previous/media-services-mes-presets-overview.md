---
title: Ortam Kodlayıcı Standardı (MES) için Görev Ön Ayarları | Microsoft Dokümanlar
description: Konu, Media Encoder Standard (MES) için hizmet tanımlı örnek hazır ayarları verir ve genel olarak verir.
author: Juliako
manager: femila
editor: johndeu
services: media-services
documentationcenter: ''
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 50c52369a5a957a4dd6279cac5079e2dea023106
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61463412"
---
# <a name="sample-presets-for-media-encoder-standard-mes"></a>Ortam Kodlayıcı Standardı (MES) için Örnek Hazır Ayarlar

**Media Encoder Standard,** kodlama işleri oluştururken kullanabileceğiniz önceden tanımlanmış sistem kodlama ön ayarlarını tanımlar. Medya Hizmetleri ile akış için bir video kodlamak istiyorsanız , "Uyarlanabilir Akış" önceden ayarlanmış kullanmanız önerilir. Bu ön ayarı belirttiğiniz zaman, Media Encoder Standard [otomatik olarak bir bitrate merdiveni oluşturur.](media-services-autogen-bitrate-ladder-with-mes.md) 

### <a name="creating-custom-presets-from-samples"></a>Örneklerden Özel Hazır Ayarlar Oluşturma
Medya Hizmetleri, özel kodlama gereksinimlerinizi ve gereksinimlerinizi karşılamak için tüm değerleri ön ayarlarda özelleştirmeyi tam olarak destekler. Bir kodlama ön ayarını özelleştirmeniz gerekiyorsa, özel yapılandırmanız için şablon olarak bu bölümde sağlanan aşağıdaki sistem hazır ayarlarından biriyle başlamalısınız. Bu hazır ayarlardaki her öğenin ne anlama geldiğini ve her öğe için geçerli değerleri açıklamaları için [Media Encoder Standart şeması](media-services-mes-schema.md) konusuna bakın.  
  
> [!NOTE]
>  4k kodları için önceden ayarlanmış kullanırken, `S3` ayrılmış birim türünü almalısınız. Daha fazla bilgi [için, Kodlamayı Ölçeklendirme ye bakın.](https://azure.microsoft.com/documentation/articles/media-services-portal-encoding-units)  

#### <a name="video-rotation-default-setting-in-presets"></a>Hazır ayarlarda Video Döndürme Varsayılan Ayarı:
Media Encoder Standard ile çalışırken, video döndürme varsayılan olarak etkinleştirilir. Videonuz Dikey modunda bir mobil aygıta kaydedildiyse, bu ön ayarlar kodlamadan önce bunları Yatay moduna döndürür.
 
## <a name="available-presets"></a>Kullanılabilir hazır ayarlar: 

 [H264 Çoklu Bitrate 1080p Audio 5.1,](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) 6000 kbps ile 400 kbps arasında değişen 8 GOP uyumlu MP4 dosyası ve AAC 5.1 ses seti üretir.  
  
 [H264 Çoklu Bitrate 1080p,](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) 6000 kbps ile 400 kbps arasında değişen 8 GOP uyumlu MP4 dosyası ve stereo AAC ses seti üretir.  
  
 [iOS için H264 Çoklu Bitrate 16x9,](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) 8500 kbps ile 200 kbps arasında değişen 8 GOP uyumlu MP4 dosyası ve stereo AAC ses seti üretir.  
  
 [H264 Çoklu Bitrate 16x9 SD Audio 5.1,](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) 1900 kbps ile 400 kbps arasında değişen 5 GOP uyumlu MP4 dosyası ve AAC 5.1 ses seti üretir.  
  
 [H264 Çoklu Bitrate 16x9 SD,](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) 1900 kbps ile 400 kbps arasında değişen 5 GOP uyumlu MP4 dosyası ve stereo AAC ses seti üretir.  
  
 [H264 Çoklu Bitrate 4K Audio 5.1,](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) 20000 kbps ile 1000 kbps arasında değişen 12 GOP uyumlu MP4 dosyası ve AAC 5.1 ses kümesi üretir.  
  
 [H264 Çoklu Bitrate 4K,](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) 20000 kbps ile 1000 kbps arasında değişen 12 GOP uyumlu MP4 dosyası ve stereo AAC ses kümesi üretir.  
  
 [iOS için H264 Çoklu Bitrate 4x3,](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) 8500 kbps ile 200 kbps arasında değişen 8 GOP uyumlu MP4 dosyası ve stereo AAC ses seti üretir.  
  
 [H264 Çoklu Bitrate 4x3 SD Audio 5.1,](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) 1600 kbps ile 400 kbps arasında değişen 5 GOP uyumlu MP4 dosyası ve AAC 5.1 ses seti üretir.  
  
 [H264 Çoklu Bitrate 4x3 SD,](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) 1600 kbps ile 400 kbps arasında değişen 5 GOP uyumlu MP4 dosyası ve stereo AAC ses seti üretir.  
  
 [H264 Çoklu Bitrate 720p Audio 5.1,](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) 3400 kbps ile 400 kbps arasında değişen 6 GOP uyumlu MP4 dosyası ve AAC 5.1 ses seti üretir.  
  
 [H264 Çoklu Bitrate 720p,](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) 3400 kbps ile 400 kbps arasında değişen 6 GOP uyumlu MP4 dosyası ve stereo AAC ses seti üretir.  
  
 [H264 Tek Bitrate 1080p Ses 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) 6750 kbps bithızı ve AAC 5.1 ses ile tek bir MP4 dosyası üretir.  
  
 [H264 Single Bitrate 1080p,](media-services-mes-preset-H264-Single-Bitrate-1080p.md) 6750 kbps bit hızına ve stereo AAC sese sahip tek bir MP4 dosyası üretir.  
  
 [H264 Tek Bitrate 4K Audio 5.1,](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) 18000 kbps bithızı na ve AAC 5.1 sese sahip tek bir MP4 dosyası üretir.  
  
 [H264 Single Bitrate 4K,](media-services-mes-preset-H264-Single-Bitrate-4K.md) 18000 kbps bit hızına ve stereo AAC sesine sahip tek bir MP4 dosyası üretir.  
  
 [H264 Tek Bitrate 4x3 SD Audio 5.1,](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) 1800 kbps bit hızına ve AAC 5.1 sese sahip tek bir MP4 dosyası üretir.  
  
 [H264 Tek Bitrate 4x3 SD,](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) 1800 kbps bit hızına ve stereo AAC sese sahip tek bir MP4 dosyası üretir.  
  
 [H264 Tek Bitrate 16x9 SD Ses 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) 2200 kbps bithızı ve AAC 5.1 ses ile tek bir MP4 dosyası üretir.  
  
 [H264 Single Bitrate 16x9 SD,](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) 2200 kbps bit hızına ve stereo AAC sese sahip tek bir MP4 dosyası üretir.  
  
 [H264 Tek Bitrate 720p Audio 5.1,](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) 4500 kbps bit hızına ve AAC 5.1 sese sahip tek bir MP4 dosyası üretir.  
  
 Android ön ayar [için H264 Tek Bitrate 720p](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) 2000 kbps bithızı ve stereo AAC ile tek bir MP4 dosyası üretir.  
  
 [H264 Single Bitrate 720p,](media-services-mes-preset-H264-Single-Bitrate-720p.md) 4500 kbps bit hızına ve stereo AAC sese sahip tek bir MP4 dosyası üretir.  
  
 [Android için H264 Tek Bitrate Yüksek Kaliteli SD](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) 500 kbps bitrate ile tek bir MP4 dosyası üretir, ve stereo AAC ses...  
  
 [Android için H264 Tek Bitrate Düşük Kaliteli SD](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) 56 kbps bitrate ve stereo AAC ses ile tek bir MP4 dosyası üretir.  
  
 Medya Hizmetleri kodlayıcıları ile ilgili daha fazla bilgi için Azure [Medya Hizmetleri ile İsteğe Bağlı Kodlama'ya](https://azure.microsoft.com/documentation/articles/media-services-encode-asset/)bakın.
