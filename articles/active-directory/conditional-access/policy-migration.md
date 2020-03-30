---
title: Koşullu Erişim ilkelerini geçir - Azure Etkin Dizini
description: Azure portalında klasik ilkeleri geçirmek için bilmeniz gerekenleri öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/04/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 33b1f76dd1489e00115d0f805add8d754038df84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77185919"
---
# <a name="conditional-access-classic-policy-migration"></a>Koşullu Erişim klasik ilke geçişi

Koşullu Erişim, Azure Active Directory tarafından sinyalleri bir araya getirmek, kararlar almak ve kuruluş ilkelerini uygulamak için kullanılan araçtır. Conditional Access, yeni kimlik güdümlü kontrol uçağının kalbinde yer alan bir yerdir. Amaç hala aynı olsa da, yeni Azure portalının sürümü Koşullu Erişim'in çalışma şekliyle ilgili önemli iyileştirmeler getirmiştir.

Azure portalında oluşturmadığınız ilkeleri geçirmeyi düşünün, çünkü:

- Artık daha önce işlenemediğiniz senaryoları ele alabilirsiniz.
- Bunları birleştirerek yönetmeniz gereken ilke sayısını azaltabilirsiniz.
- Tüm Koşullu Erişim ilkelerinizi tek bir merkezi konumda yönetebilirsiniz.
- Azure klasik portalı kullanımdan kaldırılacak.

Bu makalede, varolan Koşullu Erişim ilkelerinizi yeni çerçeveye geçirmek için bilmeniz gerekenler açıklanmaktadır.

## <a name="classic-policies"></a>Klasik ilkeler

