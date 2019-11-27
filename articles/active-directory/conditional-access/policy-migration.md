---
title: Koşullu erişim ilkelerini geçirme-Azure Active Directory
description: Azure portal klasik ilkeleri geçirmek için bilmeniz gerekenler hakkında bilgi edinin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: nigu
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75d664f6e61dbbaaf0b8ab74c392596a206ff644
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74380549"
---
# <a name="what-is-a-policy-migration-in-azure-active-directory-conditional-access"></a>Azure Active Directory Koşullu erişim ' de ilke geçişi nedir? 

[Koşullu erişim](../active-directory-conditional-access-azure-portal.md) , Azure Active Directory 'Nin (Azure AD), yetkili kullanıcıların bulut uygulamalarınıza nasıl erişebileceğini denetlemenize olanak tanıyan bir özelliktir. Amaç hala aynı olsa da, yeni Azure portal sürümü Koşullu erişimin nasıl çalıştığına ilişkin önemli iyileştirmeler getirmiştir.

Azure portal oluşturduğunuz ilkeleri şu nedenle geçirmeyi düşünün:

- Artık, daha önce işleyemeyen senaryolara adres ekleyebilirsiniz.
- Bunları birleştirerek yönetmeniz gereken ilke sayısını azaltabilirsiniz.   
- Tüm koşullu erişim ilkelerinizi tek bir merkezi konumda yönetebilirsiniz.
- Klasik Azure portalı kullanımdan kaldırılacak.   

Bu makalede, mevcut koşullu erişim ilkelerinizi yeni çerçeveye geçirmek için bilmeniz gerekenler açıklanmaktadır.
 
## <a name="classic-policies"></a>Klasik ilkeler

