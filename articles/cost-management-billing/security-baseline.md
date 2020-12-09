---
title: Maliyet Yönetimi için Azure güvenlik temeli
description: Maliyet Yönetimi güvenlik temeli Azure Güvenlik Karşılaştırması’nda belirtilen güvenlik önlemlerini uygulamaya yönelik yordam rehberini ve kaynakları sağlar.
author: msmbaldwin
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 11/16/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 0155ed4df9ee2c22d1461633ac9efe225b53f9ca
ms.sourcegitcommit: 5b93010b69895f146b5afd637a42f17d780c165b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96530519"
---
# <a name="azure-security-baseline-for-cost-management"></a>Maliyet Yönetimi için Azure güvenlik temeli

Bu güvenlik temeli, [Azure Güvenlik Karşılaştırması sürüm 2.0](../security/benchmarks/overview.md)’ın güvenlik temelini Azure Maliyet Yönetimi’ne uygular. Azure Güvenlik Karşılaştırması Azure’da bulut çözümlerinizin güvenliğini nasıl sağlayabileceğinize ilişkin öneriler getirir. İçerik, Azure Güvenlik Karşılaştırması tarafından tanımlanan **güvenlik denetimlerine** ve Maliyet Yönetimi için kullanılabilen ilgili rehbere göre gruplandırılmıştır. Maliyet Yönetimi’nde geçerli olmayan **denetimler** hariç tutulmuştur.

Maliyet Yönetimi’nin Azure Güvenlik Karşılaştırması’yla tam olarak eşleşmesini görmek için [Maliyet Yönetimi güvenlik temeli eksiksiz eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines) bakın.

## <a name="identity-management"></a>Kimlik Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Kimlik Yönetimi](../security/benchmarks/security-controls-v2-identity-management.md).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>KY-1: Azure Active Directory’yi merkezi kimlik ve kimlik doğrulaması sistemi olarak standartlaştırma

**Rehber**: Azure Maliyet Yönetimi, Azure’ın varsayılan kimlik ve erişim yönetim hizmeti olan Azure Active Directory (Azure AD) ile tümleştirilmiştir. Kuruluşunuzun şu kaynaklarda kimlik ve erişim yönetimini idare etmek için Azure AD’yi standartlaştırmanız gerekir:

- Microsoft bulut kaynakları. Örneğin Azure portal, Azure Depolama, Azure Sanal Makineler (Linux ve Windows), Azure Key Vault, PaaS ve SaaS uygulamaları.

- Kuruluşunuzun kaynakları. Örneğin Azure’daki uygulamalar veya kurumsal ağ kaynaklarınız.

Kuruluşunuzun bulut güvenliği uygulamasında Azure AD’nin güvenliğini sağlamaya yüksek öncelik verilmelidir. Microsoft’un en iyi yöntem önerilerine göre kendi kimlik güvenliği duruşunuzu değerlendirmenize yardımcı olmak için Azure AD bir kimlik güvenliği puanı sağlar. Bu puanı kullanarak yapılandırmanızın en iyi yöntem önerileriyle ne kadar uyumlu olduğunu ölçebilir ve güvenlik duruşunuzda geliştirmeler yapabilirsiniz.

Not: Azure AD dış kimlik sağlayıcılarını desteklediğinden, Microsoft hesabı olmayan kullanıcıların dış kimlikleriyle uygulamalarında ve kaynaklarında oturum açmasına izin verir.

- [Azure AD’de kiracılık](../active-directory/develop/single-and-multi-tenant-apps.md)

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Azure AD kiracılarını tanımlama](https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/)  

- [Bir uygulama için dış kimlik sağlayıcılarını kullanma](../active-directory/external-identities/identity-providers.md)

