---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: a69df0cc9ea14a2c9fa172c77663afb1d6861f9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188871"
---
#### <a name="configure-the-ios-project-in-xamarin-studio"></a>Xamarin Studio'daki iOS projesini yapılandırın
1. Xamarin.Studio'da **Info.plist'i**açın ve yeni uygulama kimliğinizle daha önce oluşturduğunuz paket kimliğiyle **Paket Tanımlayıcısını** güncelleyin.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)
2. **Arka Plan Modları'na**gidin. Arka **Plan Modlarını Etkinleştir** kutusunu ve **Uzak bildirimler** kutusunu seçin.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)
3. **Proje Seçenekleri'ni**açmak için Çözüm Paneli'nde projenizi çift tıklatın.
4. **Build**altında, **iOS Paket İmzalama'yı**seçin ve bu proje için yeni ayarladığınız ilgili kimlik ve sağlama profilini seçin.

   ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

   Bu, projenin kod imzalama için yeni profili kullanmasını sağlar. Resmi Xamarin cihaz sağlama belgeleri için Bkz. [Xamarin Cihaz Sağlama].

#### <a name="configure-the-ios-project-in-visual-studio"></a>Visual Studio'da iOS projesini yapılandırın
1. Visual Studio'da projeyi sağ tıklatın ve ardından **Özellikler'i**tıklatın.
2. Özellikler sayfalarında **iOS Uygulama** sekmesini tıklatın ve **identifier'i** daha önce oluşturduğunuz kimlikle güncelleştirin.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)
3. **iOS Paket İmzalama** sekmesinde, bu proje için ayarladığınız ilgili kimlik ve sağlama profilini seçin.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Bu, projenin kod imzalama için yeni profili kullanmasını sağlar. Resmi Xamarin cihaz sağlama belgeleri için Bkz. [Xamarin Cihaz Sağlama].
4. Açmak için Info.plist'e çift tıklayın ve **ardından Arka Plan Modları**altında Uzaktan **Bildirimler'i** etkinleştirin.

[Xamarin Cihaz Temini]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/
