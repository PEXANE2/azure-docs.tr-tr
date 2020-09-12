---
title: Cosmos DB için Azure Güvenlik temeli
description: Cosmos DB için Azure Güvenlik temeli
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 051f6e6ef15cc94e2b607c74e666bae9c91c1160
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89401631"
---
# <a name="azure-security-baseline-for-cosmos-db"></a>Cosmos DB için Azure Güvenlik temeli

Cosmos DB için Azure Güvenlik temeli, dağıtımınızın güvenlik duruşunu artırmanıza yardımcı olacak öneriler içerir.

Bu hizmetin taban çizgisi, Azure [güvenlik kıyaslama sürümü 1,0](https://docs.microsoft.com/azure/security/benchmarks/overview)' dan çizilir ve bu, en iyi yöntemler kılavuzumuzdan Azure 'da bulut çözümlerinizi nasıl güvence altına almak için öneriler sağlar.

Daha fazla bilgi için bkz. [Azure güvenlik temelleri 'ne genel bakış](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Ağ Güvenliği

*Daha fazla bilgi için bkz. [güvenlik denetimi: ağ güvenliği](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-resources-using-network-security-groups-or-azure-firewall-on-your-virtual-network"></a>1,1: sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruyun

**Kılavuz**: Azure özel bağlantısını kullanarak, bir Azure Cosmos hesabına özel bir uç nokta aracılığıyla bağlanabilirsiniz. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Ayrıca, bir ağ güvenlik grubu (NSG) üzerinde sıkı bir giden kuralları kümesi yapılandırarak ve bu NSG 'yi alt ağınızla ilişkilendirerek veri sızdırma riskini azaltabilirsiniz.

Hizmet uç noktalarını Azure Cosmos hesabınızın güvenliğini sağlamak için de kullanabilirsiniz. Bir hizmet uç noktası etkinleştirerek Azure Cosmos hesaplarını yalnızca belirli bir Azure sanal ağının alt ağından erişime izin verecek şekilde yapılandırabilirsiniz. Azure Cosmos DB hizmeti uç noktası etkinleştirildikten sonra, bir Azure Cosmos hesabına erişimi, bir sanal ağdaki bir alt ağdan gelen bağlantılarla sınırlayabilirsiniz.

Ayrıca, IP güvenlik duvarlarını kullanarak Azure Cosmos hesabınızda depolanan verilerin güvenliğini sağlayabilirsiniz. Azure Cosmos DB, gelen güvenlik duvarı desteği için IP tabanlı erişim denetimlerini destekler. Azure portal, Azure Resource Manager şablonlarını veya Azure CLı veya Azure PowerShell aracılığıyla Azure Cosmos hesabında bir IP güvenlik duvarı ayarlayabilirsiniz.

Azure özel bağlantısına genel bakış: https://docs.microsoft.com/azure/private-link/private-link-overview

Azure Cosmos DB için özel uç nokta yapılandırma:  https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints 

Güvenlik Yapılandırması ile ağ güvenlik grubu oluşturma:  https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

Cosmos DB 'de IP güvenlik duvarını yapılandırma: https://docs.microsoft.com/azure/cosmos-db/how-to-configure-firewall

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-vnets-subnets-and-nics"></a>1,2: VNet, alt ağlar ve NIC 'lerin yapılandırmasını ve trafiğini izleyin ve günlüğe kaydedin

**Kılavuz**: Azure Güvenlik Merkezi 'ni kullanın ve Azure Cosmos hesabınızla ilgili ağ kaynaklarının güvenliğini sağlamaya yardımcı olmak için ağ koruma önerilerini izleyin.

Sanal makineler Azure Cosmos hesabınızla aynı sanal ağa dağıtıldığında, veri ayıklanmasının riskini azaltmak için bir ağ güvenlik grubu (NSG) kullanabilirsiniz. NSG akış günlüklerini etkinleştirin ve trafik denetimleri için günlükleri Azure depolama hesabına gönderin. Ayrıca, NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

Azure Güvenlik Merkezi tarafından sunulan ağ güvenliğini anlayın: https://docs.microsoft.com/azure/security-center/security-center-network-recommendations

NSG akış günlüklerini etkinleştirme: https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi etkinleştirme ve kullanma: https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="13-protect-critical-web-applications"></a>1,3: kritik Web uygulamalarını koruma

**Rehberlik**: bir etki alanı altında çalışan bir Web uygulamasının başka bir etki alanındaki kaynaklara erişmesine olanak tanımak Için, çıkış noktaları arası kaynak PAYLAŞıMı (CORS) özelliğini kullanın. Web tarayıcıları, bir Web sayfasının farklı bir etki alanındaki API 'Leri aramasını önleyen aynı kaynak ilkesi olarak bilinen bir güvenlik kısıtlaması uygular. Ancak CORS, kaynak etki alanının başka bir etki alanındaki API 'Leri çağırmasını sağlamak için güvenli bir yol sağlar. Azure Cosmos hesabınız için CORS desteğini etkinleştirdikten sonra, belirttiğiniz kurallara göre izin verilip verilmeyeceğini belirlemede yalnızca kimliği doğrulanmış istekler değerlendirilir.

Çıkış noktaları arası kaynak paylaşımını yapılandırma: https://docs.microsoft.com/azure/cosmos-db/how-to-configure-cross-origin-resource-sharing

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: bilinen kötü amaçlı IP adresleriyle iletişimleri reddetme

**Rehberlik**: Azure Cosmos DB Için Gelişmiş tehdit KORUMASı (ATP) kullanın. Azure Cosmos DB için ATP, Azure Cosmos hesaplarına erişmek veya bu hesaplara yararlanmak için olağandışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, tehditleri ele almanıza ve bunları Merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır.

DDoS saldırılarına karşı koruma sağlamak için Azure Cosmos DB örneklarınızla ilişkili sanal ağlarda DDoS koruma standardını etkinleştirin. Bilinen kötü amaçlı veya kullanılmayan Internet IP adresleriyle iletişimleri reddetmek için Azure Güvenlik Merkezi tümleşik tehdit zekasını kullanın.

Gelişmiş tehdit koruması Azure Cosmos DB nasıl yapılandırılır: https://docs.microsoft.com/azure/cosmos-db/cosmos-db-advanced-threat-protection

DDoS korumasını yapılandırma: https://docs.microsoft.com/azure/virtual-network/manage-ddos-protection

Azure Güvenlik Merkezi tümleşik tehdit zekasını anlama: https://docs.microsoft.com/azure/security-center/security-center-alerts-service-layer

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="15-record-network-packets-and-flow-logs"></a>1,5: ağ paketlerini ve akış günlüklerini kaydetme

**Rehberlik**: trafik denetimi için ağ güvenlik grubu (NSG) akış günlüklerini etkinleştirin ve günlükleri bir depolama hesabına gönderin. NSG akış günlüklerini bir Log Analytics çalışma alanına gönderebilir ve Azure bulutunuzda trafik akışına Öngörüler sağlamak için Trafik Analizi kullanabilirsiniz. Trafik Analizi avantajlarından bazıları, ağ etkinliğini görselleştirme ve etkin noktaları belirlemek, güvenlik tehditlerini belirlemek, trafik akışı düzenlerini anlamak ve ağ yapılandırmalarını saptamak için kullanılır.

NSG akış günlüklerini etkinleştirme: https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-portal

Trafik Analizi etkinleştirme ve kullanma: https://docs.microsoft.com/azure/network-watcher/traffic-analytics

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: ağ tabanlı yetkisiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

**Rehberlik**: Azure Cosmos DB Için Gelişmiş tehdit KORUMASı (ATP) kullanın. Azure Cosmos DB için ATP, Azure Cosmos hesaplarına erişmek veya bu hesaplara yararlanmak için olağandışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, tehditleri ele almanıza ve bunları Merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır. 

Gelişmiş tehdit koruması Cosmos DB nasıl yapılandırılır: https://docs.microsoft.com/azure/cosmos-db/cosmos-db-advanced-threat-protection

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="17-manage-traffic-to-web-applications"></a>1,7: Web uygulamalarına trafiği yönetme

**Rehberlik**: uygulanamaz; öneri, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: ağ güvenlik kurallarının karmaşıklığını ve yönetim yükünü en aza indirme

**Kılavuz**: Azure Cosmos hesabınıza erişmesi gereken kaynaklar için, ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak üzere sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun kaynak veya hedef alanındaki hizmet etiketi adını (ör. Azuversmosdb) belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Hizmet etiketlerini kullanma hakkında daha fazla bilgi için: https://docs.microsoft.com/azure/virtual-network/service-tags-overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: ağ cihazları için standart güvenlik yapılandırmalarının bakımını yapma

**Kılavuz**: Azure ilkesiyle ağ kaynakları için standart güvenlik yapılandırması tanımlayın ve uygulayın. Azure Cosmos DB örneklerinizin ağ yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft.DocumentDB" ve "Microsoft. Network" ad alanlarında Azure Ilke diğer adlarını kullanın. Ayrıca, Azure Cosmos DB için yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:

- Cosmos DB hesapları için Gelişmiş tehdit koruması dağıtma

- Cosmos DB bir sanal ağ hizmeti uç noktası kullanmalıdır

Ayrıca, Azure Resource Manager şablonları, rol tabanlı erişim denetimi (RBAC) ve tek bir şema tanımında ilkeler gibi anahtar ortam yapıtlarını paketleyerek büyük ölçekli Azure dağıtımlarını basitleştirmek için Azure şemaları 'nı kullanabilirsiniz. Yeni aboneliklere, ortamlara kolayca şema uygulayabilir ve sürüm oluşturma aracılığıyla denetim ve yönetime yönetim sağlayabilirsiniz.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Blueprint oluşturma: https://docs.microsoft.com/azure/governance/blueprints/create-blueprint-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="110-document-traffic-configuration-rules"></a>1,10: belge trafiği yapılandırma kuralları

**Rehberlik**: bir taksonomiyle mantıksal olarak organize etmek için Azure Cosmos DB dağıtımınız ile ilişkili ağ kaynakları için Etiketler kullanın.

Etiketler oluşturma ve kullanma: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: ağ kaynağı yapılandırmasını izlemek ve değişiklikleri algılamak için otomatikleştirilmiş araçları kullanın

**Kılavuz**: Azure etkinlik günlüğü 'nü kullanarak ağ kaynak yapılandırmasını izleyin ve Azure Cosmos DB örneklerinizin ilişkili olduğu ağ kaynaklarına yönelik değişiklikleri tespit edin. Kritik ağ kaynaklarında yapılan değişiklikler yürürlüğe girdiğinde tetiklenecek Azure Izleyici içinde uyarılar oluşturun. 

Azure etkinlik günlüğü olaylarını görüntüleme ve alma: https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view

Azure Izleyici 'de uyarı oluşturma: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

*Daha fazla bilgi için bkz. [güvenlik denetimi: günlüğe kaydetme ve izleme](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: onaylanan zaman eşitleme kaynaklarını kullanın

**Rehberlik**: Microsoft, günlüklerde zaman damgalarına yönelik Azure Cosmos DB gibi Azure kaynakları için kullanılan saat kaynağını korur.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="22-configure-central-security-log-management"></a>2,2: Merkezi güvenlik günlüğü yönetimini yapılandırma

**Rehberlik**: Azure Cosmos DB tarafından oluşturulan güvenlik verilerini toplamak Için Azure izleyici aracılığıyla günlükleri alma. Azure Izleyici 'de, Log Analytics çalışma alanı (ler) kullanarak Analizi sorgulayın ve gerçekleştirin ve uzun süreli/arşiv depolama için depolama hesapları kullanın. Alternatif olarak, Azure Sentinel veya bir üçüncü taraf güvenlik olayı ve olay yönetimi (SıEM) üzerinde verileri etkinleştirebilir ve bu verilere izin verebilir. 

Azure Cosmos DB için tanılama günlüklerini etkinleştirme: https://docs.microsoft.com/azure/cosmos-db/logging

Azure Sentinel 'i ekleme: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Azure kaynakları için denetim günlüğünü etkinleştirme

**Rehberlik**: Azure Cosmos DB için tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına veya depolama hesabına gönderin. Azure Cosmos DB Tanılama ayarları, kaynak günlüklerini toplamak için kullanılır. Bu Günlükler istek başına yakalanır ve ayrıca "veri düzlemi günlükleri" olarak anırlar. Veri düzlemi işlemlerine bazı örnekler silme, ekleme ve okumayı içerir. Ayrıca, Azure etkinlik günlüğü tanılama ayarlarını da etkinleştirebilir ve aynı Log Analytics çalışma alanına gönderebilirsiniz.

Azure Cosmos DB için tanılama ayarlarını etkinleştirme: https://docs.microsoft.com/azure/cosmos-db/logging

Azure etkinlik günlüğü için tanılama ayarlarını etkinleştirme: https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-settings-legacy

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: işletim sistemlerinden güvenlik günlüklerini toplama

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="25-configure-security-log-storage-retention"></a>2,5: güvenlik günlüğü depolama bekletmesini yapılandırma

**Kılavuz**: Azure izleyici 'de, kuruluşunuzun uyumluluk düzenlemelerine göre Azure Cosmos DB örneklerinize ilişkin Log Analytics çalışma alanları için günlük tutma süresini ayarlayın.

Günlük tutma parametrelerini ayarlama: https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="26-monitor-and-review-logs"></a>2,6: günlükleri izleme ve gözden geçirme

**Rehberlik**: bir çalışma alanında Log Analytics sorguları, terimleri aramak, eğilimleri belirlemek, desenleri analiz etmek ve topladığınız Azure Cosmos DB günlüklere göre birçok öngörü sağlamak için yapabilirsiniz.

Log Analytics çalışma alanlarında Azure Cosmos DB sorguları gerçekleştirme: https://docs.microsoft.com/azure/cosmos-db/monitor-cosmos-db

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="27-enable-alerts-for-anomalous-activity"></a>2,7: anormal etkinlik için uyarıları etkinleştir

**Kılavuz**: Azure Güvenlik Merkezi 'nde güvenlik günlüklerinde ve olaylarda anormal etkinlikleri izlemek üzere Azure Cosmos DB Için Gelişmiş tehdit koruması 'nı etkinleştirin. Azure Cosmos DB tanılama ayarlarını etkinleştirin ve günlükleri bir Log Analytics çalışma alanına gönderin.

 

Ayrıca, bir güvenlik Orchestration otomatik yanıtı (SOAR) çözümü sağladığından Log Analytics çalışma alanınızı Azure Sentinel 'e ekleyebilirsiniz. Bu, güvenlik sorunlarını gidermek için PlayBook 'ları (otomatikleştirilmiş çözümlerin) oluşturulmasına ve kullanılmasına olanak tanır. Ayrıca, Azure Izleyici 'yi kullanarak Log Analytics çalışma alanınızda özel günlük uyarıları da oluşturabilirsiniz.

Azure Cosmos DB için tehdit koruması uyarıları listesi: https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurecosmos

Azure Sentinel 'i ekleme: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

Azure Izleyici 'yi kullanarak günlük uyarıları oluşturun, görüntüleyin ve yönetin: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="28-centralize-anti-malware-logging"></a>2,8: kötü amaçlı yazılımdan koruma 'yı merkezileştirme

**Rehberlik**: uygulanamaz; Azure Cosmos DB kötü amaçlı yazılımdan koruma ile ilgili günlükleri işlemez veya oluşturmaz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="29-enable-dns-query-logging"></a>2,9: DNS sorgu günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Azure Cosmos DB, DNS ile ilgili günlükleri işlemez veya oluşturmaz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="210-enable-command-line-audit-logging"></a>2,10: komut satırı denetim günlüğünü etkinleştir

**Rehberlik**: uygulanamaz; Bu öneri, işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="identity-and-access-control"></a>Kimlik ve Erişim Denetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: kimlik ve Access Control](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: yönetim hesaplarının envanterini tutma

**Rehberlik**: rol tabanlı erişim denetimi 'NI (RBAC) yapılandırmak ve Azure Cosmos DB kaynaklarında stoku sürdürmek Için Azure Portal kimlik ve erişim denetımı (IAM) bölmesini kullanabilirsiniz. Roller, kullanıcılar, gruplar, hizmet sorumluları ve Active Directory içindeki yönetilen kimliklere uygulanır. Bireyler ve gruplar için yerleşik roller veya özel roller kullanabilirsiniz.

Azure Cosmos DB, Azure Cosmos DB ortak yönetim senaryoları için yerleşik RBAC sağlar. Azure Active Directory (AD) içinde bir profili olan bir kişi, Azure Cosmos DB kaynaklardaki kaynaklara ve işlemlere erişim vermek veya erişimi reddetmek için bu Azure rollerini kullanıcılara, gruplara, hizmet sorumlularına veya yönetilen kimliklere atayabilir.

Ayrıca, yönetim gruplarının üyesi olan hesapları bulmaya yönelik geçici sorgular gerçekleştirmek için Azure AD PowerShell modülünü de kullanabilirsiniz. 

Ayrıca, Azure Cosmos DB bazı eylemler Azure Active Directory ve hesaba özgü ana anahtarlarla denetlenebilir.  Anahtar erişimini denetlemek için ' disableKeyBasedMetadataWriteAccess ' hesap ayarını kullanın.

Azure Cosmos DB içinde rol tabanlı erişim denetimini anlayın: https://docs.microsoft.com/azure/cosmos-db/role-based-access-control

Azure Cosmos DB eylemleri (Microsoft.DocumentDB ad alanı) kullanarak kendi özel rollerinizi oluşturun: https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations#microsoftdocumentdb

Azure Active Directory yeni bir rol oluşturun: https://docs.microsoft.com/azure/role-based-access-control/custom-roles

PowerShell ile Azure Active Directory bir dizin rolü alma: https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0

PowerShell ile Azure Active Directory bir dizin rolünün üyelerini alma: https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0

Yalnızca veri işlemlerine Kullanıcı erişimini kısıtla: https://docs.microsoft.com/azure/cosmos-db/how-to-restrict-user-data

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="32-change-default-passwords-where-applicable"></a>3,2: uygun yerlerde varsayılan parolaları değiştirme

**Kılavuz**: Azure AD veya Azure Cosmos DB ilişkili olarak varsayılan veya boş parola kavramı yoktur. Bunun yerine, Azure Cosmos DB kullanıcıların kimliğini doğrulamak ve veri ve kaynaklarına erişim sağlamak için iki tür anahtar kullanır; Ana anahtarlar ve kaynak belirteçleri. Anahtarlar dilediğiniz zaman yeniden oluşturulabilir.

Azure Cosmos DB verilere güvenli erişimi anlama: https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data

Azure Cosmos DB anahtarlarını yeniden oluşturma: https://docs.microsoft.com/azure/cosmos-db/manage-with-powershell#regenerate-keys

Azure Active Directory kullanarak tuşlara programlı olarak erişme: https://docs.microsoft.com/azure/cosmos-db/certificate-based-authentication

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: adanmış yönetim hesapları kullanın

**Rehberlik**: uygulanamaz; Azure Cosmos DB, yönetici hesaplarını desteklemez.  Tüm erişim Azure Active Directory ve Azure rol tabanlı erişim denetimi (Azure RBAC) ile tümleşiktir.



**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Azure Active Directory ile çoklu oturum açma (SSO) kullanın

**Rehberlik**: Azure Cosmos DB kullanıcılara yetki vermek için iki tür anahtar kullanır ve veri düzlemi düzeyinde çoklu oturum AÇMAYı (SSO) desteklemez. Cosmos DB için denetim düzlemine erişim REST API aracılığıyla kullanılabilir ve SSO 'yu destekler. Kimlik doğrulaması yapmak için isteklerinizin yetkilendirme üst bilgisini Azure Active Directory aldığınız JSON Web Token ayarlayın.

Cosmos DB REST API için Azure veritabanı 'nı anlayın: https://docs.microsoft.com/rest/api/cosmos-db/

SSO 'yu Azure Active Directory anlayın: https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: tüm Azure Active Directory tabanlı erişim için Multi-Factor Authentication kullanın

**Rehberlik**: Azure Active Directory Multi-Factor Authentication etkinleştirin ve Azure Güvenlik Merkezi kimlik ve erişim yönetimi önerilerini izleyin.

Azure 'da MFA 'yı etkinleştirme: https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

Azure Güvenlik Merkezi 'nde kimliği ve erişimi izleme: https://docs.microsoft.com/azure/security-center/security-center-identity-access

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: tüm yönetim görevleri için adanmış makineler (ayrıcalıklı erişim Iş Istasyonları) kullanın

**Kılavuz**: Azure kaynaklarını açmak ve yapılandırmak için yapılandırılmış Multi-Factor Authentication ayrıcalıklı erişim iş istasyonları (Paw) kullanın.

Ayrıcalıklı erişim Iş Istasyonları hakkında bilgi edinin: https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations

Azure 'da MFA 'yı etkinleştirme: https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="37-log-and-alert-on-suspicious-activity-from-administrative-accounts"></a>3,7: yönetim hesaplarından şüpheli etkinlikte günlüğe kaydet ve uyar

**Rehberlik**: Azure Cosmos DB Için Gelişmiş tehdit KORUMASı (ATP) kullanın. Azure Cosmos DB için ATP, Azure Cosmos hesaplarına erişmek veya bu hesaplara yararlanmak için olağandışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, tehditleri ele almanıza ve bunları Merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır. 

Ayrıca, ortamda şüpheli veya güvenli olmayan bir etkinlik olduğunda Günlükler ve uyarılar oluşturmak için Azure Active Directory (AD) Privileged Identity Management (PıM) kullanabilirsiniz.

Riskli Kullanıcı davranışında uyarıları ve raporları görüntülemek için Azure AD risk algılamalarını kullanın.

Privileged Identity Management dağıtma (PıM): https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan

Azure AD risk algılamalarını anlama: https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: Azure kaynaklarını yalnızca onaylanan konumlardan yönetme

**Rehberlik**: koşullu erişim ilkesinin konum koşulunu yapılandırın ve adlandırılmış konumlarınızı yönetin. Adlandırılmış konumlarla, IP adresi aralıkları veya ülkeler ve bölgeler için mantıksal gruplandırmaları oluşturabilirsiniz. Azure Cosmos DB örneklerinizin gibi hassas kaynaklarla erişimi, yapılandırılmış adlandırılmış konumlara kısıtlayabilirsiniz.

Azure 'da adlandırılmış konumlar nasıl yapılandırılır: https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="39-use-azure-active-directory"></a>3,9: Azure Active Directory kullanın

**Rehberlik**: merkezi kimlik doğrulama ve yetkilendirme sistemi olarak Azure ACTIVE DIRECTORY (ad) kullanın. Azure AD, bekleyen ve aktarım sırasında veriler için güçlü şifrelemeyi kullanarak verileri korur. Azure AD Ayrıca, karma ve Kullanıcı kimlik bilgilerini güvenli bir şekilde depolar.

Azure Active Directory örneği oluşturma ve yapılandırma: https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance

Azure SQL ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme: https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: Kullanıcı erişimini düzenli olarak gözden geçirin ve karşılaştırın

**Rehberlik**: Azure Active Directory eski hesapların keşfedilmesine yardımcı olmak için Günlükler sağlar. Ayrıca, grup üyeliklerini verimli bir şekilde yönetmek için Azure kimlik erişimi Incelemelerini, kurumsal uygulamalara erişimi ve rol atamalarını de kullanabilirsiniz. Kullanıcının erişimi, yalnızca doğru kullanıcıların erişmeye devam ettiğinden emin olmak için düzenli olarak incelenebilir.

Azure kimlik erişimi Incelemelerini kullanma: https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="311-monitor-attempts-to-access-deactivated-accounts"></a>3,11: devre dışı bırakılmış hesaplara erişme girişimlerini izleme

**Rehberlik**: Azure Active Directory Kullanıcı hesapları için Tanılama ayarları oluşturabilir, Denetim günlüklerini ve oturum açma günlüklerini, istenen uyarıları yapılandırabileceğiniz bir Log Analytics çalışma alanına gönderebilirsiniz.

Azure etkinlik günlüklerini Azure Izleyici ile tümleştirme: https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: müşteri

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: hesap oturum açma davranışı sapmasından uyar

**Rehberlik**: Azure Cosmos DB Için Gelişmiş tehdit KORUMASı (ATP) kullanın. Azure Cosmos DB için ATP, Azure Cosmos hesaplarına erişmek veya bu hesaplara yararlanmak için olağandışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, tehditleri ele almanıza ve bunları Merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır. 

Ayrıca, Kullanıcı kimlikleriyle ilgili şüpheli eylemleri algılanan otomatik yanıtları yapılandırmak için Azure AD Kimlik Koruması ve risk algılamaları özelliğini de kullanabilirsiniz. Ayrıca, daha fazla araştırma için günlükleri Azure Sentinel 'e aktarabilirsiniz.

Azure Active Directory riskli oturum açma işlemlerini görüntüleme: https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins

Kimlik koruması risk ilkelerini yapılandırma ve etkinleştirme: https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies

Azure Sentinel 'i ekleme: https://docs.microsoft.com/azure/sentinel/quickstart-onboard

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: destek senaryoları sırasında Microsoft 'un ilgili müşteri verilerine erişimini sağlama

**Rehberlik**: Şu anda kullanılamıyor; Müşteri Kasası Cosmos DB için Azure veritabanı için henüz desteklenmiyor.

Desteklenen Müşteri Kasası hizmetleri listesi: https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: geçerli değil

## <a name="data-protection"></a>Veri Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: hassas bilgilerin envanterini tutma

**Rehberlik**: hassas bilgileri depolayan veya işleyen Azure Cosmos DB örnekleri izlemeye yardımcı olması için Etiketler kullanın.

Etiketler oluşturma ve kullanma: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: hassas bilgileri depolayan veya işleyen sistemleri yalıtma

**Rehberlik**: geliştirme, test ve üretim için ayrı abonelikler ve/veya yönetim grupları uygulayın. Azure Cosmos DB örnekleri, sanal ağ/alt ağ tarafından ayrılır, uygun şekilde etiketlenebilir ve bir ağ güvenlik grubu (NSG) veya Azure Güvenlik duvarı içinde güvenli hale getirilir. Hassas verileri depolayan Azure Cosmos DB örneklerin yalıtılmalıdır. Azure özel bağlantısı ' nı kullanarak, bir Azure Cosmos DB örneği hesabına özel bir uç nokta aracılığıyla bağlanabilirsiniz. Özel uç nokta, sanal ağınız içindeki bir alt ağda bulunan özel IP adresleri kümesidir. Ardından, seçilen özel IP adreslerine erişimi sınırlayabilirsiniz. 

Ek Azure abonelikleri oluşturma: https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim grupları oluşturma: https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Azure Cosmos DB için özel uç nokta yapılandırma: https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints

Güvenlik Yapılandırması ile ağ güvenlik grubu oluşturma: https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: hassas bilgilerin yetkisiz aktarımını izleme ve engelleme

**Rehberlik**: Azure Cosmos DB Için Gelişmiş tehdit koruması dağıtabilirsiniz ve bu, veritabanlarına erişmek veya veritabanına yararlanma amacıyla olağan dışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tespit eder. Şu anda şu uyarıları tetikleyebilirler:

- Olağan dışı konumlardan erişim

- Olağan dışı veri ayıklama

Ayrıca, Azure Cosmos DB örneklerinizi erişmek için sanal makineler kullanılırken, veri kaybı olasılığını azaltmak için özel bağlantı, güvenlik duvarı, ağ güvenlik grupları ve hizmet etiketleri kullanın. Microsoft, Azure Cosmos DB için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

Gelişmiş tehdit koruması Cosmos DB nasıl yapılandırılır: https://docs.microsoft.com/azure/cosmos-db/cosmos-db-advanced-threat-protection

Azure 'da müşteri veri korumasını anlayın: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: yoldaki tüm hassas bilgileri şifreleyin

**Rehberlik**: Azure Cosmos DB tüm bağlantıları https 'yi destekler. Azure Cosmos DB ayrıca TLS 1.2 'yi destekler. En düşük TLS sürümü sunucu tarafını zorlamak mümkündür. Bunu yapmak için lütfen iletişim kurun [azurecosmosdbtls@service.microsoft.com](mailto:azurecosmosdbtls@service.microsoft.com) .

Cosmos DB güvenliğine genel bakış: https://docs.microsoft.com/azure/cosmos-db/database-security

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: hassas verileri belirlemek için etkin bir keşif aracı kullanın

**Rehberlik**: otomatik veri tanımlama, sınıflandırma ve kayıp önleme özellikleri henüz Azure Cosmos DB için kullanılamaz. Ancak, sınıflandırma ve veri analizi için Azure Bilişsel Arama tümleştirmesini kullanabilirsiniz. Uyumluluk amaçlarıyla gerekirse bir üçüncü taraf çözümü de uygulayabilirsiniz.

Microsoft tarafından yönetilen temel alınan platform için, Microsoft tüm müşteri içeriklerini gizli olarak değerlendirir ve müşteri veri kaybına ve açığa çıkmasına karşı koruma sağlamak için harika uzunluklara gider. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure Bilişsel Arama ile dizin Azure Cosmos DB verileri: https://docs.microsoft.com/azure/search/search-howto-index-cosmosdb?toc=/azure/cosmos-db/toc.json&amp ; BC =/azure/cosmos-db/breadcrumb/toc.js

Azure 'da müşteri veri korumasını anlayın: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: kaynaklara erişimi denetlemek için Azure RBAC kullanma

**Rehberlik**: Azure Cosmos DB Azure Cosmos DB ortak yönetim senaryoları için yerleşik rol tabanlı erişim denetımı (RBAC) sağlar. Azure Active Directory bir profili olan bir kişi, Azure Cosmos DB kaynaklardaki kaynaklara ve işlemlere erişim vermek veya erişimi reddetmek için bu Azure rollerini kullanıcılara, gruplara, hizmet sorumlularına veya yönetilen kimliklere atayabilir. Rol atamaları yalnızca, Azure Cosmos hesaplarına, veritabanlarına, kapsayıcılarına ve tekliflere (verimlilik) erişimi de içeren denetim düzlemi erişimini kapsar.

Azure Cosmos DB ' de RBAC 'yi uygulama: https://docs.microsoft.com/azure/cosmos-db/role-based-access-control

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: erişim denetimini zorlamak için ana bilgisayar tabanlı veri kaybı önleme kullanın

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

Microsoft, Cosmos DB için temel altyapıyı yönetir ve müşteri verilerinin kaybını veya açıklanmasını engellemek için katı denetimler uygulamıştır.

Azure 'da müşteri veri korumasını anlayın: https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: hassas bilgileri Rest 'te şifreleyin

**Rehberlik**: Cosmos DB depolanan tüm Kullanıcı verileri varsayılan olarak geri kalanında şifrelenir. Kapatılacak denetim yok. Azure Cosmos DB, hesabın çalıştığı tüm bölgelerde AES-256 şifrelemesini kullanır.

Varsayılan olarak, Microsoft, Azure Cosmos hesabınızdaki verileri şifrelemek için kullanılan anahtarları yönetir. İsteğe bağlı olarak, kendi anahtarlarınız ile ikinci bir şifreleme katmanı eklemeyi seçebilirsiniz.

Azure Cosmos DB ile bekleyen şifrelemeyi anlama: https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest

Azure Cosmos DB ile bekleyen şifreleme için anahtar yönetimini anlama: https://docs.microsoft.com/azure/cosmos-db/cosmos-db-security-controls

Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandırma: https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: kritik Azure kaynaklarında yapılan değişikliklerle ilgili günlük ve uyarı

**Kılavuz**: Azure Cosmos DB üretim örneklerinde değişiklik gerçekleşirken uyarı oluşturmak Için Azure etkinlik günlüğü Ile Azure izleyici 'yi kullanın.

Azure etkinlik günlüğü olayları için uyarı oluşturma: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

Azure etkinlik günlüğü olayları için uyarı oluşturma: https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="vulnerability-management"></a>Güvenlik Açığı Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenlik açığı yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: otomatikleştirilmiş güvenlik açığı tarama araçlarını çalıştırma

**Rehberlik**: Azure Cosmos DB örneklerinizin Azure Güvenlik Merkezi önerilerini izleyin. 

Microsoft, Azure Cosmos DB örneklerinizi destekleyen temel ana bilgisayarlarda sistem düzeltme eki ve güvenlik açığı yönetimi gerçekleştirir. Azure 'daki müşteri verilerinin güvende kalmasını sağlamak için Microsoft, bir dizi güçlü veri koruma denetimi ve özelliği uygulamıştır ve bakımını yapar.

Azure Güvenlik Merkezi 'nde sunulan desteklenen özellikler: https://docs.microsoft.com/azure/security-center/security-center-services?tabs=features-windows

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: otomatik işletim sistemi düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="53-deploy-automated-third-party-software-patch-management-solution"></a>5,3: otomatik üçüncü taraf yazılım düzeltme eki yönetimi çözümünü dağıtma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: geri dönüş güvenlik açığı taramalarını karşılaştırın

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: bulunan güvenlik açıklarının düzeltilmesine öncelik vermek için risk derecelendirme işlemi kullanın

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="inventory-and-asset-management"></a>Envanter ve Varlık Yönetimi

*Daha fazla bilgi için bkz. [güvenlik denetimi: envanter ve varlık yönetimi](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-azure-asset-discovery"></a>6,1: Azure varlık bulmayı kullanma

**Rehberlik**: abonelikleriniz içinde tüm kaynakları (Azure Cosmos DB sınırlı olmamak üzere, ancak işlem, diğer depolama, ağ, bağlantı noktaları ve protokoller vb.) öğrenmek için Azure Portal veya Azure Kaynak grafiğini kullanın.  Kiracınızda uygun izinlere sahip olduğunuzdan ve aboneliklerinizin içindeki kaynakların yanı sıra tüm Azure aboneliklerinin listesini belirleyebildiğinizden emin olun.

Klasik Azure kaynakları kaynak Graph aracılığıyla bulunabilse de, ileri doğru Azure Resource Manager kaynak oluşturmanız ve kullanılması kesinlikle önerilir.

Azure Kaynak Graf ile sorgu oluşturma: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

Azure aboneliklerinizi görüntüleme: https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0

Azure rol tabanlı erişim denetimini anlama: https://docs.microsoft.com/azure/role-based-access-control/overview

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="62-maintain-asset-metadata"></a>6,2: varlık meta verilerini koruma

**Rehberlik**: Azure Cosmos DB örneklerinizin ve ilgili kaynaklarınızın etiketlerini, mantıksal olarak bir taksonomi halinde düzenlemek için meta verilerle uygulayın.

Etiketler oluşturma ve kullanma: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

Hangi Azure Cosmos DB kaynakları etiketleri destekler: https://docs.microsoft.com/azure/azure-resource-manager/management/tag-support#microsoftdocumentdb

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: yetkisiz Azure kaynaklarını silme

**Rehberlik**: Azure Cosmos DB kaynaklarıyla sınırlı olmamak üzere, varlıkları düzenlemek ve izlemek için etiketleme, yönetim grupları ve ayrı abonelikler kullanın. Envanterin düzenli olarak mutabakatını yapın ve yetkisiz kaynakların aboneliğin zamanında silindiğinden emin olun.

Ek Azure abonelikleri oluşturma: https://docs.microsoft.com/azure/billing/billing-create-subscription

Yönetim Grupları oluşturma: https://docs.microsoft.com/azure/governance/management-groups/create

Etiketler oluşturma ve kullanma: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="64-maintain-an-inventory-of-approved-azure-resources-and-software-titles"></a>6,4: onaylanan Azure kaynakları ve yazılım başlıkları envanterini koruyun

**Rehberlik**: uygulanamaz; Bu kılavuz, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: onaylanmamış Azure kaynakları için izleyici

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri

- İzin verilen kaynak türleri

Ayrıca, Azure Kaynak grafiğini kullanarak abonelikler içindeki kaynakları sorgular/bulur.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Graph ile sorgu oluşturma: https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: işlem kaynakları içindeki onaylanmamış yazılım uygulamaları için izleyici

**Rehberlik**: uygulanamaz; Bu taban çizgisi, işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: onaylanmamış Azure kaynaklarını ve yazılım uygulamalarını kaldırma

**Rehberlik**: uygulanamaz; Bu kılavuz, bir bütün olarak işlem kaynaklarına ve Azure 'a yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="68-use-only-approved-applications"></a>6,8: yalnızca onaylanan uygulamaları kullan

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="69-use-only-approved-azure-services"></a>6,9: yalnızca onaylanan Azure hizmetlerini kullanın

**Rehberlik**: aşağıdaki yerleşik ilke tanımlarını kullanarak müşteri aboneliklerine oluşturulabilecek kaynak türlerine kısıtlamalar koymak Için Azure ilkesini kullanın:

- İzin verilmeyen kaynak türleri 

- İzin verilen kaynak türleri

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilkesiyle belirli bir kaynak türünü reddetme: https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="610-implement-approved-application-list"></a>6,10: onaylanan uygulama listesini Uygula

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="611-limit-users-ability-to-interact-with-azureresources-manager-via-scripts"></a>6,11: kullanıcıların betikler aracılığıyla AzureResources Manager ile etkileşim kurma yeteneğini sınırlayın

**Rehberlik**: "Microsoft Azure yönetimi" uygulaması için "erişimi engelle" yapılandırarak kullanıcıların Azure Resource Manager etkileşime geçmesini sınırlamak Için Azure koşullu erişimini kullanın. Bu, yüksek bir güvenlik ortamındaki kaynaklarda oluşturma ve değişiklik yapılmasını önleyebilir.

Azure Resource Manager erişimi engellemek için koşullu erişimi yapılandırma: https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: kullanıcıların işlem kaynakları içinde betikleri yürütme yeteneğini sınırlayın

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: yüksek riskli uygulamaları fiziksel olarak veya mantıksal olarak ayırt edin

**Rehberlik**: uygulanamaz; Bu kılavuz, Azure App Service veya işlem kaynaklarında çalışan Web uygulamalarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="secure-configuration"></a>Güvenli Yapılandırma

*Daha fazla bilgi için bkz. [güvenlik denetimi: güvenli yapılandırma](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: tüm Azure kaynakları için güvenli yapılandırma oluşturma

**Kılavuz**: Azure ilkesiyle Cosmos DB örneklerinizin standart güvenlik yapılandırmasını tanımlayın ve uygulayın. Cosmos DB örneklerinizin yapılandırmasını denetlemek veya zorlamak üzere özel ilkeler oluşturmak için "Microsoft.DocumentDB" ad alanındaki Azure Ilke diğer adlarını kullanın. Ayrıca, Azure Cosmos DB için yerleşik ilke tanımlarından da yararlanabilirsiniz, örneğin:

- Cosmos DB hesapları için Gelişmiş tehdit koruması dağıtma

- Cosmos DB bir sanal ağ hizmeti uç noktası kullanmalıdır

Kullanılabilir Azure Ilkesi diğer adlarını görüntüleme: https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-3.3.0

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: güvenli işletim sistemi yapılandırması oluşturma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: güvenli Azure Kaynak yapılandırmalarının bakımını yapma

**Kılavuz**: Azure kaynaklarınız genelinde güvenli ayarları zorlamak Için Azure ilkesi [reddetme] ve [dağıtım yoksa dağıt] kullanın.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

Azure Ilke efektlerini anlayın: https://docs.microsoft.com/azure/governance/policy/concepts/effects

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: güvenli işletim sistemi yapılandırmalarının bakımını yapma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: Azure kaynaklarının yapılandırmasını güvenli bir şekilde depolayın

**Rehberlik**: Cosmos DB veya ilgili kaynaklarınız Için özel Azure ilke tanımları kullanıyorsanız, kodunuzu güvenli bir şekilde depolamak ve yönetmek için Azure Repos kullanın.

Azure Repos belgeleri: https://docs.microsoft.com/azure/devops/repos/index?view=azure-devopshttps://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: özel işletim sistemi görüntülerini güvenli bir şekilde depolayın

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="77-deploy-system-configuration-management-tools"></a>7,7: sistem yapılandırma yönetimi araçlarını dağıtma

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft.DocumentDB" ad alanındaki Azure ilke diğer adlarını kullanın. Ayrıca, ilke özel durumlarını yönetmek için bir işlem ve işlem hattı geliştirin.

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="78-deploy-system-configuration-management-tools-for-operating-systems"></a>7,8: işletim sistemleri için sistem yapılandırma yönetimi araçları dağıtma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="79-implement-automated-configuration-monitoring-for-azure-services"></a>7,9: Azure hizmetleri için otomatik yapılandırma izlemeyi uygulayın

**Rehberlik**: sistem yapılandırmalarına uyarı vermek, denetlemek ve zorlamak için özel ilkeler oluşturmak üzere "Microsoft.DocumentDB" ad alanındaki Azure ilke diğer adlarını kullanın. Azure Cosmos DB örneklerinizin ve ilgili kaynaklarınızın yapılandırmasını otomatik olarak zorlamak için [Denetim], [reddetme] ve [dağıtım yok] Azure Ilkesini kullanın. 

Azure Ilkesini yapılandırma ve yönetme: https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: işletim sistemleri için otomatik yapılandırma izlemeyi Uygula

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="711-manage-azure-secrets-securely"></a>7,11: Azure gizli dizilerini güvenli bir şekilde yönetin

**Rehberlik**: Azure Cosmos DB örneklerinizi erişmek için kullanılan Azure App Service üzerinde çalışan Azure sanal makineleri veya Web uygulamaları için, Azure Cosmos DB gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault birlikte yönetilen hizmet kimliği kullanın. Key Vault geçici silmenin etkinleştirildiğinden emin olun.

Azure yönetilen kimliklerle tümleştirme: https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

Key Vault oluşturma: https://docs.microsoft.com/azure/key-vault/general/quick-create-portal

Key Vault kimlik doğrulaması nasıl yapılır: https://docs.microsoft.com/azure/key-vault/general/authentication

Key Vault erişim ilkesi atama: https://docs.microsoft.com/azure/key-vault/general/assign-access-policy-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: kimlikleri güvenli ve otomatik olarak yönetme

**Rehberlik**: Azure Cosmos DB örneklerinizi erişmek için kullanılan Azure App Service üzerinde çalışan Azure sanal makineleri veya Web uygulamaları için, Azure Cosmos DB gizli yönetimi basitleştirmek ve güvenli hale getirmek için Azure Key Vault birlikte yönetilen hizmet kimliği kullanın.

Azure Active Directory (AD) içinde otomatik olarak yönetilen bir kimlik ile Azure hizmetleri sağlamak için Yönetilen kimlikler kullanın. Yönetilen kimlikler, kodunuzda kimlik bilgileri olmadan Key Vault dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmette kimlik doğrulaması yapmanıza olanak sağlar.

Yönetilen kimlikleri yapılandırma: https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm

Azure yönetilen kimliklerle tümleştirme: https://docs.microsoft.com/azure/azure-app-configuration/howto-integrate-azure-managed-service-identity

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: istenmeyen kimlik bilgisi pozlamasını ortadan kaldırın

**Rehberlik**: kod içinde kimlik bilgilerini tanımlamak Için kimlik bilgisi tarayıcısı uygulayın. Kimlik bilgisi tarayıcısı, bulunan kimlik bilgilerini Azure Key Vault gibi daha güvenli konumlara taşımayı de teşvik eder.

Kimlik bilgisi tarayıcısını ayarlama: https://secdevtools.azurewebsites.net/helpcredscan.html

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="malware-defense"></a>Kötü Amaçlı Yazılımdan Koruma

*Daha fazla bilgi için bkz. [güvenlik denetimi: kötü amaçlı yazılımdan koruma](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: merkezi olarak yönetilen kötü amaçlı yazılımdan koruma yazılımı kullanma

**Rehberlik**: uygulanamaz; Bu kılavuz işlem kaynaklarına yöneliktir. Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure App Service) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: işlem dışı Azure kaynaklarına yüklenecek dosyaları önceden Tara

**Rehberlik**: Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta (örneğin, Azure App Service) etkinleştirilir, ancak müşteri içeriğinde çalışmaz.

Azure Cosmos DB dahil olmak üzere işlem dışı Azure kaynaklarına karşıya yüklenen tüm dosyaları önceden taramak sizin sorumluluğunuzdadır. Microsoft müşteri verilerine erişemez ve bu nedenle müşteri içeriğine ait kötü amaçlı yazılımdan koruma taramalarının sizin adınıza yapılamaz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: kötü amaçlı yazılımdan koruma yazılımlarının ve imzaların güncelleştirildiğinden emin olun

**Rehberlik**: uygulanamaz; Kıyaslama işlem kaynaklarına yöneliktir. Microsoft kötü amaçlı yazılımdan koruma, Azure hizmetlerini destekleyen temel alınan konakta etkinleştirilir, ancak müşteri içeriği üzerinde çalışmaz.


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: geçerli değil

## <a name="data-recovery"></a>Veri Kurtarma

*Daha fazla bilgi için bkz. [güvenlik denetimi: veri kurtarma](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: düzenli Otomatik yedeklemeli UPS sağlayın

**Rehberlik**: Azure Cosmos DB, verilerinizin her dört saatte bir anlık görüntüsünü alır. Tüm yedeklemeler bir depolama hizmetinde ayrı olarak depolanır ve bu yedeklemeler, bölgesel felate karşı dayanıklılık açısından küresel olarak çoğaltılır. Belirli bir zamanda yalnızca son iki anlık görüntü tutulur. Bununla birlikte kapsayıcı veya veritabanı silinirse Azure Cosmos DB bu kapsayıcı veya veritabanının mevcut anlık görüntülerini 30 gün boyunca saklar. Yedekten geri yüklemek için Azure desteğine başvurun.

Azure Cosmos DB otomatikleştirilmiş yedeklemeleri anlama: https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: Microsoft

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: tüm sistem yedeklemelerini gerçekleştirin ve müşterinin yönettiği tüm anahtarları yedekleyin

**Rehberlik**: Azure Cosmos DB, düzenli aralıklarla verilerinizin yedeklerini otomatik olarak alır. Veritabanı veya kapsayıcı silinirse, verileri otomatik çevrimiçi yedeklemelerden geri yüklemek için bir destek bileti oluşturabilir veya Azure Desteği ' ni çağırabilirsiniz. Azure desteği, yalnızca standart, geliştirici ve planlardan daha yüksek planlar gibi seçili planlar için kullanılabilir. Yedeklemenin belirli bir anlık görüntüsünü geri yüklemek için Azure Cosmos DB, verilerin söz konusu anlık görüntüye ait yedekleme döngüsünün süresi boyunca kullanılabilir olmasını gerektirir. 

Cosmos DB örneklerinizin kimlik bilgilerini depolamak için Key Vault kullanıyorsanız, anahtarlarınızın düzenli otomatik yedeklemelerini sağlayın.

Otomatik yedeklemeleri Azure Cosmos DB anlayın: https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore

Azure Cosmos DB verileri geri yükleme: https://docs.microsoft.com/azure/cosmos-db/how-to-backup-and-restore

Key Vault anahtarlarını yedekleme: https://docs.microsoft.com/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey

**Azure Güvenlik Merkezi izleme**: Şu anda kullanılamıyor

**Sorumluluk**: paylaşılan

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: müşterinin yönettiği anahtarlar dahil tüm yedeklemeleri doğrulama

**Rehberlik**: veritabanı veya kapsayıcı silinirse, verileri otomatik çevrimiçi yedeklemelerden geri yüklemek için bir destek bileti dosyası veya Azure desteği çağırabilirsiniz. Azure desteği, yalnızca standart, geliştirici ve planlardan daha yüksek planlar gibi seçili planlar için kullanılabilir. Yedeklemenin belirli bir anlık görüntüsünü geri yüklemek için Azure Cosmos DB, verilerin söz konusu anlık görüntüye ait yedekleme döngüsünün süresi boyunca kullanılabilir olmasını gerektirir.

PowerShell kullanarak Azure Key Vault depolanan Sırlarınızın sınamasını geri yükleme. Restore-AzureKeyVaultKey cmdlet 'i, belirtilen anahtar kasasında bir anahtar oluşturur. Bu anahtar, giriş dosyasındaki yedeklenen anahtarın bir çoğaltmasıdır ve özgün anahtarla aynı ada sahiptir.

Otomatik yedeklemeleri Azure Cosmos DB anlayın:

https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore

Azure Cosmos DB verileri geri yükleme:

https://docs.microsoft.com/azure/cosmos-db/how-to-backup-and-restore

Azure Key Vault gizli dizileri geri yükleme:

https://docs.microsoft.com/powershell/module/azurerm.keyvault/restore-azurekeyvaultkey?view=azurermps-6.13.0

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: yedeklemelerin ve müşteri tarafından yönetilen anahtarların korunmasını sağlayın

**Kılavuz**: Cosmos db içinde depolanan tüm Kullanıcı verileri REST ve aktarımda şifrelendiğinden, herhangi bir işlem gerçekleştirmeniz gerekmez. Bunu yerleştirmek için başka bir yol da, bekleyen şifrelemenin varsayılan olarak "açık" olmasını sağlar. Kapatılacak veya açık bir denetim yoktur. Azure Cosmos DB, hesabın çalıştığı tüm bölgelerde AES-256 şifrelemesini kullanır.

Anahtarları yanlışlıkla veya kötü amaçlı silmeye karşı korumak için Key Vault geçici silme özelliğini etkinleştirin.

Azure Cosmos DB 'de veri şifrelemesini anlayın: https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest

Key Vault 'da geçici silmeyi etkinleştirme: https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: paylaşılan

## <a name="incident-response"></a>Olay Yanıtı

*Daha fazla bilgi için bkz. [güvenlik denetimi: olay yanıtı](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: olay yanıtı kılavuzu oluşturma

**Rehberlik**: kuruluşunuz için bir olay yanıtı Kılavuzu oluşturun. Tüm personel rollerinin yanı sıra olay işleme/yönetim 'in algılanmasından olay sonrası gözden geçirme aşamalarını tanımlayan, yazılı olay yanıt planları bulunduğundan emin olun.

Ayrıca, NıST 'nin bilgisayar güvenliği olay Işleme kılavuzumuzu, kendi olay yanıtı planınızın oluşturulmasına yardımcı olması için de kullanabilirsiniz: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf

Azure Güvenlik Merkezi 'nde Iş akışı Otomatiklamalar nasıl yapılandırılır: https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide

Kendi güvenlik olay yanıtı işleminizi oluşturma kılavuzu: https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/

Microsoft Güvenlik Yanıt Merkezi 'nin bir olayın Anatomisi: https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/


**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: olay Puanlama ve öncelik belirlemesi prosedürü oluşturma

**Rehberlik**: Güvenlik Merkezi, ilk olarak hangi uyarıların araştırılması gerektiğini önceliklendirmenize yardımcı olmak için her bir uyarıya önem derecesi atar. Önem derecesi, güvenlik merkezi 'nin bulmada ne kadar önemli olduğunu ya da uyarıyı vermek için kullanılan analizlerin yanı sıra uyarıya yönelik etkinliğin arkasında kötü amaçlı bir amaç olduğunu belirten güven düzeyini temel alır.

Ayrıca, abonelikleri açıkça işaretleyin (örn. üretim, üretim dışı) ve Azure kaynaklarını net bir şekilde tanımlamak ve kategorilere ayırmak için bir adlandırma sistemi oluşturun.

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="103-test-security-response-procedures"></a>10,3: test Güvenliği Yanıt yordamları

**Rehberlik**: sistem olay yanıt yeteneklerini düzenli bir temposunda test etmek için alıştırmaları gerçekleştirin. Zayıf noktaları ve boşlukları belirleyip planı gerektiği şekilde gözden geçirin.

NıST 'nin yayını: BT planları ve özellikleri için test, eğitim ve alıştırma programlarını inceleyin: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: güvenlik olaylarına ilişkin iletişim ayrıntılarını sağlayın ve güvenlik olayları için uyarı bildirimleri yapılandırın

**Rehberlik**: Microsoft Güvenlik Yanıt MERKEZI (MSRC), müşterinin verilerine izinsiz veya yetkisiz bir taraf tarafından erişildiğini belirlerse, Microsoft tarafından sizinle iletişim kurmak için güvenlik olayı iletişim bilgileri kullanılacaktır.  Sorunların çözümlendiğinden emin olmak için gerçesonra olayları gözden geçirin.

Azure Güvenlik Merkezi güvenlik Ilgili kişisini ayarlama: https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details

**Azure Güvenlik Merkezi izleme**: Evet

**Sorumluluk**: müşteri

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: güvenlik uyarılarını olay yanıt sisteminizle birleştirme

**Rehberlik**: sürekli dışa aktarma özelliğini kullanarak Azure Güvenlik Merkezi uyarılarınızı ve önerilerinizi dışarı aktarın. Sürekli dışa aktarma, uyarıları ve önerileri el ile veya devam eden sürekli bir biçimde dışa aktarmanız sağlar. Uyarılar Sentinel 'i akışa almak için Azure Güvenlik Merkezi veri bağlayıcısını kullanabilirsiniz.

Sürekli dışarı aktarmayı yapılandırma: https://docs.microsoft.com/azure/security-center/continuous-export

Uyarıları Azure Sentinel 'e aktarma: https://docs.microsoft.com/azure/sentinel/connect-azure-security-center

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: güvenlik uyarılarına yanıtı otomatikleştirme

**Rehberlik**: güvenlik uyarılarında ve önerilerinde "Logic Apps" aracılığıyla yanıtları otomatik olarak tetiklemek Için Azure Güvenlik Merkezi 'Nde Iş akışı Otomasyonu özelliğini kullanın.

Iş akışı otomasyonunu yapılandırma ve Logic Apps: https://docs.microsoft.com/azure/security-center/workflow-automation

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: müşteri

## <a name="penetration-tests-and-red-team-exercises"></a>Sızma Testleri ve Red Team Alıştırmaları

*Daha fazla bilgi için bkz. [güvenlik denetimi: Penetme testleri ve Red ekibi alıştırmaları](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings-within-60-days"></a>11,1: Azure kaynaklarınızın düzenli olarak sızma testini gerçekleştirin ve 60 gün içinde tüm kritik güvenlik bulgularını düzeltmeye dikkat edin

**Rehberlik**: Penettim testlerinizin Microsoft ilkelerini ihlal etmediğinden emin olmak Için Microsoft katılım kurallarını izleyin: https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1

Microsoft 'un, Microsoft tarafından yönetilen bulut altyapısına, hizmetlerine ve uygulamalarına göre kırmızı ekip oluşturma ve canlı site sızma testini yürütme hakkında daha fazla bilgi edinebilirsiniz:  https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e

**Azure Güvenlik Merkezi izleme**: uygulanamaz

**Sorumluluk**: paylaşılan

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik kıyaslaması](https://docs.microsoft.com/azure/security/benchmarks/overview)
- [Azure güvenlik temelleri](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview) hakkında daha fazla bilgi edinin
