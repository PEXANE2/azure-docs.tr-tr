---
title: Kimlik Yönetimi - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory Identity Governance, kuruluşunuzun güvenlik ve çalışan üretkenliği ne kadar önemli olduğunu doğru süreçler ve görünürlükle dengelemenize olanak tanır.
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
ms.date: 10/24/2019
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd319dd6a83a392f6df26d07a58be22a9c8bdb61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063688"
---
# <a name="what-is-azure-ad-identity-governance"></a>Azure AD Identity Governance nedir?

Azure Active Directory (Azure AD) Kimlik Yönetimi, kuruluşunuzun güvenlik ve çalışan üretkenliği ne kadar önemli olduğunu doğru süreçler ve görünürlükle dengelemenize olanak tanır. Doğru kişilerin doğru kaynaklara doğru erişime sahip olmasını sağlamak için size yetenekler sağlar. Bu ve ilgili Azure AD ve Kurumsal Mobilite + Güvenlik özellikleri, kritik varlıklara erişimi koruyarak, izleyerek ve denetleyerek erişim riskini azaltırken çalışan ve iş ortağı üretkenliğini sağlamanızı sağlar.  

Kimlik Yönetimi, kuruluşlara çalışanlar, iş ortakları ve satıcılar arasında ve hem şirket içinde hem de bulutlarda hizmetler ve uygulamalar arasında aşağıdaki görevleri yapma olanağı sağlar:

- Kimlik yaşam döngüsünü yönetme
- Erişim yaşam döngüsünü yönetin
- Yönetim için güvenli ayrıcalıklı erişim

Özellikle, kuruluşların bu dört temel soruyu ele almalarına yardımcı olmak için tasarlanmıştır:

- Hangi kullanıcılar hangi kaynaklara erişebilmeli?
- Bu kullanıcılar bu erişimle ne yapıyor?
- Erişimi yönetmek için etkili kuruluş denetimleri var mı?
- Denetçiler denetimlerin çalıştığını doğrulayabilir mi?

## <a name="identity-lifecycle"></a>Kimlik yaşam döngüsü

Kimlik Yönetimi, kuruluşların *üretkenlik* arasında bir denge kurmasını sağlar - Bir kişi kuruluşuma katıldığında olduğu gibi ihtiyaç duyduğu kaynaklara ne kadar hızlı erişebilir? Ve *güvenlik* - Bu kişinin istihdam durumundaki değişiklikler gibi erişimleri zaman içinde nasıl değişmeli?  Kimlik yaşam döngüsü yönetimi Kimlik Yönetişiminin temelidir ve ölçekte etkili yönetişim, uygulamalar için kimlik yaşam döngüsü yönetim altyapısının modernleştirilmesini gerektirir.

![Kimlik yaşam döngüsü](./media/identity-governance-overview/identity-lifecycle.png)

