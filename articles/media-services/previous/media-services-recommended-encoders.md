---
title: Azure Medya Hizmetleri tarafından önerilen kodlayıcılar hakkında bilgi edinin | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri tarafından önerilen şirket kodlayıcıları listelemektedir.
services: media-services
keywords: kodlama;kodlayıcılar;ortam
author: dbgeorge
manager: johndeu
ms.author: johndeu
ms.date: 03/20/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 89b01a3fb066f181f5ec54b481b71feaa7a6ae08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77131396"
---
# <a name="recommended-on-premises-encoders"></a>Önerilen şirket içi kodlayıcılar

Azure Medya Hizmetleri ile canlı akış yaparken, kanalınızın giriş akışını nasıl almasını istediğinizi belirtebilirsiniz. Canlı kodlama kanalı olan bir şirket içi kodlayıcı kullanmayı seçerseniz, kodlayıcınız çıktı olarak yüksek kaliteli tek bit hızında bir akış itmelidir. Kanaldan geçişli bir şirket içi kodlayıcı kullanmayı seçerseniz, kodlayıcınız istenen tüm çıktı niteliklerine sahip çıktı olarak çok bit hızında bir akış itmelidir. Daha fazla bilgi için, [şirket içinde kodlayıcılar ile Canlı akış](media-services-live-streaming-with-onprem-encoders.md)bakın.

## <a name="encoder-requirements"></a>Kodlayıcı gereksinimleri

Kodlayıcılar HTTPS veya RTMPS protokollerini kullanırken TLS 1.2'yi desteklemelidir.

## <a name="live-encoders-that-output-rtmp"></a>RTMP çıktı canlı kodlayıcılar 

Azure Medya Hizmetleri, çıktı olarak RTMP'ye sahip aşağıdaki canlı kodlayıcılardan birini kullanmanızı önerir:

- Adobe Flash Media Live Encoder 3.2
- Haivision Makito X HEVC
- Haivision KB
- Telestream Wirecast (TLS 1.2 gereksinimi nedeniyle sürüm 13.0.2 veya üzeri)

  Kodlayıcılar RTMPS protokollerini kullanırken TLS 1.2'yi desteklemelidir.
- Teradek Slice 756
- TriCaster 8000
- Tricaster Mini HD-4
- OBS Studio
- VMIX
- xStream
- Switcher Studio (iOS)

## <a name="live-encoders-that-output-fragmented-mp4"></a>Parçalanmış MP4 çıkışı yapan canlı kodlayıcılar 

Azure Medya Hizmetleri, çıktı olarak çok bit hızında parçalanmış MP4 (Düzgün Akış) olan aşağıdaki canlı kodlayıcılardan birini kullanmanızı önerir:

- Media Excel Hero Live ve Hero 4K (UHD/HEVC)
- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental Live (TLS 1.2 gereksinimi nedeniyle sürüm 2.14.15 ve üzeri)

  Kodlayıcılar HTTPS protokollerini kullanırken TLS 1.2'yi desteklemelidir.
- Envivio 4Caster C4 Gen III
- İletişimSelenio MCP3 düşünün

> [!NOTE]
> Canlı kodlayıcı kanaldan geçen bir geçişe tek bit hızında akış gönderebilir, ancak istemciye uyarlamalı bit hızı akışına izin vermeyince bu yapılandırma önerilmez.

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Şirket içi kodlayıcı ortağı nasıl olunulur?

Kurumsal kodlayıcı iş ortağı olan bir Azure Medya Hizmetleri olarak Media Services, kodlayıcınızı kurumsal müşterilere önererek ürününüzü tanıtıyor. Şirket içi kodlayıcı ortağı olmak için, şirket içi kodlayıcınızın Medya Hizmetleri ile uyumluluğunu doğrulamanız gerekir. Bunu yapmak için aşağıdaki doğrulamaları tamamlayın:

