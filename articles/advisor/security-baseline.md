---
title: Azure Advisor için Azure Güvenlik temeli
description: Azure Advisor güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: advisor
ms.topic: conceptual
ms.date: 11/17/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 9ca8fbfb7f339c18f32379c64b40bc86e2bc47a6
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94811071"
---
# <a name="azure-security-baseline-for-azure-advisor"></a>Azure Advisor için Azure Güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslama sürümü 2,0](../security/benchmarks/overview.md) ' den Azure Advisor 'a kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure Advisor için geçerli olan ilgili kılavuza göre gruplandırılır. Azure Advisor için geçerli olmayan **denetimler** dışlandı.

Azure Advisor 'ın Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure Advisor güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="identity-management"></a>Kimlik Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik yönetimi](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>Im-1: merkezi kimlik ve kimlik doğrulama sistemi olarak Azure Active Directory standartlaştırma

**Kılavuz**: Azure Advisor varsayılan kimlik ve erişim yönetimi hizmeti olarak Azure Active Directory (Azure AD) kullanır. Kuruluşunuzun kimlik ve erişim yönetimini yönetmek için Azure AD 'yi standartlaştırın:

- Azure portal, Azure depolama, Azure sanal makinesi (Linux ve Windows), Azure Key Vault, PaaS ve SaaS uygulamaları gibi Microsoft Bulut kaynaklar
- Kuruluşunuzun Azure veya kurumsal ağ kaynaklarınızın uygulamaları gibi kaynakları

Azure AD 'nin güvenliğini sağlamak, kuruluşunuzun bulut güvenlik pratikte yüksek bir önceliktir. Azure AD Ayrıca, Microsoft 'un en iyi yöntem önerilerine göre kimlik güvenliğini değerlendirmenize yardımcı olacak bir kimlik güvenli puanı sağlar. Yapılandırmanızın en iyi yöntem önerilerini ne kadar yakından eşleştirmekte olduğunu ölçmek ve güvenlik duruşunuz üzerinde geliştirmeler yapmak için puanı kullanın.

Azure AD 'nin, Microsoft hesabı olmayan kullanıcıların kendi uygulamalarında ve kaynaklarına oturum açmasını sağlamak için dış kimlikleri desteklediğini unutmayın.

- [Azure Active Directory kiracı](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Uygulama için dış kimlik sağlayıcılarını kullanma](/azure/active-directory/b2b/identity-providers) 

