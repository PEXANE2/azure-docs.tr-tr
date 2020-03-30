---
title: Cosmos DB için Azure Güvenlik Taban Çizgisi
description: Cosmos DB için Azure Güvenlik Taban Çizgisi
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: fd2706b6a6880d7c9454619e1315b2b5b5404561
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80244282"
---
# <a name="azure-security-baseline-for-cosmos-db"></a>Cosmos DB için Azure Güvenlik Taban Çizgisi

Cosmos DB için Azure Güvenlik Taban Çizgisi, dağıtımınızın güvenlik duruşunu geliştirmenize yardımcı olacak öneriler içerir.

Bu hizmetin temeli, en iyi uygulamalar kılavuzumuzla Azure'da bulut çözümlerinizi nasıl güvenebileceğinize ilişkin öneriler sunan [Azure Güvenlik Kıyaslama sürümü 1.0'dan](https://docs.microsoft.com/azure/security/benchmarks/overview)alınmıştır.

Daha fazla bilgi için [Azure Güvenlik Taban Çizgilerini genel olarak](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)görün.

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için [Güvenlik Denetimi: Ağ Güvenliği'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security)bakın.*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1.1: Sanal Ağınızdaki Ağ Güvenlik Grupları veya Azure Güvenlik Duvarı'nı kullanarak kaynakları koruyun

**Kılavuz**: Azure Özel Bağlantı'nı kullanarak, Bir Azure Cosmos hesabına Özel Bitiş Noktası üzerinden bağlanabilirsiniz. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Ayrıca, bir ağ güvenlik grubunda (NSG) sıkı bir giden kural kümesini yapılandırarak ve bu NSG'yi alt ağınızla ilişkilendirerek veri sızma riskini de azaltabilirsiniz.

Azure Cosmos hesabınızı güvence altına almak için Hizmet Bitiş Noktaları'nı da kullanabilirsiniz. Bir Hizmet Bitiş Noktası'nı etkinleştirerek, Azure Cosmos hesaplarını yalnızca azure sanal ağının belirli bir alt noktasından erişime izin verecek şekilde yapılandırabilirsiniz. Azure Cosmos DB Hizmet Bitiş Noktası etkinleştirildikten sonra, sanal ağdaki bir alt ağdan gelen bağlantılarla bir Azure Cosmos hesabına erişimi sınırlandırabilirsiniz.

Ayrıca, IP güvenlik duvarlarını kullanarak Azure Cosmos hesabınızda depolanan verileri de güvene alabilirsiniz. Azure Cosmos DB, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Azure portalını, Azure Kaynak Yöneticisi şablonlarını kullanarak veya Azure CLI veya Azure PowerShell aracılığıyla Azure Cosmos hesabında bir IP güvenlik duvarı ayarlayabilirsiniz.

Azure Özel Bağlantı genel bakış:https://docs.microsoft.com/azure/private-link/private-link-overview

Azure Cosmos DB için Özel Bitiş Noktası nasıl yapılandırılabilen:https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints 

Güvenlik Config'i olan bir Ağ Güvenlik Grubu nasıl oluşturulur:https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Cosmos DB'de IP güvenlik duvarı nasıl yapılandırılabilen:https://docs.microsoft.com/azure/cosmos-db/how-to-configure-firewall

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1.2: Vnet, Subnet ve NIC'lerin yapılandırmasını ve trafiğini izleyin ve kaydedin

**Yönlendirme**: Azure Cosmos hesabınızla ilgili ağ kaynaklarının güvenliğini sağlamaya yardımcı olmak için Azure Güvenlik Merkezi'ni kullanın ve ağ koruma önerilerini izleyin.

Sanal makineler Azure Cosmos hesabınızla aynı sanal ağda dağıtıldığında, veri sızma riskini azaltmak için bir ağ güvenlik grubu (NSG) kullanabilirsiniz. Trafik denetimleri için NSG akış günlüklerini etkinleştirin ve günlükleri bir Azure Depolama Hesabı'na gönderin. Ayrıca NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

Azure Güvenlik Merkezi tarafından sağlanan Ağ Güvenliğini Anlayın:https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

NSG Akış Günlükleri Nasıl Etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi ni etkinleştirme ve kullanma:https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="13-protect-critical-web-applications"></a>1.3: Kritik web uygulamalarını koruyun

**Yönerge**: Bir etki alanı altında çalışan bir web uygulamasının başka bir etki alanındaki kaynaklara erişmesini sağlamak için Kökenler Arası Kaynak Paylaşımı (CORS) özelliğini kullanın. Web tarayıcıları, bir web sayfasının farklı bir etki alanında API'leri aramasını engelleyen aynı kaynak ilkesi olarak bilinen bir güvenlik kısıtlaması uygular. Ancak CORS, kaynak etki alanının başka bir etki alanında API'leri aramasına izin vermek için güvenli bir yol sağlar. Azure Cosmos hesabınız için CORS desteğini etkinleştirdikten sonra, yalnızca kimlik doğrulaması yapılan istekler, belirttiğiniz kurallara göre izin verilip verilmediğini belirlemek için değerlendirilir.

Çapraz Kaynak Paylaşımını Yapılandır:https://docs.microsoft.com/azure/cosmos-db/how-to-configure-cross-origin-resource-sharing

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4: Bilinen kötü amaçlı IP adresleriyle iletişimi reddetme

**Rehberlik**: Azure Cosmos DB için Gelişmiş Tehdit Koruması (ATP) kullanın. Azure Cosmos DB için ATP, Azure Cosmos hesaplarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, tehditleri ele almanızı ve bunları merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır.

DDoS saldırılarına karşı korunmak için Azure Cosmos DB örneklerinizle ilişkili Sanal Ağlarda DDoS Koruma Standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimi reddetmek için Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı kullanın.

Azure Cosmos DB Gelişmiş Tehdit Koruması nasıl yapılandırılabilen:https://docs.microsoft.com/azure/cosmos-db/cosmos-db-advanced-threat-protection

DDoS koruması nasıl yapılandırılabilen:https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Merkezi Tümleşik Tehdit İstihbaratı'nı Anlayın:https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1.5: Ağ paketlerini ve akış günlüklerini kaydetme

**Kılavuz**: Ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve trafik denetimi için günlükleri bir depolama hesabına gönderin. NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzdaki trafik akışıhakkında öngörüler sağlamak için Trafik Analitiği'ni kullanabilirsiniz. Traffic Analytics'in bazı avantajları, ağ etkinliğini görselleştirme ve etkin noktaları belirleme, güvenlik tehditlerini belirleme, trafik akışı desenlerini anlama ve ağ yanlış yapılandırmalarını saptabilme yeteneğidir.

NSG Akış Günlükleri Nasıl Etkinleştirilir:https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi ni etkinleştirme ve kullanma:https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6: Ağ tabanlı izinsiz giriş algılama/saldırı önleme sistemlerini (IDS/IPS) dağıtma

**Rehberlik**: Azure Cosmos DB için Gelişmiş Tehdit Koruması (ATP) kullanın. Azure Cosmos DB için ATP, Azure Cosmos hesaplarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, tehditleri ele almanızı ve bunları merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır. 

Cosmos DB Gelişmiş Tehdit Koruması nasıl yapılandırılabilen:https://docs.microsoft.com/azure/cosmos-db/cosmos-db-advanced-threat-protection

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1.7: Web uygulamalarına trafik yönetme

**Rehberlik**: Geçerli değildir; önerisi, Azure Uygulama Hizmeti veya bilgi işlem kaynakları üzerinde çalışan web uygulamaları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8: Ağ güvenliği kurallarının karmaşıklığını ve yönetim ek lerini en aza indirmek

**Kılavuz**: Azure Cosmos hesabınıza erişması gereken kaynaklar için, ağ güvenlik gGroups'larında veya Azure Güvenlik Duvarı'ndaki ağ erişim denetimlerini tanımlamak için Sanal Ağ hizmet etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adını (örn. AzureCosmosDB) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz. Microsoft, hizmet etiketinin kapsadığı adres öneklerini yönetir ve adresler değiştikçe servis etiketini otomatik olarak güncelleştirir.

Hizmet etiketlerini kullanma hakkında daha fazla bilgi için:https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9: Ağ aygıtları için standart güvenlik yapılandırmalarını koruyun

**Yönerge**: Azure İlkesi ile ağ kaynakları için standart güvenlik yapılandırmaları tanımlayın ve uygulayın. Azure Cosmos DB örneklerinizin ağ yapılandırmasını denetlemek veya zorlamak için özel ilkeler oluşturmak için "Microsoft.DocumentDB" ve "Microsoft.Network" ad alanlarında Azure İlkesi takma adlarını kullanın. Azure Cosmos DB için yerleşik ilke tanımlarından da yararlanabilirsiniz:

- Cosmos DB Hesapları için Gelişmiş Tehdit Koruması Dağıtma

- Cosmos DB sanal ağ hizmeti bitiş noktası kullanmalıdır

Azure Kaynak Yöneticisi şablonları, rol tabanlı erişim denetimi (RBAC) ve tek bir plan tanımındaki ilkeler gibi önemli ortam yapılarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure Planları'nı da kullanabilirsiniz. Sürümü aracılığıyla planı yeni aboneliklere, ortamlara ve ince ayar denetimine ve yönetimine kolayca uygulayabilirsiniz.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Blueprint nasıl oluşturulur:https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="110-document-traffic-configuration-rules"></a>1.10: Belge trafiği yapılandırma kuralları

**Yönerge**: Azure Cosmos DB dağıtımınızla ilişkili ağ kaynakları için etiketleri kullanarak bunları mantıksal olarak taksonomi olarak düzenleyin.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11: Ağ kaynağı yapılandırmalarını izlemek ve değişiklikleri algılamak için otomatik araçlar kullanın

**Yönerge**: Ağ kaynak yapılandırmalarını izlemek ve Azure Cosmos DB örneklerinizle ilgili ağ kaynaklarındaki değişiklikleri algılamak için Azure Etkinlik Günlüğü'ne kullanın. Azure Monitor'da kritik ağ kaynaklarında değişiklikler olduğunda tetikleyecek uyarılar oluşturun. 

Azure Etkinlik Günlüğü etkinliklerini görüntüleme ve alma:https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Monitor'da uyarı oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için [Güvenlik Denetimi: Günlüğe kaydetme ve izleme.](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring)*

### <a name="21-use-approved-time-synchronization-sources"></a>2.1: Onaylanmış zaman senkronizasyon kaynaklarını kullanma

**Kılavuz :** Microsoft, günlüklerde zaman damgaları için Azure Cosmos DB gibi Azure kaynakları için kullanılan zaman kaynağını korur.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2.2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Yönlendirme**: Azure Cosmos DB tarafından oluşturulan güvenlik verilerini toplamak için günlükleri Azure Monitor üzerinden alın. Azure Monitörü'nde, analitiği sorgulamak ve gerçekleştirmek için Log Analytics çalışma alanı(lar)'ı kullanın ve uzun vadeli/arşivdepolama için depolama hesaplarını kullanın. Alternatif olarak, Azure Sentinel'e veya üçüncü taraf Güvenlik Olayı ve Olay Yönetimi'ne (SIEM) veri sağlayabilir ve yerleşik veriler sağlayabilirsiniz. 

Azure Cosmos DB için tanılama günlüklerini etkinleştirme:https://docs.microsoft.com/azure/cosmos-db/logging

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3: Azure kaynakları için denetim günlüğe kaydetmeyi etkinleştirme

**Yönlendirme**: Azure Cosmos DB için tanı ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına veya depolama hesabına gönderin. Kaynak günlüklerini toplamak için Azure Cosmos DB'deki tanılama ayarları kullanılır. Bu günlükler istek başına yakalanır ve "veri düzlemi günlükleri" olarak da adlandırılır. Veri düzlemi işlemlerine bazı örnekler silme, ekleme ve okuma dır. Ayrıca Azure Etkinlik Günlüğü Tanılama Ayarlarını etkinleştirebilir ve bunları aynı Log Analytics Çalışma alanına gönderebilirsiniz.

Azure Cosmos DB için Tanı lama Ayarları nasıl etkinleştirilir:https://docs.microsoft.com/azure/cosmos-db/logging

Azure Etkinlik Günlüğü için Tanı lama Ayarları nasıl etkinleştirilir:https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4: İşletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2.5: Güvenlik günlüğü depolama tutma yapılandırma

**Kılavuz :** Azure Monitor'da, Kuruluşunuzun uyumluluk yönetmeliklerine göre Azure Cosmos DB örneklerinizle ilişkili Günlük Analizi çalışma alanlarıiçin günlük bekletme süresini ayarlayın.

Günlük bekletme parametreleri nasıl ayarlanır:https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="26-monitor-and-review-logs"></a>2.6: Günlükleri izleme ve inceleme

**Yönlendirme**: Log Analytics'te sorguları, terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve topladığınız Azure Cosmos DB günlüklerini temel alan diğer birçok öngörüsağlamak için bir çalışma alanı gerçekleştirebilirsiniz.

Log Analytics Çalışma Alanlarında Azure Cosmos DB sorguları nasıl gerçekleştirilir:https://docs.microsoft.com/azure/cosmos-db/monitor-cosmos-db

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2.7: Anormal aktivite için uyarıları etkinleştirme

**Kılavuz :** Azure Güvenlik Merkezi'nde, güvenlik günlükleri ve etkinliklerindeki anormal etkinlikleri izlemek için Azure Cosmos DB için Gelişmiş Tehdit Koruması'nı etkinleştirin. Azure Cosmos DB'de tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin.

 

Güvenlik düzenleme otomatik yanıt (SOAR) çözümü sağladığından, Log Analytics çalışma alanınızda Azure Sentinel'e de binebilirsiniz. Bu, oyun kitaplarının (otomatik çözümler) oluşturulmasına ve güvenlik sorunlarını düzeltmek için kullanılmasına olanak tanır. Ayrıca, Azure Monitor'u kullanarak Log Analytics çalışma alanınızda özel günlük uyarıları oluşturabilirsiniz.

Azure Cosmos DB için tehdit koruması uyarıları listesi:https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurecosmos

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Azure Monitor'u kullanarak günlük uyarıları oluşturun, görüntüleyin ve yönetin:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="28-centralize-anti-malware-logging"></a>2.8: Kötü amaçlı yazılımdan koruma günlüğe kaydetmeyi merkezileştirin

**Rehberlik**: Geçerli değildir; Azure Cosmos DB, kötü amaçlı yazılımdan koruma yla ilgili günlükleri işlemez veya üretmez.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="29-enable-dns-query-logging"></a>2.9: DNS sorgu günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; Azure Cosmos DB, DNS ile ilgili günlükleri işlemez veya üretmez.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2.10: Komut satırı denetim günlüğe kaydetmeyi etkinleştirme

**Rehberlik**: Geçerli değildir; bu öneri, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için [güvenlik denetimi: kimlik ve erişim denetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control)bakın.*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1: İdari hesapların envanterini korumak

**Kılavuz :** Rol tabanlı erişim denetimini (RBAC) yapılandırmak ve Azure Cosmos DB kaynaklarında envanter tutmak için Azure portalındaki Kimlik ve Erişim denetimi (IAM) bölmesini kullanabilirsiniz. Roller, Etkin Dizini'ndeki kullanıcılara, gruplara, hizmet ilkelerine ve yönetilen kimliklere uygulanır. Kişiler ve gruplar için yerleşik rolleri veya özel rolleri kullanabilirsiniz.

Azure Cosmos DB, Azure Cosmos DB'deki yaygın yönetim senaryoları için yerleşik RBAC sağlar. Azure Active Directory 'de (AD) profili olan bir kişi, Azure Cosmos DB kaynaklarındaki kaynaklara ve işlemlere erişim sağlamak veya reddetmek için bu RBAC rollerini kullanıcılara, gruplara, hizmet ilkelerine veya yönetilen kimliklere atayabilir.

Ayrıca, yönetim gruplarının üyesi hesapları bulmak için geçici sorgular gerçekleştirmek için Azure AD PowerShell modüllerini de kullanabilirsiniz. 

Ayrıca, Azure Cosmos DB'deki bazı eylemler Azure Etkin Dizin ve hesaba özgü ana anahtarlarla denetlenebilir.  Anahtar erişimini denetlemek için 'disableKeyBasedMetadataWriteAccess' hesap ayarını kullanın.

Azure Cosmos DB'de rol tabanlı erişim denetimini anlayın:https://docs.microsoft.com/azure/cosmos-db/role-based-access-control

Azure Cosmos DB Eylemleri'ni (Microsoft.DocumentDB ad alanı) kullanarak kendi özel rollerinizi oluşturun:https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftdocumentdb

Azure Etkin Dizini'nde yeni bir rol oluşturun:https://docs.microsoft.com/azure/role-based-access-control/custom-roles

PowerShell ile Azure Active Directory'de dizin rolü nasıl elde elabilirsiniz:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

PowerShell ile Azure Active Directory'de dizin rolünün üyelerini nasıl elde edinilir:https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

Yalnızca veri işlemlerinde kullanıcı erişimini kısıtlama:https://docs.microsoft.com/azure/cosmos-db/how-to-restrict-user-data

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3.2: Varsayılan parolaları varsa değiştirme

**Kılavuz**: Varsayılan veya boş parolakavramı Azure AD veya Azure Cosmos DB ile ilişkili olarak bulunmaz. Bunun yerine, Azure Cosmos DB, kullanıcıların kimliğini doğrulamak ve verilerine ve kaynaklarına erişim sağlamak için iki tür anahtar kullanır; ana anahtarlar ve kaynak belirteçleri. Anahtarlar herhangi bir zamanda yeniden oluşturulabilir.

Azure Cosmos DB'deki verilere güvenli erişimi anlama:https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data

Azure Cosmos DB Tuşları nasıl yeniden üretilir:https://docs.microsoft.com/azure/cosmos-db/manage-with-powershell#regenerate-keys

Azure Active Directory'yi kullanarak anahtarlara programlı olarak nasıl erişilir:https://docs.microsoft.com/azure/cosmos-db/certificate-based-authentication

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="33-use-dedicated-administrative-accounts"></a>3.3: Özel yönetim hesaplarını kullanma

**Rehberlik**: Geçerli değildir; Azure Cosmos DB yönetici hesaplarını desteklemez.  Tüm erişim, Azure Etkin Dizin ve Azure rol tabanlı erişim denetimi (RBAC) ile tümleştirilir.



**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3.4: Azure Active Directory ile tek oturum açma (SSO) kullanma

**Yönlendirme**: Azure Cosmos DB, kullanıcıları yetkilendirmek için iki tür anahtar kullanır ve veri düzlemi düzeyinde Tek Oturum Açma 'yı (SSO) desteklemez. Cosmos DB için kontrol uçağına REST API üzerinden erişim mevcuttur ve SSO'yu destekler. Kimlik doğrulaması yapmak için, istekleriniz için Yetkilendirme üstbilgisini Azure Active Directory'den aldığınız bir JSON Web Belirteci'ne ayarlayın.

Cosmos DB REST API için Azure Veritabanını Anlayın:https://docs.microsoft.com/rest/api/cosmos-db/

Azure Etkin Dizini ile SSO'yi anlayın:https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5: Tüm Azure Active Directory tabanlı erişim için çok faktörlü kimlik doğrulamayı kullanın

**Yönerge**: Azure Active Directory Çok Faktörlü Kimlik Doğrulamasını etkinleştirin ve Azure Güvenlik Merkezi Kimlik ve Erişim Yönetimi önerilerini izleyin.

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi'nde kimlik ve erişim nasıl izlenir:https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6: Tüm idari görevler için özel makineleri (Ayrıcalıklı Erişim İş İstasyonları) kullanın

**Kılavuz :** Azure kaynaklarına giriş yapmak ve yapılandırmak için yapılandırılan Çok Faktörlü Kimlik Doğrulama ile Ayrıcalıklı Erişim İş İstasyonlarını (PAW) kullanın.

Ayrıcalıklı Erişim İş İstasyonları hakkında bilgi edinin:https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure'da MFA nasıl etkinleştirilir:https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3.7: İdari hesaplardan şüpheli etkinliklere ilişkin günlük ve uyarı

**Rehberlik**: Azure Cosmos DB için Gelişmiş Tehdit Koruması (ATP) kullanın. Azure Cosmos DB için ATP, Azure Cosmos hesaplarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, tehditleri ele almanızı ve bunları merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır. 

Ayrıca, azure etkin dizin (AD) Ayrıcalıklı Kimlik Yönetimi'ni (PIM) ortamda şüpheli veya güvensiz etkinlik oluştuğunda günlük ve uyarı oluşturma için kullanabilirsiniz.

Riskli kullanıcı davranışıyla ilgili uyarıları ve raporları görüntülemek için Azure AD Risk Algılamalarını kullanın.

Ayrıcalıklı Kimlik Yönetimi (PIM) nasıl dağıtılır:https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure AD risk algılamalarını anlayın:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Yönerge**: Koşullu Erişim ilkesinin konum koşulunu yapılandırın ve adlandırılmış konumlarınızı yönetin. Adlandırılmış konumlarla, IP adresi aralıklarının veya ülkelerin ve bölgelerin mantıksal gruplandırmalarını oluşturabilirsiniz. Azure Cosmos DB örnekleriniz gibi hassas kaynaklara erişimi yapılandırılan konumlarınızla sınırlandırabilirsiniz.

Azure'da Adlandırılmış Konumlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="39-use-azure-active-directory"></a>3.9: Azure Etkin Dizini Kullanma

**Kılavuz :** Merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure Active Directory (AD) kullanın. Azure AD, veri için güçlü şifreleme kullanarak verileri istirahatte ve aktarım sırasında korur. Azure AD ayrıca kullanıcı kimlik bilgilerini tuzlar, iş lerle ve güvenli bir şekilde depolar.

Azure Active Directory örneğini oluşturma ve yapılandırma:https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance

Azure Active Directory kimlik doğrulamasını Azure SQL ile yapılandırma ve yönetme:https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10: Kullanıcı erişimini düzenli olarak gözden geçirin ve uzlaştırın

**Kılavuz :** Azure Active Directory, eski hesapları keşfetmeye yardımcı olmak için günlükler sağlar. Ayrıca, grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarıverimli bir şekilde yönetmek için Azure Kimlik Erişim İncelemeleri'ni kullanabilirsiniz. Yalnızca doğru Kullanıcıların sürekli erişime sahip olduğundan emin olmak için kullanıcıerişimi düzenli olarak gözden geçirilebilir.

Azure Kimlik Erişim İncelemeleri nasıl kullanılır:https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3.11: Devre dışı bırakılmış hesaplara erişim denemelerini izleme

**Yönerge**: Azure Active Directory kullanıcı hesapları için tanılama ayarları oluşturabilir, denetim günlüklerini ve oturum açma günlüklerini istediğiniz uyarıları yapılandırabileceğiniz bir Log Analytics çalışma alanına gönderebilirsiniz.

Azure Etkinlik Günlükleri Azure Monitör'e nasıl entegre eleştirilir:https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3.12: Hesap giriş davranış sapması konusunda uyarı

**Rehberlik**: Azure Cosmos DB için Gelişmiş Tehdit Koruması (ATP) kullanın. Azure Cosmos DB için ATP, Azure Cosmos hesaplarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, tehditleri ele almanızı ve bunları merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır. 

Kullanıcı kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtları yapılandırmak için Azure AD Kimlik Koruması ve risk algılama özelliğini de kullanabilirsiniz. Ayrıca, daha fazla araştırma için Azure Sentinel'deki günlükleri de alabilirsiniz.

Azure Active Directory riskli oturum açma ları nasıl görüntülenebilir:https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik Koruması risk ilkelerinin nasıl yapılandırılabilen ve etkinleştirilir:https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure Sentinel'e nasıl binilir:https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13: Destek senaryoları sırasında Microsoft'a ilgili müşteri verilerine erişim sağlama

**Rehberlik**: Şu anda mevcut değil; Cosmos DB için Azure Veritabanı için henüz desteklenmeyen Müşteri Kilit Kutusu.

Müşteri Lockbox desteklenen hizmetler listesi:https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Geçerli değil

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için [güvenlik denetimi: veri koruma.](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection)*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1: Hassas Bilgilerin envanterini koruyun

**Yönerge**: Hassas bilgileri depolayan veya işleyen Azure Cosmos DB örneklerini izlemeye yardımcı olmak için etiketleri kullanın.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2: Hassas bilgileri depolama veya işleme sistemlerini yalıtma

**Rehberlik**: Geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure Cosmos DB örnekleri sanal ağ/alt ağ tarafından ayrılır, uygun şekilde etiketlenir ve bir ağ güvenlik grubu (NSG) veya Azure Güvenlik Duvarı içinde güvenli hale sunulur. Azure Cosmos DB örnekleri hassas verileri depolamaya yalıtılmalıdır. Azure Özel Bağlantısını kullanarak, özel bir bitiş noktası üzerinden Azure Cosmos DB örnek hesabına bağlanabilirsiniz. Özel bitiş noktası, sanal ağınızdaki bir alt ağdaki özel IP adresleri kümesidir. Daha sonra seçili özel IP adreslerine erişimi sınırlayabilirsiniz. 

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Azure Cosmos DB için Özel Bitiş Noktası nasıl yapılandırılabilen:https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints

Güvenlik Config'i olan bir Ağ Güvenlik Grubu nasıl oluşturulur:https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3: Hassas bilgilerin yetkisiz aktarımının izlenmesi ve engellenmesi

**Kılavuz**: Veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılayan Azure Cosmos DB için Gelişmiş Tehdit Koruması dağıtabilirsiniz. Şu anda aşağıdaki uyarıları tetikleyebilir:

- Sıradışı konumlardan erişim

- Olağandışı veri ayıklama

Ayrıca, Azure Cosmos DB örneklerinize erişmek için sanal makineleri kullanırken, veri sızma olasılığını azaltmak için Özel Bağlantı, Güvenlik Duvarı, ağ güvenlik grupları ve hizmet etiketlerini kullanın. Microsoft, Azure Cosmos DB'nin altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Cosmos DB Gelişmiş Tehdit Koruması nasıl yapılandırılabilen:https://docs.microsoft.com/azure/cosmos-db/cosmos-db-advanced-threat-protection

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4: Tüm hassas bilgileri taşıma sırasında şifreleme

**Kılavuz :** Azure Cosmos DB'ye tüm bağlantılar HTTPS'yi destekler. Azure Cosmos DB tls1.2'yi de destekler. En az TLS sürümü sunucu tarafı zorlamak mümkündür. Bunu yapmak için [azurecosmosdbtls@service.microsoft.com](mailto:azurecosmosdbtls@service.microsoft.com)lütfen .

Cosmos DB Security'ye Genel Bakış:https://docs.microsoft.com/azure/cosmos-db/database-security

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5: Hassas verileri tanımlamak için etkin bir bulma aracı kullanın

**Yönlendirme**: Azure Cosmos DB için otomatik veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz kullanılamıyor. Ancak, sınıflandırma ve veri çözümlemesi için Azure Bilişsel Arama tümleştirmesini kullanabilirsiniz. Uyumluluk amacıyla gerekirse bir üçüncü taraf çözümü de uygulayabilirsiniz.

Microsoft tarafından yönetilen temel platform için Microsoft, tüm müşteri içeriğini hassas olarak ele almakta ve müşteri verilerinin kaybolmasına ve açığa çıkmasına karşı korunmak için büyük çaba sarf eder. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure Bilişsel Arama ile Azure Cosmos https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb?toc=/azure/cosmos-db/toc.json&ampDB verilerini dizinle: ;bc=/azure/cosmos-db/breadcrumb/toc.json

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6: Kaynaklara erişimi denetlemek için Azure RBAC'ı kullanın

**Yönlendirme**: Azure Cosmos DB, Azure Cosmos DB'deki yaygın yönetim senaryoları için yerleşik rol tabanlı erişim denetimi (RBAC) sağlar. Azure Active Directory'de profili olan bir kişi, Azure Cosmos DB kaynaklarındaki kaynaklara ve işlemlere erişim sağlamak veya reddetmek için bu RBAC rollerini kullanıcılara, gruplara, hizmet ilkelerine veya yönetilen kimliklere atayabilir. Rol atamaları, Azure Cosmos hesaplarına, veritabanlarına, kapsayıcılara ve tekliflere (iş ortası) erişimi içeren yalnızca denetim düzlemi erişimine göre kapsamlıdır.

Azure Cosmos DB'de RBAC nasıl uygulanır:https://docs.microsoft.com/azure/cosmos-db/role-based-access-control

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7: Erişim denetimini uygulamak için ana bilgisayar tabanlı veri kaybı önlemeyi kullanın

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

Microsoft, Cosmos DB'nin altında yatan altyapıyı yönetir ve müşteri verilerinin kaybolmasını veya açığa dökülmesini önlemek için sıkı denetimler uygulamıştır.

Azure'da müşteri verilerinin korunmasını anlayın:https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8: Hassas bilgileri istirahatte şifreleme

**Kılavuz**: Cosmos DB'de depolanan tüm kullanıcı verileri varsayılan olarak istirahatte şifrelenir. Kapatmak için hiçbir denetim yok. Azure Cosmos DB, hesabın çalıştığı tüm bölgelerde AES-256 şifrelemesi kullanır.

Varsayılan olarak Microsoft, Azure Cosmos hesabınızdaki verileri şifrelemek için kullanılan anahtarları yönetir. İsteğe bağlı olarak kendi anahtarlarınızla ikinci bir şifreleme katmanı eklemeyi seçebilirsiniz.

Azure Cosmos DB ile şifrelemeyi anlama:https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest

Azure Cosmos DB ile şifreleme için anahtar yönetimini anlama:https://docs.microsoft.com/azure/cosmos-db/cosmos-db-security-controls

Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarlar nasıl yapılandırılabilen:https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9: Kritik Azure kaynaklarında yapılan değişiklikleri kaydedin ve uyarı

**Kılavuz**: Azure Cosmos DB'nin üretim örneklerinde ne zaman değişiklikler yapılacağına dair uyarılar oluşturmak için Azure Etkinlik Günlüğü ile Azure Monitörünü kullanın.

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

Azure Etkinlik Günlüğü etkinlikleri için uyarılar oluşturma:https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için [Güvenlik Denetimi: Güvenlik Açığı Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management)bakın.*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1: Otomatik güvenlik açığı tarama araçlarını çalıştırın

**Yönerge**: Azure Cosmos DB örnekleriniz için Azure Güvenlik Merkezi'nden gelen önerileri izleyin. 

Microsoft, Azure Cosmos DB örneklerinizi destekleyen temel ana bilgisayarlarda sistem düzeltme ve güvenlik açığı yönetimi gerçekleştirir. Microsoft, Azure içindeki müşteri verilerinin güvenli kalmasını sağlamak için sağlam veri koruma denetimleri ve yetenekleri paketi uygulamıştır ve bunları sürdürmektedir.

Azure Güvenlik Merkezi'nde bulunan desteklenen özellikler:https://docs.microsoft.com/azure/security-center/security-center-services?tabs=features-windows

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2: Otomatik işletim sistemi yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5.3: Otomatik üçüncü taraf yazılım yama yönetimi çözümlerini dağıtma

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4: Arka arkaya güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5: Keşfedilen güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için [güvenlik denetimi: Envanter ve Varlık Yönetimi'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management)bakın.*

### <a name="61-use-azure-asset-discovery"></a>6.1: Azure Varlık Bulma'yı kullanma

**Yönlendirme**: Aboneliğinizdeki tüm kaynakları (Azure Cosmos DB ile sınırlı olmamak üzere, aynı zamanda bilgi işlem, diğer depolama, ağ, bağlantı noktaları ve protokoller vb. gibi kaynaklar da dahil olmak üzere) keşfetmek için Azure portalını veya Azure Kaynak Grafiği'ni kullanın.  Kiracınızda uygun izinlere sahip olduğundan ve aboneliklerinizdeki tüm Azure aboneliklerinin yanı sıra kaynakları sıralayabildiğinizden emin olun.

Kaynak Grafiği aracılığıyla klasik Azure kaynakları keşfedilebilir, ancak ileriye dönük Olarak Azure Kaynak Yöneticisi kaynaklarının oluşturulması ve kullanılması önerilir.

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure Aboneliklerinizi görüntüleme:https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure rol tabanlı erişim denetimini anlama:https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="62-maintain-asset-metadata"></a>6.2: Varlık meta verilerini koruma

**Yönerge**: Etiketleri Azure Cosmos DB örneklerinize ve meta verilerle ilgili kaynaklara uygulayarak bunları mantıksal olarak taksonomi olarak düzenleyin.

Etiketler oluşturma ve kullanma:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Hangi Azure Cosmos DB kaynakları etiketleri destekler:https://docs.microsoft.com/azure/azure-resource-manager/management/tag-support#microsoftdocumentdb

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6.3: Yetkisiz Azure kaynaklarını silme

**Yönlendirme**: Azure Cosmos DB kaynakları dahil ancak bunlarla sınırlı olmamak üzere varlıkları düzenlemek ve izlemek için uygun olduğunda etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanteri düzenli olarak uzlaştırın ve yetkisiz kaynakların abonelikten zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma:https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları nasıl oluşturulur:https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler nasıl oluşturulur ve kullanılır:https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6.4: Onaylanan Azure kaynaklarının ve yazılım başlıklarının envanterini koruyun

**Rehberlik**: Geçerli değildir; bu kılavuz, bir bütün olarak hesaplama kaynakları ve Azure için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5: Onaylanmamış Azure kaynakları için izleme

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, abonelik içindeki kaynakları sorgulamak/keşfetmek için Azure Kaynak Grafiği'ni kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma:https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6: Bilgi işlem kaynakları içindeki onaylanmamış yazılım uygulamalarını izleme

**Rehberlik**: Geçerli değildir; bu taban çizgisi hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7: Onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: Geçerli değildir; bu kılavuz, bir bütün olarak hesaplama kaynakları ve Azure için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="68-use-only-approved-applications"></a>6.8: Yalnızca onaylı uygulamaları kullanın

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6.9: Yalnızca onaylanmış Azure hizmetlerini kullanma

**Yönerge**: Aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliğinde oluşturulabilecek kaynak türüne kısıtlamalar getirmek için Azure Politikası'nı kullanın:

- İzin verilmeyen kaynak türleri 

- İzin verilen kaynak türleri

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlkesi ile belirli bir kaynak türü nasıl reddedilecek:https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="610-implement-approved-application-list"></a>6.10: Onaylı başvuru listesini uygulama

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azureresources-manager-via-scripts"></a>6.11: Kullanıcıların komut dosyaları aracılığıyla AzureResources Yöneticisi ile etkileşim deşmelerini sınırlandırın

**Yönerge**: "Microsoft Azure Yönetimi" Uygulaması için "Erişimi engelle" yapılandırmayı yaparak kullanıcıların Azure Kaynak Yöneticisi ile etkileşim edebilmelerini sınırlamak için Azure Koşullu Erişimi kullanın. Bu, yüksek güvenlik ortamında kaynak oluşturulmasını ve kaynaklarda değişiklik olmasını engelleyebilir.

Koşullu Erişim'i Azure Kaynak Yöneticisi'ne erişimi engellemek için yapılandırma:https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12: Kullanıcıların bilgi işlem kaynakları içinde komut dosyalarını yürütme yeteneğini sınırlama

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13: Yüksek riskli uygulamaları fiziksel veya mantıksal olarak ayırmak

**Rehberlik**: Geçerli değildir; Bu kılavuz, Azure Uygulama Hizmeti veya bilgi işlem kaynakları üzerinde çalışan web uygulamaları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için güvenlik [denetimi: Güvenli Yapılandırma'ya](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration)bakın.*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1: Tüm Azure kaynakları için güvenli yapılandırmalar oluşturma

**Yönerge**: Azure İlkesi ile Cosmos DB örnekleriniz için standart güvenlik yapılandırmalarını tanımlayın ve uygulayın. Cosmos DB örneklerinizi denetlemek veya yapılandırmayı zorlamak için özel ilkeler oluşturmak için "Microsoft.DocumentDB" ad alanında Azure İlkesi takma adlarını kullanın. Azure Cosmos DB için yerleşik ilke tanımlarından da yararlanabilirsiniz:

- Cosmos DB Hesapları için Gelişmiş Tehdit Koruması Dağıtma

- Cosmos DB sanal ağ hizmeti bitiş noktası kullanmalıdır

Kullanılabilir Azure İlkesi Diğer Adları nasıl görüntüleyebilirsiniz:https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7.2: Güvenli işletim sistemi yapılandırmaları oluşturma

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3: Güvenli Azure kaynak yapılandırmalarını koruyun

**Yol gösterici**: Azure kaynaklarınız arasında güvenli ayarlar uygulamak için Azure ilkesini [reddet] ve [varsa dağıt] kullanın.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure İlke Efektlerini Anlama:https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4: Güvenli işletim sistemi yapılandırmalarını koruyun

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5: Azure kaynaklarının yapılandırmayı güvenli bir şekilde depolama

**Kılavuz**: Cosmos DB'niz veya ilgili kaynaklarınız için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos'u kullanın.

Azure Depobelgeleri: https://docs.microsoft.com/azure/devops/repos/index?view=azure-devopshttps://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7.6: Özel işletim sistemi görüntülerini güvenli bir şekilde saklayın

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7.7: Sistem yapılandırma yönetim araçlarını dağıtma

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.DocumentDB" ad alanında Azure İlkesi takma adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve ardışık hatlar geliştirin.

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7.8: İşletim sistemleri için sistem yapılandırma yönetim araçlarını dağıtma

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7.9: Azure hizmetleri için otomatik yapılandırma izleme uygulaması

**Yönerge**: Sistem yapılandırmalarını uyarmak, denetlemek ve uygulamak için özel ilkeler oluşturmak için "Microsoft.DocumentDB" ad alanında Azure İlkesi takma adlarını kullanın. Azure Cosmos DB örnekleriniz ve ilgili kaynaklarınız için yapılandırmaları otomatik olarak uygulamak için Azure ilkesi [denetim], [reddet], [varsa dağıt] ve [varsa dağıt] kullanın. 

Azure İlkesi nasıl yapılandırılabilen ve yönetilen:https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10: İşletim sistemleri için otomatik yapılandırma izleme

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7.11: Azure sırlarını güvenli bir şekilde yönetme

**Kılavuz :** Azure Cosmos DB örneklerinize erişmek için Azure Uygulama Hizmeti'nde çalışan Azure sanal makineleri veya web uygulamaları için, Azure Cosmos DB gizli yönetimini basitleştirmek ve güvence altına almak için Yönetilen Hizmet Kimliğini Azure Key Vault ile birlikte kullanın. Key Vault Soft Delete etkin olduğundan emin olun.

Azure Yönetilen Kimliklerle nasıl tümleştirilir:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Anahtar Kasası nasıl oluşturulur:https://docs.microsoft.com/azure/key-vault/quick-create-portal

Key Vault kimlik doğrulaması yönetilen bir kimlikle nasıl sağlayacaksınız:https://docs.microsoft.com/azure/key-vault/managed-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7.12: Kimlikleri güvenli ve otomatik olarak yönetme

**Kılavuz :** Azure Cosmos DB örneklerinize erişmek için Azure Uygulama Hizmeti'nde çalışan Azure sanal makineleri veya web uygulamaları için, Azure Cosmos DB gizli yönetimini basitleştirmek ve güvence altına almak için Yönetilen Hizmet Kimliğini Azure Key Vault ile birlikte kullanın.

Azure Etkin Dizini'nde (AD) otomatik olarak yönetilen bir kimlikle Azure hizmetlerine yönetilen kimlik sağlamak için Yönetilen Kimlikler'i kullanın. Yönetilen Kimlikler, anahtarınızda herhangi bir kimlik belgesi olmadan Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlere kimlik doğrulamanızı sağlar.

Yönetilen Kimlikler nasıl yapılandırılır:https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure Yönetilen Kimliklerle nasıl tümleştirilir:https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13: İstenmeden kimlik bilgisi maruziyetini ortadan kaldırın

**Yönerge**: Kod içindeki kimlik bilgilerini tanımlamak için Kimlik Bilgisi Tarayıcısı'nı uygulayın. Kimlik Bilgisi Tarayıcısı, keşfedilen kimlik bilgilerinin Azure Anahtar Kasası gibi daha güvenli konumlara taşınmasını da teşvik edecektir.

Kimlik Bilgisi Tarayıcınasıl ayarlanır:https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için [Güvenlik Kontrolü: Malware Defense](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense)' e bakın.*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1: Merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımlarını kullanın

**Rehberlik**: Geçerli değildir; bu kılavuz, hesaplama kaynakları için tasarlanmıştır. Microsoft Antimalware, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir (örneğin, Azure Uygulama Hizmeti), ancak müşteri içeriğinde çalışmaz.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2: İşlem yapmayan Azure kaynaklarına yüklenecek dosyaları önceden taraya

**Yönlendirme**: Azure hizmetlerini destekleyen temel ana bilgisayarda (örneğin Azure Uygulama Hizmeti) Microsoft Antimalware etkindir, ancak müşteri içeriğinde çalışmaz.

Azure Cosmos DB de dahil olmak üzere, işlem yapmayan Azure kaynaklarına yüklenen dosyaları önceden tarayabilmek sizin sorumluluğunuzdadır. Microsoft müşteri verilerine erişemez ve bu nedenle sizin adınıza müşteri içeriğinin kötü amaçlı yazılımdan koruma taramalarını gerçekleştiremez.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3: Kötü amaçlı yazılımdan koruma yazılımının ve imzalarının güncelleştirilmesini sağlayın

**Rehberlik**: Geçerli değildir; kıyaslama hesaplama kaynakları için tasarlanmıştır. Microsoft Antimalware, Azure hizmetlerini destekleyen temel ana bilgisayarda etkindir, ancak müşteri içeriğinde çalışmaz.


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1: Düzenli otomatik yedeklemeler sağlayın

**Rehberlik**: Azure Cosmos DB, verilerinizin anlık görüntülerini her dört saatte bir alır. Tüm yedeklemeler bir depolama hizmetinde ayrı olarak depolanır ve bu yedeklemeler bölgesel felaketlere karşı dayanıklılık için küresel olarak çoğaltılır. Herhangi bir zamanda, yalnızca son iki anlık görüntü korunur. Bununla birlikte kapsayıcı veya veritabanı silinirse Azure Cosmos DB bu kapsayıcı veya veritabanının mevcut anlık görüntülerini 30 gün boyunca saklar. Yedeklemeden geri yüklemek için Azure Desteği'ne başvurun.

Azure Cosmos DB Otomatik Yedeklemeleri Anlama:https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Microsoft

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2: Komple sistem yedeklemelerini gerçekleştirin ve müşteri yönetilen anahtarları yedekleme

**Yönlendirme**: Azure Cosmos DB, verilerinizin yedeklerini düzenli aralıklarla otomatik olarak alır. Veritabanı veya kapsayıcı silinirse, otomatik çevrimiçi yedeklemelerden verileri geri yüklemek için bir destek bileti açabilir veya Azure desteğini arayabilirsiniz. Azure desteği yalnızca Standart, Geliştirici gibi seçili planlar ve onlardan daha yüksek planlar için kullanılabilir. Yedeklemenin belirli bir anlık görüntüsünü geri yüklemek için Azure Cosmos DB, verilerin bu anlık görüntü için yedekleme döngüsü süresince kullanılabilir olmasını gerektirir. 

Cosmos DB örnekleriniz için kimlik bilgilerini depolamak için Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

Azure Cosmos DB Otomatik Yedeklemeleri Anlayın:https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore

Azure Cosmos DB'de veri geri yükleme:https://docs.microsoft.com/azure/cosmos-db/how-to-backup-and-restore

Anahtar Vault Tuşları nasıl yedeklenebilir:https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: Paylaşılan

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3: Müşteri yönetilen anahtarlar dahil tüm yedeklemeleri doğrulayın

**Kılavuz**: Veritabanı veya kapsayıcı silinirse, bir destek bileti açabilir veya otomatik çevrimiçi yedeklemelerden verileri geri yüklemek için Azure desteğini arayabilirsiniz. Azure desteği yalnızca Standart, Geliştirici gibi seçili planlar ve onlardan daha yüksek planlar için kullanılabilir. Yedeklemenin belirli bir anlık görüntüsünü geri yüklemek için Azure Cosmos DB, verilerin bu anlık görüntü için yedekleme döngüsü süresince kullanılabilir olmasını gerektirir.

PowerShell'i kullanarak Azure Key Vault'ta depolanan sırların geri yüklemesini test edin. Geri Yükleme-AzureKeyVaultKey cmdlet belirtilen anahtar kasasında bir anahtar oluşturur. Bu anahtar, giriş dosyasındaki yedeklenmiş anahtarın bir kopyasıdır ve özgün anahtarla aynı ada sahiptir.

Azure Cosmos DB Otomatik Yedeklemeleri Anlayın:

https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore

Azure Cosmos DB'de veri geri yükleme:

https://docs.microsoft.com/azure/cosmos-db/how-to-backup-and-restore

Azure Anahtar Kasası Sırları nasıl geri yüklenir:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4: Yedeklerin ve müşteri yönetilen anahtarlarının korunmasını sağlayın

**Kılavuz**: Cosmos DB'de depolanan tüm kullanıcı verileri istirahatte ve taşıma sırasında şifrelendiği için herhangi bir işlem yapmak zorunda değilsiniz. Bunu ifade etmenin başka bir yolu da şifrelemenin varsayılan olarak "açıktır". Kapatmak veya açmak için denetim yok. Azure Cosmos DB, hesabın çalıştığı tüm bölgelerde AES-256 şifrelemesi kullanır.

Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Anahtar Kasası'nda Yumuşak Silme'yi etkinleştirin.

Azure Cosmos DB'de veri şifrelemeyi anlayın:https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest

Anahtar Kasasında Soft-Delete nasıl etkinleştirilir:https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Paylaşılan

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için [Güvenlik Denetimi: Olay Yanıtı'na](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response)bakın.*

### <a name="101-create-an-incident-response-guide"></a>10.1: Olay yanıt kılavuzu oluşturma

**Rehberlik**: Kuruluşunuz için bir olay yanıt kılavuzu oluşturun. Personelin tüm rollerini ve olay işleme/yönetiminin algılamadan olay sonrası incelemeye kadar olan aşamalarını tanımlayan yazılı olay yanıt planları olduğundan emin olun.

Ayrıca, kendi olay yanıt planınızın oluşturulmasına yardımcı olmak için NIST'nin Bilgisayar Güvenliği Olay İşlemleri Kılavuzu'ndan da yararlanabilirsiniz:https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

Azure Güvenlik Merkezi'nde İş Akışı Otomasyonları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Kendi güvenlik olayı yanıt sürecinizi oluşturma kılavuzu:https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi'nin Bir Olayın Anatomisi:https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/


**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2: Olay puanlama ve öncelik lendirme prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, öncelikle hangi uyarıların araştırılması gerektiğine öncelik vermenize yardımcı olmak için her uyarıya önem verir. Önem derecesi, Güvenlik Merkezi'nin bulmakonusunda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analizlere ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır.

Ayrıca, abonelikleri açıkça işaretleyin (eski için. azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="103-test-security-response-procedures"></a>10.3: Test güvenlik yanıt yordamları

**Rehberlik**: Sistemlerinizin olay yanıt yeteneklerini düzenli olarak test etmek için alıştırmalar yapın. Zayıf noktaları ve boşlukları belirleyin ve planı gerektiği gibi gözden geçirin.

NIST'in yayınına bakın: BT Planları ve Yetenekleri için Test, Eğitim ve Egzersiz Programları Kılavuzu:https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4: Güvenlik olayı iletişim bilgilerini sağlayın ve güvenlik olayları için uyarı bildirimlerini yapılandırma

**Kılavuz :** Microsoft Güvenlik Yanıt Merkezi (MSRC), müşterinin verilerine yasadışı veya yetkisiz bir taraf tarafından erişildiğini tespit ederse, güvenlik olayı iletişim bilgileri Microsoft tarafından sizinle iletişim kurmak için kullanılır.  Sorunların çözülmesini sağlamak için olaydan sonraki olayları gözden geçirin.

Azure Güvenlik Merkezi Güvenlik Kişisi nasıl ayarlanır:https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: Müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5: Olay yanıt sisteminize güvenlik uyarıları dahil edin

**Kılavuz**: Sürekli Dışa Aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışa aktarın. Sürekli Dışa Aktarma, uyarı ve önerileri el ile veya sürekli olarak dışa aktarmanıza olanak tanır. Sentinel uyarılarını yayınlamak için Azure Güvenlik Merkezi veri konektörünü kullanabilirsiniz.

Sürekli dışa aktarma nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel'e nasıl aktarın:https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10.6: Güvenlik uyarılarına yanıtı otomatikleştirin

**Yönerge**: Güvenlik uyarıları ve önerileri nde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek için Azure Güvenlik Merkezi'ndeki İş Akışı Otomasyonu özelliğini kullanın.

İş Akışı Otomasyonu ve Mantık Uygulamaları nasıl yapılandırılabilen:https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için [Güvenlik Kontrolü: Penetrasyon Testleri ve Kırmızı Takım Egzersizleri'ne](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises)bakın.*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11.1: Azure kaynaklarınızı düzenli olarak delme testini gerçekleştirin ve tüm kritik güvenlik bulgularının 60 gün içinde düzeltilmesini sağlayın

**Kılavuz**: Penetrasyon Testlerinizin Microsoft ilkelerini ihlal etmediğini sağlamak için Microsoft Etkileşim Kuralları'na uyun:https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft'un Stratejisi ve Red Teaming ve Microsoft tarafından yönetilen bulut altyapısı, hizmetler ve uygulamalara karşı canlı site penetrasyon testi nin uygulanması hakkında daha fazla bilgiyi burada bulabilirsiniz:https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: Uygulanamaz

**Sorumluluk**: Paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Güvenlik Kıyaslama'ya](https://docs.microsoft.com/azure/security/benchmarks/overview) bakın
- [Azure Güvenlik Taban Çizgileri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
