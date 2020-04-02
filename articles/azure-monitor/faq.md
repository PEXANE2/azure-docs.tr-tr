---
title: Azure Monitör SSS | Microsoft Dokümanlar
description: Azure Monitor hakkında sık sorulan soruların yanıtları.
services: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/26/2020
ms.openlocfilehash: 306d847c2bc5af72d37dbf8bf472a5bae63e9fd5
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80528494"
---
# <a name="azure-monitor-frequently-asked-questions"></a>Azure Monitörü Sık Sorulan Sorular

Bu Microsoft SSS, Azure Monitor hakkında sık sorulan soruların bir listesidir.

## <a name="general"></a>Genel

### <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?
[Azure Monitor,](overview.md) Azure'daki, diğer bulut ortamlarında veya şirket içi uygulamalar ve hizmetler için performans ve kullanılabilirlik izleme sağlayan Azure'daki bir hizmettir. Azure Monitor, birden çok kaynaktan gelen verileri eğilimler ve anormallikler için analiz edilebilen ortak bir veri platformunda toplar. Azure Monitor'daki zengin özellikler, uygulamanızı etkileyebilecek kritik durumları hızlı bir şekilde tanımlamanıza ve bunlara yanıt vermenize yardımcı olur.

### <a name="whats-the-difference-between-azure-monitor-log-analytics-and-application-insights"></a>Azure Monitör, Log Analytics ve Application Insights arasındaki fark nedir?
Eylül 2018'de Microsoft, uygulamalarınızın ve güvendikleri bileşenlerin uçtan uca güçlü bir şekilde izlenmesini sağlamak için Azure Monitor, Log Analytics ve Application Insights'ı tek bir hizmette birleştirilmiştir. Log Analytics ve Application Insights'taki özellikler değişmemekle birlikte, bazı özellikler yeni kapsamlarını daha iyi yansıtmak için Azure Monitor'a yeniden markalandırılmıştır. Log Analytics'in günlük veri motoru ve sorgu dili artık Azure Monitör Günlükleri olarak adlandırılır. Bkz. [Azure Monitör terminolojisi güncelleştirmeleri.](terminology.md)

### <a name="what-does-azure-monitor-cost"></a>Azure Monitör'ün maliyeti nedir?
Azure Monitor'un ölçümlerin toplanması ve etkinlik günlükleri gibi otomatik olarak etkinleştirilen özellikleri hiçbir ücret ödemeden sağlanır. Günlük sorguları ve uyarı gibi diğer özelliklerle ilişkili bir maliyet vardır. Ayrıntılı fiyatlandırma bilgileri için [Azure Monitörfiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/monitor/) bakın.

### <a name="how-do-i-enable-azure-monitor"></a>Azure Monitör'ü nasıl etkinleştirebilirim?
Azure Monitor, yeni bir Azure aboneliği oluşturduğunuz anda etkinleştirilir ve [Etkinlik günlüğü](platform/activity-logs-overview.md) ve platform [ölçümleri](platform/data-platform-metrics.md) otomatik olarak toplanır. Azure kaynaklarınızın çalışması hakkında daha ayrıntılı bilgi toplamak için [tanı lama ayarları](platform/diagnostic-settings.md) oluşturun ve belirli hizmetler için toplanan veriler üzerinde ek analizler sağlamak için izleme [çözümleri](insights/solutions.md) ve [öngörüler](insights/insights-overview.md) ekleyin. 

### <a name="how-do-i-access-azure-monitor"></a>Azure Monitör'e nasıl erişebilirim?
Azure portalındaki **Monitör** menüsünden tüm Azure Monitor özelliklerine ve verilerine erişin. Farklı Azure hizmetleri için menünün **İzleme** bölümü, belirli bir kaynağa filtre uygulanmış verilerle aynı araçlara erişim sağlar. Azure Monitor verilerine CLI, PowerShell ve REST API'yi kullanarak çeşitli senaryolar da erişebilir.

### <a name="is-there-an-on-premises-version-of-azure-monitor"></a>Azure Monitor'un şirket içi sürümü var mı?
Hayır. Azure Monitor, büyük miktarda veriyi işleyen ve depolayan ölçeklenebilir bir bulut hizmetidir, ancak Azure Monitor şirket içi ve diğer bulutlardaki kaynakları izleyebilir.

### <a name="can-azure-monitor-monitor-on-premises-resources"></a>Azure Monitör şirket içi kaynakları izleyebilir mi?
Evet, Azure kaynaklarından izleme verileri toplamanın yanı sıra, Azure Monitor diğer bulutlardaki ve şirket içi sanal makinelerden ve uygulamalardan veri toplayabilir. Bkz. [Azure Monitor için izleme verikaynakları.](platform/data-sources.md)

### <a name="does-azure-monitor-integrate-with-system-center-operations-manager"></a>Azure Monitor, System Center Operations Manager ile tümleşmi?
Aracılardan Azure Monitör Günlükleri'ne veri toplamak için mevcut Sistem Merkezi Operasyon Yöneticisi yönetim grubunuzu Azure Monitor'a bağlayabilirsiniz. Bu, aracılardan toplanan verileri çözümlemek için günlük sorgularını ve çözümlerini kullanmanıza olanak tanır. Ayrıca, verileri doğrudan Azure Monitor'a gönderecek şekilde varolan System Center Operations Manager aracılarını yapılandırabilirsiniz. Bkz. [İşlem yöneticisini Azure Monitörüne Bağla.](platform/om-agents.md)

### <a name="what-ip-addresses-does-azure-monitor-use"></a>Azure Monitor hangi IP adreslerini kullanır?
Aracıların ve diğer dış kaynakların Azure Monitor'a erişmesi için gerekli IP adreslerinin ve bağlantı noktalarının listesi için [Application Insights ve Log Analytics tarafından kullanılan IP adreslerine](app/ip-addresses.md) bakın. 

## <a name="monitoring-data"></a>Verileri izleme

### <a name="where-does-azure-monitor-get-its-data"></a>Azure Monitor verilerini nereden alır?
Azure Monitor, Azure platformundan günlükler ve ölçümler, özel uygulamalar ve sanal makinelerde çalışan aracılar dahil olmak üzere çeşitli kaynaklardan veri toplar. Azure Güvenlik Merkezi ve Ağ İzleyicisi gibi diğer hizmetler, Azure Monitor verileriyle analiz edilebilmek için bir Log Analytics çalışma alanında veri toplar. Günlükler veya ölçümler için REST API'sini kullanarak Azure Monitor'a özel veriler de gönderebilirsiniz. Bkz. [Azure Monitor için izleme verikaynakları.](platform/data-sources.md)

### <a name="what-data-is-collected-by-azure-monitor"></a>Azure Monitor tarafından hangi veriler toplanır? 
Azure Monitor, çeşitli kaynaklardan [günlüklere](platform/data-platform-logs.md) veya [ölçümlere](platform/data-platform-metrics.md)veri toplar. Her veri türünün kendi göreli avantajları vardır ve her biri Azure Monitor'da belirli bir özellik kümesini destekler. Her Azure aboneliği için tek bir ölçüm veritabanı vardır ve gereksinimlerinize bağlı olarak günlükleri toplamak için birden çok Log Analytics çalışma alanı oluşturabilirsiniz. Bkz. [Azure Monitör veri platformu.](platform/data-platform.md)

### <a name="is-there-a-maximum-amount-of-data-that-i-can-collect-in-azure-monitor"></a>Azure Monitor'da toplayabileceğim maksimum veri miktarı var mı?
Toplayabileceğiniz metrik veri miktarı için bir sınır yoktur, ancak bu veriler en fazla 93 gün saklanır. Bkz. [Ölçümlerin Bekletmesi.](platform/data-platform-metrics.md#retention-of-metrics) Toplayabileceğiniz günlük verisi miktarında bir sınır yoktur, ancak Log Analytics çalışma alanı için seçtiğiniz fiyatlandırma katmanından etkilenebilir. [Fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/monitor/)bakın.

### <a name="how-do-i-access-data-collected-by-azure-monitor"></a>Azure Monitor tarafından toplanan verilere nasıl erişebilirim?
Öngörüler ve çözümler, Azure Monitor'da depolanan verilerle çalışmak için özel bir deneyim sağlar. Kusto Query Language (KQL) ile yazılmış bir günlük sorgusu kullanarak günlük verileriyle doğrudan çalışabilirsiniz. Azure portalında, Log Analytics'i kullanarak sorgular yazabilir ve çalıştırabilir ve verileri etkileşimli olarak analiz edebilirsiniz. Azure portalındaki ölçümleri Metrics Explorer ile analiz edin. Bkz. [Azure Monitor'da günlük verilerini analiz](log-query/log-query-overview.md) et ve Azure [Ölçümleri Gezgini ile başlarken.](platform/metrics-getting-started.md)

## <a name="solutions-and-insights"></a>Çözümler ve öngörüler

### <a name="what-is-an-insight-in-azure-monitor"></a>Azure Monitor'da öngörü nedir?
Öngörüler, belirli Azure hizmetleri için özelleştirilmiş bir izleme deneyimi sağlar. Azure Monitor'daki diğer özelliklerle aynı ölçümleri ve günlükleri kullanırlar, ancak ek veri toplayabilir ve Azure portalında benzersiz bir deneyim sağlayabilirler. [Azure Monitöründe Öngörüler'e](insights/insights-overview.md)bakın.

Azure portalındaki öngörüleri görüntülemek için **Monitör** menüsünün **Öngörüler** bölümüne veya hizmet menüsünün **İzleme** bölümüne bakın.

### <a name="what-is-a-solution-in-azure-monitor"></a>Azure Monitor'da çözüm nedir?
İzleme çözümleri, Azure Monitor özelliklerine dayalı olarak belirli bir uygulamayı veya hizmeti izlemek için paketlenmiş mantık kümeleridir. Azure Monitor'da günlük verileri toplarlar ve Azure portalında ortak bir deneyim kullanarak analizleri için günlük sorguları ve görünümler sağlarlar. Bkz. [Azure Monitor'da İzleme çözümleri.](insights/solutions.md)

Azure portalındaki çözümleri görüntülemek **için, Monitör** menüsünün **Öngörüler** bölümünde **Daha Fazla'yı** tıklatın. Çalışma alanına ek çözümler eklemek için **Ekle'yi** tıklatın.

## <a name="logs"></a>Günlükler

### <a name="whats-the-difference-between-azure-monitor-logs-and-azure-data-explorer"></a>Azure Monitör Günlükleri ile Azure Veri Gezgini arasındaki fark nedir?
Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Monitör Günlükleri, Azure Veri Gezgini'nin üzerine kuruludur ve bazı küçük farklılıklarla aynı Kusto Sorgu Dilini (KQL) kullanır. Bkz. [Azure Monitor günlük sorgu dil farklılıkları.](log-query/data-explorer-difference.md)

### <a name="how-do-i-retrieve-log-data"></a>Günlük verilerini nasıl atarım?
Tüm veriler, Kusto Query Language (KQL) kullanılarak yazılmış bir günlük sorgusu kullanılarak Bir Log Analytics çalışma alanından alınır. Kendi sorgularınızı yazabilir veya belirli bir uygulama veya hizmet için günlük sorgularını içeren çözümleri ve öngörüleri kullanabilirsiniz. Bkz. [Azure Monitor'daki günlük sorgularına genel bakış.](log-query/log-query-overview.md)

### <a name="what-is-a-log-analytics-workspace"></a>Log Analytics çalışma alanı nedir?
Azure Monitor tarafından toplanan tüm günlük verileri bir Log Analytics çalışma alanında depolanır. Çalışma alanı, günlük verilerinin çeşitli kaynaklardan toplandığı bir kapsayıcıdır. Tüm izleme verileriniz için tek bir Log Analytics çalışma alanınız olabilir veya birden çok çalışma alanı için gereksinimleriniz olabilir. Bkz. [Azure Monitör Günlükleri dağıtımınızı tasarlama.](platform/design-logs-deployment.md)

### <a name="can-you-move-an-existing-log-analytics-workspace-to-another-azure-subscription"></a>Varolan bir Log Analytics çalışma alanını başka bir Azure aboneliğine taşıyabilir misiniz?
Çalışma alanını kaynak grupları veya abonelikler arasında taşıyabilir, ancak farklı bir bölgeye taşıyamazsınız. Bkz. [Günlük Analizi çalışma alanını farklı abonelik veya kaynak grubuna taşıyın.](platform/move-workspace.md)

### <a name="why-cant-i-see-query-explorer-and-save-buttons-in-log-analytics"></a>Log Analytics'te Sorgu Gezgini ve Kaydet düğmelerini neden göremiyorum?

**Sorgu Sorgusu Gezgini**, **Kaydet** ve **Yeni uyarı kuralı** düğmeleri, sorgu [kapsamı](log-query/scope.md) belirli bir kaynağa ayarlandığında kullanılamaz. Uyarı oluşturmak, sorgu kaydetmek veya yüklemek için Log Analytics'in bir çalışma alanına kapsamı nın oluşturulması gerekir. Çalışma alanı bağlamında Günlük Analizi'ni açmak için **Azure Monitor** menüsünden **Günlükler'i** seçin. En son kullanılan çalışma alanı seçilir, ancak başka bir çalışma alanı seçebilirsiniz. [Azure Monitör Günlük Analizi'nde Günlük sorgu kapsamı ve zaman aralığına](log-query/scope.md) bakın

### <a name="why-am-i-getting-the-error-register-resource-provider-microsoftinsights-for-this-subscription-to-enable-this-query-when-opening-log-analytics-from-a-vm"></a>Neden hata alıyorum: "Bu sorguyu etkinleştirmek için kaynak sağlayıcısı 'Microsoft.Insights'ı bu sorguyu etkinleştirmek için kaydedin" vm'den Log Analytics'i açarken? 
Birçok kaynak sağlayıcısı otomatik olarak kaydedilir, ancak bazı kaynak sağlayıcılarını el ile kaydetmeniz gerekebilir. Kayıt kapsamı her zaman aboneliktir. Daha fazla bilgi için bkz. [Kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal).

### <a name="why-am-i-am-getting-no-access-error-message-when-opening-log-analytics-from-a-vm"></a>Log Analytics'i VM'den açarken neden erişim hatası iletisi alıyorum? 
VM Günlüklerini görüntülemek için, VM günlüklerini depolayan çalışma alanlarına okuma izni verilmesi gerekir. Bu gibi durumlarda, yöneticinizin Azure'daki izinleri size vermesi gerekir.

## <a name="alerts"></a>Uyarılar

### <a name="what-is-an-alert-in-azure-monitor"></a>Azure Monitor'da uyarı nedir?
İzleme verilerinizde önemli koşullar bulunduğunda uyarılar sizi proaktif olarak bildirir. Sisteminizin kullanıcıları bunları fark etmeden önce sorunları tanımlamanızı ve çözmenizi sağlar. Birden çok türde uyarı vardır:

- Metrik - Metrik değer bir eşiği aşıyor.
- Günlük sorgusu - Günlük sorgusunun sonuçları tanımlanan ölçütler ile eşleşir.
- Etkinlik günlüğü - Etkinlik günlüğü olay tanımlı ölçütleri eşleşir.
- Web testi - Kullanılabilirlik testi sonuçları tanımlanan kriterler le eşleşti.


