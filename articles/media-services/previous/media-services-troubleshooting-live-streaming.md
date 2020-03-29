---
title: Canlı akış için sorun giderme kılavuzu | Microsoft Dokümanlar
description: Bu makalede, Azure Medya Hizmetleri'nin canlı akış sorunlarının nasıl giderilene ilişkin öneriler de yer avurmaktadır.
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
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: b00df5e8176aaad86be5cf3ae4e04c736f36cf5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74885611"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Canlı akış sorun giderme kılavuzu  

Bu makalede, bazı canlı akış sorunları gidermek için nasıl öneriler verir.

## <a name="issues-related-to-on-premises-encoders"></a>Şirket içi kodlayıcılarla ilgili sorunlar
Bu bölümde, canlı kodlama için etkinleştirilen AMS kanallarına tek bir bit hızı akışı gönderecek şekilde yapılandırılan şirket içi kodlayıcılarla ilgili sorunları nasıl gidereceğimize ilişkin öneriler verilir.

### <a name="problem-would-like-to-see-logs"></a>Sorun: Günlükleri görmek ister misiniz
* **Olası sorun**: Hata ayıklama sorunlarına yardımcı olabilecek kodlayıcı günlüklerini bulamıyorum.
  
  * **Telestream Wirecast**: Genellikle C:\Users\{kullanıcı adı}\AppData\Roaming\Wirecast\ altında günlükleri bulabilirsiniz 
  * **Elemental Live**: Yönetim portalında günlüklere bağlantılar bulabilirsiniz. **İstatistikler**tıklayın, sonra **Günlükleri**. Günlük **Dosyaları** sayfasında, tüm LiveEvent öğeleriiçin günlüklerin listesini görürsünüz; geçerli oturumunuzla eşleşen oturumu seçin. 
  * **Flash Media Live Encoder**: **Günlük Dizini...** Kodlama **Günlüğü** sekmesine yönlendirerek bulabilirsiniz.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Sorun: Aşamalı bir akışı çıkarmak için bir seçenek yoktur
* **Potansiyel sorun**: Kullanılan kodlayıcı otomatik olarak deinterlace değildir. 
  
    **Sorun giderme adımları**: Kodlayıcı arabirimi içinde iç içe geçmiş seçeneği arayın. Ara yıkıntma etkinleştirildikten sonra, aşamalı çıkış ayarlarını yeniden denetleyin. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Sorun: Birkaç kodlayıcı çıkış ayarını denedim ve hala bağlanamadı.
* **Olası sorun**: Azure kodlama kanalı düzgün sıfırlanmadı. 
  
    **Sorun giderme adımları**: Kodlayıcının artık AMS'ye itmediğinden emin olun, kanalı durdurun ve sıfırlayın. Bir kez daha çalışan, yeni ayarlar ile kodlayıcı bağlamayı deneyin. Bu hala sorunu düzeltmezse, tamamen yeni bir kanal oluşturmayı deneyin, bazen kanallar birkaç başarısız denemeden sonra bozuk olabilir.  
* **Potansiyel sorun**: GOP boyutu veya anahtar kare ayarları en iyi değildir. 
  
    **Sorun giderme adımları**: Önerilen GOP boyutu veya anahtar kare aralığı iki saniyedir. Bazı kodlayıcılar bu ayarı kare sayısı olarak hesaplarken, diğerleri saniye kullanır. Örneğin: 30 fps çıkarken, GOP boyutu 2 saniyeye eşdeğer 60 kare olacaktır.  
* **Potansiyel sorun**: Kapalı bağlantı noktaları akışı engelliyor. 
  
    **Sorun giderme adımları**: RTMP üzerinden akış yaparken, 1935 ve 1936 bağlantı noktalarının açık olduğunu doğrulamak için güvenlik duvarı ve/veya proxy ayarlarını kontrol edin. 

> [!NOTE]
> Sorun giderme adımlarını takip ettikten sonra hala başarılı bir şekilde akış yapamıyorsanız, Azure portalını kullanarak bir destek bileti gönderin.
> 
> 

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