- [Azure Active Directory kimlik güvenli puanı nedir?](../active-directory/fundamentals/identity-secure-score.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: uygulama erişimi için Azure AD çoklu oturum açma (SSO) kullanma

**Kılavuz**: Azure Danışmanı, Azure kaynaklarına, bulut uygulamalarına ve şirket içi uygulamalara kimlik ve erişim yönetimi sağlamak için Azure Active Directory (Azure AD) kullanır. Bu, çalışanlar gibi kuruluş kimliklerini, ayrıca iş ortakları, satıcılar ve tedarikçiler gibi dış kimlikleri içerir. 

Şirket içinde ve bulutta kuruluşunuzun verilerine ve kaynaklarına erişimi yönetmek ve güvenli hale getirmek için çoklu oturum açma kullanın. Sorunsuz, güvenli erişim ve daha fazla görünürlük ve denetim sağlamak için tüm kullanıcılarınızı, uygulamalarınızı ve cihazlarınızı Azure AD 'ye bağlayın.

- [Azure AD ile uygulama SSO 'SU anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>Im-4: tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulama denetimleri kullanma

**Kılavuz**: Azure Danışmanı, çok faktörlü kimlik doğrulaması aracılığıyla güçlü kimlik doğrulama denetimlerini destekleyen Azure Active Directory (Azure AD) kullanır ve güçlü parolasız Yöntemler.
- Çok faktörlü Authentication-Azure AD çok faktörlü kimlik doğrulamasını etkinleştirin ve çok faktörlü kimlik doğrulama kuruluminizdeki bazı en iyi uygulamalar için Azure Güvenlik Merkezi 'nin kimlik ve erişim yönetimi önerilerini izleyin. Çok faktörlü kimlik doğrulaması, tüm kullanıcılara veya oturum açma koşullarını ve risk etmenlerine göre Kullanıcı başına düzeyinde uygulanabilir.
- Passwordless kimlik doğrulaması – üç adet passwordless kimlik doğrulama seçeneği mevcuttur. Bunlar, Iş için Windows Hello, Microsoft Authenticator uygulaması ve akıllı kartlar gibi şirket içi kimlik doğrulama yöntemleri.

Yönetici ve ayrıcalıklı kullanıcılar için, güçlü kimlik doğrulama yönteminin en yüksek düzeyinin kullanıldığından emin olun ve ardından uygun güçlü kimlik doğrulama ilkesini diğer kullanıcılara teslim edin.

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Azure Active Directory için passwordless kimlik doğrulama seçeneklerine giriş](../active-directory/authentication/concept-authentication-passwordless.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>Im-5: hesap bozukluklarını Izleyin ve uyarır

**Kılavuz**: Azure Advisor, aşağıdaki veri kaynaklarını sağlayan Azure Active Directory (Azure AD) ile tümleşiktir:
- Oturum aç-oturum açma raporu, yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinliklerinin kullanımı hakkında bilgi sağlar.
- Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Denetim günlüklerine örnek olarak, Azure AD 'de Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma gibi herhangi bir kaynak üzerinde yapılan değişiklikler bulunur.
- Riskli oturum açma-riskli oturum açma, bir kullanıcı hesabının meşru sahibi olmayan birisi tarafından gerçekleştirilmiş olabilecek oturum açma girişimine yönelik bir göstergedir.
- Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Azure Izleyici, Azure Sentinel veya üçüncü taraf SıEM sistemleri ile tümleştirme için bu veri kaynaklarını kullanın. Azure Güvenlik Merkezi 'nde, çok fazla sayıda başarısız kimlik doğrulama denemesi, abonelikte kullanım dışı hesaplar gibi bazı şüpheli etkinlikler için Uyarılar ayarlayın.

Azure Gelişmiş tehdit koruması (ATP), gelişmiş tehditleri, güvenliği aşılmış kimlikleri ve kötü amaçlı Insider eylemlerini belirlemek, algılamak ve araştırmak için Active Directory sinyalleri kullanan bir güvenlik çözümüdür.

- [Azure Active Directory Denetim etkinliği raporları](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md) 

- [Azure Güvenlik Merkezi 'nin tehdit bilgileri koruma modülündeki uyarılar](../security-center/alerts-reference.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: koşullara göre Azure kaynak erişimini kısıtlama

**Kılavuz**: Azure Danışmanı, Kullanıcı tanımlı koşullara göre daha ayrıntılı erişim denetimi için Azure Active Directory (Azure AD) koşullu erişim özelliğini destekler. Örneğin, belirli IP aralıklarından kullanıcı oturumlarının oturum açmak için Multi-Factor Authentication kullanması gerekir. Ayrıntılı kimlik doğrulama oturumu yönetimi ilkesi, farklı kullanım durumları için de kullanılabilir.

- [Azure koşullu erişimine genel bakış](../active-directory/conditional-access/overview.md) 

- [Ortak koşullu erişim ilkeleri](../active-directory/conditional-access/concept-conditional-access-policy-common.md) 

- [Koşullu erişimle kimlik doğrulama oturumu yönetimini yapılandırma](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="privileged-access"></a>Ayrıcalıklı erişim

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ayrıcalıklı erişim](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: iş açısından kritik sistemlere yönetici erişimini kısıtlama

**Kılavuz**: Azure Danışmanı, aboneliğe ve yönetim gruplarına ayrıcalıklı erişim izni verildiğini kısıtlayarak iş açısından kritik sistemlere erişimi yalıtmak için Azure rol tabanlı erişim denetimi 'Ni (Azure RBAC) kullanır.

Active Directory Etki Alanı Denetleyicileri (DC), güvenlik araçları ve iş açısından kritik sistemlere yüklenen aracılarla sistem yönetimi araçları gibi iş açısından kritik erişimlerinize yönetim erişimi olan yönetim, kimlik ve güvenlik sistemlerine erişimi kısıtlayın. Bu yönetim ve güvenlik sistemlerine zarar veren saldırganlar, iş açısından kritik varlıkların güvenliğini tehlikeye atabilir.

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır.

- [Azure bileşenleri ve başvuru modeli](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Yönetim grubu erişimi](../governance/management-groups/overview.md#management-group-access)

- [Azure abonelik yöneticileri](../cost-management-billing/manage/add-change-subscription-administrator.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Kullanıcı erişimini düzenli olarak gözden geçirme ve mutabık kılma

**Kılavuz**: Azure Danışmanı, kaynaklarını yönetmek için Azure Active Directory (Azure AD) hesapları kullanır, hesapların ve erişiminin geçerli olduğundan emin olmak için Kullanıcı hesaplarını ve erişim atamasını düzenli olarak gözden geçirin. Grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını gözden geçirmek için Azure AD erişim incelemelerini uygulayın. Azure AD raporlama, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlayabilir. 

Ayrıca, gözden geçirme sürecini kolaylaştırmak için, erişim gözden geçirme raporu iş akışı oluşturmak için Azure AD 'nin Privileged Identity Management özelliklerini kullanın. Privileged Identity Management, çok fazla sayıda yönetici hesabı oluşturulduğunda ve eski veya yanlış yapılandırılmış yönetici hesaplarını belirlemek için de yapılandırılabilir.

Bazı Azure Hizmetleri, Azure AD aracılığıyla yönetilmeyen yerel kullanıcıları ve rolleri desteklediğine unutmayın. Müşterilerin bu kullanıcıları ayrı ayrı yönetmesi gerekir.

- [Privileged Identity Management (PıM) içinde Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](/azure/active-directory/governance/access-reviews-overvie)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: ayrıcalıklı erişim iş istasyonlarını kullanma

**Rehberlik**: güvenli, yalıtılmış iş istasyonları Yöneticiler, geliştiriciler ve kritik hizmet işleçleri gibi hassas rollerin güvenliği açısından kritik öneme sahiptir. 

Yönetim görevleri için yüksek düzeyde güvenli Kullanıcı iş istasyonları ve/veya Azure savunma kullanın. Yönetim görevleri için güvenli ve yönetilen bir kullanıcı iş istasyonu dağıtmak üzere Microsoft Intune dahil olmak üzere Azure Active Directory (Azure AD), Microsoft Defender Gelişmiş tehdit koruması (ATP) öğesini seçin. 

Güçlü kimlik doğrulaması, yazılım ve donanım temelleri, kısıtlı mantıksal ve ağ erişimi gibi güvenli yapılandırmayı zorlamak için güvenli iş istasyonlarını merkezi olarak yönetin.

- [Ayrıcalıklı erişim iş istasyonlarını anlama](../active-directory/devices/concept-azure-managed-workstation.md) 
 
- [Ayrıcalıklı erişim iş istasyonu dağıtma](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: yalnızca yeterli yönetim (en az ayrıcalık ilkesi) Izleyin 

**Kılavuz**: Azure Danışmanı, kaynaklarını yönetmek için Azure rol tabanlı erişim denetimi (Azure RBAC) ile tümleşiktir. Azure RBAC kullanarak rol atamaları aracılığıyla Azure kaynak erişimini yönetin. 

Kullanıcılara roller atayın, hizmet sorumlularını ve yönetilen kimlikleri gruplandırır. Belirli kaynaklar için önceden tanımlanmış yerleşik roller vardır ve bu, Azure CLı, Azure PowerShell veya Azure portal gibi araçlar aracılığıyla envantere alınabilir veya sorgulanabilecek. Azure RBAC aracılığıyla kaynaklara atanan ayrıcalıkların her zaman rollerin gerektirdiği ile sınırlı olması gerekir. Bu, Azure AD Privileged Identity Management (PıM) ' nin tam zamanında (JıT) yaklaşımını tamamlar ve düzenli olarak incelenmelidir.

İzin ayırmak ve yalnızca gerektiğinde özel rol oluşturmak için yerleşik rolleri kullanın.

Azure rol tabanlı erişim denetimi (Azure RBAC) nedir?. /Role-based-Access-Control/Overview.exe 

- [Azure 'da RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md) 

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](/azure/active-directory/governance/access-reviews-overview)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="asset-management"></a>Varlık Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: varlık yönetimi](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>ÖÖ-1: güvenlik ekibinin, varlıklar için riskleri görebilirliği sağlayın

**Rehberlik**: güvenlik ekiplerinde Azure Güvenlik Merkezi 'ni kullanarak güvenlik risklerini izleyebilmeleri için Azure kiracınızda ve aboneliklerinizde güvenlik okuyucusu izinleri verildiğinden emin olun. 

Güvenlik ekibinin sorumluluklarına bağlı olarak, güvenlik risklerini izlemek, merkezi bir güvenlik ekibinin veya yerel ekibin sorumluluğunda olabilir. Bu şekilde, güvenlik öngörüleri ve riskler her zaman bir kuruluş içinde toplanmalıdır. 

Güvenlik okuyucusu izinleri bir kiracının tamamına (kök yönetim grubu) veya yönetim gruplarına veya belirli aboneliklere göre uygulanabilir. 

Note: iş yüklerinin ve hizmetlerin görünürlüğünü almak için ek izinler gerekebilir. 

- [Güvenlik okuyucusu rolüne genel bakış](../role-based-access-control/built-in-roles.md#security-reader)

- [Azure Yönetim Grupları 'ye Genel Bakış](../governance/management-groups/overview.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>ÖÖ-5: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Kılavuz**: Azure Advisor kimlik ve kimlik doğrulama için Azure Active Directory (Azure AD) kullanır, Azure portal ve Azure Resource Manager Advisor 'ı yönetmek için kullanılır. 

İş gereksinimlerine bağlı olarak, "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" seçeneğini yapılandırarak kullanıcının Azure Kaynak Yöneticisi ile etkileşime geçmesini sınırlamak için Azure koşullu erişimi kullanın. 

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-threat-detection"></a>Günlüğe kaydetme ve tehdit algılama

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Azure kaynakları için günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: etkinlik günlükleri otomatik olarak kullanılabilir ve okuma IŞLEMLERI (Get) haricinde Azure Advisor kaynaklarınız için tüm yazma IŞLEMLERINI (put, Post, Delete) içerir. 

Etkinlik günlükleri, sorun giderirken bir hata bulmak veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizi izlemek için kullanılabilir.

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../azure-monitor/platform/platform-logs-overview.md)

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: güvenlik günlüğü yönetimini ve analizini merkezileştirme

**Rehberlik**: bağıntı sağlamak için günlük depolamayı ve çözümlemeyi merkezileştirme. Her günlük kaynağı için bir veri sahibi, erişim Kılavuzu, depolama konumu, verileri işlemek ve erişmek için kullanılan araçları ve veri saklama gereksinimlerini atadığınızdan emin olun.

Azure etkinlik günlüklerini merkezi günlüklerinizi tümleştirdiğinizden emin olun. Endpoint cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın.

Ayrıca, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'e verileri etkinleştirin ve ekleyin. Birçok kuruluş, daha az sıklıkta kullanılan "soğuk" verileri için sık kullanılan ve Azure depolama için Azure Sentinel ' i "etkin" veriler için kullanmayı seçer.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: günlük depolama bekletmesini yapılandırma

**Kılavuz**: Azure Advisor günlüklerini depolamak için kullanılan tüm depolama hesapları veya Log Analytics çalışma alanları, kuruluşunuzun uyumluluk düzenlemelerine göre ayarlanmış günlük tutma süresine sahip olduğundan emin olun.
Azure Izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayabilirsiniz. Uzun süreli ve arşiv depolama için Azure depolama, Data Lake veya Log Analytics çalışma alanı hesapları kullanın.

- [Log Analytics çalışma alanı saklama süresini yapılandırma](../azure-monitor/platform/manage-cost-storage.md) 

- [Kaynak günlüklerini bir Azure depolama hesabında depolama](/azure/azure-monitor/platform/resource-logs-collect-storage)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: hazırlık – Azure için olay yanıtı işlemini güncelleştirme

**Rehberlik**: kuruluşunuzun güvenlik olaylarına yanıt vermek için işlem yapıldığından emin olun, bu işlemlerin Azure 'da güncelleştirildiğini ve hazırlık sağlamak için bunları düzenli olarak bir şekilde kullanıma hazırlıyoruz.

- [Kurumsal ortam genelinde güvenliği uygulama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Olay yanıtı başvuru kılavuzu](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: hazırlık – olay bildirimini kurma

**Rehberlik**: Azure Güvenlik Merkezi 'nde güvenlik olayı iletişim bilgilerini ayarlama. Bu iletişim bilgileri, Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. Ayrıca, olay uyarı gereksinimlerinize göre farklı Azure hizmetlerinde olay uyarısını ve bildirimi özelleştirme seçenekleriniz vardır. 

- [Azure Güvenlik Merkezi güvenlik ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: algılama ve analiz – yüksek kaliteli uyarılara göre olaylar oluşturun

**Rehberlik**: yüksek kaliteli uyarılar oluşturmak ve uyarı kalitesini ölçmek için bir işleminiz olduğundan emin olun. Bu, geçmiş olaylardan dersler öğrenmenize ve analistlere yönelik olarak bu uyarıların önceliklendirmesine izin verir, böylelikle hatalı pozitif sonuçlar üzerinde zaman harcanmazlar. 

Yüksek kaliteli uyarılar, geçmiş olaylar, doğrulanan topluluk kaynakları ve uyarılar oluşturmak ve temizlemek için tasarlanan araçların yanı sıra çeşitli sinyal kaynaklarını kullanarak oluşturulabilir. 

Azure Güvenlik Merkezi, birçok Azure varlığı arasında yüksek kaliteli uyarılar sağlar. Azure Sentinel 'e uyarı akışı sağlamak için ASC Data bağlayıcısını kullanabilirsiniz. Azure Sentinel, bir araştırma için olayları otomatik olarak oluşturmak üzere gelişmiş uyarı kuralları oluşturmanıza olanak sağlar. 

Azure kaynakları için riskleri belirlemenize yardımcı olmak üzere dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışarı aktarın.

- [Dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: algılama ve analiz – bir olayı araştırın

**Rehberlik**: analistlerin, olası olayları araştırarak, ne olduğunu tam olarak bir görünüm oluşturmak için farklı veri kaynaklarını sorgulayabileceği ve kullanabilmesi için analist olun. Görünmeyen noktaları önlemek için, bir olası saldırganın sonlandırma zincirindeki etkinliklerini izlemek üzere farklı Günlükler toplanmalıdır.  Ayrıca, Öngörüler ve dersleri diğer analistler için yakalandığından ve gelecekteki geçmiş başvurusuyla emin olmanız gerekir.  

Araştırmaya yönelik veri kaynakları, zaten kapsam içi hizmetlerden ve çalışan sistemlerde toplanmakta olan Merkezi günlük kaynakları içerir, ancak şunları da içerebilir:

- Ağ verileri – ağ akışı günlüklerini ve diğer analiz bilgilerini yakalamak için ağ güvenlik grupları ' akış günlükleri, Azure ağ Izleyicisi ve Azure Izleyicisi ' ni kullanın. 

- Çalışan sistemlerin anlık görüntüleri: 

    - Çalışan sistemin diskinin anlık görüntüsünü oluşturmak için Azure sanal makinesinin anlık görüntü özelliğini kullanın. 

    - Çalışan sistem belleğinin bir anlık görüntüsünü oluşturmak için işletim sisteminin yerel bellek dökümü özelliğini kullanın.

    - Çalışan sistemlerin anlık görüntülerini oluşturmak için Azure hizmetlerinin anlık görüntü özelliğini veya yazılımınızın kendi özelliğini kullanın.

Azure Sentinel, olayların tam yaşam döngüsünü yönetmek için neredeyse tüm günlük kaynakları ve bir servis talebi yönetim portalı genelinde kapsamlı veri analizi sağlar. İnceleme sırasında zeka bilgileri, izleme ve raporlama amaçları için bir olayla ilişkilendirilebilir. 

- [Windows makinenin diskinin anlık görüntüsünü alma](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Linux makinesinin diskinin anlık görüntüsünü alma](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure tanılama bilgilerini ve bellek dökümü toplamayı destekler](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Azure Sentinel ile olayları araştırın](../sentinel/tutorial-investigate-cases.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: algılama ve analiz – olayların önceliklerini belirleme

**Rehberlik**: olayların, uyarı önem derecesine ve varlık duyarlığına göre ilk olarak odaklanacağı analistlere bağlam sağlar. 

Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin uyarıyı vermek için kullanılan bulma veya analitik ile ne kadar önemli olduğunu ve uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu belirten güven düzeyini temel alır.

Ayrıca, etiketleri kullanarak kaynakları işaretleyin ve Azure kaynaklarını tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: kapsama, eradleme ve kurtarma – olay işlemeyi otomatikleştirin

**Rehberlik**: yanıt süresini hızlandırmak ve analistlerin yükünü azaltmak için el ile yinelenen görevleri otomatikleştirin. El ile gerçekleştirilen görevlerin yürütülmesi daha uzun sürer, her olayı yavaşlatarak analist 'nin işleyebileceği olayların sayısını azaltır. El ile gerçekleştirilen görevler Ayrıca analist yükselini artırır ve bu da gecikme ve analistlerin karmaşık görevlere etkin bir şekilde odaklanmasına olanak tanır. İşlemleri otomatik olarak tetiklemek veya gelen güvenlik uyarılarına yanıt vermek üzere bir PlayBook çalıştırmak için Azure Güvenlik Merkezi 'nde ve Azure Sentinel 'de iş akışı Otomasyonu özelliklerini kullanın. PlayBook, bildirimler gönderme, hesapları devre dışı bırakma ve sorunlu ağları yalıtma gibi işlemleri gerçekleştirir. 

- [Güvenlik Merkezi 'nde iş akışı Otomasyonu 'nu yapılandırma](../security-center/workflow-automation.md)

- [Azure Güvenlik Merkezi 'nde otomatik tehdit yanıtlarını ayarlama](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Azure Sentinel 'de otomatik tehdit yanıtlarını ayarlama](../sentinel/tutorial-respond-threats-playbook.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="posture-and-vulnerability-management"></a>Posture ve güvenlik açığı yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Posture ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-8-conduct-regular-attack-simulation"></a>BD-8: normal saldırı simülasyonu gerçekleştir

**Rehberlik**: gerektiğinde, Azure kaynaklarınızda sızma testi veya Red takım etkinlikleri gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin.
Microsoft Bulut penme test kurallarını izleyerek, sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olun. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

- [Azure 'da sızma testi](../security/fundamentals/pen-testing.md)

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="governance-and-strategy"></a>İdare ve Strateji

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: idare ve strateji](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: varlık yönetimi ve veri koruma stratejisini tanımlama 

**Rehberlik**: sistemlerin ve verilerin sürekli izlenmesi ve korunması için açık bir strateji belgelemenizi ve iletdiğinizden emin olun. İş açısından kritik veri ve sistemlerinin bulunmasını, değerlendirmesini, korunmasını ve izlenmesini önceliklendirin. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Veri sınıflandırma standardı, iş riskleriyle uyumlu

-   Risk ve varlık envanterinde güvenlik kuruluşu görünürlüğü 

-   Azure hizmetlerinin kullanım için güvenlik kuruluşu onayı 

-   Varlıkların yaşam döngülerinde güvenliği

-   Kurumsal veri sınıflandırmasına uygun olarak gerekli erişim denetimi stratejisi

-   Azure yerel ve üçüncü taraf veri koruma özellikleri kullanımı

-   Geçiş içi ve REST kullanım örnekleri için veri şifreleme gereksinimleri

-   Uygun şifreleme standartları

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik mimarisi önerisi-depolama, veri ve şifreleme](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure güvenlik temelleri-Azure veri güvenliği, şifreleme ve depolama](../security/fundamentals/encryption-overview.md)

- [Bulut benimseme çerçevesi-Azure veri güvenliği ve şifreleme en iyi uygulamaları](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure Güvenlik kıyaslaması-varlık yönetimi](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure Güvenlik kıyaslaması-veri koruma](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Kurumsal segmentleme stratejisini tanımlama 

**Rehberlik**: diğer denetimler arasında kimlik, ağ, uygulama, abonelik, yönetim grubu birleşimini kullanarak varlıklara erişimi segmentlere ayırmak için kurumsal çapta bir strateji oluşturun.

Güvenlik ayrımı gereksinimini, birbirleriyle iletişim kurması gereken sistemlerin günlük işlemlerini etkinleştirme gereksinimiyle dikkatle dengeleyin ve verilere erişin.

Kesimleme stratejisinin ağ güvenliği, kimlik ve erişim modelleri ve uygulama izni ya da erişim modelleri ve insan süreci denetimleri dahil olmak üzere Denetim türleri arasında tutarlı bir şekilde uygulandığından emin olun.

- [Azure 'da segmentasyon stratejisi ile ilgili kılavuz (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure 'da segmentasyon stratejisi ile ilgili kılavuz (belge)](/security/compass/governance#enterprise-segmentation-strategy)

- [Kurumsal segmentasyon stratejisi ile ağ segmentlemesini hizalayın](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: güvenlik sonrası yönetim stratejisini tanımlama

**Rehberlik**: bireysel varlıklarınızda ve barındırdıkları ortamda riskleri sürekli olarak ölçün ve azaltır. Yayımlanan uygulamalar, ağ giriş ve çıkış noktaları, Kullanıcı ve yönetici uç noktaları vb. gibi yüksek değerli varlıkların ve yüksek oranda ortaya çıkarılan saldırı yüzeylerinin önceliklerini belirleyin.

- [Azure Güvenlik kıyaslaması-Posture ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: kuruluş rollerini, sorumlulukları ve accountacakları hizalayın

**Rehberlik**: güvenlik kuruluşunuzdaki roller ve sorumlulukların açık bir stratejisini belgelemenizi ve iletdiğinizden emin olun. Güvenlik kararlarını açık bir şekilde yapın, paylaşılan sorumluluk modelinde herkese eğitilendir ve bulutu güvenli hale getirmek için teknik ekipleri teknolojide eğitin.

- [Azure Güvenlik En Iyi Yöntem 1 – Insanlar: ekipleri bulut güvenlik yolculuğunda eğitin](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure Güvenlik En Iyi uygulaması 2-Insanlar: bulut güvenlik teknolojisindeki takımları eğitin](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure Güvenlik En Iyi Yöntem 3-Işlem: bulut güvenlik kararları için sorumluluk atama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-5-define-network-security-strategy"></a>GS-5: ağ güvenlik stratejisini tanımlayın

**Rehberlik**: kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak Azure ağ güvenlik yaklaşımı oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi ağ yönetimi ve güvenlik sorumluluğu

-   Kurumsal segmentasyon stratejisi ile hizalanan sanal ağ kesimleme modeli

-   Farklı tehdit ve saldırı senaryolarında düzeltme stratejisi

-   Internet Edge ve giriş ve çıkış stratejisi

-   Karma bulut ve şirket içi bağlantı stratejisi

-   Güncel ağ güvenlik yapıtları (ör. ağ diyagramları, başvuru ağ mimarisi)

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Güvenlik kıyaslaması-ağ güvenliği](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure ağ güvenliğine genel bakış](../security/fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: kimlik ve ayrıcalıklı erişim stratejisi tanımlama

**Rehberlik**: kuruluşunuzun genel kurumsal erişim denetimi stratejisinin bir parçası olarak bir Azure kimliği ve ayrıcalıklı erişim yaklaşımı oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi bir kimlik ve kimlik doğrulama sistemi ve diğer iç ve dış kimlik sistemleriyle karşılıklı bağlantısı

-   Farklı kullanım durumlarında ve koşullarda güçlü kimlik doğrulama yöntemleri

-   Yüksek ayrıcalıklı kullanıcıların korunması

-   Anomali Kullanıcı etkinlikleri izleme ve işleme  

-   Kullanıcı kimliği ve erişim gözden geçirme ve mutabakat süreci

Daha fazla bilgi için başvurulan bağlantıları gözden geçirin.

- [Azure Güvenlik kıyaslaması-kimlik yönetimi](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure Güvenlik kıyaslaması-ayrıcalıklı erişim](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure Güvenlik En Iyi Yöntem 11 mimarisi. Tek Birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Identity Management güvenliğine genel bakış](../security/fundamentals/identity-management-overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: günlüğe kaydetme ve tehdit yanıt stratejisini tanımlama

**Rehberlik**: uyumluluk gereksinimlerini karşılaırken tehditleri hızlı bir şekilde tespit etmek ve düzeltmek için bir günlüğe kaydetme ve tehdit yanıt stratejisi kurun. Tümleştirme ve el ile adımlar yerine tehditlere odaklanabilmeleri için yüksek kaliteli uyarılar ve sorunsuz deneyimlerle analistleri sağlamayı önceliklendirin. 

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Güvenlik işlemleri (SecOps) kuruluşun rolü ve sorumlulukları 

-   NıST veya başka bir sektör çerçevesiyle hizalanan iyi tanımlanmış bir olay yanıt işlemi 

-   Tehdit algılamayı, olay yanıtını ve uyumluluk gereksinimlerini desteklemek için günlüğü yakalama ve bekletme

-   SıEM, yerel Azure özellikleri ve diğer kaynakları kullanarak tehditler hakkında merkezi görünürlük ve bağıntı bilgileri 

-   Müşterilerinizle, Tedarikçilerinizden ve genel ilgi şahıslarla iletişim ve bildirim planı

-   Günlüğe kaydetme ve tehdit algılama, Forensics ve saldırı düzeltme ve doğrulama gibi olay işleme için Azure yerel ve üçüncü taraf platformlarının kullanımı

-   Olayları ve olay sonrası etkinliklerini (öğrenilmiş dersler ve kanıt bekletme gibi) işleme için süreçler

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik kıyaslaması-günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure Güvenlik kıyaslaması-olay yanıtı](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure Güvenlik En Iyi Yöntem 4-Işlem. Bulut için olay yanıtı Işlemini Güncelleştir](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure benimseme çerçevesi, günlüğe kaydetme ve raporlama kararı Kılavuzu](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure Kurumsal ölçek, yönetim ve izleme](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması v2 genel bakış](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
