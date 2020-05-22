---
title: Koşullu erişim-Azure yönetimi için MFA gerektir-Azure Active Directory
description: Azure yönetim görevleri için çok faktörlü kimlik doğrulaması gerektirmek üzere özel bir koşullu erişim ilkesi oluşturma
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
ms.openlocfilehash: 54ec1392dab19491ba8c041d0342f0fe82e671fe
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83758206"
---
# <a name="conditional-access-require-mfa-for-azure-management"></a>Koşullu erişim: Azure yönetimi için MFA gerektir

Kuruluşlar çeşitli Azure hizmetlerini kullanır ve bunları şu şekilde Azure Resource Manager tabanlı araçlarla yönetebilir:

* Azure portal
* Azure PowerShell
* Azure CLI

Bu araçlar, abonelik genelinde yapılandırmaların, hizmet ayarlarının ve abonelik faturalandırmasını değiştirebilecek kaynaklara son derece ayrıcalıklı erişim sağlayabilir. Bu ayrıcalıklı kaynakları korumak için Microsoft, bu kaynaklara erişen herhangi bir kullanıcı için Multi-Factor Authentication kullanılmasını gerektirir.

## <a name="user-exclusions"></a>Kullanıcı dışlamaları

Koşullu erişim ilkeleri güçlü araçlardır ve ilkenizde aşağıdaki hesapların dışlanmasını öneririz:

* Kiracı genelindeki hesap kilitlenmesini engellemek için **acil durum erişimi** veya **kesme camı** hesapları. Olası olmayan senaryoda tüm yöneticiler kiracınızın dışında kilitlendiğinden, acil durum erişimi yönetim hesabınız kiracıya oturum açmak için kullanılabilir ve erişimi kurtarmak için gerekli adımları uygulayın.
   * Daha fazla bilgi için [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)makalesinde bulabilirsiniz.
* Azure AD Connect eşitleme hesabı gibi **hizmet hesapları** ve **hizmet sorumluları**. Hizmet hesapları, belirli bir kullanıcıya bağlı olmayan etkileşimli olmayan hesaplardır. Bunlar normalde, uygulamalar için programlı erişime izin veren, ancak aynı zamanda yönetim amaçlarıyla sistemlerde oturum açmak için kullanılan arka uç hizmetleri tarafından kullanılır. MFA programlı olarak tamamlanmadığı için bunlar gibi hizmet hesapları dışlanmalıdır. Hizmet sorumluları tarafından yapılan çağrılar koşullu erişim tarafından engellenmiyor.
   * Kuruluşunuzun komut dosyalarında veya kodda kullanımda olan bu hesapları varsa, bunları [yönetilen kimliklerle](../managed-identities-azure-resources/overview.md)değiştirmeyi göz önünde bulundurun. Geçici bir çözüm olarak, bu belirli hesapları temel ilkeden hariç bırakabilirsiniz.

## <a name="create-a-conditional-access-policy"></a>Koşullu erişim ilkesi oluşturma

Aşağıdaki adımlar, çok faktörlü kimlik doğrulaması gerçekleştirmek üzere [Microsoft Azure Yönetim](concept-conditional-access-cloud-apps.md#microsoft-azure-management) uygulamasına erişimi olması Için bir koşullu erişim ilkesi oluşturmanıza yardımcı olur.

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar**altında **Kullanıcılar ve gruplar** ' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında, **Kullanıcılar ve gruplar** ' ı seçin ve kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemler**altında  >  **Include**, **Uygulama Seç**' i seçin, **Microsoft Azure Yönetim**' i seçin **Select** ve sonra **Tamam**' ı seçin.
1. **Koşullar**  >  **istemci uygulamaları (Önizleme)** altında **Yapılandır** ' ı **Evet**olarak ayarlayın ve **bitti**' yi seçin.
1. **Erişim denetimleri**  >  **izni**altında **erişim ver**' i seçin, **Multi-Factor Authentication gerektir**' i seçin ve **Seç**' i seçin
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
