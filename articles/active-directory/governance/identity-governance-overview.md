---
title: Identity Idare-Azure Active Directory | Microsoft Docs
description: Azure Active Directory Identity Governance, doğru işlemlerle ve görünürlüğe sahip güvenlik ve çalışanların üretkenliğini sağlamak için kuruluşunuzun gereksinimini dengelemenize olanak tanır.
services: active-directory
documentationcenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 08/25/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 014a2c3cff3804657e4e2bf624b97eceef4bf4b2
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70033317"
---
# <a name="what-is-azure-ad-identity-governance"></a>Azure AD Identity Governance nedir?

Azure Active Directory (Azure AD) Identity Idare, kuruluşunuzun güvenlik ve çalışanların verimliliğini doğru işlemlerle ve görünürlüğe dengelemenize olanak tanır. Doğru kullanıcıların doğru kaynaklara doğru erişim sahibi olmasını sağlamak için size olan özellikleri sağlar ve kritik varlıklara erişimi korumanıza, izlemenize ve denetlemenize olanak sağlarken, bu sayede çalışanların üretkenliğini temin edebilirsiniz.  

Kimlik yönetimi, kuruluşlara çalışanlar, iş ortakları ve satıcılar, hizmetler ve uygulamalar arasında aşağıdaki görevleri yapma olanağı sunar:

- Kimlik yaşam döngüsünü yönetir
- Erişim yaşam döngüsünü yönetir
- Güvenli yönetim

Özellikle, kuruluşların bu dört önemli soruyu ele almak için tasarlanmıştır:

- Hangi kullanıcıların hangi kaynaklara erişimi olmalıdır?
- Bu erişimi hangi kullanıcılar yapıyor?
- Erişimi yönetmeye yönelik etkili bir kurumsal denetim var mı?
- Denetçilerin çalıştığını doğrulaması yapılabilir mi?

## <a name="identity-lifecycle"></a>Kimlik yaşam döngüsü

Kimlik yönetimi, kuruluşların *üretkenlik* arasında bir denge elde etmesine yardımcı olur ve kuruluşa ne zaman katılabilecekleri gibi bir kişinin ihtiyacı olan kaynaklara ne kadar hızlı bir şekilde erişmesini sağlayabilir? Ve *güvenlik* -bu kişinin iş durumundaki değişiklikler nedeniyle, zaman içinde erişimin ne zaman içinde değiştirilmesi gerekir?  Kimlik yaşam döngüsü yönetimi, kimlik yönetimi için temel ve ölçekteki etkili idare, uygulamalar için kimlik yaşam döngüsü yönetim altyapısını modernleştirmeyi gerektirir.

![Kimlik yaşam döngüsü](./media/identity-governance-overview/identity-lifecycle.png)

Birçok kuruluşta, çalışanlar için kimlik yaşam döngüsü, bu kullanıcının bir HCM (insan büyük yönetim) sisteminde temsiline bağlıdır.  Azure AD Premium, Workday [gelen sağlama (Önizleme) öğreticisinde](../saas-apps/workday-inbound-tutorial.md)açıklandığı gibi Active Directory ve Azure Active Directory Workday 'de temsil edilen kişiler için Kullanıcı kimliklerini otomatik olarak korur.  Azure AD Premium ayrıca, SAP, Oracle eBusiness ve Oracle PeopleSoft gibi şirket içi HCM sistemlerinden kayıtları içeri aktarabilen [Microsoft Identity Manager](/microsoft-identity-manager/)da içerir.

Giderek, senaryolar, kuruluşunuzun dışındaki kişilerle işbirliği gerektirir. [Azure AD B2B](/azure/active-directory/b2b/) işbirliği, kuruluşunuzun uygulamalarını ve hizmetlerini herhangi bir kuruluştan Konuk kullanıcılar ve dış iş ortakları ile güvenli bir şekilde paylaşmanıza olanak sağlarken kendi şirket verileriniz üzerinde denetim sağlar.

## <a name="access-lifecycle"></a>Erişim yaşam döngüsü

Kuruluşların, Kullanıcı kimliğinin oluşturulduğu zaman bir kullanıcı için başlangıçta sağlananların ötesinde erişimi yönetmek için bir işlem yapması gerekir.  Ayrıca, kurumsal kuruluşların, erişim ilkesini ve denetimleri sürekli olarak geliştirip zorlayabilmeleri için verimli bir şekilde ölçeklendirilebilecek.

![Erişim yaşam döngüsü](./media/identity-governance-overview/access-lifecycle.png)

