---
title: Azure Güvenlik Duvarı için Azure Güvenlik temeli
description: Azure Güvenlik Duvarı güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: firewall
ms.topic: conceptual
ms.date: 08/05/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 869c6590146561f6f2f50694c5cc3f79530dad25
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87854399"
---
# <a name="azure-security-baseline-for-azure-firewall"></a>Azure Güvenlik Duvarı için Azure Güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslayıcılarından](../security/benchmarks/overview.md) Azure Güvenlik Duvarı 'na kılavuzluk uygular. Azure Güvenlik kıyaslaması, bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimleri** ve Azure Güvenlik Duvarı için geçerli olan ilgili kılavuzda gruplandırılır. Azure Güvenlik Duvarı için geçerli olmayan **denetimler** dışlandı. Azure Güvenlik duvarının Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure Güvenlik Duvarı güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](/azure/security/benchmarks/security-control-network-security).*

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1,2: sanal ağların, alt ağların ve ağ arabirimlerinin yapılandırma ve trafiğini izleme ve günlüğe kaydetme

**Kılavuz**: Azure Güvenlik Duvarı, güvenlik duvarı tarafından işlenen trafiğin günlüğe kaydedilmesi Için Azure izleyici ile tümleşiktir.

Ayrıca, Azure Güvenlik Merkezi 'ni kullanarak, Azure Güvenlik Duvarı ile ilgili ağ kaynaklarınızın güvenliğini sağlamaya yardımcı olması için ağ koruma önerilerini izleyin.

