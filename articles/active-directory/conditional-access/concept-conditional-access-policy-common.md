---
title: Genel koşullu erişim ilkeleri-Azure Active Directory
description: Kuruluşlar için yaygın olarak kullanılan koşullu erişim ilkeleri
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
ms.openlocfilehash: abcca0d2712a462e4d2ecf9c8023d0cb0e68ad6c
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69576686"
---
# <a name="common-conditional-access-policies"></a>Ortak koşullu erişim ilkeleri

Temel koruma ilkeleri harika, ancak pek çok kuruluşun sunduklarında daha fazla esneklik ihtiyacı vardır. Örneğin, birçok kuruluşun, çok faktörlü kimlik doğrulaması gerektiren koşullu erişim ilkelerinden acil erişim veya kesme camı yönetim hesapları gibi belirli hesapları hariç tutma yeteneğine ihtiyacı vardır. Bu kuruluşlar için, bu makalede başvurulan ortak ilkeler kullanılabilir.

![Azure portal koşullu erişim ilkeleri](./media/concept-conditional-access-policy-common/conditional-access-policies-azure-ad-listing.png)

## <a name="emergency-access-accounts"></a>Acil durum erişim hesapları

Acil durum erişim hesapları ve neden önemli oldukları hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir: 

* [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile dayanıklı bir erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)

## <a name="typical-policies-deployed-by-organizations"></a>Kuruluşlar tarafından dağıtılan tipik ilkeler

* [Yöneticiler için MFA gerektir](howto-conditional-access-policy-admin-mfa.md)
* [Azure yönetimi için MFA gerektir](howto-conditional-access-policy-azure-management.md)
* [Eski kimlik doğrulamasını engelle](howto-conditional-access-policy-block-legacy.md)
* [Risk tabanlı koşullu erişim (Azure AD Premium P2 gerektirir)](howto-conditional-access-policy-risk.md)
* [MFA kaydı için güvenilen konum gerektir](howto-conditional-access-policy-registration.md)
* [Konuma göre erişimi engelle](howto-conditional-access-policy-location.md)
* [Uyumlu cihaz gerektir](howto-conditional-access-policy-compliant-device.md)

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim What If aracını kullanarak oturum açma davranışının benzetimini yapma](troubleshoot-conditional-access-what-if.md)