Birçok kuruluş için, çalışanlar için kimlik yaşam döngüsü bir HCM (insan sermayesi yönetimi) sisteminde bu kullanıcının temsil bağlıdır.  Azure AD Premium, İş Günü [gelen sağlama öğreticisinde](../saas-apps/workday-inbound-tutorial.md)açıklandığı gibi, Hem Active Directory hem de Azure Active Directory'de Iş Günü'nde temsil edilen kişilerin kullanıcı kimliklerini otomatik olarak tutar.  Azure AD Premium, SAP, Oracle eBusiness ve Oracle PeopleSoft gibi şirket içi HCM sistemlerinden kayıt içe aktarabilen [Microsoft Identity Manager'ı](/microsoft-identity-manager/)da içerir.

Senaryolar giderek daha fazla, kuruluşunuz dışındaki kişilerle işbirliği gerektirir. [Azure AD B2B](/azure/active-directory/b2b/) işbirliği, kuruluşunuzun uygulamalarını ve hizmetlerini herhangi bir kuruluştan konuk kullanıcılar ve dış ortaklarla güvenli bir şekilde paylaşmanızı sağlarken, kendi kurumsal verileriniz üzerinde denetim sağlar.  [Azure AD yetkilendirme yönetimi,](entitlement-management-overview.md) hangi kuruluşun kullanıcılarının erişim isteğinde bulunabileceklerini seçmenize ve kuruluşunuzun dizinine B2B misafiri olarak eklenebilir ve bu misafirlerin erişime artık ihtiyaç duymadıklarında kaldırılmasını sağlar.

## <a name="access-lifecycle"></a>Yaşam döngüsüne erişin

Kuruluşların, kullanıcının kimliği oluşturulduğunda, başlangıçta bir kullanıcı için sağlananın ötesinde erişimi yönetmek için bir işleme ihtiyacı vardır.  Ayrıca, kurumsal kuruluşların erişim ilkesi ve denetimlerini sürekli olarak geliştirebilmeleri ve uygulayabilmeleri için verimli bir şekilde ölçeklendirebilmeleri gerekir.

![Yaşam döngüsüne erişin](./media/identity-governance-overview/access-lifecycle.png)

Genellikle, BT temsilcileri iş karar vericilerine onay kararlarına erişir.  Ayrıca, BT kullanıcıların kendilerini içerebilir.  Örneğin, bir şirketin Avrupa'daki pazarlama uygulamasındaki gizli müşteri verilerine erişen kullanıcıların şirketin ilkelerini bilmesi gerekir. Konuk kullanıcılar, davet edildikleri bir kuruluştaki verilerin işleme gereksinimlerinden habersiz olabilir.

Kuruluşlar, [dinamik gruplar](../users-groups-roles/groups-dynamic-membership.md)gibi teknolojiler aracılığıyla erişim yaşam döngüsü işlemini otomatikleştirebilir ve [saas uygulamalarına](../saas-apps/tutorial-list.md) veya [SCIM ile entegre uygulamalara](../app-provisioning/use-scim-to-provision-users-and-groups.md)kullanıcı sağlama ile birleştiğinde.  Kuruluşlar, konuk [kullanıcıların şirket içi uygulamalara](../b2b/hybrid-cloud-to-on-premises.md)erişimi nin ne olduğunu da denetleyebilir.  Bu erişim hakları daha sonra yinelenen [Azure AD erişim incelemeleri](access-reviews-overview.md)kullanılarak düzenli olarak gözden geçirilebilir.   [Azure AD yetkilendirme yönetimi,](entitlement-management-overview.md) kullanıcıların grup ve ekip üyelikleri paketleri, uygulama rolleri ve SharePoint Online rolleri arasında nasıl erişim istediğini de tanımlamanıza olanak tanır.

Bir kullanıcı uygulamalara erişmeye çalıştığında, Azure AD [Koşullu Erişim](/azure/active-directory/conditional-access/) ilkeleri uygular. Örneğin, Koşullu Erişim ilkeleri, bir [kullanım koşullarını](../conditional-access/terms-of-use.md) görüntülemeyi ve [kullanıcının](../conditional-access/require-tou.md) bir uygulamaya erişemeden önce bu koşulları kabul etmesini sağlamayı içerebilir.

## <a name="privileged-access-lifecycle"></a>Ayrıcalıklı erişim yaşam döngüsü

Tarihsel olarak, ayrıcalıklı erişim diğer satıcılar tarafından Kimlik Yönetimi ayrı bir yetenek olarak tanımlanmıştır. Ancak Microsoft'ta, ayrıcalıklı erişimin yönetilmesinin Kimlik Yönetimi'nin önemli bir parçası olduğunu düşünüyoruz -- özellikle bu yönetici haklarıyla ilişkili kötüye kullanım potansiyeli bir kuruluşa neden olabilir. İdari hakları üstlenen çalışanların, satıcıların ve yüklenicilerin yönetilmesi gerekir.

![Ayrıcalıklı erişim yaşam döngüsü](./media/identity-governance-overview/privileged-access-lifecycle.png)

[Azure AD Ayrıcalıklı Kimlik Yönetimi (PIM),](../privileged-identity-management/pim-configure.md) Azure AD, Azure ve diğer Microsoft Çevrimiçi Hizmetleri'nde kaynakların erişim haklarını güvence altına almaya özel ek denetimler sağlar.  Azure AD PIM tarafından sağlanan tam zamanında erişim ve rol değişikliği uyarı özelliklerinin yanı sıra çok faktörlü kimlik doğrulama ve Koşullu Erişim, şirketinizin kaynaklarını güvence altına almaya yardımcı olmak için kapsamlı bir yönetim denetimi kümesi sağlar (dizin, Office 365 ve Azure kaynak rolleri). Diğer erişim biçimlerinde olduğu gibi, kuruluşlar yönetici rollerindeki tüm kullanıcılar için yinelenen erişim yeniden sertifikasını yapılandırmak için erişim incelemelerini kullanabilir.

## <a name="getting-started"></a>Başlarken

Yetkilendirme yönetimi, erişim değerlendirmeleri, Ayrıcalıklı Kimlik Yönetimi ve Kullanım Koşulları'nı kullanmaya başlamak için Azure portalında **Kimlik Yönetimi'nin** Başlangıç sekmesine göz atın.

![Kimlik Yönetimi Başlıyor](./media/identity-governance-overview/getting-started.png)


Kimlik Yönetimi özellikleri hakkında herhangi bir geri bildiriminiz varsa, geri bildiriminizi göndermek için Azure portalından **Geri Bildirim Aldı'ı** tıklatın. Ekip düzenli olarak geri bildiriminizi gözden geçirir.

Her müşteri için mükemmel bir çözüm veya öneri bulunmamakla birlikte, aşağıdaki yapılandırma kılavuzları, Microsoft'un daha güvenli ve üretken bir iş gücü sağlamak için izlemenizi önerdiği temel ilkeleri de sağlar.

- [Kimlik ve cihaz erişim yapılandırmaları](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Ayrıcalıklı erişimin güvenliğini sağlama](../users-groups-roles/directory-admin-roles-secure.md)

## <a name="appendix---least-privileged-roles-for-managing-in-identity-governance-features"></a>Ek - Kimlik Yönetişimözellikleri yönetmek için en az ayrıcalıklı roller

Kimlik Yönetimi'nde idari görevleri gerçekleştirmek için en az ayrıcalıklı rolü kullanmak en iyi yöntemdir. Bu görevleri gerçekleştirmek için gereken rolü etkinleştirmek için Azure AD PIM'yi kullanmanızı öneririz. Kimlik Yönetimi özelliklerini yapılandırmak için en az ayrıcalıklı dizin rolleri şunlardır:

| Özellik | En az ayrıcalıklı rol |
| ------- | --------------------- |
| Yetkilendirme yönetimi | Kullanıcı yöneticisi (Global yönetici gerektiren kataloglara SharePoint Online siteleri eklemek dışında) |
| Erişim gözden geçirmeleri | Kullanıcı yöneticisi (Ayrıcalıklı rol yöneticisi gerektiren Azure veya Azure AD rollerinin erişim incelemeleri hariç) |
|Privileged Identity Management | Ayrıcalıklı rol yöneticisi |
| Kullanım koşulları | Güvenlik yöneticisi veya Koşullu erişim yöneticisi |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD yetkilendirme yönetimi nedir?](entitlement-management-overview.md)
- [Azure AD erişim değerlendirmeleri nelerdir?](access-reviews-overview.md)
- [Azure AD Privileged Identity Management nedir?](../privileged-identity-management/pim-configure.md)
- [Kullanım koşulları ile ne yapabilirim?](active-directory-tou.md)


