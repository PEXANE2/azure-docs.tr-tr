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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84712301"
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standard Biçimleri ve Kodlayıcılar

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Media Services sürümünü seçin:"]
> * [Sürüm 2](media-services-media-encoder-standard-formats.md)
> * [Sürüm 3](../latest/media-encoder-standard-formats.md)

Bu belge, Media Encoder Standard ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosyası biçimlerinin bir listesini içerir.

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri
| Dosya biçimleri (dosya uzantıları) | Destekleniyor |
| --- | --- |
| FLV (H.264 ve AAC codec bileşenleriyle) (.flv) |Evet |
| MXF    (.mxf) |Evet |
| GXF    (.gxf) |Evet |
| MPEG2-PS, MPEG2-TS, 3GP (.ts, .ps, .3gp, .3gpp, .mpg) |Evet |
| Windows Media Video (WMV)/ASF (.wmv, .asf) |Evet |
| AVI (Sıkıştırılmamış 8 bit/10 bit) (.avi) |Evet |
| MP4 (.mp4, .m4a, .m4v)/ISMV (.isma, .ismv) |Evet |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Evet |
| Matroska/WebM (.mkv) |Evet |
| WAVE/WAV (.wav) |Evet |
| QuickTime (.mov) |Evet |

> [!NOTE]
> Yukarıda, daha sık karşılaşılan dosya uzantılarının listesi verilmiştir. Media Encoder Standard diğerlerini destekler (örneğin:. m2ts,. mpeg2video,. qt). Bir dosyayı kodlamaya çalışırsanız ve desteklenmeyen biçim hakkında bir hata iletisi alırsanız, görüşlerinizi [buraya](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/)girin.
> 
> 

### <a name="audio-formats-in-input-containers"></a>Giriş kapsayıcılarındaki ses biçimleri
Media Encoder Standard, giriş kapsayıcılarında aşağıdaki ses biçimlerinin kullanılmasını destekler:

* Araya eklemeli stereo veya 5,1 örnekleri ile ses parçaları bulunan MXF, GXF ve QuickTime dosyaları

veya

* Sesin ayrı PCM parçaları olarak taşındığı, ancak kanal eşlemesinin (stero’ya veya 5.1’e) dosya meta verilerinden çıkarılabildiği MXF, GXF ve QuickTime dosyaları

## <a name="input-video-codecs"></a>Giriş video codec bileşenleri
| Giriş video codec bileşenleri | Destekleniyor |
| --- | --- |
| AVC 8 bit/10 bit, 4:2:2’ye kadar, AVCIntra dahil |8 bit 4:2:0 ve 4:2:2 |
| Avid DNxHD (MXF biçiminde) |Evet |
| DVCPro/DVCProHD (MXF biçiminde) |Evet |
| Digital video (DV) (AVI dosyalarında) |Evet |
| JPEG 2000 |Evet |
| MPEG-2 (422 Profili ve Yüksek Düzeye kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil) |422 Profiline kadar |
| MPEG-1 |Evet |
| VC-1/WMV9 |Evet |
| Canopus HQ/HQX |Hayır |
| MPEG-4 Bölüm 2 |Evet |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Evet |
| YUV420 sıkıştırılmamış veya mezzanine |Evet |
| Apple ProRes 422 |Evet |
| Apple ProRes 422 LT |Evet |
| Apple ProRes 422 HQ |Evet |
| Apple ProRes Proxy |Evet |
| Apple ProRes 4444 |Evet |
| Apple ProRes 4444 XQ |Evet |
| HEVC/H. 265| Ana ve ana 10 (&#42;) profilleri<br/>10. ana profil desteği, 8bit 4:2:0 içeriğine yöneliktir. |

## <a name="input-audio-codecs"></a>Giriş sesi codec bileşenleri
| Giriş Ses Codec Bileşenleri | Destekleniyor |
| --- | --- |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar) |Evet |
| MPEG Katman 2 |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Evet |
| Windows Media Ses |Evet |
| WAV/PCM |Evet |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Evet |
| [Opus](https://go.microsoft.com/fwlink/?LinkId=822667) |Evet |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Evet |
| AMR (adaptive multi-rate) |Evet |
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

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geribildirim gönderme
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
[Azure Media Services ile Isteğe bağlı Içeriği kodlama](media-services-encode-asset.md)

[Media Encoder Standard ile kodlama](media-services-dotnet-encode-with-media-encoder-standard.md)

