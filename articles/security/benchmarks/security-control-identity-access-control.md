---
title: Azure Güvenlik denetimi-kimlik ve Access Control
description: Azure güvenlik denetim kimliği ve Access Control
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 473fe838f53df0a874a2c793792533e01b7c069a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81408525"
---
# <a name="security-control-identity-and-access-control"></a>Güvenlik denetimi: kimlik ve Access Control

Kimlik ve erişim yönetimi önerileri, kimlik tabanlı erişim denetimiyle ilgili sorunları gidermeye, yönetim erişimini kilitlemeyi, kimlik ile ilgili olayları, olağan dışı hesap davranışından ve rol tabanlı erişim denetimine karşı uyarma konusuna odaklanmaktadır.

## <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3.1 | 4.1 | Müşteri |

Azure AD 'nin açıkça atanması ve sorgulanabilir olması gereken yerleşik rolleri vardır. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

- [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

## <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3,2 | 4.2 | Müşteri |

Azure AD varsayılan parola kavramına sahip değildir. Parola gerektiren diğer Azure kaynakları, karmaşıklık gereksinimleriyle ve hizmete bağlı olarak, en düşük parola uzunluğuna sahip bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanbilen üçüncü taraf uygulamalardan ve Market hizmetlerinden siz sorumlusunuz.

## <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3.3 | 4.3 | Müşteri |

Adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, Microsoft Hizmetleri için Azure AD Privileged Identity Management ayrıcalıklı rolleri kullanarak tam zamanında/tam erişimi etkinleştirebilirsiniz ve Azure Resource Manager. 

- [Privileged Identity Management hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/)

## <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3.4 | 4.4 | Müşteri |

Mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerine Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini kullanın.

- [Azure AD ile SSO 'yu anlama](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3,5 | 4,5, 11,5, 12,11, 16,3 | Müşteri |

Azure AD MFA 'yı etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

- [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

## <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3,6 | 4,6, 11,6, 12,12 | Müşteri |

Azure kaynaklarını açmak ve yapılandırmak için MFA ile Paw 'lar (ayrıcalıklı erişim iş istasyonları) kullanın.

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Azure 'da MFA 'yı etkinleştirme](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

## <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3.7 | 4,8, 4,9 | Müşteri |

Ortamda şüpheli veya güvenli olmayan bir etkinlik gerçekleştiğinde Günlükler ve uyarılar oluşturmak için Azure Active Directory güvenlik raporları kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

- [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

## <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3,8 | 11,7 | Müşteri |

IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için adlandırılmış konumlarda koşullu erişim kullanın.

- [Azure 'da adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

## <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3.9 | 16,1, 16,2, 16,4, 16,5, 16,6 | Müşteri |

Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (AAD) kullanın. AAD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. AAD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

- [AAD örneği oluşturma ve yapılandırma](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

## <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3,10 | 16,9, 16,10 | Müşteri |

Azure AD, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir. 

- [Azure AD raporlamayı anlama](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

- [Azure kimlik erişimi Incelemelerini kullanma](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

## <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3,11 | 16,12 | Müşteri |

Azure AD oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişiminiz vardır ve bu, herhangi bir SıEM/Izleme aracıyla tümleştirmenize imkan tanır.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz.

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

## <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3,12 | 16,13 | Müşteri |

Azure AD risk ve kimlik koruması özelliklerini kullanarak, Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

- [Azure AD riskli oturum açma işlemlerini görüntüleme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

- [Azure Sentinel 'i ekleme](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

## <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

| Azure KIMLIĞI | CIS kimlikleri | Ğuna |
|--|--|--|
| 3,13 | 16 | Müşteri |

Microsoft 'un müşteri verilerine erişmesi gereken destek senaryolarında, Müşteri Kasası müşteri verileri erişim isteklerini gözden geçirmeniz ve onaylayabilmeniz ya da reddetmeniz için bir arabirim sağlar.

- [Müşteri Kasası anlayın](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview)


## <a name="next-steps"></a>Sonraki adımlar

- Sonraki güvenlik denetimine bakın: [veri koruma](security-control-data-protection.md)