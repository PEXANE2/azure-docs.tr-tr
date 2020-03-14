---
title: Azure Izleyici 'de çözüm Azure SQL Analytics | Microsoft Docs
description: Azure SQL Analytics çözümünü, Azure SQL veritabanlarınızı yönetmenize yardımcı olur.
ms.subservice: logs
ms.topic: conceptual
author: danimir
ms.author: danil
ms.date: 02/21/2020
ms.reviewer: carlrab
ms.openlocfilehash: 921a05c4dc6c1d5cfa663ac71b469573b8f1925b
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79275470"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Azure SQL Analytics (Önizleme) kullanarak Azure SQL veritabanı izleme

![Azure SQL Analytics simgesi](./media/azure-sql/azure-sql-symbol.png)

Azure SQL Analytics, tek bir görünümdeki tüm Azure SQL veritabanlarınızın ve birden çok aboneliğin performansını izlemeye yönelik gelişmiş bir bulut izleme çözümüdür. Azure SQL Analytics performans sorunlarını gidermek için yerleşik zeka ile önemli performans ölçümlerini toplayıp görselleştirir.

Bu toplanan ölçümleri kullanarak, özel izleme kuralları ve uyarılar oluşturabilirsiniz. Azure SQL Analytics, uygulama yığınınızdaki her katmandaki sorunları belirlemenize yardımcı olur. Tek bir Log Analytics çalışma alanındaki tüm Azure SQL veritabanlarınızla ilgili verileri sunmak için Azure Izleyici görünümleri ile birlikte Azure tanılama ölçümlerini kullanır. Azure Izleyici, yapılandırılmış ve yapılandırılmamış verileri toplamanıza, ilişkilendirmenize ve görselleştirmenize yardımcı olur.

Azure SQL Analytics çözümünü kullanma uygulamalı bir genel bakış ve tipik kullanım senaryoları için katıştırılmış video bakın:

