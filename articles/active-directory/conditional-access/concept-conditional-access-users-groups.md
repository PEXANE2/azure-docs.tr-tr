---
title: Koşullu Erişim ilkesindeki kullanıcılar ve gruplar - Azure Etkin Dizini
description: Azure AD Koşullu Erişim ilkesinde kullanıcılar ve gruplar kimler?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 04/06/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17312e44714c8bdb20e22ad9aeb950e46eb71e3e
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80755277"
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
   - Tüm konuk ve dış kullanıcılar
      - Bu seçim, b2B `user type` `guest`konukları ve dış kullanıcıları içerir. Bu seçim, Bulut Çözüm Sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm harici kullanıcı için de geçerlidir. 
   - Dizin rolleri
      - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure REKLAM dizini rollerini seçmelerine olanak tanır. Örneğin, kuruluşlar genel yönetici rolü atanan kullanıcılar üzerinde daha kısıtlayıcı bir ilke oluşturabilir.
   - Kullanıcılar ve gruplar
      - Belirli kullanıcı kümelerinin hedeflenemesine izin verir. Örneğin, bir İk uygulaması bulut uygulaması olarak seçildiğinde kuruluşlar İk departmanının tüm üyelerini içeren bir grup seçebilir. Grup, dinamik veya atanmış güvenlik ve dağıtım grupları da dahil olmak üzere Azure AD'deki herhangi bir grup türü olabilir.

## <a name="exclude-users"></a>Kullanıcıları hariç tutma

Kuruluşlar bir kullanıcıyı veya grubu hem dahil ettihem de dışladığında, bir dışlama eylemi ilkedeki bir dahiliyeyi geçersiz kıldığından, kullanıcı veya grup ilkenin dışında tutulur. Dışlamalar genellikle acil erişim veya kesme cam hesapları için kullanılır. Acil erişim hesapları ve bunların neden önemli olduğu hakkında daha fazla bilgiyi aşağıdaki makalelerde bulabilirsiniz: 

* [Azure AD'de acil erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile esnek bir erişim denetimi yönetimi stratejisi oluşturun](../authentication/concept-resilient-controls.md)

Koşullu Erişim ilkesi oluşturulurken aşağıdaki seçenekleri hariç tutmak için kullanılabilir.

- Tüm konuk ve dış kullanıcılar
   - Bu seçim, b2B `user type` `guest`konukları ve dış kullanıcıları içerir. Bu seçim, Bulut Çözüm Sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm harici kullanıcı için de geçerlidir. 
- Dizin rolleri
   - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure REKLAM dizini rollerini seçmelerine olanak tanır. Örneğin, kuruluşlar genel yönetici rolü atanan kullanıcılar üzerinde daha kısıtlayıcı bir ilke oluşturabilir.
- Kullanıcılar ve gruplar
   - Belirli kullanıcı kümelerinin hedeflenemesine izin verir. Örneğin, bir İk uygulaması bulut uygulaması olarak seçildiğinde kuruluşlar İk departmanının tüm üyelerini içeren bir grup seçebilir. Grup, dinamik veya atanmış güvenlik ve dağıtım grupları da dahil olmak üzere Azure AD'deki herhangi bir grup türü olabilir.

### <a name="preventing-administrator-lockout"></a>Yönetici kilitlemesini önleme

**Tüm kullanıcılara** ve **tüm uygulamalara**uygulanan bir ilke oluştururken bir yöneticinin kendilerini dizinlerinin dışına kilitlemesini önlemek için aşağıdaki uyarıyı görürler.

> Kendini dışarıda bırakmayın! Bir ilkeyi beklendiği gibi doğrulamak için önce küçük bir kullanıcı kümesine uygulamanızı öneririz. Ayrıca, bu ilkeden en az bir yöneticinin hariç çıkarılmasını öneririz. Bu, hala erişiminiz olmasını sağlar ve bir değişiklik gerekirse bir ilkeyi güncelleştirebilirsiniz. Lütfen etkilenen kullanıcıları ve uygulamaları inceleyin.

Varsayılan olarak ilke, geçerli kullanıcıyı ilkeden dışlama seçeneği sağlar, ancak bu varsayılan değer aşağıdaki resimde gösterildiği gibi yönetici tarafından geçersiz kılınabilir. 

![Uyarı, kendini kilitleme!](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups-lockout-warning.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu Erişim: Bulut uygulamaları veya eylemleri](concept-conditional-access-cloud-apps.md)

- [Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)
