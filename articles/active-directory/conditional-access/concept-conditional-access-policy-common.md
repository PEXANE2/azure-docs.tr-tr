---
title: Genel koşullu erişim ilkeleri-Azure Active Directory
description: Kuruluşlar için yaygın olarak kullanılan koşullu erişim ilkeleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: d5cfe6b790d5eb2877e46845317779c89cf33884
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75978014"
---
# <a name="common-conditional-access-policies"></a>Sık kullanılan Koşullu Erişim İlkeleri

[Güvenlik Varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md) , bazı bazı kuruluşlar için çok daha fazla esneklik sağlar. Örneğin, birçok, çok faktörlü kimlik doğrulaması gerektiren koşullu erişim ilkelerinden acil erişim veya kesme camı yönetim hesapları gibi belirli hesapları dışlayabilir. Bu kuruluşlar için, bu makalede başvurulan ortak ilkeler kullanılabilir.

![Azure portal koşullu erişim ilkeleri](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Acil durum erişim hesapları

Acil durum erişim hesapları ve neden önemli oldukları hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir: 

* [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile dayanıklı bir erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Kuruluşlar tarafından dağıtılan tipik ilkeler

* [Yöneticiler IÇIN MFA gerektir](howto-conditional-access-policy-admin-mfa.md)\*
* [Azure Yönetim IÇIN MFA gerektir](howto-conditional-access-policy-azure-management.md)\*
* [Tüm kullanıcılar IÇIN MFA gerektir](howto-conditional-access-policy-all-users-mfa.md)\*
* [Eski kimlik doğrulamasını engelle](howto-conditional-access-policy-block-legacy.md)\*
* [Risk tabanlı koşullu erişim (Azure AD Premium P2 gerektirir)](howto-conditional-access-policy-risk.md)
* [MFA kaydı için güvenilen konum gerektir](howto-conditional-access-policy-registration.md)
* [Konuma göre erişimi engelle](howto-conditional-access-policy-location.md)
* [Uyumlu cihaz gerektir](howto-conditional-access-policy-compliant-device.md)

birlikte yapılandırıldığında bu dört ilkeyi \*, [güvenlik Varsayılanları](../fundamentals/concept-fundamentals-security-defaults.md)tarafından etkinleştirilen işlevselliği taklit edecektir.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapın.](troubleshoot-conditional-access-what-if.md)
- [Yeni ilke kararlarının etkisini öğrenmek için koşullu erişim için yalnızca rapor modunu kullanın.](concept-conditional-access-report-only.md)
