---
title: Media Encoder Premium İş Akışı biçimleri ve codec' leri | Microsoft Dokümanlar
description: Bu konu, Media Encoder Premium İş Akışı Biçimleri formatlarına ve codec'lere genel bir bakış sağlar
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
ms.date: 03/19/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: 87cd7c63939331190530a46071a6b4c40480562f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79269763"
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium İş Akışı biçimleri ve codec'leri

> [!NOTE]
> Media Encoder Premium İş Akışı medya işlemcisi bu konuda tartışılan Çin'de mevcut değildir. 

Bu belge, **Media Encoder Premium İş Akışı** kodlayıcısının genel önizleme sürümü tarafından desteklenen giriş ve çıktı dosya biçimlerinin ve codec'lerinin bir listesini içerir.

[Media Encoder Premium İş Akışı Giriş Biçimleri ve Codec'ler](#input_formats)

Media Encoder Premium İş Akışı Çıktı Biçimleri ve Codec'ler

**Media Encoder Premium İş Akışı,** [bu](#closed_captioning) bölümde açıklanan kapalı altyazıyı destekler. 

## <a name="media-encoder-premium-workflow-input-formats-and-codecs"></a><a id="input_formats"></a>Media Encoder Premium İş Akışı Giriş Biçimleri ve Codec'ler

Aşağıdaki bölümde, bu ortam işlemcisinin giriş olarak desteklediği kodc'lar ve dosya biçimleri listelenmektedir.

### <a name="input-containerfile-formats"></a>Giriş Kapsayıcısı/Dosya Biçimleri

* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2 Ulaşım Akışları
* MPEG-2 Program Akışları
* MPEG-4/MP4
* Windows Medya/ASF
* AVI (Sıkıştırılmamış 8bit/10bit)

### <a name="input-video-codecs"></a>Giriş Video Codec Bileşenleri

* AVC 8 bit/10 bit, 4:2:2’ye kadar, AVCIntra dahil
* Avid DNxHD (MXF biçiminde)
* DVCPro/DVCProHD (MXF biçiminde)
* HEVC/H.265, Ana ve Ana 10 Profil
* JPEG2000
* MPEG-2 (422'ye kadar Profil ve Yüksek Seviye; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi türevleri dahil)
* MPEG-1
* Windows Media Video/VC-1

### <a name="input-audio-codecs"></a>Giriş Ses Codec Bileşenleri

* AES (SMPTE 331M ve 302M, AES3-2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar)
* MPEG Katman 2
* MP3 (MPEG-1 Ses Katmanı 3)
* Windows Media Ses
* WAV/PCM

## <a name="media-encoder-premium-workflow-output-formats-and-codecs"></a><a id="output_format"></a>Media Encoder Premium İş Akışı Çıktı Biçimleri ve Codec'ler

Aşağıdaki bölümde, bu ortam işlemcisinden çıktı olarak desteklenen kodlar ve dosya biçimleri listelenir.

### <a name="output-containerfile-formats"></a>Çıktı Kapsayıcısı/Dosya Biçimleri

* Adobe® Flash® F4V
* MXF (OP1a, XDCAM ve AS02)
* DPP (AS11 dahil)
* GXF
* MPEG-4/MP4
* Windows Medya/ASF
* AVI (Sıkıştırılmamış 8bit/10bit)
* Düzgün Akış Dosya Biçimi (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Çıkış Video Codec'leri

* AVC (H.264; 8-bit; yüksek profile kadar, Seviye 5.2; 4K Ultra HD; AVC İçi)
* Avid DNxHD (MXF biçiminde)
* DVCPro/DVCProHD (MXF biçiminde)
* MPEG-2 (422'ye kadar Profil ve Yüksek Seviye; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi türevleri dahil)
* MPEG-1
* Windows Media Video/VC-1
* JPEG küçük resim oluşturma
* HEVC (H.265; 8 bit ve 10 bit, Ana ve Ana 10 Profil)


### <a name="output-audio-codecs"></a>Çıkış Ses Codecs

* AES (SMPTE 331M ve 302M, AES3-2003)
* Dolby® Digital (AC3)
* Dolby® Digital Plus (E-AC3) 7,1'e kadar
* AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar)
* MPEG Katman 2
* MP3 (MPEG-1 Ses Katmanı 3)
* Windows Media Ses

>[!NOTE]
>Dolby® Digital (AC3) koduyla kodlarsanız, çıktı yalnızca bir ISO MP4 dosyasına yazılabilir.

## <a name="support-for-closed-captioning"></a><a id="closed_captioning"></a>Kapalı Altyazı Desteği

Yutma üzerine, **Media Encoder Premium İş Akışı** destekler:

1. SCC dosyaları
2. SMPTE-TT dosyaları
3. CEA-608/CEA-708 – kullanıcı verileri olarak (H.264 temel akışları, ATSC/53, SCTE20 SEI mesajları) veya MXF/GXF dosyalarında yardımcı veri olarak taşınır
4. STL altyazı dosyaları

Çıktıda aşağıdaki seçenekler mevcuttur:

1. CEA-608 - CEA-708 çevirisi
2. CEA-608/CEA-708 geçiş (H.264 temel akışlarının SEI mesajlarına gömülü veya MXF dosyalarında yardımcı veri olarak taşınır)
3. Scc
4. SMPTE Zamanlı Metin (KaynakTAN CEA-608 Başına SMPTE RP2052; DFXP dosya oluşturma dahil)
5. SRT Altyazı dosyası
6. DVB altyazı akışları

> [!NOTE]
> Yukarıdaki çıktı biçimlerinin tümü Azure Medya Hizmetleri'nde akış yoluyla teslim için desteklenmez.

## <a name="known-issues"></a>Bilinen sorunlar

Giriş videonuz kapalı altyazı içermiyorsa, çıkış Varlığı yine de boş bir TTML dosyası içerir. 

## <a name="need-help"></a>Yardıma mı ihtiyacınız var?

[Yeni destek isteğine](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) yönlendirerek destek bileti açabilirsiniz
## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

