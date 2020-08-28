---
title: Windows sanal masaüstü için Azure Multi-Factor Authentication ayarlama-Azure
description: Windows sanal masaüstü 'nde daha yüksek güvenlik için Azure Multi-Factor Authentication ayarlama.
author: Heidilohr
ms.topic: how-to
ms.date: 08/27/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: e8e723aa26ab08c8a09e75f506802101dc07f7e8
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89017795"
---
# <a name="enable-azure-multi-factor-authentication-for-windows-virtual-desktop"></a>Windows Sanal Masaüstü için Multi-Factor Authentication'ı etkinleştirme

>[!IMPORTANT]
> Bu sayfayı Windows sanal masaüstü (klasik) belgelerinden ziyaret ediyorsanız, işiniz bittiğinde [Windows sanal masaüstü (klasik) belgelerine döntığınızdan](./virtual-desktop-fall-2019/tenant-setup-azure-active-directory.md) emin olun.

Windows sanal masaüstü için Windows istemcisi, Windows sanal masaüstünü yerel makineli tümleştirmeyle ilgili mükemmel bir seçenektir. Ancak, Windows sanal masaüstü hesabınızı Windows Istemcisi olarak yapılandırdığınızda, kendinizi ve kullanıcılarınızın güvenliğini sağlamak için uygulamanız gereken bazı ölçüler vardır.

İlk kez oturum açtığınızda, istemci kullanıcı adınızı, parolanızı ve Azure MFA 'yı ister. Bundan sonra, bir sonraki oturum açışınızda istemci, Azure Active Directory (AD) Kurumsal uygulamanızdan belirtecinizi hatırlayacaktır. **Beni anımsa**' yı seçtiğinizde kullanıcılarınız, kimlik bilgilerini yeniden girmeye gerek kalmadan istemciyi yeniden başlattıktan sonra oturum açabilirler.

Kimlik bilgilerini hatırlarken, kurumsal senaryolarda veya kişisel cihazlarda dağıtımları daha az güvenli hale da olabilir. Kullanıcılarınızı korumak için, istemcinin Azure Multi-Factor Authentication (MFA) kimlik bilgilerini sormayı sürdürdüğünden emin olmanız gerekir. Bu makalede, Windows sanal masaüstü için koşullu erişim ilkesinin bu ayarı etkinleştirmek üzere nasıl yapılandırılacağı gösterilir.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için yapmanız gerekenler şunlardır:

- Kullanıcılara Azure Active Directory Premium P1 veya P2 içeren bir lisans atayın.
- Kullanıcılarınız Grup üyeleri olarak atanmış bir Azure Active Directory grubu.
- Tüm kullanıcılarınız için Azure MFA 'yı etkinleştirin. Bunun nasıl yapılacağı hakkında daha fazla bilgi için, bkz. [bir kullanıcı için iki adımlı doğrulama gerektirme](../active-directory/authentication/howto-mfa-userstates.md#view-the-status-for-a-user).

> [!NOTE]
> Aşağıdaki ayar [Windows Sanal Masaüstü Web istemcisi](https://rdweb.wvd.microsoft.com/webclient/index.html)için de geçerlidir.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Windows sanal masaüstüne bağlanırken çok faktörlü kimlik doğrulaması gerektiren bir koşullu erişim ilkesi oluşturmak için şu adımları uygulayın:

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
2. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**'ne gidin.
3. **Yeni ilke**' yi seçin.
4. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
5. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin.
6. **Ekle**' nin altında, **Kullanıcılar ve gruplar**  >  **Kullanıcılar ve gruplar** ' ı seçin > [Önkoşullar](#prerequisites) aşamasında oluşturduğunuz grubu seçin.
7. **Bitti**'yi seçin.
8. **Bulut uygulamaları veya eylemler**altında  >  **Include**, **Uygulama Seç**' i seçin.
9. Kullanmakta olduğunuz Windows sanal masaüstü sürümüne göre aşağıdaki uygulamalardan birini seçin.
   - Windows sanal masaüstü 'Nü (klasik) kullanıyorsanız, şu uygulamayı seçin:
       - **Windows sanal masaüstü** (uygulama kimliği 5a0aa725-4958-4b0c-80a9-34562e23f3b7)
   - Windows sanal masaüstü kullanıyorsanız, bunun yerine bu uygulamayı seçin:
       -  **Windows sanal masaüstü** (uygulama kimliği 9cdead84-a844-4324-93f2-b2e6bb768d07)

   >[!IMPORTANT]
   > Windows sanal masaüstü Azure Resource Manager sağlayıcısı (50e95039-B200-4007-bc97-8d5790743a63) adlı uygulamayı seçmeyin. Bu uygulama yalnızca kullanıcı akışını almak için kullanılır ve MFA 'ya sahip olmamalıdır.

10. **Koşullar**  >  **istemci uygulamaları**' na gidin ve ilkeyi uygulamak istediğiniz yeri seçin:
    
    - İlkenin Web istemcisine uygulanmasını istiyorsanız **tarayıcı** ' yı seçin.
    - İlkeyi diğer istemcilere uygulamak istiyorsanız **mobil uygulamalar ve Masaüstü istemcileri '** ni seçin.
    - İlkeyi tüm istemcilere uygulamak istiyorsanız her iki onay kutusunu da seçin.
   
    > [!div class="mx-imgBorder"]
    > ![Istemci uygulamaları sayfasının ekran görüntüsü. Kullanıcı mobil uygulamalar ve Masaüstü istemcileri onay kutusunu seçti.](media/select-apply.png)

11. Uygulamanızı seçtikten sonra **Seç**' i seçin ve **bitti**' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Bulut uygulamaları veya eylemler sayfasının ekran görüntüsü. Windows sanal masaüstü ve Windows sanal masaüstü Istemci uygulamaları kırmızı renkle vurgulanır.](media/cloud-apps-enterprise.png)

    >[!NOTE]
    >Seçmek istediğiniz uygulamanın uygulama KIMLIĞINI bulmak için **Kurumsal uygulamalar** ' a gidin ve uygulama türü açılan menüsünde **Microsoft uygulamaları** ' nı seçin.

12. **Erişim denetimleri**  >  **izni**altında **erişim ver**' i seçin, **Multi-Factor Authentication gerektir**' i seçin ve ardından öğesini **seçin**.
13. **Erişim denetimleri**  >  **oturumu**' nun altında, **oturum açma sıklığı**' nı seçin, değeri **1** ve birimi **saat**olarak ayarlayın ve ardından **Seç**' i seçin.
14. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
15. İlkenizi etkinleştirmek için **Oluştur** ' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim ilkeleri hakkında daha fazla bilgi edinin](../active-directory/conditional-access/concept-conditional-access-policies.md)

- [Kullanıcı oturum açma sıklığı hakkında daha fazla bilgi edinin](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md#user-sign-in-frequency)
