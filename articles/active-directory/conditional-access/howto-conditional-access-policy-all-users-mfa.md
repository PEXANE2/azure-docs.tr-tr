---
title: Koşullu Erişim - Tüm kullanıcılar için MFA gerektirir - Azure Active Directory
description: Tüm kullanıcıların çok faktörlü kimlik doğrulaması gerçekleştirmesini gerektirecek özel bir Koşullu Erişim ilkesi oluşturun
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 60d0ad0a1c0a1b4d13ce4d386df22406a8ab8e51
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617622"
---
# <a name="conditional-access-require-mfa-for-all-users"></a>Koşullu Erişim: Tüm kullanıcılar için MFA gerektirir

Alex Weinert, Microsoft Kimlik Güvenliği Dizin, onun blog yazısı [Your Pa $ $word önemli değil](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984)bahseder gibi:

> Şifreniz önemli değil, ama MFA fark eder! Çalışmalarımıza dayanarak, MFA kullanırsanız hesabınızın %99,9'dan daha az tehlikeye girme olasılığı daha yüksektir.

Bu makaledeki kılavuz, kuruluşunuzun ortamınız için dengeli bir MFA ilkesi oluşturmasına yardımcı olur.

## <a name="user-exclusions"></a>Kullanıcı hariç tutmalar

Koşullu Erişim ilkeleri güçlü araçlardır, aşağıdaki hesapları politikanızdan hariç öneririz:

* Kiracı genelinde hesap kilitlemesini önlemek için **acil durum erişimi** veya kesme **hesabı.** Olası senaryoda tüm yöneticiler kiracınızın dışında kilitli, acil erişim yönetim hesabınız kiracı giriş yapmak için kullanılabilir erişimi kurtarmak için adımlar atın.
   * Daha fazla bilgi makalede bulunabilir, [Azure AD acil erişim hesaplarını yönet](../users-groups-roles/directory-emergency-access.md).
* **Azure** AD Connect Sync Hesabı gibi hizmet hesapları ve **hizmet ilkeleri.** Hizmet hesapları, belirli bir kullanıcıya bağlı olmayan etkileşimli olmayan hesaplardır. Normalde uygulamalara programlı erişim sağlayan arka uç hizmetleri tarafından kullanılır, ancak aynı zamanda yönetim amaçlı sistemlerde oturum açmaiçin kullanılır. MFA programlı olarak tamamlanamadığından, bu gibi hizmet hesapları hariç tutulmalıdır. Hizmet ilkeleri tarafından yapılan aramalar Koşullu Erişim tarafından engellenmez.
   * Kuruluşunuzun bu hesapları komut dosyası veya kodda kullanılıyorsa, [bunları yönetilen kimliklerle](../managed-identities-azure-resources/overview.md)değiştirmeyi düşünün. Geçici bir geçici çözüm olarak, bu belirli hesapları temel ilkeden hariç tutabilirsiniz.

## <a name="application-exclusions"></a>Uygulama hariç tutma

Kuruluşların birçok bulut uygulaması kullanılabilir. Bu uygulamaların tümü eşit güvenlik gerektiremez. Örneğin, bordro ve katılım uygulamaları MFA gerektirebilir, ancak kafeterya muhtemelen gerektirmez. Yöneticiler belirli uygulamaları ilkelerinden hariç tutmayı seçebilir.

## <a name="create-a-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

Aşağıdaki adımlar, tüm kullanıcıların çok faktörlü kimlik doğrulaması gerçekleştirmesini gerektiren bir Koşullu Erişim ilkesi oluşturulmasına yardımcı olur.

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Include**altında, **Tüm kullanıcıları** seçin
   1. **Dışla'nın**altında, **Kullanıcıları ve grupları** seçin ve kuruluşunuzun acil durum erişimini veya kesme cam hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > altında**Ekle**, Tüm **bulut uygulamalarını**seçin.
   1. **Dışla,** çok faktörlü kimlik doğrulaması gerektirmeyen uygulamaları seçin.
1. **Koşullar** > Altında**İstemci uygulamaları (Önizleme)**, **Evet** **yapılaşını** ayarlayın ve **Bitti'yi**seçin.
1. **Access denetimleri** > altında**Hibe, Erişim** **ver'i**seçin, **çok faktörlü kimlik doğrulaması gerektir**in ve **Seç'i**seçin.
1. Ayarlarınızı onaylayın ve Etkinleştir **ilkesini** **A'ya**ayarlayın.
1. İlkinizi etkinleştirmek için **Oluştur'u** seçin.

### <a name="named-locations"></a>Adlandırılmış konumlar

Kuruluşlar, **Adlandırılmış konumlar** olarak bilinen bilinen bilinen ağ konumlarını Koşullu Erişim ilkelerine dahil etmeyi seçebilir. Bu adlandırılmış konumlar, ana ofis konumu gibi güvenilir IPv4 ağlarını içerebilir. Adlandırılmış konumları yapılandırma hakkında daha fazla bilgi için, makaleye bakın [Azure Etkin Dizin Koşullu Erişim'deki konum koşulu nedir?](location-condition.md)

Yukarıdaki örnek ilkede, bir kuruluş kurumsal ağından bir bulut uygulamasına erişiyorsa çok faktörlü kimlik doğrulaması gerektirmemeyi seçebilir. Bu durumda, ilke için aşağıdaki yapılandırma ekleyebilirsiniz:

1. **Atamalar**altında, **Koşullar** > **Konumları'nı**seçin.
   1. **Evet'i**yapılandır.
   1. **Herhangi bir konum**ekle.
   1. **Tüm güvenilen konumları**hariç tut.
   1. **Done** (Bitti) öğesini seçin.
1. **Done** (Bitti) öğesini seçin.
1. İlke değişikliklerinizi **kaydedin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)
