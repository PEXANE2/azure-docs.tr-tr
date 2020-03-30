---
title: Azure Monitör'de Azure SQL Analytics çözümü | Microsoft Dokümanlar
description: Azure SQL Analytics çözümü, Azure SQL veritabanlarınızı yönetmenize yardımcı olur
ms.subservice: logs
ms.topic: conceptual
author: danimir
ms.author: danil
ms.date: 02/21/2020
ms.reviewer: carlrab
ms.openlocfilehash: 921a05c4dc6c1d5cfa663ac71b469573b8f1925b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275470"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Azure SQL Analytics 'i kullanarak Azure SQL Veritabanını İzle (Önizleme)

![Azure SQL Analytics simgesi](./media/azure-sql/azure-sql-symbol.png)

Azure SQL Analytics, tüm Azure SQL veritabanlarınızın performansını ölçekte ve birden çok abonelik genelinde tek bir görünümde izlemek için gelişmiş bir bulut izleme çözümüdür. Azure SQL Analytics, performans sorun giderme için yerleşik zekayla önemli performans ölçümlerini toplar ve görselleştirir.

Toplanan bu ölçümleri kullanarak, özel izleme kuralları ve uyarıları oluşturabilirsiniz. Azure SQL Analytics, uygulama yığınınızın her katmanındaki sorunları belirlemenize yardımcı olur. Tek bir Log Analytics çalışma alanında tüm Azure SQL veritabanlarınız hakkındaki verileri sunmak için Azure Monitor görünümleriyle birlikte Azure Tanılama ölçümlerini kullanır. Azure Monitör, yapılandırılmış ve yapılandırılmamış verileri toplamanıza, ilişkilendirmenize ve görselleştirmenize yardımcı olur.

Azure SQL Analytics çözümlerini kullanma ve tipik kullanım senaryoları hakkında uygulamalı bir bakış için katıştırılmış videoya bakın:

