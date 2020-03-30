---
title: Koşullu Erişim ilkesindeki kullanıcılar ve gruplar - Azure Etkin Dizini
description: Azure AD Koşullu Erişim ilkesinde kullanıcılar ve gruplar kimler?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36898e75680771a9cb084fa142bb635ddbf51c70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77192135"
---
# <a name="conditional-access-users-and-groups"></a>Koşullu Erişim: Kullanıcılar ve gruplar

Koşullu Erişim ilkesi, karar sürecindeki sinyallerden biri olarak bir kullanıcı ataması içermelidir. Kullanıcılar Koşullu Erişim ilkelerine dahil edilebilir veya hariç tutulabilir. 

![Koşullu Erişim tarafından alınan kararlarda sinyal olarak kullanıcı](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups.png)

## <a name="include-users"></a>Kullanıcıları dahil et

Bu kullanıcı listesi genellikle bir kuruluşun Koşullu Erişim ilkesinde hedef aldığı tüm kullanıcıları içerir. 

Koşullu Erişim ilkesi oluşturulurken aşağıdaki seçenekleri içerecek şekilde kullanılabilir.

- None
   - Kullanıcı seçilmedi
- Tüm kullanıcılar
   - B2B konukları da dahil olmak üzere dizinde bulunan tüm kullanıcılar.
- Kullanıcıları ve grupları seçme
   - Tüm konuk ve harici kullanıcılar (önizleme)
      - Bu seçim, b2B `user type` `guest`konukları ve dış kullanıcıları içerir. Bu seçim, Bulut Çözüm Sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm harici kullanıcı için de geçerlidir. 
   - Dizin rolleri (önizleme)
      - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure REKLAM dizini rollerini seçmelerine olanak tanır. Örneğin, kuruluşlar genel yönetici rolü atanan kullanıcılar üzerinde daha kısıtlayıcı bir ilke oluşturabilir.
   - Kullanıcılar ve gruplar
      - Belirli kullanıcı kümelerinin hedeflenemesine izin verir. Örneğin, bir İk uygulaması bulut uygulaması olarak seçildiğinde kuruluşlar İk departmanının tüm üyelerini içeren bir grup seçebilir. Grup, dinamik veya atanmış güvenlik ve dağıtım grupları da dahil olmak üzere Azure AD'deki herhangi bir grup türü olabilir.

## <a name="exclude-users"></a>Kullanıcıları hariç tutma

Dışlamalar genellikle acil erişim veya kesme cam hesapları için kullanılır. Acil erişim hesapları ve bunların neden önemli olduğu hakkında daha fazla bilgiyi aşağıdaki makalelerde bulabilirsiniz: 

* [Azure AD'de acil erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile esnek bir erişim denetimi yönetimi stratejisi oluşturun](../authentication/concept-resilient-controls.md)

Koşullu Erişim ilkesi oluşturulurken aşağıdaki seçenekleri hariç tutmak için kullanılabilir.

- Tüm konuk ve harici kullanıcılar (önizleme)
   - Bu seçim, b2B `user type` `guest`konukları ve dış kullanıcıları içerir. Bu seçim, Bulut Çözüm Sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm harici kullanıcı için de geçerlidir. 
- Dizin rolleri (önizleme)
   - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure REKLAM dizini rollerini seçmelerine olanak tanır. Örneğin, kuruluşlar genel yönetici rolü atanan kullanıcılar üzerinde daha kısıtlayıcı bir ilke oluşturabilir.
- Kullanıcılar ve gruplar
   - Belirli kullanıcı kümelerinin hedeflenemesine izin verir. Örneğin, bir İk uygulaması bulut uygulaması olarak seçildiğinde kuruluşlar İk departmanının tüm üyelerini içeren bir grup seçebilir. Grup, dinamik veya atanmış güvenlik ve dağıtım grupları da dahil olmak üzere Azure AD'deki herhangi bir grup türü olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim: Bulut uygulamaları veya eylemleri](concept-conditional-access-cloud-apps.md)

- [Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)
