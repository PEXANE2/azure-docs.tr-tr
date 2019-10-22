---
title: Azure Izleyici 'de çözüm Azure SQL Analytics | Microsoft Docs
description: Azure SQL Analytics çözümü, Azure SQL veritabanlarınızı yönetmenize yardımcı olur
services: log-analytics
ms.service: log-analytics
ms.custom: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: carlrab
manager: craigg
ms.date: 12/17/2018
ms.openlocfilehash: f27ea984e2c11be03d69295d206c1b99791a3f72
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72693274"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Azure SQL Analytics kullanarak Azure SQL veritabanını izleme (Önizleme)

![Azure SQL Analytics simgesi](./media/azure-sql/azure-sql-symbol.png)

Azure SQL Analytics, Azure SQL veritabanlarının, elastik havuzların ve yönetilen örneklerin, ölçekteki ve tek bir cam bölmesi aracılığıyla birden çok aboneliğin performansını izlemeye yönelik gelişmiş bir bulut izleme çözümüdür. Performans sorunlarını gidermeye yönelik yerleşik zekayla önemli Azure SQL Veritabanı performans ölçümlerini toplar ve görselleştirir.

Çözümle topladığınız ölçümleri kullanarak, özel izleme kuralları ve uyarılar oluşturabilirsiniz. Çözüm, uygulama yığınınızın her katmanındaki sorunları belirlemenize yardımcı olur. Tek bir Log Analytics çalışma alanında yönetilen örneklerde bulunan tüm Azure SQL veritabanları, elastik havuzlarınız ve veritabanları hakkındaki verileri sunmak için Azure Izleyici görünümleri ile birlikte Azure tanılama ölçümlerini kullanır. Azure Izleyici, yapılandırılmış ve yapılandırılmamış verileri toplamanıza, ilişkilendirmenize ve görselleştirmenize yardımcı olur.

Azure SQL Analytics çözümü kullanmaya ve tipik kullanım senaryolarına yönelik uygulamalı bir genel bakış için bkz. gömülü video:

