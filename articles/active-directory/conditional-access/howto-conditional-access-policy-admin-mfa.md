---
title: Koşullu Erişim - Yöneticiler için MFA gerektirir - Azure Etkin Dizini
description: Yöneticilerin çok faktörlü kimlik doğrulaması gerçekleştirmesini gerektirecek özel bir Koşullu Erişim ilkesi oluşturun
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
ms.openlocfilehash: 90e8a8b0926575b5a40a8c0ca7820e31827434ec
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80755218"
---
# <a name="conditional-access-require-mfa-for-administrators"></a>Koşullu Erişim: Yöneticiler için MFA gerektirir

Yönetim hakları atanan hesaplar saldırganlar tarafından hedef alınır. Bu hesaplarda çok faktörlü kimlik doğrulaması (MFA) gerektiren, bu hesapların tehlikeye atılma riskini azaltmanın kolay bir yoludur.

Microsoft, en azından aşağıdaki rolleriçin MFA'ya ihtiyaç duymanızı önerir:

* Faturalama yöneticisi
* Koşullu Erişim yöneticisi
* Exchange yöneticisi
* Genel yönetici
* Yardım Masası (Parola) yöneticisi
* Parola yöneticisi
* Güvenlik yöneticisi
* SharePoint yöneticisi
* Kullanıcı yöneticisi

Kuruluşlar uygun gördükleri rolleri eklemeyi veya hariç tutmayı seçebilir.

## <a name="user-exclusions"></a>Kullanıcı hariç tutmalar

Koşullu Erişim ilkeleri güçlü araçlardır, aşağıdaki hesapları politikanızdan hariç öneririz:

* Kiracı genelinde hesap kilitlemesini önlemek için **acil durum erişimi** veya kesme **hesabı.** Olası senaryoda tüm yöneticiler kiracınızın dışında kilitli, acil erişim yönetim hesabınız kiracı giriş yapmak için kullanılabilir erişimi kurtarmak için adımlar atın.
   * Daha fazla bilgi makalede bulunabilir, [Azure AD acil erişim hesaplarını yönet](../users-groups-roles/directory-emergency-access.md).
* **Azure** AD Connect Sync Hesabı gibi hizmet hesapları ve **hizmet ilkeleri.** Hizmet hesapları, belirli bir kullanıcıya bağlı olmayan etkileşimli olmayan hesaplardır. Normalde uygulamalara programlı erişim sağlayan arka uç hizmetleri tarafından kullanılır, ancak aynı zamanda yönetim amaçlı sistemlerde oturum açmaiçin kullanılır. MFA programlı olarak tamamlanamadığından, bu gibi hizmet hesapları hariç tutulmalıdır. Hizmet ilkeleri tarafından yapılan aramalar Koşullu Erişim tarafından engellenmez.
   * Kuruluşunuzun bu hesapları komut dosyası veya kodda kullanılıyorsa, [bunları yönetilen kimliklerle](../managed-identities-azure-resources/overview.md)değiştirmeyi düşünün. Geçici bir geçici çözüm olarak, bu belirli hesapları temel ilkeden hariç tutabilirsiniz.

## <a name="create-a-conditional-access-policy"></a>Koşullu Erişim ilkesi oluşturma

Aşağıdaki adımlar, atanan yönetim rollerinin çok faktörlü kimlik doğrulaması gerçekleştirmesini gerektirecek koşullu erişim ilkesi oluşturulmasına yardımcı olur.

1. **Azure portalında** global yönetici, güvenlik yöneticisi veya Koşullu Erişim yöneticisi olarak oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. Poliçenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmalarını öneririz.
1. **Atamalar**altında, **Kullanıcıları ve grupları** seçin
   1. **Ekle**altında, **Dizin rollerini seçin (önizleme)** ve en azından aşağıdaki rolleri seçin:
      * Kimlik Doğrulama Yöneticisi
      * Faturalama yöneticisi
      * Koşullu Erişim yöneticisi
      * Exchange yöneticisi
      * Genel yönetici
      * Yardım Masası yöneticisi
      * Parola yöneticisi
      * Güvenlik yöneticisi
      * SharePoint yöneticisi
      * Kullanıcı yöneticisi
   1. **Dışla'nın**altında, **Kullanıcıları ve grupları** seçin ve kuruluşunuzun acil durum erişimini veya kesme cam hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri** > altında**Ekle,** **Tüm bulut uygulamalarını**seçin ve **Bitti'yi**seçin.
1. **Koşullar** > Altında**İstemci uygulamaları (Önizleme)**, **Evet** **yapılaşını** ayarlayın ve **Bitti'yi**seçin.
1. **Access denetimleri** > altında**Hibe, Erişim** **ver'i**seçin, **çok faktörlü kimlik doğrulaması gerektir**in ve **Seç'i**seçin.
1. Ayarlarınızı onaylayın ve Etkinleştir **ilkesini** **A'ya**ayarlayın.
1. İlkinizi etkinleştirmek için **Oluştur'u** seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu Erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu Erişim Ne Varsa aracını kullanarak oturum açma davranışını simüle edin](troubleshoot-conditional-access-what-if.md)
