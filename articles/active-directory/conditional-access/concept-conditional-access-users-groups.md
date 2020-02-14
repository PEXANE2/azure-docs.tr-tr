---
title: Koşullu erişim ilkesindeki kullanıcılar ve gruplar-Azure Active Directory
description: Bir Azure AD koşullu erişim ilkesinde kullanıcılar ve gruplar kim
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
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77192135"
---
# <a name="conditional-access-users-and-groups"></a>Koşullu erişim: kullanıcılar ve gruplar

Koşullu erişim ilkesi, karar işlemindeki sinyallerden biri olarak bir kullanıcı ataması içermelidir. Kullanıcılar koşullu erişim ilkelerine dahil edilebilir veya dışlanıyor olabilir. 

![Koşullu erişim tarafından yapılan kararlara sinyal olarak Kullanıcı](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups.png)

## <a name="include-users"></a>Kullanıcıları dahil et

Bu kullanıcı listesi genellikle bir kuruluşun koşullu erişim ilkesinde hedeflediği tüm kullanıcıları içerir. 

Koşullu erişim ilkesi oluştururken aşağıdaki seçenekler bulunabilir.

- Hiçbiri
   - Hiçbir Kullanıcı seçilmedi
- Tüm kullanıcılar
   - B2B konukları dahil dizinde bulunan tüm kullanıcılar.
- Kullanıcıları ve grupları seçin
   - Tüm konuk ve dış kullanıcılar (Önizleme)
      - Bu seçim, `user type` özniteliği `guest`olarak ayarlanan herhangi bir kullanıcı dahil olmak üzere tüm B2B konukları ve harici kullanıcıları içerir. Bu seçim Ayrıca, bulut çözümü sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm dış kullanıcılar için de geçerlidir. 
   - Dizin rolleri (Önizleme)
      - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure AD dizin rollerini seçmesine olanak sağlar. Örneğin kuruluşlar, genel yönetici rolüne atanan kullanıcılara daha kısıtlayıcı bir ilke oluşturabilir.
   - Kullanıcılar ve gruplar
      - Belirli kullanıcı kümelerinin hedeflenmesini sağlar. Örneğin, kuruluşlar, bulut uygulaması olarak bir ık uygulaması seçildiğinde ık departmanın tüm üyelerini içeren bir grup seçebilir. Bir grup, dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere Azure AD 'de herhangi bir grup türü olabilir.

## <a name="exclude-users"></a>Kullanıcıları hariç tut

Dışlamalar genellikle acil durum erişimi veya kesme camı hesapları için kullanılır. Acil durum erişim hesapları ve neden önemli oldukları hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir: 

* [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile dayanıklı bir erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)

Koşullu erişim ilkesi oluştururken aşağıdaki seçenekler dışarıda tutulacak.

- Tüm konuk ve dış kullanıcılar (Önizleme)
   - Bu seçim, `user type` özniteliği `guest`olarak ayarlanan herhangi bir kullanıcı dahil olmak üzere tüm B2B konukları ve harici kullanıcıları içerir. Bu seçim Ayrıca, bulut çözümü sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm dış kullanıcılar için de geçerlidir. 
- Dizin rolleri (Önizleme)
   - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure AD dizin rollerini seçmesine olanak sağlar. Örneğin kuruluşlar, genel yönetici rolüne atanan kullanıcılara daha kısıtlayıcı bir ilke oluşturabilir.
- Kullanıcılar ve gruplar
   - Belirli kullanıcı kümelerinin hedeflenmesini sağlar. Örneğin, kuruluşlar, bulut uygulaması olarak bir ık uygulaması seçildiğinde ık departmanın tüm üyelerini içeren bir grup seçebilir. Bir grup, dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere Azure AD 'de herhangi bir grup türü olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim: bulut uygulamaları veya eylemleri](concept-conditional-access-cloud-apps.md)

- [Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)
