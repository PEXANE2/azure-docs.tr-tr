---
title: Kullanıcı risk tabanlı koşullu erişim-Azure Active Directory
description: Kimlik koruması Kullanıcı riskini kullanarak koşullu erişim ilkeleri oluşturma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 07/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: a015f7dd58bae328a8c18e3de3202c0ca615c9cf
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86034919"
---
# <a name="conditional-access-user-risk-based-conditional-access"></a>Koşullu erişim: Kullanıcı risk tabanlı koşullu erişim

Microsoft, Microsoft 'ta bulunan araştırmacılar, hukuk zorlaması, Microsoft 'un çeşitli güvenlik ekipleri ve sızdırılan Kullanıcı adı ve parola çiftleri bulmak için diğer güvenilen kaynaklardaki çalışmalar ile birlikte Azure AD Premium P2 lisanslarına sahip kuruluşlar, [Kullanıcı riski algılamalarını Azure AD kimlik koruması](../identity-protection/concept-identity-protection-risks.md#user-risk)dahil koşullu erişim ilkeleri oluşturabilir. 

Bu ilkenin atanabileceği iki konum vardır. Kuruluşlar, güvenli parola değişikliği gerektiren bir Kullanıcı risk tabanlı koşullu erişim ilkesini etkinleştirmek için aşağıdaki seçeneklerden birini tercih etmelidir.

## <a name="enable-with-conditional-access-policy"></a>Koşullu erişim ilkesiyle etkinleştir

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında, **Kullanıcılar ve gruplar** ' ı seçin ve kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin. 
   1. **Done** (Bitti) öğesini seçin.
1. **Bulut uygulamaları veya eylemleri**  >  **dahil**, **tüm bulut uygulamaları**' nı seçin.
1. **Koşullar**  >  **Kullanıcı riski**altında **Yapılandır** ' ı **Evet**olarak ayarlayın. **İlkenin zorlanmak için gereken Kullanıcı risk düzeylerini yapılandırma** altında **yüksek**' i seçin ve **bitti**' yi seçin.
1. **Erişim denetimleri**  >  **izni**' nın altında, **erişim ver**' i seçin, **parola değişikliği gerektir**' i seçin ve **Seç**
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="enable-through-identity-protection"></a>Kimlik koruması aracılığıyla etkinleştir

1. **Azure portalında** oturum açın.
1. **Tüm hizmetler**' i seçin ve **Azure AD kimlik koruması**gidin.
1. **Kullanıcı risk ilkesini**seçin.
1. **Atamalar**altında **Kullanıcılar**' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında hariç **tutulan kullanıcıları seç**' i seçin, kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin ve **Seç**' i seçin.
   1. **Done** (Bitti) öğesini seçin.
1. **Koşullar**' ın altında, **Kullanıcı riski**' nı seçin ve **yüksek**' ı seçin
   1. **Seç**' i ve sonra **bitti**' yi seçin.
1. **Denetimler**  >  **erişimi**altında, erişime **izin ver**' i seçin ve ardından **parola değişikliği gerektir**' i seçin.
   1. **Seç**’i seçin.
1. **Ilke uygulanmasını** **Açık**olarak ayarlayın.
1. **Kaydet**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Oturum açma riski tabanlı koşullu erişim](howto-conditional-access-policy-risk.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)

[Azure Active Directory Kimlik Koruması nedir?](../identity-protection/overview.md)
