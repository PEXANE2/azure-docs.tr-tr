---
title: Media Services tarafından önerilen canlı akış kodlayıcıları - Azure | Microsoft Dokümanlar
description: Medya Hizmetleri tarafından önerilen şirket içi kodlayıcılar hakkında bilgi edinin
services: media-services
keywords: kodlama;kodlayıcılar;ortam
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 04/16/2020
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 0676b6b183c64dcd0fb15b87de48a4afed3a0011
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641796"
---
# <a name="tested-on-premises-live-streaming-encoders"></a>Şirket içinde test edilmiş canlı akış kodlayıcıları

Azure Medya Hizmetleri'nde, [Canlı Etkinlik](https://docs.microsoft.com/rest/api/media/liveevents) (kanal) canlı akış içeriğini işlemek için bir ardışık hattı temsil eder. Canlı Etkinlik, canlı giriş akışlarını iki şekilde alır.

* Şirket içinde canlı kodlayıcı, Medya Hizmetleri ile canlı kodlama gerçekleştirmesi etkinleştirilmeyen Live Event'e çok bit hızında RTMP veya Düzgün Akış (parçalanmış MP4) akışı gönderir. Yutulan akışlar, başka bir işleme gerek kalmadan Canlı Etkinlikler'den geçer. Bu **yönteme geçiş**denir. Canlı kodlayıcının istemciye uyarlanabilir bithızı akışına izin vermek için tek bit hızında akış yerine geçiş li canlı bir etkinliğe çok bit hızında akış lar göndermesini öneririz. 

    Geçiş canlı etkinliği için çok bitrates akışları kullanıyorsanız, oynatma tarafında beklenmeyen davranışları önlemek için video GOP boyutu ve farklı bithızlarında video parçaları eşitlenmelidir.

  > [!TIP]
  > Geçiş yöntemi kullanmak, canlı akış yapmanın en ekonomik yoludur.
 
* Şirket içi canlı kodlayıcı, Medya Hizmetleri ile canlı kodlama yı aşağıdaki biçimlerden birinde gerçekleştirebilen Live Event'e tek bit hızında bir akış gönderir: RTMP veya Düzgün Akış (parçalanmış MP4). Live Event daha sonra gelen tek bit hızındaakışı n için çok bit hızında (uyarlanabilir) video akışına canlı kodlama gerçekleştirir.

Bu makalede, şirket içinde test edilmiş canlı akış kodlayıcıları anlatılmaktadır. Şirket içi canlı kodlayıcınızı nasıl doğrulayabilirsiniz hakkında talimatlar için, [şirket içi kodlayıcınızı doğrulayın](become-on-premises-encoder-partner.md)

Medya Hizmetleri ile canlı kodlama hakkında ayrıntılı bilgi [için, Medya Hizmetleri v3 ile Canlı akış](live-streaming-overview.md)bakın.

## <a name="encoder-requirements"></a>Kodlayıcı gereksinimleri

Kodlayıcılar HTTPS veya RTMPS protokollerini kullanırken TLS 1.2'yi desteklemelidir.

## <a name="live-encoders-that-output-rtmp"></a>RTMP çıktı canlı kodlayıcılar

Media Services, aşağıdaki RTMP çıkışı sağlayan gerçek zamanlı kodlayıcılardan birinin kullanılmasını önerir. Desteklenen URL düzenleri `rtmp://` veya `rtmps://`.

RTMP üzerinden akış yaparken güvenlik duvarı ve/veya ara sunucu ayarlarını kontrol ederek 1935 ve 1936 numaralı giden TCP bağlantı noktalarının açık olduğundan emin olun.<br/><br/>
RTMPS üzerinden akış yaparken güvenlik duvarı ve/veya ara sunucu ayarlarını kontrol ederek 2935 ve 2936 numaralı giden TCP bağlantı noktalarının açık olduğundan emin olun.

> [!NOTE]
> Kodlayıcılar RTMPS protokollerini kullanırken TLS 1.2'yi desteklemelidir.

