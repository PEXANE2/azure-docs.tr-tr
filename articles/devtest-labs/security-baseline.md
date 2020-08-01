---
title: Azure DevTest Labs için Azure Güvenlik temeli
description: Azure DevTest Labs için Azure Güvenlik temeli
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: b392af17a24b0a5aabdd245af236caa743762244
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87448960"
---
# <a name="azure-security-baseline-for-azure-devtest-labs"></a>Azure DevTest Labs için Azure Güvenlik temeli

Azure DevTest Labs için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' den çizilir ve bu, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın
**Rehberlik:** Microsoft, Azure kaynakları için zaman kaynaklarını korur. Ancak, işlem kaynaklarınız için zaman eşitleme ayarlarını yönetebilirsiniz.

Azure işlem kaynakları için zaman eşitlemesini yapılandırma hakkında bilgi edinmek için aşağıdaki makaleye bakın: [Azure 'Da Windows VM 'leri Için zaman eşitleme](../virtual-machines/windows/time-sync.md). 

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** MICROSOFT

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma
**Rehberlik:** Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Etkinlik günlükleri, yönetim düzlemi düzeyinde Azure DevTest Labs örneklerinizin üzerinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Azure etkinlik günlüğü verilerini kullanarak, DevTest Labs örnekleriniz için yönetim düzlemi düzeyinde herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Daha fazla bilgi için bkz. [Platform günlüklerini ve ölçümlerini farklı hedeflere göndermek için Tanılama ayarları oluşturma](../azure-monitor/platform/diagnostic-settings.md).

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme
**Rehberlik:** Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Etkinlik günlükleri, yönetim düzlemi düzeyinde Azure DevTest Labs örneklerinizin üzerinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Azure etkinlik günlüğü verilerini kullanarak, DevTest Labs örnekleriniz için yönetim düzlemi düzeyinde herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Daha fazla bilgi için bkz. [Platform günlüklerini ve ölçümlerini farklı hedeflere göndermek için Tanılama ayarları oluşturma](../azure-monitor/platform/diagnostic-settings.md).

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama
**Rehberlik:** Azure DevTest Labs sanal makineler (VM) oluşturulur ve müşteriye aittir. Bu nedenle, bu, kuruluşun izleme sorumluluğudur. İşlem işletim sistemini izlemek için Azure Güvenlik Merkezi 'ni kullanabilirsiniz. İşletim sisteminden Güvenlik Merkezi tarafından toplanan veriler işletim sistemi türü ve sürümü, işletim sistemi (Windows olay günlükleri), çalışan süreçler, makine adı, IP adresleri ve oturum açan kullanıcı içerir. Log Analytics Aracısı Ayrıca kilitlenme bilgi döküm dosyalarını da toplar.

Daha fazla bilgi için aşağıdaki makaleleri inceleyin: 

