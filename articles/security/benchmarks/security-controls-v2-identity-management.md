---
title: Azure Güvenlik kıyaslaması v2-kimlik yönetimi
description: Azure Güvenlik kıyaslaması v2 kimlik yönetimi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: bcd88f9f21c68f7f6cdda7299ac97d67e97dc009
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059413"
---
# <a name="security-control-identity-management"></a>Güvenlik denetimi: kimlik yönetimi

Kimlik yönetimi, Azure Active Directory kullanarak güvenli bir kimlik ve erişim denetimleri kurmaya yönelik denetimleri ele alır. Bu, uygulamalar, koşullu erişim ve hesap bozuklukları izleme için çoklu oturum açma, güçlü kimlik doğrulamaları, Yönetilen kimlikler (ve hizmet ilkeleri) kullanımını içerir.

## <a name="id-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>KIMLIK-1: merkezi kimlik ve kimlik doğrulama sistemi olarak Azure Active Directory standartlaştırma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| KIMLIK-1 | 16,1, 16,2, 16,4, 16,5 | IA-2, ıA-8, AC-2, AC-3 |

Azure Active Directory (Azure AD), Azure 'un varsayılan kimlik ve erişim yönetimi hizmetidir. ' De kuruluşunuzun kimlik ve erişim yönetimini yönetmek için Azure AD 'de standartlaşmanız gerekir:
- Azure portal, Azure depolama, Azure sanal makineleri (Linux ve Windows), Azure Key Vault, PaaS ve SaaS uygulamaları gibi Microsoft bulut kaynakları.

- Kuruluşunuzun Azure veya kurumsal ağ kaynaklarınızın uygulamaları gibi kaynakları.

Azure AD 'nin güvenliğini sağlamak, kuruluşunuzun bulut güvenlik pratikte yüksek öncelikli olmalıdır. Azure AD, Microsoft 'un en iyi yöntem önerilerine göre kimlik güvenliğini değerlendirmenize yardımcı olacak bir kimlik güvenli puanı sağlar. Yapılandırmanızın en iyi yöntem önerilerini ne kadar yakından eşleştirmekte olduğunu ölçmek ve güvenlik duruşunuz üzerinde geliştirmeler yapmak için puanı kullanın.

Note: Azure AD, Microsoft hesabı olmayan kullanıcıların kendi uygulamalarına ve kaynaklarına kendi dış kimlik bilgileriyle oturum açmasını sağlayan dış kimlik sağlayıcılarını destekler.

