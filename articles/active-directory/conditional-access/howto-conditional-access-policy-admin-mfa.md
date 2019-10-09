---
title: Koşullu erişim-Yöneticiler için MFA gerektir-Azure Active Directory
description: Yöneticilerin Multi-Factor Authentication gerçekleştirmesini gerektirmek için özel bir koşullu erişim ilkesi oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35a6ac955ade3eab77f01c4e234b35744f0a7be3
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170084"
---
# <a name="conditional-access-require-mfa-for-administrators"></a>Koşullu erişim: Yöneticiler için MFA gerektir

Yönetici hakları atanmış hesaplar saldırganlar tarafından hedeflenir. Bu hesaplar üzerinde Multi-Factor Authentication (MFA) gerektirme, bu hesapların tehlikeye düşmesi riskini azaltmanın kolay bir yoludur.

Microsoft, en azından aşağıdaki rollerde MFA gerektirmenizi önerir:

* Genel yönetici
* SharePoint Yöneticisi
* Exchange Yöneticisi
* Koşullu Erişim Yöneticisi
* Güvenlik Yöneticisi
* Yardım Masası (parola) Yöneticisi
* Parola yöneticisi
* Faturalama yöneticisi
* Kullanıcı Yöneticisi

Kuruluşlar, uygun gördüğünüz şekilde rolleri dahil etmek veya hariç tutmak seçebilirler.

## <a name="user-exclusions"></a>Kullanıcı dışlamaları

Koşullu erişim ilkeleri güçlü araçlardır ve ilkenizde aşağıdaki hesapların dışlanmasını öneririz:

* Kiracı genelindeki hesap kilitlenmesini engellemek için **acil durum erişimi** veya **kesme camı** hesapları. Olası olmayan senaryoda tüm yöneticiler kiracınızın dışında kilitlendiğinden, acil durum erişimi yönetim hesabınız kiracıya oturum açmak için kullanılabilir ve erişimi kurtarmak için gerekli adımları uygulayın.
   * Daha fazla bilgi için [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)makalesinde bulabilirsiniz.
* Azure AD Connect eşitleme hesabı gibi **hizmet hesapları** ve **hizmet ilkeleri**. Hizmet hesapları, belirli bir kullanıcıya bağlı olmayan etkileşimli olmayan hesaplardır. Bunlar normalde arka uç hizmetleri tarafından kullanılır ve uygulamalara programlı erişim sağlar. MFA programlı olarak tamamlanmadığı için hizmet hesapları dışlanmalıdır.
   * Kuruluşunuzun komut dosyalarında veya kodda kullanımda olan bu hesapları varsa, bunları [yönetilen kimliklerle](../managed-identities-azure-resources/overview.md)değiştirmeyi göz önünde bulundurun. Geçici bir çözüm olarak, bu belirli hesapları temel ilkeden hariç bırakabilirsiniz.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, atanan yönetim rollerinin Multi-Factor Authentication gerçekleştirmesini gerektirmek için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory** > **koşullu erişime**gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Dahil et**altında **Dizin rolleri (Önizleme)** öğesini seçin ve en azından aşağıdaki rolleri seçin:
      * Genel yönetici
      * SharePoint Yöneticisi
      * Exchange Yöneticisi
      * Koşullu Erişim Yöneticisi
      * Güvenlik Yöneticisi
      * Yardım Masası Yöneticisi
      * Parola yöneticisi
      * Faturalama yöneticisi
      * Kullanıcı Yöneticisi
   1. **Dışla**altında, **Kullanıcılar ve gruplar** ' ı seçin ve kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin. 
   1. **Bitti**’yi seçin.
1. **Bulut uygulamaları veya eylemler**altında-1**dahil**@no__t, **tüm bulut uygulamaları**' nı seçin ve **bitti**' yi seçin.
1. **Erişim denetimleri** > **izin**' ın altında, **erişim ver**' i seçin, **Multi-Factor Authentication gerektir**' i seçin ve **Seç**
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