- [Azure Izleyici ile Azure sanal makine iç konak günlüklerini toplama](../azure-monitor/learn/quick-collect-azurevm.md)
- [Azure Güvenlik Merkezi veri toplamayı anlama](../security-center/security-center-enable-data-collection.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma
***Rehberlik:** Azure Izleyici 'de, kuruluşunuzun uyumluluk düzenlemelerine göre Azure DevTest Labs örneklerinizin ilişkilendirildiği Log Analytics çalışma alanları için günlük tutma süresini ayarlayın.

Daha fazla bilgi için şu makaleye bakın: [günlük tutma parametrelerini ayarlama](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme
**Rehberlik:** Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve Azure DevTest Labs için toplanabilecek etkinlik günlüğü verilerine göre birçok diğer öngörü sağlamak için Log Analytics sorguları çalıştırın.

Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/diagnostic-settings.md)
- [Azure Izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve çözümleme](../azure-monitor/platform/activity-log.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir
**Rehberlik:** Azure Log Analytics çalışma alanını, Güvenlik günlüklerine ve Azure DevTest Labs ilgili olaylara yönelik anormal etkinlikleri izlemek ve uyarı almak için kullanın.

Daha fazla bilgi için şu makaleye bakın: [Log Analytics günlük verilerinde uyarı](../azure-monitor/learn/tutorial-response.md) alma

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme
**Rehberlik:** Uygulanamaz. Azure DevTest Labs kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir
**Rehberlik:** Uygulanamaz. Azure DevTest Labs DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir
**Rehberlik:** Azure DevTest Labs, müşteri tarafından sahip olunan ve yönetilen Azure Işlem makineleri oluşturur. İşlem oluşturma olayını ve alanını günlüğe kaydetmek için desteklenen tüm Azure Windows sanal makinelerinde Microsoft Monitoring Agent kullanın `CommandLine` . Desteklenen Azure Linux VM 'Leri için, konsol günlüğünü her düğüm temelinde el ile yapılandırabilir ve verileri depolamak için Syslog kullanabilirsiniz. Ayrıca, Azure Izleyici Log Analytics çalışma alanını kullanarak günlükleri gözden geçirin ve Azure VM 'lerinden günlüğe kaydedilen verilerde sorgular çalıştırın.

- [Azure Güvenlik Merkezinde veri toplama](../security-center/security-center-enable-data-collection.md#data-collection-tier)
- [Azure Izleyici 'de özel sorgular çalıştırma](../azure-monitor/log-query/get-started-queries.md)
- [Azure İzleyici'de Syslog veri kaynakları](../azure-monitor/platform/data-sources-syslog.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi
*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma
**Rehberlik:** Azure Active Directory (Azure AD), açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular çalıştırmak için Azure AD PowerShell modülünü kullanın.

- [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)
- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)
- [Azure DevTest Labs rolleri](devtest-lab-add-devtest-user.md)  

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme
**Rehberlik:** Azure Active Directory (Azure AD) varsayılan parola kavramına sahip değildir. Parola gerektiren diğer Azure kaynakları, karmaşıklık gereksinimleriyle ve hizmete bağlı olarak, en düşük parola uzunluğuna sahip bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanbilen üçüncü taraf uygulamalardan ve Market hizmetlerinden siz sorumlusunuz.

DevTest Labs varsayılan parola kavramına sahip değildir. 

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın
**Rehberlik:** Adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi veya yerleşik Azure Ilkeleri önerilerini kullanabilirsiniz; örneğin:

- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır

- [Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma](../security-center/security-center-identity-access.md)  
- [Azure Ilkesini kullanma](../governance/policy/tutorials/create-and-manage.md)
- [Azure DevTest Labs rolleri](devtest-lab-add-devtest-user.md)  

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın
**Rehberlik:** DevTest Labs kimlik yönetimi için Azure AD hizmetini kullanır. Kullanıcılara DevTest Labs tabanlı bir ortama erişim verdiğinizde, bu iki temel yönü göz önünde bulundurun:

- **Kaynak yönetimi:** Kaynakları yönetmek (VM oluşturma, ortamlar oluşturma, başlatma, durdurma, yeniden başlatma, silme ve yapıları uygulama vb.) için Azure portal erişim sağlar. Kaynak yönetimi, Azure 'da rol tabanlı erişim denetimi (RBAC) kullanılarak yapılır. Kullanıcılara roller atarsınız ve kaynak ve erişim düzeyi izinlerini ayarlarsınız.
- **Sanal makineler (ağ düzeyi)**: Varsayılan yapılandırmada VM 'ler bir yerel yönetici hesabı kullanır. Kullanılabilir bir etki alanı (Azure AD Domain Services, şirket içi etki alanı veya bulut tabanlı etki alanı) varsa, makineler etki alanına katılabilir. Kullanıcılar, makinelere bağlanmak için etki alanına yönelik bir yapıt kullanarak etki alanı tabanlı kimlikleri kullanabilir. 

- [DevTest Labs için başvuru mimarisi](devtest-lab-reference-architecture.md#architecture)
- [Azure AD ile SSO 'yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın
**Rehberlik:** Azure Active Directory (AD) Multi-Factor Authentication (MFA) etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)  
- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme:*** Evet

**Sorumluluk:** Müşterisi


### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın
**Rehberlik:** Azure kaynaklarında oturum açmak ve bunları yapılandırmak için yapılandırılmış MFA ile ayrıcalıklı erişim iş istasyonları (Paw) kullanın.

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](/windows-server/identity/securing-privileged-access/privileged-access-workstations)  
- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)  

**Azure Güvenlik Merkezi izleme:** yok

**Sorumluluk:** Müşterisi

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar
**Rehberlik:** Ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda günlüklerin ve uyarıların oluşturulması için Azure Active Directory (Azure AD) güvenlik raporları kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın.

- [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](../active-directory/identity-protection/overview-identity-protection.md)  
- [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md)  

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme
**Rehberlik:** IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek için adlandırılmış konumlarda koşullu erişim kullanın.

- [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)  

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın
**Rehberlik:** Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)  

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın
**Rehberlik:** Azure Active Directory (Azure AD) eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi İncelemeleri kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

- [Azure AD raporlamayı anlama](../active-directory/reports-monitoring/overview-reports.md)  
- [Azure kimlik erişimi incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)  

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme
**Rehberlik:** Herhangi bir güvenlik bilgisi ve olay yönetimi (SıEM)/Monitoring aracıyla tümleştirmenize olanak tanıyan Azure Active Directory (Azure AD) oturum açma etkinliği, denetim ve risk olay günlüğü kaynaklarına erişiminiz vardır.

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. Log Analytics çalışma alanı içinde uyarıları yapılandırabilirsiniz.

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)  

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar
**Rehberlik:** Otomatik yanıtları, Kullanıcı kimlikleriyle ilgili şüpheli eylemler için yapılandırmak üzere Azure Active Directory (Azure AD) riskini ve kimlik koruması özelliklerini kullanın.

- [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)  
- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)  

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama
**Rehberlik:** Müşteri Kasası Azure DevTest Labs için şu anda desteklenmiyor.

