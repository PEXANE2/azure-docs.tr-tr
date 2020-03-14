---
title: Media Encoder Standard biçimleri ve codec bileşenleri-Azure
description: Bu makale Media Encoder Standard biçimlerine ve codec larına genel bakış sağlar.
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
ms.date: 03/18/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: e2ccce13e4ef09426d0f3a02dcbce2f330b0ead8
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79251030"
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standard Biçimleri ve Kodlayıcılar

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Media Services sürümünü seçin:"]
> * [Sürüm 2](media-services-media-encoder-standard-formats.md)
> * [Sürüm 3](../latest/media-encoder-standard-formats.md)

Bu belge, Media Encoder Standard ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosyası biçimlerinin bir listesini içerir.

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri
| Dosya biçimleri (dosya uzantıları) | Destekleniyor |
| --- | --- |
| FLV (H., ve AAC codec bileşenleri ile) (. FLV) |Yes |
| MXF (. MXF) |Yes |
| GXF (. GXF) |Yes |
| MPEG2-PS, MPEG2-TS, 3GP (. TS,. PS,. 3GP,. 3GPP,. mpg) |Yes |
| Windows Media videosu (WMV)/ASF (. wmv,. asf) |Yes |
| AVI (sıkıştırılmamış 8bit/10bit) (. avi) |Yes |
| MP4 (. mp4,. M4A,. m4v)/ISMV (. ISA,. ismv) |Yes |
| [Microsoft dijital video kaydı (DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (. DVR-MS) |Yes |
| Matroska/webd (. MKV) |Yes |
| DALGA/WAV (. wav) |Yes |
| QuickTime (. mov) |Yes |

> [!NOTE]
> Yukarıda daha yaygın olarak karşılaşılan dosya uzantılarının listesi bulunur. Media Encoder Standard diğerlerini destekler (örneğin:. m2ts,. mpeg2video,. qt). Bir dosyayı kodlamaya çalışırsanız ve desteklenmeyen biçim hakkında bir hata iletisi alırsanız, görüşlerinizi [buraya](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/)girin.
> 
> 

### <a name="audio-formats-in-input-containers"></a>Giriş kapsayıcılarındaki ses biçimleri
Media Encoder Standard, giriş kapsayıcılarında aşağıdaki ses biçimlerinin kullanılmasını destekler:

* Araya eklemeli stereo veya 5,1 örnekleri ile ses parçaları bulunan MXF, GXF ve QuickTime dosyaları

or

* Sesin ayrı PCM parçaları olarak gerçekleştirildiği, ancak kanal eşlemesinin (stereo veya 5,1) dosya meta verilerinden çıkarılabilen MXF, GXF ve QuickTime dosyaları

## <a name="input-video-codecs"></a>Giriş video codec bileşenleri
| Giriş video codec bileşenleri | Destekleniyor |
| --- | --- |
| AVC 8 bit/10 bit, AVCIntra dahil olmak üzere 4:2:2 'e kadar |8 bit 4:2:0 ve 4:2:2 |
| AVID DNxHD (MXF içinde) |Yes |
| DVCPro/DVCProHD (in MXF) |Yes |
| Dijital video (DV) (AVI dosyaları) |Yes |
| JPEG 2000 |Yes |
| MPEG-2 (422 profile ve yüksek düzeyine kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil) |En fazla 422 profil |
| MPEG-1 |Yes |
| VC-1/WMV9 |Yes |
| Canopus HQ/HQX |Hayır |
| MPEG-4 Bölüm 2 |Yes |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Yes |
| YUV420 sıkıştırılmamış veya Mezzanine |Yes |
| Apple ProRes 422 |Yes |
| Apple ProRes 422 LT |Yes |
| Apple ProRes 422 HQ |Yes |
| Apple ProRes proxy |Yes |
| Apple ProRes 4444 |Yes |
| Apple ProRes 4444 XQ |Yes |
| HEVC/H. 265| Ana ve ana 10 (&#42;) profiller<br/>10. ana profil desteği, 8bit 4:2:0 içeriğine yöneliktir. |

## <a name="input-audio-codecs"></a>Giriş sesi codec bileşenleri
| Giriş sesi codec bileşenleri | Destekleniyor |
| --- | --- |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5,1 'e kadar) |Yes |
| MPEG Katman 2 |Yes |
| MP3 (MPEG-1 ses katmanı 3) |Yes |
| Windows Media Audio |Yes |
| WAV/PCM |Yes |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Yes |
| [Opus 'lar](https://go.microsoft.com/fwlink/?LinkId=822667) |Yes |
| [Vorb</a>](https://en.wikipedia.org/wiki/Vorbis) |Yes |
| AMR (Uyarlamalı çok hızlı) |Yes |
| AES (SMPTE 331M ve 302M, AES3-2003) |Hayır |
| Dolby® E |Hayır |
| Dolby® Digital (AC3) |Hayır |
| Dolby® Digital Plus (E-AC3) |Hayır |

## <a name="output-formats-and-codecs"></a>Çıkış biçimleri ve codec bileşenleri
Aşağıdaki tabloda, dışarı aktarma için desteklenen codec bileşenleri ve dosya biçimleri listelenmektedir.

| Dosya Biçimi | Video codec bileşeni | Ses codec bileşeni |
| --- | --- | --- |
| MP4 <br/><br/>(çoklu bit hızı MP4 kapsayıcıları dahil) |H. (yüksek, ana ve temel profiller) |AAC-LC, HE-AAC v1, HE-AAC v2 |
| MPEG2-TS |H. (yüksek, ana ve temel profiller) |AAC-LC, HE-AAC v1, HE-AAC v2 |

## <a name="media-services-learning-paths"></a>Media Services öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
[Azure Media Services ile Isteğe bağlı Içeriği kodlama](media-services-encode-asset.md)

[Media Encoder Standard ile kodlama](media-services-dotnet-encode-with-media-encoder-standard.md)