- [Azure AD'de kimlik güvenliği puanı nedir?](../active-directory/fundamentals/identity-secure-score.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>KY-3: Uygulama erişiminde Azure AD çoklu oturum açmayı (SSO) kullanma

**Rehber**: Azure Maliyet Yönetimi Azure kaynakları, bulut uygulamaları ve şirket içi uygulamalarda kimlik ve erişim yönetimi sağlamak için Azure Active Directory’yi kullanır. Bu hem çalışanlar gibi kurumsal kimlikleri hem de iş ortakları, satıcılar ve sağlayıcılar gibi dış kimlikleri içerir. Bu seçenek, kuruluşunuzun şirket içi ortamda ve bulutta bulunan verilerini ve kaynaklarını yönetip bunlara güvenli erişim sağlamak için çoklu oturum açma (SSO) özelliğinin kullanılmasını sağlar. Tüm kullanıcılarınızı, uygulamalarınızı ve cihazlarınızı Azure AD'ye bağlayarak sorunsuz ve güvenli erişimin yanı sıra daha fazla görünürlük ve denetim elde edebilirsiniz.

- [Azure AD ile uygulamalar için SSO yaklaşımını anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>KY-4: Tüm Azure Active Directory tabanlı erişim için güçlü kimlik doğrulaması denetimlerini kullanma

**Rehber**: Azure Maliyet Yönetimi çok faktörlü kimlik doğrulaması (MFA) ve güçlü parolasız yöntemler aracılığıyla güçlü kimlik doğrulaması denetimlerini destekleyen Azure AD ile tümleştirilmiştir.

- Çok faktörlü kimlik doğrulaması - Azure AD MFA’yı etkinleştirin ve MFA kurulumunuzda izleyebileceğiniz en iyi yöntemlerden bazıları için Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerine uyun. Çok faktörlü kimlik doğrulaması tüm kullanıcılara, yalnızca belirli kullanıcılara veya oturum açma koşullarıyla risk faktörlerine göre kullanıcı düzeyinde uygulanabilir.

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

- [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)

- [Riskli etkinlik bayrağıyla işaretlenen Azure AD kullanıcılarını belirleme](../active-directory/identity-protection/overview-identity-protection.md) 

- [Azure Güvenlik Merkezi’nde kullanıcıların kimliğini ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md) 

- [Azure Güvenlik Merkezi’nin tehdit analizi koruma modülündeki uyarılar](../security-center/alerts-reference.md) 

- [Azure Etkinlik Günlüklerini Azure İzleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="privileged-access"></a>Ayrıcalıklı Erişim

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Ayrıcalıklı Erişim](../security/benchmarks/security-controls-v2-privileged-access.md).*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>AE-1: Yüksek ayrıcalıklı kullanıcıları koruma ve sınırlama

**Rehber**: Kurumsal Anlaşmalar (EA) için Maliyet Yönetimi’nin aşağıdaki yüksek ayrıcalıklı hesabı vardır.

- Kuruluş Yöneticisi

Kurumsal Anlaşmanız (EA) kapsamında belirli rollere atanan kullanıcıların düzenli olanak gözden geçirilmesi önerilir.

Ayrıca bir kural olarak yüksek öncelikli hesapların veya rollerin sayısının sınırlanması ve bu hesapların yükseltilmiş bir düzeyde korunması önerilir. Çünkü bu ayrıcalığa sahip kullanıcılar Azure ortamınızdaki her kaynağı doğrudan veya dolaylı olarak okuyabilir ve değiştirebilir.

Azure AD Privileged Identity Management’ı (PIM) kullanarak Azure kaynaklarına ve Azure AD’ye tam zamanında (JIT) ayrıcalıklı erişimi etkinleştirebilirsiniz. JIT yalnızca kullanıcıların ihtiyacı olduğunda ayrıcalıklı görevler gerçekleştirmeleri için geçici izinler verir. Azure AD kuruluşunuzda güvenli olmayan veya şüpheli etkinlikler olduğunda da PIM güvenlik uyarıları oluşturabilir.

- [Azure kurumsal rollerini yönetme](manage/understand-ea-roles.md) 

- [Azure AD'de yönetici rolü izinleri](../active-directory/roles/permissions-reference.md) 

- [Azure Privileged Identity Management güvenlik uyarılarını kullanma](../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md) 

- [Azure AD'de karma ve bulut dağıtımları için ayrıcalıklı erişim güvenliğini sağlama](../active-directory/roles/security-planning.md)

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

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Veri Koruma](../security/benchmarks/security-controls-v2-data-protection.md).*

### <a name="dp-1-discovery-classify-and-label-sensitive-data"></a>VK-1: Hassas verileri bulma, sınıflandırma ve etiketleme

**Rehber**: Uygun denetimleri tasarlayarak kuruluşun teknoloji sistemleri tarafından hassas bilgilerin güvenle depolandığından, işlendiğinden ve iletildiğinden emin olabilmeniz için hassas verilerinizi bulmanız, sınıflandırmanız ve etiketlemeniz önerilir.

Azure’daki, şirket içindeki, Office 365’teki ve diğer konumlardaki hassas bilgiler için Azure Information Protection’ı (ve ilişkili tarama aracını) kullanın.

Azure SQL Information Protection’ı kullanarak Azure SQL Veritabanlarında depolanan bilgilerin sınıflandırılmasına ve etiketlenmesine yardımcı olabilirsiniz.

- [Azure Information Protection kullanarak hassas bilgileri etiketleme](/azure/information-protection/what-is-information-protection) 

- [Azure SQL Veri Bulma’yı uygulama](../azure-sql/database/data-discovery-and-classification-overview.md)

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

- [Azure Depolama ATP’yi Etkinleştirme](../storage/common/azure-defender-storage-configure.md?tabs=azure-security-center)

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

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Varlık Yönetimi](../security/benchmarks/security-controls-v2-asset-management.md).*

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

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Günlüğe Kaydetme ve Tehdit Algılama](../security/benchmarks/security-controls-v2-logging-threat-detection.md).*

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

- [Azure Güvenlik Merkezi’nde tehdit koruması](../security-center/azure-defender.md)

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Olay Yanıtı](../security/benchmarks/security-controls-v2-incident-response.md).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Hazırlık: Azure için olay yanıtı sürecini güncelleştirin

