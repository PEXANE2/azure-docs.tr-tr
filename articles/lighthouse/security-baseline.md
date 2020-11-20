---
title: Azure için Azure Güvenlik temeli açık
description: Azure Mathouse güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: lighthouse
ms.topic: conceptual
ms.date: 11/19/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 51c786d12c372276d1cb007cc5a929ab6a6302f7
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94974998"
---
# <a name="azure-security-baseline-for-azure-lighthouse"></a>Azure için Azure Güvenlik temeli açık

Bu güvenlik temeli, [Azure Güvenlik kıyaslama sürümü 2,0](../security/benchmarks/overview.md) ' dan Azure açık thouse 'a kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure Mathouse için geçerli olan ilgili kılavuza göre gruplandırılır. Azure için geçerli olmayan **denetimler** dahil değildir.

Azure Mathouse 'ın Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure mathouse güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="identity-management"></a>Kimlik Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik yönetimi](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>Im-1: merkezi kimlik ve kimlik doğrulama sistemi olarak Azure Active Directory standartlaştırma

**Kılavuz**: Azure Aydınlathouse, varsayılan kimlik ve erişim yönetimi hizmeti olarak Azure Active Directory (Azure AD) kullanır. Kuruluşunuzun kimlik ve erişim yönetimini yönetmek için Azure AD 'yi standartlaştırın:
- Azure portal, Azure depolama, Azure sanal makinesi (Linux ve Windows), Azure Key Vault, PaaS ve SaaS uygulamaları gibi Microsoft Bulut kaynaklar.
- Kuruluşunuzun Azure veya kurumsal ağ kaynaklarınızın uygulamaları gibi kaynakları.

Azure, bir yönetim kiracısındaki belirlenen kullanıcılara, bir müşterinin kiracısında Temsilcili aboneliklere ve/veya kaynak gruplarına erişmelerine olanak sağlayan bir Azure yerleşik rolü vardır. Tüm yerleşik roller, sahip veya DataActions iznine sahip herhangi bir yerleşik rol haricinde Şu anda desteklenmektedir. Kullanıcı erişimi yönetici rolü yalnızca yönetilen kimliklere rol atama konusunda sınırlı kullanım için desteklenir. Özel roller ve klasik abonelik yöneticisi rolleri desteklenmez.

- [Azure Active Directory kiracı](../active-directory/develop/single-and-multi-tenant-apps.md) 

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

- [Uygulama için dış kimlik sağlayıcılarını kullanma](/azure/active-directory/b2b/identity-providers) 