>[!VIDEO https://www.youtube.com/embed/j-NDkN4GIzg]
>

## <a name="connected-sources"></a>Bağlı kaynaklar

Azure SQL Analytics, tüm Azure SQL veritabanlarınız için tanılama telemetrisinin akışını destekleyen bir bulut yalnızca izleme çözümüdür. Azure SQL Analytics, Azure Monitor'a bağlanmak için aracılar kullanmadığından, şirket içinde veya sanal makinelerde barındırılan SQL Server'ın izlenmesini desteklemez.

| Bağlı Kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| [Tanılama ayarları](../platform/diagnostic-settings.md) | **Evet** | Azure metrik ve günlük verileri doğrudan Azure Monitör Günlükleri'ne Azure tarafından gönderilir. |
| [Azure depolama hesabı](../platform/collect-azure-metrics-logs.md) | Hayır | Azure Monitor, bir depolama hesabındaki verileri okumaz. |
| [Windows aracıları](../platform/agent-windows.md) | Hayır | Doğrudan Windows aracıları Azure SQL Analytics tarafından kullanılmaz. |
| [Linux aracıları](../learn/quick-collect-linux-computer.md) | Hayır | Doğrudan Linux aracıları Azure SQL Analytics tarafından kullanılmaz. |
| [System Center Operations Manager yönetim grubu](../platform/om-agents.md) | Hayır | İşlem Yöneticisi aracısından Azure Monitor'a doğrudan bağlantı Azure SQL Analytics tarafından kullanılmaz. |

## <a name="azure-sql-analytics-options"></a>Azure SQL Analytics seçenekleri

Aşağıdaki tabloda, biri tek ve havuzlu veritabanları ve elastik havuzlar, diğeri ise yönetilen örnekler ve örnek veritabanları olmak üzere Azure SQL Analytics panosunun iki sürümü için desteklenen seçenekler sıralanmaktadır.

| Azure SQL Analytics seçeneği | Açıklama | Tek ve havuzlu veritabanı ve elastik havuzdesteği | Yönetilen örnekler ve örnekler veritabanı desteği |
| --- | ------- | ----- | ----- |
| Türüne göre kaynak | İzlenen tüm kaynakları sayan perspektif. | Evet | Evet |
| Insights | Intelligent Insights'ı performansa doğru hiyerarşik olarak ayrıntılandırmasağlar. | Evet | Evet |
| Hatalar | Veritabanlarında gerçekleşen SQL hatalarına hiyerarşik olarak ayrıntılandırma sağlar. | Evet | Evet |
| Zaman aşımları | Veritabanlarında gerçekleşen SQL zaman larını hiyerarşik olarak ayrıntılandırmasağlar. | Evet | Hayır |
| Engellemeler | Veritabanlarında gerçekleşen SQL engellemelerine hiyerarşik ayrıntılandırma sağlar. | Evet | Hayır |
| Veritabanı bekler | Veritabanı düzeyinde SQL bekleme istatistikleri içine hiyerarşik delinme sağlar. Toplam bekleme süresinin ve bekleme türü başına bekleme süresinin özetlerini içerir. |Evet | Hayır |
| Sorgu süresi | Sorgu süresi, CPU kullanımı, Veri IO kullanımı, Log IO kullanımı gibi sorgu yürütme istatistiklerine hiyerarşik delinme sağlar. | Evet | Evet |
| Sorgu beklemeleri | Bekleme kategorisine göre sorgu bekleme istatistiklerine hiyerarşik ayrıntılandırma sağlar. | Evet | Evet |

## <a name="configuration"></a>Yapılandırma

Log Analytics çalışma alanınıza Azure SQL Analytics (Önizleme) eklemek için [Çözümler Galerisi'nden Azure Monitörü Ekle çözümlerinde](../../azure-monitor/insights/solutions.md) açıklanan işlemi kullanın.

### <a name="configure-azure-sql-databases-to-stream-diagnostics-telemetry"></a>Azure SQL veritabanlarını tanılama telemetrisini aktarmak için yapılandırın

Çalışma alanınızda Azure SQL Analytics çözümünü oluşturduktan sonra, tanılama telemetrisini Azure SQL Analytics'e aktarmak için izlemek istediğiniz her kaynağı **yapılandırmanız** gerekir. Bu sayfadaki ayrıntılı talimatları izleyin:

- Azure SQL veritabanınız için Azure Tanılama'yı etkinleştirin ve [tanılama telemetrisini Azure SQL Analytics'e aktarın.](../../sql-database/sql-database-metrics-diag-logging.md)

Yukarıdaki sayfa, tek bir Azure SQL Analytics çalışma alanından tek bir cam bölme olarak birden fazla Azure aboneliğini izlemek için destek etkinleştirme yle ilgili yönergeler de sağlar.

## <a name="using-azure-sql-analytics"></a>Azure SQL Analytics'i kullanma

Çalışma alanınıza Azure SQL Analytics eklediğinizde, Azure SQL Analytics döşemesi çalışma alanınıza eklenir ve Genel Bakış'ta görünür. Döşeme içeriğini yüklemek için Özet'i Görüntüle bağlantısını seçin.

![Azure SQL Analytics özet döşemesi](./media/azure-sql/azure-sql-sol-tile-01.png)

Yüklendikten sonra, döşeme, tek ve havuzlu veritabanlarının, esnek havuzların, yönetilen örneklerin ve Azure SQL Analytics'in tanılama telemetrisi aldığı yönetilen örnek veritabanlarının sayısını gösterir.

![Azure SQL Analytics döşemesi](./media/azure-sql/azure-sql-sol-tile-02.png)

Azure SQL Analytics, biri tek veritabanları, havuzlu veritabanları ve elastik havuzları izlemek, diğeri ise yönetilen örnekleri ve örnek veritabanlarını izlemek için olmak üzere iki ayrı görünüm sağlar.

Tek ve havuzlu veritabanları ve esnek havuzlar için Azure SQL Analytics izleme panosunu görüntülemek için döşemenin üst kısmına tıklayın. Yönetilen örnekler ve örnek veritabanları için Azure SQL Analytics izleme panosunu görüntülemek için döşemenin alt kısmına tıklayın.

### <a name="viewing-azure-sql-analytics-data"></a>Azure SQL Analytics verilerini görüntüleme

Pano, farklı bakış açıları yla izlenen tüm veritabanlarının genel görünümünü içerir. Farklı perspektiflerin çalışması için, SQL kaynaklarınızdaki uygun ölçümlerin veya günlüklerin Log Analytics çalışma alanına aktarılmasını etkinleştirmeniz gerekir.

Azure Monitor'a bazı ölçümler veya günlükler aktarılmazsa, Azure SQL Analytics'teki kutucuklar izleme bilgileriyle doldurulmez.

### <a name="single-and-pooled-databases-and-elastic-pools-view"></a>Tek ve havuzlu veritabanları ve elastik havuzgörünümü

Veritabanı için Azure SQL Analytics döşemesi seçildikten sonra izleme panosu gösterilir.

![Azure SQL Analytics'e Genel Bakış](./media/azure-sql/azure-sql-sol-overview.png)

Döşemelerden herhangi birini seçmek, belirli bir perspektife bir ayrıntı raporu açar. Perspektif seçildikten sonra, ayrıntılandırma raporu açılır.

![Azure SQL Analytics Zaman Dilimleri](./media/azure-sql/azure-sql-sol-metrics.png)

Bu görünümdeki her perspektif, abonelik, sunucu, elastik havuz ve veritabanı düzeylerinde özetler sağlar. Buna ek olarak, her perspektif sağdaki rapora özgü bir perspektif gösterir. Listeden abonelik, sunucu, havuz veya veritabanı seçimi demlemeye devam ediyor.

### <a name="managed-instance-and-instances-databases-view"></a>Yönetilen örnek ve örnek veritabanları görünümü

Veritabanları için Azure SQL Analytics döşemesi seçildikten sonra izleme panosu gösterilir.

![Azure SQL Analytics'e Genel Bakış](./media/azure-sql/azure-sql-sol-overview-mi.png)

Döşemelerden herhangi birini seçmek, belirli bir perspektife bir ayrıntı raporu açar. Perspektif seçildikten sonra, ayrıntılandırma raporu açılır.

Yönetilen örnek görünümünü seçmek, yönetilen örnek kullanımıyla ilgili ayrıntıları, içerdiği veritabanlarını ve örnek genelinde yürütülen sorgularda telemetriyi gösterir.

![Azure SQL Analytics Zaman Dilimleri](./media/azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="intelligent-insights-report"></a>Akıllı Öngörüler raporu

Azure SQL Veritabanı [Akıllı Öngörüleri,](../../sql-database/sql-database-intelligent-insights.md) tüm Azure SQL veritabanlarının performansında neler olduğunu bilmenizi sağlar. Toplanan tüm Akıllı Bilgiler, Insights perspektifi üzerinden görselleştirilebilir ve erişilebilir.

![Azure SQL Analytics Öngörüleri](./media/azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pools-and-database-reports"></a>Elastik havuzlar ve veritabanı raporları

Hem elastik havuzları n hem de veritabanları, kaynak için toplanan tüm verileri belirtilen süre içinde gösteren kendi özel raporlarına sahiptir.

![Azure SQL Analytics Veritabanı](./media/azure-sql/azure-sql-sol-database.png)

![Azure SQL elastik havuzu](./media/azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Sorgu raporları

Sorgu süresi ve sorgu perspektifleri bekler sayesinde, sorgu raporu aracılığıyla herhangi bir sorgunun performansını ilişkilendirebilirsiniz. Bu rapor, sorgu performansını farklı veritabanları arasında karşılaştırır ve seçili sorguyu iyi gerçekleştiren veritabanlarını yavaş olanlarla ayırt etmeyi kolaylaştırır.

![Azure SQL Analytics Sorguları](./media/azure-sql/azure-sql-sol-queries.png)

## <a name="permissions"></a>İzinler

Azure SQL Analytics'i kullanmak için kullanıcılara Azure'daki Reader rolü için en az izin verilmesi gerekir. Ancak bu rol, kullanıcıların sorgu metnini görmesine veya otomatik ayar eylemleri gerçekleştirmesine izin vermez. Azure'da Azure SQL Analytics'in sonuna kadar kullanılmasına izin veren daha fazla izin veren roller Sahibi, Katılımcısı, SQL DB Katılımcısı veya SQL Server Katılımcısı'dır. Ayrıca, portalda yalnızca Azure SQL Analytics'i kullanmak için gereken belirli izinlerle ve diğer kaynakları yönetmeye erişimi olmayan özel bir rol oluşturmayı da düşünebilirsiniz.

### <a name="creating-a-custom-role-in-portal"></a>Portalda özel bir rol oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bazı kuruluşların Azure'da sıkı izin denetimleri uyguladığını kabul ederek, Azure portalında en az okuma ve yazma izinleri ile özel bir rol "SQL Analytics Monitoring Operator" oluşturulmasını sağlayan aşağıdaki PowerShell komut dosyasını bulun Azure SQL Analytics'i en iyi şekilde kullanın.

Aşağıdaki komut dosyasındaki "{SubscriptionId}" ile Azure abonelik kimliğinizin değiştirilmesini ve Azure'da Sahip veya Katılımcı rolü olarak oturum açmış komut dosyasını çalıştırın.

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

Yeni rol oluşturulduktan sonra, Azure SQL Analytics'i kullanmak için özel izinler vermeniz gereken her kullanıcıya bu rolü atayın.

## <a name="analyze-data-and-create-alerts"></a>Verileri analiz edin ve uyarılar oluşturun

Azure SQL Analytics'teki veri analizi, özel sorgulama ve raporlamanız için [Log Analytics dilini](../log-query/get-started-queries.md) temel alınarak yapılır. [Kullanılabilir ölçümlerde ve günlüklerde](../../sql-database/sql-database-metrics-diag-logging.md#metrics-and-logs-available)özel sorgulama için veritabanı kaynağından toplanan kullanılabilir verilerin açıklamasını bulun.

Azure SQL Analytics'te otomatik uyarı, bir koşulun karşılanması nda bir uyarı tetikleyen bir Log Analytics sorgusu yazmaya dayanır. Azure SQL Analytics'te uyarının ayarlanabileceği Log Analytics sorgularında aşağıdaki örnekleri aşağıda bulabilirsiniz.

### <a name="creating-alerts-for-azure-sql-database"></a>Azure SQL Veritabanı için uyarı oluşturma

Azure SQL Veritabanı kaynaklarından gelen verilerle kolayca [uyarılar oluşturabilirsiniz.](../platform/alerts-metric.md) Günlük uyarısı ile kullanabileceğiniz bazı yararlı [günlük sorguları](../log-query/log-query-overview.md) aşağıda veda edebilirsiniz:

#### <a name="high-cpu-on-azure-sql-database"></a>Azure SQL Veritabanında Yüksek İşlemci

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
> - Bu uyarıyı ayarlamanın ön şartı, izlenen veritabanlarının temel ölçümleri Azure SQL Analytics'e aktarmasıdır.
> - Bunun yerine yüksek DTU sonuçları elde etmek için MetricName değerini cpu_percent dtu_consumption_percent değiştirin.

#### <a name="high-cpu-on-azure-sql-database-elastic-pools"></a>Azure SQL Veritabanı elastik havuzlarında yüksek işlemci

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
> - Bu uyarıyı ayarlamanın ön şartı, izlenen veritabanlarının Temel ölçümleri Azure SQL Analytics'e aktarmasıdır.
> - Bunun yerine yüksek DTU sonuçları elde etmek için MetricName değerini cpu_percent dtu_consumption_percent değiştirin.

#### <a name="azure-sql-database-storage-in-average-above-95-in-the-last-1-hr"></a>Son 1 saat içinde ortalama %95'in üzerinde Azure SQL Veritabanı depolama

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
> - Bu uyarıyı ayarlamanın ön şartı, izlenen veritabanlarının temel ölçümleri Azure SQL Analytics'e aktarmasıdır.
> - Bu sorgu, sorgudan sonuç (> 0 sonuç) olduğunda bir uyarıyı kapatmak için ayarlanacak bir uyarı kuralı gerektirir ve bu durum bazı veritabanlarında var olduğunu gösterir. Çıktı, tanımlanan time_range içinde storage_threshold üzerinde olan veritabanı kaynaklarının listesidir.
> - Çıktı, tanımlanan time_range içinde storage_threshold üzerinde olan veritabanı kaynaklarının listesidir.

#### <a name="alert-on-intelligent-insights"></a>Akıllı öngörüler konusunda uyarı

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
> - Bu uyarıyı ayarlamanın ön şartı, izlenen veritabanlarının SQLInsights tanılama günlüğünü Azure SQL Analytics'e aktarmasıdır.
> - Bu sorgu, yinelenen sonuçları önlemek için alert_run_interval ile aynı sıklıkta çalışacak bir uyarı kuralı nın ayarlanmasını gerektirir. Kural, sorgudan sonuçlar (> 0 sonuç) olduğunda uyarıyı kapatmak için ayarlanmalıdır.
> - SQLInsights günlüğünü Azure SQL Analytics'e aktarmak üzere yapılandırılan veritabanlarında durumun oluşmuş olup olmadığını kontrol etmek için alert_run_interval özelleştirin.
> - Insights kök neden çözümleme metninin çıktısını yakalamak için insights_string özelleştirin. Bu, Mevcut öngörülerden kullanabileceğiniz Azure SQL Analytics kullanıcı arabirimi'nde görüntülenen metinle aynıdır. Alternatif olarak, aboneliğinizde oluşturulan tüm Öngörülerin metnini görmek için aşağıdaki sorguyu kullanabilirsiniz. Öngörüler üzerinde uyarılar ayarlamak için farklı dizeleri hasat için sorgu çıktısını kullanın.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active"
| distinct rootCauseAnalysis_s
```

### <a name="creating-alerts-for-managed-instances"></a>Yönetilen durumlar için uyarılar oluşturma

#### <a name="managed-instance-storage-is-above-90"></a>Yönetilen örnek depolama % 90'ın üzerinde

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
> - Bu uyarıyı ayarlamanın ön gereksinimi, yönetilen örneğin izlenen Kaynak KullanımıStats günlüğünün Azure SQL Analytics'e etkin akışına sahip olmasıdır.
> - Bu sorgu, sorgudan sonuç (> 0 sonuç) olduğunda, yönetilen örnekte koşulun var olduğunu gösteren bir uyarı kuralının ayarlanmasını gerektirir. Çıktı, yönetilen örnekteki depolama yüzdesi tüketimidir.

#### <a name="managed-instance-cpu-average-consumption-is-above-95-in-the-last-1-hr"></a>Yönetilen örnek CPU ortalama tüketimi son 1 saat içinde% 95 üzerindedir

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
> - Bu uyarıyı ayarlamanın ön şartı, izlenen yönetilen örneğin Azure SQL Analytics'e bağlı ResourceUsageStats günlüğünün akışına sahip olmasıdır.
> - Bu sorgu, sorgudan sonuç (> 0 sonuç) olduğunda, yönetilen örnekte koşulun var olduğunu gösteren bir uyarı kuralının ayarlanmasını gerektirir. Çıktı, yönetilen örnekte tanımlı dönemdeki ortalama CPU kullanım yüzdesi tüketimidir.

### <a name="pricing"></a>Fiyatlandırma

Azure SQL Analytics kullanımı ücretsiz olsa da, tanılama telemetrisinin her ay ayrılan ücretsiz veri alım birimlerinin üzerinde tüketimi geçerlidir, [bkz.](https://azure.microsoft.com/pricing/details/monitor) Veri alım ücretsiz birimleri her ay çeşitli veritabanlarının ücretsiz izleme sağlar sağladı. Daha ağır iş yüklerine sahip daha etkin veritabanları, boşta kalan veritabanlarına karşı daha fazla veri yutarak. Azure SQL Analytics'in navigasyon menüsünde OMS Çalışma Alanı'nı seçip ardından Kullanım ve Tahmini Maliyetler'i seçerek Azure SQL Analytics'te veri alım tüketiminizi kolayca izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı Azure SQL verilerini görüntülemek için Azure Monitor'daki [günlük sorgularını](../log-query/log-query-overview.md) kullanın.
- Azure SQL verilerini gösteren [kendi panolarınızı oluşturun.](../learn/tutorial-logs-dashboards.md)
- Belirli Azure SQL olayları oluştuğunda [uyarılar oluşturun.](../platform/alerts-overview.md)
