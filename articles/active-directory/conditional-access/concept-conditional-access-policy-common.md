---
title: Genel koşullu erişim ilkeleri-Azure Active Directory
description: Kuruluşlar için yaygın olarak kullanılan koşullu erişim ilkeleri
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80295300"
---
# <a name="common-conditional-access-policies"></a>Sık kullanılan Koşullu Erişim İlkeleri

[Güvenlik Varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md) , bazı bazı kuruluşlar için çok daha fazla esneklik sağlar. Örneğin, birçok, çok faktörlü kimlik doğrulaması gerektiren koşullu erişim ilkelerinden acil erişim veya kesme camı yönetim hesapları gibi belirli hesapları dışlayabilir. Bu kuruluşlar için, bu makalede başvurulan ortak ilkeler kullanılabilir.

![Azure portal koşullu erişim ilkeleri](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Acil durum erişim hesapları

Acil durum erişim hesapları ve neden önemli oldukları hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir: 

* [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile dayanıklı bir erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Kuruluşlar tarafından dağıtılan tipik ilkeler

* [Eski kimlik doğrulamasını engelle](howto-conditional-access-policy-block-legacy.md)\*
* [Yöneticiler için MFA gerektir](howto-conditional-access-policy-admin-mfa.md)\*
* [Azure yönetimi için MFA gerektir](howto-conditional-access-policy-azure-management.md)\*
* [Tüm kullanıcılar için MFA gerektir](howto-conditional-access-policy-all-users-mfa.md)\*

\*Birlikte yapılandırıldığında bu dört ilke, [güvenlik Varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md)tarafından etkinleştirilen işlevselliği taklit edecektir.

## <a name="additional-policies"></a>Ek ilkeler

* [Risk Tabanlı Koşullu Erişim (Azure AD Premium P2 gerektirir)](howto-conditional-access-policy-risk.md)
* [MFA kaydı için güvenilen konum gerektirme](howto-conditional-access-policy-registration.md)
* [Erişimi konuma göre engelleme](howto-conditional-access-policy-location.md)
* [Uyumlu cihaz gerektirme](howto-conditional-access-policy-compliant-device.md)
* [Belirli uygulamalar haricinde erişimi engelle](howto-conditional-access-policy-block-access.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapın.](troubleshoot-conditional-access-what-if.md)

- [Yeni ilke kararlarının etkisini öğrenmek için koşullu erişim için yalnızca rapor modunu kullanın.](concept-conditional-access-report-only.md)