**Rehberlik**: Kuruluşunuzda güvenlik olaylarına yanıt vermeye yönelik süreçler bulunduğundan, bu süreçlerin Azure için güncelleştirildiğinden ve hazır olduğunuzdan emin olmak için bu süreçlerin düzenli olarak uygulandığından emin olun.

- [Güvenliği kuruluş genelinde uygulama](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Olay yanıtı başvuru kılavuzu](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Hazırlık: Olay bildirimini ayarlayın

**Rehberlik**: Azure Güvenlik Merkezi'nde güvenlik olayı iletişim bilgilerini ayarlayın. Microsoft, Microsoft Güvenlik Yanıt Merkezi'nin (MSRC) verilerinize kanuna aykırı veya yetkisiz erişim sağlanmasını keşfetmesi durumunda sizinle iletişime geçmek için bu iletişim bilgilerini kullanır. İsterseniz farklı Azure hizmetlerindeki olay uyarılarını ve bildirimlerini olay yanıt gereksinimlerinize göre özelleştirebilirsiniz. 

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

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Algılama ve analiz: Olayları önceliklendirin

**Rehber**: Uyarı önem derecesi ve uyarı hassasiyeti temelinde analistlere önce hangi olaylara odaklanılacağı konusunda bağlam sağlayın. 

Azure Güvenlik Merkezi önce hangi uyarıların araştırılması gerektiğini belirlemenize yardımcı olmak için her uyarıya bir önem derecesi atar. Önem derecesinde, Güvenlik Merkezi’nin bulguya ne kadar güvendiği veya uyarıyı verirken kullanılan analiz kadar, uyarıya yol açan etkinliğin ardında kötü bir amaç olduğuna ilişkin güvenilirlik düzeyi de temel alınır.

Buna ek olarak Azure kaynaklarını, özellikle hassas verileri işleyen kaynakları belirlemek ve kategorilere ayırmak için etiketleri kullanarak kaynakları işaretleyin ve bir adlandırma sistemi oluşturun.  Azure kaynaklarının önem düzeyine ve olayın oluştuğu ortama bağlı olarak uyarıların çözümünde önceliği belirlemek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Kapsama, ortadan kaldırma ve kurtarma: Olay işleme sürecini otomatikleştirin

**Rehberlik**: Yanıt süresini kısaltmak ve analistlerin yükünü hafifletmek için el ile yapılan yinelenen görevleri otomatikleştirin. El ile gerçekleştirilen görevlerin yürütülmesi daha uzun sürer ve olay yanıt sürecini yavaşlatarak analistlerin işleyebileceği olay sayısını azaltır. Ayrıca el ile gerçekleştirilen görevler analistler açısından daha yorucudur. Dolayısıyla gecikmelere neden olan insan hatası riskini artırır ve analistlerin karmaşık görevlere etkili bir şekilde odaklanma becerisini azaltır. Eylemleri otomatik olarak tetiklemek veya gelen güvenlik uyarılarını yanıtlamak üzere bir playbook çalıştırmak için Azure Güvenlik Merkezi’nin ve Azure Sentinel’in iş akışı otomasyonu özelliklerini kullanın. Playbook bildirimleri gönderme, hesapları devre dışı bırakma ve sorunlu ağları yalıtma gibi eylemleri gerçekleştirir. 

- [Güvenlik Merkezi’nde iş akışı otomasyonunu yapılandırma](../security-center/workflow-automation.md)

- [Azure Güvenlik Merkezi’nde otomatik tehdit yanıtlarını ayarlama](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Azure Sentinel’de otomatik tehdit yanıtlarını ayarlama](../sentinel/tutorial-respond-threats-playbook.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="posture-and-vulnerability-management"></a>Duruş ve Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: Duruş ve Güvenlik Açığı Yönetimi](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md).*

### <a name="pv-7-rapidly-and-automatically-remediate-software-vulnerabilities"></a>DG-7: Yazılım güvenlik açıklarını hızla ve otomatik olarak düzeltme

**Rehber**: İşletim sistemleri ve uygulamalardaki yazılım güvenlik açıklarını düzeltmek için yazılım güncelleştirmelerini hızla dağıtın.

Yaygın bir risk puanlama programı (örneğin Common Vulnerability Scoring System) veya üçüncü taraf tarama aracınız tarafından sağlanan varsayılan risk derecelendirmelerini kullanın ve yüksek güvenlik riskine sahip olan, ayrıca uzun çalışma süreleri gerektiren uygulamaların bağlamını kullanarak ortamınızı uyarlayın.

**Azure Güvenlik Merkezi izlemesi**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: Düzenli aralıklarla saldırı simülasyonları yapın

**Rehberlik**: Gerektiğinde Azure kaynaklarınızda sızma testi veya kırmızı takım etkinlikleri gerçekleştirerek tüm kritik güvenlik bulgularının düzeltilmesini sağlayın.
Sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak için Microsoft Bulut Sızma Testi Etkileşim Kuralları'na uygun hareket edin. Microsoft tarafından yönetilen bulut altyapısına, hizmetlere ve uygulamalara yönelik kırmızı takım ve canlı site sızma testi gerçekleştirmek için Microsoft'un stratejisini ve yürütme sürecini kullanın.

- [Azure'da sızma testi yapma](../security/fundamentals/pen-testing.md)

- [Sızma Testi Etkileşim Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Bulut ile Kırmızı Takım Oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşımlı

## <a name="governance-and-strategy"></a>İdare ve Strateji

*Daha fazla bilgi için bkz. [Azure Güvenlik Karşılaştırması: İdare ve Strateji](../security/benchmarks/security-controls-v2-governance-strategy.md).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Varlık yönetimi ve veri koruma stratejisi tanımlayın 

**Rehberlik**: Sistemlerin ve verilerin sürekli izlenmesine ve korunmasına yönelik net bir strateji belgelendirdiğinizden ve paylaştığınızdan emin olun. İş açısından kritik verilerin ve sistemlerin bulma, değerlendirme, koruma ve izleme süreçleri için öncelik belirleyin. 

Bu strateji kapsamında aşağıdaki öğeler için rehberlik, ilkeler ve standartlar belgelenmiş olmalıdır: 

-   İş risklerine göre veri sınıflandırma standardı

-   Riskler ve varlık envanteriyle ilgili güvenlik kuruluşu görünürlüğü 

-   Güvenlik kuruluşunun kullanılmasını onayladığı Azure hizmetleri 

-   Varlıkların yaşam döngüsü boyunca güvenliği

-   Kurumsal verilerin sınıflandırmasına uygun olarak gerekli erişim denetimi stratejisi

-   Azure yerel ve üçüncü taraf veri koruma özelliklerinin kullanımı

-   Aktarımdaki ve bekleyen veri kullanım örneklerine yönelik veri şifreleme gereksinimleri

-   Uygun şifreleme standartları

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:
- [Azure Güvenlik Mimarisi Önerileri: Depolama, veri ve şifreleme](/azure/architecture/framework/security/storage-data-encryption?bc=%2fsecurity%2fcompass%2fbreadcrumb%2ftoc.json&toc=%2fsecurity%2fcompass%2ftoc.json)

- [Azure Güvenliği Temelleri: Azure'da veri güvenliği, şifreleme ve depolama](../security/fundamentals/encryption-overview.md)

- [Bulut Benimseme Çerçevesi: Azure'da veri güvenliği ve şifreleme için en iyi deneyimler](../security/fundamentals/data-encryption-best-practices.md?bc=%2fazure%2fcloud-adoption-framework%2f_bread%2ftoc.json&toc=%2fazure%2fcloud-adoption-framework%2ftoc.json)

- [Azure Güvenlik Karşılaştırması: Varlık yönetimi](../security/benchmarks/security-controls-v2-asset-management.md)

- [Azure Güvenlik Karşılaştırması: Veri koruma](../security/benchmarks/security-controls-v2-data-protection.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Kurumsal segmentasyon stratejisini tanımlayın 

**Rehberlik**: Kimlik, ağ, uygulama, abonelik, yönetim grubu ve diğer denetimleri kullanarak varlık erişimi segmentasyonuna yönelik kuruluş genelinde kullanılacak bir strateji belirleyin.

Güvenlik ayrımı gereksinimiyle birbirleriyle iletişim kurma ve verilere erişme gereksinimine sahip olan sistemlerin günlük çalışmasını etkinleştirme gereksinimi arasında bir denge kurun.

Segmentasyon stratejisinin ağ güvenliği, kimlik ve erişim modelleri, uygulama izinleri/erişim modelleri ve insanlar tarafından gerçekleştirilen süreç denetimleri gibi farklı denetim türlerinde tutarlı bir şekilde uygulandığından emin olun.

- [Azure için segmentasyon stratejisi rehberi (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Azure için segmentasyon stratejisi rehberi (belge)](/security/compass/governance#enterprise-segmentation-strategy)

- [Ağ segmentasyonunu kurumsal segmentasyon stratejisiyle uyumlu hale getirme](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Güvenlik duruşu yönetim stratejisini tanımlayın

**Rehberlik**: Varlıklarınız ve içinde bulundukları ortamla ilgili riskleri sürekli olarak ölçün ve ortadan kaldırın. Yayımlanmış uygulamalar, ağ giriş ve çıkış noktaları, kullanıcı ve yönetici uç noktaları gibi değeri yüksek varlıkları ve kullanıma açık olan saldırı yüzeylerini önceliklendirin.

- [Azure Güvenlik Karşılaştırması: Duruş ve güvenlik açığı yönetimi](../security/benchmarks/security-controls-v2-posture-vulnerability-management.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Kuruluş genelindeki rolleri, sorumlulukları ve hesap verilebilirlik durumlarını uyumlu hale getirin

**Rehberlik**: Güvenlik kuruluşunuzdaki roller ve sorumluluklar için net bir strateji oluşturup bunu belgelendirdiğinizden ve paylaştığınızdan emin olun. Güvenlik kararları için net bir hesap verilebilirlik süreci oluşturmayı önceliklendirin, herkesi paylaşılan sorumluluk modeli hakkında eğitin ve teknik ekiplere bulut ortamının güvenliğini sağlamaya yönelik eğitimler verin.

- [Azure Güvenliği En İyi Deneyimi 1 - İnsanlar: Ekipleri Bulut Güvenliği Yolculuğu Hakkında Eğitin](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Azure Güvenliği En İyi Deneyimi 2 - İnsanlar: Ekipleri Bulut Güvenliği Teknolojileri Hakkında Eğitin](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Azure Güvenliği En İyi Deneyimi 3 - Süreç: Bulut Güvenliği Kararları için Hesap Verilebilirlik Ataması Yapın](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Ağ güvenlik stratejisini tanımlayın

**Rehberlik**: Kuruluşunuzun genel güvenlik erişimi denetimi stratejisi kapsamında Azure ağ güvenliği için bir yaklaşım belirleyin.  

Bu strateji kapsamında aşağıdaki öğeler için rehberlik, ilkeler ve standartlar belgelenmiş olmalıdır: 

-   Merkezi ağ yönetimi ve güvenlik sorumluluğu

-   Kurumsal segmentasyon stratejisiyle uyumlu sanal ağ segmentasyon modeli

-   Farklı tehdit ve saldırı senaryolarına yönelik düzeltme stratejisi

-   İnternet uç düğümü ile giriş ve çıkış stratejisi

-   Hibrit bulut ve şirket içi bağlantı stratejisi

-   Güncel ağ güvenliği yapıtları (ağ diyagramları, başvuru amaçlı ağ mimarisi vb.)

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:
- [Azure Güvenliği En İyi Deneyimi 11 - Mimari. Tek ve birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure Güvenlik Karşılaştırması: Ağ Güvenliği](../security/benchmarks/security-controls-v2-network-security.md)

- [Azure ile ağ güvenliğine genel bakış](../security/fundamentals/network-overview.md)

- [Kurumsal ağ mimarisi stratejisi](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Kimlik ve ayrıcalıklı erişim stratejisini tanımlayın

**Rehberlik**: Kuruluşunuzun genel güvenlik erişimi denetimi stratejisi kapsamında Azure kimlik ve ayrıcalıklı erişim yaklaşımları belirleyin.  

Bu strateji kapsamında aşağıdaki öğeler için rehberlik, ilkeler ve standartlar belgelenmiş olmalıdır: 

-   Merkezi bir kimlik ve kimlik doğrulaması sistemi ile diğer iç ve dış kimlik sistemleriyle olan bağlantısı

-   Farklı kullanım örneklerine ve koşullara yönelik güçlü kimlik doğrulama yöntemleri

-   Yüksek ayrıcalıklı kullanıcıların korunması

-   Kullanıcı etkinlikleri için anomali izleme ve işleme  

-   Kullanıcı kimliği ve erişim gözden geçirmesi ile uzlaştırma süreci

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:

- [Azure Güvenlik Karşılaştırması: Kimlik yönetimi](../security/benchmarks/security-controls-v2-identity-management.md)

- [Azure Güvenlik Karşılaştırması: Ayrıcalıklı erişim](../security/benchmarks/security-controls-v2-privileged-access.md)

- [Azure Güvenliği En İyi Deneyimi 11 - Mimari. Tek ve birleşik güvenlik stratejisi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Azure'da kimlik yönetim güvenliğine genel bakış](../security/fundamentals/identity-management-overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Günlüğe kaydetme ve tehdit yanıtı stratejisi tanımlayın

**Rehber**: Tehditleri hızla algılar ve giderirken uyumluluk gereksinimlerini de karşılamak için bir günlüğe kaydetme ve tehdit yanıtı stratejisi oluşturun. Analistlerin tümleştirmeyle ve el ile uygulanan adımlarla ilgilenmek yerine tehditlere odaklanabilmelerini sağlamak için onlara yüksek kaliteli uyarılar ve sorunsuz deneyimler sağlamaya öncelik verin. 

Bu strateji aşağıdaki öğelerle ilgili olarak belgelenmiş rehberlik, ilke ve standartlar içermelidir: 

-   Güvenlik operasyonları (SecOps) organizasyonunun rolü ve sorumlulukları 

-   NIST veya diğer sektör çerçeveleriyle uyumlu iyi tanımlanmış bir olay yanıt süreci 

-   Tehdit algılama, olay yanıtı ve uyumluluk gereksinimlerini karşılamak için günlük yakalama ve saklama

-   SIEM, yerel Azure özellikleri ve diğer kaynaklar aracılığıyla merkezi görünürlük ve bilgi bağıntısı 

-   Planı müşterilerinize, tedarikçilerinize ve diğer proje katılımcılarına iletme ve onları bilgilendirme

-   Günlüğe kaydetme ve tehdit algılama, adli araştırma, saldırı önleme ve ortadan kaldırma gibi olay işleme adımları için yerel Azure ve üçüncü taraf platformları kullanma

-   Olayları ve çıkarılan dersler ile kanıt koruma gibi olay sonrası etkinlikleri işlemeye yönelik süreçler

Daha fazla bilgi için aşağıdaki başvuruları inceleyin:

- [Azure Güvenlik Karşılaştırması: Günlüğe kaydetme ve tehdit algılama](../security/benchmarks/security-controls-v2-logging-threat-detection.md)

- [Azure Güvenlik Karşılaştırması: Olay yanıtı](../security/benchmarks/security-controls-v2-incident-response.md)

- [Azure Güvenliği En İyi Deneyimi 4 - Süreç. Bulut için Olay Yanıt Sürecini Güncelleştirme](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Azure Benimseme Çerçevesi, günlüğe kaydetme ve raporlama kararı rehberi](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure ile kurumsal ölçek, yönetim ve izleme](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik Karşılaştırması 2.0 sürümüne genel bakış](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin