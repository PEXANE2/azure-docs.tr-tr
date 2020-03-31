---
title: Azure Monitor ile Azure sanal makinelerini izleme
description: Azure Monitor'u kullanarak Azure'daki sanal makinelerden izleme verilerinin nasıl toplandığını ve analiz edilebildiğini açıklar.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/17/2020
ms.openlocfilehash: 2cb53d0c88d8c29da2bd8bf52d6536555d56c76e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283948"
---
# <a name="monitoring-azure-virtual-machines-with-azure-monitor"></a>Azure Monitor ile Azure sanal makinelerini izleme
Bu makalede, sistem durumunu korumak için Azure sanal makinelerinden izleme verileri toplamak ve analiz etmek için Azure Monitor'un nasıl kullanılacağı açıklanmaktadır. Sanal makineler diğer [Azure kaynakları](monitor-azure-resource.md)gibi Azure Monitor ile kullanılabilirlik ve performans açısından izlenebilir, ancak konuk işletim ve sistemi ve içinde çalışan iş yüklerini de izlemeniz gerektiğinden diğer kaynaklardan benzersizdirler. 

> [!NOTE]
> Bu makalede, Azure Monitor'daki sanal makineleri izlemek için kavramlar alavesin. Temel kavramlara odaklanmadan sanal makinelerinizi hızlı bir şekilde izlemeye başlamak için [Quickstart: Azure Monitor ile bir Azure sanal makinesini izleyin.](../learn/quick-monitor-azure-vm.md)


## <a name="differences-from-other-azure-resources"></a>Diğer Azure kaynaklarından farklar
[Azure kaynaklarının Azure Monitor ile izlenmesi,](monitor-azure-resource.md) Azure kaynakları tarafından oluşturulan izleme verilerini ve bu verileri analiz etmek ve uyarmak için Azure Monitor'un özelliklerini nasıl kullanabileceğinizi açıklar. Aşağıdaki farklarla Azure sanal makinelerinden aynı izleme verilerini toplayabilir ve bunları üzerinde işlem yapabilirsiniz:

- [Platform ölçümleri](../platform/data-platform-metrics.md) sanal makineler için değil, yalnızca [sanal makine ana bilgisayarı](#monitoring-data)için otomatik olarak toplanır. Konuk işletim sisteminden performans verileri toplamak için bir aracıya ihtiyacınız vardır. 
- Sanal makineler, Azure kaynağında gerçekleştirilen işlemlere ilişkin bilgi sağlayan [kaynak günlükleri](../platform/platform-logs-overview.md) oluşturmaz. Konuk işletim sisteminden günlük verileri toplamak için bir aracı kullanırsınız.
- Sanal bir makinenin depolama ve etkinlik hub'ları gibi diğer hedeflere platform ölçümleri göndermesi için [tanılama ayarları](../platform/diagnostic-settings.md) oluşturabilirsiniz, ancak bu tanılama ayarlarını Azure portalında yapılandıramazsınız. 

## <a name="monitoring-data"></a>Verileri izleme
Azure'daki Azure'daki sanal makineler, aşağıdaki diyagramda gösterilen [günlükler](../platform/data-platform-logs.md) ve [ölçümler](../platform/data-platform-metrics.md) oluşturur.

![Genel Bakış](media/monitor-vm-azure/logs-metrics.png)


### <a name="virtual-machine-host"></a>Sanal makine ana bilgisayarı
Azure'daki sanal makineler, sanal makine için aşağıdaki verileri, [İzleme verilerinde](monitor-azure-resource.md#monitoring-data)açıklandığı gibi diğer Azure kaynaklarıyla aynı ana bilgisayara oluşturur.

- [Platform ölçümleri](../platform/data-platform-metrics.md) - Düzenli aralıklarla otomatik olarak toplanan ve belirli bir zamanda kaynağın bazı yönünü açıklayan sayısal değerler. Platform ölçümleri sanal makine ana bilgisayarı için toplanır, ancak konuk işletim sistemi için ölçümleri toplamak için tanılama uzantısı gerektirir.
- [Etkinlik günlüğü](../platform/platform-logs-overview.md) - Abonelikteki her Azure kaynağındaki işlemlere dışarıdan (yönetim düzlemi) ilişkin bilgiler sağlar. Sanal bir makine için bu, ne zaman başlatıldıve yapılandırma değişiklikleri gibi bilgileri içerir.


### <a name="guest-operating-system"></a>Konuk işletim sistemi
Sanal bir makinenin konuk işletim sisteminden veri toplamak için, her sanal makinede yerel olarak çalışan ve Azure Monitor'a veri gönderen bir aracıya gereksinim duyarsınız. Azure Monitor için her biri farklı veri toplayan ve farklı konumlara veri yazan birden çok aracı kullanılabilir. [Azure Monitörü aracılarının Genel Görünümü'nde](../platform/agents-overview.md)farklı aracıların ayrıntılı bir karşılaştırmasını alın. 

