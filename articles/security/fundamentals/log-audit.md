---
title: Azure günlük kaydı ve denetimi | Microsoft Docs
description: Uygulamanız hakkında derin Öngörüler elde etmek için günlük verilerini nasıl kullanabileceğinizi öğrenin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/14/2019
ms.author: TomSh
ms.openlocfilehash: b916e3073d5f6843b58980ce49c7b7c5871d218d
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727089"
---
# <a name="azure-logging-and-auditing"></a>Azure günlük kaydı ve denetim

Azure, güvenlik ilkeleriniz ve mekanizmalarda boşlukları belirlemenize yardımcı olmak için çok sayıda yapılandırılabilir güvenlik denetimi ve günlüğe kaydetme seçeneği sunar. Bu makalede, Azure 'da barındırılan hizmetlerden güvenlik günlüklerinin oluşturulması, toplanması ve çözümlenmesi anlatılmaktadır.

> [!Note]
> Bu makaledeki bazı öneriler, verilerin, ağın veya işlem kaynağı kullanımının artmasına ve lisans ya da abonelik maliyetlerinizi artırmaya yol açabilir.

## <a name="types-of-logs-in-azure"></a>Azure 'da günlüklerin türleri

Bulut uygulamaları çok sayıda hareketli parça ile karmaşıktır. Günlükler, uygulamalarınızı çalışır durumda tutmaya yardımcı olmak için veri sağlar. Günlükler, geçmişteki sorunları gidermenize veya olası olanları önlemenize yardımcı olur. Ayrıca, uygulama performansını veya bakımlılığını artırmaya ya da aksi takdirde el ile müdahale gerektiren eylemleri otomatikleştirmenize yardımcı olabilirler.

Azure günlükleri aşağıdaki türlerde kategorilere ayrılır:
* **Denetim/Yönetim günlükleri** Azure Resource Manager oluşturma, GÜNCELLEŞTIRME ve silme işlemleri hakkında bilgi sağlar. Daha fazla bilgi için bkz. [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).

* **Veri düzlemi günlükleri** , Bölüm Azure Kaynak kullanımı olarak oluşturulan olaylar hakkında bilgi sağlar. Bu tür bir günlüğe örnek olarak, bir sanal makinede (VM) Windows olay sistemi, güvenlik ve uygulama günlükleri ve Azure Izleyici aracılığıyla yapılandırılan [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) verilebilir.

* **İşlenen olaylar** , sizin adınıza işlenmiş olan çözümlenen olaylar/uyarılar hakkında bilgi sağlar. Bu tür örnekler, Azure [Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) 'nin aboneliğinizi işlediği ve çözümleymekte olduğu ve kısa güvenlik uyarıları sağladığı [Azure Güvenlik Merkezi uyarılarından](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) oluşur.

Aşağıdaki tabloda, Azure 'da kullanılabilen en önemli günlük türleri listelenmektedir:

| Günlük kategorisi | Günlük türü | Kullanım | Tümleştirme |
| ------------ | -------- | ------ | ----------- |
|[Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Azure Resource Manager kaynaklardaki denetim düzlemi olayları|   Aboneliğinizdeki kaynaklarda gerçekleştirilen işlemlere ilişkin öngörüler sağlar.|    REST API, [Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|Abonelikte Azure Resource Manager kaynakların çalışması hakkında sık kullanılan veriler|    Kaynağınızın kendisi tarafından gerçekleştirilen işlemlere ilişkin öngörüler sağlar.| Azure Izleyici, [akış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[Azure AD raporlama](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)|Günlükler ve raporlar | Kullanıcılar ve grup yönetimiyle ilgili Kullanıcı oturum açma etkinliklerini ve sistem etkinliği bilgilerini raporlar.|[Graph API'si](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Sanal makineler ve bulut Hizmetleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-collect-azurevm)|Windows olay günlüğü hizmeti ve Linux Syslog|  , Sanal makinelerde sistem verilerini yakalar ve verileri günlüğe kaydeder ve bu verileri seçtiğiniz bir depolama hesabına aktarır.|   Azure Izleyici 'de Windows (Windows Azure Tanılama [[wad](https://docs.microsoft.com/azure/azure-diagnostics)] depolama) ve Linux 'u kullanma|
|[Azure Depolama Analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Depolama alanı günlüğü, bir depolama hesabı için ölçüm verileri sağlar|İzleme istekleri hakkında öngörüler sağlar, kullanım eğilimlerini analiz eder ve depolama hesabınızla ilgili sorunları tanılar.|   REST API veya [istemci kitaplığı](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Ağ güvenlik grubu (NSG) akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|JSON biçimi, giden ve gelen akışları kural başına temelinde gösterir|Bir ağ güvenlik grubu üzerinden giriş ve çıkış IP trafiği hakkındaki bilgileri görüntüler.|[Azure ağ Izleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)|
|[Uygulama öngörüleri](https://docs.microsoft.com/azure/application-insights/app-insights-overview)|Günlükler, özel durumlar ve özel Tanılamalar|   Birden çok platformda Web geliştiricileri için bir uygulama performansı izleme (APM) hizmeti sağlar.| REST API, [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|İşlem verileri/güvenlik uyarıları|    Azure Güvenlik Merkezi uyarıları, Azure Izleyici günlükleri uyarıları|    Güvenlik bilgileri ve uyarılar sağlar.|  REST API 'Leri, JSON|

### <a name="activity-logs"></a>Etkinlik günlükleri

[Azure etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) , aboneliğinizdeki kaynaklarda gerçekleştirilen işlemlere ilişkin öngörüler sağlar. Etkinlik günlükleri, abonelikleriniz için [Denetim düzlemi olaylarını](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) rapordıklarından daha önce "Denetim günlükleri" veya "işletimsel Günlükler" olarak bilinirdi. 

Etkinlik günlükleri, yazma işlemleri için "neleri, kim ve ne zaman" (yani, gönder veya SIL) belirlemenize yardımcı olur. Etkinlik günlükleri aynı zamanda işlemin durumunu ve ilgili diğer özellikleri anlamanıza yardımcı olur. Etkinlik günlükleri okuma (GET) işlemlerini içermez.

Bu makalede, PUT, POST ve DELETE, kaynaklar üzerinde bir etkinlik günlüğünün içerdiği tüm yazma işlemlerine başvurur. Örneğin, sorunları giderirken veya kuruluşunuzdaki bir kullanıcının bir kaynağı nasıl değiştirdiğinizdeki bir hata bulmak için etkinlik günlüklerini kullanabilirsiniz.

![Etkinlik günlüğü diyagramı](./media/log-audit/azure-log-audit-fig1.png)

Azure portal, [Azure CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell cmdlet 'Leri ve [Azure izleyici REST API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough)kullanarak etkinlik günlüğünden olay alabilirsiniz. Etkinlik günlüklerinde 90 günlük veri saklama süresi vardır.

Etkinlik günlüğü olayı için tümleştirme senaryoları:

* [Etkinlik günlüğü olayı tarafından tetiklenen bir e-posta veya Web kancası uyarısı oluşturun](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email).

* Bir üçüncü taraf hizmeti veya PowerBI gibi özel bir analiz çözümünü [almak için bir olay hub 'ına akış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) yapın.

* PowerBI [içerik paketini](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)kullanarak PowerBI 'da çözümleyin.

* [Arşiv veya el ile inceleme için bir depolama hesabına kaydedin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log). Günlük profillerini kullanarak saklama süresini (gün olarak) belirtebilirsiniz.

* Azure portal sorgulayın ve görüntüleyin.

* PowerShell cmdlet 'i, Azure CLı veya REST API aracılığıyla sorgulayın.

* Etkinlik günlüğünü günlük profilleriyle [Azure izleyici günlüklerine](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)dışarı aktarın.

Günlüğü yayıcı ile aynı abonelikte olmayan bir depolama hesabı veya [Olay Hub 'ı ad alanı](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) kullanabilirsiniz. Bu ayarı yapılandırırken, her iki aboneliğe da uygun [rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) erişimi olmalıdır.

### <a name="azure-diagnostics-logs"></a>Azure tanılama günlükleri

Azure tanılama günlükleri, söz konusu kaynağın çalışması hakkında zengin ve sık veriler sağlayan bir kaynak tarafından dağıtılır. Bu günlüklerin içeriği kaynak türüne göre değişir. Örneğin, [Windows olay sistemi günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) VM 'ler için bir tanılama günlükleri kategorisidir ve [BLOB, tablo ve kuyruk günlükleri](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) depolama hesapları için tanılama günlüğü kategorileridir. Tanılama günlükleri, aboneliğinizdeki kaynaklarda gerçekleştirilen işlemlere ilişkin Öngörüler sağlayan etkinlik günlüklerinden farklıdır.

![Azure tanılama günlükleri diyagramları](./media/log-audit/azure-log-audit-fig2.png)

Azure tanılama günlükleri Azure portal, PowerShell, Azure CLı ve REST API gibi birden çok yapılandırma seçeneği sunar.

**Tümleştirme senaryoları**

* Denetim veya el ile inceleme için bunları bir [depolama hesabına](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) kaydedin. Tanılama ayarlarını kullanarak saklama süresini (gün olarak) belirtebilirsiniz.

* Bunları bir üçüncü taraf hizmeti veya [PowerBI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)gibi özel bir analiz çözümü tarafından içeri [aktarmak için Olay Hub 'larına akıtır](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) .

* Bunları [Azure izleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)ile çözümleyin.

**Desteklenen hizmetler, her kaynak türü için tanılama günlükleri şeması ve desteklenen günlük kategorileri**


| Hizmet | Şema ve belgeler | Kaynak türü | Category |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Load Balancer için Azure Izleyici günlükleri (Önizleme)](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers|    LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Ağ Güvenlik Grupları|[Ağ güvenlik grupları için Azure Izleyici günlükleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Azure Application Gateway|[Application Gateway için tanılama günlüğü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Key Vault günlükleri](https://docs.microsoft.com/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Arama Trafik Analizi etkinleştirme ve kullanma](https://docs.microsoft.com/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Azure Data Lake Store|[Data Lake Store için tanılama günlüklerine erişin](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Denetim<br>İstekler|
|Azure Data Lake Analytics|[Data Lake Analytics için tanılama günlüklerine erişin](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Denetim<br>İstekler|
|Azure Logic Apps|[Logic Apps B2B özel izleme şeması](https://docs.microsoft.com/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|Başlatıldıktan<br>Integrationaccounttrackingevents|
|Azure Batch|[Tanılama günlüklerini Azure Batch](https://docs.microsoft.com/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Otomasyonu|[Azure Otomasyonu için Azure Izleyici günlükleri](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft. Automation/automationAccounts<br>Microsoft. Automation/automationAccounts|JobLogs<br>JobStreams|
|Azure Event Hubs|[Tanılama günlüklerini Event Hubs](https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Azure Stream Analytics|[İş tanılama günlükleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft. StreamAnalytics/streamingjobs<br>Microsoft. StreamAnalytics/streamingjobs|Yürütme<br>Yazma|
|Azure Service Bus|[Service Bus tanılama günlükleri](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Azure Active Directory raporlama

Azure Active Directory (Azure AD), bir kullanıcının dizinine yönelik güvenlik, etkinlik ve Denetim raporlarını içerir. [Azure AD denetim raporu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) , kullanıcının Azure AD örneğinde gerçekleşen ayrıcalıklı eylemleri belirlemenize yardımcı olur. Ayrıcalıklı eylemler, yükseltme değişiklikleri (örneğin, rol oluşturma veya parola sıfırlama), ilke yapılandırmalarını değiştirme (örneğin, parola ilkeleri) veya dizin yapılandırmasındaki değişiklikler (örneğin, etki alanı Federasyon ayarlarındaki değişiklikler) içerir.

Raporlar, olay adı, eylemi gerçekleştiren kullanıcı, değişiklikten etkilenen hedef kaynak ve Tarih ve saat (UTC) için denetim kaydı sağlar. Kullanıcılar, [Denetim günlüklerinizi görüntüleme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)bölümünde açıklandığı gibi [Azure Portal](https://portal.azure.com/)aracılığıyla Azure AD için denetim olaylarının listesini alabilir. 

Dahil edilen raporlar aşağıdaki tabloda listelenmiştir:

| Güvenlik raporları | Etkinlik raporları | Denetim raporları |
| :--------------- | :--------------- | :------------ |
|Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri| Uygulama kullanımı: özet| Dizin denetimi raporu|
|Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri|  Uygulama kullanımı: ayrıntılı||
|Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri|    Uygulama panosu||
|Şüpheli etkinliğin olduğu IP adreslerinden oturum açma işlemleri|   Hesap hazırlama hataları||
|Düzensiz oturum açma etkinliği|    Bireysel kullanıcı cihazları||
|Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri|   Bireysel kullanıcı etkinliği||
|Anormal oturum açma etkinliği gösteren kullanıcılar| Grup etkinlik raporu||
||Parola sıfırlama kaydı etkinlik raporu||
||Parola sıfırlama etkinliği||

Bu raporlardaki veriler, uygulamalarınız için güvenlik bilgileri ve olay yönetimi (SıEM) sistemleri, denetim ve iş zekası araçları gibi yararlı olabilir. Azure AD raporlama API'leri, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu [API 'leri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) çağırabilirsiniz.

Azure AD denetim raporundaki olay saklama, kiracıyla ilişkili lisansın türüne bağlı olarak 7-90 gün arasında farklılık gösterir. 

> [!Note]
> Rapor saklama hakkında daha fazla bilgi için bkz. [Azure AD rapor saklama ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Denetim olaylarınızın daha uzun süre beklemesine ilgileniyorsanız, [denetim olaylarını](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) düzenli olarak ayrı bir veri deposuna çekmek IÇIN Raporlama API 'sini kullanın.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>Azure Tanılama kullanan sanal makine günlükleri

[Azure tanılama](https://docs.microsoft.com/azure/azure-diagnostics) , Azure 'daki, dağıtılan bir uygulamadaki tanılama verilerinin toplanmasını sağlayan bir özelliktir. Tanılama uzantısı 'nı birkaç kaynaktan kullanabilirsiniz. Şu anda desteklenen [Azure bulut hizmeti Web ve çalışan rolleridir](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me).

![Azure Tanılama kullanan sanal makine günlükleri](./media/log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineslearnpathsdeploy-a-website-with-azure-virtual-machines-that-are-running-microsoft-windows-and-service-fabrichttpsdocsmicrosoftcomazureservice-fabricservice-fabric-overview"></a>Microsoft Windows ve [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview) çalıştıran [Azure sanal makineleri](/learn/paths/deploy-a-website-with-azure-virtual-machines/)

Aşağıdakilerden birini yaparak bir sanal makinede Azure Tanılama etkinleştirebilirsiniz:

* [Azure sanal makinelerini izlemek için Visual Studio 'Yu kullanma](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Azure sanal makinesinde uzaktan Azure Tanılama ayarlama](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

* [Azure sanal makinelerinde tanılamayı ayarlamak için PowerShell 'i kullanma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Azure Resource Manager şablonu kullanarak izleme ve Tanılama ile Windows sanal makinesi oluşturma](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Depolama Analizi

[Azure depolama Analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) Günlükler ve bir depolama hesabı için ölçüm verileri sağlar. Bu verileri kullanarak istekleri izleyebilir, kullanım eğilimlerini çözümleyebilir ve depolama hesabınızdaki sorunları tanılayabilirsiniz. [Azure blob, Azure kuyruğu ve Azure Tablo Depolama Hizmetleri](https://docs.microsoft.com/azure/storage/storage-introduction)için depolama Analizi günlüğe kaydetme kullanılabilir. Depolama Analizi, başarılı ve başarısız isteklerle ilgili ayrıntılı bilgileri bir depolama hizmetine kaydeder.

Bu bilgileri, tek tek istekleri izlemek ve bir depolama hizmetiyle ilgili sorunları tanılamak için kullanabilirsiniz. İstekler en iyi çaba temelinde günlüğe kaydedilir. Günlük girişleri yalnızca hizmet uç noktasında yapılan istekler varsa oluşturulur. Örneğin, bir depolama hesabının kendi BLOB uç noktasında etkinliği varsa ancak tablo veya sıra uç noktalarında etkinlik varsa, yalnızca BLOB depolama hizmeti 'ne ait günlükler oluşturulur.

Depolama Analizi kullanmak için, izlemek istediğiniz her hizmet için tek tek etkinleştirin. [Azure Portal](https://portal.azure.com/)etkinleştirebilirsiniz. Daha fazla bilgi için, [Azure Portal depolama hesabını izleme](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account)bölümüne bakın. Ayrıca, REST API veya istemci kitaplığı aracılığıyla Depolama Analizi programlı bir şekilde etkinleştirebilirsiniz. Her hizmet için Depolama Analizi tek tek etkinleştirmek üzere hizmet özelliklerini ayarlama işlemini kullanın.

Toplanan veriler, blob Storage hizmeti ve tablo depolama hizmeti API 'Leri kullanarak erişebileceğiniz iyi bilinen bir blob 'da (günlük kaydı için) ve iyi bilinen tablolarda (ölçümler için) depolanır.

Depolama Analizi, depolama hesabınız için toplam sınırdan bağımsız olan depolanan verilerin miktarına göre 20 terabaytlık (TB) bir sınıra sahiptir. Tüm Günlükler, bir depolama hesabı için Depolama Analizi etkinleştirdiğinizde otomatik olarak oluşturulan $logs adlı kapsayıcıda [Blok Bloblarında](https://docs.microsoft.com/azure/storage/storage-analytics) depolanır.

> [!Note]
> * Faturalandırma ve veri saklama ilkeleri hakkında daha fazla bilgi için bkz. [depolama Analizi ve faturalandırma](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Depolama hesabı sınırları hakkında daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Depolama Analizi, aşağıdaki kimlik doğrulamalı ve anonim isteklerin türlerini günlüğe kaydeder:

| Kimliği Doğrulandı  | Adsız|
| :------------- | :-------------|
| Başarılı istekler | Başarılı istekler |
|Zaman aşımı, azaltma, ağ, yetkilendirme ve diğer hatalar da dahil olmak üzere başarısız istekler | Başarısız ve başarılı istekler dahil, paylaşılan erişim imzası kullanan istekler |
| Başarısız ve başarılı istekler dahil, paylaşılan erişim imzası kullanan istekler |İstemci ve sunucu için zaman aşımı hataları |
|   Analiz verilerine yönelik istekler |    304 hata koduyla başarısız olan istek ALıNAMADı (değiştirilmedi) |
| Günlük oluşturma veya silme gibi Depolama Analizi kendisi tarafından yapılan istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi [depolama Analizi günlüğe kaydedilmiş işlemler ve durum iletileri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analizi günlük biçiminde](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format)belgelenmiştir. | Diğer tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi [depolama Analizi günlüğe kaydedilmiş işlemler ve durum iletileri](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) ve [depolama Analizi günlük biçiminde](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format)belgelenmiştir. |

### <a name="azure-networking-logs"></a>Azure ağ günlükleri

Azure 'da ağ günlüğe kaydetme ve izleme kapsamlı ve iki geniş kategori ele alınmaktadır:

* [Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Senaryo tabanlı ağ izleme, ağ izleyicisinden özelliklerle birlikte sağlanır. Bu hizmet, paket yakalama, sonraki atlama, IP akışı doğrulama, güvenlik grubu görünümü, NSG akış günlükleri içerir. Senaryo düzeyi izleme, tek tek ağ kaynağı izlemeye karşılık olarak ağ kaynaklarının uçtan uca bir görünümünü sağlar.

* [Kaynak izleme](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Kaynak düzeyinde izleme dört özellik, tanılama günlüğü, ölçüm, sorun giderme ve kaynak sistem durumu içerir. Bu özelliklerin tümü, ağ kaynak düzeyinde oluşturulmuştur.

![Azure ağ günlükleri](./media/log-audit/azure-log-audit-fig4.png)

Ağ Izleyicisi, Azure 'daki ve Azure 'dan bir ağ senaryosu düzeyinde koşulları izlemenizi ve tanılamanıza olanak tanıyan bölgesel bir hizmettir. Ağ Izleyicisi ile kullanılabilen ağ tanılama ve görselleştirme araçları, Azure 'da ağınızı anlamanıza, tanılamanıza ve elde etmenize yardımcı olur.

### <a name="network-security-group-flow-logging"></a>Ağ güvenlik grubu akış günlüğü

[NSG akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) , BIR NSG aracılığıyla giriş ve çıkış IP trafiği hakkındaki bilgileri görüntülemek için kullanabileceğiniz bir ağ izleyicisi özelliğidir. Bu akış günlükleri JSON biçiminde yazılır ve şunları gösterir:
* Her kural için giden ve gelen akış.
* Akışın geçerli olduğu NIC.
* 5-akışla ilgili kayıt bilgileri: kaynak veya hedef IP, kaynak veya hedef bağlantı noktası ve protokol.
* Trafiğe izin verilip verilmediği.

Flow, NSG 'leri hedeflerse, diğer Günlükler ile aynı şekilde gösterilmezler. Akış günlükleri yalnızca bir depolama hesabı içinde depolanır.

Diğer günlüklerde görülen aynı bekletme ilkeleri akış günlükleri için geçerlidir. Günlüklerde, 1 günden 365 güne ayarlayabileceğiniz bir bekletme ilkesi vardır. Bekletme ilkesi ayarlanmazsa günlükler süresiz olarak saklanır.

**Tanılama günlükleri**

Düzenli ve Spontaneous olayları ağ kaynakları tarafından oluşturulur ve depolama hesaplarında günlüğe kaydedilir ve bir olay hub 'ına ya da Azure Izleyici günlüklerine gönderilir. Günlükler, bir kaynağın sistem durumuna ilişkin öngörüler sağlar. Bunlar, Power BI ve Azure Izleyici günlükleri gibi araçlarla görüntülenebilir. Tanılama günlüklerini görüntülemeyi öğrenmek için bkz. [Azure izleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

![Tanılama günlükleri](./media/log-audit/azure-log-audit-fig5.png)

Tanılama günlükleri [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), rotalar ve [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)için kullanılabilir.

Ağ Izleyicisi bir tanılama günlükleri görünümü sağlar. Bu görünüm, tanılama günlüğünü destekleyen tüm ağ kaynaklarını içerir. Bu görünümden, ağ kaynaklarını kolay ve hızlı bir şekilde etkinleştirebilir ve devre dışı bırakabilirsiniz.


Daha önce bahsedilen günlüğe kaydetme özelliklerine ek olarak, ağ Izleyicisi Şu anda aşağıdaki yeteneklere sahiptir:
- [Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): Bir kaynak grubundaki ağ kaynakları arasındaki çeşitli bağlantılar ve ilişkilendirmeleri gösteren ağ düzeyinde bir görünüm sağlar.

- [Değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): Sanal bir makinenin içindeki ve içindeki paket verilerini yakalar. Gelişmiş filtreleme seçenekleri ve zaman ve boyut sınırlaması ayarları gibi ince ayar denetimleri, çok yönlülük sağlar. Paket verileri bir blob deposunda veya *. Cap* dosya biçimindeki yerel diskte depolanabilir.

- [IP akışı doğrulaması](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): Akış bilgileri 5 demet paket parametrelerine (yani, hedef IP, kaynak IP, hedef bağlantı noktası, kaynak bağlantı noktası ve protokol) bağlı olarak bir paketin izin verilip verilmeyeceğini veya reddedildiğini denetler. Paket bir güvenlik grubu tarafından reddedilirse, paketi reddeden kural ve Grup döndürülür.

- [Sonraki atlama](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Azure ağ dokusunda yönlendirilmekte olan paketlerin bir sonraki atlamasını belirler, böylece yanlış yapılandırılmış kullanıcı tanımlı yolların tanınabilmesi sağlanır.

- [Güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): Bir VM 'ye uygulanan geçerli ve uygulanan güvenlik kurallarını alır.

- [Sanal ağ geçidi ve bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): Sanal ağ geçitlerinin ve bağlantılarının sorunlarını gidermenize yardımcı olur.

- [Ağ abonelik limitleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Ağ kaynak kullanımını sınırlara karşı görüntülemenize olanak sağlar.

### <a name="application-insights"></a>Application Insights

[Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) , birden çok platformda Web geliştiricileri için GENIŞLETILEBILIR bir APM hizmetidir. Canlı Web uygulamalarını izlemek için kullanın. Performans bozuklulıkları otomatik olarak algılar. Sorunları tanılamanıza ve kullanıcıların uygulamanızla aslında neler yaptığını anlamanıza yardımcı olan güçlü analiz araçları içerir.

Application Insights, performansı ve kullanılabilirliği sürekli olarak iyileştirmenize yardımcı olmak için tasarlanmıştır.

Şirket içinde veya bulutta barındırıldığından, .NET, Node. js ve Java EE dahil olmak üzere çok çeşitli platformlarda uygulamalar için geçerlidir. DevOps süreciyle tümleştirilir ve çeşitli geliştirme araçlarıyla bağlantı noktalarına sahiptir.

![Application Insights diyagramı](./media/log-audit/azure-log-audit-fig6.png)

Geliştirme takımına yönelik olan Application Insights, uygulamanızın performansını ve nasıl kullanıldığını anlamanıza yardımcı olur. Şunları izler:

* **İstek ücretleri, yanıt süreleri ve hata oranları**: En popüler sayfaları, günün hangi saatlerinde ve kullanıcılarınızın nerede olduğunu öğrenin. En iyi performansı hangi sayfaların gösterdiğini görün. Daha fazla istek olduğunda yanıt süreleriniz ve hata oranları yüksek olursa, kaynak alma sorununuz olabilir.

* **Bağımlılık oranları, yanıt süreleri ve hata oranları**: Dış hizmetlerin sizi yavaşlatılıp yavaşlatmadığını öğrenin.

* **Özel durumlar**: Toplu istatistikleri çözümleyin veya belirli örnekleri seçip yığın izlemenin ve ilgili isteklerin detayına gidin. Hem sunucu hem de tarayıcı özel durumları raporlanır.

* **Sayfa görünümleri ve yük performansı**: Kullanıcılarınızın tarayıcılarındaki raporları alın.

* **Ajax çağrıları**: Web sayfası ücretleri, yanıt süreleri ve hata ücretleri alın.

* **Kullanıcı ve oturum sayıları**.

* **Performans sayaçları**: Windows veya Linux sunucu makinelerinizden CPU, bellek ve ağ kullanımı gibi verileri alın.

* **Konak tanılama**: Docker veya Azure 'dan veri alın.

* **Tanılama izleme günlükleri**: İzleme olaylarını isteklerle ilişkilendirebilmeniz için uygulamanızdan veri alın.

* **Özel olaylar ve ölçümler**: Satılan veya oyun kazanılan öğeler gibi iş olaylarını izlemek için istemci veya sunucu koduna yazdığınız verileri alın.

Aşağıdaki tabloda, tümleştirme senaryoları listelenmiştir ve açıklanmaktadır:

| Tümleştirme senaryosu | Açıklama |
| --------------------- | :---------- |
|[Uygulama eşlemesi](https://docs.microsoft.com/azure/application-insights/app-insights-app-map)|Uygulamanızın bileşenlerinin yanı sıra önemli ölçüm ve uyarılar.|
|[Örnek verileri için tanılama araması](https://docs.microsoft.com/azure/application-insights/app-insights-diagnostic-search)| İstekler, özel durumlar, bağımlılık çağrıları, günlük izlemeleri ve sayfa görüntülemeleri gibi olaylarda arama yapın ve bunları filtreleyin.|
|[Toplanan veriler için Ölçüm Gezgini](https://docs.microsoft.com/azure/azure-monitor/app/metrics-explorer)|İstek, hata ve özel durum oranları; yanıt süreleri, sayfa yükleme süreleri gibi toplu verileri keşfedin, filtreleyin ve bölümlere ayırın.|
|[Panolar](https://docs.microsoft.com/azure/azure-monitor/app/overview-dashboard)|Birden çok kaynaktan toplanan verileri birleştirin ve başkalarıyla paylaşın. Çok bileşenli uygulamalar ve takım odasında sürekli görüntüleme için idealdir.|
|[Canlı Ölçüm Akışı](https://docs.microsoft.com/azure/azure-monitor/app/live-stream)|Yeni bir derleme dağıttığınızda, her şeyin beklendiği gibi çalıştığından emin olmak için bu neredeyse gerçek zamanlı performans göstergelerini izleyin.|
|[Analizler](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)|Bu güçlü sorgulama dilini kullanarak uygulamanızın performansı ve kullanımıyla ilgili zor soruları yanıtlayın.|
|[Otomatik ve el ile uyarılar](https://docs.microsoft.com/azure/application-insights/app-insights-alerts)|Otomatik uyarılar, uygulamanızın normal telemetri desenlerine uyarlar ve normal desen dışında bir şey olduğunda tetiklenir. Belirli özel veya standart ölçüm düzeylerinde de uyarılar ayarlayabilirsiniz.|
|[Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)|Koddaki performans verilerini görüntüleyin. Yığın izlemelerinden koda gidin.|
|[Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)|Kullanım ölçümlerini diğer iş zekası verileriyle tümleştirin.|
|[REST API](https://dev.applicationinsights.io/)|Ölçümleriniz ve ham verileriniz üzerinde sorgu çalıştırmak için kod yazın.|
|[Sürekli dışarı aktarma](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)|Ham verilerin, ulaştığında depoya toplu olarak dışarı aktarılması.|

### <a name="azure-security-center-alerts"></a>Azure Güvenlik Merkezi uyarıları

Azure Güvenlik Merkezi tehdit algılama, Azure kaynaklarınızdan, ağınızdan ve bağlı iş ortağı çözümlerinden güvenlik bilgilerini otomatik olarak toplayarak işe yarar. Tehditleri belirlemek amacıyla bu bilgileri genellikle birden fazla kaynaktan bilgileri ilişkilendirerek analiz eder. Güvenlik uyarıları, Güvenlik Merkezi’nde tehdidin nasıl düzeltileceğine ilişkin önerilerle birlikte öncelik sırasına koyulur. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro).

![Azure Güvenlik Merkezi diyagramı](./media/log-audit/azure-log-audit-fig7.png)

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Tüm bulut dokusu genelinde olayları değerlendirmek için büyük veri ve [makine öğrenimi](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) teknolojilerinde sıçramalar uygular. Bu şekilde, el ile yaklaşımlar kullanarak ve saldırı evrimini tahmin ederek belirlenmesi olanaksız olabilecek tehditleri algılar. Bu güvenlik analizleri şunlardır:

* **Tümleşik tehdit bilgileri**: , Microsoft ürün ve Hizmetleri, Microsoft Digital Crimes birimi (DCU), Microsoft Güvenlik Yanıt Merkezi (MSRC) ve dış akışların küresel tehdit bilgilerini uygulayarak bilinen kötü aktörlere bakar.

* **Davranış analizi**: Kötü amaçlı davranışı bulmaya yönelik bilinen desenler uygular.

* **Anomali algılama**: Bir geçmiş taban çizgisi oluşturmak için istatistiksel profil oluşturma kullanır. Olası bir saldırı vektörüne uygun olan yerleşik taban çizgilerinden sapmalar konusunda uyarır.

Birçok güvenlik işlemi ve olay yanıt takımları, güvenlik uyarılarını önceliklendirme ve araştırmak için başlangıç noktası olarak bir SıEM çözümünü kullanır. Azure günlük tümleştirmesi ile Azure tanılama ve denetim günlükleri tarafından toplanan Güvenlik Merkezi uyarılarını ve sanal makine güvenlik olaylarını, neredeyse gerçek zamanlı olarak Azure Izleyici günlüklerinizi veya SıEM çözümüne eşitleyebilirsiniz.

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Azure Izleyici günlükleri, bulut ve şirket içi ortamlarınızdaki kaynaklar tarafından oluşturulan verileri toplayıp analiz etmenize yardımcı olan bir Azure hizmetidir. Bu, fiziksel konumlarından bağımsız olarak tüm iş yüklerinizde ve sunucularınızda milyonlarca kaydı kolayca çözümlemek için tümleşik arama ve özel panolar kullanarak gerçek zamanlı içgörüler sağlar.

![Azure Izleyici günlükleri diyagramı](./media/log-audit/azure-log-audit-fig8.png)

Azure Izleyici günlüklerinin merkezinde Azure 'da barındırılan Log Analytics çalışma alanı bulunur. Azure Izleyici günlükleri veri kaynaklarını yapılandırarak ve aboneliğinize çözümler ekleyerek çalışma alanındaki verileri bağlı kaynaklardan toplar. Her biri kendi özellik kümesine sahip olan veri kaynakları ve çözümlerin her biri farklı kayıt türleri oluşturur. Ancak kaynaklar ve çözümler, çalışma alanına yönelik sorgularda birlikte çözümlenmeye devam edebilir. Bu özellik, çeşitli kaynaklarla toplanan çeşitli verilerle çalışmak için aynı araçları ve yöntemleri kullanmanıza olanak sağlar.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Bağlı kaynaklar, Azure Izleyici günlükleri tarafından toplanan verileri oluşturan bilgisayarlar ve diğer kaynaklardır. Kaynaklar, doğrudan bağlanan [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) ve [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) bilgisayarlarına yüklenen aracıları veya [bağlı bir System Center Operations Manager yönetim grubundaki](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents)aracıları içerebilir. Azure Izleyici günlükleri, [Azure depolama hesabından](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)de veri toplayabilir.

[Veri kaynakları](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) , her bağlı kaynaktan toplanan çeşitli veri türleridir. Kaynaklar, [IIS günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs) ve [özel metin günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs)gibi kaynaklara ek olarak [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) ve Linux aracılarından gelen olayları ve [performans verilerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) içerir. Toplamak istediğiniz her veri kaynağını yapılandırabilirsiniz. Yapılandırma, otomatik olarak bağlı her kaynağa dağıtılır.

[Azure hizmetleri için günlükleri ve ölçümleri toplamanın](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)dört yolu vardır:

* Azure Izleyici günlüklerine doğrudan Azure Tanılama (aşağıdaki tabloda**Tanılama** )

* Azure depolama 'ya Azure Izleyici günlüklerine Azure Tanılama (aşağıdaki tabloda bulunan**depolama** )

* Azure hizmetleri için bağlayıcılar (aşağıdaki tabloda**bağlayıcı** )

* Verileri toplamak ve Azure Izleyici günlüklerine göndermek için betikler (aşağıdaki tablodaki boş hücreler ve listelenmemiş hizmetler için)

| Hizmet | Kaynak türü | Günlükler | Ölçümler | Çözüm |
| :------ | :------------ | :--- | :------ | :------- |
|Azure Application Gateway| Microsoft. Network/<br>Applicationgateway 'ler|  Tanılama|Tanılama|    [Azure uygulaması](https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics) [Ağ geçidi Analizi](https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics#azure-application-gateway-analytics-solution-in-azure-monitor)|
|Application Insights||     Bağlayıcı|  Bağlayıcı|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Bağlayıcı (Önizleme)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Azure Otomasyonu hesapları| Microsoft. Automation/<br>AutomationAccounts|    Tanılama||       [Daha fazla bilgi](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Azure Batch hesapları|  Microsoft. Batch/<br>batchAccounts|  Tanılama|    Tanılama||
|Klasik bulut Hizmetleri||       Depolama||       [Daha fazla bilgi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Bilişsel Hizmetler|    Microsoft. Biliveservices/<br>hesaplar|       Tanılama|||
|Azure Data Lake Analytics| Microsoft.DataLakeAnalytics/<br>hesaplar|   Tanılama|||
|Azure Data Lake Store| Microsoft.DataLakeStore/<br>hesaplar|   Tanılama|||
|Azure Olay Hub 'ı ad alanı| Microsoft.EventHub/<br>ad alanları|  Tanılama|    Tanılama||
|Azure IoT Hub| Microsoft. Devices/<br>Iothubs||     Tanılama||
|Azure Key Vault|   Microsoft.KeyVault/<br>kasaları|  Tanılama  || [Anahtar Kasası Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault)|
|Azure Load Balancer|   Microsoft. Network/<br>loadBalancers|    Tanılama|||
|Azure Logic Apps|  Microsoft.Logic/<br>sürdürülen|  Tanılama|    Tanılama||
||Microsoft.Logic/<br>Tümleştirme hesapları||||
|Ağ Güvenlik Grupları|   Microsoft. Network/<br>networksecuritygroups|Tanılama||   [Azure ağ güvenlik grubu Analizi](https://docs.microsoft.com/azure/azure-monitor/insights/azure-networking-analytics#azure-application-gateway-and-network-security-group-analytics)|
|Kurtarma kasaları|   Microsoft. RecoveryServices/<br>kasaları|||[Azure kurtarma Hizmetleri analizi (Önizleme)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Hizmet ara|   Microsoft. Search/<br>searchServices|    Tanılama|    Tanılama||
|Service Bus ad alanı| Microsoft. ServiceBus/<br>ad alanları|    Tanılama|Tanılama|    [Service Bus analizi (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Depolama||    [Service Fabric analizi (Önizleme)](https://docs.microsoft.com/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>larý<br>databases||       Tanılama||
||Microsoft.Sql/<br>larý<br>Elaun havuzları||||
|Depolama|||         Betik| [Azure depolama analizi (Önizleme)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Azure sanal makineleri|    Microsoft. COMPUTE/<br>virtualMachines|  Dahili numara|  Dahili numara||
||||Tanılama||
|Sanal makine ölçek kümeleri|    Microsoft. COMPUTE/<br>virtualMachines    ||Tanılama||
||Microsoft. COMPUTE/<br>virtualMachineScaleSets<br>virtualMachines||||
|Web sunucusu grupları|Microsoft.Web/<br>serverfarms||   Tanılama
|Web siteleri|  Microsoft.Web/<br>siteler ||      Tanılama|    [Daha fazla bilgi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>barındıra<br>Lara||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Şirket içi SıEM sistemleriyle günlük tümleştirmesi

Azure günlük tümleştirmesi sayesinde, Azure kaynaklarınızdan ham günlükleri şirket içi SıEM sisteminizle (güvenlik bilgileri ve olay yönetim sistemi) tümleştirebilirsiniz. AzLog İndirmeleri 27 Haz 2018 ' de devre dışı bırakıldı. İleriye doğru iletme hakkında yönergeler için gönderiyi [SıEM araçlarıyla tümleştirme Için Azure Izleyicisini kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

![Günlük tümleştirmesi diyagramı](./media/log-audit/azure-log-audit-fig9.png)

Günlük tümleştirmesi, Windows sanal makinelerinizden, Azure etkinlik günlüklerinizi, Azure Güvenlik Merkezi uyarılarından ve Azure Kaynak sağlayıcısı günlüklerinden Azure tanılama 'yı toplar. Bu tümleştirme, tüm varlıklarınız için şirket içinde veya bulutta olup olmalarından bağımsız olarak, güvenlik olaylarını toplayabilmek, ilişkilendirebilmeniz, analiz etmek ve uyarmak için birleştirilmiş bir pano sağlar.

Azure aboneliğiniz, Azure Güvenlik Merkezi uyarıları, Azure tanılama günlükleri ve Azure AD denetim günlükleriniz ile Azure etkinlik günlüklerinin tümleştirilmesi Şu anda Azure etkinlik günlükleri, Windows sanal makinelerinizdeki Windows olay günlükleri desteklemektedir.

| Günlük türü | JSON (splunk, Arcgözetimi ve IBM QRadar) destekleyen Azure Izleyici günlükleri |
| :------- | :-------------------------------------------------------- |
|Azure AD denetim günlükleri|   Evet|
|Etkinlik günlükleri| Evet|
|Güvenlik Merkezi uyarıları |Evet|
|Tanılama günlükleri (kaynak günlükleri)|  Evet|
|VM günlükleri|   Evet, iletilen olaylar aracılığıyla JSON aracılığıyla değil|

[Azure günlük tümleştirmesi 'ni kullanmaya](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started)başlayın: Bu öğretici, Azure günlük tümleştirmesini yükleme ve günlükleri Azure depolama, Azure etkinlik günlükleri, Azure Güvenlik Merkezi uyarıları ve Azure AD denetim günlüklerinden tümleştirme konusunda size kılavuzluk eder.

SıEM için tümleştirme senaryoları:

* [Iş ortağı yapılandırma adımları](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Bu blog gönderisi, Azure günlük tümleştirmesinin, iş ortağı çözümleri ile birlikte çalışmak üzere nasıl yapılandırılacağını gösterir.

* [Azure günlük tümleştirmesi hakkında SSS](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq): Bu makalede Azure günlük tümleştirmesi hakkında sorular yanıtlanmaktadır.

* [Güvenlik Merkezi uyarılarını Azure günlük tümleştirmesi Ile tümleştirme](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration): Bu makalede, Azure Izleyici günlüklerinizi veya SıEM çözümünüzle Azure tanılama günlükleri tarafından toplanan Güvenlik Merkezi uyarılarını, sanal makine güvenlik olaylarını ve Azure denetim günlüklerini nasıl eşitleyebileceğinizi ele alınmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

- [Denetim ve günlüğe kaydetme](https://docs.microsoft.com/azure/security/security-management-and-monitoring-overview): Görünürlüğü koruyarak verileri koruyun ve zamanında güvenlik uyarılarına hızla yanıt verebilirsiniz.

- [Azure 'Da güvenlik günlüğü ve denetim günlüğü koleksiyonu](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): Azure örneklerinizin doğru güvenlik ve denetim günlüklerini toplamasını sağlamak için bu ayarları uygulayın.

- [Site koleksiyonu için denetim ayarlarını yapılandırın](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): Bir site koleksiyonu yöneticisiyseniz, bireysel kullanıcıların eylemlerinin geçmişini ve belirli bir tarih aralığında gerçekleştirilen eylemlerin geçmişini alın. 

- [Office 365 güvenlik ve Uyumluluk Merkezi denetim günlüğünde arama](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US)yapın: Office 365 kuruluşunuzda birleştirilmiş denetim günlüğünü aramak ve Kullanıcı ve yönetici etkinliğini görüntülemek için Office 365 Güvenlik ve Uyumluluk Merkezi kullanın.


