---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 75bcb9d27ee6f66a1d9c15093d9f933a3ad25881
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188868"
---
1. Visual Studio Solution Explorer'da Windows Mağazası uygulama projesine sağ tıklayın. Ardından **Store** > **Mağaza ile Mağaza Ilişkilendir Uygulamasını**seçin.

    ![Uygulamayı Windows Mağazası ile ilişkilendirme](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
2. Sihirbazda **İleri'yi**seçin. Ardından Microsoft hesabınızla oturum açın. **Rezerv'de yeni bir uygulama adı,** uygulamanız için bir ad yazın ve ardından **Rezerv'i**seçin.
3. Uygulama kaydı başarıyla oluşturulduktan sonra yeni uygulama adını seçin. **İleri'yi**seçin ve ardından **Ilişkilendir'i**seçin. Bu işlem, gerekli Windows Mağazası kayıt bilgilerini uygulama bildirimine ekler.
4. Windows Mağazası uygulaması için daha önce oluşturduğunuz kaydı kullanarak Windows Phone Mağazası uygulaması projesi için 1 ve 3 adımlarını yineleyin.  
5. [Windows Dev Center'a](https://dev.windows.com/en-us/overview)gidin ve ardından Microsoft hesabınızla oturum açın. **Uygulamalarım'da**yeni uygulama kaydını seçin. Ardından **Hizmetleri** > **Push bildirimlerini**genişletin.
6. Anında **Ilet bildirimleri** sayfasında, **Windows Anında İtme Bildirim Hizmetleri (WNS) ve Microsoft Azure Mobil Uygulamaları** **altında, Canlı Hizmetler sitesini**seçin.  **Paket SID** değerlerini ve **Application Secret'taki** *geçerli* değeri not edin. 

    ![Geliştirici merkezinde uygulama ayarı](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

   > [!IMPORTANT]
   > Uygulama gizli anahtarı ve paket SID'si önemli güvenlik kimlik bilgileridir. Bu değerleri kimseyle paylaşmayın veya uygulamanızla dağıtmayın.
   >
   >