Bkz. [Microsoft Azure'daki uyarılara genel bakış.](platform/alerts-overview.md)


### <a name="what-is-an-action-group"></a>Eylem grubu nedir?
Eylem grubu, bir uyarı tarafından tetiklenebilen bildirimler ve eylemler topluluğudur. Birden çok uyarı, ortak bildirim ve eylem kümelerinden yararlanmanıza olanak tanıyan tek bir eylem grubunu kullanabilir. Bkz. [Azure portalında eylem grupları oluşturma ve yönetme.](platform/action-groups.md)


### <a name="what-is-an-action-rule"></a>Eylem kuralı nedir?
Eylem kuralı, belirli bir ölçütle eşleşen bir uyarı kümesinin davranışını değiştirmenize olanak tanır. Bu, bakım penceresi sırasında uyarı eylemlerini devre dışı etme gibi gereksinimleri gerçekleştirmenize olanak tanır. Bir eylem grubunu doğrudan uyarı kurallarına uygulamak yerine bir dizi uyarıya da uygulayabilirsiniz. Bkz. [Eylem kuralları.](platform/alerts-action-rules.md)

## <a name="agents"></a>Aracılar

### <a name="does-azure-monitor-require-an-agent"></a>Azure Monitör'ün aracıya ihtiyacı var mı?
Aracının yalnızca işletim sisteminden ve sanal makinelerdeki iş yüklerinden veri toplaması gerekir. Sanal makineler Azure'da, başka bir bulut ortamında veya şirket içinde bulunabilir. Bkz. [Azure Monitörü aracılarının genel görünümü.](platform/agents-overview.md)


### <a name="whats-the-difference-between-the-azure-monitor-agents"></a>Azure Monitör aracıları arasındaki fark nedir?
Azure Tanı uzantısı Azure sanal makineleri içindir ve Azure Monitör Ölçümleri, Azure Depolama ve Azure Etkinlik Hub'larına veri toplar. Log Analytics aracısı, Azure'daki sanal makineler için, başka bir bulut ortamı veya şirket içi ve Azure Monitör Günlükleri için veri toplar. Bağımlılık aracısı, Log Analytics aracısını ve toplanan işlem ayrıntılarını ve bağımlılıklarını gerektirir. Bkz. [Azure Monitörü aracılarının genel görünümü.](platform/agents-overview.md)


### <a name="does-my-agent-traffic-use-my-expressroute-connection"></a>Aracı trafiğim ExpressRoute bağlantımı kullanıyor mu?
Azure Monitor'a trafik, Microsoft peering ExpressRoute devresini kullanır. Farklı ExpressRoute trafiğinin açıklaması için [ExpressRoute belgelerine](../expressroute/expressroute-faqs.md#supported-services) bakın. 

### <a name="how-can-i-confirm-that-the-log-analytics-agent-is-able-to-communicate-with-azure-monitor"></a>Log Analytics aracısının Azure Monitor ile iletişim kurabileceğini nasıl doğrulayabilirim?
Aracı bilgisayarındaki Denetim Masası'ndan **Güvenlik & Ayarları**, Microsoft **Monitoring Agent'ı** seçin. Azure **Günlük Analizi (OMS)** sekmesi altında, yeşil onay işareti simgesi aracının Azure Monitor ile iletişim kurabildiğini doğrular. Sarı uyarı simgesi, aracının sorunları olduğu anlamına gelir. Yaygın nedenlerden **biri, Microsoft İzleme Aracısı** hizmetinin durmuş olmasıdır. Hizmeti yeniden başlatmak için hizmet denetim yöneticisini kullanın.

### <a name="how-do-i-stop-the-log-analytics-agent-from-communicating-with-azure-monitor"></a>Log Analytics aracısının Azure Monitor ile iletişim kurmasını nasıl durdurabilirim?
Log Analytics'e doğrudan bağlı aracılar için Kontrol Panelini açın ve **Güvenlik & Ayarları**, Microsoft **Monitoring Agent'ı**seçin. Azure **Günlük Analizi (OMS)** sekmesi altında listelenen tüm çalışma alanlarını kaldırın. System Center Operations Manager'da bilgisayarı Log Analytics yönetilen bilgisayarlar listesinden kaldırın. Operations Manager, artık Log Analytics'e rapor vermek için aracının yapılandırmasını güncelleştirir. 

### <a name="how-much-data-is-sent-per-agent"></a>Aracı başına ne kadar veri gönderilir?
Aracı başına gönderilen veri miktarı aşağıdakilere bağlıdır:

* Etkinleştirdiğiniz çözümler
* Toplanan günlük lerin ve performans sayaçlarının sayısı
* Günlüklerde veri hacmi

Ayrıntılar [için Azure Monitör Günlükleri ile kullanımı ve maliyetleri yönet'e](platform/manage-cost-storage.md) bakın.

WireData aracısını çalıştırabilen bilgisayarlar için, ne kadar veri gönderildiğini görmek için aşağıdaki sorguyu kullanın:

```Kusto
WireData
| where ProcessName == "C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe" 
| where Direction == "Outbound" 
| summarize sum(TotalBytes) by Computer 
```

### <a name="how-much-network-bandwidth-is-used-by-the-microsoft-management-agent-mma-when-sending-data-to-azure-monitor"></a>Azure Monitor'a veri gönderirken Microsoft Management Agent (MMA) tarafından ne kadar ağ bant genişliği kullanılır?
Bant genişliği gönderilen veri miktarı üzerinde bir işlevdir. Veriler ağ üzerinden gönderildiğinde sıkıştırılır.


### <a name="how-can-i-be-notified-when-data-collection-from-the-log-analytics-agent-stops"></a>Log Analytics aracısından veri toplama durduğunda nasıl bilgilendirilebilirim?

Veri toplama durduğunda bildirilmek üzere [yeni bir günlük uyarısı oluşturmada](platform/alerts-metric.md) açıklanan adımları kullanın. Uyarı kuralı için aşağıdaki ayarları kullanın:

- **Uyarı koşulunu tanımlayın**: Günlük Analizi çalışma alanınızı kaynak hedefi olarak belirtin.
- **Uyarı ölçütleri** 
   - **Sinyal Adı**: *Özel günlük arama*
   - **Arama sorgusu**:`Heartbeat | summarize LastCall = max(TimeGenerated) by Computer | where LastCall < ago(15m)`
   - **Uyarı mantığı**: *Sonuç sayısına* **göre** , **Koşul** *Büyük*, **Eşik değeri** *0*
   - **Göre değerlendirilir**: **Dönem (dakika içinde)** *30*, **Frekans (dakika içinde)** *10*
- **Uyarı ayrıntılarını tanımlama** 
   - **Adı**: *Veri toplama durduruldu*
   - **Önem derecesi**: *Uyarı*

Günlük uyarısı ölçütleri ile eşleştiğinde, 15 dakikadan uzun süredir kayıp bir kalp atışınız varsa size bildirilen varolan veya yeni bir [Eylem Grubu](platform/action-groups.md) belirtin.


### <a name="what-are-the-firewall-requirements-for-azure-monitor-agents"></a>Azure Monitor aracıları için güvenlik duvarı gereksinimleri nelerdir?
Güvenlik duvarı gereksinimleriyle ilgili ayrıntılar için [Ağ güvenlik duvarı gereksinimlerine](platform/log-analytics-agent.md#firewall-requirements)bakın.


## <a name="visualizations"></a>Görsel öğeler

### <a name="why-cant-i-see-view-designer"></a>Görünüm Tasarımcısı'nı neden göremiyorum?

View Designer yalnızca Katılımcı izinleri veya Daha yüksek olan Log Analytics çalışma alanında atanan kullanıcılar için kullanılabilir.

## <a name="application-insights"></a>Application Insights

### <a name="configuration-problems"></a>Yapılandırma sorunları
*Ben kurmakta sorun yaşıyorum benim:*

* [.NET uygulaması](app/asp-net-troubleshoot-no-data.md)
* [Zaten çalışan bir uygulamayı izleme](app/monitor-performance-live-website-now.md#troubleshoot)
* [Azure tanılama](platform/diagnostics-extension-to-application-insights.md)
* [Java web uygulaması](app/java-troubleshoot.md)

*Sunucumdan veri alamıyorum*

* [Güvenlik duvarı özel durumlarını ayarlama](app/ip-addresses.md)
* [ASP.NET sunucusu ayarlama](app/monitor-performance-live-website-now.md)
* [Java sunucusu ayarlama](app/java-agent.md)

### <a name="can-i-use-application-insights-with-"></a>Application Insights'ı ...?

* [Azure VM veya Azure sanal makine ölçeğinde bir IIS sunucusundaki web uygulamaları](app/azure-vm-vmss-apps.md)
* [Bir IIS sunucusunda web uygulamaları - şirket içinde veya VM'de](app/asp-net.md)
* [Java web uygulamaları](app/java-get-started.md)
* [Node.js uygulamaları](app/nodejs.md)
* [Azure'da Web uygulamaları](app/azure-web-apps.md)
* [Azure'da Bulut Hizmetleri](app/cloudservices.md)
* [Docker'da çalışan uygulama sunucuları](app/docker.md)
* [Tek sayfalık web uygulamaları](app/javascript.md)
* [SharePoint](app/sharepoint.md)
* [Windows masaüstü uygulaması](app/windows-desktop.md)
* [Diğer platformlar](app/platforms.md)

### <a name="is-it-free"></a>Bedava mı?

Evet, deneysel kullanım için. Temel fiyatlandırma planında, uygulamanız her ay belirli bir veri ödeneğini ücretsiz olarak gönderebilir. Ücretsiz ödenek, geliştirmeyi ve az sayıda kullanıcı için bir uygulama yayımlamayı kapsayacak kadar büyüktür. Belirli bir miktardan fazla verinin işlenmesini önlemek için bir kapak ayarlayabilirsiniz.

Daha büyük hacimlerde telemetri Gb tarafından ücretlendirilir. [Ücretlerinizi](app/pricing.md)sınırlamakonusunda bazı ipuçları salıyoruz.

Enterprise planı, her web sunucusu düğümünün telemetri gönderdiği her gün için bir ücrete tabidir. Sürekli Dışa Aktarma'yı büyük ölçekte kullanmak istiyorsanız uygundur.

[Fiyatlandırma planını okuyun.](https://azure.microsoft.com/pricing/details/application-insights/)

### <a name="how-much-does-it-cost"></a>Fiyatı nedir?

* Uygulama Öngörüleri kaynağında **Kullanım ve tahmini maliyetler sayfasını** açın. Son kullanım şeması var. İsterseniz bir veri hacmi kapağı ayarlayabilirsiniz.
* Faturalarınızı tüm kaynaklarda görmek için [Azure Faturalandırma bıçağını](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview) açın.

### <a name="what-does-application-insights-modify-in-my-project"></a><a name="q14"></a>Uygulama Öngörüleri projemde neyi değiştirir?
Ayrıntılar projenin türüne bağlıdır. Bir web uygulaması için:

* Bu dosyaları projenize ekler:
  * ApplicationInsights.config
  * ai.js
* Bu NuGet paketlerini yükler:
  * *Uygulama Öngörüleri API* - çekirdek API
  * *Web Uygulamaları için Uygulama Öngörüleri API* - sunucudan telemetri göndermek için kullanılan
  * *JavaScript Uygulamaları için Uygulama Öngörüleri API* - istemciden telemetri göndermek için kullanılan
* Paketler şu derlemeleri içerir:
  * Microsoft.ApplicationInsights
  * Microsoft.ApplicationInsights.Platform
* Öğeleri şu şekilde ekler:
  * Web.config
  * packages.config
* (Yalnızca yeni projeler - [Varolan bir projeye Uygulama Öngörüleri eklerseniz,][start]bunu el ile yapmanız gerekir.) Uygulama Öngörüleri kaynak kimliğiyle bunları başlatmak için istemci ve sunucu koduna parçacıklar ekler. Örneğin, bir MVC uygulamasında, kod ana sayfaya eklenir Görünümler/Paylaşılan/\_Layout.cshtml

### <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Eski SDK sürümlerinden nasıl yükseltebilirim?
Uygulama türünüze uygun SDK'nın [sürüm notlarına](app/release-notes.md) bakın.

### <a name="how-can-i-change-which-azure-resource-my-project-sends-data-to"></a><a name="update"></a>Projemin veri gönderdiği Azure kaynağını nasıl değiştirebilirim?
Solution Explorer'da, `ApplicationInsights.config` Uygulama **Öngörülerini Güncelleştir'e**sağ tıklayın ve seçin. Verileri Azure'da varolan veya yeni bir kaynağa gönderebilirsiniz. Güncelleştirme sihirbazı, Sunucu SDK'nın verilerinizi nereye gönderdiğini belirleyen ApplicationInsights.config'deki enstrümantasyon anahtarını değiştirir. "Tümünü güncelleştir" seçeneğini değiştirmediğiniz sürece, web sayfalarınızda görünen anahtarı da değiştirir.

### <a name="what-is-status-monitor"></a>Durum İzleyicisi nedir?

IIS web sunucunuzda kullanabileceğiniz bir masaüstü uygulaması, web uygulamalarında Uygulama Öngörüleri'ni yapılandırmaya yardımcı olur. Telemetri toplamaz: bir uygulamayı yapılandırmadığınızda durdurabilirsiniz. 

[Daha fazla bilgi edinin](app/monitor-performance-live-website-now.md#questions).

### <a name="what-telemetry-is-collected-by-application-insights"></a>Uygulama Öngörüleri tarafından hangi telemetri toplanır?

Sunucu web uygulamalarından:

* HTTP istekleri
* [Bağımlılıklar](app/asp-net-dependencies.md). Aramalar: SQL Veritabanları; HTTP'nin harici hizmetlere çağrıları; Azure Cosmos DB, tablo, blob depolama ve kuyruk. 
* [Özel durumlar](app/asp-net-exceptions.md) ve yığın izleri.
* [Performans Sayaçları](app/performance-counters.md) - [Durum İzleme](app/monitor-performance-live-website-now.md), [Uygulama Hizmetleri için Azure izleme,](app/azure-web-apps.md) [VM veya sanal makine ölçeği kümesi için Azure izleme veya](app/azure-vm-vmss-apps.md)Application [Insights toplanan yazar](app/java-collectd.md)kullanıyorsanız.
* Kodladığınız [özel olaylar ve ölçümler.](app/api-custom-events-metrics.md)
* Uygun toplayıcıyı yapılandırırsanız, [izleme günlükleri.](app/asp-net-trace-logs.md)

[İstemci web sayfalarından:](app/javascript.md)

* [Sayfa görüntüleme sayıları](app/usage-overview.md)
* [AJAX aramaları](app/asp-net-dependencies.md) Çalışan bir komut dosyasından yapılan istekler.
* Sayfa görünümü yük verileri
* Kullanıcı ve oturum sayıları
* [Kimlik doğrulaması kullanıcı kimlikleri](app/api-custom-events-metrics.md#authenticated-users)

Diğer kaynaklardan, bunları yapılandırarak:

* [Azure tanılama](platform/diagnostics-extension-to-application-insights.md)
* [Analytics'e İthalat](platform/data-collector-api.md)
* [Günlük Analizi](platform/data-collector-api.md)
* [Logstash](platform/data-collector-api.md)

### <a name="can-i-filter-out-or-modify-some-telemetry"></a>Bazı telemetrileri filtreleyebilir veya değiştirebilir miyim?

Evet, sunucuda şunları yazabilirsiniz:

* Telemetri İşlemcisi, uygulamanızdan gönderilmeden önce seçili telemetri öğelerine filtre leme veya özellikler eklemek için.
* Telemetri Initializer telemetri tüm öğeleri özellikleri eklemek için.

[ASP.NET](app/api-filtering-sampling.md) veya [Java](app/java-filter-telemetry.md)için daha fazla bilgi edinin.

### <a name="how-are-city-countryregion-and-other-geo-location-data-calculated"></a>Şehir, ülke/bölge ve diğer coğrafi konum verileri nasıl hesaplanır?

[GeoLite2](https://dev.maxmind.com/geoip/geoip2/geolite2/)kullanarak web istemcisinin IP adresini (IPv4 veya IPv6) alıyoruz.

* Tarayıcı telemetrisi: Gönderenin IP adresini toplarız.
* Sunucu telemetrisi: Application Insights modülü istemci IP adresini toplar. Ayarlanırsa `X-Forwarded-For` toplanmaz.
* Uygulama Öngörüleri'nde IP adresi ve coğrafi konum verilerinin nasıl toplandığı hakkında daha fazla bilgi edinmek için bu [makaleye](https://docs.microsoft.com/azure/azure-monitor/app/ip-collection)bakın.


IP adresini farklı `ClientIpHeaderTelemetryInitializer` bir üstbilgiden alacak şekilde yapılandırabilirsiniz. Bazı sistemlerde, örneğin, bir proxy, yük dengeleyici veya CDN tarafından `X-Originating-IP`taşınır. [Daha fazla bilgi edinin](https://apmtips.com/blog/2016/07/05/client-ip-address/).

İstek telemetrinizi haritaüzerinde görüntülemek için [Power BI'yi kullanabilirsiniz.](app/export-power-bi.md )


### <a name="how-long-is-data-retained-in-the-portal-is-it-secure"></a><a name="data"></a>Veriler portalda ne kadar süreyle saklanır? Güvenli mi?
Veri Saklama [ve Gizlilik'e][data]bir göz atın.

### <a name="what-happens-to-application-insights-telemetry-when-a-server-or-device-loses-connection-with-azure"></a>Bir sunucu veya aygıt Azure ile bağlantısını kaybettiğinde Application Insight'ın telemetrisine ne olur?

Web SDK de dahil olmak üzere tüm SDK'larımız "güvenilir ulaşım" veya "sağlam taşıma" içerir. Sunucu veya aygıt Azure ile bağlantısını kaybettiğinde, telemetri yerel olarak dosya sisteminde (Sunucu SDK'ları) veya HTML5 Oturum Depolama 'nda (Web SDK) [depolanır.](https://docs.microsoft.com/azure/azure-monitor/app/data-retention-privacy#does-the-sdk-create-temporary-local-storage) SDK periyodik olarak bizim yutma hizmeti "bayat" (günlükleri için 48 saat, ölçümler için 30 dakika) dikkate kadar bu telemetri göndermek için yeniden çalışacağız. Bayat telemetri düşecek. Yerel depolama alanının dolu olması gibi bazı durumlarda yeniden deneme yapılmaz.


### <a name="could-personal-data-be-sent-in-the-telemetry"></a>Kişisel veriler telemetride gönderilebilir mi?

Kodunuz bu tür verileri gönderirse bu mümkündür. Yığın izlerinde değişkenler kişisel verileri içeriyorsa da gerçekleşebilir. Geliştirme ekibiniz, kişisel verilerin düzgün bir şekilde işlendiğinden emin olmak için risk değerlendirmeleri yapmalıdır. [Veri saklama ve gizlilik hakkında daha fazla bilgi edinin.](app/data-retention-privacy.md)

Coğrafi konum öznitelikleri arandıktan sonra istemci web adresinin **tüm** sekizlileri her zaman 0 olarak ayarlanır.

### <a name="my-instrumentation-key-is-visible-in-my-web-page-source"></a>Enstrümantasyon Anahtarım web sayfa kaynağımda görülebilir. 

* Bu, çözümleri izlemede yaygın bir uygulamadır.
* Verilerinizi çalmak için kullanılamaz.
* Verilerinizi çarpıtmak veya uyarıları tetiklemek için kullanılabilir.
* Biz herhangi bir müşteri böyle sorunlar olduğunu duymadım.

Şunları yapabilirsiniz:

* İstemci ve sunucu verileri için iki ayrı Enstrümantasyon Anahtarı (ayrı Application Insights kaynakları) kullanın. Veya
* Sunucunuzda çalışan bir proxy yazın ve web istemcisinin bu proxy üzerinden veri göndermesini sorun.

### <a name="how-do-i-see-post-data-in-diagnostic-search"></a><a name="post"></a>Tanılama aramasında POST verilerini nasıl görebiliyorum?
POST verilerini otomatik olarak kaydetmeyiz, ancak TrackTrace araması kullanabilirsiniz: verileri ileti parametreye koyun. Bu, dize özelliklerindeki sınırlardan daha uzun bir boyut sınırına sahiptir, ancak üzerinde filtre uygulayamazsınız.

### <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Tek veya birden çok Application Insights kaynağı kullanmalı mıyım?

Tek bir iş sistemindeki tüm bileşenler veya roller için tek bir kaynak kullanın. Geliştirme, sınama ve sürüm sürümleri ve bağımsız uygulamalar için ayrı kaynaklar kullanın.

* [Tartışmayı buradan görebilirsiniz](app/separate-resources.md)
* [Örnek - çalışan ve web rolleri ile bulut hizmeti](app/cloudservices.md)

### <a name="how-do-i-dynamically-change-the-instrumentation-key"></a>Enstrümantasyon anahtarını dinamik olarak nasıl değiştirebilirim?

* [Tartışma burada](app/separate-resources.md)
* [Örnek - çalışan ve web rolleri ile bulut hizmeti](app/cloudservices.md)

### <a name="what-are-the-user-and-session-counts"></a>Kullanıcı ve Oturum sayıları nelerdir?

* JavaScript SDK, geri dönen kullanıcıları belirlemek için web istemcisine bir kullanıcı çerezi ve grup etkinlikleri için bir oturum çerezi ayarlar.
* İstemci tarafı komut dosyası yoksa, [sunucuda tanımlama bilgileri](https://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/)ayarlayabilirsiniz.
* Gerçek bir kullanıcı sitenizi farklı tarayıcılarda veya özel/gizli tarama veya farklı makineler kullanıyorsa, birden fazla kez sayılır.
* Makineler ve tarayıcılar arasında oturum açmış bir kullanıcıyı tanımlamak için [AuthenticatedUserContext() 'yi](app/api-custom-events-metrics.md#authenticated-users)ayarla'ya bir çağrı ekleyin.

### <a name="have-i-enabled-everything-in-application-insights"></a><a name="q17"></a>Uygulama Öngörüleri'nde her şeyi etkinleştirdim mi?
| Görmeniz gereken | Nasıl elde etmek için | Neden istediğinizi |
| --- | --- | --- |
| Kullanılabilirlik grafikleri |[Web testleri](app/monitor-web-app-availability.md) |Web uygulamanızın dolduğunu bilin |
| Sunucu uygulaması perf: yanıt süreleri, ... |[Projenize Uygulama Öngörüleri ekleme](app/asp-net.md) veya [sunucuya AI Durum Monitörü yükleme](app/monitor-performance-live-website-now.md) (veya [bağımlılıkları izlemek](app/api-custom-events-metrics.md#trackdependency)için kendi kodunuzu yazın) |Perf sorunlarını algılama |
| Bağımlılık telemetrisi |[Sunucuya AI Durum Monitörü yükleme](app/monitor-performance-live-website-now.md) |Veritabanları veya diğer dış bileşenlerle ilgili sorunları tanılama |
| Özel durumlardan yığın izlemeleri alma |[Kodunuzda TrackException çağrıları ekleme](app/asp-net-exceptions.md) (ancak bazıları otomatik olarak bildirilir) |Özel durumları algılama ve tanılama |
| Arama günlüğü izleri |[Günlük bağdaştırıcısı ekleme](app/asp-net-trace-logs.md) |Özel durumları, perf sorunlarını tanıla |
| İstemci kullanım temelleri: sayfa görünümleri, oturumlar, ... |[Web sayfalarında JavaScript başharf](app/javascript.md) |Kullanım analizi |
| İstemci özel ölçümleri |[Web sayfalarındaki aramaları izleme](app/api-custom-events-metrics.md) |Kullanıcı deneyimini geliştirin |
| Sunucu özel ölçümleri |[Sunucudaki aramaları izleme](app/api-custom-events-metrics.md) |İş zekası |

### <a name="why-are-the-counts-in-search-and-metrics-charts-unequal"></a>Arama ve Ölçümler grafiklerinde sayımlar neden eşit değil?

[Örnekleme,](app/sampling.md) uygulamanızdan portala gerçekte gönderilen telemetri öğesi (istekler, özel olaylar vb.) sayısını azaltır. Arama'da, gerçekte alınan öğe sayısını görürsünüz. Olay sayısını görüntüleyen metrik grafiklerde, oluşan özgün olayların sayısını görürsünüz. 

Aktarılan her öğe, öğenin kaç orijinal olayı temsil ettiğini gösteren bir `itemCount` özellik taşır. İşlemde örneklemeyi gözlemlemek için bu sorguyabilirsiniz:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


### <a name="automation"></a>Otomasyon

#### <a name="configuring-application-insights"></a>Uygulama Öngörülerini Yapılandırma

Aşağıdakiler için Azure Kaynak Monitörünü kullanarak [PowerShell komut dosyaları yazabilirsiniz:](app/powershell.md)

* Application Insights kaynaklarını oluşturun ve güncelleyin.
* Fiyatlandırma planını ayarlayın.
* Enstrümantasyon tuşunu al.
* Metrik uyarı ekleyin.
* Kullanılabilirlik testi ekleyin.

Metrik Explorer raporu ayarlayamaz veya sürekli dışa aktarma ayarlayamazsınız.

#### <a name="querying-the-telemetry"></a>Telemetriyi sorgulama

[Analytics](app/analytics.md) sorgularını çalıştırmak için [REST API'sini](https://dev.applicationinsights.io/) kullanın.

### <a name="how-can-i-set-an-alert-on-an-event"></a>Bir etkinlikte nasıl uyarı ayarlayabilirim?

Azure uyarıları yalnızca ölçümlerde dir. Etkinliğiniz her gerçekleştiğinde değer eşiğini aşan özel bir metrik oluşturun. Ardından metrikte bir uyarı ayarlayın. Metrik her iki yönde de eşiği geçtiğinde bir bildirim alırsınız; ilk geçişe kadar, ilk değer yüksek veya düşük olsun, bildirim almazsınız; her zaman birkaç dakika gecikmesi vardır.

### <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Azure web uygulaması ile Application Insights arasında veri aktarım ücretleri var mı?

* Azure web uygulamanız, Application Insights toplama bitiş noktasının bulunduğu bir veri merkezinde barındırılıyorsa, ücret alınmaz. 
* Ana bilgisayar veri merkezinizde toplama bitiş noktası yoksa, uygulamanızın telemetrisi [Azure giden ücretlerine](https://azure.microsoft.com/pricing/details/bandwidth/)neden olur.

Bu, Uygulama Öngörüleri kaynağınızın nerede barındırıldığına bağlı değildir. Bu sadece uç noktalarımızın dağılımına bağlı.

### <a name="can-i-send-telemetry-to-the-application-insights-portal"></a>Uygulama Öngörüleri portalına telemetri gönderebilir miyim?

SDK'larımızı kullanmanızı ve [SDK API'yi](app/api-custom-events-metrics.md)kullanmanızı öneririz. Çeşitli [platformlar](app/platforms.md)için SDK varyantları vardır. Bu SDK'lar arabelleğe alma, sıkıştırma, azaltma, yeniden deneme ve benzeri işler. Ancak, [yutma şeması](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) ve [bitiş noktası protokolü](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) herkese açıktır.

### <a name="can-i-monitor-an-intranet-web-server"></a>Bir intranet web sunucusuizleyebilir miyim?

Evet, ancak güvenlik duvarı özel durumları veya proxy yönlendirmeleri ile hizmetlerimize trafik izni niz gerekir.
- QuickPulse`https://rt.services.visualstudio.com:443` 
- ApplicationIdProvider`https://dc.services.visualstudio.com:443` 
- TelemetryKanal`https://dc.services.visualstudio.com:443` 


Hizmetlerin ve IP adreslerinin tam listesini [buradan](app/ip-addresses.md)inceleyin.

#### <a name="firewall-exception"></a>Güvenlik duvarı özel durumu

Web sunucunuzun telemetriyi uç noktalarımıza göndermesine izin verin. 

#### <a name="gateway-redirect"></a>Ağ geçidi yönlendirme

Yapılandırmanızda Uç Noktaları'nı üzerine yazarak trafiği sunucunuzdan intranetinizdeki bir ağ geçidine yönlendirin. Bu "Uç Nokta" özellikleri config'inizde yoksa, bu sınıflar ApplicationInsights.config örneğinde aşağıda gösterilen varsayılan değerleri kullanır. 

Ağ geçidiniz trafiği bitiş noktamızın temel adresine yönlendirmelidir. Yapılandırmanızda varsayılan değerleri `http://<your.gateway.address>/<relative path>`.


##### <a name="example-applicationinsightsconfig-with-default-endpoints"></a>Örnek ApplicationInsights.config varsayılan uç noktaları ile:
```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>https://rt.services.visualstudio.com/QuickPulseService.svc</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>https://dc.services.visualstudio.com/v2/track</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>https://dc.services.visualstudio.com/api/profiles/{0}/appId</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

> [!NOTE]
> ApplicationIdProvider v2.6.0'dan başlayarak kullanılabilir.



#### <a name="proxy-passthrough"></a>Proxy geçişi

Proxy geçişi, makine düzeyi veya uygulama düzeyi proxy'si yapılandırılarak elde edilebilir.
Daha fazla bilgi için [DefaultProxy](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/network/defaultproxy-element-network-settings)dotnet makalesine bakın.
 
 Örnek Web.config:
 ```xml
<system.net>
    <defaultProxy>
      <proxy proxyaddress="http://xx.xx.xx.xx:yyyy" bypassonlocal="true"/>
    </defaultProxy>
</system.net>
```
 

### <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Bir intranet sunucusunda Kullanılabilirlik web testlerini çalıştırabilir miyim?

[Web testlerimiz,](app/monitor-web-app-availability.md) dünya çapında dağıtılan varlık noktaları üzerinde çalışır. İki çözüm vardır:

* Güvenlik duvarı kapısı - Web [test aracılarının uzun ve değiştirilebilir listesinden](app/ip-addresses.md)sunucunuza istekler izin verin.
* Intranetinizin içinden sunucunuza periyodik istekler göndermek için kendi kodunuzu yazın. Bu amaçla Visual Studio web testleri çalıştırabilirsiniz. Test eden, TrackAvailability() API'sini kullanarak sonuçları Uygulama Öngörüleri'ne gönderebilir.

### <a name="how-long-does-it-take-for-telemetry-to-be-collected"></a>Telemetrinin toplanması ne kadar sürer?

Çoğu Uygulama Öngörüleri verisi 5 dakikanın altında bir gecikme süresine sahiptir. Bazı veriler daha uzun sürebilir; genellikle daha büyük günlük dosyaları. Daha fazla bilgi için [Uygulama Öngörüleri SLA'ya](https://azure.microsoft.com/support/legal/sla/application-insights/v1_2/)bakın.



<!--Link references-->

[data]: app/data-retention-privacy.md
[platforms]: app/platforms.md
[start]: app/app-insights-overview.md
[windows]: app/app-insights-windows-get-started.md


## <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici

Bu Microsoft SSS kapsayıcılar için Azure Monitor hakkında sık sorulan soruların bir listesidir. Çözüm le ilgili ek sorularınız varsa, [tartışma forumuna](https://feedback.azure.com/forums/34192--general-feedback) gidin ve sorularınızı gönderin. Bir soru sık sık sorulduğunda, hızlı ve kolay bir şekilde bulunabilmesi için bu makaleye ekliyoruz.

### <a name="what-does-other-processes-represent-under-the-node-view"></a>Düğüm görünümü altında *Diğer İşlemler* neyi temsil eder?

**Diğer işlemler,** düğümünüzdeki yüksek kaynak kullanımının temel nedenini açıkça anlamanıza yardımcı olmak için tasarlanmıştır. Bu, kullanımı kapsayıcılaştırılmış işlemler ile kapsayıcı olmayan işlemler arasında ayırt etmenizi sağlar.

Bu **Diğer Süreçler**Nelerdir? 

Bunlar düğümünüzde çalışan kapsayıcı olmayan işlemlerdir.  

Bunu nasıl hesaplıyoruz?

**Diğer İşlemler** =  - *Konteynerleştirilmiş işlemden* *CAdvisor Kullanımından toplam kullanım*

**Diğer süreçler** şunları içerir:

- Konteynersiz kubernetes kendi kendini yöneten veya yönetilen Kubernetes 

- Konteyner Çalışma zamanı süreçleri  

- Kubelet  

- Düğümünüzde çalışan sistem süreçleri 

- Düğüm donanımı veya VM üzerinde çalışan diğer Kubernetes dışı iş yükleri 

### <a name="i-dont-see-image-and-name-property-values-populated-when-i-query-the-containerlog-table"></a>ContainerLog tablosunu sorgularken Görüntü ve Ad özellik değerlerinin doldurulduğunu görmüyorum.

Aracı sürümü ciprod12042019 ve daha sonra için, varsayılan olarak bu iki özellik toplanan günlük verileri tahakkuk eden maliyeti en aza indirmek için her günlük satırı için doldurulur değildir. Tabloyu, değerleriyle bu özellikleri içeren sorgulamak için iki seçenek vardır:

#### <a name="option-1"></a>Seçenek 1 

Sonuçlara bu özellik değerlerini eklemek için diğer tabloları birleştirin.

Sorgularınızı, ContainerID özelliğine ```ContainerInventory``` katılarak tablodan Resim ve ImageTag özelliklerini içerecek şekilde değiştirin. KubepodInventory tablosunun ContaineName alanından, ContainerID özelliğine katılarak Ad özelliğini ```ContainerLog``` (tabloda daha önce göründüğü gibi) ekleyebilirsiniz. Bu önerilen seçenektir.

Aşağıdaki örnek, bu alan değerlerinin birleştirmelerle nasıl alınabildiğini açıklayan örnek ayrıntılı bir sorgudur.

```
//lets say we are querying an hour worth of logs
let startTime = ago(1h);
let endTime = now();
//below gets the latest Image & ImageTag for every containerID, during the time window
let ContainerInv = ContainerInventory | where TimeGenerated >= startTime and TimeGenerated < endTime | summarize arg_max(TimeGenerated, *)  by ContainerID, Image, ImageTag | project-away TimeGenerated | project ContainerID1=ContainerID, Image1=Image ,ImageTag1=ImageTag;
//below gets the latest Name for every containerID, during the time window
let KubePodInv  = KubePodInventory | where ContainerID != "" | where TimeGenerated >= startTime | where TimeGenerated < endTime | summarize arg_max(TimeGenerated, *)  by ContainerID2 = ContainerID, Name1=ContainerName | project ContainerID2 , Name1;
//now join the above 2 to get a 'jointed table' that has name, image & imagetag. Outer left is safer in-case there are no kubepod records are if they are latent
let ContainerData = ContainerInv | join kind=leftouter (KubePodInv) on $left.ContainerID1 == $right.ContainerID2;
//now join ContainerLog table with the 'jointed table' above and project-away redundant fields/columns and rename columns that were re-written
//Outer left is safer so you dont lose logs even if we cannot find container metadata for loglines (due to latency, time skew between data types etc...)
ContainerLog
| where TimeGenerated >= startTime and TimeGenerated < endTime 
| join kind= leftouter (
   ContainerData
) on $left.ContainerID == $right.ContainerID2 | project-away ContainerID1, ContainerID2, Name, Image, ImageTag | project-rename Name = Name1, Image=Image1, ImageTag=ImageTag1 

```

#### <a name="option-2"></a>2. Seçenek

Her kapsayıcı günlük satırı için bu özellikler için yeniden toplamayı etkinleştirin.

Sorgu değişiklikleri nedeniyle ilk seçenek uygun değilse, [veri toplama yapılandırma ayarlarında](insights/container-insights-agent-config.md)açıklandığı ```log_collection_settings.enrich_container_logs``` gibi aracı config haritasında ayar etkinleştirerek bu alanları toplamayı yeniden etkinleştirebilirsiniz.

> [!NOTE]
> Bu zenginleştirme gerçekleştirmek için kümedeki her düğümden API sunucu çağrıları oluşturduğundan, 50'den fazla düğümü olan büyük kümelerle ikinci seçenek önerilmez. Bu seçenek, toplanan her günlük satırı için veri boyutunu da artırır.

### <a name="can-i-view-metrics-collected-in-grafana"></a>Grafana'da toplanan ölçümleri görüntüleyebilir miyim?

Kapsayıcılar için Azure Monitor, Grafana panolarındaki Günlük Analizi çalışma alanınızda depolanan görüntüleme ölçümlerini destekler. Özel Grafana panolarında görselleştirmek için izlenen kümelerinizden ek verileri sorgulamayı öğrenmenize yardımcı olmak için Grafana'nın [pano deposundan](https://grafana.com/grafana/dashboards?dataSource=grafana-azure-monitor-datasource&category=docker) indirebileceğiniz bir şablon ve referans sağladık. 

### <a name="can-i-monitor-my-aks-engine-cluster-with-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Monitor ile AKS motor kümemi izleyebilir miyim?

Kapsayıcılar için Azure Monitor, Azure'da barındırılan AKS motoruna (eski adıyla ACS motoru) küme(ler) dağıtılan kapsayıcı iş yüklerinin izlenmesini destekler. Daha fazla ayrıntı ve bu senaryo için izlemeyi etkinleştirmek için gereken adımların genel görünümü [için AKS altyapısı için kapsayıcılar için Azure Monitörünü Kullanma bölümüne](https://github.com/microsoft/OMS-docker/tree/aks-engine)bakın.

### <a name="why-dont-i-see-data-in-my-log-analytics-workspace"></a>Log Analytics çalışma alanımda neden veri göremiyorum?

Günlük Analizi çalışma alanında her gün belirli bir zamanda herhangi bir veri göremiyorsanız, varsayılan 500 MB sınırına veya günlük toplanacak veri miktarını denetlemek için belirtilen günlük kap'a ulaşmış olabilirsiniz. Gün için sınır karşılandığında, veri toplama durur ve yalnızca ertesi gün devam eder. Veri kullanımınızı gözden geçirmek ve beklenen kullanım desenlerinizi temel alan farklı bir fiyatlandırma katmanına güncelleştirmek için Günlük [veri kullanımı ve maliyeti'ne](platform/manage-cost-storage.md)bakın. 

### <a name="what-are-the-container-states-specified-in-the-containerinventory-table"></a>ContainerInventory tablosunda belirtilen kapsayıcı durumları nelerdir?

ContainerInventory tablosu hem durduruldu hem de çalışan kapsayıcılar hakkında bilgi içerir. Tablo, tüm kapsayıcılar (çalışan ve durdurulan) için docker sorgulayan aracı içinde bir iş akışı tarafından doldurulur ve bu verileri Log Analytics çalışma alanı iletir.
 
### <a name="how-do-i-resolve-missing-subscription-registration-error"></a>Eksik Abonelik *kayıt* hatalarını nasıl çözebilirim?

**Microsoft.OperationsManagement için Eksik Abonelik kaydı**hatasını alırsanız, çalışma alanının tanımlandığı abonelikte kaynak sağlayıcısı **Microsoft.OperationsManagement'ı** kaydederek sorunu çözebilirsiniz. Bunun nasıl yapılacılailgili belgeleri [burada](../azure-resource-manager/templates/error-register-resource-provider.md)bulabilirsiniz.

### <a name="is-there-support-for-rbac-enabled-aks-clusters"></a>RBAC etkin AKS kümeleri için destek var mı?

Kapsayıcı İzleme çözümü RBAC'ı desteklemez, ancak Kapsayıcılar için Azure Monitörü ile desteklenir. Çözüm ayrıntıları sayfası, bu kümeler için veri gösteren bıçaklarda doğru bilgileri göstermeyebilir.

### <a name="how-do-i-enable-log-collection-for-containers-in-the-kube-system-namespace-through-helm"></a>Helm aracılığıyla kube-sistem ad alanında kapsayıcılar için günlük toplamayı nasıl etkinleştirebilirim?

Kube-sistem ad alanında kapsayıcılardan günlük toplama varsayılan olarak devre dışı bırakılır. Log toplama omsagent üzerinde bir ortam değişkeni ayarlayarak etkinleştirilebilir. Daha fazla bilgi [için, github kapları için Azure Monitörü](https://github.com/helm/charts/tree/master/incubator/azuremonitor-containers) sayfasına bakın. 

### <a name="how-do-i-update-the-omsagent-to-the-latest-released-version"></a>Omsagent'ı en son yayımlanan sürüme nasıl güncellerim?

Aracıyı nasıl yükselteceklerini öğrenmek için [Temsilci yönetimine](insights/container-insights-manage-agent.md)bakın.

### <a name="how-do-i-enable-multi-line-logging"></a>Çok satırlı günlüğe kaydetmeyi nasıl etkinleştirebilirim?

Şu anda kapsayıcılar için Azure Monitor çok satırlı günlüğe kaydetmeyi desteklemez, ancak geçici geçici çözüm kullanılabilir. Tüm hizmetleri JSON biçiminde yazmak üzere yapılandırabilirsiniz ve docker/Moby bunları tek bir satır olarak yazar.

Örneğin, bir örnek düğüm.js uygulaması için aşağıdaki örnekte gösterildiği gibi günlük ünüzü JSON nesnesi olarak sarabilirsiniz:

```
console.log(json.stringify({ 
      "Hello": "This example has multiple lines:",
      "Docker/Moby": "will not break this into multiple lines",
      "and you will receive":"all of them in log analytics",
      "as one": "log entry"
      }));
```

Bu veriler, günlükleri için Azure Monitor'da aşağıdaki örnek gibi görünür:

```
LogEntry : ({"Hello": "This example has multiple lines:","Docker/Moby": "will not break this into multiple lines", "and you will receive":"all of them in log analytics", "as one": "log entry"}

```

Konuya ayrıntılı bir bakış için aşağıdaki [GitHub bağlantısını](https://github.com/moby/moby/issues/22920)inceleyin.

### <a name="how-do-i-resolve-azure-ad-errors-when-i-enable-live-logs"></a>Canlı günlükleri etkinleştirdiğimde Azure REKLAM hatalarını nasıl çözebilirim? 

Aşağıdaki hatayı görebilirsiniz: **İstekte belirtilen yanıt url'si, uygulama için yapılandırılan yanıt url'leri ile eşleşmiyor: '<uygulama kimliği\>'**. Bunu çözmek için çözüm, [kapsayıcılar için Azure Monitor ile konteyner verilerinin gerçek zamanlı](insights/container-insights-livedata-setup.md#configure-ad-integrated-authentication)olarak nasıl görüntülenebilir makalesinde bulunabilir. 

### <a name="why-cant-i-upgrade-cluster-after-onboarding"></a>Uçağa bindikten sonra kümeyi neden yükseltemiyorum?

Bir AKS kümesi için kapsayıcılar için Azure Monitor'u etkinleştirdikten sonra, kümenin verilerini gönderdiği Günlük Analizi çalışma alanını silerseniz, kümeyi yükseltmeye çalışırken başarısız olur. Bu durumu aşmak için izlemeyi devre dışı bırakıp aboneliğinizde farklı bir geçerli çalışma alanına başvurmayı yeniden etkinleştirmeniz gerekir. Küme yükseltmesini yeniden gerçekleştirmeye çalıştığınızda, küme yükseltmeişlemini işlemeli ve başarıyla tamamlamalıdır.  

### <a name="which-ports-and-domains-do-i-need-to-openwhitelist-for-the-agent"></a>Aracı için hangi bağlantı noktalarını ve etki alanlarını açmam/beyaz listeye açmam gerekiyor?

Azure, Azure ABD Hükümeti ve Azure China 21Vianet bulutları içeren kapsayıcı aracı için gereken proxy ve güvenlik duvarı yapılandırma bilgileri için [Ağ güvenlik duvarı gereksinimlerine](insights/container-insights-onboard.md#network-firewall-requirements) bakın.

## <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
Bu Microsoft SSS, VM'ler için Azure Monitor hakkında sık sorulan soruların bir listesidir. Çözüm le ilgili ek sorularınız varsa, [tartışma forumuna](https://feedback.azure.com/forums/34192--general-feedback) gidin ve sorularınızı gönderin. Bir soru sık sık sorulduğunda, hızlı ve kolay bir şekilde bulunabilmesi için bu makaleye ekliyoruz.

### <a name="can-i-onboard-to-an-existing-workspace"></a>Varolan bir çalışma alanına binebilir miyim?
Sanal makineleriniz bir Log Analytics çalışma alanına zaten bağlıysa, [burada](insights/vminsights-enable-overview.md#prerequisites)listelenen desteklenen bölgelerden birinde olması koşuluyla, Sanal Makineler için Azure Monitor'a binerken bu çalışma alanını kullanmaya devam edebilirsiniz.


### <a name="can-i-onboard-to-a-new-workspace"></a>Yeni bir çalışma alanına gidebilir miyim? 
Sanal Tom'larınız şu anda varolan bir Log Analytics çalışma alanına bağlı değilse, verilerinizi depolamak için yeni bir çalışma alanı oluşturmanız gerekir. Azure portalı üzerinden VM'ler için Azure Monitörü için tek bir Azure VM'i yapılandırdığınızda, yeni bir varsayılan çalışma alanı oluşturma otomatik olarak yapılır.

Komut dosyası tabanlı yöntemi kullanmayı seçerseniz, bu adımlar [Azure PowerShell veya Kaynak Yöneticisi şablon uyrması makalesini kullanarak VM'ler için Azure Monitörünü Etkinleştir'de](insights/vminsights-enable-at-scale-powershell.md) yer alabilirsiniz. 

### <a name="what-do-i-do-if-my-vm-is-already-reporting-to-an-existing-workspace"></a>VM'im zaten varolan bir çalışma alanına bildiriyorsa ne yapmalıyım?
Sanal makinelerinizden zaten veri topluyorsanız, verileri varolan bir Log Analytics çalışma alanına rapor etmek için zaten yapılandırmış olabilirsiniz.  Bu çalışma alanı desteklenen bölgelerimizden birinde olduğu sürece, VM'ler için Azure Monitor'u önceden varolan çalışma alanına etkinleştirebilirsiniz.  Kullanmakta olduğunuz çalışma alanı desteklenen bölgelerimizden birinde değilse, şu anda VM'ler için Azure Monitor'a binemezsiniz.  Ek bölgeleri desteklemek için aktif olarak çalışıyoruz.


### <a name="why-did-my-vm-fail-to-onboard"></a>VM'im neden uçağa binemedi?
Azure portalından bir Azure VM'ye binerken aşağıdaki adımlar oluşur:

* Bu seçenek seçiliyse, varsayılan bir Günlük Analizi çalışma alanı oluşturulur.
* Log Analytics aracısı, gerekli olduğu tespit edilirse, Azure VM'lerine VM uzantısı kullanılarak yüklenir.  
* VM'ler için Azure Monitörü Harita Bağımlılık aracısı, gerekli olduğu tespit edilirse bir uzantı kullanılarak Azure VM'lere yüklenir. 

Dahili işlem sırasında, portalda size bir bildirim durumu döndürmek için yukarıdakilerin her birinin durumunu kontrol ediyoruz. Çalışma alanı nın ve aracı yüklemesinin yapılandırması genellikle 5 ila 10 dakika sürer. Portaldaki izleme verilerini görüntülemeek 5 ila 10 dakika sürer.  

Uçağa binmeye başladıysanız ve VM'nin gemiye binilmesi gerektiğini belirten iletiler görüyorsanız, VM'nin işlemi tamamlaması için 30 dakikaya kadar bekleyin. 


### <a name="i-dont-see-some-or-any-data-in-the-performance-charts-for-my-vm"></a>VM'imin performans grafiklerinde bazı veya herhangi bir veri görmüyorum
Performans *çizelgelerimiz, InsightsMetrics* tablosunda depolanan verileri kullanmak üzere güncelleştirildi.  Bu grafiklerdeki verileri görmek için yeni VM Insights çözümlerini kullanmak için yükseltmeniz gerekir.  Ek bilgi için lütfen [GA SSS](insights/vminsights-ga-release-faq.md) bölümüne bakın.

Disk tablosunda veya performans grafiklerinin bazılarında performans verilerini görmüyorsanız, performans sayaçlarınız çalışma alanında yapılandırılamayabilir. Çözmek için aşağıdaki [PowerShell komut dosyasını](insights/vminsights-enable-at-scale-powershell.md#enable-with-powershell)çalıştırın.


### <a name="how-is-azure-monitor-for-vms-map-feature-different-from-service-map"></a>VMs Haritası için Azure Monitor özelliğinin Hizmet Haritası'ndan farkı nedir?
VMs Haritası için Azure Monitörözelliği Hizmet Haritası'nı temel almıştır, ancak aşağıdaki farklılıklar ayarı vardır:

* Harita görünümüne VM bıçağından ve Azure Monitor altındaki VM'ler için Azure Monitör'den erişilebilir.
* Haritadaki bağlantılar artık tıklanabilir ve seçili bağlantı için yan panelde bağlantı metrik verilerinin görünümünü görüntüler.
* Daha karmaşık haritaları daha iyi desteklemek için haritaları oluşturmak için kullanılan yeni bir API vardır.
* İzlenen VM'ler artık istemci grup düğümüne dahil edilir ve donut grafiği gruptaki izlenen ve izlenmeyen sanal makinelerin oranını gösterir.  Grup genişletildiğinde makinelerin listesini filtrelemek için de kullanılabilir.
* İzlenen sanal makineler artık sunucu bağlantı noktası grubu düğümlerine dahil edilir ve donut grafiği gruptaki izlenen ve izlenmeyen makinelerin oranını gösterir.  Grup genişletildiğinde makinelerin listesini filtrelemek için de kullanılabilir.
* Harita stili, Uygulama istatistiklerinden Uygulama Haritası ile daha tutarlı olacak şekilde güncellendi.
* Yan paneller güncelleştirildi ve Hizmet Haritası - Güncelleme Yönetimi, Değişiklik İzleme, Güvenlik ve Servis Masası'nda desteklenen tümleştirmenin tam kümesi yok. 
* Eşlenecek gruplar ve makineler seçme seçeneği güncelleştirildi ve artık Abonelikler, Kaynak Grupları, Azure sanal makine ölçek kümeleri ve Bulut hizmetlerini destekliyor.
* VMs Map özelliği için Azure Monitörü'nde yeni Hizmet Haritası makine grupları oluşturamazsınız.  

### <a name="why-do-my-performance-charts-show-dotted-lines"></a>Performans çizelgelerim neden noktalı çizgiler gösteriyor?
Bu birkaç nedenden dolayı oluşabilir.  Veri toplamada bir boşluk olduğu durumlarda çizgileri noktalı olarak tasvir ediyoruz.  Etkinleştirilmiş performans sayaçları için veri örnekleme sıklığını değiştirdiyseniz (varsayılan ayar her 60 saniyede bir veri toplamaktır), grafik için dar bir zaman aralığı seçerseniz ve örnekleme sıklığınız grafikte kullanılan kova boyutundan küçükse grafikte noktalı çizgiler imal eder (örneğin, örnekleme sıklığı her 10 dakikada bir ve grafikteki her kova 5 dakikadır).  Görüntülemek için daha geniş bir zaman aralığı seçmek, grafik çizgilerinin bu durumda nokta yerine düz çizgiler olarak görünmesine neden olmalıdır.

### <a name="are-groups-supported-with-azure-monitor-for-vms"></a>Gruplar VM'ler için Azure Monitor ile mi destekleniyor?
Evet, Bağımlılık aracısını yükledikten sonra, abonelik, kaynak grubu, sanal makine ölçek kümeleri ve bulut hizmetlerine dayalı grupları görüntülemek için Sanal Kaynaklar'dan bilgi toplarız.  Hizmet Eşlemi'ni kullanıyorsanız ve makine grupları oluşturduysanız, bunlar da görüntülenir.  Görüntülediğiniz çalışma alanı için oluşturduysanız, bilgisayar grupları da gruplar filtresinde görünür. 

### <a name="how-do-i-see-the-details-for-what-is-driving-the-95th-percentile-line-in-the-aggregate-performance-charts"></a>Toplam performans grafiklerinde yüzde 95'lik çizgiyi neyin yönlendiren ayrıntılarını nasıl görebiliyorum?
Varsayılan olarak, liste, seçilen metrik için en yüksek yüzdelik değere sahip VM'leri, en düşük yüzdelik değerine sahip makineleri gösteren Kullanılabilir bellek grafiği dışında göstermek üzere sıralanır.  Grafiğe tıkladığınızda, seçilen uygun metrikle **En İyi N Listesi** görünümü açılır.

### <a name="how-does-the-map-feature-handle-duplicate-ips-across-different-vnets-and-subnets"></a>Harita özelliği, farklı vnet'ler ve alt ağlar arasında yinelenen IP'leri nasıl işler?
Alt ağlar ve vnet'ler arasında VM'ler veya Azure sanal makine ölçek kümeleri ile IP aralıklarını çoğaltıyorsanız, VMs Haritası için Azure Monitor'un yanlış bilgileri görüntülemesine neden olabilir. Bu bilinen bir konudur ve bu deneyimi geliştirmek için seçenekleri araştırıyoruz.

### <a name="does-map-feature-support-ipv6"></a>Harita özelliği IPv6'yı destekliyor mu?
Harita özelliği şu anda sadece IPv4 destekler ve biz IPv6 için destek araştırıyoruz. Ayrıca IPv6 içinde tünelli IPv4'u destekliyoruz.

### <a name="when-i-load-a-map-for-a-resource-group-or-other-large-group-the-map-is-difficult-to-view"></a>Kaynak Grubu veya diğer büyük grup için bir harita yüklediğimde haritayı görüntülemek zordur
Büyük ve karmaşık yapılandırmaları işlemek için Harita'da iyileştirmeler yapmış olsak da, bir haritanın küme olarak çalışan çok sayıda düğüm, bağlantı ve düğüm olabileceğini fark ediyoruz.  Ölçeklenebilirliği artırmak için desteği artırmaya devam etmeye kararlıyız.   

### <a name="why-does-the-network-chart-on-the-performance-tab-look-different-than-the-network-chart-on-the-azure-vm-overview-page"></a>Performans sekmesindeki ağ grafiği neden Azure VM Genel Bakış sayfasındaki ağ grafiğinden farklı görünüyor?

Azure VM'nin genel bakış sayfası, konuk VM'deki ev sahibinin etkinlik ölçümüne göre grafikleri görüntüler.  Azure VM Genel Bakışı'ndaki ağ grafiği nde yalnızca faturalandırılacak ağ trafiği görüntülenir.  Bu, sanal ağ trafiğini içermez.  VM'ler için Azure Monitor için gösterilen veriler ve grafikler konuk VM'den gelen verilere dayanır ve ağ grafiği, sanal ağ arası ağ da dahil olmak üzere bu VM'ye gelen ve giden tüm TCP/IP trafiğini görüntüler.

### <a name="how-is-response-time-measured-for-data-stored-in-vmconnection-and-displayed-in-the-connection-panel-and-workbooks"></a>VMConnection'da depolanan ve bağlantı panelinde ve çalışma kitaplarında görüntülenen veriler için yanıt süresi nasıl ölçülür?

Yanıt süresi bir yaklaşımdır. Uygulamanın kodunu uygulamadığımız için, bir isteğin ne zaman başladığını ve yanıtın ne zaman geldiğini gerçekten bilmiyoruz. Bunun yerine, bir bağlantı üzerinde gönderilen verileri gözlemleriz ve sonra bu bağlantıdan gelen verileri gözlemleriz. Temsilcimiz bu gönderiyi ve alır ve eşleştirmeye çalışır: bir dizi gönderme, ardından bir dizi alır bir istek/yanıt çifti olarak yorumlanır. Bu işlemler arasındaki zamanlama yanıt süresidir. Bu ağ gecikmesi ve sunucu işleme süresi içerecektir.

Bu yaklaşım, istek/yanıt tabanlı protokoller için iyi çalışır: bağlantıda tek bir istek gider ve tek bir yanıt gelir. Bu, HTTP(S) (pipelining olmadan) için geçerlidir, ancak diğer protokoller için tatmin değildir.

### <a name="are-their-limitations-if-i-am-on-the-log-analytics-free-pricing-plan"></a>Log Analytics Free fiyatlandırma planında yer alıyorsam sınırlamaları bunlar mı?
Azure Monitörü *Ücretsiz* fiyatlandırma katmanını kullanarak Bir Günlük Analizi çalışma alanıyla yapılandırıldıysanız, VMs Haritası için Azure Monitor özelliği yalnızca çalışma alanına bağlı beş bağlı makineyi destekler. Boş bir çalışma alanına bağlı beş VM'iniz varsa, VM'lerden birinin bağlantısını keser ve daha sonra yeni bir VM bağlarsınız, yeni VM izlenmez ve Harita sayfasına yansıtılır.  

Bu koşulda, VM'yi açtığınızda ve VM'ye yüklendikten sonra bile sol bölmeden **Öngörüler** seçtiğinizde **Şimdi Dene** seçeneği ile istenir.  Ancak, bu VM VM'ler için Azure Monitor'a dahil edilmediyse, normalde oluşacağı gibi seçenekler elenmez. 


## <a name="next-steps"></a>Sonraki adımlar
Sorunuz burada yanıtlanmamışsa, aşağıdaki forumlara ek sorular ve yanıtlar için başvurabilirsiniz.

- [Günlük Analizi](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights)
- [Uygulama Bilgileri](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=ApplicationInsights)

Azure Monitor hakkında genel geri bildirim için lütfen [geri bildirim forumuna](https://feedback.azure.com/forums/34192--general-feedback)katılın.
