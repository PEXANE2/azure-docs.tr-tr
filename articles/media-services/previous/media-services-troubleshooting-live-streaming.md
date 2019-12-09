---
title: Canlı akış için sorun giderme kılavuzu | Microsoft Docs
description: Bu makale, canlı akış sorunlarını Azure Media Services gidermeye yönelik öneriler sağlar.
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
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74885611"
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Canlı akış sorun giderme kılavuzu  

Bu makalede, bazı canlı akış sorunlarını gidermeye yönelik öneriler verilmektedir.

## <a name="issues-related-to-on-premises-encoders"></a>Şirket içi kodlayıcılarla ilgili sorunlar
Bu bölümde, canlı kodlama için etkinleştirilmiş AMS kanallarına tek bir bit hızlı akış gönderecek şekilde yapılandırılan şirket içi kodlayıcılarla ilgili sorunları gidermeye yönelik öneriler verilmektedir.

### <a name="problem-would-like-to-see-logs"></a>Sorun: günlükleri görmek Ister misiniz?
* **Olası sorun**: hata ayıklama sorunlarında yardımcı olabilecek kodlayıcı günlükleri bulunamıyor.
  
  * **Telestream kablolu dönüştürme**: genellikle C:\Users\{username} \appdata\roaming\kablo 
  * **Dinamik**kullanım: bulma, yönetim portalında günlüklere bağlantılar içeriyor. **İstatistikler**, ardından **Günlükler**' e tıklayın. **Günlük dosyaları** sayfasında, tüm liveevent öğeleri için günlüklerin bir listesini görürsünüz. geçerli oturumunuzla eşleşen bir tane seçin. 
  * **Flash Media Live Encoder**: **günlük dizinini** , **kodlama günlüğü** sekmesine giderek bulabilirsiniz.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Sorun: aşamalı bir akışın çıktısını almak için bir seçenek yoktur
* **Olası sorun**: kullanılan kodlayıcı otomatik olarak titreşimsiz hale gelmiyor. 
  
    **Sorun giderme adımları**: kodlayıcı arabirimi içinde bir taramasız seçenek bulun. Taramayı kaldırma etkinleştirildikten sonra, aşamalı çıkış ayarları için tekrar kontrol edin. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Sorun: birkaç kodlayıcı çıkış ayarı denendi ve yine de bağlantı kurulamıyor.
* **Olası sorun**: Azure kodlama kanalı düzgün şekilde sıfırlanamadı. 
  
    **Sorun giderme adımları**: kodlayıcının artık AMS 'ye itilmemesi, kanalı durdurup sıfırlaması durumunda olduğundan emin olun. Yeniden çalışmaya başladıktan sonra, kodlayıcınızı yeni ayarlarla bağlamayı deneyin. Bu sorunu düzeltmezse, tamamen yeni bir kanal oluşturmayı deneyin, bazen birkaç başarısız denemeden sonra kanallar bozulabilir.  
* **Olası sorun**: GOP boyutu veya anahtar çerçevesi ayarları en uygun değildir. 
  
    **Sorun giderme adımları**: önerilen GOP boyutu veya ana kare aralığı iki saniyedir. Bazı kodlayıcılar bu ayarı çerçeve sayısında hesaplar, diğerleri saniyeler kullanır. Örneğin: 30 fps 'ye kadar, GOP boyutu 60 kare olur ve bu da 2 saniyeye eşittir.  
* **Olası sorun**: kapalı bağlantı noktaları akışı engelliyor. 
  
    **Sorun giderme adımları**: RTMP aracılığıyla akış yaparken, 1935 ve 1936 giden bağlantı noktalarının açık olduğunu onaylamak için güvenlik duvarı ve/veya proxy ayarlarını kontrol edin. 

> [!NOTE]
> Sorun giderme adımlarını takip ettikten sonra, Azure portal kullanarak bir destek bileti gönderebilirsiniz.
> 
> 

## <a name="provide-feedback"></a>Geri bildirim sağlayın
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