Kanal doğrulamadan geçiş
1. Azure Medya Hizmetleri hesabınızı oluşturun veya ziyaret edin
2. **Geçiş** kanalı oluşturma ve başlatma
3. Kodlayıcınızı çok bit hızında bir canlı akışı itecek şekilde yapılandırın.
4. Yayımlanmış bir canlı etkinlik oluşturma
5. Canlı kodlayıcınızı yaklaşık 10 dakika çalıştırın
6. Canlı etkinliği durdurun
7. Oynatmanın tüm kalite düzeyleri için görünür bir hatası olmadığından emin olmak için arşivlenmiş varlığı izlemek için [Azure Media Player](https://aka.ms/azuremediaplayer) gibi bir oynatıcı oluşturun, Bir Akış bitiş noktası kullanın(Veya alternatif olarak 6. adımdan önceki canlı oturumda Önizleme URL'sini izleyin ve doğrulayın)
8. Varlık Kimliğini, canlı arşiv için yayınlanan akış URL'sini ve canlı kodlayıcınızdan kullanılan ayarları ve sürümü kaydedin
9. Her örnek oluşturduktan sonra kanal durumunu sıfırlama
10. Kodlayıcınız tarafından desteklenen tüm yapılandırmalar için 3'ten 9'a kadar olan adımları tekrarlayın (reklam sinyali/altyazı/farklı kodlama hızları ile ve olmadan)

Canlı kodlama kanalı doğrulaması
1. Azure Medya Hizmetleri hesabınızı oluşturun veya ziyaret edin
2. **Canlı kodlama** kanalı oluşturma ve başlatma
3. Kodlayıcınızı tek bit hızında canlı akışı itecek şekilde yapılandırın.
4. Yayımlanmış bir canlı etkinlik oluşturma
5. Canlı kodlayıcınızı yaklaşık 10 dakika çalıştırın
6. Canlı etkinliği durdurun
7. Oynatmanın tüm kalite düzeyleri için görünür bir hatası olmadığından emin olmak için arşivlenmiş varlığı izlemek için [Azure Media Player](https://aka.ms/azuremediaplayer) gibi bir oynatıcı oluşturun, Bir Akış bitiş noktası kullanın(Veya alternatif olarak 6. adımdan önceki canlı oturumda Önizleme URL'sini izleyin ve doğrulayın)
8. Varlık Kimliğini, canlı arşiv için yayınlanan akış URL'sini ve canlı kodlayıcınızdan kullanılan ayarları ve sürümü kaydedin
9. Her örnek oluşturduktan sonra kanal durumunu sıfırlama
10. Kodlayıcınız tarafından desteklenen tüm yapılandırmalar için 3'ten 9'a kadar olan adımları tekrarlayın (reklam sinyali/altyazıları/çeşitli kodlama hızları ile ve olmadan)

Uzun ömür doğrulaması
1. Azure Medya Hizmetleri hesabınızı oluşturun veya ziyaret edin
2. **Geçiş** kanalı oluşturma ve başlatma
3. Kodlayıcınızı çok bit hızında bir canlı akışı itecek şekilde yapılandırın.
4. Yayımlanmış bir canlı etkinlik oluşturma
5. Canlı kodlayıcınızı bir hafta veya daha uzun süre çalıştırın
6. Oynatmanın görünür bir hatası olmadığından emin olmak için zaman zaman canlı akışı (veya arşivlenmiş varlık) izlemek için [Azure Media Player](https://aka.ms/azuremediaplayer) gibi bir oynatıcı kullanın
7. Canlı etkinliği durdurun
8. Varlık Kimliğini, canlı arşiv için yayınlanan akış URL'sini ve canlı kodlayıcınızdan kullanılan ayarları ve sürümü kaydedin

Son olarak, kaydedilmiş ayarlarınızı ve canlı arşiv parametrelerinizi amsstreaming@microsoft.come-posta ile Medya Hizmetlerine gönderin. Medya Hizmetleri, alındıktan sonra canlı kodlayıcınızdan alınan örnekler üzerinde doğrulama testleri gerçekleştirir. Bu işlemle ilgili herhangi bir sorunuz için Medya Hizmetleri ile iletişime geçebilirsiniz.
