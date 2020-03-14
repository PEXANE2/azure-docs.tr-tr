---
title: Media Services-Azure tarafından önerilen canlı akış kodlayıcıları | Microsoft Docs
description: Media Services tarafından önerilen canlı akış şirket içi kodlayıcılar hakkında bilgi edinin
services: media-services
keywords: kodlama; kodlayıcılar; medya
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 02/10/2020
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 3b7a75ac1c0876d562dc49e9253fe734475a551a
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79298963"
---
# <a name="tested-on-premises-live-streaming-encoders"></a>Sınanan şirket içi canlı akış kodlayıcıları

Azure Media Services, canlı bir [olay](https://docs.microsoft.com/rest/api/media/liveevents) (kanal) canlı akış içeriğini işlemek için bir işlem hattını temsil eder. Canlı olay, canlı giriş akışlarını iki şekilde alır.

* Şirket içi bir Live Encoder, Media Services ile canlı kodlama gerçekleştirmek için etkinleştirilmemiş canlı olayına çoklu bit hızlı bir RTMP veya Kesintisiz Akış (parçalanmış MP4) akışı gönderir. Alınan akışlar, daha fazla işlem yapmadan canlı olayları geçer. Bu yönteme **doğrudan geçiş**adı verilir. Canlı kodlayıcının, istemciye Uyarlamalı bit hızında akışa izin vermek için tek bit hızlı bir akış yerine çoklu bit hızına sahip akışlar göndermesini öneririz. 

    Geçişli canlı etkinlik için Multi-bitücretler akışları kullanıyorsanız, kayıttan yürütme tarafında beklenmeyen bir davranışın olmaması için video GOP boyutu ve farklı bit hızlarındaki video parçaları eşitlenmelidir.

  > [!TIP]
  > Doğrudan geçiş yöntemi kullanmak, canlı akış yapmanın en ekonomik yoludur.
 
* Şirket içi bir Live Encoder, aşağıdaki biçimlerden birinde Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilen canlı olaya tek bit hızlı bir akış gönderir: RTMP veya Kesintisiz Akış (parçalanmış MP4). Canlı etkinlik daha sonra, gelen tek bit hızlı akışın çoklu bit hızında (Uyarlamalı) bir video akışına canlı kodlamasını gerçekleştirir.

Bu makalede, sınanan şirket içi canlı akış kodlayıcıları açıklanmaktadır. Şirket içi Live Encoder ' ı nasıl doğrulayacağınız hakkında yönergeler için bkz. Şirket [içi kodlayıcınızı doğrulama](become-on-premises-encoder-partner.md)

Media Services ile canlı kodlama hakkında ayrıntılı bilgi için bkz. [Media Services v3 Ile canlı akış](live-streaming-overview.md).

## <a name="encoder-requirements"></a>Kodlayıcı gereksinimleri

Kodlayıcılar, HTTPS veya RTMPS protokollerini kullanırken TLS 1,2 ' i desteklemelidir.

## <a name="live-encoders-that-output-rtmp"></a>RTMP çıkış yapan canlı kodlayıcılar

Media Services, aşağıdaki RTMP çıkışı sağlayan gerçek zamanlı kodlayıcılardan birinin kullanılmasını önerir. Desteklenen URL şemaları `rtmp://` veya `rtmps://`.

RTMP üzerinden akış yaparken güvenlik duvarı ve/veya ara sunucu ayarlarını kontrol ederek 1935 ve 1936 numaralı giden TCP bağlantı noktalarının açık olduğundan emin olun.<br/><br/>
RTMPS üzerinden akış yaparken güvenlik duvarı ve/veya ara sunucu ayarlarını kontrol ederek 2935 ve 2936 numaralı giden TCP bağlantı noktalarının açık olduğundan emin olun.

> [!NOTE]
> RTMPS protokolleri kullanılırken kodlayıcılar TLS 1,2 ' i desteklemelidir.

- Adobe Flash Media Live Encoder 3.2
- [Cambrıa canlı 4,3](https://www.capellasystems.net/products/cambria-live/)
- Elete canlı (sürüm 2.14.15 ve üzeri)
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Switcher Studio (iOS)
- Telestream kablolu dönüştürme (TLS 1,2 gereksinimi nedeniyle sürüm 13.0.2 veya üzeri)
- Telestream kablolu dönüştürme öğeleri (yalnızca RTMP desteklenir)
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream
- [FFmpeg](https://www.ffmpeg.org)
- [GoPro](https://gopro.com/help/articles/block/getting-started-with-live-streaming) Hero 7 ve Hero 8
- [Restream.io](https://restream.io/)

## <a name="live-encoders-that-output-fragmented-mp4"></a>Parçalanan MP4 veren canlı kodlayıcılar

Media Services, çıkış olarak çoklu bit hızına Kesintisiz Akış (parçalanmış MP4) sahip aşağıdaki canlı kodlayıcılardan birini kullanmanızı önerir. Desteklenen URL şemaları `http://` veya `https://`.

> [!NOTE]
> Kodlayıcılar, HTTPS protokollerini kullanırken TLS 1,2 ' i desteklemelidir.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Dinamik kullanım (TLS 1,2 gereksinimi nedeniyle sürüm 2.14.15 ve üzeri)
- Envivio 4Caster C4 Gen III 
- Imagine Communications Selenıo MCP3
- Media Excel Hero Live ve Hero 4K (UHD/HEVC)
- [FFmpeg](https://www.ffmpeg.org)

> [!TIP]
>  Canlı olayları birden çok dilde (örneğin, bir Ingilizce ses izi ve bir Ispanyolca ses izi) akışdıysanız, canlı akışı bir geçişli canlı olaya göndermek üzere yapılandırılmış medya Excel Live Encoder ile bunu yapabilirsiniz.

## <a name="configuring-on-premises-live-encoder-settings"></a>Şirket içi Live Encoder ayarlarını yapılandırma

Canlı olay türleriniz için geçerli olan ayarlar hakkında daha fazla bilgi için bkz. [canlı olay türleri karşılaştırması](live-event-types-comparison.md).

### <a name="playback-requirements"></a>Kayıttan yürütme gereksinimleri

İçeriği kayıttan yürütmek için hem ses hem de video akışının mevcut olması gerekir. Yalnızca video akışının kayıttan yürütülmesi desteklenmez.

### <a name="configuration-tips"></a>Yapılandırma ipuçları

- Mümkün olduğunda, bir sabit internet bağlantısı kullanın.
- Bant genişliği gereksinimlerini belirlerken, akış bit hızları ' nı çift belirleyin. Zorunlu olmasa da bu basit kural, ağ tıkanıklığı etkisini azaltmaya yardımcı olur.
- Yazılım tabanlı kodlayıcılar kullanırken, gereksiz tüm programları kapatın.
- Kodlayıcı yapılandırmasını başlattıktan sonra dönüştürme işlemi, olayda olumsuz etkilere sahiptir. Yapılandırma değişiklikleri olayın kararsız hale gelmesine neden olabilir. 
- Olaylarınızı ayarlamak için kendinize bir zaman kazandırdığınızdan emin olun. Yüksek ölçekli olaylar için, kurulum 'un etkinlikten bir saat öncesine başlamasını öneririz.

## <a name="see-also"></a>Ayrıca bkz.

[Media Services v3 ile canlı akış](live-streaming-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

[Kodlayıcınızı doğrulama](become-on-premises-encoder-partner.md)
