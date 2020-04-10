---
title: Windows Sanal Masaüstü için Azure çok faktörlü kimlik doğrulaması ayarlama - Azure
description: Windows Sanal Masaüstü'nde artırılmış güvenlik için Azure çok faktörlü kimlik doğrulaması nasıl ayarlanır?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: b470f9278bdca94d1fe98c64b11b070fb36cb075
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998467"
---
# <a name="set-up-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication’ı ayarlama

Windows Sanal Masaüstü için Windows istemcisi, Windows Sanal Masaüstü'nü yerel makinenizle tümleştirmek için mükemmel bir seçenektir. Ancak, Windows Sanal Masaüstü hesabınızı Windows İstemci'ye yapılandırdığınızda, kendinizi ve kullanıcılarınızı güvende tutmak için almanız gereken bazı önlemler vardır.

İlk oturum unuzu ilk oturum açğınızda, istemci kullanıcı adınızı, parolanızı ve Azure MFA'nızı sorar. Bundan sonra, bir sonraki oturum açğınızda, istemci azure Etkin Dizin (AD) Kurumsal Uygulamanızdan belirtecinizi hatırlar. **Beni Hatırla'yı**seçtiğinizde, kullanıcılarınızın kimlik bilgilerini yeniden girmelerine gerek kalmadan istemciyi yeniden başlattınktan sonra oturum açabilir.

Kimlik bilgilerini hatırlamak kullanışlı olsa da, Kurumsal senaryolarda veya kişisel aygıtlarda dağıtımları daha az güvenli hale getirebilir. Kullanıcılarınızı korumak için, istemcinin Azure Çok Faktörlü Kimlik Doğrulaması (MFA) kimlik bilgilerini sormaya devam edeceğinden emin olmanız gerekir. Bu makalede, bu ayarı etkinleştirmek için Windows Sanal Masaüstü için Koşullu Erişim ilkesini nasıl yapılandıracağınızı gösterecektir.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için gerekenler şunlardır:

- Tüm kullanıcılarınıza aşağıdaki lisanslardan birini atama:
  - Microsoft 365 E3 veya E5
  - Azure Active Directory Premium P1 veya P2
  - Kurumsal Mobilite + Güvenlik E3 veya E5
- Kullanıcılarınızın grup üyesi olarak atandığı bir Azure Etkin Dizin grubu.
- Tüm kullanıcılarınız için Azure MFA'sını etkinleştirin. Bunun nasıl yapılacıyla ilgili daha fazla bilgi için, bir kullanıcı için [iki adlı doyğunlaşnın gerekli olduÄ](../active-directory/authentication/howto-mfa-userstates.md#view-the-status-for-a-user)una bakın.

>[!NOTE]
>Aşağıdaki [ayar, Windows Sanal Masaüstü web istemcisi](https://rdweb.wvd.microsoft.com/webclient/index.html)için de geçerlidir.

## <a name="opt-in-to-the-conditional-access-policy"></a>Koşullu Erişim ilkesine katılma

1. **Azure Etkin Dizini**Açın.

2. **Tüm uygulamalar** sekmesine gidin. "Uygulama türü" açılır menüsünde, **Kurumsal Uygulamalar'ı**seçin, ardından **Windows Sanal Masaüstü İstemci'sini**arayın.

    ![Tüm uygulamalar sekmesinin ekran görüntüsü. Kullanıcı arama çubuğuna "windows sanal masaüstü istemcisi" girdi ve uygulama arama sonuçlarında ortaya çıktı.](media/all-applications-search.png)

3. **Koşullu Erişim'i**seçin.

    ![Kullanıcının fare imlecini Koşullu Erişim sekmesi üzerinde gezindiğini gösteren bir ekran görüntüsü.](media/conditional-access-location.png)

4. + **Yeni ilke**yi seçin.

   ![Koşullu Erişim sayfasının ekran görüntüsü. Kullanıcı fare imlecini yeni ilke düğmesinin üzerinde geziniyor.](media/new-policy-button.png)

5. **Kural**için bir **ad** girin, ardından önkoşullarda oluşturduğunuz **grubun** *adını **seçin.**

6. **Seç'i**seçin, ardından **Bitti'yi**seçin.

7. Ardından, **Bulut Uygulamaları veya eylemleri**açın.

8. **Select** panelinde Windows **Virtual Desktop** Enterprise uygulamasını seçin.

    ![Bulut uygulamaları veya eylemleri sayfasının ekran görüntüsü. Kullanıcı yanındaki onay işaretini seçerek Windows Sanal Masaüstü uygulamasını seçmiştir. Seçili uygulama kırmızı ile vurgulanır.](media/cloud-apps-select.png)
    
    >[!NOTE]
    >Aşağıdaki resimde gösterildiği gibi ekranın sol tarafında seçilen Windows Sanal Masaüstü İstemci uygulamasını da görmeniz gerekir. İlkenin çalışması için hem Windows Sanal Masaüstü hem de Windows Sanal Masaüstü İstemci Kurumsal uygulamalarına ihtiyacınız vardır.
    >
    > ![Bulut uygulamaları veya eylemleri sayfasının ekran görüntüsü. Windows Sanal Masaüstü ve Windows Sanal Masaüstü İstemci uygulamaları kırmızı renkle vurgulanır.](media/cloud-apps-enterprise-selected.png)

9. **Seç'i Seç**

10. Sonraki, **Açık Grant** 

11. **Çok faktörlü kimlik doğrulamayı**denetle'yi seçin, ardından **seçili denetimlerden birini iste'yi**seçin.
   
    ![Grant sayfasının ekran görüntüsü. "Çok faktörlü kimlik doğrulaması gerektir" seçilir.](media/grant-page.png)

    >[!NOTE]
    >Kuruluşunuzda MDM'ye kayıtlı aygıtlarınız varsa ve MFA istemini göstermelerini istemiyorsanız, **uyumlu olarak işaretlenecek aygıtı da**seçebilirsiniz.

12. **Oturum'u**seçin.

13. Oturum **açma sıklığını** **Etkin**olarak ayarlayın ve değerini **1 Saat**olarak değiştirin.

    ![Oturum sayfasının ekran görüntüsü. Oturum menüsünde oturum açma sıklığı açılır menülerin "1" ve "Saat" olarak değiştirildiğini gösterir.](media/sign-in-frequency.png)
   
    >[!NOTE]
    >Windows Sanal Masaüstü ortamınızdaki etkin oturumlar, siz ilkeyi değiştirin. Ancak, bağlantıyı keser veya kapatırsanız, kimlik bilgilerinizi 60 dakika sonra yeniden sağlamanız gerekir. Ayarları değiştirince, zaman ödeme süresini istediğiniz kadar uzatabilirsiniz (kuruluşunuzun güvenlik ilkesiyle uyumlu olduğu sürece).
    >
    >Varsayılan ayar 90 günlük bir yuvarlanma penceresidir, bu da istemcinin kullanıcılardan 90 gün veya daha uzun süre makinelerinde etkin olmayan bir kaynağa erişmeye çalıştıklarında yeniden oturum açmalarını isteyeceği anlamına gelir.

14. İlkeyi etkinleştirin.

15. İlkeyi onaylamak için **Oluştur'u** seçin.

Bitirdiniz! İzin veren listenizin beklendiği gibi çalıştığından emin olmak için ilkeyi test etmekten çekinmeyin.
