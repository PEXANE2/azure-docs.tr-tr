---
title: Azure Güvenlik kıyaslaması v2-ayrıcalıklı erişim
description: Azure Güvenlik kıyaslama v2 ayrıcalıklı erişim
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/20/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: b5315b4d2ec8b757f7fa1075a438419679f5e798
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91317982"
---
# <a name="security-control-v2-privileged-access"></a>Güvenlik denetimi v2: ayrıcalıklı erişim

Ayrıcalıklı erişim, Azure kiracınıza ve kaynaklarınıza ayrıcalıklı erişimi korumaya yönelik denetimleri ele alır. Bu, yönetim modelinizi, yönetim hesaplarınızı ve ayrıcalıklı erişim İş istasyonlarınızı bilinçli ve yanlışlıkla riske karşı korumak için bir denetim aralığı içerir.

## <a name="pa-1-protect-and-limit-highly-privileged-users"></a>PA-1: yüksek ayrıcalıklı kullanıcıları koruyun ve sınırlayın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-1 | 4,3, 4,8 | AC-2 |

Yüksek ayrıcalıklı kullanıcı hesabı sayısını sınırlayın ve bu hesapları yükseltilmiş bir düzeyde koruyun. Azure AD 'deki en kritik yerleşik roller, genel yönetici ve ayrıcalıklı rol yöneticisidir çünkü bu iki role atanan kullanıcılar yönetici rollerini temsil edebilir. Bu ayrıcalıklarla, kullanıcılar Azure ortamınızdaki her kaynağı doğrudan veya dolaylı olarak okuyabilir ve değiştirebilir:

- Genel yönetici/Şirket Yöneticisi: Bu role sahip olan kullanıcılar, Azure AD 'deki tüm yönetim özelliklerine, ayrıca Azure AD kimliklerini kullanan hizmetlere erişebilir.

- Ayrıcalıklı rol yöneticisi: Bu role sahip olan kullanıcılar, Azure AD 'de rol atamalarını ve Azure AD Privileged Identity Management (PıM) içinde yönetebilir. Ayrıca, bu rol, PıM ve yönetim birimlerinin tüm yönlerinin yönetimine izin verir.

Note: özel rolleri atanmış belirli ayrıcalıklı izinlerle kullanıyorsanız, yönetilmelidir başka kritik rolleriniz olabilir. Ayrıca, önemli iş varlıklarının yönetici hesabına de benzer denetimler uygulamak isteyebilirsiniz.  

Azure kaynaklarına ve Azure AD 'ye Azure AD Privileged Identity Management (PıM) kullanarak tam zamanında (JıT) ayrıcalıklı erişim sağlayabilirsiniz. JıT, ayrıcalıklı görevleri yalnızca kullanıcılara ihtiyaç duyduklarından gerçekleştirmek için geçici izinler verir. PıM Ayrıca, Azure AD kuruluşunuzda şüpheli veya güvenli olmayan bir etkinlik olduğunda güvenlik uyarıları oluşturabilir.

