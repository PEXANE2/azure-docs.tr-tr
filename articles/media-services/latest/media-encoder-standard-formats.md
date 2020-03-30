---
title: Standart Encoder biçimleri ve codec'ler - Azure
description: Bu makalede, StandardEncoderPreset ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosya biçimlerinin bir listesini içerir.
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
ms.date: 02/10/2019
ms.author: juliako
ms.reviewer: anilmur
ms.openlocfilehash: f1d4d4f4006702ebe0d057e56cf24a022e73b83e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251368"
---
# <a name="standard-encoder-formats-and-codecs"></a>Standart Kodlayıcı biçimleri ve codec'ler

Bu makalede, [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset)ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosya biçimlerinin bir listesini içerir. **StandardEncoderPreset**kullanarak özel hazır ayarları nasıl oluşturabilirsiniz hakkında bilgi için [bkz.](customize-encoder-presets-how-to.md)

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

### <a name="audio-formats-in-input-containers"></a>Giriş kaplarında ses biçimleri

Standart Encoder, giriş kaplarında aşağıdaki ses biçimlerini taşımayı destekler:

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
| HEVC/H.265| Ana Profil|

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

## <a name="next-steps"></a>Sonraki adımlar

[Özel bir ön ayarla dönüştürme oluşturma](customize-encoder-presets-how-to.md)
