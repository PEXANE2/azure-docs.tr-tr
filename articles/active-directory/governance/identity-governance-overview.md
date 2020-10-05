---
title: Identity Idare-Azure Active Directory | Microsoft Docs
description: Azure Active Directory Identity Governance, doğru işlemlerle ve görünürlüğe sahip güvenlik ve çalışanların üretkenliğini sağlamak için kuruluşunuzun gereksinimini dengelemenize olanak tanır.
services: active-directory
documentationcenter: ''
author: ajburnle
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ecbebfc75cb8c77ebb99ad04b1f9e33b3c4ef64
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91306472"
---
# <a name="what-is-azure-ad-identity-governance"></a>Azure AD Identity Governance nedir?

Azure Active Directory (Azure AD) Identity Idare, kuruluşunuzun güvenlik ve çalışanların verimliliğini doğru işlemlerle ve görünürlüğe dengelemenize olanak tanır. Doğru kişilerin doğru kaynaklara doğru erişimi olduğundan emin olmak için size yetenekler sağlar. Bu ve ilgili Azure AD ve Enterprise Mobility + Security özellikleri, kritik varlıklara erişimi koruyarak, izleyerek ve denetleyerek, çalışan ve iş ortağı üretkenliğini sağlarken erişim riskini azaltmanıza olanak sağlar.  

Kimlik yönetimi, kuruluşlara çalışanlar, iş ortakları ve satıcılar arasında ve hem şirket içi hem de bulutlarda bulunan hizmetler ve uygulamalar arasında aşağıdaki görevleri yapma olanağı sağlar:

- Kimlik yaşam döngüsünü yönetir
- Erişim yaşam döngüsünü yönetir
- Yönetim için güvenli ayrıcalıklı erişim

Özellikle, kuruluşların bu dört önemli soruyu ele almak için tasarlanmıştır:

- Hangi kullanıcıların hangi kaynaklara erişimi olmalıdır?
- Bu erişimi hangi kullanıcılar yapıyor?
- Erişimi yönetmeye yönelik etkili bir kurumsal denetim var mı?
- Denetçilerin çalıştığını doğrulaması yapılabilir mi?

## <a name="identity-lifecycle"></a>Kimlik yaşam döngüsü

Kimlik yönetimi, kuruluşların *üretkenlik* arasında bir denge elde etmesine yardımcı olur ve kuruluşa ne zaman katılabilecekleri gibi bir kişinin ihtiyacı olan kaynaklara ne kadar hızlı bir şekilde erişmesini sağlayabilir? Ve *güvenlik* -bu kişinin iş durumundaki değişiklikler nedeniyle, zaman içinde erişimin ne zaman içinde değiştirilmesi gerekir?  Kimlik yaşam döngüsü yönetimi, kimlik yönetimi için temel ve ölçekteki etkili idare, uygulamalar için kimlik yaşam döngüsü yönetim altyapısını modernleştirmeyi gerektirir.

![Kimlik yaşam döngüsü](./media/identity-governance-overview/identity-lifecycle.png)

Birçok kuruluşta, çalışanlar için kimlik yaşam döngüsü, bu kullanıcının bir HCM (insan büyük yönetim) sisteminde temsiline bağlıdır.  Azure AD Premium, iş günü içinde temsil edilen kişiler için Kullanıcı kimliklerini otomatik olarak korur ve  [Kullanıcı sağlama planlama kılavuzu Azure Active Directory için bulut HR uygulamasında](../app-provisioning/plan-cloud-hr-provision.md)açıklandığı gibi hem Active Directory hem de Azure Active Directory.  Azure AD Premium ayrıca, SAP HCM, Oracle eBusiness ve Oracle PeopleSoft gibi şirket içi HCM sistemlerinden kayıtları içeri aktarabilen [Microsoft Identity Manager](/microsoft-identity-manager/)da içerir.