- [Azure AD 'de yönetici rolü izinleri](../../active-directory/users-groups-roles/directory-assign-admin-roles.md)

- [Azure Privileged Identity Management güvenlik uyarılarını kullanma](../../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](../../active-directory/users-groups-roles/directory-admin-roles-secure.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Kimlik ve anahtar yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Güvenlik Işlemleri](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

## <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: iş açısından kritik sistemlere yönetici erişimini kısıtlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-2 | 13,2, 2,10 | AC-2, SC-3, SC-7 |

İş açısından kritik sistemlere erişimi, hangi hesaplara, içinde bulundukları aboneliklere ve yönetim gruplarına ayrıcalıklı erişim verildiğini kısıtlayarak ayırın. Active Directory Etki Alanı Denetleyicileri (DC), güvenlik araçları ve iş açısından kritik sistemlerde yüklü aracılarla sistem yönetimi araçları gibi iş açısından kritik varlıklarınıza yönetici erişimi olan yönetim, kimlik ve güvenlik sistemlerine erişimi de kısıtlatığınızdan emin olun. Bu yönetim ve güvenlik sistemlerine zarar veren saldırganlar, iş açısından kritik varlıkların güvenliğini tehlikeye atabilir. 

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır. 

E-posta, göz atma ve üretkenlik görevlerinde kullanılan standart kullanıcı hesaplarından ayrı ayrıcalıklı ayrıcalıklı hesaplar atadığınızdan emin olun.

- [Azure bileşenleri ve başvuru modeli](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Yönetim grubu erişimi](../../governance/management-groups/overview.md#management-group-access)

- [Azure abonelik yöneticileri](../../cost-management-billing/manage/add-change-subscription-administrator.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Kimlik ve anahtar yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

## <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Kullanıcı erişimini düzenli olarak gözden geçirme ve mutabık kılma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-3 | 4,1, 16,9, 16,10 | AC-2 |

Hesapların ve erişim düzeyinin geçerli olduğundan emin olmak için Kullanıcı hesaplarını ve erişim atamasını düzenli olarak gözden geçirin. Azure AD erişim incelemelerini, grup üyeliklerini ve kurumsal uygulamalara erişimi ve rol atamalarını gözden geçirmek için kullanabilirsiniz. Azure AD raporlama, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlayabilir. Ayrıca, gözden geçirme sürecini kolaylaştıran bir erişim gözden geçirme raporu iş akışı oluşturmak için Azure AD Privileged Identity Management de kullanabilirsiniz.
Ayrıca, Azure Privileged Identity Management, çok fazla sayıda yönetici hesabı oluşturulduğunda ve eski veya yanlış yapılandırılmış yönetici hesaplarını belirlemek için de kullanılabilir. 

Note: bazı Azure Hizmetleri, Azure AD aracılığıyla yönetilmeyen yerel kullanıcıları ve rolleri destekler. Bu kullanıcıları ayrı ayrı yönetmeniz gerekir.

- [Privileged Identity Management (PıM) içinde Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma](../../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md)

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](../../active-directory/governance/access-reviews-overview.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Kimlik ve anahtar yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Azure AD 'de acil durum erişimi ayarlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-4 | 16 | AC-2, CP-2 |

Yanlışlıkla Azure AD kuruluşunuzun dışına kilitlenmesini engellemek için, normal yönetim hesapları kullanılmazsa erişim için bir acil durum erişim hesabı ayarlayın. Acil durum erişim hesapları genellikle yüksek ayrıcalıklı olur ve belirli kişilere atanmamalıdır. Acil durum erişim hesapları, normal yönetim hesaplarının kullanılabileceği acil durum veya "cam camı" senaryolarıyla sınırlıdır.
Acil durum erişim hesapları için kimlik bilgilerinin (parola, sertifika veya akıllı kart gibi) güvenli tutulduğundan ve yalnızca bir acil durumda bunları kullanma yetkisine sahip olan bireyler için bilindiğinden emin olmanız gerekir.

- [Azure AD 'de acil durum erişim hesaplarını yönetme](../../active-directory/users-groups-roles/directory-emergency-access.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Kimlik ve anahtar yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Güvenlik Işlemleri (SecOps)](//azure/cloud-adoption-framework/organize/cloud-security-operations-center)

## <a name="pa-5-automate-entitlement-management"></a>PA-5: yetkilendirme yönetimini otomatikleştirme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-5 | 16 | AC-2, AC-5, PM-10 |

Erişim atamaları, incelemeler ve süre sonu dahil olmak üzere, erişim isteği iş akışlarını otomatikleştirmek için Azure AD yetkilendirme yönetimi özelliklerini kullanın. Çift veya çok aşamalı onay de desteklenir.
- [Azure AD erişim gözden geçirmeleri nelerdir?](../../active-directory/governance/access-reviews-overview.md) 

- [Azure AD yetkilendirme yönetimi nedir?](../../active-directory/governance/entitlement-management-overview.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Kimlik ve anahtar yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="pa-6-use-privileged-access-workstations"></a>PA-6: ayrıcalıklı erişim iş istasyonlarını kullanma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-6 | 4,6, 11,6, 12,12 | AC-2, SC-3, SC-7 |

Güvenli, yalıtılmış iş istasyonları Yöneticiler, geliştiriciler ve kritik hizmet işleçleri gibi hassas rollerin güvenliği açısından kritik öneme sahiptir. Yönetim görevleri için yüksek düzeyde güvenli Kullanıcı iş istasyonları ve/veya Azure savunma kullanın. Yönetim görevleri için güvenli ve yönetilen bir kullanıcı iş istasyonu dağıtmak üzere Azure Active Directory, Microsoft Defender Gelişmiş tehdit koruması (ATP) ve/veya Microsoft Intune kullanın. Güvenli iş istasyonları, güçlü kimlik doğrulaması, yazılım ve donanım temelleri ve kısıtlı mantıksal ve ağ erişimi gibi güvenli yapılandırmayı zorlamak için merkezi olarak yönetilebilir. 

- [Ayrıcalıklı erişim iş istasyonlarını anlama](../../active-directory/devices/concept-azure-managed-workstation.md)

- [Ayrıcalıklı erişim iş istasyonu dağıtma](../../active-directory/devices/howto-azure-managed-workstation.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik Işlemleri (SecOps)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Kimlik ve anahtar yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

## <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: yalnızca yeterli yönetim (en az ayrıcalık ilkesi) Izleyin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-7 | 14,6 | AC-2, AC-3, SC-3 |

Azure rol tabanlı erişim denetimi (Azure RBAC), Azure kaynak erişimini rol atamaları aracılığıyla yönetmenizi sağlar. Kullanıcılara, Grup hizmeti sorumlularına ve yönetilen kimliklere bu rolleri atayabilirsiniz. Belirli kaynaklar için önceden tanımlı yerleşik roller vardır ve bu roller Azure CLı, Azure PowerShell ve Azure portal gibi araçlarla envantere alınabilir veya sorgulanabilir. Azure RBAC aracılığıyla kaynaklara atadığınız ayrıcalıklar, her zaman roller için gerekdiklere göre sınırlandırılmalıdır. Sınırlı ayrıcalıklar Azure AD Privileged Identity Management (PıM) tam zamanında (JıT) yaklaşımını tamamlar ve bu ayrıcalıkların düzenli olarak gözden geçirilmesi gerekir.
İzin ayırmak ve yalnızca gerektiğinde özel rol oluşturmak için yerleşik rolleri kullanın. 

- [Azure rol tabanlı erişim denetimi nedir (Azure RBAC)](../../role-based-access-control/overview.md)

- [Azure RBAC 'yi yapılandırma](../../role-based-access-control/role-assignments-portal.md)

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](../../active-directory/governance/access-reviews-overview.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management) 

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)    

- [Kimlik ve anahtar yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

## <a name="pa-8-choose-approval-process-for-microsoft-support"></a>PA-8: Microsoft desteği için onay işlemini seçin 

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-8 | 16 | AC-2, AC-3, AC-4 |

Microsoft 'un müşteri verilerine erişmesi gereken destek senaryolarında, Müşteri Kasası her müşteri veri erişimi isteğini açık bir şekilde gözden geçirmeniz ve onaylamanız ya da reddetmeniz için bir yetenek sağlar.

- [Müşteri Kasası anlayın](../fundamentals/customer-lockbox-overview.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik paydaşları** ([daha fazla bilgi](/azure/cloud-adoption-framework/organize/cloud-security#security-functions)):

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik Uyumluluk Yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management) 

- [Kimlik ve anahtar yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

