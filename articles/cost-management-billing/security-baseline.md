---
title: Maliyet Yönetimi için Azure güvenlik temeli
description: Maliyet Yönetimi güvenlik temeli Azure Güvenlik Karşılaştırması’nda belirtilen güvenlik önlemlerini uygulamaya yönelik yordam rehberini ve kaynakları sağlar.
author: msmbaldwin
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 11/16/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 40baab2a4c577ee96bcebeb4880235a81810b2b4
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94660330"
---
# <a name="azure-security-baseline-for-cost-management"></a>Maliyet Yönetimi için Azure güvenlik temeli

Bu güvenlik temeli, [Azure Güvenlik Karşılaştırması sürüm 2.0](../security/benchmarks/overview.md)’ın güvenlik temelini Azure Maliyet Yönetimi’ne uygular. Azure Güvenlik Karşılaştırması Azure’da bulut çözümlerinizin güvenliğini nasıl sağlayabileceğinize ilişkin öneriler getirir. İçerik, Azure Güvenlik Karşılaştırması tarafından tanımlanan **güvenlik denetimlerine** ve Maliyet Yönetimi için kullanılabilen ilgili rehbere göre gruplandırılmıştır. Maliyet Yönetimi’nde geçerli olmayan **denetimler** hariç tutulmuştur.

Maliyet Yönetimi’nin Azure Güvenlik Karşılaştırması’yla tam olarak eşleşmesini görmek için [Maliyet Yönetimi güvenlik temeli eksiksiz eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines) bakın.

## <a name="identity-management"></a>Kimlik Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Kimlik Yönetimi](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>KY-1: Azure Active Directory’yi merkezi kimlik ve kimlik doğrulaması sistemi olarak standartlaştırma

**Rehber**: Azure Maliyet Yönetimi, Azure’ın varsayılan kimlik ve erişim yönetim hizmeti olan Azure Active Directory (Azure AD) ile tümleştirilmiştir. Kuruluşunuzun şu kaynaklarda kimlik ve erişim yönetimini idare etmek için Azure AD’yi standartlaştırmanız gerekir:

- Microsoft bulut kaynakları. Örneğin Azure portal, Azure Depolama, Azure Sanal Makineler (Linux ve Windows), Azure Key Vault, PaaS ve SaaS uygulamaları.

- Kuruluşunuzun kaynakları. Örneğin Azure’daki uygulamalar veya kurumsal ağ kaynaklarınız.

Kuruluşunuzun bulut güvenliği uygulamasında Azure AD’nin güvenliğini sağlamaya yüksek öncelik verilmelidir. Microsoft’un en iyi yöntem önerilerine göre kendi kimlik güvenliği duruşunuzu değerlendirmenize yardımcı olmak için Azure AD bir kimlik güvenliği puanı sağlar. Bu puanı kullanarak yapılandırmanızın en iyi yöntem önerileriyle ne kadar uyumlu olduğunu ölçebilir ve güvenlik duruşunuzda geliştirmeler yapabilirsiniz.

Not: Azure AD dış kimlik sağlayıcılarını desteklediğinden, Microsoft hesabı olmayan kullanıcıların dış kimlikleriyle uygulamalarında ve kaynaklarında oturum açmasına izin verir.

- [Azure AD’de kiracılık](../active-directory/develop/single-and-multi-tenant-apps.md)

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Azure AD kiracılarını tanımlama](https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/)  

- [Bir uygulama için dış kimlik sağlayıcılarını kullanma](/azure/active-directory/b2b/identity-providers)