- [Desteklenen Müşteri Kasası hizmetleri listesi](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability) 

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi
*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma
**Rehberlik:** Azure DevTest Labs örneklerinizi ve ilgili kaynaklarınızı güvenli hale getirmek için Azure Güvenlik Merkezi önerilerini izleyin.

Microsoft, Azure DevTest Labs destekleyen temel kaynaklarda güvenlik açığı yönetimi gerçekleştirir.

- [Azure Güvenlik Merkezi önerilerini anlama](../security-center/recommendations-reference.md) 

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Paylaşılan

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma
**Rehberlik:** DevTest Labs içinde barındırılan Windows ve Linux sanal makinelerinize en son güvenlik güncelleştirmelerinin yüklendiğinden emin olmak için Azure Güncelleştirme Yönetimi kullanın. Windows VM 'Leri için Windows Update etkinleştirildiğinden ve otomatik olarak güncelleştirilecek şekilde ayarlandığından emin olun. Bu ayar şu anda DevTest Labs aracılığıyla yapılandırma için kullanılamaz, ancak Laboratuvar Yöneticisi/abonelik Yöneticisi bu ayarı aboneliklerindeki temel alınan işlem VM 'lerinde yapılandırabilir. 

- [Azure 'da VM 'Ler için Güncelleştirme Yönetimi Yapılandırma](../automation/update-management/update-mgmt-overview.md)
- [Güvenlik Merkezi tarafından izlenen Azure Güvenlik ilkelerini anlama](../security-center/security-center-policy-definitions.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma
***Rehberlik:*** Laboratuvar Yöneticisi olarak, güvenlik düzeltme ekleri ve diğer güncelleştirmeler de dahil olmak üzere laboratuar özel görüntülerinin güncelleştirmelerini otomatikleştirmek için [DevTest Labs yapılarını](add-artifact-vm.md) kullanabilirsiniz. 

Tüm istenen yapılandırmalara göre düzenli olarak görüntü oluşturup dağıtan bir yapılandırma olarak yapılandırma çözümü olan [DevTest Labs görüntü fabrikası](image-factory-create.md)hakkında daha fazla bilgi edinin. 

Abonelik Yöneticisi olarak, DevTest Labs VM 'Leri için güncelleştirmeleri ve düzeltme eklerini yönetmek üzere Azure Güncelleştirme Yönetimi çözümünü de kullanabilirsiniz. Güncelleştirme Yönetimi, desteklenen Windows sistemlerine yama yapmak için yerel olarak yapılandırılmış güncelleştirme deposuna bağımlıdır. System Center Updates Publisher (Updates Publisher) gibi araçlar, Windows Server Update Services (WSUS) içinde özel güncelleştirmeler yayımlamanıza olanak sağlar. Bu senaryo Güncelleştirme Yönetimi, üçüncü taraf yazılımlarla güncelleştirme deposu olarak Configuration Manager kullanan makinelere yaması sağlar.

- [Azure 'da Güncelleştirme Yönetimi çözümü](../automation/update-management/update-mgmt-overview.md)
- [VM 'niz için güncelleştirmeleri ve düzeltme eklerini yönetme](../automation/update-management/update-mgmt-overview.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın
**Rehberlik:** Tarama sonuçlarını tutarlı aralıklarla dışarı aktarın ve güvenlik açıklarının düzeltildiğini doğrulamak için sonuçları karşılaştırın. Azure Güvenlik Merkezi tarafından önerilen güvenlik açığı yönetimi önerisi kullanılırken, müşteri, geçmiş tarama verilerini görüntülemek için seçili çözümün portalına pivot olarak eklenebilir.

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın
**Rehberlik:** Azure Güvenlik Merkezi tarafından sunulan varsayılan risk derecelendirmelerini (güvenli puan) kullanın.

- [Azure Güvenlik Merkezi güvenli Puanını anlama](../security-center/secure-score-security-controls.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi
*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma
**Rehberlik:** Abonelikleriniz dahilinde tüm kaynakları (DevTest Labs kaynakları dahil) sorgulamak ve öğrenmek için Azure Kaynak grafiğini kullanın. Kiracınızda uygun (okuma) izinleriniz olduğundan ve aboneliklerinizde tüm Azure aboneliklerini ve kaynaklarını numaralandırabilmeniz için emin olun.

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)
- [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)
- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma
**Rehberlik:** Meta verileri bir taksonomiye göre mantıksal olarak düzenlemek için meta veriler sağlayan Azure kaynaklarına Etiketler uygulayın.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)
- [DevTest Labs için Etiketler yapılandırma](devtest-lab-add-tag.md)

**Azure Güvenlik Merkezi izleme:** Kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme
**Rehberlik:** Labs ve laboratuvara ilgili kaynakları düzenlemek ve izlemek için etiketleme, yönetim grupları ve ayrı abonelikler ve uygun yerlerde farklı laboratuvarlar kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların abonelikten hızlıca silindiğinden emin olun.

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)
- [Yönetim Grupları oluşturma](../governance/management-groups/create.md)
- [DevTest Labs kullanarak laboratuvar oluşturma](devtest-lab-create-lab.md)
- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)
- [Laboratuvar için etiketleri yapılandırma](devtest-lab-add-tag.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın
**Rehberlik:** Kurumsal ihtiyaçlarına göre işlem kaynakları için onaylanan Azure kaynakları ve onaylanan yazılım envanterini oluşturun. Abonelik Yöneticisi olarak, yapılandırılmış laboratuvar makineleri gruplarında çalışmasına izin verilen bir uygulamalar kümesi tanımlamanıza yardımcı olması için Azure Güvenlik Merkezi 'nin bir özelliği olan Uyarlamalı uygulama denetimlerini de kullanabilirsiniz. Bu özellik hem Azure hem de Azure olmayan Windows (tüm sürümler, klasik veya Azure Resource Manager) ve Linux makineler için kullanılabilir.

- [Uyarlamalı uygulama denetimini etkinleştirme](../security-center/security-center-adaptive-application.md)
 
**Azure Güvenlik Merkezi izleme:** Geçerli **sorumluluk değil:** müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici
**Rehberlik:** Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur. Depolama hesapları gibi yüksek güvenlik tabanlı ortamlarda yardımcı olabilir.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici
**Rehberlik:** Azure Otomasyonu, dağıtım, işlemler ve iş yüklerinin ve kaynakların yetkisini alma sırasında tamamen denetim sağlar. Abonelik Yöneticisi olarak, aboneliğinizdeki DevTest Labs sanal makinelerinde bulunan tüm yazılımlarla ilgili bilgi toplamayı otomatik hale getirmek için Azure sanal makine envanterinden yararlanabilirsiniz. Yazılım adı, sürüm, yayımcı ve yenileme süresi özellikleri Azure portal kullanılabilir. Yüklemenin tarih ve diğer bilgilerine erişim sağlamak için, müşteri, Konuk düzeyinde tanılamayı etkinleştirmek ve Windows olay günlüklerini bir Log Analytics çalışma alanına getirmelerini sağlamak için gereklidir.

Yazılım uygulamalarının izlenmesi için Değişiklik İzleme kullanmanın yanı sıra, Azure Güvenlik Merkezi 'ndeki Uyarlamalı uygulama denetimleri, makinelerinizde çalışan uygulamaları analiz etmek ve bu zekası aracılığıyla bir izin verilenler listesi oluşturmak için makine öğrenimini kullanır. Bu özellik, uygulama izin verilenler listesi ilkelerini yapılandırma ve sürdürme sürecini önemli ölçüde basitleştirir. böylece, ortamınızda istenmeyen yazılımların kullanılmasını önleyebilirsiniz. Denetim modunu veya zorlama modunu yapılandırabilirsiniz. Denetim modu yalnızca korumalı VM 'lerdeki etkinliği denetler. Zorla modu kuralları zorunlu kılar ve çalışmasına izin verilmeyen uygulamaların engellenmiş olmasını sağlar. 

- [Azure Otomasyonu’na giriş](../automation/automation-intro.md)
- [Azure VM envanterini etkinleştirme](../automation/automation-tutorial-installed-software.md)
- [Uyarlamalı uygulama denetimlerini anlama](../security-center/security-center-adaptive-application.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi


### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma
**Rehberlik:** Azure Otomasyonu, dağıtım, işlemler ve iş yüklerinin ve kaynakların yetkisini alma sırasında tamamen denetim sağlar. Abonelik Yöneticisi olarak, DevTest Labs 'de barındırılan VM 'lerde yüklü tüm yazılımları tanımlamak için Değişiklik İzleme kullanabilirsiniz. Kendi işleminizi uygulayabilir veya yetkisiz yazılımı kaldırmak için Azure Otomasyonu durum yapılandırması ' nı kullanabilirsiniz.

- [Azure Otomasyonu’na giriş](../automation/automation-intro.md)
- [Değişiklik İzleme çözümü ile ortamınızdaki değişiklikleri izleyin](../automation/change-tracking.md)
- [Azure Otomasyonu durum yapılandırmasına genel bakış](../automation/automation-dsc-overview.md)

**Azure Güvenlik Merkezi izleme:** Kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan
**Rehberlik:** Abonelik Yöneticisi olarak, yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların DevTest Labs 'de barındırılan Azure VM 'lerde yürütülmesini engellediği sağlamak için Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanabilirsiniz.

- [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](../security-center/security-center-adaptive-application.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın
**Rehberlik:** Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri abonelikleri içinde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure ilkesini kullanın: 

- İzin verilmeyen kaynak türleri
- İzin verilen kaynak türleri

Aşağıdaki makalelere bakın: 
- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/not-allowed-resource-types.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi


### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: onaylanan yazılım başlıkları envanterini koruyun
**Rehberlik:** Uyarlamalı uygulama denetimi, DevTest Labs 'de barındırılan Azure ve Azure dışı makinelerde (Windows ve Linux) hangi uygulamaların çalıştırılacağını denetlemenize yardımcı olan Azure Güvenlik Merkezi 'ndeki akıllı, otomatik, uçtan uca bir çözümdür. Bu ayarı DevTest Labs 'de barındırılan temeldeki işlem kaynakları için yapılandırabilmek üzere bir abonelik yöneticisi olmanız gerektiğini unutmayın. Bu ayar kuruluşunuzun gereksinimini karşılamıyorsa üçüncü taraf çözümünü uygulayın.

- [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](../security-center/security-center-adaptive-application.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın
**Rehberlik:** **Microsoft Azure Yönetim** uygulaması için "* *" **blok erişimini**yapılandırarak kullanıcıların Azure Resource Manager etkileşim kurmasına Izin vermek için Azure koşullu erişimi kullanın.

- [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın
**Rehberlik:** Betiklerin türüne bağlı olarak, kullanıcıların DevTest Labs 'de barındırılan VM 'lerdeki betikleri yürütme yeteneğini sınırlandırmak için işletim sistemine özgü yapılandırma veya üçüncü taraf kaynakları kullanabilirsiniz. Ayrıca, Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini yalnızca yetkili yazılımın yürütüldüğünü ve tüm yetkisiz yazılımların temel alınan Azure VM 'lerinde yürütülmesini engellediği şekilde kullanabilirsiniz.

- [Windows ortamlarında PowerShell betiği yürütmeyi denetleme](/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6)
- [Azure Güvenlik Merkezi Uyarlamalı uygulama denetimlerini kullanma](../security-center/security-center-adaptive-application.md)

**Azure Güvenlik Merkezi izleme:** Kullanılamıyor

**Sorumluluk:** Müşterisi


### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel veya mantıksal olarak ayırt edin
**Rehberlik:** Azure ortamınızda dağıtılan yüksek riskli uygulamalar, sanal ağ, alt ağ, abonelikler, yönetim grupları vb. kullanılarak yalıtılabilir. ve bir Azure Güvenlik Duvarı, Web uygulaması güvenlik duvarı (WAF) veya ağ güvenlik grubu (NSG) ile yeterince güvenli hale getirilir.

- [DevTest Labs için sanal ağı yapılandırma](devtest-lab-configure-vnet.md)
- [Azure Güvenlik Duvarı 'na genel bakış](../firewall/overview.md)
- [Web uygulaması güvenlik duvarına genel bakış](../web-application-firewall/overview.md)
- [Ağ güvenliğine genel bakış](../virtual-network/security-overview.md)
- [Azure sanal ağına genel bakış]()
- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../governance/management-groups/overview.md)
- [Abonelik kararı kılavuzu](/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Azure Güvenlik Merkezi izleme:** Kullanılamıyor

**Sorumluluk:** Müşterisi


## <a name="malware-defense"></a>Kötü amaçlı yazılımdan koruma
*Daha fazla bilgi için bkz. güvenlik denetimi: kötü amaçlı yazılımdan koruma.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma
**Rehberlik:** Kaynaklarınızı sürekli olarak izlemek ve savunmak için Azure Cloud Services ve sanal makineler için Microsoft kötü amaçlı yazılımdan koruma kullanın. Linux için üçüncü taraf kötü amaçlı yazılımdan koruma çözümünü kullanın. Ayrıca, depolama hesaplarına yüklenen kötü amaçlı yazılımları algılamak için veri Hizmetleri için Azure Güvenlik Merkezi 'nin tehdit algılamasını kullanın.

- Cloud Services ve sanal makineler için Microsoft Antimalware 'i yapılandırma
- Azure Güvenlik Merkezi’nde tehdit koruması

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara
**Rehberlik:** Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayar üzerinde (örneğin, laboratuarda barındırılan Azure App Service) etkinleştirilir, ancak içeriğiniz üzerinde çalışmaz.
App Service, Data Lake Storage, BLOB depolama vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm dosyaları önceden tarayın.

Depolama hesaplarına yüklenen kötü amaçlı yazılımları algılamak için veri Hizmetleri için Azure Güvenlik Merkezi 'nin tehdit algılamasını kullanın.

- Azure Cloud Services ve sanal makineler için Microsoft Antimalware 'i anlayın
- Azure Güvenlik Merkezi 'nin veri Hizmetleri tehdit algılamasını anlama

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Uygulanamaz


### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun
**Rehberlik:** Dağıtıldığında, Azure için Microsoft Antimalware, varsayılan olarak en son imza, platform ve altyapı güncelleştirmelerini otomatik olarak yükler. Azure Güvenlik Merkezi 'ndeki önerileri takip edin: "Işlem & uygulamalar" bölümünde, DevTest Labs 'in temel alınan işlem kaynakları için tüm uç noktaların en son imzalarla güncel olduğundan emin olun. Windows işletim sistemi, Azure Güvenlik Merkezi ile tümleşen Microsoft Defender Gelişmiş tehdit koruması hizmeti ile virüs veya kötü amaçlı yazılım tabanlı saldırılar riskini sınırlamak üzere ek güvenlik ile daha da korunabilir.

- Azure Cloud Services ve sanal makineler için Microsoft Antimalware nasıl dağıtılır
- Microsoft Defender Gelişmiş Tehdit Koruması

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

## <a name="data-recovery"></a>Veri kurtarma
*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli otomatik arka pencerelere emin olun
**Rehberlik:** Şu anda Azure DevTest Labs VM yedeklemelerini ve anlık görüntülerini desteklemez. Ancak, DevTest Labs 'de barındırılan temel Azure VM 'lerinde Azure Backup etkinleştirebilir ve yapılandırabilirsiniz. Ayrıca, temeldeki işlem kaynaklarına uygun erişiminiz olduğu sürece otomatik yedeklemeler için istenen sıklık ve bekletme süresini de yapılandırabilirsiniz. 

- [Azure VM yedeklemesine genel bakış](../backup/backup-azure-vms-introduction.md)
- [VM ayarlarından bir Azure VM 'yi yedekleme](../backup/backup-azure-vms-first-look-arm.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin
**Rehberlik:** Şu anda Azure DevTest Labs VM yedeklemelerini ve anlık görüntülerini desteklemez. Ancak, temel alınan işlem kaynaklarına uygun erişiminiz olduğu sürece, DevTest Labs veya bu örneklere eklenen yönetilen diskler için, PowerShell veya REST API 'Leri kullanılarak barındırılan temel Azure sanal makinelerinizin anlık görüntülerini oluşturabilirsiniz. Ayrıca, Azure Key Vault içinde müşteri tarafından yönetilen tüm anahtarları da yedekleyebilirsiniz.

Hedef Azure VM 'lerinde, istenen sıklık ve bekletme dönemlerinde Azure Backup etkinleştirin. Sistem durumu yedeklemesini tamamen içerir. Azure Disk Şifrelemesi kullanıyorsanız, Azure VM yedeklemesi, müşteri tarafından yönetilen anahtarların yedeklemesini otomatik olarak işler.

- [Şifreleme kullanan Azure VM 'lerinde yedekleme](../backup/backup-azure-vms-encryption.md)
- [Azure VM yedeklemesine genel bakış](../backup/backup-azure-vms-introduction.md)
- [Azure VM yedeklemesine genel bakış](../backup/backup-azure-vms-introduction.md)
- [Azure 'da Key Vault anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama
**Rehberlik:** Azure Backup içinde içeriğin düzenli olarak veri geri yüklemesini gerçekleştirme olanağı sağlayın. Gerekirse, içeriği yalıtılmış bir sanal ağa veya aboneliğe geri yüklemeyi test edin. Ayrıca, yedeklenen müşteri tarafından yönetilen anahtarların sınamasını geri yükleme.

Azure Disk Şifrelemesi kullanıyorsanız, Azure VM 'yi disk şifreleme anahtarlarıyla geri yükleyebilirsiniz. Disk şifrelemeyi kullanırken, Azure VM 'yi disk şifreleme anahtarlarıyla geri yükleyebilirsiniz.

- [Şifreleme kullanan Azure VM 'lerinde yedekleme](../backup/backup-azure-vms-encryption.md)
- [Azure VM yedeğinden dosyaları kurtarma](../backup/backup-azure-restore-files-from-vm.md)
- [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)
- [Şifrelenmiş bir VM 'yi yedekleme ve geri yükleme](../backup/backup-azure-vms-encryption.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın
**Rehberlik:** Yönetilen diskleri Azure Backup yedeklediyseniz, VM 'Ler Depolama Hizmeti Şifrelemesi (SSE) ile geri kalanıyla şifrelenir. Azure Backup, Azure disk şifrelemesi kullanılarak şifrelenmiş Azure VM 'Leri de yedekleyebilir. Azure disk şifrelemesi, bir anahtar kasasında gizli dizi olarak korunmuş olan BitLocker şifreleme anahtarları (BEKs) ile tümleşir. Azure disk şifrelemesi Ayrıca Azure Key Vault anahtar şifreleme anahtarları (KEKs) ile tümleşir. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault geçici silme özelliğini etkinleştirin.

- [VM 'Ler için geçici silme](../backup/soft-delete-virtual-machines.md)
- [Azure Key Vault-geçici genel bakış](../key-vault/general/soft-delete-overview.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

## <a name="incident-response"></a>Olay yanıtı
*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma
**Rehberlik:** Kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerini ve olay işleme/yönetim aşamalarını, algılama aşamasından olay sonrası incelemeye göre tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)
- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)
- [Kendi olay yanıtı planınızı oluşturmaya yardımcı olmak için NıST 'nin bilgisayar güvenliği olay işleme kılavuzuyla yararlanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma
**Rehberlik:** Azure Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin uyarıyı vermek için kullanılan bulma veya analizte ne kadar önemli olduğunu ve uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu bilmenin yanı sıra güven düzeyini temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) etiketleri kullanarak Azure kaynaklarını açıkça tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun. Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır.

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md)
- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları
**Rehberlik:** Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıtı yeteneklerini düzenli bir temposunda test etmek üzere alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin Yayımlama Kılavuzu, BT planları ve becerileri için programları test etme, eğitim ve alıştırma Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın
**Rehberlik:** Microsoft Güvenlik Yanıt Merkezi (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme
**Rehberlik:** Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)
- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

#### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme
**Rehberlik:** Azure Güvenlik Merkezi 'nde Iş akışı Otomasyonu özelliğini kullanarak, güvenlik uyarılarındaki "Logic Apps" aracılığıyla yanıtları otomatik olarak tetikleyin ve Azure kaynaklarınızı korumaya yönelik öneriler alın.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)
 
Azure Güvenlik Merkezi izleme: * * * * uygulanamaz

**Sorumluluk:** Müşterisi


## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları
*Daha fazla bilgi için bkz. güvenlik denetimi: [Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*


### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin
**Rehberlik:** Sızma testlerinizin Microsoft ilkelerini ihlal etmemesini sağlamak için Microsoft katılım kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın.

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)
- [Microsoft bulutu kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleye bakın:

- [Azure DevTest Labs ortamlar için güvenlik uyarıları](environment-security-alerts.md)
