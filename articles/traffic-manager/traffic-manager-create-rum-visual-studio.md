---
title: Visual Studio Mobile Center ile Gerçek Kullanıcı Ölçümleri - Azure Trafik Yöneticisi
description: Gerçek Kullanıcı Ölçümlerini Trafik Yöneticisine göndermek için Visual Studio Mobile Center'ı kullanarak geliştirilen mobil uygulamanızı ayarlayın
services: traffic-manager
documentationcenter: traffic-manager
author: rohinkoul
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: rohink
ms.custom: ''
ms.openlocfilehash: 3106334e1fb3e3000cbd09e00e413b34a1b55e54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76939179"
---
# <a name="how-to-send-real-user-measurements-to-traffic-manager-with-visual-studio-mobile-center"></a>Visual Studio Mobile Center ile Gerçek Kullanıcı Ölçümleri Trafik Yöneticisine Nasıl Gönderilir?

Visual Studio Mobile Center kullanarak geliştirilen mobil uygulamanızı, Aşağıdaki adımları izleyerek Trafik Yöneticisi'ne Gerçek Kullanıcı Ölçümleri göndermek için ayarlayabilirsiniz:

>[!NOTE]
> Şu anda, Gerçek Kullanıcı Ölçümlerini Trafik yöneticisine göndermek yalnızca Android için desteklenir.

Gerçek Kullanıcı Ölçümlerini yapılandırmak için bir anahtar edinmeniz ve RUM paketiyle uygulamanızı enstrümanlamanız gerekir.

## <a name="step-1-obtain-a-key"></a>Adım 1: Bir anahtar edinin
    
Müşteri uygulamanızdan aldığınız ve Trafik Yöneticisi'ne gönderdiğiniz ölçümler, gerçek kullanıcı ölçümleri (RUM) Anahtarı adı verilen benzersiz bir dize kullanılarak servis tarafından tanımlanır. Azure portalını, REST API'sini kullanarak veya PowerShell / CLI arabirimlerini kullanarak bir RUM anahtarı alabilirsiniz.

Aşağıdaki yordamı kullanarak Azure portalını kullanarak RUM Anahtarı'nı elde etmek için:
1. Bir tarayıcıdan Azure portalında oturum açın. Henüz bir hesabınız yoksa, bir aylık ücretsiz denemeye kaydolabilirsiniz.
2. Portalın arama çubuğunda, değiştirmek istediğiniz Traffic Manager profili adını arayın ve ardından gösterilen sonuçlardaki Traffic Manager profiline tıklayın.
3. Trafik Yöneticisi profil sayfasında, **Ayarlar**altında **Gerçek Kullanıcı Ölçümleri'ni** tıklatın.
4. Yeni bir RUM Anahtarı oluşturmak için **Oluştur Tuşu'na** tıklayın.
        
   ![Gerçek Kullanıcı Ölçümleri oluşturma anahtarı](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Şekil 1: Gerçek Kullanıcı Ölçümleri anahtar üretimi**

5. Sayfada oluşturulan RUM Anahtarı ve HTML sayfanıza katıştırılması gereken bir JavaScript kod parçacığı görüntülenir.
 
   ![Gerçek Kullanıcı Ölçümleri anahtarı için Javascript kodu](./media/traffic-manager-create-rum-visual-studio/rum-key.png)

   **Şekil 2: Gerçek Kullanıcı Ölçümleri Anahtar ve Ölçüm JavaScript**
 
6. RUM Anahtarını kopyalamak için **Kopyala** düğmesini tıklatın. 

## <a name="step-2-instrument-your-app-with-the-rum-package-of-mobile-center-sdk"></a>Adım 2: Mobil Merkezi SDK RUM paketi ile uygulama enstrüman

Visual Studio Mobile Center'da yeniyseniz, [web sitesini](https://mobile.azure.com)ziyaret edin. SDK entegrasyonu hakkında ayrıntılı talimatlar için Android [SDK ile başlarken](https://docs.microsoft.com/mobile-center/sdk/getting-started/Android)bakın.

Gerçek Kullanıcı Ölçümlerini kullanmak için aşağıdaki yordamı tamamlayın:

1.  Projeye SDK ekle

    ATM RUM SDK önizlemesi sırasında, paket deposuna açıkça başvurmanız gerekir.

    **Uygulama/build.gradle** dosyanızda aşağıdaki satırları ekleyin:

    ```groovy
    repositories {
        maven {
            url "https://dl.bintray.com/mobile-center/mobile-center-snapshot"
        }
    }
    ```
    **Uygulama/build.gradle** dosyanızda aşağıdaki satırları ekleyin:

    ```groovy
    dependencies {
     
        def mobileCenterSdkVersion = '0.12.1-16+3fe5b08'
        compile "com.microsoft.azure.mobile:mobile-center-rum:${mobileCenterSdkVersion}"
    }
    ```

2. SDK'yı başlatın

    Uygulamanızın ana etkinlik sınıfını açın ve aşağıdaki alma bildirimlerini ekleyin:

    ```java
    import com.microsoft.azure.mobile.MobileCenter;
    import com.microsoft.azure.mobile.rum.RealUserMeasurements;
    ```

    Aynı dosyada `onCreate` geri arama arayın ve aşağıdaki kodu ekleyin:

    ```java
    RealUserMeasurements.setRumKey("<Your RUM Key>");
    MobileCenter.start(getApplication(), "<Your Mobile Center AppSecret>", RealUserMeasurements.class);
    ```

## <a name="next-steps"></a>Sonraki adımlar
- [Gerçek Kullanıcı Ölçümleri](traffic-manager-rum-overview.md) hakkında daha fazla bilgi edinin
- [Trafik Yöneticisi'nin nasıl çalıştığını](traffic-manager-overview.md) öğrenin
- [Mobile Center](https://docs.microsoft.com/mobile-center/) hakkında daha fazla bilgi edinin
- Mobil Merkez'e [kaydolun](https://mobile.azure.com)
- Trafik Yöneticisi tarafından desteklenen [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) hakkında daha fazla bilgi edinin
- Trafik Yöneticisi profilini nasıl [oluşturabilirsiniz](traffic-manager-create-profile.md) öğrenin