- [Azure AD'de kimlik güvenliği puanı nedir?](../active-directory/fundamentals/identity-secure-score.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>KY-3: Uygulama erişiminde Azure AD çoklu oturum açmayı (SSO) kullanma

**Rehber**: Azure Maliyet Yönetimi Azure kaynakları, bulut uygulamaları ve şirket içi uygulamalarda kimlik ve erişim yönetimi sağlamak için Azure Active Directory’yi kullanır. Bu hem çalışanlar gibi kurumsal kimlikleri hem de iş ortakları, satıcılar ve sağlayıcılar gibi dış kimlikleri içerir. Bu sayede kuruluşunuzun şirket içinde ve buluttaki verilerini ve kaynaklarını yönetmek ve güvenliklerini sağlamak için çoklu oturum açma (SSO) kullanılabilir. Sorunsuz, güvenli erişim, daha fazla görünürlük ve denetim için tüm kullanıcılarınızı, uygulamalarınızı ve cihazlarınızı Azure AD’ye bağlayın.

- [Azure AD ile Uygulama SSO’yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>KY-4: Tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulaması denetimlerini kullanma

**Rehber**: Azure Maliyet Yönetimi çok faktörlü kimlik doğrulaması (MFA) ve güçlü parolasız yöntemler aracılığıyla güçlü kimlik doğrulaması denetimlerini destekleyen Azure AD ile tümleştirilmiştir.

- Çok faktörlü kimlik doğrulaması - Azure AD MFA’yı etkinleştirin ve MFA kurulumunuzda izleyebileceğiniz en iyi yöntemlerden bazıları için Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerine uyun. MFA oturum açma koşulları ve risk faktörleri temelinde tüm kullanıcılarda, seçilen kullanıcılarda veya tek tek kullanıcılar düzeyinde zorunlu tutulabilir.

- Parolasız kimlik doğrulaması – Üç parolasız kimlik doğrulaması seçeneği kullanılabilir: İş İçin Windows Hello, Microsoft Authenticator uygulaması ve akıllı kartlar gibi şirket içi kimlik doğrulama yöntemleri.

Yöneticiler ve ayrıcalıklı kullanıcılar için en yüksek düzeyde güçlü kimlik doğrulama yönteminin kullanıldığından emin olun ve bunun ardından uygun güçlü kimlik doğrulama ilkesini diğer kullanıcılara da dağıtın.

- [Azure’da MFA’yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Azure Active Directory için parolasız kimlik doğrulaması seçeneklerine giriş](../active-directory/authentication/concept-authentication-passwordless.md) 

- [Azure AD varsayılan parola ilkesi](../active-directory/authentication/concept-sspr-policy.md#password-policies-that-only-apply-to-cloud-user-accounts)

- [Azure AD Parola Koruması kullanarak hatalı parolaları ortadan kaldırma](../active-directory/authentication/concept-password-ban-bad.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>KY-5: Hesap anomalilerini izleme ve uyarı verme

**Rehber**: Azure Maliyet Yönetimi aşağıdaki veri kaynaklarını sağladığı Azure Active Directory ile tümleştirilmiştir:

- Oturum açma bilgileri - Oturum açma bilgileri raporu yönetilen uygulamaların kullanımı ve kullanıcıların oturum açma etkinlikleri hakkında bilgi sağlar.

- Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Azure AD içindeki herhangi bir kaynakta yapılan kullanıcı, uygulama, grup, rol ve ilkeleri ekleme veya kaldırma işlemleri gibi değişiklikler, denetim günlüklerine örnek gösterilebilir.

- Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

- Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Bu veri kaynakları Azure İzleyici, Azure Sentinel veya üçüncü taraf SIEM sistemleriyle tümleştirilebilir.

Azure Güvenlik Merkezi aşırı fazla sayıda başarısız kimlik doğrulaması girişimi, abonelikte kullanım dışı kalan hesaplar gibi bazı şüpheli etkinliklerde de uyarı verebilir.

Azure Gelişmiş Tehdit Koruması (ATP) gelişmiş tehditleri, risk altındaki kimlikleri ve içeriden kötü amaçlı eylemleri belirlemek, algılamak ve araştırmak için Active Directory sinyallerini kullanabilen bir güvenlik çözümüdür.

- [Azure Active Directory’de denetim etkinliği raporları](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Azure AD riskli oturum açma işlemlerini görüntüleme](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Riskli etkinlik bayrağıyla işaretlenen Azure AD kullanıcılarını belirleme](/azure/active-directory/reports-monitoring/concept-user-at-risk) 

- [Azure Güvenlik Merkezi’nde kullanıcıların kimliğini ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md) 

- [Azure Güvenlik Merkezi’nin tehdit analizi koruma modülündeki uyarılar](../security-center/alerts-reference.md) 

- [Azure Etkinlik Günlüklerini Azure İzleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="privileged-access"></a>Ayrıcalıklı Erişim

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Ayrıcalıklı Erişim](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>AE-1: Yüksek ayrıcalıklı kullanıcıları koruma ve sınırlama

**Rehber**: Kurumsal Anlaşmalar (EA) için Maliyet Yönetimi’nin aşağıdaki yüksek ayrıcalıklı hesabı vardır.

- Kuruluş Yöneticisi

Kurumsal Anlaşmanız (EA) kapsamında belirli rollere atanan kullanıcıların düzenli olanak gözden geçirilmesi önerilir.

Ayrıca bir kural olarak yüksek öncelikli hesapların veya rollerin sayısının sınırlanması ve bu hesapların yükseltilmiş bir düzeyde korunması önerilir. Çünkü bu ayrıcalığa sahip kullanıcılar Azure ortamınızdaki her kaynağı doğrudan veya dolaylı olarak okuyabilir ve değiştirebilir.

Azure AD Privileged Identity Management’ı (PIM) kullanarak Azure kaynaklarına ve Azure AD’ye tam zamanında (JIT) ayrıcalıklı erişimi etkinleştirebilirsiniz. JIT yalnızca kullanıcıların ihtiyacı olduğunda ayrıcalıklı görevler gerçekleştirmeleri için geçici izinler verir. Azure AD kuruluşunuzda güvenli olmayan veya şüpheli etkinlikler olduğunda da PIM güvenlik uyarıları oluşturabilir.

- [Azure kurumsal rollerini yönetme](manage/understand-ea-roles.md) 

- [Azure AD'de yönetici rolü izinleri](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 

- [Azure Privileged Identity Management güvenlik uyarılarını kullanma](../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md) 

- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>AE-3: Kullanıcı erişimini düzenli olarak gözden geçirme ve uzlaştırma

**Rehber**: Azure Maliyet Yönetimi kurumsal maliyet verilerini görüntülemek ve yönetmek için uygun erişime bağlıdır. Erişim abonelik düzeyinde Azure rol tabanlı erişim denetimiyle (Azure RBAC) ve faturalama kapsamları için Kurumsal Anlaşmalar (EA) veya Microsoft Müşteri Sözleşmesi (MCA) ile sağlanan yönetim rolleriyle denetlenir. Kullanıcıların ve grupların gerekli erişime sahip olduğundan emin olmak için verilen erişimi düzenli aralıklarla denetleyin ve gözden geçirin.

- [Azure için faturalama bilgilerine erişimi yönetme](manage/manage-billing-access.md)

- [Maliyet Yönetimi verilerine erişim atama](costs/assign-access-acm-data.md)

- [Azure kurumsal rollerini yönetme](manage/understand-ea-roles.md)

- [Azure’daki Microsoft Müşteri Sözleşmesi yönetici rollerini anlama](manage/understand-mca-roles.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>AE-7: Tam yetecek kadar yönetim uygulama (en düşük ayrıcalık ilkesi) 

**Rehber**: Azure Maliyet Yönetimi kaynakları (bütçeler, kaydedilmiş raporlar gibi) yönetmek için Azure rol tabanlı erişim denetimi (RBAC) ile tümleştirilmiştir. Azure RBAC ağ atamaları aracılığıyla Azure kaynak erişimini yönetmenize olanak tanır. Bu rolleri kullanıcılara, gruplara ve hizmet sorumlularına atayabilirsiniz. Belirli kaynaklar için önceden tanımlanmış yerleşik roller vardır ve Azure CLI, Azure PowerShell veya Azure portal gibi araçlardan yararlanarak bu rollerin envanteri çıkarılabilir ve bunlar sorgulanabilir. Azure RBAC aracılığıyla kaynaklara atadığınız ayrıcalıklar her zaman yalnızca söz konusu rollere gerekenlerle sınırlanmalıdır. Bu yöntem Azure AD Privileged Identity Management’ın (PIM) tam zamanında (JIT) yaklaşımını tamamlayıcı niteliktedir ve düzenli aralıklarla gözden geçirilmelidir.

Yerleşik rolleri kullanarak izin verin ve yalnızca gerektiğinde özel rol oluşturun.

Azure Maliyet Yönetimi yerleşik roller, okuyucular ve katkıda bulunanlar sağlar.

- [Azure Maliyet Yönetimi Okuyucusu](../role-based-access-control/built-in-roles.md#cost-management-reader)

- [Azure Maliyet Yönetimi Katkıda Bulunanı](../role-based-access-control/built-in-roles.md#cost-management-contributor)

Azure rol tabanlı erişim denetimi (Azure RBAC) nedir? ../role-based-access-control/overview.md 

- [Azure’da RBAC’yi yapılandırma](../role-based-access-control/role-assignments-portal.md) 

- [Azure AD kimlik ve erişim gözden geçirmelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Veri Koruma](/azure/security/benchmarks/security-controls-v2-data-protection).*

### <a name="dp-1-discovery-classify-and-label-sensitive-data"></a>VK-1: Hassas verileri bulma, sınıflandırma ve etiketleme

**Rehber**: Uygun denetimleri tasarlayarak kuruluşun teknoloji sistemleri tarafından hassas bilgilerin güvenle depolandığından, işlendiğinden ve iletildiğinden emin olabilmeniz için hassas verilerinizi bulmanız, sınıflandırmanız ve etiketlemeniz önerilir.

Azure’daki, şirket içindeki, Office 365’teki ve diğer konumlardaki hassas bilgiler için Azure Information Protection’ı (ve ilişkili tarama aracını) kullanın.

Azure SQL Information Protection’ı kullanarak Azure SQL Veritabanlarında depolanan bilgilerin sınıflandırılmasına ve etiketlenmesine yardımcı olabilirsiniz.

- [Azure Information Protection kullanarak hassas bilgileri etiketleme](/azure/information-protection/what-is-information-protection) 

- [Azure SQL Veri Bulma’yı uygulama](/azure/sql-database/sql-database-data-discovery-and-classification)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="dp-2-protect-sensitive-data"></a>VK-2: Hassas verileri koruma

**Rehber**: Azure Rol Tabanlı Erişim Denetimi’ni (Azure RBAC), ağ tabanlı erişim denetimlerini ve Azure hizmetlerindeki belirli denetimleri (SQL ve diğer veritabanlarındaki şifreleme gibi) kullanarak erişimi kısıtlayıp hassas verileri korumanız önerilir.

Tutarlı erişim denetimi sağlamak için tüm erişim denetimi türlerinin kurumsal segmentasyon stratejinizle uyumlu olması gerekir. Ayrıca kurumsal segmentasyon stratejisi, hassas ve iş açısından kritik verilerle sistemlerin konumu hakkında da bilgilendirilmelidir.

Microsoft tarafından yönetilen temel platform açısından, Microsoft tüm müşteri içeriğini hassas olarak değerlendirir ve müşteri verilerinin kaybına ve açığa çıkmasına karşı koruma sağlar. Azure içindeki müşteri verilerinin güvenlik altında kalmasını sağlamak için Microsoft bazı varsayılan veri koruma denetimleri ve özellikleri uygulamıştır.

- [Maliyet Yönetimi verilerine erişim atama](costs/assign-access-acm-data.md)

- [Azure Rol Tabanlı Erişim Denetimi (RBAC)](../role-based-access-control/overview.md) 

- [Azure’da müşteri verilerinin korunmasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="dp-3-monitor-for-unauthorized-transfer-of-sensitive-data"></a>VK-3: Hassas verilerin yetkisiz aktarımını izleme

**Rehber**: Kurumsal görünürlüğün ve denetimin dışındaki konumlara yetkisiz veri aktarımlarını izleyin. Bu normalde yetkisiz veri sızdırmaya işaret ediyor olabilecek anormal etkinliklerin (büyük veya alışılmamış aktarımlar) izlenmesini içerir.

Azure Depolama Gelişmiş Tehdit Koruması (ATP) ve Azure SQL ATP, hassas bilgilerin yetkisiz aktarımlarına işaret ediyor olabilecek anormal bilgi aktarımında uyarı verebilir.

Azure Information Protection (AIP) sınıflandırılmış ve etiketlenmiş bilgileri izleme özellikleri sağlar.

Veri kaybı önleme (DLP) uyumluluğu için gerekirse, veri sızdırmayı önlemeye yönelik algılama ve/veya koruma denetimlerini zorunlu tutmak üzere konak tabanlı bir DLP çözümü kullanabilirsiniz.

- [Azure SQL ATP’yi Etkinleştirme](../azure-sql/database/threat-detection-overview.md) 

- [Azure Depolama ATP’yi Etkinleştirme](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-security-center)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="dp-5-encrypt-sensitive-data-at-rest"></a>VK-5: Bekleyen hassas verileri şifreleme

**Rehber**: Erişim denetimlerini tamamlamak üzere, Azure Maliyet Yönetimi’nin Dışarı Aktarma özelliği Azure Depolama’nın bekleyen verileri şifreleme özelliğini destekler. Bu sayede Microsoft tarafından yönetilen anahtar şifrelemesi kullanılarak ‘bant dışı’ saldırılara (temel depolamaya erişme gibi) karşı koruma sağlanır. Bu varsayılan ayarlar saldırganların verileri kolayca okuyabilmesini veya değiştirebilmesini önlemeye yardımcı olur.

Daha fazla bilgi için bkz.:

- [Bekleyen veri için Azure Depolama şifrelemesi](../storage/common/storage-service-encryption.md)

- [Azure’da bekleyen veri şifrelemesini anlama](../security/fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="asset-management"></a>Varlık Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Varlık Yönetimi](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>VY-1: Güvenlik ekibinin varlıklara yönelik risklerde görünürlük elde etmesini sağlama

**Rehber**: Güvenlik ekiplerinin Azure Güvenlik Merkezi’ni kullanarak güvenlik risklerini izlemesini sağlamak için bu ekiplere Azure kiracınız ve abonelikleriniz üzerinde Güvenlik Okuyucusu izinleri verildiğinden emin olun. 

Güvenlik ekibi sorumluluklarının yapılandırmasına bağlı olarak, güvenlik risklerinin izlenmesi merkezi güvenlik ekibinin veya yerel bir ekibin sorumluluğunda olabilir. Gerçi güvenlik içgörüleri ve risklerinin kuruluş içinde her zaman merkezi bir konumda toplanması gerekir. 

Güvenlik Okuyucusu izinleri kiracının tamamına (Kök Yönetim Grubu) geniş kapsamlı olarak uygulanabileceği gibi, izinlerin kapsamı yönetim gruplarıyla veya belirli aboneliklerle de sınırlanabilir. 

Not: İş yükleri ve hizmetlerin görünürlüğünü elde etmek için ek izinler gerekebilir. 

- [Güvenlik Okuyucusu Rolüne Genel Bakış](../role-based-access-control/built-in-roles.md#security-reader)

- [Azure Yönetim Gruplarına Genel Bakış](../governance/management-groups/overview.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>VY-4: Varlık yaşam döngüsü yönetiminin güvenliğini sağlama

**Rehber**: Çok etkili olabilecek değişiklikler için varlık yaşam döngüsü yönetim süreçleri sağlayan güvenlik ilkelerini oluşturun veya güncelleştirin. Bu değişiklikler kimlik sağlayıcıları ve erişim, veri duyarlılığı, ağ yapılandırması ve yönetici ayrıcalığı ataması ile ilgili değişikliklerdir.

Artık gerekmeyen Azure kaynaklarını kaldırın.

- [Azure kaynak grubunu ve kaynağını silme](../azure-resource-manager/management/delete-resource-group.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-threat-detection"></a>Günlüğe Kaydetme ve Tehdit Algılama

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Günlüğe Kaydetme ve Tehdit Algılama](/azure/security/benchmarks/security-controls-v2-logging-threat-protection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>GT-2: Azure kimlik ve erişim yönetimi için tehdit algılamayı etkinleştirme

**Rehber**: Azure AD’nin sağladığı aşağıdaki kullanıcı günlükleri Azure AD raporlamasında görüntülenebilir veya daha gelişmiş izleme ve analiz kullanım örnekleri için Azure İzleyici, Azure Sentinel veya diğer SIEM/izleme araçlarıyla tümleştirilebilir:

- Oturum açma bilgileri – Oturum açma bilgileri raporu, yönetilen uygulamaların kullanımı ve kullanıcı oturum açma etkinlikleri hakkında bilgi sağlar.

- Denetim günlükleri - Azure AD içindeki çeşitli özellikler tarafından yapılan tüm değişiklikler için günlükler aracılığıyla izlenebilirlik sağlar. Azure AD içindeki herhangi bir kaynakta yapılan kullanıcı, uygulama, grup, rol ve ilkeleri ekleme veya kaldırma işlemleri gibi değişiklikler, denetim günlüklerine örnek gösterilebilir.

- Riskli oturum açma işlemleri - Riskli oturum açma işlemi bir kullanıcı hesabının meşru sahibi olmayan bir kişi tarafından gerçekleştirilmiş olabilecek oturum açma girişiminin göstergesidir.

- Riskli oldukları belirlenen kullanıcılar - Riskli kullanıcı, güvenliği tehlikeye girmiş olabilecek bir kullanıcı hesabının göstergesidir.

Azure Güvenlik Merkezi aşırı fazla sayıda başarısız kimlik doğrulaması girişimi, abonelikte kullanım dışı kalan hesaplar gibi bazı şüpheli etkinliklerde de uyarı verebilir. Temel güvenlik durumunu izlemeye ek olarak, Azure Güvenlik Merkezi’nin Tehdit Koruması modülü tek tek Azure işlem kaynaklarından (sanal makineler, kapsayıcılar, uygulama hizmeti), veri kaynaklarından (SQL DB ve depolama) ve Azure hizmet katmanlarından daha kapsamlı güvenlik uyarıları da toplayabilir. Bu özellik tek tek kaynakların içindeki hesap anomalilerine görünürlük kazandırmanızı da sağlar.

Ayrıca Kurumsal Anlaşmanız (EA) çerçevesinde rollere atanan kullanıcıların düzenli olanak gözden geçirilmesi önerilir. 

- [Azure kurumsal rollerini yönetme](manage/understand-ea-roles.md)

- [Azure Active Directory’de denetim etkinliği raporları](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Azure Kimlik Koruması’nı etkinleştirme](../active-directory/identity-protection/overview-identity-protection.md)

- [Azure Güvenlik Merkezi’nde tehdit koruması](/azure/security-center/threat-protection)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Olay Yanıtı](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>OY-1: Hazırlık – Azure için olay yanıtı sürecini güncelleştirme

**Rehber**: Kuruluşunuzda güvenlik olaylarını yanıtlama süreçleri olduğundan, bu süreçlerin Azure için güncelleştirildiğinden ve hazırlıklı olmak için bu süreçlerin düzenli olarak uygulandığından emin olun.

- [Kurumsal ortam genelinde güvenlik uygulama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Olay yanıtı başvuru kılavuzu](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="ir-2-preparation--setup-incident-notification"></a>OY-2: Hazırlık – olay bildirimi ayarlama

**Rehber**: Azure Güvenlik Merkezi’nde güvenli olayı iletişim bilgilerini ayarlayın. Bu iletişim bilgileri, Microsoft Güvenlik Yanıt Merkezi (MSRC) verilerinize yasadışı veya yetkisiz bir tarafın eriştiğini belirlerse Microsoft tarafından sizinle iletişim kurmak için kullanılır. Ayrıca olay yanıtı gereksinimleriniz temelinde farklı Azure hizmetlerinde olay uyarı ve bildirimlerini özelleştirme seçenekleriniz de vardır. 

- [Azure Güvenlik Merkezi güvenlik iletişimini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>OY-3: Algılama ve analiz – yüksek kaliteli uyarılar temelinde olaylar oluşturma

**Rehber**: Yüksek kaliteli uyarılar oluşturmak ve uyarıların kalitesini ölçmek için bir süreciniz olduğundan emin olun. Bu sayede geçmiş olaylardan ders çıkarabilir ve analistler için uyarıların önceliğini belirleyerek yanlış pozitif sonuçlarla zaman kaybetmelerini önleyebilirsiniz. 

Yüksek kaliteli uyarılar geçmiş olaylardan edinilen deneyime, doğrulanmış topluluk kaynaklarına ve çeşitli sinyal kaynaklarını birleştirerek ve aralarında bağıntı kurarak uyarılar oluşturmak ve temizlemek için tasarlanmış araçlara dayalı olarak oluşturulabilir. 

Azure Güvenlik Merkezi birçok Azure varlığı arasında yüksek kaliteli uyarılar sağlar. ASC veri bağlayıcısını kullanarak uyarıların Azure Sentinel’e akışını yapabilirsiniz. Azure Sentinel bir araştırmaya yönelik olayların otomatik olarak oluşturulması için gelişmiş uyarı kuralları oluşturur. 

Azure kaynaklarına yönelik riskleri tanımlamaya yardımcı olmak için, dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Uyarıları ve önerileri el ile veya sürekli bir biçimde dışarı aktarabilirsiniz.

- [Dışarı aktarma işlemini yapılandırma](../security-center/continuous-export.md)

- [Uyarıların Azure Sentinel’e akışını yapma](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>OY-4: Algılama ve analiz – olayı araştırma

**Rehber**: Analistlerin olası olayları araştırırken neler olup bittiğini tam olarak görmek için çeşitli veri kaynaklarını sorgulayabildiğinden ve kullanabildiğinden emin olun. Sonlandırma zinciri boyunca olası saldırganın etkinliklerinin izlenebilmesi için çeşitli günlükler toplanmalı, kör noktaların olması önlenmelidir.  Ayrıca diğer analistler için ve gelecekte başvurmak üzere içgörülerin ve çıkarılan derslerin yakalandığından da emin olmalısınız.  

Araştırmaya yönelik veri kaynakları kapsam içindeki hizmetlerden ve çalışan sistemlerden zaten toplanmakta olan merkezi günlük kaynaklarının yanı sıra şunları da içerebilir:

- Ağ verileri – ağ akış günlüklerini ve diğer analiz bilgilerini yakalamak için ağ güvenlik gruplarının akış günlüklerini, Azure Ağ İzleyicisi’ni ve Azure İzleyici’ni kullanın. 

- Çalışan sistemlerin anlık görüntüleri: 

    - Çalışan sistemin diskinin anlık görüntüsünü oluşturmak için Azure sanal makinesinin anlık görüntü özelliğini kullanın. 

    - Çalışan sistemin belleğinin anlık görüntüsünü oluşturmak için işletim sisteminin yerel bellek dökümü özelliğini kullanın.

    - Çalışan sistemlerin anlık görüntülerini oluşturmak için Azure hizmetlerinin anlık görüntü özelliğini veya yazılımınızın kendi özelliğini kullanın.

Azure Sentinel hemen her veri kaynağı üzerinde kapsamlı veri analizi gerçekleştirdiği gibi, olayların tüm yaşam döngüsünü yöneten bir olay yönetim portalı da sağlar. Araştırma sırasında elde edilen inceleme bilgileri izleme ve raporlama amacıyla bir olayla ilişkilendirilebilir. 

- [Windows makinesi diskinin anlık görüntüsü](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Linux makinesi diskinin anlık görüntüsü](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure Desteği tanılama bilgileri ve bellek dökümü toplama](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Azure Sentinel ile olayları araştırma](../sentinel/tutorial-investigate-cases.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>OY-5: Algılama ve analiz – olayların önceliklerini belirleme

**Rehber**: Uyarı önem derecesi ve uyarı hassasiyeti temelinde analistlere önce hangi olaylara odaklanılacağı konusunda bağlam sağlayın. 

Azure Güvenlik Merkezi önce hangi uyarıların araştırılması gerektiğini belirlemenize yardımcı olmak için her uyarıya bir önem derecesi atar. Önem derecesinde, Güvenlik Merkezi’nin bulguya ne kadar güvendiği veya uyarıyı verirken kullanılan analiz kadar, uyarıya yol açan etkinliğin ardında kötü bir amaç olduğuna ilişkin güvenilirlik düzeyi de temel alınır.

Buna ek olarak Azure kaynaklarını, özellikle hassas verileri işleyen kaynakları belirlemek ve kategorilere ayırmak için etiketleri kullanarak kaynakları işaretleyin ve bir adlandırma sistemi oluşturun.  Azure kaynaklarının önem düzeyine ve olayın oluştuğu ortama bağlı olarak uyarıların çözümünde önceliği belirlemek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>OY-6: Sınırlama, yok etme ve kurtarma – olay işlemeyi otomatikleştirme

**Rehber**: Yanıt süresini kısaltmak ve analistlerin önündeki engelleri azaltmak için el ile gerçekleştirilen yinelemeli görevleri otomatikleştirin. El ile gerçekleştirilen görevlerin yürütme süresi daha uzun olduğundan her olayı yavaşlatır ve bir analistin işleyebileceği olay sayısını azaltır. Ayrıca el ile gerçekleştirilen görevler analistler açısından daha yorucudur. Dolayısıyla gecikmelere neden olan insan hatası riskini artırır ve analistlerin karmaşık görevlere etkili bir şekilde odaklanma becerisini azaltır. Eylemleri otomatik olarak tetiklemek veya gelen güvenlik uyarılarını yanıtlamak üzere bir playbook çalıştırmak için Azure Güvenlik Merkezi’nin ve Azure Sentinel’in iş akışı otomasyonu özelliklerini kullanın. Playbook bildirimleri gönderme, hesapları devre dışı bırakma ve sorunlu ağları yalıtma gibi eylemleri gerçekleştirir. 

- [Güvenlik Merkezi’nde iş akışı otomasyonunu yapılandırma](../security-center/workflow-automation.md)

- [Azure Güvenlik Merkezi’nde otomatik tehdit yanıtlarını ayarlama](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Azure Sentinel’de otomatik tehdit yanıtlarını ayarlama](../sentinel/tutorial-respond-threats-playbook.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="posture-and-vulnerability-management"></a>Duruş ve Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Duruş ve Güvenlik Açığı Yönetimi](/azure/security/benchmarks/security-controls-v2-vulnerability-management).*

### <a name="pv-7-rapidly-and-automatically-remediate-software-vulnerabilities"></a>DG-7: Yazılım güvenlik açıklarını hızla ve otomatik olarak düzeltme

**Rehber**: İşletim sistemleri ve uygulamalardaki yazılım güvenlik açıklarını düzeltmek için yazılım güncelleştirmelerini hızla dağıtın.

Yaygın bir risk puanlama programı (örneğin Common Vulnerability Scoring System) veya üçüncü taraf tarama aracınız tarafından sağlanan varsayılan risk derecelendirmelerini kullanın ve yüksek güvenlik riskine sahip olan, ayrıca uzun çalışma süreleri gerektiren uygulamaların bağlamını kullanarak ortamınızı uyarlayın.

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pv-8-conduct-regular-attack-simulation"></a>DG-8: Düzenli saldırı simülasyonu çalıştırma

**Rehber**: Gerektiğinde Azure kaynaklarınızda sızma testi veya kırmızı ekip etkinlikleri çalıştırın ve tüm kritik güvenlik bulgularının düzeltildiğinden emin olun.
Sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak için Microsoft Cloud Sızma Testi Angajman Kuralları’na uyun. Microsoft tarafından yönetilen bulut altyapısında, hizmetlerde ve uygulamalarda Microsoft'un stratejisini ve Kırmızı Ekip ile canlı site sızma testi yürütmesini kullanın.

- [Azure’da sızma testi](../security/fundamentals/pen-testing.md)

- [Sızma Testi Angajman Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Cloud Kırmızı Ekibi](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Paylaşımlı

## <a name="governance-and-strategy"></a>İdare ve Strateji

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: İdare ve Strateji](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>İS-1: Varlık yönetimi ve veri koruma stratejisini tanımlama 

**Rehber**: Sistemlerin ve verilerin sürekli izlenmesi ve korunması için tek bir stratejiyi belgelediğinizden ve ilettiğinizden emin olun. İş açısından kritik veriler ve sistemlerin bulunması, değerlendirilmesi, korunması ve izlenmesine öncelik verin. 

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   İş risklerine uygun olarak veri sınıflandırma standardı

-   Riskler ve varlık envanterinde güvenlik organizasyonu görünürlüğü 

-   Kullanılacak Azure hizmetleri konusunda güvenlik organizasyonu onayı 

-   Varlıkların yaşam döngüleri boyunca güvenliği

-   Kurumsal verilerin sınıflandırmasına uygun olarak gerekli erişim denetimi stratejisi

-   Azure yerel ve üçüncü taraf veri koruma özelliklerinin kullanımı

-   Aktarımdaki ve bekleyen veri kullanım örneklerine yönelik veri şifreleme gereksinimleri

-   Uygun şifreleme standartları

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenlik Mimarisi Önerisi - Depolama, veri ve şifreleme](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Azure Güvenlik Temelleri - Azure Veri güvenliği, şifrelemesi ve depolaması](../security/fundamentals/encryption-overview.md)

- [Bulut Benimseme Çerçevesi - Azure veri güvenliği ve şifrelemesi için en iyi yöntemler](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Azure Güvenlik Karşılaştırması - Varlık yönetimi](/azure/security/benchmarks/security-benchmark-v2-asset-management)

- [Azure Güvenlik Karşılaştırması - Veri Koruma](/azure/security/benchmarks/security-benchmark-v2-data-protection)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>İS-2: Kurumsal segmentasyon stratejisini tanımlama 

**Rehber**: Kimlik, ağ, uygulama, abonelik, yönetim grubu ve diğer denetimleri kullanarak varlıklara erişimi segmentlere ayırmak için kurumsal çapta bir strateji oluşturun.

Güvenlik ayrımı ihtiyacı ile birbiriyle iletişim kurması ve verilere erişmesi gereken sistemlerin günlük operasyonuna olanak sağlama ihtiyacını dikkatle dengeleyin.

Segmentasyon stratejisinin ağ güvenliği, kimlik ve erişim modelleri, uygulama izni/erişimi modelleri ve insan işlemi denetimleri gibi denetim türleri arasında tutarlı bir şekilde uygulandığından emin olun.

- [Azure’da segmentasyon stratejisi rehberliği (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure’da segmentasyon stratejisi rehberliği (belge)](/security/compass/governance#enterprise-segmentation-strategy)

- [Ağ segmentasyonunu kurumsal segmentasyon stratejisiyle uyumlu hale getirme](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-3-define-security-posture-management-strategy"></a>İS-3: Güvenlik duruşu yönetim stratejisini tanımlama

**Rehber**: Tek tek varlıklarınıza ve bunların barındırıldığı ortama yönelik riskleri sürekli ölçün ve azaltın. Yayımlanmış uygulamalar, ağ giriş ve çıkış noktaları, kullanıcı ve yönetici uç noktaları gibi yüksek değerli varlıklara ve çok fazla kullanıma sunulan yüzeylere öncelik verin.

- [Azure Güvenlik Karşılaştırması - Duruş ve güvenlik açığı yönetimi](/azure/security/benchmarks/security-benchmark-v2-posture-vulnerability-management)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>İS-4: Kuruluş rollerini, sorumluluklarını ve hesap verilebilirliğini uyumlu hale getirme

**Rehber**: Güvenlik kuruluşunuzdaki roller ve sorumluluklar için açık bir stratejiyi belgelediğinizden ve ilettiğinizden emin olun. Paylaşılan sorumluluk modeli için herkesi eğiterek ve bulutun güvenliğini sağlayacak teknolojiler için teknik ekipleri eğiterek güvenlik kararlarında net bir hesap verilebilirlik sağlamaya öncelik verin.

- [Azure Güvenliği En İyi Yöntemi 1 – Kişiler: Bulut Güvenliği Yolculuğunda Ekipleri Eğitme](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure Güvenliği En İyi Yöntemi 2 – Kişiler: Bulut Güvenliği Teknolojinde Ekipleri Eğitme](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure Güvenliği En İyi Yöntemi 3 – İşlem: Bulut Güvenliği Kararları için Sorumluluk Atama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-5-define-network-security-strategy"></a>İS-5: Ağ güvenliği stratejisini tanımlama

**Rehber**: Kuruluşunuzun genel güvenlik erişim denetimi stratejisinin bir parçası olarak bir Azure ağ güvenliği yaklaşımı oluşturun.  

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   Merkezi ağ yönetimi ve güvenlik sorumluluğu

-   Kurumsal segmentasyon stratejisiyle uyumlu bir sanal ağ segmentasyon modeli

-   Farklı tehdit ve saldırı senaryolarında düzeltme stratejisi

-   İnternet ucu, giriş ve çıkış stratejisi

-   Hibrit bulut ve şirket içi karşılıklı bağlantı stratejisi

-   Güncel ağ güvenliği yapıtları (örneğin ağ diyagramları, referans ağ mimarisi)

Daha fazla bilgi için aşağıdaki başvurulara bakın:
- [Azure Güvenliği En İyi Yöntemi 11 – Mimari. Tek birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Güvenlik Karşılaştırması - Ağ Güvenliği](/azure/security/benchmarks/security-benchmark-v2-network-security)

- [Azure ağ güvenliğine genel bakış](../security/fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>İS-6: Kimlik ve ayrıcalıklı erişim stratejisi tanımlama

**Rehber**: Kuruluşunuzun genel güvenlik erişim denetimi stratejisinin bir parçası olarak bir Azure kimlik ve ayrıcalıklı erişim yaklaşımları oluşturun.  

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   Merkezi kimlik ve kimlik doğrulaması sistemi ile bu sistemin diğer iç ve dış kimlik sistemleriyle karşılıklı bağlantısı

-   Farklı kullanım örnekleri ve koşullarında güçlü kimlik doğrulama yöntemleri

-   Yüksek ayrıcalıklı kullanıcıları koruma

-   Anormal kullanıcı etkinliklerini izleme ve işleme  

-   Kullanıcı kimliği ve erişimini gözden geçirme ve mutabakat süreci

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik Karşılaştırması - Kimlik yönetimi](/azure/security/benchmarks/security-benchmark-v2-identity-management)

- [Azure Güvenlik Karşılaştırması - Ayrıcalıklı erişim](/azure/security/benchmarks/security-benchmark-v2-privileged-access)

- [Azure Güvenliği En İyi Yöntemi 11 – Mimari. Tek birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure kimlik yönetimi güvenliğine genel bakış](../security/fundamentals/identity-management-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>İS-7: Günlüğe kaydetme ve tehdit yanıtı stratejisini tanımlama

**Rehber**: Tehditleri hızla algılar ve giderirken uyumluluk gereksinimlerini de karşılamak için bir günlüğe kaydetme ve tehdit yanıtı stratejisi oluşturun. Analistlerin tümleştirmeyle ve el ile uygulanan adımlarla ilgilenmek yerine tehditlere odaklanabilmelerini sağlamak için onlara yüksek kaliteli uyarılar ve sorunsuz deneyimler sağlamaya öncelik verin. 

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   Güvenlik operasyonları (SecOps) kuruluşunun rolü ve sorumlulukları 

-   NIST veya başka bir endüstri çerçevesiyle uyumlu, iyi tanımlanmış bir olay yanıt süreci 

-   Tehdit algılama, olay yanıtı ve uyumluluk gereksinimlerini desteklemek için günlük yakalama ve saklama

-   SIEM’yi, yerel Azure özelliklerini ve diğer kaynakları kullanarak tehditlerin merkezi görünürlüğü ve tehditler hakkındaki bağıntı bilgileri 

-   Müşterileriniz, sağlayıcılarınız ve genel olarak ilgili taraflarla iletişim ve bildirim planı

-   Günlüğe kaydetme ve tehdit algılama, adli araştırma, saldırı düzeltme ve yok etme gibi olayları işlemek için Azure yerel platformunu ve üçüncü taraf platformlarını kullanma

-   Olayları ve olay sonrası çıkarılan dersler ve kanıt saklama gibi etkinlikleri işlemeye yönelik süreçler

Daha fazla bilgi için aşağıdaki başvurulara bakın:

- [Azure Güvenlik Karşılaştırması - Günlüğe kaydetme ve tehdit algılama](/azure/security/benchmarks/security-benchmark-v2-logging-threat-detection)

- [Azure Güvenlik Karşılaştırması - Olay yanıtı](/azure/security/benchmarks/security-benchmark-v2-incident-response)

- [Azure Güvenliği En İyi Yöntemi 4 – İşlem. Bulut için Olay Yanıtı Süreçlerini Güncelleştirme](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure Benimseme Çerçevesi, günlük ve raporlama karar kılavuzu](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure kurumsal ölçeği, yönetimi ve izlemesi](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Karşılaştırması V2’ye genel bakış](/azure/security/benchmarks/overview) konusuna bakın
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