>[!VIDEO https://www.youtube.com/embed/j-NDkN4GIzg]
>

## <a name="connected-sources"></a>Bağlı kaynaklar

Azure SQL Analytics, Azure SQL veritabanları için tanılama telemetrisi akışını destekleyen yalnızca bulut izleme çözümüdür: tek, havuza alınmış ve yönetilen örnek veritabanları. Çözüm, Azure Izleyici 'ye bağlanmak için aracıları kullanmıyorsa, çözüm şirket içinde veya VM 'lerde barındırılan SQL Server izlemeyi desteklemez, aşağıdaki uyumluluk tablosuna bakın.

| Bağlı Kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| [Tanılama ayarları](../platform/diagnostic-settings.md) | **Evet** | Azure ölçümü ve günlük verileri Azure Izleyici günlüklerine doğrudan Azure tarafından gönderilir. |
| [Azure depolama hesabı](../platform/collect-azure-metrics-logs.md) | Hayır | Azure Izleyici, bir depolama hesabındaki verileri okuyamıyor. |
| [Windows aracıları](../platform/agent-windows.md) | Hayır | Doğrudan Windows aracıları çözüm tarafından kullanılmıyor. |
| [Linux aracıları](../learn/quick-collect-linux-computer.md) | Hayır | Doğrudan Linux aracıları çözüm tarafından kullanılmıyor. |
| [System Center Operations Manager yönetim grubu](../platform/om-agents.md) | Hayır | Operations Manager aracısından Azure Izleyici 'ye doğrudan bağlantı, çözüm tarafından kullanılmaz. |

## <a name="configuration"></a>Yapılandırma
Log Analytics çalışma alanınıza Azure SQL Analytics (Önizleme) çözümünü eklemek için [Çözüm Galerisi Azure izleyici çözümlerini ekleme](../../azure-monitor/insights/solutions.md) bölümünde açıklanan işlemi kullanın.

### <a name="configure-azure-sql-databases-elastic-pools-and-managed-instances-to-stream-diagnostics-telemetry"></a>Tanılama telemetrisini akışa almak için Azure SQL veritabanlarını, elastik havuzları ve yönetilen örnekleri yapılandırma

Çalışma alanınızda Azure SQL Analytics çözümü oluşturduktan sonra, izlemek istediğiniz her kaynağı, tanılama telemetrisini çözüme akışa almak üzere **yapılandırmanız** gerekir. Bu sayfadaki ayrıntılı yönergeleri izleyin:

- [Tanılama telemetrisini Azure SQL Analytics akışa](../../sql-database/sql-database-metrics-diag-logging.md)almak IÇIN Azure SQL veritabanınızın Azure tanılama etkinleştirin.

Yukarıdaki sayfada, tek bir Azure SQL Analytics çalışma alanındaki birden çok Azure aboneliğini tek bir cam bölmesi olarak izleme desteğinin etkinleştirilmesi hakkında yönergeler de sağlanmaktadır.

## <a name="using-the-solution"></a>Çözümü kullanma

Çalışma alanınıza çözüm eklediğinizde Azure SQL Analytics kutucuğu çalışma alanınıza eklenir ve genel bakış ' da görünür. Kutucuk içeriğini yüklemek için Özeti görüntüle bağlantısını seçin.

![Azure SQL Analytics Özet kutucuğu](./media/azure-sql/azure-sql-sol-tile-01.png)

Bir kez yüklendikten sonra kutucuk, yönetilen örneklerdeki Azure SQL veritabanlarının, elastik havuzların, yönetilen örneklerin ve çözüm tarafından tanılama telemetrisini aldığını gösteren veritabanlarının sayısını gösterir.

![Azure SQL Analytics kutucuğu](./media/azure-sql/azure-sql-sol-tile-02.png)

Çözüm iki ayrı görünüm sağlar--Azure SQL veritabanları ve elastik havuzların izlenmesi için bir tane, yönetilen örnek izleme için diğer görünüm ve yönetilen örnekteki veritabanları.

Azure SQL veritabanları ve elastik havuzlar için Azure SQL Analytics izleme panosunu görüntülemek için kutucuğun üst kısmına tıklayın. Yönetilen örnek için Azure SQL Analytics izleme panosunu ve yönetilen örnekteki veritabanlarını görüntülemek için kutucuğun alt kısmına tıklayın.

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL Analytics verileri görüntüleme

Pano, farklı perspektiflerle izlenen tüm veritabanlarına genel bakış içerir. Farklı perspektiflerin çalışması için, SQL kaynaklarınızın Log Analytics çalışma alanına akışını sağlamak üzere uygun ölçümleri veya günlükleri etkinleştirmeniz gerekir.

Azure Izleyici 'ye bazı ölçümler veya Günlükler aktarılmamışsa, çözümdeki kutucukların izleme bilgileriyle doldurulmadığını unutmayın.

### <a name="azure-sql-database-and-elastic-pool-view"></a>Azure SQL veritabanı ve elastik havuz görünümü

Veritabanı için Azure SQL Analytics kutucuğu seçildikten sonra, izleme panosu gösterilir.

![Azure SQL Analytics genel bakış](./media/azure-sql/azure-sql-sol-overview.png)

Kutucukların herhangi birini seçtiğinizde, belirli bir perspektife bir ayrıntıya gitme raporu açılır. Perspektif seçildikten sonra, ayrıntıya gitme raporu açılır.

![Azure SQL Analytics zaman aşımları](./media/azure-sql/azure-sql-sol-metrics.png)

Bu görünümdeki her bakış, abonelik, sunucu, elastik havuz ve veritabanı düzeyinde özetler sağlar. Bunlara ek olarak, her bir perspektifte, sağda rapora özgü bir perspektif gösterilmektedir. Listeden abonelik, sunucu, havuz veya veritabanı seçildiğinde ayrıntıya gitme devam eder.

### <a name="managed-instance-and-databases-in-managed-instance-view"></a>Yönetilen örnek görünümünde yönetilen örnek ve veritabanları

Veritabanları için Azure SQL Analytics kutucuğu seçildikten sonra, izleme panosu gösterilir.

![Azure SQL Analytics genel bakış](./media/azure-sql/azure-sql-sol-overview-mi.png)

Kutucukların herhangi birini seçtiğinizde, belirli bir perspektife bir ayrıntıya gitme raporu açılır. Perspektif seçildikten sonra, ayrıntıya gitme raporu açılır.

Yönetilen örnek görünümünü seçme, yönetilen örnek kullanımı, içerdiği veritabanları ve örnek genelinde yürütülen sorgularda telemetri hakkında ayrıntılı bilgi gösterir.

![Azure SQL Analytics zaman aşımları](./media/azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="perspectives"></a>Perspektifler

Aşağıdaki tabloda, biri Azure SQL veritabanı ve elastik havuzlar için, diğeri yönetilen örnek için olmak üzere iki Pano sürümü için desteklenen Perspektifler özetlenmektedir.

| Perspektif | Açıklama | SQL veritabanı ve elastik havuzlar desteği | Yönetilen örnek desteği |
| --- | ------- | ----- | ----- |
| Türe göre kaynak | İzlenen tüm kaynakları sayan perspektif. | Yes | Yes |
| Bilgiler | Akıllı İçgörüler, performans için hiyerarşik ayrıntıya gitme sağlar. | Yes | Yes |
| Hatalar | Veritabanlarında gerçekleşen SQL hatalarına hiyerarşik ayrıntıya gitme sağlar. | Yes | Yes |
| Zaman aşımları | Veritabanlarında gerçekleşen SQL zaman aşımları için hiyerarşik ayrıntıya gitme sağlar. | Yes | Hayır |
| Blok | Veritabanlarında gerçekleşen SQL blokları için hiyerarşik ayrıntıya gitme sağlar. | Yes | Hayır |
| Veritabanı bekleme süresi | Veritabanı düzeyinde SQL bekleme istatistikleri için hiyerarşik ayrıntıya gitme sağlar. Bekleme türü başına düşen toplam bekleme süresinin ve bekleme zamanının özetlerini içerir. |Yes | Yes |
| Sorgu süresi | Sorgu süresi, CPU kullanımı, veri GÇ kullanımı, günlük GÇ kullanımı gibi sorgu yürütme istatistiklerinde hiyerarşik ayrıntıya gitme sağlar. | Yes | Yes |
| Sorgu bekler | Bekleme kategorisine göre sorgu bekleme istatistikleri için hiyerarşik ayrıntıya gitme sağlar. | Yes | Yes |

### <a name="intelligent-insights-report"></a>Akıllı İçgörüler raporu

Azure SQL veritabanı [akıllı içgörüler](../../sql-database/sql-database-intelligent-insights.md) , tüm Azure SQL veritabanlarının performansı ile neler olduğunu bilmenizi sağlar. Toplanan tüm Akıllı İçgörüler öngörülere göre görselleştirilebilir ve erişilebilir.

![Azure SQL Analytics Öngörüler](./media/azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Elastik havuz ve veritabanı raporları

Esnek havuzlar ve SQL veritabanlarının her ikisi de kaynak için belirtilen zamanda toplanan tüm verileri gösteren özel raporlara sahiptir.

![Azure SQL Analytics veritabanı](./media/azure-sql/azure-sql-sol-database.png)

![Azure SQL elastik havuzu](./media/azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Sorgu raporları

Sorgu süresi ve sorgu perspektifleri bekler, sorgu raporu aracılığıyla herhangi bir sorgunun performansını ilişkilendirebilmeniz gerekir. Bu rapor, farklı veritabanları genelinde sorgu performansını karşılaştırır ve seçili sorgu için yavaş olan veritabanlarının yerini almanızı kolaylaştırır.

![Azure SQL Analytics sorguları](./media/azure-sql/azure-sql-sol-queries.png)

## <a name="permissions"></a>İzinler

Azure SQL Analytics kullanmak için, kullanıcıların Azure 'da okuyucu rolüne en az bir izin verilmesi gerekir. Ancak, bu rol, kullanıcıların sorgu metnini görmesine veya herhangi bir otomatik ayarlama eylemi gerçekleştirmesine izin vermez. Azure 'da çözümü en yüksek ölçüde kullanmaya izin veren daha fazla izin verilen rol, sahip, katkıda bulunan, SQL DB katılımcısı veya SQL Server katılımcısı. Ayrıca, portalda yalnızca Azure SQL Analytics kullanmak için gerekli izinleri olan ve diğer kaynakları yönetmeye yönelik erişim olmadan özel bir rol oluşturmayı düşünmek isteyebilirsiniz.

### <a name="creating-a-custom-role-in-portal"></a>Portalda özel bir rol oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bazı kuruluşların Azure 'da kesin izin denetimleri uyguladığını tanıma, aşağıdaki PowerShell komut dosyasını, için gereken en düşük okuma ve yazma izinlerine sahip Azure portal "SQL Analytics Izleme operatörü" özel rolünün oluşturulmasını etkinleştirme konusunda bulabilirsiniz. en uzun ölçüde Azure SQL Analytics kullanın.

Aşağıdaki betikteki "{SubscriptionID}" öğesini Azure abonelik KIMLIĞINIZLE değiştirin ve Azure 'da bir sahip veya katkıda bulunan rolü olarak oturum açmış betiği yürütün.

   ```powershell
    Connect-AzAccount
    Select-AzSubscription {SubscriptionId}
    $role = Get-AzRoleDefinition -Name Reader
    $role.Name = "SQL Analytics Monitoring Operator"
    $role.Description = "Lets you monitor database performance with Azure SQL Analytics as a reader. Does not allow change of resources."
    $role.IsCustom = $true
    $role.Actions.Add("Microsoft.SQL/servers/databases/read");
    $role.Actions.Add("Microsoft.SQL/servers/databases/topQueries/queryText/*");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/write");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Resources/deployments/write");
    $role.AssignableScopes = "/subscriptions/{SubscriptionId}"
    New-AzRoleDefinition $role
   ```

Yeni rol oluşturulduktan sonra, Azure SQL Analytics kullanmak için özel izinler vermeniz gereken her kullanıcıya bu rolü atayın.

## <a name="analyze-data-and-create-alerts"></a>Verileri çözümleme ve uyarı oluşturma

Azure SQL Analytics veri analizi, özel sorgulama ve raporlamanın [Log Analytics diline](../log-query/get-started-queries.md) bağlıdır. [Ölçüm ve günlüklerde](../../sql-database/sql-database-metrics-diag-logging.md#metrics-and-logs-available)bulunan özel sorgulama için veritabanı kaynağından toplanan kullanılabilir verilerin açıklamasını bulun.

Çözümdeki otomatik uyarı, bir koşulun karşılanmasıyla ilgili bir uyarıyı tetikleyen Log Analytics bir sorgu yazmaya dayalıdır. Çözümde uyarı ayarlanınca Log Analytics sorgularda birkaç örnek aşağıda bulabilirsiniz.

### <a name="creating-alerts-for-azure-sql-database"></a>Azure SQL veritabanı için uyarı oluşturma

Azure SQL veritabanı kaynaklarından gelen verilerle kolayca [uyarı oluşturabilirsiniz](../platform/alerts-metric.md) . Bir günlük uyarısıyla kullanabileceğiniz bazı yararlı [günlük sorguları](../log-query/log-query-overview.md) aşağıda verilmiştir:

#### <a name="high-cpu-on-azure-sql-database"></a>Azure SQL veritabanı 'nda yüksek CPU

```
AzureMetrics
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent"
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen veritabanlarının çözüme temel ölçümleri akışını sağlar.
> - Bunun yerine yüksek DTU sonuçları almak için cpu_percent MetricName değerini dtu_consumption_percent ile değiştirin.

#### <a name="high-cpu-on-azure-sql-database-elastic-pools"></a>Azure SQL veritabanı elastik havuzlarında yüksek CPU

```
AzureMetrics
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent"
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen veritabanlarının çözüme temel ölçümleri akışını sağlar.
> - Bunun yerine yüksek DTU sonuçları almak için cpu_percent MetricName değerini dtu_consumption_percent ile değiştirin.

#### <a name="azure-sql-database-storage-in-average-above-95-in-the-last-1-hr"></a>Son 1 saat içinde %95 üzerinde ortalama olan Azure SQL veritabanı depolaması

```
let time_range = 1h;
let storage_threshold = 95;
AzureMetrics
| where ResourceId contains "/DATABASES/"
| where MetricName == "storage_percent"
| summarize max_storage = max(Average) by ResourceId, bin(TimeGenerated, time_range)
| where max_storage > storage_threshold
| distinct ResourceId
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen veritabanlarının çözüme temel ölçümleri akışını sağlar.
> - Bu sorgu, sorgunun bazı veritabanlarında mevcut olduğunu belirten bir uyarı (> 0 sonuç) olduğunda bir uyarının tetikleneceği bir uyarı kuralının ayarlanmasını gerektirir. Çıktı, tanımlanan time_range içindeki storage_threshold üzerinde bulunan veritabanı kaynaklarının bir listesidir.
> - Çıktı, tanımlanan time_range içindeki storage_threshold üzerinde bulunan veritabanı kaynaklarının bir listesidir.

#### <a name="alert-on-intelligent-insights"></a>Akıllı Öngörüler üzerinde uyarı

```
let alert_run_interval = 1h;
let insights_string = "hitting its CPU limits";
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active"
| where TimeGenerated > ago(alert_run_interval)
| where rootCauseAnalysis_s contains insights_string
| distinct ResourceId
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen veritabanlarının çözüm için Sqlinsıghts tanılama günlüğü akışını sağlar.
> - Bu sorgu, yinelenen sonuçların oluşmaması için alert_run_interval ile aynı sıklıkta çalışacak şekilde ayarlanmış bir uyarı kuralı gerektirir. Sorgudan, bir sorgu (> 0 sonuç) varsa, uyarının tetiklenmesi için ayarlanmış olması gerekir.
> - Alert_run_interval ' i, koşulun Sqlinsıghts günlüğünü akışa almak üzere yapılandırılmış veritabanlarında oluşup gerçekleştiyse denetlenecek zaman aralığını belirlemek için özelleştirin.
> - İnsights_string ' i, Öngörüler kök nedeni analiz metninin çıkışını yakalamak için özelleştirin. Bu, çözümün Kullanıcı arabiriminde, mevcut öngörülerden kullanabileceğiniz aynı metindir. Alternatif olarak, aboneliğinizde oluşturulan tüm öngörülerin metnini görmek için aşağıdaki sorguyu kullanabilirsiniz. Öngörüler üzerinde uyarıları ayarlamaya yönelik ayrı dizeleri toplamak için sorgunun çıkışını kullanın.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active"
| distinct rootCauseAnalysis_s
```

### <a name="creating-alerts-for-managed-instance"></a>Yönetilen örnek için uyarı oluşturma

#### <a name="managed-instance-storage-is-above-90"></a>Yönetilen örnek depolama %90 üzerinde

```
let storage_percentage_threshold = 90;
AzureDiagnostics
| where Category =="ResourceUsageStats"
| summarize (TimeGenerated, calculated_storage_percentage) = arg_max(TimeGenerated, todouble(storage_space_used_mb_s) *100 / todouble (reserved_storage_mb_s))
   by ResourceId
| where calculated_storage_percentage > storage_percentage_threshold
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen yönetilen örnek, çözüm için ResourceUsageStats günlüğü akışının etkin olmasını sağlar.
> - Bu sorgu, sorgunun yönetilen örnekte mevcut olduğunu belirten, sorgudan bir uyarı (> 0 sonuç) olduğunda bir uyarının tetikleneceği bir uyarı kuralının ayarlanmasını gerektirir. Çıktı, yönetilen örnekteki depolama yüzdesi tüketimidir.

#### <a name="managed-instance-cpu-average-consumption-is-above-95-in-the-last-1-hr"></a>Yönetilen örnek CPU ortalama tüketimi, son 1 saat içinde %95 üzerinde.

```
let cpu_percentage_threshold = 95;
let time_threshold = ago(1h);
AzureDiagnostics
| where Category == "ResourceUsageStats" and TimeGenerated > time_threshold
| summarize avg_cpu = max(todouble(avg_cpu_percent_s)) by ResourceId
| where avg_cpu > cpu_percentage_threshold
```

> [!NOTE]
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen yönetilen örnek, çözüm için ResourceUsageStats günlüğü akışının etkin olmasını sağlar.
> - Bu sorgu, sorgunun yönetilen örnekte mevcut olduğunu belirten, sorgudan bir uyarı (> 0 sonuç) olduğunda bir uyarının tetikleneceği bir uyarı kuralının ayarlanmasını gerektirir. Çıktı, yönetilen örnekteki tanımlı dönemdeki ortalama CPU kullanımı yüzdesi tüketimidir.

### <a name="pricing"></a>Fiyatlandırma

Çözüm kullanımda olmakla birlikte, her ay ayrılan boş veri alma birimlerinin üzerinde tanılama telemetrisine ilişkin tüketim geçerlidir. [Log Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor)' na bakın. Sunulan ücretsiz veri alma birimleri, her ay birkaç veritabanının ücretsiz olarak izlenmesini sağlar. Daha ağır iş yüklerine sahip daha fazla etkin veritabanlarının, boştaki veritabanlarına göre daha fazla veri alan olduğunu unutmayın. Azure SQL Analytics gezinti menüsünde OMS çalışma alanı ' nı seçerek ve ardından kullanım ve tahmini maliyetler ' i seçerek, çözüm içindeki veri alımı kullanımını kolayca izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı Azure SQL verilerini görüntülemek için Azure Izleyici 'de [günlük sorguları](../log-query/log-query-overview.md) ' nı kullanın.
- Azure SQL verilerini gösteren [kendi panolarınızı oluşturun](../learn/tutorial-logs-dashboards.md) .
- Belirli Azure SQL olayları gerçekleştiğinde [uyarılar oluşturun](../platform/alerts-overview.md) .
