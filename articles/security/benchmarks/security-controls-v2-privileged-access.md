---
title: Azure Güvenlik kıyaslaması v2-ayrıcalıklı erişim
description: Azure Güvenlik kıyaslama v2 ayrıcalıklı erişim
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 8727cbd07fad1960f4bdb33742729b6bda3a369e
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059436"
---
# <a name="security-control-privileged-access"></a>Güvenlik denetimi: ayrıcalıklı erişim

Ayrıcalıklı erişim, Azure kiracınıza ve kaynaklarınıza ayrıcalıklı erişimi korumaya yönelik denetimleri ele alır. Bu, yönetim modelinizi, yönetim hesaplarınızı ve ayrıcalıklı erişim İş istasyonlarınızı bilinçli ve yanlışlıkla riske karşı korumak için bir denetim aralığı içerir.

## <a name="pa-1-protect-and-limit-the-global-administrators"></a>PA-1: genel yöneticileri koruma ve sınırlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-1 | 4,3, 4,8 | AC-2 |

Genel yönetici rolüne atanan kullanıcılar, Azure AD kuruluşunuzda her yönetim ayarını okuyabilir ve değiştirebilir. Azure genel yönetici hesaplarınızın sayısını her abonelik için ikiden fazla olmayacak şekilde sınırlayın. Azure AD 'deki en kritik yerleşik roller, genel yöneticisidir ve ayrıcalıklı rol yöneticisidir ve bu iki role atanmış kullanıcılar yönetici rollerini temsil edebilir:
- Genel yönetici/Şirket Yöneticisi: Bu role sahip olan kullanıcılar, Azure AD 'deki tüm yönetim özelliklerine, ayrıca Azure AD kimliklerini kullanan hizmetlere erişebilir.

- Ayrıcalıklı rol yöneticisi: Bu role sahip olan kullanıcılar, Azure AD 'de rol atamalarını ve Azure AD Privileged Identity Management (PıM) içinde yönetebilir. Ayrıca, bu rol, PıM ve yönetim birimlerinin tüm yönlerinin yönetimine izin verir.

Note: özel rolleri atanmış belirli ayrıcalıklı izinlerle kullanıyorsanız, yönetilmelidir başka kritik rolleriniz olabilir. Ayrıca, önemli iş varlıklarının yönetici hesabına de benzer denetimler uygulamak isteyebilirsiniz.  

Azure kaynaklarına ve Azure AD 'ye Azure AD Privileged Identity Management (PıM) kullanarak tam zamanında (JıT) ayrıcalıklı erişim sağlayabilirsiniz. JıT, ayrıcalıklı görevleri yalnızca kullanıcılara ihtiyaç duyduklarından gerçekleştirmek için geçici izinler verir. PıM Ayrıca, Azure AD kuruluşunuzda şüpheli veya güvenli olmayan bir etkinlik olduğunda güvenlik uyarıları oluşturabilir.

Note: Azure SQL gibi bazı Azure Hizmetleri, Azure AD kimlik doğrulamasına ek olarak yerel kullanıcı kimlik doğrulamasını destekler. Bu tür yerel kullanıcı kimlik doğrulaması Azure AD aracılığıyla yönetilmez. Bu kullanıcıları ayrı ayrı yönetmeniz gerekir.