Azure [portalında,](https://portal.azure.com)Koşullu Erişim ilkeleri **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim**altında bulunabilir. Kuruluşunuz da bu sayfayı kullanarak oluşturulmayan eski Koşullu Erişim ilkeleri olabilir. Bu ilkeler klasik *ilkeler*olarak bilinir. Klasik ilkeler Koşullu Erişim ilkeleridir, oluşturduğunuz:

- Azure klasik portalı
- Intune klasik portal
- Intune Uygulama Koruma portalı

**Koşullu Erişim** sayfasında, **Yönet** bölümündeki [**Klasik ilkeleri**](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/ClassicPolicies) tıklayarak klasik ilkelerinize erişebilirsiniz. 

![Azure AD'de koşullu erişim klasik ilkeler görünümünü gösterir](./media/policy-migration/71.png)

**Klasik ilkeler** görünümü size aşağıdakileri yapmak için bir seçenek sunar:

- Klasik ilkelerinizi filtreleyin.
- Klasik ilkeleri devre dışı.
- Klasik bir ilkenin ayarlarını gözden geçirin ve devre dışı edin.

   ![Varolan ilke yapılandırması da dahil olmak üzere klasik ilke ayrıntıları](./media/policy-migration/74.png)

> [!WARNING]
> Klasik bir ilke devre dışı bırakıldıktan sonra yeniden etkinleştirilenemez.

Klasik bir ilkenin ayrıntıları görünümü, ayarları belgelemenize, dahil edilen veya dışlanan grupları değiştirmenize ve ilkeyi devre dışı etmenize olanak tanır.

![İlke ayrıntıları - İçerecek veya hariç tutacağı gruplar](./media/policy-migration/75.png)

Seçili grupları değiştirerek veya belirli grupları hariç tutarak, dahil olan tüm kullanıcılar ve gruplar için ilkeyi devre dışı bırakmadan önce birkaç test kullanıcısı için devre dışı bırakılmış klasik ilkenin etkisini sınayabilirsiniz.
 
## <a name="migration-considerations"></a>Geçiş sırasında dikkat edilmesi gerekenler

Bu makalede, Azure AD Koşullu Erişim ilkeleri *de yeni ilkeler*olarak adlandırılır.
Klasik ilkeleriniz, siz bunları devre dışı bırakana veya silene kadar yeni ilkelerinizle yan yana çalışmaya devam eder. 

Bir politika konsolidasyonu bağlamında aşağıdaki hususlar önemlidir:

- Klasik ilkeler belirli bir bulut uygulamasına bağlı olsa da, yeni bir ilkede istediğiniz kadar bulut uygulaması seçebilirsiniz.
- Klasik bir ilkenin denetimleri ve bir bulut uygulaması için yeni bir ilke tüm denetimlerin *(AND)* yerine getirilmesini gerektirir. 
- Yeni bir ilkede şunları yapabilirsiniz:
   - Senaryonuz tarafından gerekirse birden çok koşulu birleştirin. 
   - Erişim denetimi olarak birkaç hibe gereksinimi seçin ve bunları mantıksal bir *OR* (seçili denetimlerden birini gerektirir) veya mantıksal *ve* (seçili denetimlerin tümünü gerektirir) ile birleştirin.

### <a name="office-365-exchange-online"></a>Office 365 Exchange çevrimiçi

**Office 365 Exchange** için istemci uygulamaları koşulu olarak **Exchange Active Sync'i** içeren klasik ilkeleri çevrimiçi olarak geçirmek istiyorsanız, bunları yeni bir ilke halinde birleştiremeyebilirsiniz. 

Bu, örneğin, tüm istemci uygulama türlerini desteklemek istiyorsanız durumdur. İstemci uygulamaları koşulu olarak **Exchange Active Sync** olan yeni bir ilkede, diğer istemci uygulamalarını seçemezsiniz.

![İstemci uygulamalarını seçme koşullu Erişim](./media/policy-migration/64.png)

Klasik ilkeleriniz çeşitli koşullar içeriyorsa, yeni bir ilke de birleştirilmesi mümkün değildir. Müşteri uygulamaları koşulolarak yapılandırılan **Exchange Active Sync** olan yeni bir ilke diğer koşulları desteklemez:   

![Exchange ActiveSync seçili koşulları desteklemez](./media/policy-migration/08.png)

İstemci uygulamaları koşulu yapılandırıldıkça **Exchange Active Sync'e** sahip yeni bir ilkeniz varsa, diğer tüm koşulların yapılandırılmadığını zedeleniz gerekir. 

![Koşullu Erişim koşulları](./media/policy-migration/16.png)
 
Office 365 Exchange Online için, istemci uygulamaları koşulu olarak **Exchange Active Sync'i** içeren uygulama tabanlı klasik ilkeler **desteklenen** ve **desteklenmeyen** aygıt platformlarına izin verir. İlgili yeni bir ilkede tek tek aygıt platformlarını yapılandıramasanız da, desteği yalnızca [desteklenen aygıt platformları](concept-conditional-access-conditions.md#device-platforms) ile sınırlandırabilirsiniz. 

![Koşullu Erişim Exchange ActiveSync seçin](./media/policy-migration/65.png)

Varsa, istemci uygulamaları koşulu olarak **Exchange Active Sync'i** içeren birden çok klasik ilkeyi birleştirebilirsiniz:

- Koşul olarak yalnızca **Exchange Etkin Eşitleme** 
- Yapılandırılan erişim izni için çeşitli gereksinimler

Ortak senaryolardan biri şu şekildedir:

- Azure klasik portalından aygıt tabanlı klasik ilke 
- Intune uygulama koruma portalında uygulama tabanlı klasik bir ilke 
 
Bu durumda, klasik ilkelerinizi her iki gereksinimi de seçili olan yeni bir ilke de birleştirebilirsiniz.

![Koşullu Erişim hibe denetimleri](./media/policy-migration/62.png)

### <a name="device-platforms"></a>Cihaz platformları

Uygulama tabanlı denetimlere sahip klasik ilkeler, aygıt platformu koşulu olarak iOS ve Android ile önceden yapılandırılmıştır. 

Yeni bir ilkede, tek tek desteklemek istediğiniz [aygıt platformlarını](concept-conditional-access-conditions.md#device-platforms) seçmeniz gerekir.

![Koşullu Erişim aygıt platformları seçimi](./media/policy-migration/41.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni ilke kararlarının etkisini belirlemek için Koşullu Erişim için yalnızca rapor modunu kullanın.](concept-conditional-access-report-only.md)
- Koşullu Erişim ilkesini nasıl yapılandıracağınıbilmek istiyorsanız, [Koşullu Erişim ortak ilkelerine](concept-conditional-access-policy-common.md)bakın.
- Ortamınız için Koşullu Erişim ilkelerini yapılandırmaya hazırsanız, [Azure Etkin Dizini'nde Koşullu Erişim dağıtımınızı nasıl planlayın makalesine](plan-conditional-access.md)bakın. 
