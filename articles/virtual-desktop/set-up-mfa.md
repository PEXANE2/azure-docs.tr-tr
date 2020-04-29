---
title: Windows sanal masaüstü için Azure Multi-Factor Authentication 'ı ayarlama-Azure
description: Windows sanal masaüstü 'nde daha yüksek güvenlik için Azure Multi-Factor Authentication 'ı ayarlama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b470f9278bdca94d1fe98c64b11b070fb36cb075
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80998467"
---
# <a name="set-up-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication’ı ayarlama

Windows sanal masaüstü için Windows istemcisi, Windows sanal masaüstünü yerel makineli tümleştirmeyle ilgili mükemmel bir seçenektir. Ancak, Windows sanal masaüstü hesabınızı Windows Istemcisi olarak yapılandırdığınızda, kendinizi ve kullanıcılarınızın güvenliğini sağlamak için uygulamanız gereken bazı ölçüler vardır.

İlk kez oturum açtığınızda, istemci kullanıcı adınızı, parolanızı ve Azure MFA 'yı ister. Bundan sonra, bir sonraki oturum açışınızda istemci, Azure Active Directory (AD) Kurumsal uygulamanızdan belirtecinizi hatırlayacaktır. **Beni anımsa**' yı seçtiğinizde kullanıcılarınız, kimlik bilgilerini yeniden girmeye gerek kalmadan istemciyi yeniden başlattıktan sonra oturum açabilirler.

Kimlik bilgilerini hatırlarken, kurumsal senaryolarda veya kişisel cihazlarda dağıtımları daha az güvenli hale da olabilir. Kullanıcılarınızı korumak için, istemcinin Azure Multi-Factor Authentication (MFA) kimlik bilgilerini sormayı sürdürdüğünden emin olmanız gerekir. Bu makalede, Windows sanal masaüstü için koşullu erişim ilkesinin bu ayarı etkinleştirmek üzere nasıl yapılandırılacağı gösterilir.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için yapmanız gerekenler şunlardır:

- Tüm kullanıcılarınıza aşağıdaki lisanslardan birini atayın:
  - Microsoft 365 E3 veya E5
  - Azure Active Directory Premium P1 veya P2
  - Enterprise Mobility + Security E3 veya E5
- Kullanıcılarınız Grup üyeleri olarak atanmış bir Azure Active Directory grubu.
- Tüm kullanıcılarınız için Azure MFA 'yı etkinleştirin. Bunun nasıl yapılacağı hakkında daha fazla bilgi için, bkz. [bir kullanıcı için iki adımlı doğrulama gerektirme](../active-directory/authentication/howto-mfa-userstates.md#view-the-status-for-a-user).

>[!NOTE]
>Aşağıdaki ayar [Windows Sanal Masaüstü Web istemcisi](https://rdweb.wvd.microsoft.com/webclient/index.html)için de geçerlidir.

## <a name="opt-in-to-the-conditional-access-policy"></a>Koşullu erişim ilkesini kabul etme

1. **Azure Active Directory**açın.

2. **Tüm uygulamalar** sekmesine gidin. "Uygulama türü" açılan menüsünde **Kurumsal uygulamalar**' ı seçin ve ardından **Windows sanal masaüstü istemcisi**' ni arayın.

    ![Tüm uygulamalar sekmesinin ekran görüntüsü. Kullanıcı arama çubuğuna "Windows sanal masaüstü istemcisi" girdi ve uygulama arama sonuçlarında gösteriliyor.](media/all-applications-search.png)

3. **Koşullu erişim**' i seçin.

    ![Kullanıcının fare imlecini koşullu erişim sekmesinin üzerine imlediği bir ekran görüntüsü.](media/conditional-access-location.png)

4. **+ Yeni ilke**' yi seçin.

   ![Koşullu erişim sayfasının ekran görüntüsü. Kullanıcı, fare imlecini yeni ilke düğmesinin üzerine getirildiğinde.](media/new-policy-button.png)

5. **Kural**için bir **ad** girin, ardından önkoşullarda oluşturduğunuz **grubun** adını **seçin** .

6. **Seç**' i seçin ve **bitti**' yi seçin.

7. Ardından, **bulut uygulamalarını veya eylemlerini**açın.

8. **Seç** panelinde **Windows sanal masaüstü** kurumsal uygulamasını seçin.

    ![Bulut uygulamaları veya eylemler sayfasının ekran görüntüsü. Kullanıcı, yanındaki onay işaretini seçerek Windows sanal masaüstü uygulamasını seçti. Seçilen uygulama kırmızı renkle vurgulanır.](media/cloud-apps-select.png)
    
    >[!NOTE]
    >Aşağıdaki görüntüde gösterildiği gibi, ekranın sol tarafında seçilen Windows sanal masaüstü Istemci uygulamasını da görmeniz gerekir. İlkenin çalışması için hem Windows sanal masaüstü hem de Windows sanal masaüstü Istemcisi kurumsal uygulamalarının olması gerekir.
    >
    > ![Bulut uygulamaları veya eylemler sayfasının ekran görüntüsü. Windows sanal masaüstü ve Windows sanal masaüstü Istemci uygulamaları kırmızı renkle vurgulanır.](media/cloud-apps-enterprise-selected.png)

9. **Seç** ' i seçin

10. Sonra, açık **ver** 

11. **Multi-Factor Authentication gerektir**' i seçin, sonra **Seçili denetimlerden birini gerektir**' i seçin.
   
    ![Izin sayfasının ekran görüntüsü. "Multi-Factor Authentication gerektir" seçilidir.](media/grant-page.png)

    >[!NOTE]
    >Kuruluşunuzda MDM 'ye kayıtlı cihazlar varsa ve MFA isteğini göstermesini istemiyorsanız, **cihazın uyumlu olarak Işaretlenmesini gerektir**' i de seçebilirsiniz.

12. **Oturum**seçin.

13. **Oturum açma sıklığını** **etkin**olarak ayarlayın, ardından değerini **1 saat**olarak değiştirin.

    ![Oturum sayfasının ekran görüntüsü. Oturum menüsü, oturum açma sıklığı açılan menüsünün "1" ve "saat" olarak değiştirildiğini gösterir.](media/sign-in-frequency.png)
   
    >[!NOTE]
    >Windows sanal masaüstü ortamınızdaki etkin oturumlar, ilkeyi değiştirirken çalışmaya devam edecektir. Bununla birlikte, bağlantısını keserseniz veya oturumu kapatırsanız, kimlik bilgilerinizi 60 dakika sonra yeniden sağlamanız gerekir. Ayarları değiştirirken, zaman aşımı süresini istediğiniz kadar genişletebilirsiniz (kuruluşunuzun güvenlik ilkesiyle aynı olduğu sürece).
    >
    >Varsayılan ayar, 90 günlük bir sıralı penceredir ve bu, istemcinin, makinenizde 90 gün veya daha uzun bir süre boyunca devre dışı kaldıktan sonra bir kaynağa erişmeyi denediğinde kullanıcılardan yeniden oturum açmasını isteytikleri anlamına gelir.

14. İlkeyi etkinleştirin.

15. İlkeyi onaylamak için **Oluştur** ' u seçin.

Bitirdiniz! İzin verilenler listenizin istendiği gibi çalıştığından emin olmak için ilkeyi test etme ücretsizdir.