- [Azure AD 'de kiracı](../../active-directory/develop/single-and-multi-tenant-apps.md)

- [Azure AD örneği oluşturma ve yapılandırma](../../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Azure AD kiracılarını tanımlama](https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/)  

- [Bir uygulama için dış kimlik sağlayıcılarını kullanma](/azure/active-directory/b2b/identity-providers)

- [Azure AD 'de kimlik güvenli puanı nedir?](../../active-directory/fundamentals/identity-secure-score.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys) 

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-2-manage-application-identities-securely-and-automatically"></a>KIMLIK-2: uygulama kimliklerini güvenli ve otomatik olarak yönetme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| KIMLIK-2 | Yok | AC-2, AC-3, ıA-2, ıA-4, ıA-9 |

Hizmetler veya Otomasyon gibi insan dışı hesaplar için, kaynaklara erişmek veya kod yürütmek üzere daha güçlü bir insan hesabı oluşturmak yerine Azure Yönetilen kimlikler ' i kullanın. Azure Yönetilen kimlikler, Azure hizmetleri ve Azure AD kimlik doğrulamasını destekleyen kaynaklar için kimlik doğrulaması yapabilir. Kimlik doğrulaması, önceden tanımlanmış erişim verme kuralları aracılığıyla etkinleştirilir ve kaynak kodunda veya yapılandırma dosyalarında sabit kodlu kimlik bilgileri önlenir. 

Yönetilen kimlikleri desteklemeyen hizmetler için, bunun yerine kaynak düzeyinde kısıtlanmış izinlerle bir hizmet sorumlusu oluşturmak için Azure AD 'yi kullanın.  Hizmet sorumlularını sertifika kimlik bilgileriyle yapılandırmanız ve istemci gizliliklerinin geri dönmesi önerilir. Her iki durumda da, Azure Key Vault çalışma zamanı ortamının (bir Azure işlevi gibi) anahtar kasasından kimlik bilgilerini alabilmesi için Azure tarafından yönetilen kimliklerle birlikte kullanılabilir.

- [Azure Yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md)

- [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [Azure hizmet sorumlusu](/powershell/azure/create-azure-service-principal-azureps)

- [Sertifikalarla hizmet sorumlusu oluşturma](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)

Güvenlik sorumlusu kaydı için Azure Key Vault kullan: kimlik doğrulama # Yetkilendir-a-güvenlik-asıl-erişim-anahtar-kasa

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>KIMLIK-3: uygulama erişimi için Azure AD çoklu oturum açma (SSO) kullanın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| KIMLIK-3 | 4.4 | IA-2, ıA-4 |

Azure AD, Azure kaynaklarına, bulut uygulamalarına ve şirket içi uygulamalara kimlik ve erişim yönetimi sağlar. Kimlik ve erişim yönetimi, çalışanlar gibi kurumsal kimliklere ve iş ortakları, satıcılar ve tedarikçiler gibi dış kimliklere uygulanır.

Şirket içinde ve bulutta kuruluşunuzun verilerine ve kaynaklarına erişimi yönetmek ve güvenli hale getirmek için Azure AD çoklu oturum açma (SSO) kullanın. Sorunsuz, güvenli erişim ve daha fazla görünürlük ve denetim sağlamak için tüm kullanıcılarınızı, uygulamalarınızı ve cihazlarınızı Azure AD 'ye bağlayın. 

- [Azure AD ile uygulama SSO 'SU anlama](../../active-directory/manage-apps/what-is-single-sign-on.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>ID-4: tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulama denetimleri kullanın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| KIMLIK-4 | 4,2, 4,4 4,5, 11,5, 12,11, 16,3 | AC-2, AC-3, ıA-2, ıA-4 |

Azure AD, Multi-Factor Authentication (MFA) ve güçlü parolasız yöntemler aracılığıyla güçlü kimlik doğrulama denetimlerini destekler.  
- Multi-Factor Authentication: Azure AD MFA 'yı etkinleştirin ve MFA kurulumlarınız için Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin. MFA, tüm kullanıcılar, kullanıcılar veya oturum açma koşulları ve risk faktörleri temelinde Kullanıcı başına düzeyinde zorlanabilir. 

- Passwordless kimlik doğrulaması: üç adet passwordless kimlik doğrulama seçeneği mevcuttur: Iş için Windows Hello, Microsoft Authenticator uygulaması ve akıllı kartlar gibi şirket içi kimlik doğrulama yöntemleri. 

Yönetici ve ayrıcalıklı kullanıcılar için, güçlü kimlik doğrulama yönteminin en yüksek düzeyinin kullanıldığından emin olun ve ardından uygun güçlü kimlik doğrulama ilkesini diğer kullanıcılara teslim edin.

Azure AD kimlik doğrulaması için eski parola tabanlı kimlik doğrulaması kullanılmaya devam ediyorsa, lütfen yalnızca bulutta bulunan hesapların (doğrudan Azure 'da oluşturulan kullanıcı hesaplarının) varsayılan bir temel parola ilkesine sahip olduğunu unutmayın. Ve karma hesaplar (Şirket içi Active Directory gelen Kullanıcı hesapları) Şirket içi parola ilkelerini izler. Azure AD, parola tabanlı kimlik doğrulaması kullanırken kullanıcıların tahmin edilmesi kolay parolalar ayarlamamasını engelleyen bir parola koruma yeteneği sağlar. Microsoft, telemetri temelinde güncellenen yasaklanmış parolaların küresel bir listesini sağlar ve müşteriler bu listeyi gereksinimlerine göre artırabilir (örn. marka, kültürel başvuruları, vb.). Bu parola koruması, yalnızca bulut ve karma hesaplar için kullanılabilir. 

Note: tek başına parola kimlik bilgileri tabanlı kimlik doğrulaması popüler saldırı yöntemlerine açıktır. Daha yüksek güvenlik için MFA ve güçlü bir parola ilkesi gibi güçlü kimlik doğrulaması kullanın. Varsayılan parolalara sahip olabilecek üçüncü taraf uygulamalar ve Market Hizmetleri için, bunları ilk hizmet kurulumu sırasında değiştirmelisiniz. 

- [Azure 'da MFA 'yı etkinleştirme](../../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Active Directory için passwordless kimlik doğrulama seçeneklerine giriş](../../active-directory/authentication/concept-authentication-passwordless.md)

- [Azure AD varsayılan parola ilkesi](../../active-directory/authentication/concept-sspr-policy.md#password-policies-that-only-apply-to-cloud-user-accounts)

- [Azure AD parola korumasını kullanarak kötü parolaları kaldırma](../../active-directory/authentication/concept-password-ban-bad.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="id-5-monitor-and-alert-on-account-anomalies"></a>ID-5: hesap bozuklulıkları hakkında Izleyici ve uyarı

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| KIMLIK-5 | 4,8, 4,9, 16,12, 16,13 | AC-2, AC-3, AC-7, AU-6 |

Azure AD aşağıdaki veri kaynaklarını sağlar: 
-   Oturum açma işlemleri – oturum açma işlemleri raporu, yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinliklerinin kullanımı hakkında bilgi sağlar.

-   Denetim günlükleri-Azure AD 'deki çeşitli özelliklerle yapılan tüm değişiklikler için Günlükler aracılığıyla izlenebilirlik sağlar. Günlüğe kaydedilen değişiklikler denetim günlüklerine örnek olarak Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma sayılabilir.

-   Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

-   Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Bu veri kaynakları Azure Izleyici, Azure Sentinel veya üçüncü taraf SıEM sistemleriyle tümleştirilebilir.

Azure Güvenlik Merkezi, çok fazla sayıda başarısız kimlik doğrulama girişimi ve abonelikteki kullanım dışı hesaplar gibi bazı şüpheli etkinlikler üzerinde de uyarı verebilir. 

Azure Gelişmiş tehdit koruması (ATP), gelişmiş tehditleri, güvenliği aşılmış kimlikleri ve kötü amaçlı Insider eylemlerini belirlemek, algılamak ve araştırmak için şirket içi Active Directory sinyalleri kullanan bir güvenlik çözümüdür.

- [Azure AD 'de denetim etkinlik raporları](../../active-directory/reports-monitoring/concept-audit-logs.md)

- [Azure AD riskli oturum açma işlemlerini görüntüleme](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](/azure/active-directory/reports-monitoring/concept-user-at-risk)

- [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](../../security-center/security-center-identity-access.md)

- [Azure Güvenlik Merkezi 'nin tehdit bilgileri koruma modülündeki uyarılar](//azure/security-center/alerts-reference)

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Azure AD Kimlik Koruması verileri bağlama](../../sentinel/connect-azure-ad-identity-protection.md)

- [Azure Gelişmiş Tehdit Koruması](/azure-advanced-threat-protection/what-is-atp)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-6-restrict-azure-resource-access-based-on-conditions"></a>ID-6: koşullara göre Azure kaynak erişimini kısıtlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| KIMLIK-6 | Yok | AC-2, AC-3 |

Belirli IP aralıklarından MFA 'yı kullanmak için Kullanıcı oturumu açma gerekliliği gibi Kullanıcı tanımlı koşullara dayalı daha ayrıntılı erişim denetimi için Azure AD koşullu erişimi kullanın. Ayrıntılı bir kimlik doğrulama oturumu yönetimi, farklı kullanım durumları için Azure AD koşullu erişim ilkesi aracılığıyla da kullanılabilir. 

- [Azure koşullu erişimine genel bakış](../../active-directory/conditional-access/overview.md)

- [Sık kullanılan Koşullu Erişim İlkeleri](../../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Erişim Denetimi ile kimlik doğrulama oturum yönetimini yapılandırma](../../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Kimlik ve anahtarlar](/azure/cloud-adoption-framework/organize/cloud-security-identity-keys)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

- [Tehdit bilgileri](/azure/cloud-adoption-framework/organize/cloud-security-threat-intelligence)

## <a name="id-7-eliminate-unintended-credential-exposure"></a>KIMLIK-7: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| KIMLIK-7 | 18,1, 18,7 | IA-5 |

Kod içinde kimlik bilgilerini tanımlamak için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerinin Azure Key Vault gibi daha güvenli konumlara taşınmasını da önerir.

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)

## <a name="id-8-secure-user-access-to-legacy-applications"></a>KIMLIK-8: eski uygulamalara Kullanıcı erişimini güvenli hale getirme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| KIMLIK-8 | 14,6 | AC-2, AC-3, SC-11 |

Eski uygulamalar için modern erişim denetimlerine ve oturum izlemeye sahip olduğunuzdan ve depotıkları ve işledikleri veriler olduğundan emin olun. VPN 'Ler, eski uygulamalara erişmek için yaygın olarak kullanıldığında, genellikle yalnızca temel erişim denetimi ve sınırlı oturum izleme olur.

Azure AD Uygulama Ara Sunucusu, uzak kullanıcıların ve cihazların Azure AD koşullu erişimiyle güvenilirliğini açıkça doğrulayarak, çoklu oturum açma (SSO) ile uzak kullanıcılara eski şirket içi uygulamaları yayımlamanıza olanak sağlar. 

Alternatif olarak Microsoft Cloud App Security, bir kullanıcının uygulama oturumlarını ve engelleme eylemlerini izleme (Şirket içi uygulamalar ve hizmet olarak yazılım (SaaS) uygulamaları için) denetimleri sağlayabilen bir bulut erişim güvenlik Aracısı (CASB) hizmetidir. 

- [Azure AD Uygulama Ara Sunucusu](../../active-directory/manage-apps/application-proxy.md#what-is-application-proxy)

- [En iyi Microsoft Cloud App Security uygulamalar](/cloud-app-security/best-practices)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Altyapı ve uç nokta güvenliği](/azure/cloud-adoption-framework/organize/cloud-security-architecture)

- [Uygulama güvenliği ve DevSecOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

