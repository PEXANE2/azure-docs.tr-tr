---
title: Oturum açma riski tabanlı koşullu erişim-Azure Active Directory
description: Kimlik koruması oturum açma riskini kullanarak koşullu erişim ilkeleri oluşturma
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
ms.openlocfilehash: 2564e3f246d23028d6c73832a25f37110e7670c8
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88948105"
---
# <a name="conditional-access-sign-in-risk-based-conditional-access"></a>Koşullu erişim: oturum açma riski tabanlı koşullu erişim

Çoğu kullanıcı, takip edilebilen normal bir davranışa sahiptir ve davranışları normalin dışına çıktığında oturum açmalarına izin vermek riskli olabilir. Bu kullanıcıyı engellemek veya belki de gerçekten söylediklerini kanıtlamak üzere çok faktörlü kimlik doğrulaması gerçekleştirmesini istemeniz gerekebilir. 

Bir oturum açma riski, belirli bir kimlik doğrulama isteğinin kimlik sahibi tarafından yetkilendirilmemiş olma olasılığını temsil eder. Azure AD Premium P2 lisanslarına sahip kuruluşlar, [Azure AD kimlik koruması oturum açma riskini algılamalarını](../identity-protection/concept-identity-protection-risks.md#sign-in-risk)dahil etmek Için koşullu erişim ilkeleri oluşturabilir.

Bu ilkenin atanabileceği iki konum vardır. Kuruluşların güvenli parola değişikliği gerektiren bir oturum açma riski tabanlı koşullu erişim ilkesini etkinleştirmek için aşağıdaki seçeneklerden birini seçmesi gerekir.

## <a name="enable-with-conditional-access-policy"></a>Koşullu erişim ilkesiyle etkinleştir

1. **Azure Portal** genel yönetici, güvenlik yöneticisi veya koşullu erişim Yöneticisi olarak oturum açın.
1. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişimi**'ne gidin.
1. **Yeni ilke**' yi seçin.
1. İlkenize bir ad verin. Kuruluşların ilkelerinin adları için anlamlı bir standart oluşturmasını öneririz.
1. **Atamalar** altında **Kullanıcılar ve gruplar**’ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında, **Kullanıcılar ve gruplar** ' ı seçin ve kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin. 
   1. **Bitti**'yi seçin.
1. **Bulut uygulamaları veya eylemleri**  >  **dahil**, **tüm bulut uygulamaları**' nı seçin.
1. **Koşullar**  >  **Kullanıcı riski**altında **Yapılandır** ' ı **Evet**olarak ayarlayın. **Bu ilkenin uygulanacağı oturum açma risk düzeyini seçin** altında 
   1. **Yüksek** ve **Orta**' yı seçin.
   1. **Bitti**'yi seçin.
1. **Erişim denetimleri**  >  **izni**altında **erişim ver**' i seçin, **Multi-Factor Authentication gerektir**' i seçin ve **Seç**' i seçin
1. Ayarlarınızı doğrulayın ve **ilke** ayarını **Açık**olarak ayarlayın.
1. İlkenizi etkinleştirmek için oluşturmak **için Oluştur ' u seçin.**

## <a name="enable-through-identity-protection"></a>Kimlik koruması aracılığıyla etkinleştir

1. **Azure portalında** oturum açın.
1. **Tüm hizmetler**' i seçin ve **Azure AD kimlik koruması**gidin.
1. **Oturum açma risk İlkesi '** ni seçin.
1. **Atamalar**altında **Kullanıcılar**' ı seçin.
   1. **Ekle**' nin altında **tüm kullanıcılar**' ı seçin.
   1. **Dışla**altında hariç **tutulan kullanıcıları seç**' i seçin, kuruluşunuzun acil erişim veya kesme camı hesaplarını seçin ve **Seç**' i seçin.
   1. **Bitti**'yi seçin.
1. **Koşullar**' ın altında, **oturum açma riski**' nı **ve ardından orta ve üst**' i seçin.
   1. **Seç**' i ve sonra **bitti**' yi seçin.
1. **Denetimleri**  >  **erişimi**altında, erişime **izin ver**' i seçin ve ardından **çok faktörlü kimlik doğrulaması gerektir**' i seçin.
   1. **Seç**’i seçin.
1. **Ilke uygulanmasını** **Açık**olarak ayarlayın.
1. **Kaydet**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

[Kullanıcı risk tabanlı Koşullu Erişim](howto-conditional-access-policy-risk-user.md)

[Koşullu erişim yalnızca rapor modunu kullanarak etkiyi belirleme](howto-conditional-access-report-only.md)

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)

[Azure Active Directory Kimlik Koruması nedir?](../identity-protection/overview-identity-protection.md)