[Azure Portal](https://portal.azure.com) [koşullu erişim Ilkeleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) sayfası, koşullu erişim ilkeleriniz için giriş noktasıdır. Ancak ortamınızda, bu sayfayı kullanarak, sizin oluşturduğunuz koşullu erişim ilkelerine da sahip olabilirsiniz. Bu ilkeler *Klasik ilkeler*olarak bilinir. Klasik ilkeler, içinde oluşturduğunuz koşullu erişim ilkelerdir:

- Klasik Azure portalı
- Klasik Intune portalı
- Intune Uygulama Koruması portalı

**Koşullu erişim** sayfasında, **Yönet** bölümünde [**Klasik ilkeler (Önizleme)** ](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) seçeneğine tıklayarak klasik ilkelerinize erişebilirsiniz. 

![Azure Active Directory](./media/policy-migration/71.png)

**Klasik ilkeler** görünümü şunları yapmak için bir seçenek sunar:

- Klasik ilkelerinize filtre uygulayın.
 
   ![Azure Active Directory](./media/policy-migration/72.png)

- Klasik ilkeleri devre dışı bırakın.

   ![Azure Active Directory](./media/policy-migration/73.png)
   
- Klasik bir ilkenin ayarlarını gözden geçirin (ve devre dışı bırakmak için).

   ![Azure Active Directory](./media/policy-migration/74.png)

Klasik bir ilkeyi devre dışı bırakırsanız, bu adımı artık döndüremezsiniz. Bu nedenle, **Ayrıntılar** görünümünü kullanarak klasik bir ilkede grup üyeliğini değiştirebilirsiniz. 

![Azure Active Directory](./media/policy-migration/75.png)

Seçilen grupları değiştirerek veya belirli grupları dışlayarak, tüm dahil edilen kullanıcılar ve gruplar için ilkeyi devre dışı bırakmadan önce, devre dışı bırakılmış bir klasik ilkenin etkisini birkaç test kullanıcısı için test edebilirsiniz. 

## <a name="azure-ad-conditional-access-policies"></a>Azure AD koşullu erişim ilkeleri

Azure portal koşullu erişimle, tüm ilkelerinizi tek bir merkezi konumda yönetebilirsiniz. Koşullu erişimin nasıl değiştiği konusunda, klasik ilkelerinizi geçirmeden önce temel kavramları öğrenmeniz gerekir.

Bkz.

- Temel kavramlar ve terminoloji hakkında bilgi edinmek için [Azure Active Directory Koşullu erişim nedir?](../active-directory-conditional-access-azure-portal.md) .
- Kuruluşunuzda koşullu erişim dağıtımı hakkında bazı yönergeler almak için [Azure Active Directory ' de koşullu erişim Için en iyi yöntemler](best-practices.md) .
- [Azure Active Directory Koşullu erişimi olan belirli uygulamalar IÇIN MFA](app-based-mfa.md) 'yı, Azure Portal Kullanıcı arabirimine tanımak üzere isteyin.
 
## <a name="migration-considerations"></a>Geçiş sırasında dikkat edilmesi gerekenler

Bu makalede, Azure AD koşullu erişim ilkelerine *yeni ilkeler*de denir.
Klasik ilkelerinizde, siz devre dışı bırakana kadar yeni ilkeleriniz ile yan yana çalışmaya devam edersiniz. 

Aşağıdaki yönleri bir ilke birleştirme bağlamında önemlidir:

- Klasik ilkeler belirli bir bulut uygulamasına bağlı olsa da, yeni bir ilkede gereken sayıda bulut uygulaması seçebilirsiniz.
- Klasik bir ilkenin denetimleri ve bir bulut uygulaması için yeni bir ilke, tüm denetimlerin (*ve*) yerine getirilmesi gerekir. 
- Yeni bir ilkede şunları yapabilirsiniz:
   - Senaryonuz için gerekliyse birden çok koşulu birleştirin. 
   - Erişim denetimi olarak birkaç verme gereksinimi seçin ve bunları mantıksal *veya* (seçili denetimlerden birini gerektir) ya da mantıksal *ve* (seçili denetimlerin tümünü gerektir) ile birleştirin.

   ![Azure Active Directory](./media/policy-migration/25.png)

### <a name="office-365-exchange-online"></a>Office 365 Exchange Online

İstemci uygulamaları gibi **Exchange Active Sync** içeren **Office 365 Exchange Online** için klasik ilkeleri geçirmek istiyorsanız, bunları yeni bir ilkede birleştirmeyebilirsiniz. 

Bu, örneğin, tüm istemci uygulama türlerini desteklemek istiyorsanız, bu durumda olur. İstemci uygulamaları olarak **Exchange Active Sync** olan yeni bir ilkede, diğer istemci uygulamalarını seçemezsiniz.

![Azure Active Directory](./media/policy-migration/64.png)

Klasik ilkeleriniz çeşitli koşullar içeriyorsa, tek bir yeni ilkeye birleştirme de mümkün değildir. İstemci uygulamaları tarafından yapılandırılan **Exchange Active Sync** olan yeni bir ilke diğer koşulları desteklemez:   

![Azure Active Directory](./media/policy-migration/08.png)

İstemci uygulamaları koşulu yapılandırılmış **Exchange Active Sync** olan yeni bir ilkeniz varsa, diğer tüm koşulların yapılandırılmadığından emin olmanız gerekir. 

![Azure Active Directory](./media/policy-migration/16.png)
 
İstemci uygulamaları koşulu olarak **Exchange Active Sync** içeren Office 365 Exchange Online için [uygulama tabanlı](technical-reference.md#approved-client-app-requirement) klasik ilkeler **desteklenen** ve **Desteklenmeyen** [cihaz platformlarına](technical-reference.md#device-platform-condition)izin verir. İlişkili yeni ilkede ayrı cihaz platformları yapılandıramamanıza karşın, desteği yalnızca [desteklenen cihaz platformlarıyla](technical-reference.md#device-platform-condition) sınırlayabilirsiniz. 

![Azure Active Directory](./media/policy-migration/65.png)

Şunları içeriyorsa, istemci uygulamaları olarak **Exchange Active Sync** içeren birden fazla klasik ilkeyi birleştirebilirsiniz:

- Yalnızca koşul olarak **Exchange Active Sync** 
- Erişim vermek için çeşitli gereksinimler

Yaygın olarak kullanılan bir senaryo şu şekilde birleştirilir:

- Klasik Azure portalından cihaz tabanlı klasik bir ilke 
- Intune uygulama koruma portalında uygulama tabanlı klasik bir ilke 
 
Bu durumda, klasik ilkelerinizi her iki gereksinimin de seçildiği yeni bir ilkede birleştirebilirsiniz.

![Azure Active Directory](./media/policy-migration/62.png)

### <a name="device-platforms"></a>Cihaz platformları

[Uygulama tabanlı denetimlerle](technical-reference.md#approved-client-app-requirement) klasik ilkeler, [Cihaz platformu koşulu](technical-reference.md#device-platform-condition)olarak iOS ve Android ile önceden yapılandırılmıştır. 

Yeni bir ilkede, desteklemek istediğiniz [cihaz platformlarını](technical-reference.md#device-platform-condition) tek tek seçmeniz gerekir.

![Azure Active Directory](./media/policy-migration/41.png)

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini nasıl yapılandıracağınızı öğrenmek isterseniz bkz. [koşullu erişim Azure Active Directory belirli uygulamalar IÇIN MFA gerektirme](app-based-mfa.md).
- Ortamınız için koşullu erişim ilkelerini yapılandırmaya hazırsanız, [Azure Active Directory Koşullu erişim için en iyi yöntemlere](best-practices.md)bakın. 
