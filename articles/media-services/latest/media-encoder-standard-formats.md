---
title: Standart kodlayıcı biçimleri ve codec bileşenleri-Azure
description: Bu makale, Standardencoderönayar ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosyası biçimlerinin bir listesini içerir.
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
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74888079"
---
# <a name="standard-encoder-formats-and-codecs"></a>Standart kodlayıcı biçimleri ve codec bileşenleri

Bu makale, [Standardencoderönayar](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset)ile kullanabileceğiniz en yaygın içe aktarma ve dışa aktarma dosyası biçimlerinin bir listesini içerir. **Standardencoderönayar**kullanarak özel ön ayarlar oluşturma hakkında bilgi için bkz. [özel önayar ile dönüşüm oluşturma](customize-encoder-presets-how-to.md).

## <a name="input-containerfile-formats"></a>Giriş kapsayıcısı/dosya biçimleri

| Dosya biçimleri (dosya uzantıları) | Desteklenen |
| --- | --- |
| FLV (H.264 ve AAC codec bileşenleriyle) (.flv) |Yes |
| MXF    (.mxf) |Yes |
| GXF    (.gxf) |Yes |
| MPEG2-PS, MPEG2-TS, 3GP (.ts, .ps, .3gp, .3gpp, .mpg) |Yes |
| Windows Media Video (WMV)/ASF (.wmv, .asf) |Yes |
| AVI (Sıkıştırılmamış 8 bit/10 bit) (.avi) |Yes |
| MP4 (.mp4, .m4a, .m4v)/ISMV (.isma, .ismv) |Yes |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Yes |
| Matroska/WebM (.mkv) |Yes |
| WAVE/WAV (.wav) |Yes |
| QuickTime (.mov) |Yes |

### <a name="audio-formats-in-input-containers"></a>Giriş kapsayıcılarındaki ses biçimleri

Standart kodlayıcı, giriş kapsayıcılarında aşağıdaki ses biçimlerinin kullanılmasını destekler:

* Araya eklemeli stereo veya 5,1 örnekleri ile ses parçaları bulunan MXF, GXF ve QuickTime dosyaları

or

* Sesin ayrı PCM parçaları olarak taşındığı, ancak kanal eşlemesinin (stero’ya veya 5.1’e) dosya meta verilerinden çıkarılabildiği MXF, GXF ve QuickTime dosyaları

## <a name="input-video-codecs"></a>Giriş video codec bileşenleri
| Giriş video codec bileşenleri | Desteklenen |
| --- | --- |
| AVC 8 bit/10 bit, 4:2:2’ye kadar, AVCIntra dahil |8 bit 4:2:0 ve 4:2:2 |
| Avid DNxHD (MXF biçiminde) |Yes |
| DVCPro/DVCProHD (in MXF) |Yes |
| Digital video (DV) (AVI dosyalarında) |Yes |
| JPEG 2000 |Yes |
| MPEG-2 (422 Profili ve Yüksek Düzeye kadar; XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ve D10 gibi çeşitler dahil) |422 Profiline kadar |
| MPEG-1 |Yes |
| VC-1/WMV9 |Yes |
| Canopus HQ/HQX |Hayır |
| MPEG-4 Bölüm 2 |Yes |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Yes |
| YUV420 sıkıştırılmamış veya mezzanine |Yes |
| Apple ProRes 422 |Yes |
| Apple ProRes 422 LT |Yes |
| Apple ProRes 422 HQ |Yes |
| Apple ProRes Proxy |Yes |
| Apple ProRes 4444 |Yes |
| Apple ProRes 4444 XQ |Yes |
| HEVC/H. 265| Ana profil|

## <a name="input-audio-codecs"></a>Giriş sesi codec bileşenleri
| Giriş Ses Codec Bileşenleri | Desteklenen |
| --- | --- |
| AAC (AAC-LC, AAC-HE ve AAC-HEv2; 5.1’e kadar) |Yes |
| MPEG Katman 2 |Yes |
| MP3 (MPEG-1 Ses Katmanı 3) |Yes |
| Windows Media Ses |Yes |
| WAV/PCM |Yes |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Yes |
| [Opus](https://go.microsoft.com/fwlink/?LinkId=822667) |Yes |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Yes |
| AMR (adaptive multi-rate) |Yes |
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

## <a name="next-steps"></a>Sonraki adımlar

[Özel önayar olan bir dönüşüm oluşturma](customize-encoder-presets-how-to.md)