- [Azure Active Directory kimlik güvenli puanı nedir?](../active-directory/fundamentals/identity-secure-score.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="im-2-manage-application-identities-securely-and-automatically"></a>Im-2: uygulama kimliklerini güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure Yönetilen kimlikler, Azure hizmetleri ve Azure AD kimlik doğrulamasını destekleyen kaynaklar için kimlik doğrulaması yapabilir. Kimlik doğrulaması, önceden tanımlanmış erişim verme kuralları aracılığıyla etkinleştirilir ve kaynak kodunda veya yapılandırma dosyalarında sabit kodlu kimlik bilgileri önlenir. Azure 'un bir müşteri aboneliğinde Kullanıcı erişimi yönetici rolüne sahip kullanıcılar, bu müşterinin kiracısında yönetilen bir kimlik oluşturabilir. Bu rol, Azure Athouse ile genel olarak desteklenmekle birlikte, bu özel senaryoda kullanılabilir ve bu izne sahip kullanıcılar yönetilen kimliklere bir veya daha fazla yerleşik rol atayabilir.

Yönetilen kimlikleri desteklemeyen hizmetler için, bunun yerine kaynak düzeyinde kısıtlanmış izinlerle bir hizmet sorumlusu oluşturmak için Azure AD 'yi kullanın. Azure Kathouse, hizmet sorumlularının, ekleme işlemi sırasında verilen rollere göre müşteri kaynaklarına erişmesine olanak sağlar. Hizmet sorumlularını sertifika kimlik bilgileriyle yapılandırmanız ve istemci gizliliklerinin geri dönmesi önerilir. Her iki durumda da, Azure Key Vault çalışma zamanı ortamının (bir Azure işlevi gibi) anahtar kasasından kimlik bilgilerini alabilmesi için Azure tarafından yönetilen kimliklerle birlikte kullanılabilir.

- [Azure Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md)

- [Azure hizmet sorumlusu](/powershell/azure/create-azure-service-principal-azureps)

- [Güvenlik sorumlusu kaydı için Azure Key Vault kullanma](../key-vault/general/authentication.md#authorize-a-security-principal-to-access-key-vault)

- [Bir müşteri kiracısında yönetilen bir kimliğe roller atayabilecek bir Kullanıcı oluşturma](how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>Im-4: tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulama denetimleri kullanma

**Rehberlik**: Temsilcili müşteri kaynaklarına erişimi olacak kullanıcılar da dahil olmak üzere, yönettiğiniz kiracınızdaki tüm kullanıcılar için MULTI-Factor AUTHENTICATION (MFA) gerektir. Müşterilerinizin kiracılarında MFA 'yı da uygulamasına sormasını istediğinizi öneririz.

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>Im-5: hesap bozukluklarını Izleyin ve uyarır

**Kılavuz**: Azure AD aşağıdaki veri kaynaklarını sağlar: 

-   Oturum açma işlemleri – oturum açma işlemleri raporu, yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinliklerinin kullanımı hakkında bilgi sağlar.

-   Denetim günlükleri-Azure AD 'deki çeşitli özelliklerle yapılan tüm değişiklikler için Günlükler aracılığıyla izlenebilirlik sağlar. Günlüğe kaydedilen değişiklikler denetim günlüklerine örnek olarak Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma sayılabilir.

-   Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

-   Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Bu veri kaynakları Azure Izleyici, Azure Sentinel veya üçüncü taraf SıEM sistemleriyle tümleştirilebilir.

Azure Pthouse kullanan hizmet sağlayıcıları Azure AD günlüklerini Azure Sentinel 'e iletebilir ve hesap bozuklularını izlemek ve uyarmak için kiracılar genelinde uyarıları görüntüleyebilir/ayarlayabilir.

- [Azure AD 'de denetim etkinlik raporları](../active-directory/reports-monitoring/concept-audit-logs.md)

- [Azure AD riskli oturum açma işlemlerini görüntüleme](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Azure Sentinel çalışma alanlarını ölçekli olarak yönetme](how-to/manage-sentinel-workspaces.md)

- [Azure AD 'den Azure Sentinel 'e veri bağlama](../sentinel/connect-azure-active-directory.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="im-6-restrict-azure-resource-access-based-on-conditions"></a>IM-6: koşullara göre Azure kaynak erişimini kısıtlama

**Kılavuz**: Azure Aydınlathouse, temsilcili müşteri kaynakları için koşullu erişim özelliğini desteklemez. Yönetim kiracısında, belirli IP aralıklarından Multi-Factor Authentication (MFA) kullanılmasına yönelik Kullanıcı oturumu açma gerekliliği gibi Kullanıcı tanımlı koşullara göre daha ayrıntılı erişim denetimi için Azure AD koşullu erişimi kullanın. Ayrıntılı bir kimlik doğrulama oturumu yönetimi, farklı kullanım durumları için Azure AD koşullu erişim ilkesi aracılığıyla da kullanılabilir. 

Temsilcili müşteri kaynaklarına erişimi olacak kullanıcılar da dahil olmak üzere, yönettiğiniz kiracınızdaki tüm kullanıcılar için MFA gerektirmelisiniz. Müşterilerinizin kiracılarında MFA 'yı da uygulamasına sormasını istediğinizi öneririz.

- [Azure koşullu erişimine genel bakış](../active-directory/conditional-access/overview.md)

- [Sık kullanılan Koşullu Erişim İlkeleri](../active-directory/conditional-access/concept-conditional-access-policy-common.md)

- [Erişim Denetimi ile kimlik doğrulama oturum yönetimini yapılandırma](../active-directory/conditional-access/howto-conditional-access-session-lifetime.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="privileged-access"></a>Ayrıcalıklı erişim

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ayrıcalıklı erişim](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>PA-1: yüksek ayrıcalıklı kullanıcıları koruyun ve sınırlayın

**Rehberlik**: yüksek ayrıcalıklı kullanıcı hesabı sayısını sınırlayın ve bu hesapları yükseltilmiş bir düzeyde koruyun. Azure Aydınlathouse özelliğini etkinleştirmek ve kullanmak için genel yönetici hesabı gerekli değildir.

Kiracı düzeyindeki etkinlik günlüğü verilerine erişmek için bir hesaba, kök kapsamdaki (/) Izleme okuyucusu Azure yerleşik rolünün atanması gerekir. Kök kapsamdaki Izleme okuyucusu rolü geniş bir erişim düzeyi olduğundan, bu rolü tek bir kullanıcı veya gruba göre değil, bir hizmet sorumlusu hesabına atamanızı öneririz. Bu atama, ek yükseltilmiş erişimle genel yönetici rolüne sahip bir kullanıcı tarafından gerçekleştirilmelidir. Bu yükseltilmiş erişim, rol ataması yapılmadan önce hemen eklenmelidir, sonra atama tamamlandığında kaldırılır.

- [Azure AD 'de yönetici rolü izinleri](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

- [Kiracı düzeyindeki etkinlik günlüğü verilerini izlemek için erişim atama](how-to/monitor-delegation-changes.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="pa-2-restrict-administrative-access-to-business-critical-systems"></a>PA-2: iş açısından kritik sistemlere yönetici erişimini kısıtlama

**Kılavuz**: Azure ıthouse, iş açısından kritik sistemlere erişimi yalıtmak için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanır.

Kullanıcıların yalnızca belirli görevlerini gerçekleştirmek için gerekli izinlere sahip olması için en az ayrıcalık ilkesini izlediğinizden emin olun.

Active Directory Etki Alanı Denetleyicileri (DC 'Ler), güvenlik araçları ve iş açısından kritik sistemlerde yüklü aracılarla sistem yönetimi araçları gibi iş açısından kritik erişimlerinize yönetim erişimi olan yönetim, kimlik ve güvenlik sistemlerine erişimi de kısıtlatığınızdan emin olun. Bu yönetim ve güvenlik sistemlerine zarar veren saldırganlar, iş açısından kritik varlıkların güvenliğini tehlikeye atabilir.

Tutarlı erişim denetimi sağlamak için, tüm erişim denetimi türleri kurumsal segmentasyon stratejinize hizalanmalıdır.

- [Azure bileşenleri ve başvuru modeli](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Yönetim grubu erişimi](../governance/management-groups/overview.md#management-group-access)

- [Azure abonelik yöneticileri](../cost-management-billing/manage/add-change-subscription-administrator.md)

- [Azure ışıklı kullanımı için kullanıcıları ve rolleri tanımlamaya yönelik en iyi uygulamalar](concepts/tenants-users-roles.md#best-practices-for-defining-users-and-roles)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Kullanıcı erişimini düzenli olarak gözden geçirme ve mutabık kılma

**Kılavuz**: Azure ışıklı kullanım, kaynaklarını yönetmek için Azure Active Directory (Azure AD) hesapları kullanır, hesapların ve erişiminin geçerli olduğundan emin olmak için Kullanıcı hesaplarını ve erişim atamasını düzenli olarak gözden geçirin. Azure AD erişim incelemelerini, grup üyeliklerini ve kurumsal uygulamalara erişimi ve rol atamalarını gözden geçirmek için kullanabilirsiniz. Azure AD raporlama, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlayabilir. Gözden geçirme sürecini kolaylaştırmak için, erişim gözden geçirme raporu iş akışı oluşturmak için Azure AD Privileged Identity Management de kullanabilirsiniz.

Müşteriler, Azure portal Azure ışıklı aracılığıyla kiracı yönetimi aracılığıyla kullanıcılara verilen erişim düzeyini gözden geçirebilir. Bu erişimi istediğiniz zaman kaldırabilirler.

Ayrıca, Azure Privileged Identity Management, çok fazla sayıda yönetici hesabı oluşturulduğunda ve eski veya yanlış yapılandırılmış yönetici hesaplarını belirlemek için de yapılandırılabilir.

Not: bazı Azure Hizmetleri, Azure AD aracılığıyla yönetilmeyen yerel kullanıcıları ve rolleri destekler. Bu kullanıcıları ayrı ayrı yönetmeniz gerekir.

- [Privileged Identity Management (PıM) içinde Azure Kaynak rolleri için erişim gözden geçirmesi oluşturma](../active-directory/privileged-identity-management/pim-resource-roles-start-access-review.md) 

- [Bir temsilciye erişimi kaldırma](how-to/remove-delegation.md)

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](../active-directory/governance/access-reviews-overview.md)

- [Azure portal hizmet sağlayıcıları sayfasını görüntüleme](how-to/view-manage-service-providers.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="pa-4-set-up-emergency-access-in-azure-ad"></a>PA-4: Azure AD 'de acil durum erişimi ayarlama

**Kılavuz**: Azure açık thouse, kaynaklarını yönetmek için Azure Active Directory ile tümleşiktir. Yanlışlıkla Azure AD kuruluşunuzun dışına kilitlenmesini engellemek için, normal yönetim hesapları kullanılmazsa erişim için bir acil durum erişim hesabı ayarlayın. Acil durum erişim hesapları genellikle yüksek ayrıcalıklı olur ve belirli kişilere atanmamalıdır. Acil durum erişim hesapları, normal yönetim hesaplarının kullanılabileceği acil durum veya "cam camı" senaryolarıyla sınırlıdır.

Acil durum erişim hesapları için kimlik bilgilerinin (parola, sertifika veya akıllı kart gibi) güvenli tutulduğundan ve yalnızca bir acil durumda bunları kullanma yetkisine sahip olan bireyler için bilindiğinden emin olmanız gerekir.

- [Azure AD 'de acil durum erişim hesaplarını yönetme](/azure/active-directory/users-groups-roles/directory-emergency-access)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="pa-5-automate-entitlement-management"></a>PA-5: yetkilendirme yönetimini otomatikleştirme 

**Kılavuz**: Azure Aydınlathouse, kaynaklarını yönetmek için Azure Active Directory (Azure AD) ile tümleşiktir. Erişim atamaları, incelemeler ve süre sonu dahil olmak üzere, erişim isteği iş akışlarını otomatikleştirmek için Azure AD yetkilendirme yönetimi özelliklerini kullanın. Çift veya çok aşamalı onay de desteklenir.

- [Azure AD erişim gözden geçirmeleri nelerdir?](../active-directory/governance/access-reviews-overview.md) 

- [Azure AD yetkilendirme yönetimi nedir?](../active-directory/governance/entitlement-management-overview.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="pa-6-use-privileged-access-workstations"></a>PA-6: ayrıcalıklı erişim iş istasyonlarını kullanma

**Rehberlik**: güvenli, yalıtılmış iş istasyonları Yöneticiler, geliştiriciler ve kritik hizmet işleçleri gibi hassas rollerin güvenliği açısından kritik öneme sahiptir. Gereksinimlerinize bağlı olarak, üretim ortamlarında Azure açık Thouse ile yönetim görevlerini gerçekleştirmek için yüksek düzeyde güvenli Kullanıcı iş istasyonları ve/veya Azure savunma kullanabilirsiniz. Yönetim görevleri için güvenli ve yönetilen bir kullanıcı iş istasyonu dağıtmak üzere Azure Active Directory, Microsoft Defender Gelişmiş tehdit koruması (ATP) ve/veya Microsoft Intune kullanın. Güvenli iş istasyonları, güçlü kimlik doğrulaması, yazılım ve donanım temelleri ve kısıtlı mantıksal ve ağ erişimi gibi güvenli yapılandırmayı zorlamak için merkezi olarak yönetilebilir. 

- [Ayrıcalıklı erişim iş istasyonlarını anlama](../active-directory/devices/concept-azure-managed-workstation.md)

- [Ayrıcalıklı erişim iş istasyonu dağıtma](../active-directory/devices/howto-azure-managed-workstation.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: yalnızca yeterli yönetim (en az ayrıcalık ilkesi) Izleyin 

**Kılavuz**: Azure Aydınlathouse, kaynaklarını yönetmek için Azure rol tabanlı erişim denetımı (RBAC) ile tümleşiktir. Azure RBAC, rol atamaları aracılığıyla Azure kaynak erişimini yönetmenizi sağlar. Kullanıcılar, gruplar, hizmet sorumluları ve yönetilen kimliklere bu yerleşik rolleri atayabilirsiniz. Belirli kaynaklar için önceden tanımlı yerleşik roller vardır ve bu roller Azure CLı, Azure PowerShell veya Azure portal gibi araçlarla envantere alınabilir veya sorgulanabilir. Azure RBAC aracılığıyla kaynaklara atadığınız ayrıcalıkların her zaman roller için gerekli olan ile sınırlı olması gerekir. Bu, Azure AD Privileged Identity Management (PıM) ' nin tam zamanında (JıT) yaklaşımını tamamlar ve düzenli olarak incelenmelidir. İzin ayırmak için yerleşik roller kullanın ve gerektiğinde yalnızca özel roller oluşturun.

Azure kathouse, Azure yerleşik rollerini kullanarak Temsilcili müşteri kaynaklarına erişim sağlar. Çoğu durumda, bu rolleri birçok bireysel kullanıcı hesabı yerine bir grup veya hizmet sorumlusuna atamak isteyeceksiniz. Bu, erişim gereksinimleriniz değiştiğinde planı güncelleştirmek ve yeniden yayınlamak zorunda kalmadan bireysel kullanıcılar için erişim eklemenize veya kaldırmanıza olanak sağlar.

Müşteri kaynaklarını bir yönetim kiracısına devretmek için bir dağıtımın, eklendi olan abonelik (veya eklendi olan kaynak gruplarını içeren) için sahip yerleşik rolüne sahip olan, müşterinin kiracısındaki Konuk olmayan bir hesap tarafından yapılması gerekir.

- [Azure açık Thouse kiracılarını, kullanıcıları ve rolleri anlayın](concepts/tenants-users-roles.md)

- [En az ayrıcalık ilkesini Azure ışıklı kullanım için uygulama](concepts/recommended-security-practices.md#assign-permissions-to-groups-using-the-principle-of-least-privilege)

- [Azure rol tabanlı erişim denetimi nedir (Azure RBAC)](../role-based-access-control/overview.md) 

- [Azure AD kimlik ve erişim gözden geçirmeleri kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

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

### <a name="am-2-ensure-security-team-has-access-to-asset-inventory-and-metadata"></a>-2: güvenlik ekibinin varlık envanterini ve meta verileri erişimine sahip olduğundan emin olun

**Rehberlik**: müşterilerin güvenlik takımları, Azure açık thouse kullanan hizmet sağlayıcılarının yaptığı etkinlikleri görmek için etkinlik günlüklerini gözden geçirebilir. 

Bir hizmet sağlayıcı, güvenlik ekibinin Temsilcili müşteri kaynaklarını incelemesine izin vermek isterse, güvenlik ekibinin yetkilendirmeleri okuyucu yerleşik rolünü içermelidir.

- [Müşterinin hizmet sağlayıcısı etkinliğini nasıl gözden geçirebilirler](how-to/view-service-provider-activity.md)

- [Azure 'da bir müşteriyi kullanıma ekleme sırasında rolleri belirtme](how-to/onboard-customer.md#define-roles-and-permissions)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="am-3-use-only-approved-azure-services"></a>HAR-3: yalnızca onaylanan Azure hizmetlerini kullanma

**Rehberlik**: kullanıcıların ortamınızda hangi hizmetleri sağlayabileceğini denetlemek ve kısıtlamak Için Azure İlkesi ' ni kullanın. Azure Kaynak Grafiği 'ni kullanarak aboneliklerindeki kaynakları sorgulama ve bulma. Onaylanmamış bir hizmet algılandığında uyarıları tetiklemeye yönelik kurallar oluşturmak için Azure Izleyici 'yi de kullanabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/built-in-policies.md#general) 

- [Azure Kaynak Grafiği Gezgini ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>ÖÖ-4: varlık yaşam döngüsü yönetiminin güvenliğinin yapıldığından emin olun

**Rehberlik**: artık gerekli olmadıklarında Azure açık thouse tarafından temsilci seçilmiş kaynaklara erişimi kaldırın, böylece hizmet sağlayıcılarının artık erişimi olmaz. Erişim, müşteri ya da hizmet sağlayıcısı tarafından kaldırılabilir. 

- [Temsilci erişimini kaldırma](how-to/remove-delegation.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="am-5-limit-users-ability-to-interact-with-azure-resource-manager"></a>ÖÖ-5: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Kılavuz**: Azure Aydınlathouse, kimlik ve kimlik doğrulama için Azure Active Directory (Azure AD) ile tümleşiktir. "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure kaynakları yöneticisi ile etkileşime geçmesini sınırlamak için Azure koşullu erişimi kullanabilirsiniz.

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="logging-and-threat-detection"></a>Günlüğe kaydetme ve tehdit algılama

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-1-enable-threat-detection-for-azure-resources"></a>LT-1: Azure kaynakları için tehdit algılamayı etkinleştirme

**Kılavuz**: Azure ışıklı kullanım sayesinde müşterilerinizin Azure kaynaklarını olası tehditler ve bozukluklar için izleyebilirsiniz. Analistlerin sıralama için yanlış pozitif durumları azaltmaya yönelik yüksek kaliteli uyarılar almaya odaklanın. Uyarılar, günlük verilerinden, aracılardan veya diğer verilerden kaynaklıdır.

Azure hizmet telemetrisini izlemeyi ve hizmet günlüklerini çözümlemeyi temel alan Azure Güvenlik Merkezi 'nin yerleşik tehdit algılama özelliğini kullanın. Veriler, sistemden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve analiz için verileri çalışma alanınıza kopyalayan Log Analytics Aracı kullanılarak toplanır. 

Ayrıca, müşteri ortamınızda belirli ölçütlerle eşleşen tehditleri sağlayan analiz kuralları oluşturmak için Azure Sentinel ' i kullanın. Kurallar, her olayı araştırmak için ölçütler eşleştiğinde olaylar oluşturur. Azure Sentinel, tehdit algılama yeteneklerini geliştirmek için üçüncü taraf tehdit bilgilerini de içeri aktarabilir. 

- [Azure Güvenlik Merkezi’nde tehdit koruması](/azure/security-center/threat-protection)

- [Azure Güvenlik Merkezi güvenlik uyarıları başvuru kılavuzu](../security-center/alerts-reference.md)

- [Tehditleri algılamak için özel analiz kuralları oluşturma](../sentinel/tutorial-detect-threats-custom.md)

- [Azure Sentinel çalışma alanlarını ölçekli olarak yönetme](how-to/manage-sentinel-workspaces.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Azure kimlik ve erişim yönetimi için tehdit algılamayı etkinleştirme

**Kılavuz**: Azure Dıthouse sayesinde, çok sayıda başarısız kimlik doğrulama girişimi ve abonelikteki kullanım dışı hesaplar gibi, yönettiğiniz müşteri kiracılarında belirli şüpheli etkinliklerle ilgili uyarı almak Için Azure Güvenlik Merkezi 'ni kullanabilirsiniz.

Azure Active Directory (Azure AD), daha karmaşık izleme ve analiz kullanım örnekleri için Azure AD raporlama 'da görüntülenebilen veya Azure Izleyici, Azure Sentinel veya diğer SıEM/izleme araçlarıyla tümleştirilen aşağıdaki kullanıcı günlüklerini sağlar:
- Oturum açma-oturum açma raporu, yönetilen uygulamaların kullanımı ve Kullanıcı oturum açma etkinliklerinin kullanımı hakkında bilgi sağlar.
- Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Denetim günlüklerine örnek olarak, Azure AD 'de Kullanıcı, uygulama, Grup, rol ve ilke ekleme veya kaldırma gibi herhangi bir kaynak üzerinde yapılan değişiklikler bulunur.
- Riskli oturum açma-riskli oturum açma, bir kullanıcı hesabının meşru sahibi olmayan birisi tarafından gerçekleştirilmiş olabilecek oturum açma girişimine yönelik bir göstergedir.
- Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Azure Güvenlik Merkezi, çok fazla sayıda başarısız kimlik doğrulama denemesi, aboneliğin kullanım dışı bırakılmış hesaplar gibi bazı şüpheli etkinlikler üzerinde de uyarı verebilir. Azure Güvenlik Merkezi 'nin tehdit koruması modülü, temel güvenlik durumu izlemeye ek olarak, bireysel Azure işlem kaynakları (sanal makineler, kapsayıcılar, App Service), veri kaynakları (SQL DB ve depolama) ve Azure hizmet katmanlarından daha ayrıntılı güvenlik uyarıları da toplayabilirler. Bu özellik, bireysel kaynakların içindeki hesap anormallikleri hakkında görünürlük sağlar. 

- [Azure Pthouse ile geliştirilmiş hizmetler ve senaryolar](concepts/cross-tenant-management-experience.md#enhanced-services-and-scenarios)

- [Azure Active Directory Denetim etkinliği raporları](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Azure kimlik koruması 'nı etkinleştirme](../active-directory/identity-protection/overview-identity-protection.md) 

- [Azure Güvenlik Merkezi’nde tehdit koruması](/azure/security-center/threat-protection)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="lt-4-enable-logging-for-azure-resources"></a>LT-4: Azure kaynakları için günlüğe kaydetmeyi etkinleştirme

**Kılavuz**: otomatik olarak kullanılabilen etkinlik günlükleri, okuma IŞLEMLERI (Get) dışında Azure açık thouse kaynaklarınız için tüm yazma IŞLEMLERINI (put, Post, Delete) içerir. Etkinlik günlükleri, sorun giderirken bir hata bulmak veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizi izlemek için kullanılabilir.

Azure Use ile Azure Izleyici günlüklerini yönettiğiniz müşteri kiracılarında ölçeklenebilir bir şekilde kullanabilirsiniz. Müşteri verileri, kendilerine aktarılması yerine kiracılarında kalacak şekilde doğrudan müşteri kiracılarında Log Analytics çalışma alanları oluşturun. Bu Ayrıca, Log Analytics tarafından desteklenen herhangi bir kaynak ve hizmetin merkezi olarak izlenmesini sağlar ve bu sayede izlediğiniz veri türlerine daha fazla esneklik sunar.

Azure açık bir kullanım için abonelikleri temsilci seçen müşteriler, gerçekleştirilen tüm eylemleri görmek için Azure etkinlik günlüğü verilerini görüntüleyebilir. Bu, müşterilere hizmet sağlayıcılarının yaptığı işlemlere ve müşterinin kendi Azure Active Directory (Azure AD) kiracısındaki kullanıcılar tarafından gerçekleştirilen işlemlere tam görünürlük sağlar.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md) 

- [Azure 'da günlüğe kaydetme ve farklı günlük türlerini anlama](../azure-monitor/platform/platform-logs-overview.md)

- [Temsil edilen kaynakları ölçeklendirmeye göre izleme](how-to/monitor-at-scale.md)

- [Hizmet sağlayıcısı etkinliğini görüntüleme](how-to/view-service-provider-activity.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="lt-5-centralize-security-log-management-and-analysis"></a>LT-5: güvenlik günlüğü yönetimini ve analizini merkezileştirme

**Rehberlik**: bağıntı sağlamak için günlük depolamayı ve çözümlemeyi merkezileştirme. Her günlük kaynağı için bir veri sahibi, erişim Kılavuzu, depolama konumu, verileri işlemek ve erişmek için kullanılan araçları ve veri saklama gereksinimlerini atadığınızdan emin olun.

Azure etkinlik günlüklerini merkezi günlüklerinizi tümleştirdiğinizden emin olun. Endpoint cihazları, ağ kaynakları ve diğer güvenlik sistemleri tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlük alma. Azure Izleyici 'de analiz sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanlarını kullanın ve uzun süreli ve arşiv depolama için Azure depolama hesaplarını kullanın.

Ayrıca, Azure Sentinel 'e veya bir üçüncü taraf SıEM 'e verileri etkinleştirin ve ekleyin.

Azure Use ile Azure Izleyici günlüklerini yönettiğiniz müşteri kiracılarında ölçeklenebilir bir şekilde kullanabilirsiniz. Müşteri verileri, kendilerine aktarılması yerine kiracılarında kalacak şekilde doğrudan müşteri kiracılarında Log Analytics çalışma alanları oluşturun. Bu Ayrıca, Log Analytics tarafından desteklenen herhangi bir kaynak ve hizmetin merkezi olarak izlenmesini sağlar ve bu sayede izlediğiniz veri türlerine daha fazla esneklik sunar.

Azure açık bir kullanım için abonelikleri temsilci seçen müşteriler, gerçekleştirilen tüm eylemleri görmek için Azure etkinlik günlüğü verilerini görüntüleyebilir. Bu, müşterilere hizmet sağlayıcılarının yaptığı işlemlere ve müşterinin kendi Azure Active Directory (Azure AD) kiracısındaki kullanıcılar tarafından gerçekleştirilen işlemlere tam görünürlük sağlar.

Birçok kuruluş, daha az sıklıkta kullanılan "soğuk" verileri için sık kullanılan ve Azure depolama için Azure Sentinel ' i "etkin" veriler için kullanmayı seçer.

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

- [Temsil edilen kaynakları ölçeklendirmeye göre izleme](how-to/monitor-at-scale.md)

- [Müşterilerin hizmet sağlayıcısı etkinliğini nasıl görüntüleyebilirler](how-to/view-service-provider-activity.md)

- [Azure Sentinel çalışma alanlarını ölçekli olarak yönetme](how-to/manage-sentinel-workspaces.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="lt-6-configure-log-storage-retention"></a>LT-6: günlük depolama bekletmesini yapılandırma

**Kılavuz**: Azure Mathouse Şu anda güvenlikle ilgili hiçbir günlük oluşturmuyor. Hizmet sağlayıcısı etkinliğini görüntülemek isteyen müşteriler, uyumluluk, düzenleme ve iş gereksinimlerine göre günlük bekletmeyi yapılandırabilir. 

Azure Izleyici 'de, Log Analytics çalışma alanı saklama dönemini kuruluşunuzun uyumluluk düzenlemelerine göre ayarlayabilirsiniz. Uzun süreli ve arşiv depolama için Azure depolama, Data Lake veya Log Analytics çalışma alanı hesapları kullanın.

- [Log Analytics veri saklama süresini değiştirme](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](../storage/common/storage-monitor-storage-account.md#configure-logging)

- [Azure Güvenlik Merkezi uyarıları ve öneriler dışa aktarma](../security-center/continuous-export.md) yapılandırması, müşteri herhangi bir günlük bekletme kuramıyor.

- [Müşteri, hizmet sağlayıcılarının etkinlik günlüğü verilerini nasıl gözden geçirebilir?](how-to/view-service-provider-activity.md)

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

Yüksek kaliteli uyarılar, geçmiş olaylar, doğrulanan topluluk kaynakları ve uyarılar oluşturmak ve temizlemek için tasarlanan araçlar ve farklı sinyal kaynaklarını kullanarak oluşturulabilir. 

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

### <a name="pv-1-establish-secure-configurations-for-azure-services"></a>BD-1: Azure hizmetleri için güvenli yapılandırma oluşturma 

**Kılavuz**: Azure Mathouse, Azure kaynaklarınızın yapılandırmasını denetlemek ve zorlamak Için Azure Güvenlik Merkezi 'nde mevcut olan hizmete özgü ilkelerin altına daha fazla destek sağlar. Bu, Azure Güvenlik Merkezi veya Azure Ilke girişimleri içinde yapılandırılabilir.

- Kiracı kimliklerinin Azure Dıthouse aracılığıyla eklenmesine izin ver

- Bir yönetim kiracısına kapsamları yetkilendirmeyi denetleme

Tek bir şema tanımında Azure Resource Manager şablonları, Azure RBAC denetimleri ve ilkeleri de dahil olmak üzere hizmetlerin ve uygulama ortamlarının dağıtımını ve yapılandırmasını otomatik hale getirmek için Azure şemaları ' nı kullanabilirsiniz.

- [Azure açık kullanım Ilkeleri](samples/policy-reference.md)

- [Öğretici-uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md) 

- [Azure Blueprints](../governance/blueprints/overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="pv-2-sustain-secure-configurations-for-azure-services"></a>BD-2: Azure hizmetleri için güvenli yapılandırmalara dayanmalar

**Kılavuz**: Azure Mathouse, Azure kaynaklarınızın yapılandırmasını denetlemek ve zorlamak Için Azure Güvenlik Merkezi 'nde mevcut olan hizmete özgü ilkelerin altına daha fazla destek sağlar. Bu, Azure Güvenlik Merkezi veya Azure Ilke girişimleri içinde yapılandırılabilir.

- [Azure açık kullanım Ilkeleri](samples/policy-reference.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="pv-3-establish-secure-configurations-for-compute-resources"></a>BD-3: işlem kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: VM 'ler, kapsayıcılar ve diğerleri dahil tüm işlem kaynaklarında güvenli konfigürasyonlar oluşturmak Için Azure Güvenlik Merkezi 'Ni ve Azure ilkesini kullanın.

- [Azure Güvenlik Merkezi önerilerini izleme](../security-center/security-center-recommendations.md) 

- [Güvenlik önerileri - başvuru kılavuzu](../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

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

**Rehberlik**: kimlik, ağ, uygulama, abonelik, yönetim grubu ve diğer denetimlerin birleşimini kullanarak varlıklara erişimi parçalara ayırma konusunda kurumsal çapta bir strateji oluşturun.

Güvenlik ayrımı gereksinimini, birbirleriyle iletişim kurması gereken sistemlerin günlük işlemlerini etkinleştirme gereksinimiyle dikkatle dengeleyin ve verilere erişin.

Segmentleme stratejisinin ağ güvenliği, kimlik ve erişim modelleri ve uygulama izni/erişim modelleri ve insan işlem denetimleri gibi denetim türleri arasında tutarlı bir şekilde uygulandığından emin olun.

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

**Rehberlik**: kuruluşunuzun genel güvenlik erişimi denetim stratejisinin bir parçası olarak Azure kimliği ve ayrıcalıklı erişim yaklaşımları oluşturun.  

Bu strateji aşağıdaki öğeler için belgelenmiş kılavuz, ilke ve standartları içermelidir: 

-   Merkezi bir kimlik ve kimlik doğrulama sistemi ve diğer iç ve dış kimlik sistemleriyle karşılıklı bağlantısı

-   Farklı kullanım durumlarında ve koşullarda güçlü kimlik doğrulama yöntemleri

-   Yüksek ayrıcalıklı kullanıcıların korunması

-   Anomali Kullanıcı etkinlikleri izleme ve işleme  

-   Kullanıcı kimliği ve erişim gözden geçirme ve mutabakat süreci

Daha fazla bilgi için aşağıdaki başvurulara bakın:

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
