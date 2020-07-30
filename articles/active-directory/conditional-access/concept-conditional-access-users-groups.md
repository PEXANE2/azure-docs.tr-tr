---
title: Koşullu erişim ilkesindeki kullanıcılar ve gruplar-Azure Active Directory
description: Bir Azure AD koşullu erişim ilkesinde kullanıcılar ve gruplar kim
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
ms.openlocfilehash: 127d76bedafd4c857686e93f21d3cccdc36b98dd
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87274672"
---
# <a name="conditional-access-users-and-groups"></a>Koşullu erişim: kullanıcılar ve gruplar

Koşullu erişim ilkesi, karar işlemindeki sinyallerden biri olarak bir kullanıcı ataması içermelidir. Kullanıcılar koşullu erişim ilkelerine dahil edilebilir veya dışlanıyor olabilir. 

![Koşullu erişim tarafından yapılan kararlara sinyal olarak Kullanıcı](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups.png)

## <a name="include-users"></a>Kullanıcıları dahil et

Bu kullanıcı listesi genellikle bir kuruluşun koşullu erişim ilkesinde hedeflediği tüm kullanıcıları içerir. 

Koşullu erişim ilkesi oluştururken aşağıdaki seçenekler bulunabilir.

- Yok
   - Hiçbir Kullanıcı seçilmedi
- Tüm kullanıcılar
   - B2B konukları dahil dizinde bulunan tüm kullanıcılar.
- Kullanıcıları ve grupları seçme
   - Tüm konuk ve dış kullanıcılar
      - Bu seçim, özniteliği olarak ayarlanmış herhangi bir kullanıcı dahil olmak üzere tüm B2B konukları ve harici kullanıcıları içerir `user type` `guest` . Bu seçim Ayrıca, bulut çözümü sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm dış kullanıcılar için de geçerlidir. 
   - Dizin rolleri
      - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure AD dizin rollerini seçmesine olanak sağlar. Örneğin kuruluşlar, genel yönetici rolüne atanan kullanıcılara daha kısıtlayıcı bir ilke oluşturabilir.
   - Kullanıcılar ve gruplar
      - Belirli kullanıcı kümelerinin hedeflenmesini sağlar. Örneğin, kuruluşlar, bulut uygulaması olarak bir ık uygulaması seçildiğinde ık departmanın tüm üyelerini içeren bir grup seçebilir. Bir grup, dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere Azure AD 'de herhangi bir grup türü olabilir. İlke, iç içe geçmiş kullanıcılara ve gruplara uygulanır.

> [!WARNING]
> Kullanıcılar veya gruplar 2048 'den fazla grup üyesiyse, erişimleri engellenebilir. Bu sınır hem doğrudan hem de iç içe Grup üyeliği için geçerlidir.

## <a name="exclude-users"></a>Kullanıcıları hariç tut

Kuruluşların her ikisi de bir kullanıcıyı veya grubu dahil ve hariç tutdığında, Kullanıcı veya grup ilkeden hariç tutulur, hariç tutma eylemi ilkede bir içerme işlemini geçersiz kılar. Dışlamalar genellikle acil durum erişimi veya kesme camı hesapları için kullanılır. Acil durum erişim hesapları ve neden önemli oldukları hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir: 

* [Azure AD 'de acil durum erişim hesaplarını yönetme](../users-groups-roles/directory-emergency-access.md)
* [Azure Active Directory ile dayanıklı bir erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)

Koşullu erişim ilkesi oluştururken aşağıdaki seçenekler dışarıda tutulacak.

- Tüm konuk ve dış kullanıcılar
   - Bu seçim, özniteliği olarak ayarlanmış herhangi bir kullanıcı dahil olmak üzere tüm B2B konukları ve harici kullanıcıları içerir `user type` `guest` . Bu seçim Ayrıca, bulut çözümü sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm dış kullanıcılar için de geçerlidir. 
- Dizin rolleri
   - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure AD dizin rollerini seçmesine olanak sağlar. Örneğin kuruluşlar, genel yönetici rolüne atanan kullanıcılara daha kısıtlayıcı bir ilke oluşturabilir.
- Kullanıcılar ve gruplar
   - Belirli kullanıcı kümelerinin hedeflenmesini sağlar. Örneğin, kuruluşlar, bulut uygulaması olarak bir ık uygulaması seçildiğinde ık departmanın tüm üyelerini içeren bir grup seçebilir. Bir grup, dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere Azure AD 'de herhangi bir grup türü olabilir.

### <a name="preventing-administrator-lockout"></a>Yönetici kilitlemeyi önler

**Tüm kullanıcılara** ve **tüm uygulamalara**uygulanan bir ilke oluştururken bir yöneticinin kendilerini kendi dizininden kilitlemesini engellemek için aşağıdaki uyarıyı görür.

> Kendinizi kilitlemeyin! Önce küçük bir kullanıcı kümesine, beklendiği gibi davrandığını doğrulamak için bir ilke uygulanmasını öneririz. Ayrıca, bu ilkeden en az bir yöneticiyi dışlamamız önerilir. Bu, hala erişiminizin olmasını sağlar ve bir değişiklik gerekliyse bir ilkeyi güncelleştirebilir. Lütfen etkilenen kullanıcıları ve uygulamaları gözden geçirin.

Varsayılan olarak, ilke geçerli kullanıcıyı ilkeden hariç tutmak için bir seçenek sağlar, ancak bu varsayılan, aşağıdaki görüntüde gösterildiği gibi yönetici tarafından geçersiz kılınabilir. 

![Uyarı, kendinizi kilitlemeyin!](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups-lockout-warning.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim: bulut uygulamaları veya eylemleri](concept-conditional-access-cloud-apps.md)

- [Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)
