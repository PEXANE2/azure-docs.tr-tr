---
title: Azure'un isteğe bağlı medya kodlayıcıları üzerine karşılaştırılması | Microsoft Dokümanlar
description: Bu **konu, Media Encoder Standard** ve **Media Encoder Premium İş Akışı**kodlama yeteneklerini karşılaştırır.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: a79437c0-4832-423a-bca8-82632b2c47cc
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: 4767f7bb5ba02c838c0e21721e55a6564a14acd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69016650"
---
# <a name="comparison-of-azure-on-demand-media-encoders"></a>Azure'un isteğe bağlı medya kodlayıcıları üzerine karşılaştırılması  

Bu **konu, Media Encoder Standard** ve **Media Encoder Premium İş Akışı**kodlama yeteneklerini karşılaştırır.

## <a name="video-and-audio-processing-capabilities"></a>Video ve ses işleme özellikleri

Aşağıdaki tablo, Media Encoder Standard (MES) ile Media Encoder Premium İş Akışı (MEPW) arasındaki işlevselliği karşılaştırın. 

|Özellik|Media Encoder Standard|Media Encoder Premium İş Akışı|
|---|---|---|
|Kodlama yaparken koşullu mantık uygulayın<br/>(örneğin, giriş HD ise, 5.1 sesi kodlayın)|Hayır|Evet|
|Kapalı açıklamalı alt yazı|Hayır|[Evet](media-services-premium-workflow-encoder-formats.md#closed_captioning)|
|[Dolby® Profesyonel Yükseklik Düzeltme](https://www.dolby.com/us/en/technologies/dolby-professional-loudness-solutions.pdf)<br/> Diyalog İstihbaratı ile™|Hayır|Evet|
|De-interlacing, ters telecine|Temel|Yayın kalitesi|
|Siyah kenarlıkları algılama ve kaldırma <br/>(sütun kutuları, mektup kutuları)|Hayır|Evet|
|Küçük resim üretimi|[Evet](media-services-dotnet-generate-thumbnail-with-mes.md)|[Evet](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)|
|Videoların kırpma/kırpma ve dikiş|[Evet](media-services-advanced-encoding-with-mes.md#trim_video)|Evet|
|Ses veya video bindirmeleri|[Evet](media-services-advanced-encoding-with-mes.md#overlay)|[Evet](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-1--overlay-an-image-on-top-of-the-video)|
|Grafik kaplamaları|Görüntü kaynaklarından|Resim ve metin kaynaklarından|
|Birden çok ses dili parçası|Sınırlı|[Evet](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-2--multiple-audio-language-encoding)|

## <a name="billing-meter-used-by-each-encoder"></a><a id="billing"></a>Her kodlayıcı tarafından kullanılan faturalama ölçer
| Medya İşlemci Adı | Geçerli Fiyatlandırma | Notlar |
| --- | --- | --- |
| **Media Encoder Standard** |Kodlayıcı |Kodlama Görevleri, çıktı olarak üretilen tüm medya dosyalarının, [burada][1]encoder sütunu altında belirtilen oranda, dakika cinsinden toplam süresine göre ücretlendirilir. |
| **Media Encoder Premium İş Akışı** |PREMIUM KODLAYıCı |Kodlama Görevleri, çıktı olarak üretilen tüm medya dosyalarının toplam süresine, dakikalar içinde, [burada][1]belirtilen oranda , PREMIUM ENCODER sütunu altında ücretlendirilir. |

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri
| Giriş Kapsayıcısı/Dosya Biçimleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| Adobe® Flash® F4V |Evet |Evet |
| MXF/SMPTE 377M |Evet |Evet |
| GXF |Evet |Evet |
| MPEG-2 Ulaşım Akışları |Evet |Evet |
| MPEG-2 Program Akışları |Evet |Evet |
| MPEG-4/MP4 |Evet |Evet |
| Windows Medya/ASF |Evet |Evet |
| AVI (Sıkıştırılmamış 8bit/10bit) |Evet |Evet |
| 3GPP/3GPP2 |Evet |Hayır |
| Düzgün Akış Dosya Biçimi (PIFF 1.3) |Evet |Hayır |
| [Microsoft Dijital Video Kaydı(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) |Evet |Hayır |
| Matroska/WebM |Evet |Hayır |
| QuickTime (.mov) |Evet |Hayır |

## <a name="input-video-codecs"></a>Giriş video codec'leri
| Giriş Video Codec Bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AVC 8 bit/10 bit, 4:2:2’ye kadar, AVCIntra dahil |8 bit 4:2:0 ve 4:2:2 |Evet |
| Avid DNxHD (MXF biçiminde) |Evet |Evet |
| DVCPro/DVCProHD (MXF biçiminde) |Evet |Evet |
| JPEG2000 |Evet |Evet |
| MPEG-2 (422'ye kadar Profil ve Yüksek Seviye; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi türevleri dahil) |422 Profiline kadar |Evet |
| MPEG-1 |Evet |Evet |
| Windows Media Video/VC-1 |Evet |Evet |
| Canopus HQ/HQX |Hayır |Hayır |
| MPEG-4 Bölüm 2 |Evet |Hayır |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Evet |Hayır |
| Apple ProRes 422 |Evet |Hayır |
| Apple ProRes 422 LT |Evet |Hayır |
| Apple ProRes 422 HQ |Evet |Hayır |
| Apple ProRes Proxy |Evet |Hayır |
| Apple ProRes 4444 |Evet |Hayır |
| Apple ProRes 4444 XQ |Evet |Hayır |
| HEVC/H.265|Ana Profil|Ana ve Ana 10 Profil|

## <a name="input-audio-codecs"></a>Ses codec'lerini girin
| Giriş Ses Codec Bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AES (SMPTE 331M ve 302M, AES3-2003) |Hayır |Evet |
| Dolby® E |Hayır |Evet |
| Dolby® Digital (AC3) |Hayır |Evet |
| Dolby® Digital Plus (E-AC3) |Hayır |Evet |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar) |Evet |Evet |
| MPEG Katman 2 |Evet |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Evet |Evet |
| Windows Media Ses |Evet |Evet |
| WAV/PCM |Evet |Evet |
| [Flac](https://en.wikipedia.org/wiki/FLAC)</a> |Evet |Hayır |
| [Opus](https://en.wikipedia.org/wiki/Opus_\(audio_format\)) |Evet |Hayır |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Evet |Hayır |

## <a name="output-containerfile-formats"></a>Çıkış kapsayıcısı/dosya biçimleri
| Çıktı Kapsayıcısı/Dosya Biçimleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| Adobe® Flash® F4V |Hayır |Evet |
| MXF (OP1a, XDCAM ve AS02) |Hayır |Evet |
| DPP (AS11 dahil) |Hayır |Evet |
| GXF |Hayır |Evet |
| MPEG-4/MP4 |Evet |Evet |
| MPEG-TS |Evet |Evet |
| Windows Medya/ASF |Hayır |Evet |
| AVI (Sıkıştırılmamış 8bit/10bit) |Hayır |Evet |
| Düzgün Akış Dosya Biçimi (PIFF 1.3) |Hayır |Evet |

## <a name="output-video-codecs"></a>Çıkış video codec'leri
| Çıkış Video Codec'leri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AVC (H.264; 8-bit; yüksek profile kadar, Seviye 5.2; 4K Ultra HD; AVC İçi) |Sadece 8 bit 4:2:0 |Evet |
| HEVC (H.265; 8-bit ve 10-bit;)  |Hayır |Evet |
| Avid DNxHD (MXF biçiminde) |Hayır |Evet |
| MPEG-2 (422'ye kadar Profil ve Yüksek Seviye; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi türevleri dahil) |Hayır |Evet |
| MPEG-1 |Hayır |Evet |
| Windows Media Video/VC-1 |Hayır |Evet |
| JPEG küçük resim oluşturma |Evet |Evet |
| PNG küçük resim oluşturma |Evet |Evet |
| BMP küçük resim oluşturma |Evet |Hayır |

## <a name="output-audio-codecs"></a>Çıkış ses codec'leri
| Çıkış Ses Codecs | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AES (SMPTE 331M ve 302M, AES3-2003) |Hayır |Evet |
| Dolby® Digital (AC3) |Hayır |Evet |
| Dolby® Digital Plus (E-AC3) 7,1'e kadar |Hayır |Evet |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar) |Evet |Evet |
| MPEG Katman 2 |Hayır |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Hayır |Evet |
| Windows Media Ses |Hayır |Evet |

>[!NOTE]
>Dolby® Digital (AC3) koduyla kodlarsanız, çıktı yalnızca bir ISO MP4 dosyasına yazılabilir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler:
* [Media Encoder Standart ön ayarlarını özelleştirerek gelişmiş kodlama görevleri gerçekleştirin](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve Sınırlamalar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