Giderek, senaryolar, kuruluşunuzun dışındaki kişilerle işbirliği gerektirir. [Azure AD B2B](/azure/active-directory/b2b/) işbirliği, kuruluşunuzun uygulamalarını ve hizmetlerini herhangi bir kuruluştan Konuk kullanıcılar ve dış iş ortakları ile güvenli bir şekilde paylaşmanıza olanak sağlarken kendi şirket verileriniz üzerinde denetim sağlar.  [Azure AD Yetkilendirme Yönetimi](entitlement-management-overview.md) , hangi kuruluşun kullanıcılarının erişim istemesine ve KURULUŞUNUZUN dizinine B2B konukları olarak eklenmesine izin verileceğini seçmenizi sağlar ve artık erişime gerek kalmadığında bu konukların kaldırılmasını sağlar.

## <a name="access-lifecycle"></a>Erişim yaşam döngüsü

Kuruluşların, Kullanıcı kimliğinin oluşturulduğu zaman bir kullanıcı için başlangıçta sağlananların ötesinde erişimi yönetmek için bir işlem yapması gerekir.  Ayrıca, kurumsal kuruluşların, erişim ilkesini ve denetimleri sürekli olarak geliştirip zorlayabilmeleri için verimli bir şekilde ölçeklendirilebilecek.

![Erişim yaşam döngüsü](./media/identity-governance-overview/access-lifecycle.png)

Genellikle, iş karar mekanizmalarının onay kararlarını devreder.  Ayrıca, kullanıcıların kendilerini de içerebilir.  Örneğin, Avrupa 'daki bir şirketin pazarlama uygulamasındaki gizli müşteri verilerine erişen kullanıcıların şirket ilkelerini bilmeleri gerekir. Konuk kullanıcılar, davet edildikleri bir kuruluştaki veriler için işleme gereksinimlerini farkında olabilir.

Kuruluşlar, [SIM ile tümleştirilmiş](../app-provisioning/use-scim-to-provision-users-and-groups.md) [SaaS uygulamalarına](../saas-apps/tutorial-list.md) veya uygulamalarına Kullanıcı sağlama ile bağlanmış [Dinamik Gruplar](../users-groups-roles/groups-dynamic-membership.md)gibi teknolojiler aracılığıyla erişim yaşam döngüsü işlemini otomatikleştirebilir.  Kuruluşlar ayrıca hangi [Konuk kullanıcıların şirket içi uygulamalara erişebileceğini](../external-identities/hybrid-cloud-to-on-premises.md)de denetleyebilir.  Bu erişim hakları daha sonra yinelenen [Azure AD erişim İncelemeleri](access-reviews-overview.md)kullanılarak düzenli olarak gözden geçirilebilir.   [Azure AD Yetkilendirme Yönetimi](entitlement-management-overview.md) , kullanıcıların grup ve takım üyelikleri, uygulama rolleri ve SharePoint Online rollerinin paketleri arasında nasıl erişim isteyeceğini tanımlamanızı da sağlar.

Bir Kullanıcı uygulamalara erişmeye çalıştığında, Azure AD [koşullu erişim](../conditional-access/index.yml) ilkeleri uygular. Örneğin, koşullu erişim ilkeleri bir [kullanım koşulları](../conditional-access/terms-of-use.md) görüntülemeyi ve kullanıcının bir uygulamaya erişebilmek üzere [Bu koşulları kabul](../conditional-access/require-tou.md) etmesinin mümkün olmasını içerebilir.

## <a name="privileged-access-lifecycle"></a>Ayrıcalıklı erişim yaşam döngüsü

Geçmişte, ayrıcalıklı erişim diğer satıcılar tarafından kimlik yönetimi 'nden ayrı bir özellik olarak açıklanmıştır. Ancak, Microsoft 'ta, ayrıcalıklı erişim 'in bir kimlik yönetimi 'nin önemli bir parçası olduğunu düşündük, özellikle de bu yönetici haklarıyla ilişkili kötüye kullanımı olasılığı kuruluşa neden olabilir. Yönetim haklarını kullanan çalışanların, satıcıların ve yüklenicilerin yönetilmelidir.

![Ayrıcalıklı erişim yaşam döngüsü](./media/identity-governance-overview/privileged-access-lifecycle.png)

[Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) , Azure AD, Azure ve diğer Microsoft çevrimiçi hizmetleri arasında kaynaklara yönelik erişim haklarının güvenliğini sağlamaya yönelik ek denetimler sağlar.  Azure AD PıM tarafından sağlanan tam zamanında erişim ve rol değişikliği uyarısı özellikleri, Multi-Factor Authentication ve koşullu erişim 'in yanı sıra, şirketinizin kaynaklarını (Dizin, Microsoft 365 ve Azure Kaynak rolleri) güvenli hale getirmeye yardımcı olmak için kapsamlı bir idare denetimleri kümesi sağlar. Diğer erişim formlarında olduğu gibi, kuruluşlar yönetici rollerindeki tüm kullanıcılar için yinelenen erişim yeniden sertifika yapılandırmak üzere erişim gözden geçirmeleri kullanabilir.

## <a name="governance-capabilities-in-other-azure-ad-features"></a>Diğer Azure AD özelliklerinde idare özellikleri

Yukarıda listelenen özelliklere ek olarak, kimlik idare senaryolarını sağlamak için sık kullanılan ek Azure AD özellikleri şunlardır:

| Özellik | Senaryo |Özellik
| ------- | --------------------- |-----|
|Kimlik yaşam döngüsü (çalışanlar)|Yöneticiler, Workday 'den Kullanıcı hesabı sağlamayı veya bulut HR ya da şirket içi HR 'yi başarılı bir şekilde etkinleştirebilir.|[bulut-Azure AD Kullanıcı sağlama](../app-provisioning/plan-cloud-hr-provision.md)|
|Kimlik yaşam döngüsü (konuklar)|Yöneticiler, başka bir Azure AD kiracısı, doğrudan Federasyon, bir saat geçiş kodu (OTP) veya Google hesaplarından self servis Konuk Kullanıcı ekleme özelliğini etkinleştirebilir.  Konuk kullanıcılar otomatik olarak sağlanır ve yaşam döngüsü ilkelerine tabidir.|[B2B](../external-identities/what-is-b2b.md) kullanarak [Yetkilendirme Yönetimi](entitlement-management-overview.md)|
|Yetkilendirme yönetimi|Kaynak sahipleri; uygulamalar, takımlar, Azure AD ve Microsoft 365 grupları ve SharePoint Online siteleri içeren erişim paketleri oluşturabilir.|[Yetkilendirme yönetimi](entitlement-management-overview.md)|
|Erişim istekleri|Son kullanıcılar, Grup üyeliği veya uygulama erişimi isteyebilir. Diğer kuruluşların konukları da dahil olmak üzere son kullanıcılar, erişim paketlerine erişim isteğinde bulunabilir.|[Yetkilendirme yönetimi](entitlement-management-overview.md)|
|İş akışı|Kaynak sahipleri, rol etkinleştirme istekleri için erişim istekleri ve onaylayanlara yönelik onaylayanları ve yükseltme onaylayanlarını tanımlayabilir.  |[Yetkilendirme Yönetimi](entitlement-management-overview.md) ve [PIM](../privileged-identity-management/pim-configure.md)|
|İlke ve rol yönetimi|Yönetici, uygulamalara yönelik çalışma zamanı erişimi için koşullu erişim ilkeleri tanımlayabilir.  Kaynak sahipleri, kullanıcıların erişim paketleri aracılığıyla erişim ilkelerini tanımlayabilir.|[Koşullu erişim](../conditional-access/overview.md) ve [Yetkilendirme Yönetimi](entitlement-management-overview.md) ilkeleri|
|Erişim sertifikası|Yöneticiler: SaaS uygulamaları veya bulut grubu üyelikleri, Azure AD veya Azure Kaynak rolü atamaları için yinelenen erişim yeniden sertifikasyon etkinleştirebilir. Kaynak erişimini otomatik olarak kaldır, konuk erişimini engelle ve konuk hesaplarını Sil.|[Erişim gözden geçirmeleri](access-reviews-overview.md), [PIM](../privileged-identity-management/pim-how-to-start-security-review.md) 'de de ortaya çıkmış|
|Karşılama ve sağlama|SCıM ve SharePoint Online sitelerine dahil olmak üzere Azure AD bağlı uygulamalarına otomatik sağlama ve sağlamayı kaldırma. |[Kullanıcı hazırlama](../app-provisioning/user-provisioning.md)|
|Raporlama ve analiz|Yöneticiler, Son Kullanıcı sağlama ve oturum açma etkinliğinin denetim günlüklerini alabilir. Azure Izleyici ve erişim paketleri aracılığıyla ' erişime sahip ' ile tümleştirme.|[Azure AD raporları](../reports-monitoring/overview-reports.md) ve [izleme](../reports-monitoring/overview-monitoring.md)|
|Ayrıcalıklı erişim|Azure AD rolleri (özel roller dahil) ve Azure Kaynak rolleri için tam zamanında ve zamanlanmış erişim, uyarı, onay iş akışları.|[Azure AD PıM](../privileged-identity-management/pim-configure.md)|
|Denetim|Yöneticiler, yönetici hesaplarının oluşturulması hakkında uyarı verebilir.|[Azure AD PıM uyarıları](../privileged-identity-management/pim-how-to-configure-security-alerts.md)|

