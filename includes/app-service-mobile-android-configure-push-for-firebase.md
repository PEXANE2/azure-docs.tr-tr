---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 51a75ee7bf87c38e3916bdbc8d85abcfb14dca8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188876"
---
1. Azure [portalında,](https://portal.azure.com/) > Tüm**Uygulama Hizmetlerine** **Gözat'ı**ve ardından Mobil Uygulamalarınızı arka uca tıklatın. **Ayarlar'ın** **altında, Uygulama Hizmeti Push'u**tıklatın ve ardından bildirim hub adınızı tıklatın.
2. **Google'a (GCM)** gidin, önceki yordamda Firebase'den aldığınız **Sunucu Anahtarı** değerini girin ve ardından **Kaydet'i**tıklatın.

    ![PORTALDAKI API anahtarını ayarlama](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

Mobil Uygulamalar arka uç artık Firebase Cloud Messaging kullanmak için yapılandırılmıştır. Bu, bildirim merkezini kullanarak uygulamanıza android cihazda çalışan anında iletme bildirimleri göndermenizi sağlar.