>[!VIDEO https://www.youtube.com/embed/j-NDkN4GIzg]
>

## <a name="connected-sources"></a>Bağlı kaynaklar

Azure SQL Analytics, tüm Azure SQL veritabanlarınızda tanılama telemetrisi akışını destekleyen bir yalnızca bulut izleme çözümüdür. Azure SQL Analytics Azure Izleyici 'ye bağlanmak için aracıları kullanmadığından, şirket içinde veya sanal makinelerde barındırılan SQL Server izlemeyi desteklemez.

| Bağlı Kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| [Tanılama ayarları](../platform/diagnostic-settings.md) | **Evet** | Azure ölçümü ve günlük verileri Azure Izleyici günlüklerine doğrudan Azure tarafından gönderilir. |
| [Azure depolama hesabı](../platform/collect-azure-metrics-logs.md) | Hayır | Azure Izleyici, bir depolama hesabındaki verileri okuyamıyor. |
| [Windows aracıları](../platform/agent-windows.md) | Hayır | Doğrudan Windows aracıları Azure SQL Analytics tarafından kullanılmaz. |
| [Linux aracıları](../learn/quick-collect-linux-computer.md) | Hayır | Doğrudan Linux aracıları Azure SQL Analytics tarafından kullanılmaz. |
| [System Center Operations Manager yönetim grubu](../platform/om-agents.md) | Hayır | Operations Manager aracısından Azure Izleyici 'ye doğrudan bağlantı Azure SQL Analytics tarafından kullanılmaz. |

## <a name="azure-sql-analytics-options"></a>Azure SQL Analytics seçenekleri

Aşağıdaki tabloda, biri tek ve havuza alınmış veritabanları ve elastik havuzlar için, diğeri ise yönetilen örnekler ve örnek veritabanları için Azure SQL Analytics panosunun iki sürümü için desteklenen seçenekler özetlenmektedir.

| Azure SQL Analytics seçeneği | Açıklama | Tek ve havuza alınmış veritabanı ve elastik havuzlar desteği | Yönetilen örnekler ve örnekler veritabanı desteği |
| --- | ------- | ----- | ----- |
| Kaynak türüne göre | Bu perspektif izlenen tüm kaynakları sayar. | Yes | Yes |
| Insights | Hiyerarşik detaya gitme performans akıllı Öngörüler sağlar. | Yes | Yes |
| Hatalar | Hiyerarşik detaya gitme veritabanlarında meydana gelen hatalara SQL sağlar. | Yes | Yes |
| Zaman aşımları | Hiyerarşik detaya gitme veritabanlarında gerçekleşen SQL zaman aşımları sağlar. | Yes | Hayır |
| Durdurmalar | Hiyerarşik detaya gitme veritabanlarında gerçekleşen SQL blockings sağlar. | Yes | Hayır |
| Veritabanı beklemeleri | Hiyerarşik detaya gitme SQL bekleme istatistikleri veritabanı düzeyi sağlar. Toplam bekleme süresi ve bekleme türü başına bekleme süresi bir özetini içerir. |Yes | Hayır |
| Sorgu süresi | Sorgu yürütme istatistikleri sorgu süresi, CPU kullanımı, veri GÇ kullanımını, günlük GÇ kullanımını gibi hiyerarşik detaya gitme sağlar. | Yes | Yes |
| Sorgu beklemeleri | Hiyerarşik detaya gitme sorgu bekleme istatistikleri bekleme kategoriye göre sağlar. | Yes | Yes |

## <a name="configuration"></a>Yapılandırma

Log Analytics çalışma alanınıza Azure SQL Analytics (Önizleme) eklemek için [Çözüm Galerisi Azure izleyici çözümlerini ekleme](../../azure-monitor/insights/solutions.md) bölümünde açıklanan işlemi kullanın.

### <a name="configure-azure-sql-databases-to-stream-diagnostics-telemetry"></a>Azure SQL veritabanlarını, tanılama telemetrisini akışa yönelik olarak yapılandırma

Çalışma alanınızda Azure SQL Analytics çözüm oluşturduktan sonra, tanılama telemetrisini Azure SQL Analytics akışa almak için izlemek istediğiniz **her kaynağı yapılandırmanız** gerekir. Bu sayfada ayrıntılı yönergeleri izleyin:

- [Tanılama telemetrisini Azure SQL Analytics akışa](../../sql-database/sql-database-metrics-diag-logging.md)almak IÇIN Azure SQL veritabanınızın Azure tanılama etkinleştirin.

Yukarıdaki sayfayı ayrıca tek bir Azure SQL Analytics çalışma alanı birden çok Azure aboneliklerinde tek bir cam bölmeyle izlemek için destek etkinleştirme hakkında yönergeler sağlar.

## <a name="using-azure-sql-analytics"></a>Azure SQL Analytics kullanma

Çalışma alanınıza Azure SQL Analytics eklediğinizde, Azure SQL Analytics kutucuk çalışma alanınıza eklenir ve genel bakış ' da görünür. Kutucuk içeriğini yüklemek için Özeti görüntüle bağlantısını seçin.

![Azure SQL Analytics Özet kutucuğu](./media/azure-sql/azure-sql-sol-tile-01.png)

Bu kutucuk, yüklendikten sonra tek ve havuza alınmış veritabanlarının sayısını, elastik havuzları, yönetilen örnekleri ve Azure SQL Analytics tanılama telemetrisini alan yönetilen örnek veritabanlarını gösterir.

![Azure SQL Analytics kutucuğuna](./media/azure-sql/azure-sql-sol-tile-02.png)

Azure SQL Analytics, tek veritabanlarını ve havuza alınmış veritabanlarını ve elastik havuzları izlemek için bir tane olmak üzere iki ayrı görünüm sağlar ve yönetilen örnekleri ve örnek veritabanlarını izlemeye yönelik diğer görünümü içerir.

Tek ve havuza alınmış veritabanları ve elastik havuzların Azure SQL Analytics izleme panosunu görüntülemek için kutucuğun üst kısmına tıklayın. Yönetilen örnekler ve örnek veritabanları için Azure SQL Analytics izleme panosunu görüntülemek için kutucuğun alt kısmına tıklayın.

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL Analytics verileri görüntüleme

Pano farklı perspektiflerini izlenen tüm veritabanlarının genel bakış içerir. Farklı perspektiflerin çalışması için, SQL kaynaklarınızın Log Analytics çalışma alanına akışını sağlamak üzere uygun ölçümleri veya günlükleri etkinleştirmeniz gerekir.

Bazı ölçümler veya Günlükler Azure Izleyici 'ye akışı yoksa, Azure SQL Analytics içindeki kutucuklar izleme bilgileriyle doldurulmaz.

### <a name="single-and-pooled-databases-and-elastic-pools-view"></a>Tek ve havuza alınmış veritabanları ve elastik havuzlar görünümü

Veritabanını Azure SQL Analytics kutucuğuna seçildikten sonra izleme Panosu gösterilmektedir.

![Azure SQL Analytics genel bakış](./media/azure-sql/azure-sql-sol-overview.png)

Herhangi bir döşeme seçerek, belirli bir perspektife detaya gitme rapor açılır. Perspektif seçildikten sonra detaya gitme rapor açılır.

![Azure SQL Analytics zaman aşımları](./media/azure-sql/azure-sql-sol-metrics.png)

Bu görünümdeki her bakış, abonelik, sunucu, elastik havuz ve veritabanı düzeylerinde özetler sağlar. Ayrıca, her bir perspektif bir perspektif sağ tarafta raporuna özgü gösterir. Abonelik, sunucu, havuz veya veritabanı listeden seçerek detaya gitme devam eder.

### <a name="managed-instance-and-instances-databases-view"></a>Yönetilen örnek ve örnekler veritabanları görünümü

Veritabanlarını Azure SQL Analytics kutucuğuna seçildikten sonra izleme Panosu gösterilmektedir.

![Azure SQL Analytics genel bakış](./media/azure-sql/azure-sql-sol-overview-mi.png)

Herhangi bir döşeme seçerek, belirli bir perspektife detaya gitme rapor açılır. Perspektif seçildikten sonra detaya gitme rapor açılır.

Yönetilen örnek görünümünü seçtiğinizde, yönetilen örnek kullanımı, içerdiği veritabanları ve örnek genelinde yürütülen sorgularda telemetri ile ilgili ayrıntılar gösterilir.

![Azure SQL Analytics zaman aşımları](./media/azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="intelligent-insights-report"></a>Akıllı İçgörüler raporu

Azure SQL veritabanı [akıllı içgörüler](../../sql-database/sql-database-intelligent-insights.md) , tüm Azure SQL veritabanlarının performansı ile neler olduğunu bilmenizi sağlar. Toplanan tüm akıllı İçgörüler görselleştirileceğini ve öngörüleri perspektif erişilebilir.

![Azure SQL Analytics öngörüleri](./media/azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pools-and-database-reports"></a>Elastik havuzlar ve veritabanı raporları

Esnek havuzların ve veritabanlarının her ikisi de kaynak için belirtilen zamanda toplanan tüm verileri gösteren özel raporlara sahiptir.

![Azure SQL Analytics veritabanı](./media/azure-sql/azure-sql-sol-database.png)

![Azure SQL esnek havuzu](./media/azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Sorgu raporları

Sorgu süresi ve sorgu perspektifleri bekler, sorgu raporu aracılığıyla herhangi bir sorgunun performansını ilişkilendirebilmeniz gerekir. Bu rapor, farklı veritabanları arasında sorgu performansını karşılaştırır ve de yavaş olan olanları karşı seçili sorguyu gerçekleştirmek veritabanları saptamak kolaylaştırır.

![Azure SQL Analytics sorguları](./media/azure-sql/azure-sql-sol-queries.png)

## <a name="permissions"></a>İzinler

Azure SQL Analytics kullanmak için kullanıcıların azure'daki okuyucu rolünün en az bir izin verilmesi gerekir. Bu rol, ancak değil sorgu metni görmesine olanak veya tüm otomatik ayarlama eylemleri gerçekleştirin. Azure 'da, en yüksek ölçüde Azure SQL Analytics kullanılmasına izin veren daha fazla izin verilen roller sahip, katkıda bulunan, SQL DB katılımcısı veya SQL Server katılımcısı. Yalnızca Azure SQL Analytics kullanmak için gereken belirli izinleri ile ve diğer kaynakları yönetmek için erişim olmaksızın Portalı'nda özel rol oluşturma düşünmek isteyebilirsiniz.

### <a name="creating-a-custom-role-in-portal"></a>Portalda özel rol oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bazı kuruluşların azure'daki katı izin denetimleri zorunlu tanıma, en düşük Azure portalıyla "SQL Analytics izleme operatörü" okuma ve yazma izinleri için gereken özel bir rol oluşturulmasını etkinleştirme aşağıdaki PowerShell Betiği bulma Azure SQL Analytics tam boyutuna kullanın.

"{Subscriptionıd}" Değiştir aşağıdaki betiği, Azure abonelik kimliği ile bir Azure sahibi veya katkıda bulunan rolü olarak günlüğe bu betiği yürütün.

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

Yeni rol oluşturulduktan sonra Azure SQL Analytics kullanmak için özel izinleri vermek için gereken her kullanıcıya bu rolü atayın.

## <a name="analyze-data-and-create-alerts"></a>Verileri analiz etmek ve uyarılar oluşturun

Azure SQL Analytics veri analizi, özel sorgulama ve raporlamanın [Log Analytics diline](../log-query/get-started-queries.md) bağlıdır. [Ölçüm ve günlüklerde](../../sql-database/sql-database-metrics-diag-logging.md#metrics-and-logs-available)bulunan özel sorgulama için veritabanı kaynağından toplanan kullanılabilir verilerin açıklamasını bulun.

Azure SQL Analytics otomatik uyarı, bir koşulun karşılanmasıyla ilgili uyarıyı tetikleyen bir Log Analytics sorgusunun yazılmasına bağlıdır. Azure SQL Analytics ' de uyarı ayarlantırabileceği Log Analytics sorgularda birkaç örnek aşağıda bulabilirsiniz.

### <a name="creating-alerts-for-azure-sql-database"></a>Uyarılar için Azure SQL veritabanı oluşturma

Azure SQL veritabanı kaynaklarından gelen verilerle kolayca [uyarı oluşturabilirsiniz](../platform/alerts-metric.md) . Bir günlük uyarısıyla kullanabileceğiniz bazı yararlı [günlük sorguları](../log-query/log-query-overview.md) aşağıda verilmiştir:

#### <a name="high-cpu-on-azure-sql-database"></a>Azure SQL veritabanı yüksek CPU

```
AzureMetrics
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent"
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
>
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen veritabanlarının Azure SQL Analytics için temel ölçümleri akışıdır.
> - MetricName değer cpu_percent dtu_consumption_percent yüksek DTU sonuçları yerine elde etmek için'ile değiştirin.

#### <a name="high-cpu-on-azure-sql-database-elastic-pools"></a>Azure SQL veritabanı elastik havuzlar yüksek CPU

```
AzureMetrics
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent"
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
>
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen veritabanlarının Azure SQL Analytics için temel ölçümleri akışıdır.
> - MetricName değer cpu_percent dtu_consumption_percent yüksek DTU sonuçları yerine elde etmek için'ile değiştirin.

#### <a name="azure-sql-database-storage-in-average-above-95-in-the-last-1-hr"></a>Ortalamanın üstünde %95 son 1 saat içinde Azure SQL veritabanı depolama

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
>
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen veritabanlarının Azure SQL Analytics için temel ölçümleri akışıdır.
> - Bu sorgu bir uyarı kuralı sorgudan koşul bazı veritabanlarında bulunduğunu belirten bir uyarı sonuçlar (> 0 sonuç) varken ateşlenmesine kurulu olmasını gerektirir. Çıktı, yukarıda tanımlanan time_range içinde storage_threshold olan veritabanı kaynakların listesidir.
> - Çıktı, yukarıda tanımlanan time_range içinde storage_threshold olan veritabanı kaynakların listesidir.

#### <a name="alert-on-intelligent-insights"></a>Akıllı Öngörüler uyar

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
>
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen veritabanlarının Azure SQL Analytics için Sqlinsıghts tanılama günlüğünü açmasını sağlar.
> - Bu sorgu, yinelenen sonuçlar önlemek için aynı sıklıkta alert_run_interval olarak çalıştırılmak üzere ayarlanması için bir uyarı kuralı gerektirir. Sonuçlar (> 0 sonuç) bulunduğunda uyarıyı sorgudan ateşlenmesine kural ayarlanmış olması.
> - Alert_run_interval, koşulun Sqlinsıghts Azure SQL Analytics günlüğünü akışa almak üzere yapılandırılan veritabanlarında oluşup olmadığını denetlemek için zaman aralığını belirlemek üzere ' i özelleştirin.
> - Insights kök neden analizi metin çıktısını yakalamak için insights_string özelleştirin. Bu, mevcut Öngörüler arasından kullanabileceğiniz Azure SQL Analytics Kullanıcı arabiriminde görüntülenen aynı metindir. Alternatif olarak, tüm öngörü aboneliğinizde oluşturulan metin görmek için aşağıdaki sorguyu kullanabilirsiniz. Insights uyarıları ayarlamak için ayrı dizeleri Hasat için sorgunun çıkışı kullanın.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active"
| distinct rootCauseAnalysis_s
```

### <a name="creating-alerts-for-managed-instances"></a>Yönetilen örnekler için uyarı oluşturma

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
>
> - Bu uyarıyı ayarlamanın ön gerekliliği, yönetilen örnek tarafından izlenen Azure SQL Analytics için ResourceUsageStats günlüğü akışının etkin olmasını sağlar.
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
>
> - Bu uyarıyı ayarlamanın ön gerekliliği, izlenen yönetilen örnek Azure SQL Analytics için ResourceUsageStats günlüğü akışının etkin olmasını sağlar.
> - Bu sorgu, sorgunun yönetilen örnekte mevcut olduğunu belirten, sorgudan bir uyarı (> 0 sonuç) olduğunda bir uyarının tetikleneceği bir uyarı kuralının ayarlanmasını gerektirir. Çıktı, yönetilen örnekteki tanımlı dönemdeki ortalama CPU kullanımı yüzdesi tüketimidir.

### <a name="pricing"></a>Fiyatlandırma

Azure SQL Analytics kullanımı ücretsizdir, her ay ayrılan boş veri alma birimlerinin üzerinde tanılama telemetrinin tüketimi geçerlidir, bkz. [Log Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor). Sağlanan veri alımı ücretsiz birimlerinin ücretsiz çeşitli veritabanları her ay izlemeyi etkinleştirin. Daha yoğun iş yüklerine sahip daha fazla veri ve boştaki veritabanlarına kıyasla daha fazla etkin veritabanı. Azure SQL Analytics gezinti menüsünde OMS çalışma alanı ' nı ve ardından kullanım ve tahmini maliyetler ' i seçerek Azure SQL Analytics veri alımı kullanımını kolayca izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı Azure SQL verilerini görüntülemek için Azure Izleyici 'de [günlük sorguları](../log-query/log-query-overview.md) ' nı kullanın.
- Azure SQL verilerini gösteren [kendi panolarınızı oluşturun](../learn/tutorial-logs-dashboards.md) .
- Belirli Azure SQL olayları gerçekleştiğinde [uyarılar oluşturun](../platform/alerts-overview.md) .
