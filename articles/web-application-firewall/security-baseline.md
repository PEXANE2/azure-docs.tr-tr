---
title: Azure Web uygulaması güvenlik duvarı için Azure Güvenlik temeli
description: Azure Web uygulaması güvenlik duvarı güvenlik temeli, Azure Güvenlik kıyaslaması 'nda belirtilen güvenlik önerilerini uygulamaya yönelik yordamsal kılavuz ve kaynaklar sağlar.
author: msmbaldwin
ms.service: web-application-firewall
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 4bda3622c1bb1cb66e83b82df0d13cbfee9e20a3
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98878130"
---
# <a name="azure-security-baseline-for-azure-web-application-firewall"></a>Azure Web uygulaması güvenlik duvarı için Azure Güvenlik temeli

Bu güvenlik temeli [Azure Güvenlik kıyaslama sürümü 1,0](../security/benchmarks/overview-v1.md) ' dan Azure Web uygulaması güvenlik duvarı 'na kılavuzluk uygular. Azure Güvenlik Karşılaştırması, Azure üzerindeki bulut çözümlerinizin güvenliğini sağlamaya yönelik öneriler sunar. İçerik, Azure Güvenlik kıyaslaması tarafından tanımlanan **güvenlik denetimlerine** ve Azure Web uygulaması güvenlik duvarı için geçerli olan ilgili kılavuza göre gruplandırılır. Azure Web uygulaması güvenlik duvarı için geçerli olmayan **denetimler** dışlandı. 

Azure Web uygulaması güvenlik duvarının Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için, [tam Azure Web uygulaması güvenlik duvarı güvenlik temeli eşleme dosyasına](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines)bakın.

## <a name="network-security"></a>Ağ güvenliği

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: ağ güvenliği](../security/benchmarks/security-control-network-security.md).*

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: SQL ekleme ve siteler arası betik oluşturma gibi yaygın güvenlik açıklarından ve güvenlik açıklarından Web uygulamalarının merkezi koruması Için Microsoft Azure Web uygulaması güvenlik duvarını (WAF) kullanın. 

- Algılama modu: ağ trafiğini öğrenmek, anlaşılması ve hatalı pozitif sonuçları gözden geçirmek için bu modu kullanın. Tüm tehdit uyarılarını izler ve günlüğe kaydeder. Diagnostics ve WAF günlüğünün seçili ve açık olduğundan emin olun. WAF 'nin algılama modunda çalışırken gelen istekleri engellemediğine unutmayın.
- Önleme modu: kurallar tarafından algılanan saldırılar ve saldırıları engeller. Saldırgan bir "403 yetkisiz erişim" özel durumu alır ve bağlantı kapatılır. Önleme modu WAF günlüklerinde bu tür saldırıları kaydeder.

WAF algılama moduyla ağ trafiğinizi temel alarak. Trafik ihtiyaçlarınızı belirledikten sonra, istenmeyen trafiği Bock önlemek için WAF 'yi önleme modunda yapılandırın.

WAF tarafından korunmayan web özellikli herhangi bir kaynak için Güvenlik Merkezi 'nden yüksek öneme sahip önerilere uyun.  

- [Web uygulaması güvenlik duvarı, kural grupları ve kuralları](ag/application-gateway-crs-rulegroups-rules.md) 

- [Application Gateway WAF modları](ag/ag-overview.md#waf-modes)

- [Ön kapıda WAF modları](afds/afds-overview.md#waf-modes)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: trafiğe izin vermek ve trafiği engellemek Için Azure Web uygulaması güvenlik duvarı (WAF) ile özel kurallar kullanın. Örneğin, bir IP adresi aralığından gelen tüm trafik engellenebilir. Azure WAF ' i önleme modunda çalışacak şekilde yapılandırarak, kurallar tarafından algılanan yetkisiz saldırıları ve saldırıları engeller. Saldırgan bir "403 yetkisiz erişim" özel durumu alır ve bağlantı kapatılır. Önleme modu WAF günlüklerinde bu tür saldırıları kaydeder.

- [Application Gateway WAF modları](ag/ag-overview.md#waf-modes)

- [Ön kapıda WAF modları](afds/afds-overview.md#waf-modes)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: Azure Web uygulaması güvenlik duvarı (WAF), Azure 'un web uygulaması korumalarının temel bileşenidir. Azure WAF ' i kullanarak Web uygulamaları için önceden yapılandırılmış yönetilen kural kümesi ile genel sık kullanılan ve güvenlik açıklarından, OWASP Ilk 10 kategorisinden gelen bilinen saldırı imzaları için merkezi koruma sağlayın.

Kurallar ve kural gruplarıyla Azure WAF 'yi, Web uygulaması gereksinimlerine uyacak ve hatalı pozitif sonuçları ortadan kaldıracak şekilde özelleştirin. Bir WAF arkasındaki her site için bir Azure WAF Ilkesini siteye özgü yapılandırmaya izin verecek şekilde ilişkilendirin. Azure WAF, coğrafi filtreleme, hız sınırlaması, Azure tarafından yönetilen varsayılan kural kümesi kurallarını destekler. ve özel kurallar, bir uygulamanın ihtiyaçlarına uyacak şekilde oluşturulabilir. 

Belirlenen bir süre boyunca algılama modundaki ağ trafiğini taban çizgisi oluşturduktan sonra, Azure WAF 'yi önleme modunda çalışacak şekilde yapılandırın. Azure WAF, önleme modundaki kurallar tarafından algılanan yetkisiz saldırıları ve saldırıları engeller. Saldırgan bir "403 yetkisiz erişim" özel durumu alır ve bağlantı kapatılır. Önleme modu WAF günlüklerinde bu tür saldırıları kaydeder.

- [Application Gateway WAF modları](ag/ag-overview.md#waf-modes)

- [Ön kapıda WAF modları](afds/afds-overview.md#waf-modes)

- [Web uygulaması güvenlik duvarı, kural grupları ve kuralları](ag/application-gateway-crs-rulegroups-rules.md?tabs=owasp31)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Rehberlik**: ortak uygulama yapılandırmalarını (örneğin, Apache ve IIS) algılamak için Etiketler Ile bir Azure aboneliği üzerinde kaynakları oluşturun, ilişkilendirin ve mantıksal olarak düzenleyin. 

Uygulanan etiket meta verilerine dayalı olarak Azure Web uygulaması güvenlik duvarı (WAF) ilkelerine kurallar ve kural grupları uygulayın.

- [Application Gateway WAF ilkesi](/cli/azure/network/application-gateway/waf-policy?view=azure-cli-latest) 

- [Ön kapıda WAF ilkesi](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Kılavuz**: Azure Application Gateway alt ağınızdaki Azure Web uygulaması güvenlik duvarı (WAF) ile ilişkili ağ güvenlik grupları ve ağ güvenliği ve trafik akışıyla ilgili diğer kaynaklar için Etiketler kullanın. Tek tek ağ güvenlik grubu kuralları için "Açıklama" alanını kullanarak ağa veya ağdan trafiğe izin veren tüm kurallar için iş ihtiyacı, süresi vb. belirtin.

Tüm kaynakların etiketlerle oluşturulmasını ve mevcut etiketlenmemiş kaynakları bilgilendirmesini sağlamak için etiketlemeyle ilgili yerleşik Azure Ilke tanımlarından herhangi birini ("etiket ve onun değeri gerektir" gibi) kullanın.

Etiketlere göre kaynakları aramak veya eylemler gerçekleştirmek için Azure PowerShell veya Azure CLı ' yi seçin.

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

- [Sanal ağ oluşturma](../virtual-network/quick-create-portal.md)

- [Güvenlik Yapılandırması ile NSG oluşturma](../virtual-network/tutorial-filter-network-traffic.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını Izleyin ve Azure Web uygulaması güvenlik duvarı (WAF) dağıtımlarınızla ilgili ağ ayarları ve kaynakları için değişiklikleri tespit edin. Kritik ağ ayarlarında veya kaynaklarda değişiklik yapıldığında tetiklenecek Azure Izleyici içinde uyarılar oluşturun.

- [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](../azure-monitor/platform/activity-log.md#view-the-activity-log)

- [Azure Izleyici 'de uyarı oluşturma](../azure-monitor/platform/alerts-activity-log.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: günlüğe kaydetme ve izleme](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) IÇIN, UDP üzerinden bağlantı noktası 123 gibi uygun bağlantı noktası ve PROTOKOLLE bir NTP sunucusuna erişime izin vermek üzere bir ağ kuralı oluşturun.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) günlüklerini Azure Sentinel veya üçüncü taraf SIEM gibi bir merkezi güvenlik günlüğü yönetimi çözümüne gönderilmek üzere yapılandırın. Bu Günlükler Azure etkinlik, tanılama ve gerçek zamanlı WAF günlüklerini içerir, bu Günlükler daha sonra Azure Izleyici, Excel ve Power BI gibi farklı araçlarla görüntülenebilir. Azure Web uygulaması güvenlik duvarı günlükleri, Azure WAF 'nin değerlendirdiği, eşleştiği ve engellediği veriler hakkında fikir verir.

Azure Sentinel 'de, Azure WAF 'deki güvenlik olaylarına genel bakış sağlayan yerleşik bir Azure WAF çalışma kitabı vardır. Bu çalışma kitabı olayları, eşleşen ve engellenen kuralları ve güvenlik duvarı günlüklerinde günlüğe kaydedilen diğer her şeyi içerir. Bu telemetri, Sentinel tarafından toplanan WAF olaylarına dayalı olarak bildirim almak veya düzeltme eylemleri gerçekleştirmek için PlayBook Otomasyonu 'nu açmak üzere kullanılabilir.

- [Etkinlik günlüklerini görüntüle](../azure-resource-manager/management/view-activity-logs.md)

- [Application Gateway için tanılama günlükleri](../application-gateway/application-gateway-diagnostics.md)

- [Microsoft Web uygulaması güvenlik duvarındaki verileri Azure Sentinel 'e bağlama](../sentinel/connect-azure-waf.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: denetim, güvenlik ve tanılama günlüklerine erişmek Için Azure Web uygulaması güvenlik duvarı (WAF) kaynaklarınızın günlüğünü etkinleştirin. Azure Web uygulaması güvenlik duvarı, algılanan her tehdit üzerinde, yapılandırılmış tanılama günlüklerinde kullanılabilir hale getirilen ayrıntılı raporlama sağlar. Otomatik olarak kullanılabilen etkinlik günlükleri Olay kaynağını, tarihi, kullanıcıyı, zaman damgasını, kaynak adreslerini, hedef adreslerini ve diğer yararlı öğeleri içerir.

- [Günlüğe kaydetmeye genel bakış](ag/ag-overview.md#logging)

- [Azure Izleyici günlük sorgusuna genel bakış](../azure-monitor/log-query/log-query-overview.md)

- [Azure platformu günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) günlüklerini özel bir depolama hesabına gönderin ve bekletme ilkesini tanımlayın. Kuruluşunuzun uyumluluk gereksinimlerine göre Log Analytics çalışma alanı saklama süresini ayarlamak için Azure Izleyici 'yi kullanın.
- [Depolama hesabı için izlemeyi yapılandırma](../storage/common/storage-monitor-storage-account.md#configure-logging)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF), algılanan her tehdit hakkında ayrıntılı raporlama sağlar. Günlük kaydı, denetim ve sorun giderme için önemli olan işlemler ve hatalar hakkında zengin bilgiler sağlayan Azure Tanılama günlükleriyle tümleşiktir. 

Azure WAF örnekleri, raporlama için uyarı ve sistem durumu bilgilerini göndermek üzere güvenlik merkezi ile tümleşiktir. Korumasız Web uygulamalarını algılamak ve bu güvenlik açığı olan kaynakları korumak için Güvenlik Merkezi 'nin önerilerini kullanın. 

Azure Sentinel, WAF 'deki güvenlik olaylarına genel bakış sağlayan yerleşik bir WAF-güvenlik duvarı olayları çalışma kitabına sahiptir. Bunlar, olayları, eşleşen ve engellenen kuralları ve güvenlik duvarı günlüklerinde günlüğe kaydedilen diğer her şeyi içerir.

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/index.yml) 

- [Azure Application Gateway için tanılama ayarlarını etkinleştirme](../application-gateway/application-gateway-diagnostics.md)

- [Azure ön kapılarında ölçümleri ve günlükleri izleme](../frontdoor/front-door-diagnostics.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: anormal etkinlikler için uyarıları etkinleştir

**Kılavuz**: Azure etkinlik günlüğü tanılama ayarlarını ve Azure WAF 'niz için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin. Terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve toplanan verilere göre birçok diğer öngörü sağlamak için Log Analytics sorguları gerçekleştirin. WAF ölçümleri temelinde anormal etkinlikler için uyarılar oluşturun. Örneğin, engellenen istek sayısı ' X ' ile aşıldıysanız, ' Y ' yapın.

- [Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme](../azure-monitor/platform/activity-log.md)

- [Azure 'da uyarı oluşturma](../azure-monitor/learn/tutorial-response.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Kılavuz**: Azure Web uygulaması güvenlik duvarını (WAF), gelen trafiğin ek incelemesi için kritik Web uygulamaları önünde dağıtın. 

Azure WAF, Web uygulamalarınızın yaygın güvenlik açıklarından ve güvenlik açıklarından merkezi bir şekilde korunmasını sağlar ve SQL kullanımı, siteler arası komut dosyası oluşturma, kötü amaçlı yazılım yüklemeleri ve DDoS saldırıları gibi saldırıları engellemek için gelen Web trafiğini inceleyerek uygulamalarınızın güvenliğini sağlar.

- [Azure WAF dağıtma](ag/create-waf-policy-ag.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="identity-and-access-control"></a>Kimlik ve erişim denetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: kimlik ve erişim denetimi](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: Azure Active Directory (Azure AD) yerleşik rollere sahiptir ve açıkça atanması gerekir. Yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü kullanın.

- [Azure AD 'de PowerShell ile dizin rolü alma](/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0)

- [Azure AD 'de PowerShell ile bir dizin rolünün üyelerini alma](/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: WAF içinde kullanılabilir yerel yönetici ataması yok. Ancak, ortamda Azure WAF kaynakları üzerinde yönetim denetimi için izin verebilen Azure Active Directory (Azure AD) yönetici rolleri olabilir.
Azure Web uygulaması güvenlik duvarı (WAF) örneklerine erişimi olan adanmış yönetim hesaplarının kullanımı etrafında standart işletim yordamları oluşturmak iyi bir uygulamadır. Yönetim hesaplarının sayısını izlemek için Güvenlik Merkezi 'nin kimlik ve erişim yönetimi özelliklerini kullanın.

- [Azure Güvenlik Merkezi kimlik ve erişimini anlama](../security-center/security-center-identity-access.md)

- [PostgreSQL için Azure veritabanı 'nda yönetici kullanıcılar oluşturmayı anlayın](../postgresql/howto-create-users.md#the-server-admin-account)

- [Azure Ilkesini kullanma](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory (Azure AD) MULTI-Factor AUTHENTICATION (MFA) etkinleştirin ve Güvenlik Merkezi 'nin kimlik ve erişim yönetimi önerilerini izleyin.

- [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

- [Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) ve ilgili kaynakları açmak ve yapılandırmak için yapılandırılmış MULTI-Factor AUTHENTICATION (MFA) Ile ayrıcalıklı erişim iş istasyonu (Paw) kullanın. 

- [Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/) 

- [Azure'da çok faktörlü kimlik doğrulamasını etkinleştirme](../active-directory/authentication/howto-mfa-getstarted.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinliklerle ilgili günlüğe kaydet ve uyar

**Rehberlik**: ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda Günlükler ve uyarılar oluşturmak için Azure Active Directory (Azure AD) güvenlik raporları kullanın. Kimlik ve erişim etkinliğini izlemek için Güvenlik Merkezi 'ni kullanın.

- [Riskli etkinlik bayrağıyla işaretlenen Azure AD kullanıcılarını belirleme](../active-directory/identity-protection/overview-identity-protection.md)

- [Azure Güvenlik Merkezi’nde kullanıcıların kimliğini ve erişim etkinliğini izleme](../security-center/security-center-identity-access.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: koşullu erişim ilkesinin konum koşulunu yapılandırın ve adlandırılmış konumlarınızı yönetin. 

Adlandırılmış konumlara sahip IP adresi aralıklarının veya ülkelerin ve bölgelerin mantıksal gruplandırmaları oluşturun. Azure Key Vault gizli dizileri gibi hassas kaynaklarınıza erişimi, yapılandırılmış adlandırılmış konumlara göre kısıtlayın.

- [Koşullu erişim Azure Active Directory konum koşulu nedir?](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (Azure AD) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifreleme kullanarak verileri korur ve ayrıca, salları, karmaları ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.
- [Azure AD örneği oluşturma ve yapılandırma](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) 

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory (Azure AD) eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Grup üyeliklerini etkin bir şekilde yönetmek, kurumsal uygulamalara erişmek ve rol atamalarına yönelik Azure kimlik erişimi Incelemelerini kullanın. Yalnızca etkin kullanıcıların erişmeye devam ettiğinden emin olmak için Kullanıcı erişimini düzenli aralıklarla gözden geçirin.

- [Azure AD raporlamayı anlama](../active-directory/reports-monitoring/index.yml)

- [Azure kimlik erişimi Incelemelerini kullanma](../active-directory/governance/access-reviews-overview.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: devre dışı bırakılmış kimlik bilgilerine erişme girişimlerini izleme

**Rehberlik**: Azure Active Directory (Azure AD) oturum açma etkinliğini tümleştirin, Azure Sentinel gibi herhangi bir SIEM veya izleme aracıyla, olay günlüğü kaynaklarını denetleyin ve riskten yararlanın.

Azure Active Directory (Azure AD) Kullanıcı hesapları için Tanılama ayarları oluşturarak ve denetim günlüklerini ve oturum açma günlüklerini bir Log Analytics çalışma alanına göndererek bu işlemi kolaylaştırın. Log Analytics çalışma alanında istenen uyarıları yapılandırın.

- [Azure Etkinlik Günlüklerini Azure İzleyici ile tümleştirme](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapması üzerinde uyarı

**Rehberlik**: Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak için Azure Active Directory (Azure AD) risk ve kimlik koruması özelliklerini kullanın. Daha fazla araştırma için Azure Sentinel 'e veri alma.

- [Azure AD riskli oturum açma işlemlerini görüntüleme](../active-directory/identity-protection/overview-identity-protection.md)

- [Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Azure Sentinel 'i ekleme](../sentinel/quickstart-onboard.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

## <a name="data-protection"></a>Veri koruma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri koruma](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) ve hassas bilgileri depolayan veya işleyen ilgili kaynakları izlemeye yardımcı olması için Etiketler kullanın.
- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: ortam türü ve veri duyarlılığı düzeyi gibi bireysel güvenlik etki alanları için, geliştirme, test ve üretim ortamları gibi ayrı abonelikler ve yönetim grupları kullanarak yalıtım uygulayın. 

Azure rol tabanlı erişim denetimi (Azure RBAC) ile Azure kaynaklarına erişimi denetleme.

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../governance/management-groups/create-management-group-portal.md)

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: yoldaki tüm hassas bilgileri şifreleyin. Azure Web uygulaması güvenlik duvarı (WAF) örneklerine ve ilgili kaynaklara bağlanan tüm istemcilerin TLS 1,2 veya üzerini anlaşamadığından emin olun.

Uygun olduğunda, bekleyen ve şifreli şifreleme için Güvenlik Merkezi önerilerini izleyin.

- [Azure ile iletim sırasında şifrelemeyi anlama](../security/fundamentals/encryption-overview.md#encryption-of-data-in-transit)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Kılavuz**: Azure rol tabanlı erişim denetimi (Azure RBAC) ile Azure kaynaklarına erişimi denetleme.
- [Azure RBAC 'yi yapılandırma](../role-based-access-control/role-assignments-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) ve ilgili kaynakları dahil tüm Azure kaynakları için bekleyen şifreleme kullanın. Microsoft, Azure 'un şifreleme anahtarlarınızı yönetmesine izin vermesini önerir, ancak bazı örneklerde kendi anahtarlarınızı yönetmeniz için seçenek vardır.

- [Azure’da bekleyen veri şifrelemesini anlama](../security/fundamentals/encryption-atrest.md)

- [Müşteri tarafından yönetilen şifreleme anahtarlarını yapılandırma](../storage/common/customer-managed-keys-configure-key-vault.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Web uygulaması güvenlik duvarını (WAF), önceden belirlenmiş bir süre için algılama modundaki ağ trafiğini taban çizgisi oluşturulduktan sonra önleme modunda çalışacak şekilde yapılandırın. 

Önleme modundaki Azure WAF, kurallar tarafından algılanan yetkisiz saldırıları ve saldırıları engeller. Saldırgan bir "403 yetkisiz erişim" özel durumu alır ve bağlantı kapatılır. Önleme modu WAF günlüklerinde bu tür saldırıları kaydeder.

- [Application Gateway ile Azure Güvenlik Merkezi arasındaki tümleştirmeye genel bakış](../security-center/security-center-partner-integration.md)

- [Application Gateway WAF modları](ag/ag-overview.md#waf-modes)

- [Ön kapıda WAF modları](afds/afds-overview.md#waf-modes)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="inventory-and-asset-management"></a>Envanter ve varlık yönetimi

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: envanter ve varlık yönetimi](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: otomatik varlık bulma çözümünü kullanma

**Rehberlik**: aboneliklerinizde, işlem, depolama, ağ, bağlantı noktaları ve protokoller gibi tüm kaynakları sorgulamak veya öğrenmek Için Azure Kaynak grafiğini kullanın. 

Kiracınızda uygun (okuma) izinlere sahip olun ve aboneliklerinizdeki kaynakların yanı sıra tüm Azure aboneliklerini numaralandırın. Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

- [Azure Kaynak Graf ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

- [Azure aboneliklerinizi görüntüleme](/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

- [Azure RBAC 'yi anlama](../role-based-access-control/overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) ilkelerinde Etiketler kullanın. Etiketler, kaynaklarla ilişkilendirilebilir ve bir müşteri aboneliğinde bu kaynaklara erişimi düzenlemek için mantıksal olarak uygulanabilir. 

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure WAF ve ilgili kaynakları düzenlemek ve izlemek için uygun yerlerde etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

- [Ek Azure abonelikleri oluşturma](../cost-management-billing/manage/create-subscription.md)

- [Yönetim Grupları oluşturma](../governance/management-groups/create-management-group-portal.md)

- [Etiketler oluşturma ve kullanma](../azure-resource-manager/management/tag-resources.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: onaylanan Azure kaynaklarının envanterini tanımlayın ve saklayın

**Rehberlik**: Kurumsal ihtiyaçlarına göre yapılandırma dahil olmak üzere onaylanan kaynakların envanterini oluşturun.

Aboneliklerinizde oluşturulabilecek kaynak türlerine kısıtlamalar koymak için Azure Ilkesini kullanın. Abonelikler içindeki kaynakları sorgulamak ve bulmak için Azure Kaynak Grafı'nı kullanın. Ortamda bulunan tüm Azure kaynaklarının onaylandığından emin olun.

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Kaynak Graf ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aboneliklerinizde oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın.
Azure Kaynak Grafiği 'ni kullanarak aboneliklerindeki Azure Web uygulaması güvenlik duvarı (WAF) kaynaklarını sorgulama veya bulma. Ortamda bulunan tüm Azure WAF ve ilgili kaynakların onaylandığından emin olun.

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Graph ile sorgu oluşturma](../governance/resource-graph/first-query-portal.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: Azure WAF dağıtımını veya belirli bir WAF türünü (örneğin, Azure WAF v1 vs v2) reddetmek Için Azure Ilkesiyle onaylanmamış Azure WAF kaynaklarını izleyin ve kaldırın.

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: ortamınızda sağlayabileceğiniz hizmetleri kısıtlamak Için Azure ilkesini kullanın.

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Ilkesiyle belirli bir kaynak türünü reddetme](../governance/policy/samples/index.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: kullanıcıların Azure Resource Manager etkileşime geçme yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" özelliğini yapılandırarak bir kullanıcının Azure Kaynak Yöneticisi ile etkileşime geçmesini sınırlamak Için Azure koşullu erişimi kullanın.

- [Azure Kaynak Yöneticisi 'ne erişimi engellemek için koşullu erişimi yapılandırma](../role-based-access-control/conditional-access-azure-management.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: Azure Web uygulaması güvenlik duvarı (WAF), yüksek riskli uygulamaları ayırmak için ağlar, kaynak grupları veya abonelikler aracılığıyla farklı ortamlarla ilişkilendirilebilir.

- [Azure sanal ağına genel bakış](../virtual-network/virtual-networks-overview.md)

- [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../governance/management-groups/overview.md)

- [Abonelik kararı kılavuzu](/azure/cloud-adoption-framework/decision-guides/subscriptions/)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="secure-configuration"></a>Güvenli yapılandırma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: güvenli yapılandırma](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) dağıtımlarınızla ilgili ağ ayarları için standart güvenlik yapılandırması tanımlayın ve uygulayın.
Azure uygulama ağ geçitleri, sanal ağlar, ağ güvenlik grupları ve yerleşik ilke tanımları kullanmak üzere özel ilkeler oluşturmak için "Microsoft. Network" ad alanındaki Azure Ilke diğer adlarını kullanın.

- [Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme](/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0)

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure Web uygulaması güvenlik duvarı (WAF) ve ilgili kaynaklarınız üzerinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yok] etkileri kullanın. 

Azure WAF 'nizin güvenlik yapılandırmasını ve kuruluşunuzun gerektirdiği ilgili kaynakları korumak için Azure Resource Manager şablonlarını kullanın.

- [Azure Ilke efektlerini anlama](../governance/policy/concepts/effects.md)

- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure Resource Manager şablonlarına genel bakış](../azure-resource-manager/templates/overview.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Kılavuz**: özel Azure ilkeleri ve Azure Resource Manager şablonları gibi kodunuzu güvenli bir şekilde depolamak ve yönetmek Için Azure DevOps kullanın. 

Belirli kullanıcılar, yerleşik güvenlik grupları veya Azure Active Directory (Azure AD) içinde tanımlanan gruplar, Azure DevOps ile tümleşikse veya Team Foundation Server (TFS) ile tümleşikse Active Directory izin verin veya reddedin.

- [Azure DevOps 'da kod depolama](/azure/devops/repos/git/gitworkflow?view=azure-devops)

- [Azure DevOps 'da izinler ve gruplar hakkında](/azure/devops/organizations/security/about-permissions)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Azure kaynakları için yapılandırma yönetimi araçları dağıtma

**Rehberlik**: yerleşik Azure ilke tanımlarını ve sistem yapılandırmalarının uyarı vermesi, denetlemesi ve uygulanması için özel ilkeler oluşturmak üzere "Microsoft. Network" ad alanındaki Azure ilkesi diğer adları ' nı kullanın. İlke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure İlkesi belgeleri](../governance/policy/index.yml)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Azure kaynakları için otomatik yapılandırma izlemeyi uygulama

**Rehberlik**: yerleşik Azure ilke tanımlarını ve sistem yapılandırmalarının uyarı vermesi, denetlemesi ve uygulanması için özel ilkeler oluşturmak üzere "Microsoft. Network" ad alanındaki Azure ilkesi diğer adları ' nı kullanın. 

Azure kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yok] etkileri ' nı kullanın.

- [Azure İlkesi'ni yapılandırma ve yönetme](../governance/policy/tutorials/create-and-manage.md)

- [Azure İlkesi belgeleri](../governance/policy/index.yml)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: sertifikaları güvenli bir şekilde depolamak için Azure Key Vault kullanın. Azure Key Vault, gizli dizileri, anahtarları ve SSL sertifikalarını korumak için kullanabileceğiniz, platform tarafından yönetilen bir gizli depodır. 

Azure Application Gateway, HTTPS özellikli dinleyicilerine eklenen sunucu sertifikaları için Key Vault tümleştirmeyi destekler. 

- [Azure PowerShell kullanarak SSL sonlandırmasını Key Vault sertifikalarla yapılandırma](../application-gateway/configure-keyvault-ps.md)

**Azure Güvenlik Merkezi izlemesi**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini belirlemek Için kimlik bilgisi tarayıcısını uygulayın ve bu da keşfedilen kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı teşvik eder.
- [Kimlik bilgisi tarayıcısı kurulumu](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="data-recovery"></a>Veri kurtarma

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: veri kurtarma](../security/benchmarks/security-control-data-recovery.md).*

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Rehberlik**: Azure Key Vault için geçici silmenin etkinleştirildiğinden emin olun. Geçici silme, silinen anahtar kasalarının ve anahtar, gizli dizi ve sertifika gibi kasa nesnelerinin kurtarılmasına olanak tanır.

- [Azure Key Vault geçici silme kullanma](../key-vault/general/key-vault-recovery.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

## <a name="incident-response"></a>Olay yanıtı

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: olay yanıtı](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu geliştirin. Tüm personel rollerinin yanı sıra olay işleme ve yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun. 

- [Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/) 

- [Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın anatomisi](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/) 

- [Kendi olay yanıtı planınızın oluşturulmasına yardımcı olması için NıST 'nin bilgisayar güvenliği olay Işleme kılavuzunu kullanın](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her uyarıya önem derecesi atar. Önem derecesi, uyarı veren etkinliğin arkasında kötü amaçlı bir amaç olduğunu ve uyarıyı vermek için kullanılan analitik düzeyini, ne kadar güvenli bir güvenlik merkezinin olduğunu temel alır.
Abonelikleri açık bir şekilde işaretleyin (örneğin, üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve açıkları belirleyip planı gerektiği şekilde gözden geçirin.
- [BT planları ve özellikleri için test, eğitim ve alıştırma programlarını denemek üzere NıST 'nin yayın kılavuzuna bakın](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır. Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.
- [Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama](../security-center/security-center-provide-security-contact-details.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak güvenlik merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Azure Güvenlik Merkezi veri bağlayıcısını, kuruluşunuzun gereksinimlerine göre uyarıları Azure Sentinel 'e akışa almak için kullanın.

- [Sürekli dışarı aktarmayı yapılandırma](../security-center/continuous-export.md)

- [Uyarıların Azure Sentinel’e akışını yapma](../sentinel/connect-azure-security-center.md)

**Azure Güvenlik Merkezi izlemesi**: Yes

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Yönergeler**: güvenlik uyarıları ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Güvenlik Merkezi 'Ndeki Iş akışı Otomasyonu özelliğini kullanın.
- [Iş akışı otomasyonu ve Logic Apps yapılandırma](../security-center/workflow-automation.md)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma testleri ve red team alıştırmaları

*Daha fazla bilgi için bkz. [Azure Güvenlik kıyaslaması: Penetme testleri ve Red ekibi alıştırmaları](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: sızma testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin. Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına karşı kırmızı ekip oluşturma ve canlı site sızma testi stratejisini kullanın. 

- [Sızma Testi Etkileşim Kuralları](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Microsoft Bulut ile Kırmızı Takım Oluşturma](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Güvenlik Merkezi ile izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](../security/benchmarks/overview.md)
- [Azure güvenlik temelleri](../security/benchmarks/security-baselines-overview.md) hakkında daha fazla bilgi edinin
