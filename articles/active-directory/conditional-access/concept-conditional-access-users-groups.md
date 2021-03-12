---
title: Koşullu erişim ilkesindeki kullanıcılar ve gruplar-Azure Active Directory
description: Bir Azure AD koşullu erişim ilkesinde kullanıcılar ve gruplar kim
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/04/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7f316b17096e1241fe23cbf2c965122fd8966522
ms.sourcegitcommit: b572ce40f979ebfb75e1039b95cea7fce1a83452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2021
ms.locfileid: "102630886"
---
# <a name="conditional-access-users-and-groups"></a>Koşullu erişim: kullanıcılar ve gruplar

Koşullu erişim ilkesi, karar işlemindeki sinyallerden biri olarak bir kullanıcı ataması içermelidir. Kullanıcılar koşullu erişim ilkelerine dahil edilebilir veya dışlanıyor olabilir. Azure Active Directory tüm ilkeleri değerlendirir ve kullanıcıya erişim vermeden önce tüm gereksinimlerin karşılanmasını sağlar. Bu makaleye ek olarak, aşağıda özetlenen işlem boyunca size yol gösteren [koşullu erişim ilkelerinin kullanıcılarını nasıl dahil ettireceğiz veya hariç bırakacağınız](https://www.youtube.com/watch?v=5DsW1hB3Jqs) hakkında bir video sunuyoruz. 

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
      - Yöneticilerin, ilke atamasını belirlemek için kullanılan yerleşik Azure AD dizin rollerini seçmesine olanak sağlar. Örneğin kuruluşlar, genel yönetici rolüne atanan kullanıcılara daha kısıtlayıcı bir ilke oluşturabilir. Yönetim birimi kapsamlı Dizin rolleri, özel roller de dahil olmak üzere diğer rol türleri desteklenmez.
   - Kullanıcılar ve gruplar
      - Belirli kullanıcı kümelerinin hedeflenmesini sağlar. Örneğin, kuruluşlar, bulut uygulaması olarak bir ık uygulaması seçildiğinde ık departmanın tüm üyelerini içeren bir grup seçebilir. Bir grup, dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere Azure AD 'de herhangi bir grup türü olabilir. İlke, iç içe geçmiş kullanıcılara ve gruplara uygulanır.

> [!IMPORTANT]
> Koşullu erişim Ilkesine hangi kullanıcıların ve grupların dahil edileceğini seçerken, doğrudan koşullu erişim ilkesine eklenebilen bireysel kullanıcı sayısı için bir sınır vardır. Bir koşullu erişim ilkesine doğrudan eklenmesi gereken büyük miktarda bireysel kullanıcı varsa, kullanıcıları bir gruba yerleştirmenizi ve bunun yerine grubu koşullu erişim ilkesine atamayı öneririz.

> [!WARNING]
> Kullanıcılar veya gruplar 2048 'den fazla grup üyesiyse, erişimleri engellenebilir. Bu sınır hem doğrudan hem de iç içe Grup üyeliği için geçerlidir.

> [!WARNING]
> Koşullu erişim ilkeleri, [özel roller](../roles/custom-create.md)gibi doğrudan bir nesne için bir [yönetim birimi](../roles/admin-units-assign-roles.md) veya dizin rollerine kapsamlı bir dizin rolü atanmış olan kullanıcıları desteklemez.

## <a name="exclude-users"></a>Kullanıcıları hariç tut

Kuruluşların her ikisi de bir kullanıcıyı veya grubu dahil ve hariç tutdığında, Kullanıcı veya grup ilkeden hariç tutulur, hariç tutma eylemi ilkede bir içerme işlemini geçersiz kılar. Dışlamalar genellikle acil durum erişimi veya kesme camı hesapları için kullanılır. Acil durum erişim hesapları ve neden önemli oldukları hakkında daha fazla bilgi aşağıdaki makalelerde bulunabilir: 

* [Azure AD'deki acil durum erişim hesaplarını yönetme](../roles/security-emergency-access.md)
* [Azure Active Directory ile dayanıklı bir erişim denetimi yönetim stratejisi oluşturma](../authentication/concept-resilient-controls.md)

Koşullu erişim ilkesi oluştururken aşağıdaki seçenekler dışarıda tutulacak.

- Tüm konuk ve dış kullanıcılar
   - Bu seçim, özniteliği olarak ayarlanmış herhangi bir kullanıcı dahil olmak üzere tüm B2B konukları ve harici kullanıcıları içerir `user type` `guest` . Bu seçim Ayrıca, bulut çözümü sağlayıcısı (CSP) gibi farklı bir kuruluştan oturum açmış tüm dış kullanıcılar için de geçerlidir. 
- Dizin rolleri
   - Yöneticilerin atamayı belirlemek için kullanılan belirli Azure AD dizin rollerini seçmesine olanak sağlar. Örneğin kuruluşlar, genel yönetici rolüne atanan kullanıcılara daha kısıtlayıcı bir ilke oluşturabilir.
- Kullanıcılar ve gruplar
   - Belirli kullanıcı kümelerinin hedeflenmesini sağlar. Örneğin, kuruluşlar, bulut uygulaması olarak bir ık uygulaması seçildiğinde ık departmanın tüm üyelerini içeren bir grup seçebilir. Bir grup, dinamik veya atanan güvenlik ve dağıtım grupları dahil olmak üzere Azure AD 'de herhangi bir grup türü olabilir.

### <a name="preventing-administrator-lockout"></a>Yönetici kilitlemeyi önler

**Tüm kullanıcılara** ve **tüm uygulamalara** uygulanan bir ilke oluştururken bir yöneticinin kendilerini kendi dizininden kilitlemesini engellemek için aşağıdaki uyarıyı görür.

> Kendinizi kilitlemeyin! Önce küçük bir kullanıcı kümesine, beklendiği gibi davrandığını doğrulamak için bir ilke uygulanmasını öneririz. Ayrıca, bu ilkeden en az bir yöneticiyi dışlamamız önerilir. Bu, hala erişiminizin olmasını sağlar ve bir değişiklik gerekliyse bir ilkeyi güncelleştirebilir. Lütfen etkilenen kullanıcıları ve uygulamaları gözden geçirin.

Varsayılan olarak, ilke geçerli kullanıcıyı ilkeden hariç tutmak için bir seçenek sağlar, ancak bu varsayılan, aşağıdaki görüntüde gösterildiği gibi yönetici tarafından geçersiz kılınabilir. 

![Uyarı, kendinizi kilitlemeyin!](./media/concept-conditional-access-users-groups/conditional-access-users-and-groups-lockout-warning.png)

[Azure portal kilitlediyseniz ne yapmanız gerekir?](troubleshoot-conditional-access.md#what-to-do-if-you-are-locked-out-of-the-azure-portal)

## <a name="next-steps"></a>Sonraki adımlar

- [Koşullu erişim: bulut uygulamaları veya eylemleri](concept-conditional-access-cloud-apps.md)

- [Koşullu erişim ortak ilkeleri](concept-conditional-access-policy-common.md)
