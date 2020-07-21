---
title: Azure isteğe bağlı medya kodlayıcılarını karşılaştırma | Microsoft Docs
description: Bu konu, **Media Encoder Standard** ve **Media Encoder Premium Workflow**kodlama yeteneklerini karşılaştırır.
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
ms.openlocfilehash: ca17ffdc2a54b25bbfd855834a366ed381a54678
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86530397"
---
# <a name="comparison-of-azure-on-demand-media-encoders"></a>Azure isteğe bağlı medya kodlayıcılarını karşılaştırma  

Bu konu, **Media Encoder Standard** ve **Media Encoder Premium Workflow**kodlama yeteneklerini karşılaştırır.

## <a name="video-and-audio-processing-capabilities"></a>Video ve ses işleme özellikleri

Aşağıdaki tabloda Media Encoder Standard (MES) ve Media Encoder Premium Workflow (MEPW) arasındaki işlevler karşılaştırılmaktadır. 

|Yetenek|Media Encoder Standard|Media Encoder Premium İş Akışı|
|---|---|---|
|Kodlama sırasında koşullu mantık Uygula<br/>(örneğin, giriş HD ise, 5,1 ses kodlayın)|Hayır|Evet|
|Kapalı açıklamalı alt yazı|No|[Evet](media-services-premium-workflow-encoder-formats.md#closed_captioning)|
|[Dolby &reg; Professional ses yüksekliği düzeltmesi](https://professional.dolby.com/product/broadcast/vm600/)<br/> Iletişim zekası ile&trade;|Hayır|Evet|
|Tarama, ters telesine|Temel|Yayın kalitesi|
|Siyah kenarlıkları Algıla ve Kaldır <br/>(pillarkutular, harf kutuları)|Hayır|Evet|
|Küçük resim oluşturma|[Evet](media-services-dotnet-generate-thumbnail-with-mes.md)|[Evet](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)|
|Videoları kırpma/kırpma ve dikiş|[Evet](media-services-advanced-encoding-with-mes.md#trim_video)|Yes|
|Ses veya videonun Yerpaylaşımları|[Evet](media-services-advanced-encoding-with-mes.md#overlay)|[Evet](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-1--overlay-an-image-on-top-of-the-video)|
|Grafiklerin Yerpaylaşımları|Görüntü kaynaklarından|Görüntü ve metin kaynaklarından|
|Birden çok ses dili parçası|Sınırlı|[Evet](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-2--multiple-audio-language-encoding)|

## <a name="billing-meter-used-by-each-encoder"></a><a id="billing"></a>Her kodlayıcı tarafından kullanılan faturalandırma ölçümü
| Medya Işlemcisi adı | Geçerli fiyatlandırma | Notlar |
| --- | --- | --- |
| **Media Encoder Standard** |KODLAYıCıSı |Kodlama görevleri, [burada][1]belirtilen HıZDA, kodlayıcı sütunu altında, çıktı olarak üretilen tüm medya dosyalarının dakika cinsinden toplam süresine göre ücretlendirilecektir. |
| **Media Encoder Premium İş Akışı** |PREMIUM KODLAYıCı |Kodlama görevleri, [burada][1]BELIRTILEN HıZDA PREMIUM kodlayıcı sütunu altında, çıktı olarak üretilen tüm medya dosyalarının dakika cinsinden toplam süresine göre ücretlendirilecektir. |

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri
| Giriş Kapsayıcısı/Dosya Biçimleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| Adobe &reg; Flash &reg; F4V |Yes |Yes |
| MXF/SMPTE 377D |Yes |Yes |
| GXF |Yes |Yes |
| MPEG-2 aktarım akışları |Yes |Yes |
| MPEG-2 program akışları |Yes |Yes |
| MPEG-4/MP4 |Yes |Yes |
| Windows Media/ASF |Yes |Yes |
| AVI (sıkıştırılmamış 8bit/10bit) |Yes |Yes |
| 3GPP/3GPP2 |Evet |Hayır |
| Kesintisiz Akış dosya biçimi (pff 1,3) |Evet |Hayır |
| [Microsoft dijital video kaydı (DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) |Evet |Hayır |
| Matroska/webd |Evet |Hayır |
| QuickTime (.mov) |Evet |Hayır |

## <a name="input-video-codecs"></a>Giriş video codec bileşenleri
| Giriş Video Codec Bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AVC 8 bit/10 bit, 4:2:2’ye kadar, AVCIntra dahil |8 bit 4:2:0 ve 4:2:2 |Yes |
| Avid DNxHD (MXF biçiminde) |Yes |Yes |
| DVCPro/DVCProHD (MXF biçiminde) |Yes |Yes |
| JPEG2000 |Yes |Yes |
| MPEG-2 (422 profile ve yüksek düzeyine kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs ve D10 gibi çeşitler dahil &reg; ) |422 Profiline kadar |Yes |
| MPEG-1 |Yes |Yes |
| Windows Media videosu/VC-1 |Yes |Yes |
| Canopus HQ/HQX |Hayır |Hayır |
| MPEG-4 Bölüm 2 |Evet |No |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Evet |No |
| Apple ProRes 422 |Evet |No |
| Apple ProRes 422 LT |Evet |No |
| Apple ProRes 422 HQ |Evet |No |
| Apple ProRes Proxy |Evet |No |
| Apple ProRes 4444 |Evet |No |
| Apple ProRes 4444 XQ |Evet |No |
| HEVC/H. 265|Ana profil|Ana ve ana 10 profili|

## <a name="input-audio-codecs"></a>Giriş sesi codec bileşenleri
| Giriş Ses Codec Bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AES (SMPTE 331M ve 302M, AES3-2003) |No |Evet |
| Dolby &reg; E |No |Evet |
| Dolby &reg; Digital (AC3) |No |Evet |
| Dolby &reg; Digital Plus (E-AC3) |No |Evet |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar) |Yes |Yes |
| MPEG Katman 2 |Yes |Yes |
| MP3 (MPEG-1 Ses Katmanı 3) |Yes |Yes |
| Windows Media Ses |Yes |Yes |
| WAV/PCM |Yes |Yes |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Evet |No |
| [Opus](https://en.wikipedia.org/wiki/Opus_\(audio_format\)) |Evet |No |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Evet |No |

## <a name="output-containerfile-formats"></a>Çıkış kapsayıcısı/dosya biçimleri
| Çıkış kapsayıcısı/dosya biçimleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| Adobe &reg; Flash &reg; F4V |No |Evet |
| MXF (OP1a, XDCAM ve AS02) |No |Evet |
| DPP (AS11 dahil) |Hayır |Evet |
| GXF |Hayır |Evet |
| MPEG-4/MP4 |Yes |Yes |
| MPEG-TS |Yes |Yes |
| Windows Media/ASF |Hayır |Evet |
| AVI (sıkıştırılmamış 8bit/10bit) |Hayır |Evet |
| Kesintisiz Akış dosya biçimi (pff 1,3) |Hayır |Evet |

## <a name="output-video-codecs"></a>Çıkış video codec bileşenleri
| Çıkış video codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AVC (H., 8 bit; en fazla yüksek profile, düzey 5,2; 4K Ultra HD; AVC Içi) |Yalnızca 8 bit 4:2:0 |Yes |
| HEVC (H. 265; 8 bit ve 10 bit;)  |Hayır |Evet |
| Avid DNxHD (MXF biçiminde) |Hayır |Evet |
| MPEG-2 (422 profile ve yüksek düzeyine kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs ve D10 gibi çeşitler dahil &reg; ) |Hayır |Evet |
| MPEG-1 |Hayır |Evet |
| Windows Media videosu/VC-1 |Hayır |Evet |
| JPEG küçük resim oluşturma |Yes |Yes |
| PNG küçük resim oluşturma |Yes |Yes |
| BMP küçük resim oluşturma |Evet |Hayır |

## <a name="output-audio-codecs"></a>Çıkış ses codec bileşenleri
| Çıkış ses codec bileşenleri | Media Encoder Standard | Media Encoder Premium İş Akışı |
| --- | --- | --- |
| AES (SMPTE 331M ve 302M, AES3-2003) |Hayır |Evet |
| Dolby &reg; Digital (AC3) |Hayır |Evet |
| Dolby &reg; Digital Plus (E-AC3) 7,1 'e kadar |Hayır |Evet |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar) |Yes |Yes |
| MPEG Katman 2 |Hayır |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Hayır |Evet |
| Windows Media Ses |Hayır |Evet |

>[!NOTE]
>Dolby &reg; Digital (AC3) olarak kodlarsanız, çıkış yalnızca BIR ISO MP4 dosyasına yazılabilir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler:
* [Media Encoder Standard ön ayarlarını özelleştirerek gelişmiş kodlama görevleri gerçekleştirin](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