## <a name="getting-started"></a>Başlarken

Yetkilendirme Yönetimi, erişim incelemeleri, Privileged Identity Management ve Kullanım koşulları kullanmaya başlamak için Azure portal **kimlik** yönetimi 'nin Başlarken sekmesine göz atın.

![Identity Idare Başlarken](./media/identity-governance-overview/getting-started.png)


Kimlik yönetimi özellikleriyle ilgili geri bildiriminiz varsa, görüşlerinizi göndermek için Azure portal **geri bildirimde bulunun?** öğesine tıklayın. Ekip geri bildirimlerinizi düzenli olarak inceler.

Her müşteri için kusursuz bir çözüm veya öneri olmasa da, aşağıdaki yapılandırma kılavuzlarında, Microsoft 'un daha güvenli ve üretken bir iş gücünün sağlanması için izlemeniz önerilen temel ilkeler de sağlanmaktadır.

- [Kimlik ve cihaz erişim yapılandırmaları](/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Ayrıcalıklı erişimin güvenliğini sağlama](../users-groups-roles/directory-admin-roles-secure.md)

## <a name="appendix---least-privileged-roles-for-managing-in-identity-governance-features"></a>Ek-kimlik Idare özelliklerinde yönetmek için en az ayrıcalıklı roller

Kimlik yönetimi 'nde Yönetim görevlerini gerçekleştirmek için en az ayrıcalıklı rolü kullanmak en iyi uygulamadır. Bu görevleri gerçekleştirmek için gerektiğinde bir rolü etkinleştirmek üzere Azure AD PıM kullanmanızı öneririz. Aşağıda, kimlik Idare özelliklerini yapılandırmak için en az ayrıcalıklı dizin rolü verilmiştir:

| Özellik | En az ayrıcalıklı rol |
| ------- | --------------------- |
| Yetkilendirme yönetimi | Kullanıcı Yöneticisi (genel yönetici gerektiren kataloglara SharePoint Online siteleri ekleme hariç) |
| Erişim gözden geçirmeleri | Kullanıcı Yöneticisi (ayrıcalıklı rol yöneticisi gerektiren Azure veya Azure AD rolleri için erişim gözden geçirmeleri hariç) |
|Privileged Identity Management | Ayrıcalıklı rol yöneticisi |
| Kullanım koşulları | Güvenlik Yöneticisi veya koşullu erişim Yöneticisi |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD yetkilendirme yönetimi nedir?](entitlement-management-overview.md)
- [Azure AD erişim gözden geçirmeleri nelerdir?](access-reviews-overview.md)
- [Azure AD Privileged Identity Management nedir?](../privileged-identity-management/pim-configure.md)
- [Kullanım koşulları ile ne yapabilirim?](../conditional-access/terms-of-use.md)