Genellikle, iş karar mekanizmalarının onay kararlarını devreder.  Ayrıca, kullanıcıların kendilerini de içerebilir.  Örneğin, Avrupa 'daki bir şirketin pazarlama uygulamasındaki gizli müşteri verilerine erişen kullanıcıların şirket ilkelerini bilmeleri gerekir. Konuk kullanıcılar, davet edildikleri bir kuruluştaki veriler için işleme gereksinimlerini farkında olabilir.

Kuruluşlar, [SIM ile tümleştirilmiş](../manage-apps/use-scim-to-provision-users-and-groups.md) [SaaS uygulamalarına](../saas-apps/tutorial-list.md) veya uygulamalarına Kullanıcı sağlama ile bağlanmış [Dinamik Gruplar](../users-groups-roles/groups-dynamic-membership.md)gibi teknolojiler aracılığıyla erişim yaşam döngüsü işlemini otomatikleştirebilir.  Kuruluşlar ayrıca hangi [Konuk kullanıcıların şirket içi uygulamalara erişebileceğini](../b2b/hybrid-cloud-to-on-premises.md)de denetleyebilir.  Bu erişim hakları daha sonra yinelenen [Azure AD erişim İncelemeleri](access-reviews-overview.md)kullanılarak düzenli olarak gözden geçirilebilir.

Bir Kullanıcı uygulamalara erişmeye çalıştığında, Azure AD [koşullu erişim](/azure/active-directory/conditional-access/) ilkeleri uygular. Örneğin, koşullu erişim ilkeleri bir [kullanım koşulları](../conditional-access/terms-of-use.md) görüntülemeyi ve kullanıcının bir uygulamaya erişebilmek üzere [Bu koşulları kabul](../conditional-access/require-tou.md) etmesinin mümkün olmasını içerebilir.

## <a name="privileged-access-lifecycle"></a>Ayrıcalıklı erişim yaşam döngüsü

Geçmişte, ayrıcalıklı erişim diğer satıcılar tarafından kimlik yönetimi 'nden ayrı bir özellik olarak açıklanmıştır. Ancak, Microsoft 'ta, ayrıcalıklı erişim 'in bir kimlik yönetimi 'nin önemli bir parçası olduğunu düşündük, özellikle de bu yönetici haklarıyla ilişkili kötüye kullanımı olasılığı kuruluşa neden olabilir. Yönetim haklarını kullanan çalışanların, satıcıların ve yüklenicilerin yönetilmelidir.

![Ayrıcalıklı erişim yaşam döngüsü](./media/identity-governance-overview/privileged-access-lifecycle.png)

Azure AD Privileged Identity Management (PıM), Azure AD, Azure ve diğer Microsoft çevrimiçi hizmetleri arasında kaynaklara yönelik erişim haklarının güvenliğini sağlamaya yönelik ek denetimler sağlar.  Azure AD PıM tarafından sağlanan tam zamanında erişim ve rol değişikliği uyarısı özellikleri, Multi-Factor Authentication ve koşullu erişim 'in yanı sıra, şirketinizin kaynaklarını güvenli hale getirmeye yardımcı olmak için kapsamlı bir idare denetimleri kümesi sağlar (Dizin, Office 365 ve Azure Kaynak rolleri). Diğer erişim formlarında olduğu gibi, kuruluşlar yönetici rollerindeki tüm kullanıcılar için yinelenen erişim yeniden sertifika yapılandırmak üzere erişim gözden geçirmeleri kullanabilir.

## <a name="getting-started"></a>Başlarken

Her müşteri için kusursuz bir çözüm veya öneri olmasa da, aşağıdaki yapılandırmalarda, Microsoft 'un daha güvenli ve üretken bir iş gücünün sağlanması için izlemeniz önerilen temel ilkeler hakkında bir kılavuz bulunur.

- [Kimlik ve cihaz erişim yapılandırmaları](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Ayrıcalıklı erişimin güvenliğini sağlama](../users-groups-roles/directory-admin-roles-secure.md)

Yetkilendirme Yönetimi, erişim incelemeleri, Privileged Identity Management ve Kullanım koşulları kullanmaya başlamak için Azure portal **kimlik** yönetimi 'nin Başlarken sekmesini de denetleyebilirsiniz.

![Identity Idare Başlarken](./media/identity-governance-overview/getting-started.png)

Kimlik yönetimi özellikleriyle ilgili geri bildiriminiz varsa, görüşlerinizi göndermek için Azure portal **geri bildirimde bulunun?** öğesine tıklayın. Ekip geri bildirimlerinizi düzenli olarak inceler.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD yetkilendirme yönetimi nedir? (Önizleme)](entitlement-management-overview.md)
- [Azure AD erişim gözden geçirmesi nedir?](access-reviews-overview.md)
- [Azure AD Privileged Identity Management nedir?](../privileged-identity-management/pim-configure.md)
- [Kullanım koşulları ile ne yapabilirim?](active-directory-tou.md)
