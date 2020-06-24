---
title: Windows sanal masaüstü için Azure Multi-Factor Authentication ayarlama-Azure
description: Windows sanal masaüstü 'nde daha yüksek güvenlik için Azure Multi-Factor Authentication ayarlama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 468597c82ae6c99aadeb33fafae78e3b3ed343cd
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213678"
---
# <a name="enable-azure-multi-factor-authentication-for-windows-virtual-desktop"></a>Windows sanal masaüstü için Azure Multi-Factor Authentication 'yi etkinleştirme

Windows sanal masaüstü için Windows istemcisi, Windows sanal masaüstünü yerel makineli tümleştirmeyle ilgili mükemmel bir seçenektir. Ancak, Windows sanal masaüstü hesabınızı Windows Istemcisi olarak yapılandırdığınızda, kendinizi ve kullanıcılarınızın güvenliğini sağlamak için uygulamanız gereken bazı ölçüler vardır.

İlk kez oturum açtığınızda, istemci kullanıcı adınızı, parolanızı ve Azure MFA 'yı ister. Bundan sonra, bir sonraki oturum açışınızda istemci, Azure Active Directory (AD) Kurumsal uygulamanızdan belirtecinizi hatırlayacaktır. **Beni anımsa**' yı seçtiğinizde kullanıcılarınız, kimlik bilgilerini yeniden girmeye gerek kalmadan istemciyi yeniden başlattıktan sonra oturum açabilirler.

Kimlik bilgilerini hatırlarken, kurumsal senaryolarda veya kişisel cihazlarda dağıtımları daha az güvenli hale da olabilir. Kullanıcılarınızı korumak için, istemcinin Azure Multi-Factor Authentication (MFA) kimlik bilgilerini sormayı sürdürdüğünden emin olmanız gerekir. Bu makalede, Windows sanal masaüstü için koşullu erişim ilkesinin bu ayarı etkinleştirmek üzere nasıl yapılandırılacağı gösterilir.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için yapmanız gerekenler şunlardır:

- Kullanıcılara Azure Active Directory Premium P1 veya P2 içeren bir lisans atayın.
- Kullanıcılarınız Grup üyeleri olarak atanmış bir Azure Active Directory grubu.
- Tüm kullanıcılarınız için Azure MFA 'yı etkinleştirin. Bunun nasıl yapılacağı hakkında daha fazla bilgi için, bkz. [bir kullanıcı için iki adımlı doğrulama gerektirme](../active-directory/authentication/howto-mfa-userstates.md#view-the-status-for-a-user).

> [!NOTE]
> Aşağıdaki ayar [Windows Sanal Masaüstü Web istemcisi](https://rdweb.wvd.microsoft.com/webclient/index.html)için de geçerlidir.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Bu bölümde, Windows sanal masaüstüne bağlanırken çok faktörlü kimlik doğrulaması gerektiren bir koşullu erişim ilkesinin nasıl oluşturulacağı gösterilir.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
2. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**'ne gidin.
3. **Yeni ilke**' yi seçin.
4. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
5. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin.
   - **Ekle**' nin altında, **Kullanıcılar ve gruplar**  >  **Kullanıcılar ve gruplar** ' ı seçin > Önkoşullar aşamasında oluşturulan grubu seçin.
   - **Done** (Bitti) öğesini seçin.
6. **Bulut uygulamaları veya eylemler**altında  >  **Include**, **Uygulama Seç**' i seçin.
   - **Windows sanal masaüstü** (uygulama kimliği 9cdead84-a844-4324-93f2-b2e6bb768d07) öğesini seçin, sonra ve sonra **Tamam**' ı **seçin**.

     ![Bulut uygulamaları veya eylemler sayfasının ekran görüntüsü. Windows sanal masaüstü ve Windows sanal masaüstü Istemci uygulamaları kırmızı renkle vurgulanır.](media/cloud-apps-enterprise.png)

     >[!NOTE]
     >Seçmek istediğiniz uygulamanın uygulama KIMLIĞINI bulmak için **Kurumsal uygulamalar** ' a gidin ve uygulama türü açılan menüsünde **Microsoft uygulamaları** ' nı seçin.

7. **Erişim denetimleri**  >  **izni**altında **erişim ver**' i seçin, **Multi-Factor Authentication gerektir**' i seçin ve ardından öğesini **seçin**.
8. **Erişim denetimleri**  >  **oturumu**' nun altında, **oturum açma sıklığı**' nı seçin, değeri **1** ve birimi **saat**olarak ayarlayın ve ardından **Seç**' i seçin.
9. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
10. İlkenizi etkinleştirmek için **Oluştur** ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim ilkeleri hakkında daha fazla bilgi edinin](../active-directory/conditional-access/concept-conditional-access-policies.md)

- [Kullanıcı oturum açma sıklığı hakkında daha fazla bilgi edinin](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md#user-sign-in-frequency)
