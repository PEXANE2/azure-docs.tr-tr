---
title: Yedekleme için Azure Güvenlik temeli
description: Yedekleme için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 72c038bda88ecc9b7518f1f764a41bd4c49f52c6
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88589427"
---
# <a name="azure-security-baseline-for-backup"></a>Yedekleme için Azure Güvenlik temeli

Yedekleme için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview.md)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](../security/benchmarks/security-baselines-overview.md).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Rehberlik**: uygulanamaz; bir sanal ağ, alt ağ veya ağ güvenlik grubunu bir kurtarma hizmetleri kasası ile ilişkilendiremezsiniz. Azure sanal makinesi yedeklenirken, veriler Azure omurgası üzerinden aktarılır. Şirket içi bir makineden yedeklenirken, Azure 'da belirli bir uç nokta ile şifrelenmiş bir tünel oluşturulur ve kimlik bilgileri, şifrelenen tünelden gönderilmeden önce verileri önceden şifrelemek için kullanılır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Rehberlik**: uygulanamaz; bir sanal ağ, alt ağ veya ağ güvenlik grubunu bir kurtarma hizmetleri kasası ile ilişkilendiremezsiniz. Azure sanal makinesi yedeklenirken, veriler Azure omurgası üzerinden aktarılır. Şirket içi bir makineden yedeklenirken, Azure 'da belirli bir uç nokta ile şifrelenmiş bir tünel oluşturulur ve kimlik bilgileri, şifrelenen tünelden gönderilmeden önce verileri önceden şifrelemek için kullanılır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: Azure Backup (Microsoft Azure kurtarma hizmetleri Aracısı dahil) tarafından kullanılan uç noktalar Microsoft tarafından yönetilir. Şirket içi sistemlerinize dağıtmak istediğiniz ek denetimlerden siz sorumlusunuz.

