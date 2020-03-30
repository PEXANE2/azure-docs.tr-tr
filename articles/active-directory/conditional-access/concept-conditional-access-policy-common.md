---
title: Ortak Koşullu Erişim ilkeleri - Azure Etkin Dizini
description: Kuruluşlar için yaygın olarak kullanılan Koşullu Erişim ilkeleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20b699f0672b49dd2f947e0cf00d0ffcef7961e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295300"
---
# <a name="common-conditional-access-policies"></a>Sık kullanılan Koşullu Erişim İlkeleri

[Güvenlik varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md) bazıları için harikadır, ancak birçok kuruluş sunduklarından daha fazla esnekliğe ihtiyaç duyar. Örneğin, birçok kişi, çok faktörlü kimlik doğrulaması gerektiren Koşullu Erişim ilkelerinden acil erişim veya kesme cam yönetim hesapları gibi belirli hesapları hariç tutma yeteneğine ihtiyaç duyar. Bu kuruluşlar için, bu makalede başvurulan ortak ilkeler kullanılabilir.

![Azure portalında Koşullu Erişim ilkeleri](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Acil erişim hesapları

Acil erişim hesapları ve bunların neden önemli olduğu hakkında daha fazla bilgiyi aşağıdaki makalelerde bulabilirsiniz: 

* [Azure AD'de acil erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile esnek bir erişim denetimi yönetimi stratejisi oluşturun](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Kuruluşlar tarafından dağıtılan tipik ilkeler

* [Eski kimlik doğrulamasını engelleme](howto-conditional-access-policy-block-legacy.md)\*
* [Yöneticiler için MFA gerektirir](howto-conditional-access-policy-admin-mfa.md)\*
* [Azure yönetimi için MFA gerektirir](howto-conditional-access-policy-azure-management.md)\*
* [Tüm kullanıcılar için MFA gerektirir](howto-conditional-access-policy-all-users-mfa.md)\*

\*Bu dört ilke birlikte yapılandırıldığında, [güvenlik varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md)tarafından etkinleştirilen işlevselliği taklit eder.

## <a name="additional-policies"></a>Ek ilkeler

* [Risk Tabanlı Koşullu Erişim (Azure AD Premium P2 gerektirir)](howto-conditional-access-policy-risk.md)
* [MFA kaydı için güvenilen konum gerektirme](howto-conditional-access-policy-registration.md)
* [Erişimi konuma göre engelleme](howto-conditional-access-policy-location.md)
* [Uyumlu cihaz gerektirme](howto-conditional-access-policy-compliant-device.md)
* [Belirli uygulamalar dışında erişimi engelleme](howto-conditional-access-policy-block-access.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim Ne Olursa Aracı'nı kullanarak oturum açma davranışını simüle edin.](troubleshoot-conditional-access-what-if.md)

- [Yeni ilke kararlarının etkisini belirlemek için Koşullu Erişim için yalnızca rapor modunu kullanın.](concept-conditional-access-report-only.md)
