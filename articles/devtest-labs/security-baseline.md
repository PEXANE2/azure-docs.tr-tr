---
title: Azure DevTest Labs için Azure Güvenlik temeli
description: Azure DevTest Labs için Azure Güvenlik temeli
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: 49b07242068df5d7c46c602140c8b3e1f778e90c
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89398332"
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
**Rehberlik:** Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Etkinlik günlükleri, yönetim düzlemi düzeyinde Azure DevTest Labs örneklerinizin üzerinde gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Azure etkinlik günlüğü verilerini kullanarak, DevTest Labs örnekleriniz için yönetim düzlemi düzeyinde herhangi bir yazma işlemi (PUT, POST, DELETE) için "ne, kim ve ne zaman" belirleyebilirsiniz.

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

- **Kaynak yönetimi:** Kaynakları yönetmek (VM oluşturma, ortamlar oluşturma, başlatma, durdurma, yeniden başlatma, silme ve yapıları uygulama vb.) için Azure portal erişim sağlar. Azure rol tabanlı erişim denetimi (Azure RBAC) kullanılarak kaynak yönetimi Azure 'da yapılır. Kullanıcılara roller atarsınız ve kaynak ve erişim düzeyi izinlerini ayarlarsınız.
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

## <a name="data-protection"></a>Veri Koruma
*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma
**Rehberlik:** Gizli bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/resource-group-using-tags.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma
**Rehberlik:** Geliştirme, test ve üretim için ayrı abonelikler veya yönetim grupları uygulayın. Azure DevTest Labs örneklerin sanal ağ/alt ağ ile ayrılması ve uygun şekilde etiketlenmesi gerekir. 

- [Ek Azure abonelikleri oluşturma](../billing/billing-create-subscription.md)
- [Yönetim grupları oluşturma](../governance/management-groups/create.md)
- [DevTest Labs için sanal ağ yapılandırma](devtest-lab-configure-vnet.md)
- [Etiketler oluşturma ve kullanma](../azure-resource-manager/resource-group-using-tags.md)
- [DevTest Labs için Etiketler oluşturma ve kullanma](devtest-lab-add-tag.md)

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme
**Rehberlik:** Henüz kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure DevTest Labs için kullanılamaz.

Microsoft, Azure DevTest Labs için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin
**Rehberlik:** Azure DevTest Labs, varsayılan olarak TLS şifreli iletişim gerektirir. TLS sürümleri 1,2 Şu anda destekleniyor. İstemci kitaplığınız veya aracınız TLS desteklemiyorsa, şifrelenmemiş bağlantıları etkinleştirmek Azure portal veya yönetim API 'Leri aracılığıyla yapılabilir. Şifrelenmiş bağlantıların mümkün olmadığı durumlarda, laboratuvar ve istemci uygulamasının sanal bir ağa yerleştirilmesi önerilir.

