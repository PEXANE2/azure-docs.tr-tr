---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 852a3b00e8513f9ce68c5aae54c974505d0c9af6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188869"
---
1. Mac'inizde **Anahtarlık Erişimi'ni**başlatın. Soldaki gezinti çubuğunda, **Kategori**altında , **Sertifikalarımı**açın. Önceki bölümde indirdiğiniz SSL sertifikasını bulun ve içeriğini açıklayın. Yalnızca sertifikayı seçin (özel anahtarı seçmeyin). Sonra [dışa aktarın.](https://support.apple.com/kb/PH20122?locale=en_US)
2. Azure [portalında](https://portal.azure.com/)Tüm > **Uygulama Hizmetlerine** **Gözat'ı**seçin. Ardından Mobil Uygulamalarınızı arka uçta seçin. 
3. **Ayarlar**altında, **Uygulama Hizmeti İtme'yi**seçin. Ardından bildirim merkezi adınızı seçin. 
4. Apple **Push Bildirim Hizmetleri** > **Yükleme Sertifikası'na**gidin. .p12 dosyasını yükleyin ve doğru **Modu** seçin (önceki istemci SSL sertifikanızın üretim mi yoksa kum havuzu mu olduğuna bağlı olarak). Değişiklikleri kaydedin.

Hizmetiniz artık iOS'taki anında iletme bildirimleriyle çalışacak şekilde yapılandırılmıştır.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