- [Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlama](../security-center/security-center-network-recommendations.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: tehdit zeka tabanlı filtrelemeyi, gelen/veya bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen trafiği uyarmak ve reddetmek için etkinleştirin. Tehdit zeka tabanlı filtreleme, güvenlik duvarınızın, trafiği ve bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen trafiği uyarmasını ve reddetmesini sağlamak için etkinleştirilebilir.

- [Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme](threat-intel.md)

- [Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama](/azure/security-center/security-center-alerts-service-layer)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Kılavuz**: bir Azure Güvenlik duvarında hizmet etiketi, güvenlik kuralı oluşturma karmaşıklığını en aza indirmeye yardımcı olmak IÇIN bir IP adresi ön eki grubunu temsil eder.

Azure Güvenlik Duvarı hizmet etiketleri, ağ kuralları hedef alanında kullanılabilir ve Azure Güvenlik duvarında ağ erişim denetimleri tanımlayabilir. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz.

Ayrıca, IP grupları gibi müşteri tanımlı Etiketler de desteklenir ve bir ağ kuralında veya bir uygulama kuralında kullanılabilir. Uygulama kurallarında FQDN etiketleri, güvenlik duvarınız aracılığıyla gerekli giden ağ trafiğine izin vermek için desteklenir.

Kendi hizmet etiketinizi oluşturmayacağınızı ve bir etiket içinde hangi IP adreslerinin ekleneceğini de belirtmeniz gerektiğini unutmayın. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

 

- [Azure Güvenlik Duvarı hizmeti etiketleri](service-tags.md)

- [Kullanılabilir hizmet etiketleri](../virtual-network/service-tags-overview.md#available-service-tags)

- [Azure Güvenlik duvarında IP grupları](ip-groups.md)

- [FQDN etiketlerine genel bakış](fqdn-tags.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Ilkesi henüz Azure Güvenlik Duvarı için tam olarak desteklenmiyor. Azure Güvenlik Duvarı Yöneticisi, güvenlik yapılandırmalarının standartlaştırma düzeyini elde etmek için kullanılabilir.

Tek bir şema tanımında Azure kaynakları Yöneticisi şablonları, RBAC denetimleri ve ilkeleri gibi temel ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı da kullanabilirsiniz. Şema 'i yeni aboneliklere uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetimi ayarlayabilirsiniz.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Ağ için Azure Ilke örnekleri](/azure/governance/policy/samples/#network)

- [Azure Blueprint oluşturma](../governance/blueprints/create-blueprint-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak kaynak yapılandırmasını Izleyin ve Azure Güvenlik Duvarı kaynaklarınızda yapılan değişiklikleri tespit edin. Kritik kaynaklardaki değişiklikler gerçekleşirken tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

- [Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme](/azure/firewall/tutorial-diagnostics)

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](/azure/azure-monitor/platform/activity-log-view)

- [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md) 

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve izleme](/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, Azure Güvenlik Duvarı için Azure kaynakları için zaman kaynaklarını korur. Müşterilerin bu erişime izin vermek için bir ağ kuralı veya ortamlarında kullandığınız bir zaman sunucusu olması gerekir.

- [NTP sunucusu erişimi](protect-windows-virtual-desktop.md#additional-considerations)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: çeşitli günlüklerin Merkezi güvenlik günlük yönetimi Için Azure Sentinel 'e veya bir üçüncü taraf SIEM 'e, pano günlük verilerini etkinleştirebilir ve bu verileri kullanabilirsiniz.

Etkinlik günlükleri, Azure Güvenlik duvarındaki işlemleri denetlemek ve kaynaklardaki eylemleri izlemek için kullanılabilir. Etkinlik günlüğü, kaynaklarınız için okuma işlemleri (GET) dışında tüm yazma işlemlerini (PUT, POST, DELETE) içerir. Etkinlik günlükleri, sorun giderirken bir hata bulmak veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizi izlemek için kullanılabilir.

Azure Güvenlik Duvarı, müşteri uygulamaları ve ağ kuralları hakkında bilgi sağlamak için aşağıdaki tanılama günlüklerini de sağlar.

Uygulama kuralı günlüğü: yapılandırılmış uygulama kurallarınızdan biriyle eşleşen her yeni bağlantı, kabul edilen/reddedilen bağlantı için bir günlük ile sonuçlanır.

Ağ kuralı günlüğü: yapılandırılmış ağ kurallarınızdan biriyle eşleşen her yeni bağlantı, kabul edilen/reddedilen bağlantı için bir günlük ile sonuçlanır

Note: her Iki günlük de bir depolama hesabına kaydedilebilir, Olay Hub 'larına kaydedilebilir ve/veya yalnızca bir ortamdaki her bir Azure Güvenlik Duvarı için etkinleştirildiyse Azure Izleyici günlüklerine gönderilebilir.

- [Azure Güvenlik Duvarı günlükleri](logs-and-metrics.md)

Etkinlik günlüklerindeki kaynak eylemlerinin listesi: Azure Resource Manager kaynak sağlayıcısı işlemleri

- [Azure Izleyici ile platform günlükleri ve ölçümleri toplama](../azure-monitor/platform/diagnostic-settings.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Izleyici ve üçüncü taraf SıEM tümleştirmesi ile çalışmaya başlama](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: etkinlik günlükleri, Azure Güvenlik duvarında işlemleri denetlemek ve kaynaklardaki eylemleri izlemek için kullanılabilir. Etkinlik günlüğü, okuma işlemleri (GET) haricinde Azure kaynakları için tüm yazma işlemlerini (PUT, POST, SIL) içerir. Azure Güvenlik Duvarı, müşteri uygulamaları ve ağ kuralları hakkında bilgi sağlamak için aşağıdaki tanılama günlüklerini de sağlar. 

Uygulama kuralı günlüğü: yapılandırılmış uygulama kurallarınızdan biriyle eşleşen her yeni bağlantı, kabul edilen/reddedilen bağlantı için bir günlük ile sonuçlanır.

Ağ kuralı günlüğü: yapılandırılmış ağ kurallarınızdan biriyle eşleşen her yeni bağlantı, kabul edilen/reddedilen bağlantı için bir günlük ile sonuçlanır

Her iki günlük de bir depolama hesabına kaydedilebilir, Olay Hub 'larına kaydedilir ve/veya Azure Izleyici günlüklerine gönderilebilir, ancak yalnızca bir ortamdaki her bir Azure Güvenlik Duvarı için etkinleştirilirse.

- [Azure Güvenlik Duvarı günlükleri](logs-and-metrics.md)

- [Etkinlik günlüklerindeki kaynak eylemlerinin listesi](../role-based-access-control/resource-provider-operations.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Rehberlik**: Log Analytics çalışma alanı saklama süresi, kuruluşun Azure izleyici içindeki uyumluluk düzenlemelerine göre ayarlanabilir. Veri saklama, seçilen fiyatlandırma katmanına bağlı olarak, tüm çalışma alanları için 30 ila 730 gün (2 yıl) yapılandırılabilir.

Günlük depolama bekletmesini depolamak için 3 seçenek vardır:

Depolama hesapları, Günlükler daha uzun bir süre boyunca depolandığında ve gerektiğinde incelenip günlüklerde en iyi şekilde kullanılır.

Olay Hub 'ları, kaynaklarınızda uyarı almak için diğer güvenlik bilgileri ve olay yönetimi (SEıM) araçlarıyla tümleştirme için harika bir seçenektir.

Azure Izleyici günlükleri, uygulamanızın genel gerçek zamanlı izleme için en iyi şekilde kullanılır veya eğilimleri göz yormaktadır.

- [Azure Güvenlik Duvarı günlükleri ve ölçümleri](logs-and-metrics.md)

- [Log Analytics veri saklama süresini değiştirme](../azure-monitor/platform/manage-cost-storage.md)

- [Azure depolama hesabı günlükleri için bekletme ilkesini yapılandırma](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure Güvenlik Duvarı, güvenlik duvarı günlüklerini görüntülemek ve analiz etmek Için Azure izleyici ile tümleşiktir. Günlükler Log Analytics, Azure depolama veya Event Hubs gönderilebilir. Log Analytics veya Excel ve Power BI gibi farklı araçlarla analiz edilebilir. Birkaç farklı Azure Güvenlik duvarı günlüğü türü vardır.

Etkinlik günlükleri, Azure Güvenlik duvarındaki işlemleri denetlemek ve kaynaklardaki eylemleri izlemek için kullanılabilir. Etkinlik günlüğü, okuma işlemleri (GET) dışındaki kaynaklar için tüm yazma işlemlerini (PUT, POST, SIL) içerir. Etkinlik günlükleri, sorun giderirken bir hata bulmak veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizi izlemek için kullanılabilir.

Azure Güvenlik Duvarı ayrıca, müşteri uygulamaları ve ağ kuralları hakkında bilgi sağlamak için tanılama günlükleri de sağlar.

Uygulama kuralı günlükleri, yapılandırılmış uygulama kurallarınızdan biriyle eşleşen her yeni bağlantı kabul edilen/reddedilen bağlantı için bir günlük ile sonuçlandıklarında oluşturulur. 

Yapılandırılmış ağ kurallarınızdan biriyle eşleşen her yeni bağlantı, kabul edilen/reddedilen bağlantı için bir günlük ile sonuçlandıklarında ağ kuralı günlükleri oluşturulur

Her iki günlük bir depolama hesabına kaydedilebilir, Olay Hub 'larına ve/veya Azure Izleyici günlüklerine gönderilebilir, ancak bu, yalnızca bir ortamdaki her bir Azure Güvenlik Duvarı için etkinse.

Azure Izleyici günlükleri, uygulamanızın genel gerçek zamanlı olarak izlenmesi veya eğilimleri göz yormasında kullanılabilir.

- [Azure Güvenlik Duvarı günlükleri ve ölçümleri](logs-and-metrics.md)

- [Tanılama günlükleri](logs-and-metrics.md#diagnostic-logs)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Rehberlik**: güvenlik günlükleri ve olayları 'nda bulunan anormal etkinlikleri izlemek ve uyarmak Için Log Analytics çalışma alanıyla Azure Güvenlik Merkezi 'ni kullanın. 

Alternatif olarak, Azure Sentinel 'de ve yerleşik verileri etkinleştirebilir. 

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

- [Azure Güvenlik Merkezi 'nde uyarıları yönetme](../security-center/security-center-managing-and-responding-alerts.md)

- [Log Analytics günlük verilerinde uyarı alma](../azure-monitor/learn/tutorial-response.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik ve erişim denetimi](/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Kılavuz**: Azure AD 'nin açıkça atanması ve sorgulanabilir olması gereken yerleşik rolleri vardır. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

- [Azure AD 'de PowerShell ile dizin rolü alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturun. Yönetim hesaplarının sayısını izlemek için Azure Güvenlik Merkezi kimlik ve erişim yönetimi 'ni kullanın.

Ayrıca, Microsoft Hizmetleri için Azure AD Privileged Identity Management ayrıcalıklı rolleri kullanarak tam zamanında/tam erişimi etkinleştirebilirsiniz ve Azure Resource Manager.

- [Privileged Identity Management hakkında daha fazla bilgi edinin](/azure/active-directory/privileged-identity-management)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3,4: Azure Active Directory çoklu oturum açma kullan (SSO)

**Rehberlik**: mümkün olan yerlerde, tek başına bağımsız kimlik bilgilerini hizmet başına yapılandırmak yerıne Azure Active Directory SSO kullanın. Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini kullanın.

- [Azure AD ile SSO 'yu anlama](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Multi-Factor AUTHENTICATION (MFA) Azure Active Directory etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure Güvenlik Duvarı ve ilgili kaynakları oturum açmak ve yapılandırmak için yapılandırılmış çok faktörlü kimlik DOĞRULAMASıYLA (MFA) Paws (ayrıcalıklı erişim iş istasyonları) kullanın. 

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](/windows-server/identity/securing-privileged-access/privileged-access-workstations) 

- [Azure 'da MFA 'yı etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda günlüklerin ve uyarıların üretilmesi için Azure Active Directory güvenlik raporlarını kullanın. Kimlik ve erişim etkinliğini izlemek için Azure Güvenlik Merkezi 'ni kullanın. 

- [Riskli etkinlik için işaretlenen Azure AD kullanıcılarını belirleme](/azure/active-directory/reports-monitoring/concept-user-at-risk) 

- [Azure Güvenlik Merkezi 'nde kullanıcıların kimlik ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: IP adresi aralıklarının veya ülkelerin/bölgelerin yalnızca belirli mantıksal gruplarından erişime izin vermek Için adlandırılmış konumlar kullanın. 

- [Azure 'da adlandırılmış konumları yapılandırma](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar. 

- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Kılavuz**: Azure AD, eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamaları için Azure kimlik erişimi Incelemelerini kullanın. Yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için, Kullanıcı erişimi düzenli olarak incelenebilir. 

- [Azure AD raporlamayı anlama](/azure/active-directory/reports-monitoring/)

- [Azure kimlik erişimi Incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Kılavuz**: Azure AD oturum açma etkinliğine, denetim ve risk olay günlüğü kaynaklarına erişerek herhangi bir SIEM/izleme aracıyla tümleştirmenize imkan tanır. 

Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırabilirsiniz. İstenen uyarıları Log Analytics çalışma alanı içinde yapılandırabilirsiniz. 

- [Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapması üzerinde uyarı

**Rehberlik**: Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak IÇIN Azure AD risk ve kimlik koruması özelliklerini kullanın. Ayrıca, daha fazla araştırma için verileri Azure Sentinel 'e aktarabilirsiniz. 

- [Azure AD riskli oturum açma işlemlerini görüntüleme](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: gizli bilgileri depolayan veya Işleyen Azure Güvenlik Duvarı ve ilgili kaynakları izlemeye yardımcı olması için Etiketler kullanın. 

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için ayrı abonelikler ve yönetim grupları kullanarak yalıtım uygulayın. Uygulamalarınızın ve kurumsal ortamların talep ettiği Azure Güvenlik Duvarı kaynaklarınıza erişim düzeyini kısıtlayabilirsiniz. Azure kaynaklarına erişimi, Azure Active Directory rol tabanlı erişim denetimi aracılığıyla denetleyebilirsiniz. 

- [Ek Azure abonelikleri oluşturma](/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](../governance/management-groups/create.md)

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: bir üçüncü taraf çözümünü, hassas bilgilerin yetkisiz aktarımını izleyen ve bilgi güvenliği uzmanlarına uyarı ederken bu tür aktarımları engelleyen bir üçüncü taraf çözümünden yararlanın. 

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini, müşteri veri kaybına ve pozlamaya karşı hassas ve koruma olarak değerlendirir. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar. 

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: yoldaki tüm hassas bilgileri şifreleyin. Azure Güvenlik duvarınızın ve ilgili kaynaklarınızın bağlandığı tüm istemcilerin TLS 1,2 veya daha büyük bir güvenlik anlaşması yapabilmesini sağlayın. 

Azure Güvenlik Merkezi önerilerini, varsa, bekleyen ve geçişte şifreleme için kullanın. 

- [Azure ile iletim sırasında şifrelemeyi anlama](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Kılavuz**: Azure Güvenlik Duvarı ve ilgili kaynakları kullanarak Azure kaynağında depolanan tüm hassas bilgileri tanımlamak ve kuruluşun hassas bilgi envanterini güncelleştirmek için üçüncü taraf bir etkin bulma aracı kullanın.

- [Azure 'da müşteri veri korumasını anlama](../security/fundamentals/protection-customer-data.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için rol tabanlı erişim denetimi kullanma

**Kılavuz**: Azure Güvenlik Duvarı ve ilgili kaynaklara erişimi denetlemek için Azure Active Directory rol tabanlı erişim denetımı (RBAC) kullanın.

- [Azure 'da RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: Azure Güvenlik Duvarı ve ilgili kaynakları kullanarak tüm Azure kaynaklarında bekleyen şifreleme kullanın. Microsoft, Azure 'un şifreleme anahtarlarınızı yönetmesine izin vermesini önerir, ancak bazı örneklerde kendi anahtarlarınızı yönetmeniz için seçenek vardır. 

- [Azure 'da bekleyen şifrelemeyi anlama](../security/fundamentals/encryption-atrest.md)

- [Müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma](../storage/common/storage-encryption-keys-portal.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Güvenlik duvarında değişiklik gerçekleşirken uyarı oluşturmak Için Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.

- [Azure etkinlik günlüğü olayları için uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: envanter ve varlık yönetimi](/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure Güvenlik Duvarı ve ilgili kaynaklara Etiketler uygulayarak bunları bir taksonomi halinde mantıksal olarak organize etmek için meta veriler sağlarsınız. 

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure Güvenlik Duvarı ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

- [Ek Azure abonelikleri oluşturma](/azure/billing/billing-create-subscription)

- [Yönetim Grupları oluşturma](../governance/management-groups/create.md)

- [Etiketler oluşturma ve kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Rehberlik**: Kurumsal gereksinimlerinize göre yapılandırma dahil olmak üzere onaylanan Azure Güvenlik Duvarı kaynaklarının envanterini oluşturun.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: abonelikleriniz içinde oluşturulabilecek kaynak türlerine yönelik kısıtlamalar koymak Için Azure ilkesini kullanın.

Azure Kaynak Grafiği 'ni kullanarak aboneliklerinde bulunan Azure Güvenlik Duvarı kaynaklarını sorgulama/bulma. Ortamda bulunan tüm Azure Güvenlik Duvarı ve ilgili kaynakların onaylandığından emin olun.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: yetkisiz Azure Güvenlik duvarını ve ilgili kaynakları kaldırmak için kendi işleminizi uygulayın. Onaylanmamış Azure Güvenlik duvarını ve ilgili kaynakları belirlemek için bir üçüncü taraf çözümü de kullanabilirsiniz

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: ortamınızda sağlayabileceğiniz hizmetleri kısıtlamak Için Azure ilkesini kullanın.

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](/azure/governance/policy/samples/not-allowed-resource-types)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak kullanıcıların Azure kaynakları yöneticisiyle etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın. 

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: iş işlemleri için gerekebilecek uygulamalar veya kuruluş için farklı risk profillerine sahip ortamlar, ayrı Azure Güvenlik Duvarı örnekleriyle yalıtılarak ayrılmalıdır.

- [Azure portalını kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](deploy-cli.md)

- [Sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

- [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenli yapılandırma](/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Rehberlik**: Azure Resource Manager, yapılandırmaların kuruluşunuzun güvenlik gereksinimlerini karşıladığından/aştığından emin olmak için gözden geçirilmesi gereken JAVASCRIPT nesne GÖSTERIMI (JSON) içinde şablonu dışarı aktarma özelliğine sahiptir.

Azure Güvenlik Merkezi 'nin önerilerini Azure kaynaklarınız için güvenli bir yapılandırma temeli olarak da kullanabilirsiniz.

Azure ilkesi Şu anda tam olarak desteklenmiyor. 

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Güvenlik önerileri-bir başvuru kılavuzu](../security-center/recommendations-reference.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Güvenlik duvarınız ve ilgili kaynaklarınız genelinde güvenli ayarları zorlamak için Azure ilkesi [reddetme] ve [dağıtım yok] kullanın.  Ayrıca, Azure Güvenlik duvarınızın ve kuruluşunuzun gerektirdiği ilgili kaynakların güvenlik yapılandırmasını sürdürmek için Azure Resource Manager şablonları kullanabilirsiniz.

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Resource Manager şablonlarına genel bakış](../azure-resource-manager/templates/overview.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilkeleri ve Azure Resource Manager şablonları gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynaklara erişmek için, Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure Active Directory (Azure AD) tanımlanmış gruplara izin verebilir veya vermeyebilirsiniz.

- [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: Azure ilkesi 'Ni kullanarak Azure Güvenlik Duvarı ve ilgili kaynaklar için standart güvenlik yapılandırması tanımlama ve uygulama. Azure Güvenlik Duvarı kaynaklarınızın ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak üzere Azure Ilke diğer adlarını kullanın. Ayrıca, belirli kaynaklarınızla ilgili yerleşik ilke tanımlarından da yararlanabilirsiniz.  

- [Azure Ilkesini yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Diğer adları kullanma](../governance/policy/concepts/definition-structure.md#aliases)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz**: Azure AD 'de otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak Için Yönetilen kimlikler kullanın. Yönetilen kimlikler, Azure Resource Manager için Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar ve API/Azure Portal/CLı/PowerShell ile kullanılabilir.

- [Yönetilen kimlikleri yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder. 

- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri kurtarma](/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli otomatik arka pencerelere emin olun

**Kılavuz**: Azure Güvenlik Duvarı ve ilgili yapılandırma için yedekleme olarak kullanılabilecek bir JAVASCRIPT nesne GÖSTERIMI (JSON) şablonunda Azure Güvenlik Duvarı ve ilgili kaynakları dışarı aktarmak için Azure Resource Manager kullanın.  Ayrıca, Azure portal 'den Azure Güvenlik duvarının şablon dışarı aktarma özelliğini kullanarak Azure Güvenlik Duvarı yapılandırmasını dışarı aktarabilirsiniz.  Yedekleme betiklerini otomatik olarak çalıştırmak için Azure Otomasyonu 'nu kullanın.

- [Azure portal bir şablona tek ve çoklu kaynak verme](../azure-resource-manager/templates/export-template-portal.md)

- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)

- [Microsoft ağı Azure Güvenlik duvarları şablon başvurusu](/azure/templates/microsoft.network/azurefirewalls)

- [Azure Otomasyonu hakkında](../automation/automation-intro.md)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tam sistem yedeklemeleri gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Kılavuz**: Azure Güvenlik Duvarı ve ilgili yapılandırma için yedekleme olarak kullanılabilecek bir JAVASCRIPT nesne GÖSTERIMI (JSON) şablonunda Azure Güvenlik Duvarı ve ilgili kaynakları dışarı aktarmak için Azure Resource Manager kullanın.  Ayrıca, Azure portal 'den Azure Güvenlik duvarının şablon dışarı aktarma özelliğini kullanarak Azure Güvenlik Duvarı yapılandırmasını dışarı aktarabilirsiniz.

- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)

- [Microsoft ağı Azure Güvenlik duvarları şablon başvurusu](/azure/templates/microsoft.network/azurefirewalls)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşteri tarafından yönetilen anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: Azure Resource Manager şablonu desteklenen dosyaları kullanarak düzenli aralıklarla geri yükleme gerçekleştirme olanağı sağlayın.  

- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)

- [Microsoft ağı Azure Güvenlik duvarları şablon başvurusu](/azure/templates/microsoft.network/azurefirewalls)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Kılavuz**: özel Azure ilkeleri, Azure Resource Manager şablonları gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. Azure DevOps 'da yönettiğiniz kaynakları korumak için, belirli kullanıcılara, yerleşik güvenlik gruplarına veya Azure DevOps ile tümleşikse veya TFS ile tümleşikse Active Directory Azure Active Directory (Azure AD) içinde tanımlanan gruplara izin verebilir veya vermeyebilirsiniz.

- [Azure DevOps 'da kod depolama](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun. 

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Kendi olay yanıtı planınızı oluşturmaya yardımcı olmak için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzuyla yararlanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır. 

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) etiketleri kullanarak Azure kaynaklarını açıkça tanımlamak ve kategorilere ayırmak için özellikle de hassas verileri işleyen bir adlandırma sistemi oluşturun.  Olayın gerçekleştiği Azure kaynakları ve ortamının önem derecesine bağlı olarak, uyarıların düzeltilmesine öncelik vermek sizin sorumluluğunuzdadır. 

- [Azure Güvenlik Merkezi'nde güvenlik uyarıları](../security-center/security-center-alerts-overview.md) 

- [Azure kaynaklarınızı düzenlemek için etiketleri kullanma](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: Azure kaynaklarınızın korunmasına yardımcı olmak için, sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin. 

- [NıST 'nin Yayımlama Kılavuzu, BT planları ve becerileri için programları test etme, eğitim ve alıştırma Kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), verilerinize izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin. 

- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Kılavuz**: Azure kaynaklarına yönelik riskleri belirlemenize yardımcı olmak Için sürekli dışarı aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın.

Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını kullanarak uyarıları Azure Sentinel 'e akışını sağlayabilirsiniz.

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıları Azure Sentinel 'e akış](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Kılavuz**: Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanarak, güvenlik uyarılarındaki "Logic Apps" aracılığıyla yanıtları otomatik olarak tetikleyin ve Azure kaynaklarınızı korumaya yönelik öneriler alın. 

- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Penetme testleri ve Red ekibi alıştırmaları](/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın. 

- [Katılım test kurallarını oyma](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Bulut Kırmızı ekip oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