[DevTest Labs için iletim senaryosunda şifrelemeyi anlama](https://techcommunity.microsoft.com/t5/azure-developer-community-blog/azure-devtest-labs-enforcing-tls-1-2-starting-may-01-2020/ba-p/1236279)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın
**Rehberlik:** Veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure DevTest Labs için kullanılamaz. Gizli bilgiler içeren örnekleri Etiketler ve uyumluluk amaçları için gerekliyse üçüncü taraf çözümü uygular.

Microsoft tarafından yönetilen temel platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme:** Şu anda kullanılamıyor

**Sorumluluk:** Müşterisi

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma
**Rehberlik:** Azure DevTest Labs 'daki laboratuvarlara erişimi denetlemek için Azure rol tabanlı erişim denetimi (Azure RBAC) kullanın.

- [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)
- [DevTest Labs 'de rolleri anlayın](devtest-lab-add-devtest-user.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın
**Rehberlik:** DevTest Labs 'nin bir parçası olarak oluşturulan işlem kaynaklarında uyumluluk için gerekliyse, verileri bir sistemden kopyalandıklarında bile verilere erişim denetimlerine zorlamak için otomatik ana bilgisayar tabanlı veri kaybı önleme çözümü gibi bir üçüncü taraf aracı uygulayın.

Microsoft tarafından yönetilen temel platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Uygulanamaz

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin
**Rehberlik:** Azure DevTest Labs aşağıdaki müşteri verilerini depolar:

- Yapıtları uygulamadan oluşturulan dağıtım ve uzantı günlüklerini içeren [yapıt sonuçları](add-artifact-vm.md)
- Formüllerden sanal makineler oluşturmak için kullanılan [Formül belgeleri](devtest-lab-manage-formulas.md)
- Laboratuvar sanal makineleri için işletim sistemi ve veri diskleri 

Yapıt sonuçları ve formül belgeleri, her laboratuvar dağıtımının bir parçası olarak oluşturulan bir Azure depolama hesabına gönderilir. Azure depolama 'daki veriler, 256 bit AES şifrelemesi kullanılarak şifrelenmiş ve şifresi çözülür, en güçlü blok şifrelemeleri kullanılabilir ve FIPS 140-2 uyumludur. Azure depolama şifrelemesi devre dışı bırakılamaz. Depolama hesabınızın şifrelenmesi için Microsoft tarafından yönetilen anahtarları kullanabilir veya kendi anahtarınızla şifrelemeyi yönetebilirsiniz. Daha fazla bilgi için bkz. [Laboratuvar depolama hesabı Için şifreleme](encrypt-storage.md).

Varsayılan olarak, tüm laboratuar işletim sistemi ve veri diskleri, platform tarafından yönetilen bir anahtarla şifrelenir. Mevcut yönetilen disklere yazılan tüm yönetilen diskler, anlık görüntüler, görüntüler ve veriler, platform tarafından yönetilen anahtarlarla otomatik olarak şifrelenir. Laboratuvar sahibi olarak, laboratuvar işletim sistemi disklerini, müşteri tarafından yönetilen bir anahtarla şifrelenecek şekilde yapılandırabilirsiniz. Laboratuvar veri diskleri için müşteri tarafından yönetilen anahtar kullanan şifreleme Şu anda laboratuvar aracılığıyla yapılandırılamaz. Ancak bir abonelik Yöneticisi, bu ayarı bir abonelik içindeki laboratuvar diskleri için şimdilik yapılandırabilir. Daha fazla bilgi için bkz. [müşteri tarafından yönetilen anahtarları kullanarak Lab DevTest Labs işletim sistemi disklerini şifreleme](encrypt-disks-customer-managed-keys.md).

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı
**Rehberlik:** DevTest Labs örneklerine ve diğer kritik veya ilgili kaynaklara yapılan değişikliklerin ne zaman gerçekleştiği hakkında uyarı oluşturmak için Azure Izleyici ile Azure etkinlik günlüğünü kullanın.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)
- [DevTest Labs etkinlik günlüğü olayları için uyarı oluşturma](create-alerts.md)

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
**Rehberlik:** Azure Otomasyonu, dağıtım, işlemler ve iş yüklerinin ve kaynakların yetkisini alma sırasında tamamen denetim sağlar. Abonelik Yöneticisi olarak, aboneliğinizdeki DevTest Labs sanal makinelerinde bulunan tüm yazılımlarla ilgili bilgi toplamayı otomatik hale getirmek için Azure sanal makine envanterini kullanabilirsiniz. Yazılım adı, sürüm, yayımcı ve yenileme süresi özellikleri Azure portal kullanılabilir. Yüklemenin tarih ve diğer bilgilerine erişim sağlamak için, müşteri, Konuk düzeyinde tanılamayı etkinleştirmek ve Windows olay günlüklerini bir Log Analytics çalışma alanına getirmelerini sağlamak için gereklidir.

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
**Rehberlik:** Uyarlamalı uygulama denetimi, DevTest Labs 'de barındırılan Azure ve Azure dışı makinelerde (Windows ve Linux) hangi uygulamaların çalıştırılacağını denetlemenize yardımcı olan Azure Güvenlik Merkezi 'ndeki akıllı, otomatik, uçtan uca bir çözümdür. Bu ayarı, DevTest Labs 'de barındırılan temeldeki işlem kaynakları için yapılandırmak üzere bir abonelik yöneticisi olmanız gerektiğini unutmayın. Bu ayar kuruluşunuzun gereksinimini karşılamıyorsa üçüncü taraf çözümünü uygulayın.

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

## <a name="secure-configuration"></a>Güvenli yapılandırma
**Daha fazla bilgi için bkz. güvenlik denetimi: güvenli yapılandırma.**

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma
**Rehberlik:** DevTest Labs 'in bir parçası olarak oluşturulan Azure kaynaklarınızın yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure Ilke diğer adlarını kullanın. Yerleşik Azure Ilke tanımlarını da kullanabilirsiniz.

Ayrıca, Azure Resource Manager, yapılandırmanın kuruluşunuzun güvenlik gereksinimlerini karşıladığından/aştığından emin olmak için gözden geçirilmesi gereken JavaScript Nesne Gösterimi (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

Azure Güvenlik Merkezi 'nin önerilerini Azure kaynaklarınız için güvenli bir yapılandırma temeli olarak da kullanabilirsiniz.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)
- [Öğretici: uyumluluğu zorlamak için ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)
- [Güvenlik önerileri-bir başvuru kılavuzu](../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma
**Rehberlik:** DevTest Labs 'nin bir parçası olarak oluşturulan tüm temeldeki işlem kaynaklarında güvenlik yapılandırmalarının bakımını yapmak için Azure Güvenlik Merkezi önerilerini kullanın. Ayrıca, kuruluşunuz tarafından istenen işletim sisteminin güvenlik yapılandırmasını oluşturmak için özel işletim sistemi görüntülerini veya Azure Automation durum yapılandırması veya DevTest Labs yapılarını kullanabilirsiniz.

- [Azure Güvenlik Merkezi önerilerini izleme](../security-center/security-center-recommendations.md)
- [Güvenlik önerileri-bir başvuru kılavuzu](../security-center/recommendations-reference.md)
- [Azure Otomasyonu durum yapılandırmasına genel bakış](../automation/automation-dsc-overview.md)
- [Bir VHD 'YI karşıya yükleyin ve Azure 'da yeni Windows VM 'Leri oluşturmak için kullanın](../virtual-machines/windows/upload-generalized-managed.md)
- [Azure CLı ile özel diskten bir Linux VM oluşturma](../virtual-machines/linux/upload-vhd.md)
- [Özel görüntüleri oluşturma ve birden çok DevTest Labs 'e dağıtma](image-factory-save-distribute-custom-images.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Uygulanamaz

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma
**Rehberlik:** DevTest Labs 'in bir parçası olarak oluşturulan Azure kaynaklarınız genelinde güvenli ayarları zorlamak için Azure Ilkesi **reddetme** ve **dağıtım yok** kurallarını kullanın. Ayrıca, kuruluşunuz tarafından gerekli olan Azure kaynaklarınızın güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonları kullanabilirsiniz.

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)
- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
- [Azure Resource Manager şablonlarına genel bakış](../azure-resource-manager/templates/overview.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma
**Rehberlik:** Laboratuvarın bir parçası olarak oluşturulan temel Azure işlem kaynaklarınız üzerinde güvenlik açığı değerlendirmeleri gerçekleştirmeye yönelik Azure Güvenlik Merkezi önerilerini izleyin. Ayrıca, kuruluşunuz tarafından istenen işletim sisteminin güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonları, özel işletim sistemi görüntüleri veya Azure Otomasyonu durum yapılandırması ' nı kullanabilirsiniz. Ayrıca, istenen tüm yapılandırmalara göre düzenli olarak görüntü oluşturup dağıtan bir kod yapılandırma çözümü olan Image Factory çözümünü de kullanabilirsiniz.

Ayrıca, Microsoft tarafından yayımlanan Azure Market sanal makine görüntüleri Microsoft tarafından yönetilir ve sürdürülür.

- [Azure Güvenlik Merkezi güvenlik açığı değerlendirmesi önerilerini uygulama](../security-center/security-center-vulnerability-assessment-recommendations.md)
- [Azure Otomasyonu durum yapılandırmasına genel bakış](../automation/automation-dsc-overview.md)
- [Bir VHD’yi Azure’a yüklemek ve yeni bir sanal makine oluşturmak için örnek betik](../virtual-machines/scripts/virtual-machines-windows-powershell-upload-generalized-script.md)
- [DevTest Labs 'de görüntü fabrikası oluşturma](image-factory-create.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Paylaşılan

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın
**Rehberlik:** Kodunuzu özel Azure ilkeleri, Azure Resource Manager şablonları ve Istenen durum yapılandırması betikleri gibi güvenli bir şekilde depolamak ve yönetmek için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynaklara erişmek için, Azure DevOps ile tümleşikse, belirli kullanıcılar, yerleşik güvenlik grupları veya Azure Active Directory (Azure AD) içinde tanımlanan gruplar için izin verebilir veya vermeyebilirsiniz.

- [Git öğreticisi Azure Repos](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow)
- [İzinler ve gruplar hakkında](https://docs.microsoft.com/azure/devops/organizations/security/about-permissions?view=azure-devops&tabs=preview-page)
- [Azure DevTest Labs ile Azure DevOps iş akışı arasında tümleştirme](devtest-lab-dev-ops.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın
**Rehberlik:** Özel görüntüler kullanıyorsanız, görüntülere yalnızca yetkili kullanıcıların erişebildiğinden emin olmak için Azure rol tabanlı erişim denetimi 'ni (Azure RBAC) kullanın. Paylaşılan bir görüntü Galerisi 'ni kullanarak görüntülerinizi, ihtiyaç duyulan belirli laboratuvarlara paylaşabilirsiniz. Kapsayıcı görüntüleri için, bunları Azure Container Registry depolayın ve yalnızca yetkili kullanıcıların görüntülere erişebildiğinden emin olmak için Azure RBAC kullanın.

- [Azure RBAC 'yi anlama](../role-based-access-control/rbac-and-directory-admin-roles.md)
- [Azure RBAC 'yi yapılandırma](../role-based-access-control/quickstart-assign-role-user-portal.md)
- [DevTest Labs için paylaşılan görüntü Galerisi yapılandırma](configure-shared-image-gallery.md)
- [Container Registry için Azure RBAC 'nı anlama](../container-registry/container-registry-roles.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma
**Rehberlik:** Azure Ilkesi 'ni kullanarak Azure kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. DevTest Labs altında oluşturulan Azure kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure Ilke diğer adlarını kullanın. Ayrıca, belirli kaynaklarınızla ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz. Ayrıca, Azure Otomasyonu ' nu yapılandırma değişikliklerini dağıtmak için de kullanabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
- [Diğer adları kullanma](../governance/policy/concepts/definition-structure.md#aliases)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma
**Rehberlik:** Azure Otomasyonu durum yapılandırması, herhangi bir bulutta veya şirket içi veri merkezinde Istenen durum yapılandırması (DSC) düğümleri için bir yapılandırma yönetim hizmetidir. Makineleri kolayca ekleyebilir, bunlara bildirime dayalı yapılandırmalara atayabilir ve her makinenin, belirttiğiniz istenen duruma göre uyumluluğunu gösteren raporları görüntüleyebilirsiniz. Ayrıca, kurumsal ilkelerin izlediklerinden emin olmak için her laboratuvar makinesinde yüklenebilen özel bir yapıt da yazabilirsiniz. 

- [Azure Otomasyonu durum yapılandırmasına göre yönetim için makine ekleme](../automation/automation-dsc-onboarding.md)
- [DevTest Labs sanal makineleri için özel yapılar oluşturma](devtest-lab-artifact-author.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın
**Rehberlik:** DevTest Labs altında oluşturulan Azure kaynaklarınıza yönelik temel taramalar gerçekleştirmek için Azure Güvenlik Merkezi 'ni kullanın. Ayrıca Azure Ilkesi 'ni kullanarak Azure Kaynak konfigürasyonları 'nı uyarır ve denetleyin.

- [Azure Güvenlik Merkezi 'nde öneriler nasıl düzeltileceği](../security-center/security-center-remediate-recommendations.md)
 
**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula
**Rehberlik:** Kapsayıcılar için işletim sistemi ve Docker ayarlarına yönelik temel taramalar gerçekleştirmek üzere Azure Güvenlik Merkezi 'ni kullanın.

- [Azure Güvenlik Merkezi'ndeki kapsayıcı önerilerini anlama](../security-center/security-center-container-recommendations.md)

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

**Sorumluluk:** Müşterisi

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin
**Rehberlik:** Bulut uygulamalarınız için gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault ile birlikte Yönetilen Hizmet Kimliği kullanın.

- [DevTest Labs 'de Azure Resource Manager ortamları dağıtmak için yönetilen kimlik yapılandırma](use-managed-identities-environments.md)
- [DevTest Labs 'de sanal makineleri dağıtmak için yönetilen kimlik yapılandırma](enable-managed-identities-lab-vms.md)
- [Anahtar Kasası oluşturma](../key-vault/quick-create-portal.md)
- [Key Vault kimlik doğrulaması yapma](../key-vault/general/authentication.md)
- [Key Vault erişim ilkesi atama](../key-vault/general/assign-access-policy-portal.md)

**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme
**Rehberlik:** Azure AD 'de otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

- [DevTest Labs 'de Azure Resource Manager ortamları dağıtmak için yönetilen kimlik yapılandırma](use-managed-identities-environments.md)
- [DevTest Labs 'de sanal makineleri dağıtmak için yönetilen kimlik yapılandırma](enable-managed-identities-lab-vms.md)
 
**Azure Güvenlik Merkezi izleme:** Yes

**Sorumluluk:** Müşterisi

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın
**Rehberlik:** Kod içinde kimlik bilgilerini tanımlamak için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

- Kimlik bilgisi tarayıcısını ayarlama

**Azure Güvenlik Merkezi izleme:** Uygulanamaz

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
App Service, Data Lake Storage, BLOB depolama gibi işlem dışı Azure kaynaklarına yüklenen tüm dosyaları önceden tarayın.

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
- [Kendi olay yanıtı planınızın oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay işleme kılavuzunu kullanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

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