- [MARS Aracısı için ağ ve erişim desteğini anlama](./backup-support-matrix-mars-agent.md#networking-and-access-support)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: uygulanamaz; bir sanal ağ, alt ağ veya ağ güvenlik grubunu bir kurtarma hizmetleri kasası ile ilişkilendiremezsiniz. Azure sanal makinesi yedeklenirken, veriler Azure omurgası üzerinden aktarılır. Şirket içi makinelerden yedeklenirken, Azure 'da belirli bir uç nokta ile şifrelenmiş bir tünel oluşturulur ve kimlik bilgileri, şifrelenen tünelden gönderilmeden önce verileri önceden şifrelemek için kullanılır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: Azure Backup (Microsoft Azure kurtarma hizmetleri Aracısı dahil) tarafından kullanılan uç noktalar Microsoft tarafından yönetilir. Şirket içi sistemlerinize dağıtmak istediğiniz ek denetimlerden siz sorumlusunuz.

- [MARS Aracısı için ağ ve erişim desteğini anlama](./backup-support-matrix-mars-agent.md#networking-and-access-support)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Kılavuz**: bir Azure sanal makinesinde Mars Aracısı kullanıyorsanız, Azure Backup giden erişime izin vermek için NSG veya Azure Güvenlik duvarınızdaki AzureBackup hizmet etiketini kullanın.

- [Azure VM 'lerinde SQL Server veritabanları yedekleme](./backup-sql-server-database-azure-vms.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Azure Backup (Microsoft Azure Kurtarma Hizmetleri Aracısı dahil) tarafından kullanılan uç noktalar Microsoft tarafından yönetilir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Kılavuz**: bir Azure sanal makinesinde Mars Aracısı kullanıyorsanız, bu VM 'yi bir ağ güvenlik grubuyla ilişkilendirin kurala yönelik iş gereksinimini belirtmek için açıklamayı kullanın

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Rehberlik**: BIR NSG veya Azure Güvenlik Duvarı tarafından korunan bir Azure sanal makinesinde Mars Aracısı kullanıyorsanız, NSG veya güvenlik duvarının yapılandırmasını Izlemek Için Azure etkinlik günlüğü ' nu kullanın. Azure Izleyici 'de, bu kaynaklarda yapılan değişiklikler gerçekleştiği zaman tetikleyecek uyarılar oluşturabilirsiniz.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Azure Izleyici 'yi kullanarak etkinlik günlüğü uyarıları oluşturma, görüntüleme ve yönetme](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: uygulanamaz; Microsoft, günlüklerdeki zaman damgaları için Azure Backup gibi Azure kaynakları için kullanılan saat kaynağını korur.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Ayrıca, Azure Backup tarafından oluşturulan güvenlik verilerini toplamak için Azure Izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için depolama hesapları kullanın. Alternatif olarak, Azure Sentinel veya bir üçüncü taraf güvenlik olayı ve olay yönetimi (SıEM) üzerinde verileri etkinleştirebilir ve bu verilere izin verebilir.

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

- [Kurtarma Hizmetleri kasaları için tanılama ayarlarını kullanma](./backup-azure-diagnostic-events.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim düzlemi denetim günlüğü Için Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına, Azure Olay Hub 'ına veya Arşiv için Azure depolama hesabına gönderin. Azure etkinlik günlüğü verilerini kullanarak, Azure kaynaklarınızın denetim düzlemi düzeyinde gerçekleştirilen herhangi bir yazma işlemi (PUT, POST, SILME) için "ne, kim ve ne zaman" seçeneğini belirleyebilirsiniz.

Ayrıca, Azure Backup analiz, uyarı ve raporlama amacıyla toplanabilecek ve kullanılabilecek tanılama olayları gönderir. Kurtarma Hizmetleri Kasası için tanılama ayarlarını Azure portal aracılığıyla yapılandırabilirsiniz. Bir depolama hesabına, Olay Hub 'ına veya bir Log Analytics çalışma alanına bir veya daha fazla tanılama olayı gönderebilirsiniz.

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

- [Kurtarma Hizmetleri kasaları için tanılama ayarlarını kullanma](./backup-azure-diagnostic-events.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, kuruluşunuzun uyumluluk düzenlemelerine göre Azure kurtarma hizmetleri kasalarıyla ilişkili Log Analytics çalışma alanları için günlük tutma süresini ayarlayın.

- [Günlük tutma parametrelerini ayarlama](../azure-monitor/platform/manage-cost-storage.md#change-the-data-retention-period)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: Azure Backup, bir kurtarma hizmetleri kasasında yerleşik izleme ve uyarı özellikleri sağlar. Bu yetenekler ek bir yönetim altyapısı olmadan kullanılabilir. Ayrıca, Azure Izleyici 'yi kullanarak izlemenin ve raporlamanın ölçeğini de artırabilirsiniz.

Azure etkinlik günlüğü tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve kurtarma hizmetleri kasaları için toplanabilecek etkinlik günlüğü verilerine göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin.

- [Azure Backup iş yüklerini izleme](./backup-azure-monitoring-built-in-monitor.md)

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

- [Azure Izleyici 'de Log Analytics çalışma alanında Azure etkinlik günlüklerini toplama ve çözümleme](../azure-monitor/platform/activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Rehberlik**: Azure Backup, bir kurtarma hizmetleri kasasında yerleşik izleme ve uyarı özellikleri sağlar. Bu yetenekler ek bir yönetim altyapısı olmadan kullanılabilir. Ayrıca, Azure Izleyici 'yi kullanarak izlemenin ve raporlamanın ölçeğini de artırabilirsiniz.

Uyarılar, kullanıcıların ilgili eylemi yapabilmesi için bilgilendirildikleri senaryolara yöneliktir. Yedekleme uyarıları bölümü Azure Backup hizmeti tarafından oluşturulan uyarıları gösterir. Bu uyarılar hizmet tarafından tanımlanır ve özel uyarı oluşturamazsınız.

Ayrıca, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağladığından Azure Sentinel 'e bir Log Analytics çalışma alanı ekleyebilirsiniz. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır. Ayrıca, Azure Izleyici 'yi kullanarak Log Analytics çalışma alanınızda özel günlük uyarıları da oluşturabilirsiniz.

- [Azure Backup iş yüklerini izleme](./backup-azure-monitoring-built-in-monitor.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Izleyici 'yi kullanarak günlük uyarıları oluşturma, görüntüleme ve yönetme](../azure-monitor/platform/alerts-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Azure Backup kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure Backup, DNS ile ilgili günlükleri işlemez veya oluşturmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) açıkça atanması ve sorgulanabilir olması gereken yerleşik roller içerir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

Destekleyici belgeler:

- [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Kılavuz**: Azure AD varsayılan parola kavramına sahip değildir. Parola gerektiren diğer Azure kaynakları, karmaşıklık gereksinimleriyle ve hizmete bağlı olarak, en düşük parola uzunluğuna sahip bir parola oluşturulmasını zorlar. Varsayılan parolaları kullanbilen üçüncü taraf uygulamalardan ve Market hizmetlerinden siz sorumlusunuz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, özel yönetim hesaplarını izlemenize yardımcı olmak için Azure Güvenlik Merkezi 'nden veya yerleşik Azure Ilkelerinden öneriler kullanabilirsiniz; Örneğin, aboneliğiniz için birden fazla sahip olması gerekir sahip izinlere sahip olan kullanım dışı hesaplar aboneliğinizden çıkarılmalıdır

- [Kimlik ve erişim (Önizleme) izlemek için Azure Güvenlik Merkezi 'ni kullanma](../security-center/security-center-identity-access.md)

- [Azure Ilkesini kullanma](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: API çağrıları aracılığıyla kurtarma hizmetleri kasalarınızla etkileşim kurmak için kullanılabilecek bir belirteç almak üzere bir Azure uygulama kaydı (hizmet sorumlusu) kullanın.

- [Azure REST API 'Lerini çağırma](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

- [Azure AD ile istemci uygulamanızı (hizmet sorumlusu) kaydetme](/rest/api/azure/#register-your-client-application-with-azure-ad)

- [Azure Kurtarma Hizmetleri API bilgileri](/rest/api/recoveryservices/)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Backup kritik işlemler gerçekleştirdiğinizde, Azure Portal kullanılabilen BIR güvenlik PIN 'i girmeniz gerekir. Azure Multi-Factor Authentication etkinleştirilmesi bir güvenlik katmanı ekler. Yalnızca geçerli Azure kimlik bilgilerine sahip yetkili kullanıcılar ve ikinci bir cihazdan kimlik doğrulaması yapılabilir Azure portal.

- [Azure Backup Multi-Factor Authentication](./backup-azure-security-feature.md)

- [Bulut tabanlı Azure Multi-Factor Authentication dağıtımı planlama](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Rehberlik**: Azure Backup etkin kaynaklarınızı oturum açmak ve yapılandırmak Için yapılandırılmış Azure MULTI-Factor AUTHENTICATION (MFA) ile ayrıcalıklı bir erişim iş istasyonu (Paw) kullanın.

- [Ayrıcalıklı Erişim İş İstasyonları](/windows-server/identity/securing-privileged-access/privileged-access-workstations)

- [Bulut tabanlı Azure Multi-Factor Authentication dağıtımı planlama](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik oluştuğunda günlükler ve uyarılar oluşturmak için Azure ACTIVE DIRECTORY (AD) PRIVILEGED IDENTITY Management (PIM) kullanın.

Ayrıca, riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

- [Privileged Identity Management dağıtma (PıM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Azure AD risk algılamalarını anlama](../active-directory/identity-protection/overview-identity-protection.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından Azure Portal erişimine izin vermek Için, koşullu erişim adlı konum kullanın.

- [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: Azure Backup örneklerinizin merkezi kimlik doğrulaması ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

- [Azure Backup Azure AD oturum açma kullanacak şekilde yapılandırma](../app-service/configure-authentication-provider-aad.md)

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure ACTIVE DIRECTORY (ad) eski hesapları keşfetmenize yardımcı olacak Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir.

- [Azure AD raporlamayı anlama](../active-directory/reports-monitoring/index.yml)

- [Azure kimlik erişimi Incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: Azure Backup örneklerinizin merkezi kimlik doğrulaması ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure AD oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişerek Azure Sentinel veya bir üçüncü taraf SıEM ile tümleştirmenize olanak tanır.

Bu işlemi, Azure AD Kullanıcı hesapları için Tanılama ayarları oluşturarak ve Log Analytics çalışma alanına denetim günlüklerini ve oturum açma günlüklerini göndererek kolaylaştırabilirsiniz. Log Analytics içinde, istenen günlük uyarılarını yapılandırabilirsiniz.

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

- [Yerleşik Azure Sentinel](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: kurtarma hizmetleri kasaları için merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Denetim düzleminde hesap oturum açma davranışı sapması (Azure portal) için, otomatik yanıtları Kullanıcı kimlikleriyle ilgili şüpheli eylemlere yönelik olarak yapılandırmak için Azure AD Kimlik Koruması ve risk algılama özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz.

- [Azure Backup Azure AD oturum açma kullanacak şekilde yapılandırma](../app-service/configure-authentication-provider-aad.md)

- [Azure AD riskli oturum açma nasıl görüntülenir](../active-directory/identity-protection/overview-identity-protection.md)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası, Azure Backup için henüz desteklenmiyor.

- [Müşteri Kasası tarafından desteklenen hizmetler listesi](../security/fundamentals/customer-lockbox-overview.md#supported-services-and-scenarios-in-general-availability)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure kaynaklarını izlemeye yardımcı olması için etiketleri kullanın.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Kılavuz**: Azure IaaS sanal makineleri yedeklenirken, Azure Backup özgün verilerin yanlışlıkla yok edilmesiyle karşı koruma sağlamak için bağımsız ve yalıtılmış yedeklemeler sağlar. Yedeklemeler, kurtarma noktalarının yerleşik yönetimi ile bir kurtarma hizmetleri kasasında depolanır.

Geliştirme, test ve üretim kurtarma hizmetleri kasaları için ayrı abonelikler ve/veya yönetim grupları uygulayın. Kaynaklar VNet/subnet ile ayrılmalıdır, uygun şekilde etiketlenebilir ve bir NSG veya Azure güvenlik duvarıyla korunmuş olmalıdır. Hassas verileri depolayan veya işleyen kaynaklar yeterince yalıtılmalıdır. Hassas verileri depolayan veya işleyen sanal makineler için, kullanımda olmadığında devre dışı bırakmak üzere ilke ve yordam uygulayın.

Destekleyici belgeler:

- [Azure Backup genel bakış](./backup-overview.md)

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../governance/management-groups/create.md)

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Şu anda kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure Backup için kullanılamaz.

Microsoft, Azure Backup için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: sunuculardan kurtarma hizmetleri kasasına yedekleme trafiği GÜVENLI bir https bağlantısı üzerinden aktarılır ve kasada depolandığında GELIŞMIŞ ŞIFRELEME standardı (AES) 256 kullanılarak şifrelenir.

- [Azure Backup bekleyen şifrelemeyi anlayın](./backup-encryption.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: Şu anda kullanılamıyor; veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure Backup için kullanılamaz.

Microsoft, Azure Backup için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Şu anda kullanılamıyor

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure rol tabanlı erişim denetimi (Azure RBAC), Azure için ayrıntılı erişim yönetimine izin vermez. RBAC kullanarak ekibiniz içinde görevleri ayırabilir, bu işlere gerek duyan kişilere sadece erişim miktarını verebilirsiniz.

Azure Backup yedekleme yönetim işlemlerini denetlemek için üç yerleşik rol sağlar: yedek katkıda bulunan, yedekleme operatörü ve yedek okuyucu. Yedekleme yerleşik rollerini çeşitli yedekleme yönetimi eylemlerine eşleyebilirsiniz.

- [Azure 'da RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

- [Azure Backup kurtarma noktalarını yönetmek için rol tabanlı Access Control kullanma](./backup-rbac-rs-vault.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft, Azure Backup için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

- [Azure müşteri verileri koruması](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: Azure Backup Rest verileri için şifrelemeyi destekler. Şirket içi yedekleme için, beklemedeyken şifreleme özelliği Azure'a yedekleme yaparken sağladığınız parola kullanılarak sağlanır. Bulut iş yükleri için, veriler Depolama Hizmeti Şifrelemesi (SSE) kullanılarak şifrelenir. Microsoft herhangi bir noktada yedekleme verilerinin şifresini çözmez.

MARS aracısıyla yedekleme yaparken veya müşteri tarafından yönetilen bir anahtarla şifrelenen bir kurtarma hizmetleri Kasası kullandığınızda yalnızca şifreleme anahtarına erişebilirsiniz. Microsoft hiçbir şekilde kopya korumaz ve anahtara erişemez. Anahtar yanlış olursa Microsoft, yedekleme verilerini kurtaramaz.

- [Azure Backup için bekleyen şifrelemeyi anlayın](./backup-encryption.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Izleyici 'Yi Azure etkinlik günlüğü ile birlikte kullanarak, üretim Azure kurtarma hizmetleri kasalarının yanı sıra diğer kritik veya ilgili kaynaklara yönelik değişikliklerin ne zaman gerçekleştiği hakkında uyarılar oluşturun.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: henüz kullanılamıyor; Azure Güvenlik Merkezi 'ndeki güvenlik açığı değerlendirmesi Azure Backup için henüz kullanılamıyor.

Microsoft tarafından taranan ve düzeltme eki uygulanan temel platform. Hizmet yapılandırması ile ilgili güvenlik açıklarını azaltmak için Azure Backup kullanılabilen güvenlik denetimlerini gözden geçirin.

- [Azure Backup için kullanılabilen güvenlik denetimlerini anlama](./backup-security-controls.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Şu anda kullanılamıyor; Azure Backup için güvenlik yapılandırması henüz Azure Güvenlik Merkezi 'nde desteklenmiyor.

- [Azure Güvenlik Merkezi tarafından desteklenen PaaS hizmetlerinin listesi](../security-center/features-paas.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz dahilinde (işlem, depolama, ağ, bağlantı noktaları ve protokoller vb.) tüm kaynakları sorgulamak/öğrenmek Için Azure Kaynak grafiğini kullanın.  Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

- [Azure Kaynak Graf ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription)

- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure kaynaklarına Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler verirsiniz.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure kaynaklarını düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ayrıca, aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesi 'ni kullanın: izin verilen kaynak türleri izin verilen kaynak türleri

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../governance/management-groups/create.md)

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: işlem kaynakları Için onaylanan Azure kaynakları ve onaylanan yazılımlar tanımlayın.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: abonelikleriniz içinde oluşturulabilecek kaynak türlerine yönelik kısıtlamalar koymak Için Azure ilkesini kullanın.

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde kaynakları sorgulama/bulma.  Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: Şu yerleşik ilke tanımlarını kullanarak müşteri aboneliklerde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın: izin verilen kaynak türleri izin verilen kaynak türleri

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/index.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-divlimit-users-ability-to-interact-with-azure-resource-manager-via-scriptsdiv"></a>6,11: <div>Kullanıcıların betikler aracılığıyla Azure Resource Manager etkileşim kurma yeteneğini sınırlayın</div>

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak üzere Azure koşullu erişimini yapılandırın.

- [Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure Ilkesiyle kurtarma hizmetleri kasanızda standart güvenlik yapılandırması tanımlayın ve uygulayın. Kurtarma Hizmetleri Kasalarınızın yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft. RecoveryServices" ad alanındaki Azure Ilke diğer adlarını kullanın.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias)

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure devops veya Azure Repos kullanın.

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure Repos belgeleri](/azure/devops/repos/index?view=azure-devops)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: yerleşik Azure ilke tanımlarını ve sistem yapılandırmalarının uyarı vermesi, denetlemesi ve uygulanması için özel ilkeler oluşturmak üzere "Microsoft. recoveryservices" ad alanındaki Azure ilkesi diğer adları 'nı kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: yerleşik Azure ilke tanımlarını ve sistem yapılandırmalarının uyarı vermesi, denetlemesi ve uygulanması için özel ilkeler oluşturmak üzere "Microsoft. recoveryservices" ad alanındaki Azure ilkesi diğer adları 'nı kullanın. Azure kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yoksa dağıt] Azure Ilkesini kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: Mars aracısını ayarlarken, şifreleme parolanızı Azure Key Vault içinde depolayın.

- [Key Vault oluşturma](../key-vault/secrets/quick-create-portal.md)

- [Yönetilen kimlik ile Key Vault kimlik doğrulaması sağlama](../key-vault/general/managed-identity.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: uygulanamaz; Yönetilen kimlikler Azure Backup için desteklenmez.

- [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir. Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin, Azure Backup) etkinleştirilir, ancak müşteri içeriklerde çalıştırılmaz.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure Backup) etkinleştirilir, ancak içeriğiniz üzerinde çalışmaz.

App Service, Data Lake Storage, BLOB depolama vb. gibi işlem dışı Azure kaynaklarına yüklenen tüm dosyaları önceden tarayın.

Depolama hesaplarına yüklenen kötü amaçlı yazılımları algılamak için veri Hizmetleri için Azure Güvenlik Merkezi 'nin tehdit algılamasını kullanın.

- [Azure Cloud Services ve sanal makineler için Microsoft Antimalware 'i anlayın](../security/fundamentals/antimalware.md)

- [Azure Güvenlik Merkezi 'nin veri Hizmetleri tehdit algılamasını anlama](../security-center/threat-protection.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: uygulanamaz; Bu öneri, Azure Backup yedeklenmekte olan kaynaklar için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Kılavuz**: yerel olarak yedekli depolama (LRS), verilerinizi üç kez çoğaltır (verilerinizin üç kopyasını oluşturur) bir veri merkezinde depolama ölçek birimi. Verilerin tüm kopyaları aynı bölgenin içinde yer alır. LRS, verilerinizi yerel donanım hatalarından korumak için düşük maliyetli bir seçenektir. Coğrafi olarak yedekli depolama (GRS), varsayılan ve önerilen çoğaltma seçeneğidir. GRS, verilerinizi ikincil bir bölgeye (kaynak verilerin birincil konumundan yüzlerce kilometre uzakta) kopyalar. GRS 'nin maliyeti LRS 'den fazladır, ancak bölgesel bir kesinti olsa bile, verileriniz için GRS daha yüksek düzeyde dayanıklılık sağlar.

Müşteri tarafından yönetilen anahtarları Azure Key Vault içinde yedekleyin.

- [Azure Backup genel bakış](./backup-overview.md)

- [Azure 'da Anahtar Kasası anahtarlarını yedekleme](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

- [Azure Backup şifrelemeyi anlama](./backup-encryption.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: yedeklenen müşteri tarafından yönetilen anahtarların sınamasını geri yükleme.

- [Azure 'da Anahtar Kasası anahtarlarını geri yükleme](/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: şirket içi yedekleme Için, Azure 'a yedeklerken sağladığınız parola kullanılarak bekleyen şifreleme sağlanır. Azure VM'leri için, veriler Depolama Hizmeti Şifrelemesi (SSE) kullanılarak beklemedeyken şifrelenir. Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault ' de geçici silme özelliğini etkinleştirebilirsiniz.

- [Key Vault 'da geçici silme özelliğini etkinleştirme](../storage/blobs/soft-delete-overview.md?tabs=azure-portal)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

- [Azure Güvenlik Merkezi 'nde Iş akışı otomasyonlarını yapılandırma](../security-center/security-center-planning-and-operations-guide.md)

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Ayrıca, NıST 'nin bilgisayar güvenliği olay Işleme kılavuzumuzu, kendi olay yanıtı planınızın oluşturulmasına yardımcı olması için de kullanabilirsiniz](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

- [NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**:- [Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin