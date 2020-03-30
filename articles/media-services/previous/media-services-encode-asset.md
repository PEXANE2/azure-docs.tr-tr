---
title: Azure isteğe bağlı medya kodlayıcıları | Microsoft Dokümanlar
description: Azure Medya Hizmetleri, bulutta ortam kodlaması için birden çok seçenek sunar. Bu makalede, Azure isteğe bağlı medya kodlayıcıları hakkında genel bir bakış yer almaktadır.
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
ms.date: 06/25/2019
ms.author: juliako
ms.openlocfilehash: d5d5b8a7328ee82e94d494795617832cb0258667
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251108"
---
# <a name="overview-of-azure-on-demand-media-encoders"></a>Azure isteğe bağlı medya kodlayıcıları genel bakış 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Azure Medya Hizmetleri, bulutta ortam kodlaması için birden çok seçenek sunar.

Medya Hizmetleri ile başlarken, codec ve dosya biçimleri arasındaki farkı anlamak önemlidir.
Codec'ler sıkıştırma/dekompresyon algoritmalarını uygulayan yazılımdır, oysa dosya biçimleri sıkıştırılmış videoyu tutan kapsayıcılardır.

Medya Hizmetleri, uyarlanabilir bitrate MP4 veya Smooth Streaming kodlanmış içeriğinizi, ortam hizmetlerine yeniden paketlemek zorunda kalmadan Medya Hizmetleri (MPEG DASH, HLS, Smooth Streaming) tarafından desteklenen akış biçimlerinde sunmanızı sağlayan dinamik ambalajlar sağlar akış biçimleri.

Media Services hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. Akış uç noktaları için faturalandırma, bitiş noktası **Çalışan** durumda olduğunda oluşur.

Medya Hizmetleri, bu makalede açıklanan talep kodlayıcıları aşağıdakileri destekler:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium İş Akışı](media-services-encode-asset.md#media-encoder-premium-workflow)

Bu makalede, isteğe bağlı medya kodlayıcıları hakkında kısa bir genel bakış sağlar ve daha ayrıntılı bilgi veren makalelere bağlantılar sağlar. Konu aynı zamanda kodlayıcıların karşılaştırmasını da sağlar.

Varsayılan olarak, her Medya Hizmetleri hesabının aynı anda bir etkin kodlama görevi olabilir. Satın aldığınız her kodlama ayrılmış birim için aynı anda çalışan birden çok kodlama görevi olmasını sağlayan kodlama birimleri ayırabilirsiniz. Bilgi için [bkz.](media-services-scale-media-processing-overview.md)

## <a name="media-encoder-standard"></a>Media Encoder Standard

### <a name="how-to-use"></a>Nasıl kullanılır
[Media Encoder Standard ile kodlama](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Biçimler
[Biçimler ve codec'ler](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Önayarları
Media Encoder [Standard, burada](https://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409)açıklanan kodlayıcı hazır ayarlarından biri kullanılarak yapılandırılır.

### <a name="input-and-output-metadata"></a>Giriş ve çıktı meta verileri
Kodlayıcılar giriş meta verileri [burada](media-services-input-metadata-schema.md)açıklanmıştır.

Kodlayıcı çıktı meta verileri [burada](media-services-output-metadata-schema.md)açıklanmıştır.

### <a name="generate-thumbnails"></a>Küçük resim oluşturma
Daha fazla bilgi için, [Media Encoder Standard'ı kullanarak küçük resimlerin nasıl oluşturacağı](media-services-advanced-encoding-with-mes.md#thumbnails)bilgisini görün.

### <a name="trim-videos-clipping"></a>Videoları kırpma (kırpma)
Daha fazla bilgi için Media [Encoder Standard'ı kullanarak videoları nasıl kırpabilirsiniz'](media-services-advanced-encoding-with-mes.md#trim_video)a bakın.

### <a name="create-overlays"></a>Bindirmeler oluşturma
Bilgi için, [Media Encoder Standard'ı kullanarak bindirmeoluşturma yı görme.](media-services-advanced-encoding-with-mes.md#overlay)

### <a name="see-also"></a>Ayrıca bkz.
[Medya Hizmetleri günlüğü](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Media Encoder Premium İş Akışı
### <a name="overview"></a>Genel Bakış
[Azure Medya Hizmetlerinde Premium Kodlama Ile Tanış](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a>Nasıl kullanılır
Media Encoder Premium İş Akışı karmaşık iş akışları kullanılarak yapılandırılır. İş Akışı [Dosyaları, İş Akışı Tasarımcısı](media-services-workflow-designer.md) aracı kullanılarak oluşturulabilir ve güncelleştirilebilir.

[Azure Medya Hizmetlerinde Premium Kodlama Nasıl Kullanılır?](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Bilinen sorunlar
Giriş videonuz kapalı altyazı içermiyorsa, çıkış Varlığı yine de boş bir TTML dosyası içerir.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>İlgili makaleler:
* [Media Encoder Standart ön ayarlarını özelleştirerek gelişmiş kodlama görevleri gerçekleştirin](media-services-custom-mes-presets-with-dotnet.md)
* [Kotalar ve Sınırlamalar](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
