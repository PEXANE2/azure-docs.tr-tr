---
title: İzleme ve performans ayarlama
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde izleme ve performans ayarlama özelliklerine ve metodolojiye genel bakış.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 7ac3a2947c84eca0e22e0888495f9992222d4dd6
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220541"
---
# <a name="monitoring-and-performance-tuning-in-azure-sql-database-and-azure-sql-managed-instance"></a>Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde izleme ve performans ayarlaması
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde bir veritabanının performansını izlemek için, belirli bir hizmet katmanını ve performans düzeyini seçerken seçtiğiniz veritabanı performansı düzeyine göre iş yükünüz tarafından kullanılan CPU ve GÇ kaynaklarını izlemeye başlayın. Bunu gerçekleştirmek için Azure SQL veritabanı ve Azure SQL yönetilen örneği, Azure portal görüntülenebilecek veya şu SQL Server yönetim araçlarından birini kullanarak kaynak ölçümleri yayar: [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) ya da [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

Azure SQL veritabanı, performansı artırmak için akıllı performans ayarlama önerilerini ve otomatik ayarlama seçeneklerini sağlamak üzere çeşitli veritabanı danışmanları sağlar. Ayrıca, Sorgu Performansı İçgörüleri tek ve havuza alınmış veritabanları için en fazla CPU ve GÇ kullanımından sorumlu sorgular hakkındaki ayrıntıları gösterir.

Azure SQL veritabanı ve Azure SQL yönetilen örneği, sorun gidermede ve veritabanlarınızın ve çözümlerin performansını en üst düzeye çıkarmak için yapay zeka tarafından desteklenen gelişmiş izleme ve ayarlama özellikleri sağlar. Bu [akıllı içgörüler](intelligent-insights-overview.md) ve diğer veritabanı kaynak günlüklerinin ve ölçümlerinin [akış dışa aktarılmasını](metrics-diagnostic-telemetry-logging-streaming-export-configure.md) , özellikle [SQL Analytics](../../azure-monitor/insights/azure-sql.md)'i kullanarak, tüketim ve analiz için birkaç hedefden birine yapılandırmayı seçebilirsiniz. Azure SQL Analytics, tüm veritabanlarınızın performansını ölçek ve birden çok abonelik arasında tek bir görünümde izlemek için gelişmiş bir bulut izleme çözümüdür. Dışarı aktarmak istediğiniz günlüklerin ve ölçümlerin bir listesi için bkz. [dışarı aktarma için tanılama telemetrisi](metrics-diagnostic-telemetry-logging-streaming-export-configure.md#diagnostic-telemetry-for-export)

Son olarak SQL Server, SQL veritabanı ve SQL yönetilen örneği için [sorgu deposu](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) ve [dinamik yönetim görünümleri (DMVs)](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)gibi kendi izleme ve tanılama özelliklerine sahiptir. Çeşitli performans sorunlarını izlemek için bkz. komut dosyaları için [DMVs kullanarak izleme](monitoring-with-dmvs.md) .

## <a name="monitoring-and-tuning-capabilities-in-the-azure-portal"></a>Azure portal özellikleri izleme ve ayarlama

Azure portal, Azure SQL veritabanı ve Azure SQL yönetilen örneği, kaynak ölçümlerinin izlenmesini sağlar. Ayrıca, Azure SQL veritabanı, veritabanı danışmanları sağlar ve Sorgu Performansı İçgörüleri sorgu ayarlama önerilerini ve sorgu performans analizini sağlar. Son olarak, Azure portal, [MANTıKSAL SQL sunucuları](logical-servers.md) ve bunların tek ve havuza alınmış veritabanları için otomatik ' i etkinleştirebilirsiniz.

### <a name="azure-sql-database-and-azure-sql-managed-instance-resource-monitoring"></a>Azure SQL veritabanı ve Azure SQL yönetilen örnek kaynak izleme

Çeşitli kaynak ölçümlerini, **ölçüm** görünümündeki Azure Portal hızlı bir şekilde izleyebilirsiniz. Bu ölçümler, bir veritabanının işlemci, bellek veya GÇ kaynaklarının %100 ' ine ulaşmasını görmenizi sağlar. Yüksek DTU veya işlemci yüzdesi ve yüksek GÇ yüzdesi, iş yükünüzün daha fazla CPU veya GÇ kaynağına ihtiyacı olabileceğini gösterir. Ayrıca, iyileştirilmesi gereken sorguları da belirtebilir.

  ![Kaynak ölçümleri](./media/monitor-tune-overview/resource-metrics.png)

### <a name="database-advisors-in-azure-sql-database"></a>Azure SQL veritabanı 'nda veritabanı danışmanları

Azure SQL veritabanı, tek ve havuza alınmış veritabanları için performans ayarlama önerilerini sağlayan [veritabanı danışmanlarını](database-advisor-implement-performance-recommendations.md) içerir. Bu öneriler, Azure portal ve [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseadvisor)kullanılarak kullanılabilir. Ayrıca, [otomatik ayarlamayı](automatic-tuning-overview.md) ETKINLEŞTIREREK Azure SQL veritabanı 'nın bu ayarlama önerilerini otomatik olarak uygulayabilmesini sağlayabilirsiniz.

### <a name="query-performance-insight-in-azure-sql-database"></a>Azure SQL veritabanı 'nda Sorgu Performansı İçgörüleri

[Sorgu performansı içgörüleri](query-performance-insight-use.md) , tek ve havuza alınmış veritabanları için en çok kullanılan ve en uzun çalışan sorguların Azure Portal performansını gösterir.

## <a name="generate-intelligent-assessments-of-performance-issues"></a>Performans sorunlarının akıllı değerlendirmelerini oluştur

Azure SQL veritabanı ve Azure SQL yönetilen örneği için [akıllı içgörüler](intelligent-insights-overview.md) , yapay zeka aracılığıyla veritabanı kullanımını sürekli olarak izlemek ve düşük performansa neden olan kötü etkinlikleri saptamak için yerleşik zeka kullanır. Akıllı İçgörüler sorgu yürütme bekleme süreleri, hatalar veya zaman aşımları temelinde veritabanları ile ilgili performans sorunlarını otomatik olarak algılar. Algılandıktan sonra, [sorunların akıllı değerlendirmesi](intelligent-insights-troubleshoot-performance.md)ile bir kaynak günlüğü (Sqlinsıghts olarak adlandırılır) oluşturan ayrıntılı bir analiz yapılır. Bu değerlendirme, veritabanı performans sorununun bir kök neden analizinden oluşur ve mümkün olduğunda performans iyileştirmeleri için öneriler içerir.

Akıllı İçgörüler, Azure yerleşik zekanın aşağıdaki değeri sağlayan benzersiz bir özelliğidir:

- Öngörülebilir izleme
- Özel performans öngörüleri
- Veritabanı performansı performansında erken algılama
- Algılanan sorunların kök neden analizi
- Performans geliştirme önerileri
- Yüzlerce binlerce veritabanında ölçeği genişletme özelliği
- DevOps kaynaklarına yönelik olumlu etki ve toplam sahip olma maliyeti

## <a name="enable-the-streaming-export-of-metrics-and-resource-logs"></a>Ölçüm ve kaynak günlüklerinin akış dışa aktarılmasını etkinleştirme

Akıllı İçgörüler kaynak günlüğü de dahil olmak üzere, [Tanılama telemetrinin akış dışa aktarılmasını](metrics-diagnostic-telemetry-logging-streaming-export-configure.md) , birkaç hedefe göre etkinleştirebilir ve yapılandırabilirsiniz. Performans sorunlarını belirlemek ve çözmek üzere bu ek tanılama telemetrisini kullanmak için [SQL Analytics](../../azure-monitor/insights/azure-sql.md) ve diğer özellikleri kullanın.

Tanılama ayarlarını, tek veritabanları, havuza alınmış veritabanları, elastik havuzlar, yönetilen örnekler ve örnek veritabanları için aşağıdaki Azure kaynaklarından birine yönelik ölçüm kategorilerini ve kaynak günlüklerini akışa almak üzere yapılandırırsınız.

### <a name="log-analytics-workspace-in-azure-monitor"></a>Azure Izleyici 'de Log Analytics çalışma alanı

Ölçümleri ve kaynak günlüklerini [Azure izleyici 'de bir Log Analytics çalışma alanına](../../azure-monitor/platform/resource-logs-collect-workspace.md)akışını sağlayabilirsiniz. Burada akan veriler, performans raporları, uyarılar ve risk azaltma önerilerini içeren veritabanlarınızı akıllı olarak izlemeye yönelik yalnızca bir bulut izleme çözümü olan [SQL Analytics](../../azure-monitor/insights/azure-sql.md)tarafından tüketilebilir. Bir Log Analytics çalışma alanına akan veriler, toplanan diğer izleme verileriyle analiz edilebilir ve ayrıca uyarılar ve görselleştirmeler gibi diğer Azure Izleyici özelliklerinden yararlanmanızı sağlar.

### <a name="azure-event-hubs"></a>Azure Event Hubs

[Azure Event Hubs](../../azure-monitor/platform/resource-logs-stream-event-hubs.md)ölçümleri ve kaynak günlüklerini akışla aktarabilirsiniz. Aşağıdaki işlevleri sağlamak için Olay Hub 'larına yönelik tanılama telemetrisini akışa alma:

- **Üçüncü taraf günlüğe kaydetme ve telemetri sistemlerine akış günlükleri**

  Günlük verilerini bir üçüncü taraf SıEM veya Log Analytics aracına yöneltmek için tüm ölçümleri ve kaynak günlüklerinizi tek bir olay hub 'ına akışla.
- **Özel telemetri ve günlüğe kaydetme platformu oluşturma**

  Olay Hub 'larının yüksek düzeyde ölçeklenebilir yayımla-abone ol yapısı, ölçümleri ve kaynak günlüklerini özel bir telemetri platformunda esnek bir şekilde içe almanızı sağlar. Ayrıntılar için bkz. [Event Hubs Azure 'Da küresel ölçekli telemetri platformunu tasarlama ve boyutlandırma](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) .
- **Power BI veri akışı yaparak hizmet durumunu görüntüleyin**

  Tanılama verilerinizi Azure hizmetlerinizden neredeyse gerçek zamanlı içgörüler halinde dönüştürmek için Event Hubs, Stream Analytics ve Power BI kullanın. Bkz. [Stream Analytics ve Power BI: Bu çözümdeki Ayrıntılar için veri akışı için gerçek zamanlı analiz panosu](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-power-bi-dashboard) .

### <a name="azure-storage"></a>Azure Storage

[Azure depolama](../../azure-monitor/platform/resource-logs-collect-storage.md)'ya ölçüm ve kaynak günlükleri akışı yapın. Önceki iki akış seçeneğinin maliyetinin bir bölümü boyunca çok miktarda tanılama telemetrisini arşivlemek için Azure Storage 'ı kullanın.

## <a name="use-extended-events"></a>Genişletilmiş olayları kullanma 

Ayrıca, Gelişmiş izleme ve sorun giderme için SQL Server [Genişletilmiş olayları](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) da kullanabilirsiniz. Genişletilmiş olaylar mimarisi, kullanıcıların sorunları gidermek veya bir performans sorunu tanımlamak için gereken kadar çok veya az veri toplamasına olanak sağlar. Azure SQL veritabanı 'nda genişletilmiş olayları kullanma hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda genişletilmiş olaylar](xevent-db-diff-from-svr.md).

## <a name="next-steps"></a>Sonraki adımlar

- Tek ve havuza alınmış veritabanlarının akıllı performans önerileri hakkında daha fazla bilgi için bkz. [veritabanı Danışmanı performans önerileri](database-advisor-implement-performance-recommendations.md).
- Performans sorunlarının otomatik tanılama ve kök neden analizine sahip veritabanı performansını otomatik olarak izleme hakkında daha fazla bilgi için bkz. [Azure SQL akıllı içgörüler](intelligent-insights-overview.md).
