---
title: Genel koşullu erişim ilkeleri-Azure Active Directory
description: Kuruluşlar için yaygın olarak kullanılan koşullu erişim ilkeleri
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/03/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: e3d85850fb18b80490bba44b293ece7765124133
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74846122"
---
# <a name="common-conditional-access-policies"></a>Sık kullanılan Koşullu Erişim İlkeleri

Temel koruma ilkeleri harika, ancak pek çok kuruluşun sunduklarında daha fazla esneklik ihtiyacı vardır. Örneğin, birçok kuruluşun, çok faktörlü kimlik doğrulaması gerektiren koşullu erişim ilkelerinden acil erişim veya kesme camı yönetim hesapları gibi belirli hesapları hariç tutma yeteneğine ihtiyacı vardır. Bu kuruluşlar için, bu makalede başvurulan ortak ilkeler kullanılabilir.

![Azure portal koşullu erişim ilkeleri](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Acil durum erişim hesapları

Acil durum erişim hesapları ve neden önemli oldukları hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir: 

* [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile dayanıklı bir erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Kuruluşlar tarafından dağıtılan tipik ilkeler

* [Yöneticiler için MFA gerektir](howto-conditional-access-policy-admin-mfa.md)
* [Azure yönetimi için MFA gerektir](howto-conditional-access-policy-azure-management.md)
* [Tüm kullanıcılar için MFA gerektir](howto-conditional-access-policy-all-users-mfa.md)
* [Eski kimlik doğrulamasını engelle](howto-conditional-access-policy-block-legacy.md)
* [Risk tabanlı koşullu erişim (Azure AD Premium P2 gerektirir)](howto-conditional-access-policy-risk.md)
* [MFA kaydı için güvenilen konum gerektir](howto-conditional-access-policy-registration.md)
* [Konuma göre erişimi engelle](howto-conditional-access-policy-location.md)
* [Uyumlu cihaz gerektir](howto-conditional-access-policy-compliant-device.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapın.](troubleshoot-conditional-access-what-if.md)
- [Yeni ilke kararlarının etkisini öğrenmek için koşullu erişim için yalnızca rapor modunu kullanın.](concept-conditional-access-report-only.md)