- [Azure AD 'de yönetici rolü izinleri](../../active-directory/users-groups-roles/directory-assign-admin-roles.md)

- [Özel Azure rolleri](../../role-based-access-control/custom-roles.md)

- [Azure Privileged Identity Management güvenlik uyarılarını kullanma](../../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Kimliği ve erişimi izlemek için Azure Güvenlik Merkezi 'ni kullanma](../../security-center/security-center-identity-access.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="pa-2-review-and-reconcile-user-access-regularly"></a>PA-2: Kullanıcı erişimini düzenli olarak gözden geçirme ve mutabık kılma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-2 | 4,1, 16,9, 16,10 | AC-2 |

Kullanıcı hesaplarının ve erişiminin geçerli olduğundan emin olmak için Kullanıcı hesaplarını ve erişim yetkilendirmelerini düzenli olarak gözden geçirin. 

Grup üyeliklerini yönetmek, kurumsal uygulamalara erişmek ve rol atamalarına ulaşmak için Azure AD kimlik ve erişim gözden geçirmeleri kullanın. Azure AD raporlama, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlayabilir. Gözden geçirme sürecini kolaylaştırmak için, erişim gözden geçirme raporu iş akışı oluşturmak için Azure AD Privileged Identity Management de kullanabilirsiniz.

Azure hizmeti ve iş yükü düzeyindeki yönetici kullanıcılar için, daha sık bir Kullanıcı ve erişim incelemesi gerçekleştirilmelidir. Ayrıca, yönetici hesaplarının ne zaman oluşturulduğunu algılamak ve eski veya yanlış yapılandırılmış yönetici hesaplarını bulmak için Azure Privileged Identity Management güvenlik uyarılarını da kullanabilirsiniz. 

Not: Azure SQL gibi bazı Azure Hizmetleri, Azure AD aracılığıyla yönetilmeyen yerel kullanıcıları destekler. Bu kullanıcıları ayrı ayrı yönetmeniz gerekir.

- [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

- [Kimliği ve erişimi izlemek için Azure Güvenlik Merkezi 'ni kullanma](../../security-center/security-center-identity-access.md)

- [Azure Privileged Identity Management güvenlik uyarılarını kullanma](../../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md)

- [Ekipler genelinde yönetim sorumluluklarını hizalayın](/azure/cloud-adoption-framework/organize/raci-alignment) 

- [Azure AD raporlamayı anlama](/azure/active-directory/reports-monitoring/)

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](../../active-directory/governance/access-reviews-overview.md)

- [Privileged Identity Management-Azure AD rollerine erişimi gözden geçirme](../../active-directory/privileged-identity-management/pim-how-to-start-security-review.md)

- [Azure Güvenlik Merkezi-kimliği ve erişimi Izleme](../../security-center/security-center-identity-access.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik uyumluluğu yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="pa-3-set-up-an-emergency-access-account-in-azure-ad"></a>PA-3: Azure AD 'de bir acil durum erişim hesabı ayarlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-3 | 12,3 | AC-2 |

Yanlışlıkla Azure AD kuruluşunuzun dışına kilitlenmesini engellemek için, normal yönetim hesapları kullanılmazsa erişim için bir acil durum erişim hesabı ayarlayın. Acil durum erişim hesapları genellikle yüksek ayrıcalıklı olur ve belirli kişilere atanmamalıdır. Acil durum erişim hesapları, normal yönetim hesaplarının kullanılabileceği acil durum veya "cam camı" senaryolarıyla sınırlıdır.

Acil durum erişim hesapları için kimlik bilgilerinin (parola, sertifika veya akıllı kart gibi) güvenli tutulduğundan ve yalnızca bir acil durumda bunları kullanma yetkisine sahip olan bireyler için bilindiğinden emin olmanız gerekir.

- [Azure AD 'de acil durum erişim hesaplarını yönetme](../../active-directory/users-groups-roles/directory-emergency-access.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik uyumluluğu yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Güvenlik işlemleri merkezi (SOC)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

## <a name="pa-4-automate-azure-identity-and-access-request-workflow"></a>PA-4: Azure kimlik ve erişim isteği iş akışını otomatikleştirin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-4 | Yok | AC-2, AC-5, PM-10 |

Erişim atamaları, incelemeler ve süre sonu dahil olmak üzere Azure erişim isteği iş akışlarını otomatikleştirmek için Azure AD yetkilendirme yönetimi özelliklerini kullanın. Çift veya çok aşamalı onay de desteklenir.  

- [Azure AD yetkilendirme yönetimi nedir?](../../active-directory/governance/entitlement-management-overview.md)

- [Erişim isteği ve onay işlemini ayarlama](../../active-directory/governance/entitlement-management-access-package-request-policy.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik uyumluluğu yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

## <a name="pa-5-use-highly-secured-machines-for-administrative-tasks"></a>PA-5: yönetim görevleri için yüksek güvenlikli makineler kullanma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-5 | 4,6, 11,6, 12,12 | AC-2, SC-7 |

Güvenli, yalıtılmış iş istasyonları Yöneticiler, geliştiriciler ve kritik hizmet işleçleri gibi hassas rollerin güvenliği açısından kritik öneme sahiptir. Yönetim görevleri için yüksek düzeyde güvenli Kullanıcı iş istasyonları ve/veya Azure savunma kullanın:
- Yönetim görevleri için güvenli ve yönetilen bir kullanıcı iş istasyonu dağıtmak üzere Azure Active Directory, Microsoft Defender Gelişmiş tehdit koruması (ATP) ve/veya Microsoft Intune kullanın. Güvenli iş istasyonları, güçlü kimlik doğrulaması, yazılım ve donanım temelleri, kısıtlı mantıksal ve ağ erişimi gibi güvenli yapılandırmayı zorlamak için merkezi olarak yönetilebilir. 

- Sanal makinelerinize RDP veya SSH aracılığıyla erişmek için güvenli bir yol için Azure savunma özelliğini kullanın. Azure savunma, sanal ağ başına yeni bir sanal ağ oluşturmadan temin edilebilir, tam olarak yönetilen bir PaaS hizmetidir. 

- [Güvenli, Azure tarafından yönetilen iş istasyonlarını anlayın](../../active-directory/devices/concept-azure-managed-workstation.md)

- [Güvenli, Azure tarafından yönetilen bir iş istasyonu dağıtma](../../active-directory/devices/howto-azure-managed-workstation.md)

- [Azure savunma konağını kullanma](../../bastion/bastion-create-host-portal.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik işlemleri merkezi (SOC)](/azure/cloud-adoption-framework/organize/cloud-security-operations-center)

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

## <a name="pa-6-assign-privileges-to-resources-using-azure-rbac"></a>PA-6: Azure RBAC kullanarak kaynaklara ayrıcalıklar atama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-6 | 14,6 | AC-2, AC-3 |

Azure rol tabanlı erişim denetimi (RBAC), rol atamaları aracılığıyla Azure kaynak erişimi ayrıcalıklarını yönetmenizi sağlar. Bu rolleri kullanıcılara atayabilir, hizmet sorumlularını ve yönetilen kimlikleri gruplara atayabilirsiniz. Belirli kaynaklar için önceden tanımlı yerleşik roller vardır ve bu roller Azure CLı, Azure PowerShell veya Azure portal gibi araçlarla envantere alınabilir veya sorgulanabilir. 

- [Azure rol tabanlı erişim denetimi nedir (Azure RBAC)](../../role-based-access-control/overview.md)

- [Azure 'da RBAC 'yi yapılandırma](../../role-based-access-control/role-assignments-portal.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik uyumluluğu yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

## <a name="pa-7-choose-approval-process-for-microsoft-support"></a>PA-7: Microsoft desteği için onay işlemini seçin

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| PA-7 | 16 | AC-2, AC-3, AC-4 |

Microsoft 'un müşteri verilerine erişmesi gereken destek senaryolarında, Müşteri Kasası her müşteri veri erişimi isteğini açık bir şekilde gözden geçirmeniz ve onaylamanız ya da reddetmeniz için bir yetenek sağlar.

- [Müşteri Kasası anlayın](../fundamentals/customer-lockbox-overview.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Güvenlik uyumluluğu yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-compliance-management)

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

