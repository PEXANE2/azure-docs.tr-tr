---
title: Media Encoder Standart biçimleri ve codec'ler - Azure
description: Bu makalede, Media Encoder Standart biçimleri ve codec genel bir bakış sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251030"
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standard Biçimleri ve Kodlayıcılar

> [!div class="op_single_selector" title1="Kullandığınız Medya Hizmetleri sürümünü seçin:"]
> * [Sürüm 2](media-services-media-encoder-standard-formats.md)
> * [Sürüm 3](../latest/media-encoder-standard-formats.md)

Bu belge, Media Encoder Standardı ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosya biçimlerinin bir listesini içerir.

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya Biçimleri
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
> Yukarıda, daha sık karşılaşılan dosya uzantılarının listesi verilmiştir. Media Encoder Standard diğer lerini destekler (örneğin: .m2ts, .mpeg2video, .qt). Bir dosyayı kodlamaya çalışırsanız ve biçimin desteklenmemesi ile ilgili bir hata iletisi alırsanız, geri bildiriminizi [burada](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/)sağlayın.
> 
> 

### <a name="audio-formats-in-input-containers"></a>Giriş kaplarında ses biçimleri
Media Encoder Standard, giriş kaplarında aşağıdaki ses biçimlerini taşımayı destekler:

* Mayalı stereo veya 5,1 örnekli ses parçalarına sahip MXF, GXF ve QuickTime dosyaları

or

* Sesin ayrı PCM parçaları olarak taşındığı, ancak kanal eşlemesinin (stero’ya veya 5.1’e) dosya meta verilerinden çıkarılabildiği MXF, GXF ve QuickTime dosyaları

## <a name="input-video-codecs"></a>Giriş video codec'leri
| Giriş video codec'leri | Destekleniyor |
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
| HEVC/H.265| Ana ve Ana 10 (&#42;) Profilleri<br/>Ana 10 Profil desteği 8bit 4:2:0 içeriği için tasarlanmıştır. |

## <a name="input-audio-codecs"></a>Ses codec'lerini girin
| Giriş Ses Codec Bileşenleri | Destekleniyor |
| --- | --- |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar) |Evet |
| MPEG Katman 2 |Evet |
| MP3 (MPEG-1 Ses Katmanı 3) |Evet |
| Windows Media Ses |Evet |
| WAV/PCM |Evet |
| [Flac](https://en.wikipedia.org/wiki/FLAC)</a> |Evet |
| [Opus](https://go.microsoft.com/fwlink/?LinkId=822667) |Evet |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Evet |
| AMR (adaptive multi-rate) |Evet |
| AES (SMPTE 331M ve 302M, AES3-2003) |Hayır |
| Dolby® E |Hayır |
| Dolby® Digital (AC3) |Hayır |
| Dolby® Digital Plus (E-AC3) |Hayır |

## <a name="output-formats-and-codecs"></a>Çıktı biçimleri ve codec'ler
Aşağıdaki tabloda dışa aktarma için desteklenen codec'ler ve dosya biçimleri listelenir.

| Dosya Biçimi | Video Codec | Ses Codec |
| --- | --- | --- |
| MP4 <br/><br/>(çok bit hızında MP4 kapları dahil) |H.264 (Yüksek, Ana ve Temel Profiller) |AAC-LC, HE-AAC v1, HE-AAC v2 |
| MPEG2-TS |H.264 (Yüksek, Ana ve Temel Profiller) |AAC-LC, HE-AAC v1, HE-AAC v2 |

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca bkz.
[Azure Medya Hizmetleri ile İsteğe Bağlı İçeriği Kodlama](media-services-encode-asset.md)

[Media Encoder Standard ile kodlama](media-services-dotnet-encode-with-media-encoder-standard.md)