- [Günlük Analizi aracısı](../platform/agents-overview.md#log-analytics-agent) - Azure'daki sanal makineler, diğer bulut ortamları ve şirket içi makineler için kullanılabilir. Azure Monitör Günlükleri için veri toplar. VM'ler ve izleme çözümleri için Azure Monitörünü destekler. Bu, System Center Operations Manager için kullanılan aracının aynısıdır.
- [Bağımlılık aracısı](../platform/agents-overview.md#dependency-agent) - Sanal makinede çalışan işlemler ve bunların bağımlılıkları hakkında veri toplar. Verileri Azure'a aktarmak için Log Analytics aracısına güvenir ve VM'ler, Hizmet Haritası ve Tel Verileri 2.0 çözümleri için Azure Monitor'u destekler.
- [Azure Tanı lama uzantısı](../platform/agents-overview.md#azure-diagnostics-extension) - Yalnızca Azure Monitor sanal makineleri için kullanılabilir. Birden çok konuma veri toplayabilir, ancak öncelikle Windows sanal makineleri için Azure Monitör Ölçümleri'ne konuk performans verileri toplamak için kullanılabilir.
- [Telegraf aracısı](../platform/collect-custom-metrics-linux-telegraf.md) - Linux VM'lerden Azure Monitör Ölçümleri'ne performans verileri toplayın.


## <a name="configuration-requirements"></a>Yapılandırma gereksinimleri
Sanal bir makineyi izlemek için Azure Monitor'un tüm özelliklerini etkinleştirmek için, sanal makine ana bilgisayardan ve konuk işletim sisteminden hem [Azure Monitör Ölçümleri'ne](../platform/data-platform-logs.md) hem de Azure Monitör [Günlükleri'ne](../platform/data-platform-logs.md)izleme verileri toplamanız gerekir. Aşağıdaki tabloda, bu koleksiyonu etkinleştirmek için gerçekleştirilmesi gereken yapılandırma listeleilmektedir. Bu adımların tümünün belirli gereksinimlerinize bağlı olarak gerçekleştirilmemeyi seçebilirsiniz.

| Yapılandırma adımı | Tamamlanan eylemler | Özellikler etkin |
|:---|:---|:---|
| Yapılandırma yok | - Ölçümleriçin toplanan ana bilgisayar platformu ölçümleri.<br>- Faaliyet günlüğü toplanır. | - Ana bilgisayar için ölçüler kaşifi.<br>- Ana bilgisayar için ölçümler uyarıları.<br>- Etkinlik günlüğü uyarıları. |
| [VM'ler için Azure Monitörünü etkinleştirme](#enable-azure-monitor-for-vms) | - Log Analytics aracısı yüklendi.<br>- Bağımlılık aracısı yüklendi.<br>- Günlüklere toplanan konuk performans verileri.<br>- İşlem ve bağımlılık bilgileri Günlükler'de toplanır. | - Konuk performans verileri için performans grafikleri ve çalışma kitapları.<br>- Konuk performans verileri için günlük sorguları.<br>- Konuk performans verileri için uyarılarını günlüğe kaydedin.<br>- Bağımlılık haritası. |
| [Tanılama uzantısını ve telegraf aracısını yükleyin](#enable-diagnostics-extension-and-telegraf-agent) | - Metrikler'e toplanan konuk performans verileri. | - Konuk için Metrics explorer.<br>- Misafirler için ölçümler uyarıları.  |
| [Günlük Analizi çalışma alanını yapılandırma](#configure-log-analytics-workspace) | - Konuklardan toplanan etkinlikler. | - Konuk etkinlikleri için sorguları günlüğe kaydedin.<br>- Konuk etkinlikleri için uyarılarını günlüğe kaydedin. |
| [Sanal makine için tanılama ayarı oluşturma](#collect-platform-metrics-and-activity-log) | - Günlüklere toplanan platform ölçümleri.<br>- Günlüklere toplanan etkinlik günlüğü. | - Ana bilgisayar ölçümleri için Loq sorguları.<br>- Ana bilgisayar ölçümleri için günlük uyarıları.<br>- Etkinlik günlüğü için günlük sorguları.

Bu yapılandırma adımlarının her biri aşağıdaki bölümlerde açıklanmıştır.

### <a name="enable-azure-monitor-for-vms"></a>VM'ler için Azure Monitörünü etkinleştirme
[Sanal Makineler için Azure Monitör,](vminsights-overview.md) Azure Monitor'daki sanal makineleri izlemek için birincil araç olan Azure Monitor'da bir [kavrayıştır.](insights-overview.md) Standart Azure Monitör özelliklerine göre aşağıdaki ek değeri sağlar.

- Sanal makine konuk işletim sisteminin ve iş yüklerinin izlenmesini sağlamak için Log Analytics aracısı ve Bağımlılık aracısının basitleştirilmiş onboarding'i. 
- Sanal makinenin konuk işletim sisteminden temel performans ölçümlerini analiz etmenize olanak tanıyan önceden tanımlanmış trend performans grafikleri ve çalışma kitapları.
- Her sanal makinede çalışan işlemleri ve birbirine bağlı bileşenleri diğer makineler ve dış kaynaklarla görüntüleyen bağımlılık haritası.

![VM'ler için Azure İzleyici](media/monitor-vm-azure/vminsights-01.png)

![VM'ler için Azure İzleyici](media/monitor-vm-azure/vminsights-02.png)


Azure portalının sanal makine menüsündeki **Öngörüler** seçeneğinden Sanal M'ler için Azure Monitörünü etkinleştirin. Ayrıntılar ve diğer yapılandırma yöntemleri [için VM'lere genel bakış için Azure Monitörünü Etkinleştir'e](vminsights-enable-overview.md) bakın.

![VM'ler için Azure Monitörünü etkinleştirme](media/monitor-vm-azure/enable-vminsights.png)

### <a name="configure-log-analytics-workspace"></a>Günlük Analizi çalışma alanını yapılandırma
Azure Monitor tarafından VM'ler için kullanılan Log Analytics aracısı, verileri bir [Log Analytics çalışma alanına](../platform/data-platform-logs.md#how-is-data-in-azure-monitor-logs-structured)gönderir. Log Analytics çalışma alanını yapılandırarak aracıdan ek performans verileri, olaylar ve diğer izleme verilerinin toplanmasını etkinleştirebilirsiniz. Çalışma alanına bağlanan herhangi bir aracı yapılandırmayı otomatik olarak karşıdan yükleyeceği ve tanımlanan verileri hemen toplamaya başlayacağından, yalnızca bir kez yapılandırılması gerekir. 

**Başlangıç Al'dan** **Workspace yapılandırmasını** seçerek, çalışma alanı yapılandırmasına doğrudan Azure Monitor'dan VM'ler için erişebilirsiniz. Menüsünü açmak için çalışma alanı adını tıklatın.

![Çalışma alanı yapılandırması](media/monitor-vm-azure/workspace-configuration.png)

Veri kaynaklarını yapılandırmak için çalışma alanı menüsünden **Gelişmiş Ayarlar'ı** ve ardından **Verileri** seçin. Windows aracıları için **Windows Olay Günlükleri'ni** seçin ve *Sistem* ve *Uygulama*gibi yaygın olay günlükleri ekleyin. Linux aracıları için **Syslog'u** seçin ve *kern* ve *daemon*gibi ortak tesisleri ekleyin. Kullanılabilir veri kaynaklarının listesi ve bunları yapılandırmayla ilgili ayrıntılar için [Azure Monitor'daki Aracı veri kaynaklarına](../platform/agent-data-sources.md) bakın. 

![Olayları yapılandırma](media/monitor-vm-azure/configure-events.png)


> [!NOTE]
> Performans sayaçlarını çalışma alanı yapılandırmasından toplanacak şekilde yapılandırabilirsiniz, ancak bu, Azure Monitor tarafından VM'ler için toplanan performans sayaçlarıyla gereksiz olabilir. VM'ler için Azure Monitörü, en yaygın sayaç kümesini dakikada bir sıklıkta toplar. Yalnızca Azure Monitor tarafından VM'ler için zaten toplanmış sayacı toplamak istiyorsanız veya performans verilerini kullanarak varolan sorgularınız varsa, çalışma alanı tarafından toplanacak performans sayaçlarını yapılandırın.


### <a name="enable-diagnostics-extension-and-telegraf-agent"></a>Tanılama uzantısı ve Telegraf ajan etkinleştirin
Sanal Oturumlar için Azure Monitor, Bir Log Analytics çalışma alanına veri toplayan Log Analytics aracısını temel alınarak temel adatır. Bu, [Azure Monitor'un](../platform/data-platform-logs.md#what-can-you-do-with-azure-monitor-logs) [günlük sorguları, günlük](../log-query/log-query-overview.md) [uyarıları](../platform/alerts-log.md)ve [çalışma kitapları](../platform/workbooks-overview.md)gibi birden çok özelliğini destekler. [Tanılama uzantısı,](../platform/diagnostics-extension-overview.md) Windows sanal makinelerinkonuk işletim sisteminden Azure Depolama'ya performans verileri toplar ve isteğe bağlı olarak performans verilerini [Azure Monitör Ölçümleri'ne](../platform/data-platform-metrics.md)gönderir. Linux sanal makineleri [için, Telegraf aracısının](../platform/collect-custom-metrics-linux-telegraf.md) Azure Ölçümleri'ne veri göndermesi gerekir.  Bu, Azure Monitor'un [metrikler gezgini](../platform/metrics-getting-started.md) ve [metrik uyarıları](../platform/alerts-metric.md)gibi diğer özelliklerini etkinleştirirken. Ayrıca, Azure Etkinlik Hub'larını kullanarak etkinlikleri ve performans verilerini Azure Monitor dışına gönderecek tanılama uzantısını da yapılandırabilirsiniz.

Azure portalındaki tek bir Windows sanal makinesinin tanılama uzantısını VM menüsündeki **Tanılama ayarı** seçeneğinden yükleyin. **Lavabolar** sekmesinde **Azure Monitor'u** etkinleştirme seçeneğini seçin. Birden çok sanal makine için şablon veya komut satırından uzantıyı etkinleştirmek için [Yükleme ve yapılandırmaya](../platform/diagnostics-extension-overview.md#installation-and-configuration)bakın. Log Analytics aracısının aksine, toplanacak veriler her sanal makinedeki uzantı için yapılandırmada tanımlanır.

![Tanılama ayarı](media/monitor-vm-azure/diagnostic-setting.png)

Telegraf aracılarını Linux sanal makinelerinde yapılandırma hakkında ayrıntılı bilgi için [Telegraf'ı Yükle ve yapılandırın.](../platform/collect-custom-metrics-linux-telegraf.md#install-and-configure-telegraf) **Tanılama ayarı** menüsü Linux için kullanılabilir, ancak yalnızca Azure depolamasına veri göndermenize olanak tanır.

### <a name="collect-platform-metrics-and-activity-log"></a>Platform ölçümlerini ve Etkinlik günlüğünü toplama
Azure portalında her sanal makine ana bilgisayarı için toplanan platform ölçümlerini ve Etkinlik günlüğünü görüntüleyebilirsiniz. Sanal makine için toplanan diğer izleme verileriyle analiz etmek için bu verileri, Sanal Aygıtlar için Azure Monitörü ile aynı Log Analytics çalışma alanına toplayın. Bu koleksiyon [tanılama ayarı](../platform/diagnostic-settings.md)ile yapılandırılır. [Abonelik için tanılama ayarı](../platform/diagnostic-settings.md#create-diagnostic-settings-in-azure-portal)ile Etkinlik günlüğünü toplayın.

Sanal makine için tanılama ayarı ile platform ölçümlerini toplayın. Diğer Azure kaynaklarının aksine, Azure portalında sanal bir makine için tanılama ayarı oluşturamazsınız, ancak başka bir [yöntem](../platform/diagnostic-settings.md#create-diagnostic-settings-using-powershell)kullanmanız gerekir. Aşağıdaki örnekler, hem PowerShell hem de CLI kullanarak sanal bir makine için ölçümlerin nasıl toplandığını gösterir.

```powershell
Set-AzDiagnosticSetting -Name vm-diagnostics -ResourceId "/subscriptions/monitor diagnostic-settings create \
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-vm" -Enabled $true -MetricCategory AllMetrics -workspaceId "/subscriptions/monitor diagnostic-settings create \
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace"
```

```CLI
az monitor diagnostic-settings create \
--name VM-Diagnostics 
--resource /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-vm \
--metrics '[{"category": "AllMetrics","enabled": true}]' \
--workspace /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace

```

## <a name="monitoring-in-the-azure-portal"></a>Azure portalında izleme 
Sanal bir makine için izleme verilerinin koleksiyonunu yapılandırdıktan sonra, Azure portalında erişmek için birden çok seçeneğiniz olur:

- Tüm izlenen kaynaklardan verilere erişmek için **Azure Monitor** menüsünü kullanın. 
- Sanal makinelerin kümelerini ölçekte izlemek için Sanal Ayarlar için Azure Monitörünü kullanın.
- Azure portalındaki menüsünden tek bir sanal makinenin verilerini analiz edin. Aşağıdaki tabloda sanal makineler menüsünü izlemek için farklı seçenekler listelenmektedir.

![Azure portalında izleme](media/monitor-vm-azure/monitor-menu.png)

| Menü seçeneği | Açıklama |
|:---|:---|
| Genel Bakış | Sanal makine ana bilgisayarı için [platform ölçümlerini](../platform/data-platform-metrics.md) görüntüler. Bu verilerle çalışmak için bir grafiğe [tıklayın.](../platform/metrics-getting-started.md) |
| Etkinlik günlüğü | [Geçerli](../platform/activity-log-view.md) sanal makine için filtre uygulanmış etkinlik günlüğü girişleri. |
| Insights | Seçilen geçerli sanal makinenin haritasıyla [Sanal Makineler için Azure Monitörünü](../insights/vminsights-overview.md) açar. |
| Uyarılar | Geçerli sanal makine için [uyarıları](../platform/alerts-overview.md) görüntüler.  |
| Ölçümler | Geçerli sanal makineye ayarlanan kapsamile [açık ölçümler gezgini.](../platform/metrics-getting-started.md) |
| Tanılama ayarları | Geçerli sanal makine için [tanılama uzantısını](../platform/diagnostics-extension-overview.md) etkinleştirin ve yapılandırın. |
| Danışman önerileri | [Azure Advisor'dan](/azure/advisor/)geçerli sanal makine için öneriler. |
| Günlükler | Geçerli sanal makineye ayarlanan [kapsamla](../log-query/scope.md) [Log Analytics'i](../log-query/log-query-overview.md#what-is-log-analytics) açın. |
| Bağlantı monitörü | Mevcut sanal makine ve diğer sanal makineler arasındaki bağlantıları izlemek için [Ağ İzleyicisi Bağlantı Monitörünü](../../network-watcher/connection-monitor-preview.md) açın. |


## <a name="analyzing-metric-data"></a>Metrik verileri çözümleme
Sanal makine menüsünden **Ölçümleri** açarak metrikler explorer'ı kullanarak sanal makinelerin ölçümlerini analiz edebilirsiniz. Bu aracı kullanma yla ilgili ayrıntılar için [Azure Ölçümleri Gezgini ile başlarken](../platform/metrics-getting-started.md) bkz. 

Sanal makineler tarafından ölçümler için kullanılan iki ad alanı vardır:

| Ad Alanı | Açıklama |
|:---|:---|
| Sanal Makine Ana Bilgisayarı | Tüm Azure sanal makineleri için otomatik olarak toplanan ana bilgisayar ölçümleri. [Microsoft.Compute/virtualMachines](../platform/metrics-supported.md#microsoftcomputevirtualmachines)adresindeki ölçümlerin ayrıntılı listesi. |
| Sanal Makine Konuk | Tanılama uzantısı yüklü ve Azure Monitor lavabo göndermek için yapılandırılan sanal makinelerden toplanan konuk işletim sistemi ölçümleri. |

![Ölçümler](media/monitor-vm-azure/metrics.png)

## <a name="analyzing-log-data"></a>Günlük verilerini çözümleme
Azure sanal makineleri aşağıdaki verileri Azure Monitor Günlükleri'nde toplar. 

VM'ler için Azure Monitor, *InsightsMetrics* tablosuna yazılmış önceden belirlenmiş bir performans sayacı kümesinin toplanmasını sağlar. Bu, Azure Monitor tarafından [Kapsayıcılar için](container-insights-overview.md)kullanılan tablonun aynısi. 

| Veri kaynağı | Gereksinimler | Tablolar |
|:---|:---|:---|
| VM'ler için Azure İzleyici | Her sanal makinede etkinleştirin. | ÖngörülerÖlçümler<br>VMboundPort<br>VMComputer<br>VMConnection<br>VMProcess<br>Ayrıntılar [için VM'ler için Azure Monitor'dan günlükleri nasıl sorgularınız.](vminsights-log-search.md) |
| Etkinlik günlüğü | Abonelik için tanılama ayarı. | AzureActivity |
| Ana bilgisayar ölçümleri | Sanal makine için tanılama ayarı. | AzureMetrics |
| Konuk işletim sisteminden veri kaynakları | Günlük Analizi aracısını etkinleştirin ve veri kaynaklarını yapılandırın. | Her veri kaynağı için belgelere bakın. |


> [!NOTE]
> Log Analytics aracısı tarafından toplanan performans verileri *Perf* tablosuna yazarken, Sanal Taşıtlar için Azure Monitörü bunu *InsightsMetrics* tablosuna toplar. Bu aynı veri, ancak tablolar farklı bir yapıya sahip. *Perf'e*dayalı varolan sorgularınız varsa, *InsightsMetrics'i*kullanmak için yeniden yazılması gerekir.


## <a name="alerts"></a>Uyarılar
Azure Monitor'daki [uyarılar,](../platform/alerts-overview.md) izleme verilerinizde önemli koşullar bulunduğunda sizi proaktif olarak bilgilendirir ve Bir Mantık Uygulaması başlatma veya web hook'u çağırma gibi bir eylem başlatma potansiyeline neden olabilir. Uyarı kuralları, bir uyarının ne zaman oluşturulmasını belirlemek için kullanılan mantığı tanımlar. Azure Monitor uyarı kuralları tarafından kullanılan verileri toplar, ancak Azure aboneliğinizdeki uyarı koşullarını tanımlamak için kurallar oluşturmanız gerekir.

Aşağıdaki bölümlerde, her birini ne zaman kullanmanız gerektiğine ilişkin uyarı kuralları ve önerileri türleri açıklayınız. Bu öneri, uyarı kuralı türünün işlevselliğini ve maliyetini temel alır. Uyarıların ayrıntılı fiyatlandırması için [Azure Monitor fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/)bölümüne bakın.


### <a name="activity-log-alert-rules"></a>Etkinlik günlüğü uyarı kuralları
[Etkinlik günlüğünde](../platform/alerts-activity-log.md) belirli ölçütleri eşleştiren bir giriş oluşturulduğunda etkinlik günlüğü uyarı kuralları ateşlenir. Herhangi bir maliyeti yoktur, bu nedenle gereksinim duyduğunuz mantık etkinlik günlüğündeyse ilk tercihiniz olmalıdır. 

Etkinlik günlüğü uyarıları için hedef kaynak belirli bir sanal makine, kaynak grubundaki tüm sanal makineler veya abonelikteki tüm sanal makineler olabilir.

Örneğin, sinyal adı için *Power Off Virtual Machine'i* seçerek kritik bir sanal makine durdurulursa bir uyarı oluşturun.

![Etkinlik günlüğü uyarısı](media/monitor-vm-azure/activity-log-alert.png)


### <a name="metric-alert-rules"></a>Ölçüm uyarı kuralları
[Metrik uyarı kuralları,](../platform/alerts-metric.md) metrik değer bir eşiği aştığında ateş eder. Belirli bir eşik değeri tanımlayabilir veya Azure Monitor'un geçmiş verilere dayalı bir eşiği dinamik olarak belirlemesine izin verebilirsiniz.  Daha az maliyetli oldukları ve günlük uyarı kurallarından daha duyarlı oldukları ndan, metrik verilerle mümkün olduğunca metrik uyarıları kullanın. Ayrıca, metrik eşiğin altına düştüğünde kendilerini çözecekleri anlamına da gelir.

Etkinlik günlüğü uyarıları için hedef kaynak belirli bir sanal makine veya bir kaynak grubundaki tüm sanal makineler olabilir.

Örneğin, sanal bir makinenin işlemcisi belirli bir değeri aştığında bir uyarı oluşturmak için, sinyal türü olarak *Yüzde CPU'yu* kullanarak bir metrik uyarı kuralı oluşturun. Belirli bir eşik değeri ayarlayın veya Azure Monitor'un dinamik bir eşik belirlemesine izin verin. 

![Metrik uyarı](media/monitor-vm-azure/metric-alert.png)

### <a name="log-alerts"></a>Günlük uyarıları
[Günlük uyarı kuralları,](../platform/alerts-log.md) zamanlanmış bir günlük sorgusunun sonuçları belirli ölçütlere denk geldiğinde yangın atanır. Günlük sorgu uyarıları, uyarı kurallarının en pahalı ve en az yanıt veren uyarılarıdır, ancak en çeşitli verilere erişebilir ve diğer uyarı kuralları tarafından gerçekleştirilmeyen karmaşık mantık gerçekleştirebilirler. 

Günlük sorgusunun hedef kaynağı Log Analytics çalışma alanıdır. Sorgudaki belirli bilgisayarlar için filtre uygulayın.

Örneğin, belirli bir kaynak grubundaki sanal makinelerin çevrimdışı olup olmadığını kontrol eden bir uyarı oluşturmak için, son on dakika içinde bir kalp atışı kaçıran her bilgisayar için bir kayıt döndüren aşağıdaki sorguyu kullanın. En az bir bilgisayarın cevapsız sinyali varsa 1'in eşiğini kullanın.

```kusto
Heartbeat
| where TimeGenerated < ago(10m)
| where ResourceGroup == "my-resource-group"
| summarize max(TimeGenerated) by Computer
```

![Günlük uyarısı](media/monitor-vm-azure/log-alert-01.png)

Abonelikteki herhangi bir Windows sanal makinesinde aşırı sayıda başarısız oturum açma oluştuysa, son bir saat içinde başarısız olan her oturum açma olayı için bir kayıt döndüren aşağıdaki sorguyu kullanın. İzin verilebilen başarısız oturum açma sayısına ayarlı bir eşik kullanın. 

```kusto
Event
| where TimeGenerated < ago(1hr)
| where EventID == 4625
```

![Günlük uyarısı](media/monitor-vm-azure/log-alert-02.png)


## <a name="system-center-operations-manager"></a>System Center Operations Manager
System Center Operations Manager (SCOM), sanal makinelerdeki iş yüklerinin parçalı izlenmesini sağlar. İzleme platformları ve uygulama için farklı stratejilerkarşılaştırması için [Bulut İzleme Kılavuzu'na](https://docs.microsoft.com/azure/cloud-adoption-framework/manage/monitor/) bakın.

Kullanmaya devam etmek istediğiniz varolan bir SCOM ortamınız varsa, ek işlevsellik sağlamak için azure monitor ile tümleştirebilirsiniz. Azure Monitor tarafından kullanılan Log Analytics aracısı, sanal makinelerin her ikisine de veri göndermesini izleyebilmeniz için SCOM için kullanılan aracıyla aynıdır. Yine de aracıyı VM'ler için Azure Monitor'a eklemeniz ve çalışma alanını yukarıda belirtildiği gibi ek veri toplamak üzere yapılandırmanız gerekir, ancak sanal makineler varolan yönetim paketlerini değişiklik yapmadan SCOM ortamında çalıştırmaya devam edebilir.

Azure Monitor'un varolan scom özelliklerini artıran özellikleri şunlardır:

- Günlük ve performans verilerinizi etkileşimli olarak analiz etmek için Log Analytics'i kullanın.
- Birden çok sanal makinede uyarı koşullarını tanımlamak ve SCOM'daki uyarıları kullanarak mümkün olmayan uzun vadeli eğilimleri kullanmak için günlük uyarılarını kullanın.   

Mevcut SCOM yönetim grubunuzu Log Analytics çalışma alanınıza bağlama hakkında ayrıntılı bilgi için [Azure Monitor'a Bağlan Operasyon Yöneticisi'ne](../platform/om-agents.md) bakın.


## <a name="next-steps"></a>Sonraki adımlar

* [Günlük sorgularını kullanarak Azure Monitor günlüklerinde verileri nasıl analiz edebilirsiniz öğrenin.](../log-query/get-started-queries.md)
* [Azure Monitor'da ölçümleri ve günlükleri kullanarak uyarılar hakkında bilgi edinin.](../platform/alerts-overview.md)