- Adobe Flash Media Live Encoder 3.2
- [Cambria Canlı 4.3](https://www.capellasystems.net/products/cambria-live/)
- Elemental Live (sürüm 2.14.15 ve üzeri)
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Switcher Studio (iOS)
- Telestream Wirecast (TLS 1.2 gereksinimi nedeniyle sürüm 13.0.2 veya üzeri)
- Telestream Wirecast S (sadece RTMP desteklenir)
- Teradek Slice 756
- VMIX
- xStream
- [Ffmpeg](https://www.ffmpeg.org)
- [GoPro](https://gopro.com/help/articles/block/getting-started-with-live-streaming) Kahraman 7 ve Kahraman 8
- [Restream.io](https://restream.io/)

## <a name="live-encoders-that-output-fragmented-mp4"></a>Parçalanmış MP4 çıkışı yapan canlı kodlayıcılar

Medya Hizmetleri, çıktı olarak çok bit hızında Düzgün Akış (parçalanmış MP4) olan aşağıdaki canlı kodlayıcılardan birini kullanmanızı önerir. Desteklenen URL düzenleri `http://` veya `https://`.

> [!NOTE]
> Kodlayıcılar HTTPS protokollerini kullanırken TLS 1.2'yi desteklemelidir.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live (TLS 1.2 gereksinimi nedeniyle sürüm 2.14.15 ve üzeri)
- Envivio 4Caster C4 Gen III 
- İletişimSelenio MCP3 düşünün
- Media Excel Hero Live ve Hero 4K (UHD/HEVC)
- [Ffmpeg](https://www.ffmpeg.org)

> [!TIP]
>  Birden çok dilde (örneğin, bir İngilizce ses parçası ve bir İspanyolca ses parçası) canlı etkinlikler yayınıyorsanız, bunu, canlı yayını geçişli Canlı Etkinliğe göndermek üzere yapılandırılan Media Excel canlı kodlayıcısı yla gerçekleştirebilirsiniz.

## <a name="configuring-on-premises-live-encoder-settings"></a>Şirket içi canlı kodlayıcı ayarlarını yapılandırma

Canlı etkinlik türünüz için hangi ayarların geçerli olduğu hakkında bilgi için [Canlı Etkinlik türleri karşılaştırması'na](live-event-types-comparison.md)bakın.

### <a name="playback-requirements"></a>Kayıttan yürütme gereksinimleri

İçeriği oynatmak için hem ses hem de video akışı olmalıdır. Yalnızca video akışının oynatılmı desteklenmez.

### <a name="configuration-tips"></a>Yapılandırma ipuçları

- Mümkünse kablolu internet bağlantısı kullanın.
- Bant genişliği gereksinimlerini belirlerken, akış bit hızlarını iki katına çıkar. Zorunlu olmasa da, bu basit kural ağ tıkanıklığının etkisini azaltmaya yardımcı olur.
- Yazılım tabanlı kodlayıcılar kullanırken, gereksiz programları kapatın.
- Bastırmaya başladıktan sonra kodlayıcı yapılandırmanızı değiştirmek olay üzerinde olumsuz etkilere sahiptir. Yapılandırma değişiklikleri olayın kararsız olmasına neden olabilir. 
- Etkinliğinizi ayarlamak için kendinize yeterli zaman verdiğinizden emin olun. Yüksek ölçekli etkinlikler için kuruluma etkinliğinizden bir saat önce başlamanızı öneririz.
- H.264 video ve AAC ses codec çıkışını kullanın.
- Video nitelikleri arasında anahtar çerçevesi veya GOP zamansal hizalama olduğundan emin olun.
- Her video kalitesi için benzersiz bir akış adı olduğundan emin olun.
- Optimum uyarlanabilir bitrate performansı için önerilen sıkı CBR kodlaması kullanın.

> [!IMPORTANT]
> Parçaların buluta yüklenmesi CPU ve IO işlemlerini içerdiğinden makinenin fiziksel durumunu (CPU / Memory / vb. izleyin. Kodlayıcıdaki herhangi bir ayarı değiştirirseniz, değişikliğin etkili olması için kanalları / canlı olayı sıfırladığından emin olun.

## <a name="see-also"></a>Ayrıca bkz.

[Medya Hizmetleri v3 ile canlı akış](live-streaming-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

[Kodlayıcınızı doğrulama](become-on-premises-encoder-partner.md)
