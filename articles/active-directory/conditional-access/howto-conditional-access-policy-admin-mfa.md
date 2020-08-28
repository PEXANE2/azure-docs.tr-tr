---
title: Koşullu erişim-Yöneticiler için MFA gerektir-Azure Active Directory
description: Yöneticilerin Multi-Factor Authentication gerçekleştirmesini gerektirmek için özel bir koşullu erişim ilkesi oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 08/03/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45263ca0448042aa972ee53093b51dd47bd51190
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89049359"
---
# <a name="conditional-access-require-mfa-for-administrators"></a>Koşullu erişim: Yöneticiler için MFA gerektir

Yönetici hakları atanmış hesaplar saldırganlar tarafından hedeflenir. Bu hesaplar üzerinde Multi-Factor Authentication (MFA) gerektirme, bu hesapların tehlikeye düşmesi riskini azaltmanın kolay bir yoludur.

Microsoft, en azından aşağıdaki rollerde MFA gerektirmenizi önerir:

* Faturalama yöneticisi
* Koşullu Erişim Yöneticisi
* Exchange yöneticisi
* Genel yönetici
* Yardım Masası (parola) Yöneticisi
* Parola yöneticisi
* Güvenlik yöneticisi
* SharePoint yöneticisi
* Kullanıcı yöneticisi

Kuruluşlar, uygun gördüğünüz şekilde rolleri dahil etmek veya hariç tutmak seçebilirler.

## <a name="user-exclusions"></a>Kullanıcı dışlamaları

Koşullu erişim ilkeleri güçlü araçlardır ve ilkenizde aşağıdaki hesapların dışlanmasını öneririz:

* Kiracı genelindeki hesap kilitlenmesini engellemek için **acil durum erişimi** veya **kesme camı** hesapları. Olası olmayan senaryoda tüm yöneticiler kiracınızın dışında kilitlendiğinden, acil durum erişimi yönetim hesabınız kiracıya oturum açmak için kullanılabilir ve erişimi kurtarmak için gerekli adımları uygulayın.
   * Daha fazla bilgi için [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)makalesinde bulabilirsiniz.
* Azure AD Connect eşitleme hesabı gibi **hizmet hesapları** ve **hizmet sorumluları**. Hizmet hesapları, belirli bir kullanıcıya bağlı olmayan etkileşimli olmayan hesaplardır. Bunlar normalde, uygulamalar için programlı erişime izin veren, ancak aynı zamanda yönetim amaçlarıyla sistemlerde oturum açmak için kullanılan arka uç hizmetleri tarafından kullanılır. MFA programlı olarak tamamlanmadığı için bunlar gibi hizmet hesapları dışlanmalıdır. Hizmet sorumluları tarafından yapılan çağrılar koşullu erişim tarafından engellenmiyor.
   * Kuruluşunuzun komut dosyalarında veya kodda kullanımda olan bu hesapları varsa, bunları [yönetilen kimliklerle](../managed-identities-azure-resources/overview.md)değiştirmeyi göz önünde bulundurun. Geçici bir çözüm olarak, bu belirli hesapları temel ilkeden hariç bırakabilirsiniz.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, atanan yönetim rollerinin Multi-Factor Authentication gerçekleştirmesini gerektirmek için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Dahil et**altında **Dizin rolleri (Önizleme)** öğesini seçin ve en azından aşağıdaki rolleri seçin:
      * Kimlik doğrulama Yöneticisi
      * Faturalama yöneticisi
      * Koşullu Erişim Yöneticisi
      * Exchange yöneticisi
      * Genel yönetici
      * Yardım Masası Yöneticisi
      * Parola yöneticisi
      * Güvenlik yöneticisi
      * SharePoint yöneticisi
      * Kullanıcı yöneticisi
   
      > [!WARNING]
      > Koşullu erişim ilkeleri, [özel roller](../users-groups-roles/roles-create-custom.md)gibi doğrudan bir nesne için bir [yönetim birimi](../users-groups-roles/roles-admin-units-assign-roles.md) veya dizin rollerine kapsamlı bir dizin rolü atanmış olan kullanıcıları desteklemez.

   1. **Dışla**altında, **Kullanıcılar ve gruplar** ' ı seçin ve kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin. 
   1. **Bitti**'yi seçin.
1. **Bulut uygulamaları veya eylemleri**  >  **dahil**, **tüm bulut uygulamaları**' nı seçin ve **bitti**' yi seçin.
1. **Koşullar**  >  **istemci uygulamaları (Önizleme)** altında, bu ilke tüm varsayılanları seçili bırakmak **için geçerli olur** ve **bitti**' yi seçin.
1. **Erişim denetimleri**  >  **izni**altında **erişim ver**' i seçin, **Multi-Factor Authentication gerektir**' i seçin ve **Seç**' i seçin
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-insights-reporting.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
