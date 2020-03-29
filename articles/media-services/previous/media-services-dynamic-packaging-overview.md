---
title: Azure Medya Hizmetleri dinamik ambalaja genel bakış | Microsoft Dokümanlar
description: Bu makale, Microsoft Azure Medya Hizmetleri dinamik ambalajına genel bir bakış sağlar.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: juliako
ms.openlocfilehash: 079094965775c140c0343da98e40fd008995d45a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74901182"
---
# <a name="dynamic-packaging"></a>Dinamik paketleme

> [!div class="op_single_selector" title1="Kullandığınız Medya Hizmetleri sürümünü seçin:"]
> * [Sürüm 3](../latest/dynamic-packaging-overview.md)
> * [Sürüm 2](media-services-dynamic-packaging-overview.md)

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Microsoft Azure Medya Hizmetleri, çeşitli istemci teknolojilerine (örneğin, iOS, XBOX, Silverlight, Windows 8) birçok medya kaynağı dosya biçimi, medya akışı biçimi ve içerik koruma biçimi sunmak için kullanılabilir. Bu istemciler farklı protokolleri anlar, örneğin iOS bir HTTP Live Streaming (HLS) V4 formatı gerektirir ve Silverlight ve Xbox Düzgün Akış gerektirir. MPEG DASH, HLS veya Smooth Streaming'i anlayan istemcilere hizmet vermek istediğiniz bir dizi uyarlanabilir bitrate (çok bit hızında) MP4 (ISO Base Media 14496-12) dosyanız veya bir dizi uyarlanabilir bitrate Düzgün Akış dosyanız varsa, Medya'dan yararlanmalısınız Hizmetler dinamik ambalaj.

Dinamik paketleme tüm, adaptif bitrate MP4 dosyaları veya uyarlanabilir bitrate Smooth Streaming dosyaları bir dizi içeren bir varlık oluşturmaktır. Daha sonra, bildirimde veya parça isteğinde belirtilen biçime bağlı olarak, İsteğe Bağlı Akış sunucusu seçtiğiniz protokoldeki akışı almanızı sağlar. Bunu sonucunda, dosyaları yalnızca tek bir depolama biçiminde depolamanız ve buna göre ödeme yapmanız gerekir. Media Services hizmeti, istemciden gelen isteklere göre uygun yanıtı derler ve sunar.

Aşağıdaki diyagram, geleneksel kodlama ve statik paketleme iş akışını gösterir.

![Statik Kodlama](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Aşağıdaki diyagram dinamik ambalaj iş akışını gösterir.

![Dinamik Kodlama](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="common-scenario"></a>Sık karşılaşılan senaryo

1. Giriş dosyası yükleyin (asma dosya olarak adlandırılır). Örneğin, H.264, MP4 veya WMV (desteklenen biçimler listesi için [Bkz. Media Encoder Standardı tarafından desteklenen Biçimler.](media-services-media-encoder-standard-formats.md)
2. Asma dosyanızı H.264 MP4 uyarlamalı bithızı kümelerine kodlayın.
3. İsteğe Bağlı Bulucu oluşturarak uyarlanabilir bitrate MP4 kümesini içeren kıymeti yayımlayın.
4. İçeriğinize erişmek ve akışı sağlamak için akış URL'leri oluşturun.

## <a name="preparing-assets-for-dynamic-streaming"></a>Varlıkları dinamik akışa hazırlama

Varlığınızı dinamik akışa hazırlamak için aşağıdaki seçeneklere sahipsiniz:

- [Ana dosya yükleyin.](media-services-dotnet-upload-files.md)
- [H.264 MP4 adaptif bitrate setleri üretmek için Media Encoder Standart kodlayıcı kullanın.](media-services-dotnet-encode-with-media-encoder-standard.md)
- [İçeriğinizi akış.](media-services-deliver-content-overview.md)

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Dinamik ambalajla desteklenen ses codec'leri

Dinamik Ambalaj, [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, HE-AAC v2), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus)(Enhanced AC-3 veya E-AC3), Dolby Atmos veya [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, DTS HD, DTS HD Lossless) ile kodlanmış ses içeren MP4 dosyalarını destekler. Dolby Atmos içeriğinin akışı, Ortak Akış Biçimi (Bos) veya Ortak Medya Uygulama Biçimi (CMAF) parçalanmış MP4 ile MPEG-DASH protokolü gibi standartlar için ve CMAF ile HTTP Live Streaming (HLS) üzerinden desteklenir.

> [!NOTE]
> Dinamik [Ambalaj, Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) ses içeren dosyaları desteklemez (eski bir codec'dir).

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

