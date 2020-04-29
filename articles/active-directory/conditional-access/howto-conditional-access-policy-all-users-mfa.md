---
title: Koşullu erişim-tüm kullanıcılar için MFA gerektir-Azure Active Directory
description: Tüm kullanıcıların Multi-Factor Authentication gerçekleştirmesini gerektirmek için özel bir koşullu erişim ilkesi oluşturun
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81617622"
---
# <a name="conditional-access-require-mfa-for-all-users"></a>Koşullu erişim: tüm kullanıcılar için MFA gerektir

Alex Weinert olarak, Microsoft 'taki kimlik güvenliği dizini, [PA $ $Word Web günlüğü Gönderinizde bahsetmez](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984):

> Parolanız ne kadar önemlidir, ancak MFA bunu yapar! Önerilerimize bağlı olarak, MFA kullanıyorsanız, hesabınız% 99,9 ' den daha az olabilir.

Bu makaledeki kılavuz, kuruluşunuzun ortamınız için dengeli bir MFA ilkesi oluşturmasına yardımcı olur.

## <a name="user-exclusions"></a>Kullanıcı dışlamaları

Koşullu erişim ilkeleri güçlü araçlardır ve ilkenizde aşağıdaki hesapların dışlanmasını öneririz:

* Kiracı genelindeki hesap kilitlenmesini engellemek için **acil durum erişimi** veya **kesme camı** hesapları. Olası olmayan senaryoda tüm yöneticiler kiracınızın dışında kilitlendiğinden, acil durum erişimi yönetim hesabınız kiracıya oturum açmak için kullanılabilir ve erişimi kurtarmak için gerekli adımları uygulayın.
   * Daha fazla bilgi için [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)makalesinde bulabilirsiniz.
* Azure AD Connect eşitleme hesabı gibi **hizmet hesapları** ve **hizmet sorumluları**. Hizmet hesapları, belirli bir kullanıcıya bağlı olmayan etkileşimli olmayan hesaplardır. Bunlar normalde, uygulamalar için programlı erişime izin veren, ancak aynı zamanda yönetim amaçlarıyla sistemlerde oturum açmak için kullanılan arka uç hizmetleri tarafından kullanılır. MFA programlı olarak tamamlanmadığı için bunlar gibi hizmet hesapları dışlanmalıdır. Hizmet sorumluları tarafından yapılan çağrılar koşullu erişim tarafından engellenmiyor.
   * Kuruluşunuzun komut dosyalarında veya kodda kullanımda olan bu hesapları varsa, bunları [yönetilen kimliklerle](../managed-identities-azure-resources/overview.md)değiştirmeyi göz önünde bulundurun. Geçici bir çözüm olarak, bu belirli hesapları temel ilkeden hariç bırakabilirsiniz.

## <a name="application-exclusions"></a>Uygulama dışlamaları

Kuruluşların kullanımda olan çok sayıda bulut uygulaması olabilir. Bu uygulamaların hepsi eşit güvenlik gerektirmeyebilir. Örneğin, bordro ve katılımcı uygulamalar MFA gerektirebilir, ancak kafeterya olmayabilir. Yöneticiler, belirli uygulamaları ilkesinden dışlamalarını seçebilirler.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, tüm kullanıcıların Multi-Factor Authentication gerçekleştirmesini gerektirmek için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory** > **Security**güvenlik > **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Dahil et**altında **tüm kullanıcılar** ' ı seçin
   1. **Dışla**altında, **Kullanıcılar ve gruplar** ' ı seçin ve kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > **dahil**, **tüm bulut uygulamaları**' nı seçin.
   1. **Hariç tut**' un altında, Multi-Factor Authentication gerektirmeyen tüm uygulamaları seçin.
1. **Koşullar** > **istemci uygulamaları (Önizleme)** altında **Yapılandır** ' ı **Evet**olarak ayarlayın ve **bitti**' yi seçin.
1. **Erişim denetimleri** > **izni**altında **erişim ver**' i seçin, **Multi-Factor Authentication gerektir**' i seçin ve **Seç**' i seçin
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

### <a name="named-locations"></a>Adlandırılmış konumlar

Kuruluşlar, **adlandırılmış konumlar** olarak bilinen bilinen ağ konumlarını, koşullu erişim ilkelerine dahil etmek için seçim gösterebilir. Bu adlandırılmış konumlar, ana ofis konumu gibi güvenilir IPv4 ağları içerebilir. Adlandırılmış konumları yapılandırma hakkında daha fazla bilgi için, [koşullu erişim Azure Active Directory konum koşulunun ne olduğuna](location-condition.md) ilişkin makaleye bakın.

Yukarıdaki örnek ilkede, bir kuruluş, kurumsal ağından bir bulut uygulamasına erişiyorsanız çok faktörlü kimlik doğrulaması gerektirmeyebilir. Bu durumda, şu yapılandırmayı ilkeye ekleyebilirler:

1. **Atamalar**' ın altında, **koşullar** > **konumlar**' ı seçin.
   1. **Evet 'i**yapılandırın.
   1. **Herhangi bir konum**ekleyin.
   1. **Tüm güvenilen konumları**hariç tutun.
   1. **Done** (Bitti) öğesini seçin.
1. **Done** (Bitti) öğesini seçin.
1. İlke değişikliklerinizi **kaydedin** .

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
