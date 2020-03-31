---
title: Azure Güvenlik Kontrolü - Kimlik ve Erişim Denetimi
description: Güvenlik Kontrolü Kimlik ve Erişim Kontrolü
author: msmbaldwin
manager: rkarlin
ms.service: security
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: mbaldwin
ms.custom: security-recommendations
ms.openlocfilehash: 543573610c2ea3ab0bcd89e1b8f4ee5f5a34dbc0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934425"
---
# <a name="security-control-identity-and-access-control"></a>Güvenlik Kontrolü: Kimlik ve Erişim Kontrolü

Kimlik ve erişim yönetimi önerileri, kimlik tabanlı erişim denetimiyle ilgili sorunları ele almaya, yönetime erişimi kilitlemeye, kimlikle ilgili olaylara karşı uyarı yapmaya, anormal hesap davranışına ve rol tabanlı erişim denetimine odaklanır.

## <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: İdari hesapların envanterini korumak

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.1 | 4.1 | Müşteri |

Azure AD,açıkça atanması gereken ve sorgulanabilen yerleşik rollere sahiptir. Yönetim gruplarına üye hesapları bulmak için geçici sorgular gerçekleştirmek için Azure AD PowerShell modüllerini kullanın.

PowerShell ile Azure AD'de dizin rolü nasıl elde elabilirsiniz:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

PowerShell ile Azure AD'de dizin rolünün üyelerini nasıl edinire bilgili olabilirsiniz:

https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

## <a name="32-change-default-passwords-where-applicable"></a>3.2: Varsayılan parolaları varsa değiştirme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3,2 | 4.2 | Müşteri |

Azure AD varsayılan parola kavramına sahip değildir. Parola gerektiren diğer Azure kaynakları, hizmete bağlı olarak farklılık gösteren karmaşıklık gereksinimleri ve minimum parola uzunluğuyla bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanabilen üçüncü taraf uygulamalardan ve pazar yeri hizmetlerinden siz sorumlusunuz.

## <a name="33-use-dedicated-administrative-accounts"></a>3.3: Özel yönetim hesaplarını kullanma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.3 | 4.3 | Müşteri |

Özel yönetim hesaplarının kullanımı yla ilgili standart işletim yordamları oluşturun. Yönetim hesabı sayısını izlemek için Azure Güvenlik Merkezi Kimliği ve Erişim Yönetimi'ni kullanın.

Ayrıca, Microsoft Hizmetleri için Azure AD Ayrıcalıklı Kimlik Yönetimi Ayrıcalıklı Rolleri ve Azure Kaynak Yöneticisi'ni kullanarak Tam Zamanında / Yeterli Erişim'i etkinleştirebilirsiniz. 

Daha fazla bilgi edinin:https://docs.microsoft.com/azure/active-directory/privileged-identity-management/

## <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile tek oturum açma (SSO) kullanma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.4 | 4.4 | Müşteri |

Mümkün olan her yerde, hizmet başına tek tek kimlik bilgilerini yapılandırmak yerine Azure Active Directory SSO'su kullanın. Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini kullanın.

Azure AD ile SSO'yi anlayın:

https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

## <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3,5 | 4.5, 11.5, 12.11, 16.3 | Müşteri |

Azure AD MFA'sını etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin.

Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

## <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.6 | 4.6, 11.6, 12.12 | Müşteri |

Azure kaynaklarında oturum açmak ve yapılandırmak için MFA yapılandırılmış PAW'ları (ayrıcalıklı erişim iş istasyonları) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:

https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted


## <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.7 | 4.8, 4.9 | Müşteri |

Çevrede şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için Azure Etkin Dizin güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi'ni kullanın.

Riskli etkinlikler için işaretlenen Azure AD kullanıcılarını nasıl tanımlarım:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-user-at-risk

Azure Güvenlik Merkezi'nde kullanıcıların kimlik ve erişim etkinliklerini izleme:

https://docs.microsoft.com/azure/security-center/security-center-identity-access

## <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.8 | 11.7 | Müşteri |

Yalnızca IP adresi aralıklarının veya ülkelerin/bölgelerinin belirli mantıksal gruplandırmalarından erişime izin vermek için Koşullu Erişim Adlandırılmış Konumlar'ı kullanın.

Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

## <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.9 | 16.1, 16.2, 16.4, 16.5, 16.6 | Müşteri |

Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Etkin Dizini'ni (AAD) kullanın. AAD, istirahatte ve aktarım sırasında veriler için güçlü şifreleme kullanarak verileri korur. AAD ayrıca kullanıcı kimlik bilgilerini tuzlar, iş lerle ve güvenli bir şekilde saklar.

AAD örneğini oluşturma ve yapılandırma:

https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant

## <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.10 | 16.9, 16.10 | Müşteri |

Azure AD, eski hesapları keşfetmeye yardımcı olmak için günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmek için Azure Kimlik Erişim İncelemeleri'ni kullanın. Kullanıcı erişimi, yalnızca doğru Kullanıcıların sürekli erişime sahip olduğundan emin olmak için düzenli olarak gözden geçirilebilir. 

Azure REKLAM Raporlama:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/

Azure Kimlik Erişim İncelemeleri nasıl kullanılır:

https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

## <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.11 | 16.12 | Müşteri |

Herhangi bir SIEM/İzleme aracıyla tümleştirmenize olanak tanıyan Azure AD Oturum Açma Etkinliği, Denetim ve Risk Etkinliği günlük kaynaklarına erişebilirsiniz.

Azure Active Directory kullanıcı hesapları için Tanılama Ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics Çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. Log Analytics Çalışma Alanı içinde istediğiniz Uyarıları yapılandırabilirsiniz.

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

## <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.12 | 16.13 | Müşteri |

Kullanıcı kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure AD Riski ve Kimlik Koruması özelliklerini kullanın. Ayrıca, daha fazla araştırma için Azure Sentinel'e veri sindirebilirsiniz.

Azure AD riskli oturum açmaları nasıl görüntülenebilen:

https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:

https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure Sentinel'e nasıl binilir:

https://docs.microsoft.com/azure/sentinel/quickstart-onboard

## <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

| Azure Kimliği | BDT'ler | Sorumluluk |
|--|--|--|
| 3.13 | 16 | Müşteri |

Microsoft'un müşteri verilerine erişmesi gereken destek senaryolarında, Customer Lockbox müşteri veri erişim isteklerini gözden geçirmeniz ve onaylamanız veya reddetmeniz için bir arabirim sağlar.

Müşteri Kilit Kutusunu Anlayın:

https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki güvenlik denetimine bakın: [Veri Koruması](security-control-data-protection.md)
