---
title: İzleme ve performans ayarlama
description: Azure SQL veritabanı 'nda izleme ve performans ayarlama özelliklerine ve metodolojiye genel bakış.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 837d88665c1fdffe902c9c478e5d6dc65a2e402a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79268736"
---
# <a name="monitoring-and-performance-tuning-in-azure-sql-database"></a>Azure SQL veritabanı 'nda izleme ve performans ayarlaması

Azure SQL veritabanı 'nda bir veritabanının performansını izlemek için, iş yükünüz tarafından kullanılan CPU ve GÇ kaynaklarını belirli bir hizmet katmanı ve performans düzeyi seçerken seçtiğiniz veritabanı performansı düzeyine göre izleyerek başlatın. Bunu gerçekleştirmek için Azure SQL veritabanı, Azure portal görüntülenebilecek veya şu SQL yönetim araçlarından birini kullanarak kaynak ölçümleri yayar: [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/what-is) veya [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

Azure SQL veritabanı, tek ve havuza alınmış veritabanlarında, performansı artırmak için akıllı performans ayarlama önerilerini ve otomatik ayarlama seçeneklerini sağlamak üzere çeşitli veritabanı danışmanları sağlar. Ayrıca, Sorgu Performansı İçgörüleri tek ve havuza alınmış veritabanları için en fazla CPU ve GÇ kullanımından sorumlu sorgular hakkındaki ayrıntıları gösterir.

Azure SQL veritabanı, sorun gidermede ve veritabanlarınızın ve çözümlerin performansını en üst düzeye çıkarmak için yapay zeka tarafından desteklenen ek izleme ve ayarlama özellikleri sağlar. Bu [akıllı içgörüler](sql-database-intelligent-insights.md) ve diğer SQL veritabanı kaynak günlüklerinin ve ölçümlerinin [akış dışa aktarılmasını](sql-database-metrics-diag-logging.md) , özellikle [SQL Analytics](../azure-monitor/insights/azure-sql.md)'i kullanarak, tüketim ve analiz için birkaç hedefden birine yapılandırmayı seçebilirsiniz. Azure SQL Analytics, tek bir görünümdeki tüm Azure SQL veritabanlarınızın ve birden çok aboneliğin performansını izlemeye yönelik gelişmiş bir bulut izleme çözümüdür. Dışarı aktarmak istediğiniz günlüklerin ve ölçümlerin bir listesi için bkz. [dışarı aktarma için tanılama telemetrisi](sql-database-metrics-diag-logging.md#diagnostic-telemetry-for-export-for-azure-sql-database)

Son olarak, SQL 'in kendi izleme ve tanılama özelliklerine sahip [SQL Server sorgu deposu](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) ve [dinamik yönetim görünümleri (DMVs)](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)vardır. Çeşitli performans sorunlarını izlemek için bkz. komut dosyaları için [DMVs kullanarak izleme](sql-database-monitoring-with-dmvs.md) .

## <a name="monitoring-and-tuning-capabilities-in-the-azure-portal"></a>Azure portal özellikleri izleme ve ayarlama

Azure portal Azure SQL veritabanı, tüm dağıtım türleri için kaynak ölçümlerinin izlenmesini sağlar. Ek olarak, tek ve havuza alınmış veritabanları için veritabanı danışmanları ve Sorgu Performansı İçgörüleri sorgu ayarlama önerilerini ve sorgu performans analizini sağlar. Son olarak, Azure portal mantıksal sunucular ve bunların tek ve havuza alınmış veritabanları için otomatik ' i etkinleştirebilirsiniz.

### <a name="sql-database-resource-monitoring"></a>SQL veritabanı kaynak izleme

**Ölçüm** görünümündeki Azure Portal aşağıdaki kaynak ölçümlerini hızlıca izleyebilirsiniz:

- **DTU kullanımı**

  Bir veritabanı veya elastik havuzun, uzun bir süre için DTU kullanımının yüzde 100 ' una ulaşmasını denetleyin. Yüksek DTU kullanımı, iş yükünüzün daha fazla CPU veya GÇ kaynağına ihtiyacı olabileceğini gösterir. Ayrıca, iyileştirilmesi gereken sorguları da belirtebilir.
- **CPU kullanımı**

  Bir veritabanının, elastik havuzun veya yönetilen Örneğin, CPU kullanımının yüzde 100 ' unun uzun bir süre boyunca harcanmakta olup olmadığını denetleyin. Yüksek CPU, iş yükünüzün daha fazla CPU veya GÇ kaynağına ihtiyacı olabileceğini gösterir. Ayrıca, iyileştirilmesi gereken sorguları da belirtebilir.
- **GÇ kullanımı**

  Bir veritabanı, elastik havuz veya Yönetim ' örneğinin temeldeki depolamanın GÇ sınırlarına ulaşmakta olup olmadığını denetleyin. Yüksek GÇ kullanımı, iş yükünüzün daha fazla CPU veya GÇ kaynağına ihtiyacı olabileceğini gösterir. Ayrıca, iyileştirilmesi gereken sorguları da belirtebilir.

  ![Kaynak ölçümleri](./media/sql-database-monitor-tune-overview/resource-metrics.png)

### <a name="database-advisors"></a>Veritabanı danışmanları
' Azure SQL veritabanı, tek ve havuza alınmış veritabanları için performans ayarlama önerilerini sağlayan [veritabanı danışmanlarını](sql-database-advisor.md) içerir. Bu öneriler, Azure portal ve [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseadvisor)kullanılarak kullanılabilir. Ayrıca, [otomatik ayarlamayı](sql-database-automatic-tuning.md) etkinleştirerek SQL veritabanı 'nın bu ayarlama önerilerini otomatik olarak uygulayabilmesini sağlayabilirsiniz.

### <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu performansı içgörüleri](sql-database-query-performance.md) , tek ve havuza alınmış veritabanları için en çok kullanılan ve en uzun çalışan sorguların Azure Portal performansını gösterir.

## <a name="generate-intelligent-assessments-of-performance-issues"></a>Performans sorunlarının akıllı değerlendirmelerini oluştur

Azure SQL veritabanı [akıllı içgörüler](sql-database-intelligent-insights.md) , yapay zeka aracılığıyla veritabanı kullanımını sürekli olarak izlemek ve zayıf performansa neden olan olayları saptamak için yerleşik zekayı kullanır. Akıllı İçgörüler, sorgu yürütme bekleme süreleri, hatalar veya zaman aşımları temelinde Azure SQL veritabanı 'ndaki veritabanları ile ilgili performans sorunlarını otomatik olarak algılar. Algılandıktan sonra, [sorunların akıllı değerlendirmesi](sql-database-intelligent-insights-troubleshoot-performance.md)ile bir kaynak günlüğü (Sqlinsıghts olarak adlandırılır) oluşturan ayrıntılı bir analiz yapılır. Bu değerlendirme, veritabanı performans sorununun bir kök neden analizinden oluşur ve mümkün olduğunda performans iyileştirmeleri için öneriler içerir.

Akıllı İçgörüler, Azure yerleşik zekanın aşağıdaki değeri sağlayan benzersiz bir özelliğidir:

- Öngörülebilir izleme
- Özel performans öngörüleri
- Veritabanı performansı performansında erken algılama
- Algılanan sorunların kök neden analizi
- Performans geliştirme önerileri
- Yüzlerce binlerce veritabanında ölçeği genişletme özelliği
- DevOps kaynaklarına yönelik olumlu etki ve toplam sahip olma maliyeti

## <a name="enable-the-streaming-export-of-metrics-and-resource-logs"></a>Ölçüm ve kaynak günlüklerinin akış dışa aktarılmasını etkinleştirme

Akıllı İçgörüler kaynak günlüğü de dahil olmak üzere, [Tanılama telemetrinin akış dışa aktarılmasını](sql-database-metrics-diag-logging.md) , birkaç hedefe göre etkinleştirebilir ve yapılandırabilirsiniz. Performans sorunlarını belirlemek ve çözmek üzere bu ek tanılama telemetrisini kullanmak için [SQL Analytics](../azure-monitor/insights/azure-sql.md) ve diğer özellikleri kullanın.

Tanılama ayarlarını, tek veritabanları, havuza alınmış veritabanları, elastik havuzlar, yönetilen örnekler ve örnek veritabanları için aşağıdaki Azure kaynaklarından birine yönelik ölçüm kategorilerini ve kaynak günlüklerini akışa almak üzere yapılandırırsınız.

### <a name="log-analytics-workspace-in-azure-monitor"></a>Azure izleyici 'de Log Analytics çalışma alanı

Ölçümleri ve kaynak günlüklerini [Azure izleyici 'de bir Log Analytics çalışma alanına](../azure-monitor/platform/resource-logs-collect-workspace.md)akışını sağlayabilirsiniz. Burada akan veriler, performans raporları, uyarılar ve risk azaltma önerilerini içeren veritabanlarınızı akıllı olarak izlemeye yönelik yalnızca bir bulut izleme çözümü olan [SQL Analytics](../azure-monitor/insights/azure-sql.md)tarafından tüketilebilir. Bir Log Analytics çalışma alanına akan veriler, toplanan diğer izleme verileriyle analiz edilebilir ve ayrıca uyarılar ve görselleştirmeler gibi diğer Azure Izleyici özelliklerinden yararlanmanızı sağlar.

### <a name="azure-event-hubs"></a>Azure Event Hubs

[Azure Event Hubs](../azure-monitor/platform/resource-logs-stream-event-hubs.md)ölçümleri ve kaynak günlüklerini akışla aktarabilirsiniz. Aşağıdaki işlevleri sağlamak için Olay Hub 'larına yönelik tanılama telemetrisini akışa alma:

- **Günlük kaydı ve telemetri sistemlerine akış günlükleri**

  Günlük verilerini bir üçüncü taraf SıEM veya Log Analytics aracına yöneltmek için tüm ölçümleri ve kaynak günlüklerinizi tek bir olay hub 'ına akışla.
- **Özel telemetri ve günlüğe kaydetme platformu oluşturma**

  Olay Hub 'larının yüksek düzeyde ölçeklenebilir yayımla-abone ol yapısı, ölçümleri ve kaynak günlüklerini özel bir telemetri platformunda esnek bir şekilde içe almanızı sağlar. Ayrıntılar için bkz. [Event Hubs Azure 'Da küresel ölçekli telemetri platformunu tasarlama ve boyutlandırma](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) .
- **Power BI veri akışı yaparak hizmet durumunu görüntüleyin**

  Tanılama verilerinizi Azure hizmetlerinizden neredeyse gerçek zamanlı içgörüler halinde dönüştürmek için Event Hubs, Stream Analytics ve Power BI kullanın. Bkz. [Stream Analytics ve Power BI: Bu çözümdeki Ayrıntılar için veri akışı için gerçek zamanlı analiz panosu](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-power-bi-dashboard) .

### <a name="azure-storage"></a>Azure Storage

[Azure depolama](../azure-monitor/platform/resource-logs-collect-storage.md)'ya ölçüm ve kaynak günlükleri akışı yapın. Önceki iki akış seçeneğinin maliyetinin bir bölümü boyunca çok miktarda tanılama telemetrisini arşivlemek için Azure Storage 'ı kullanın.

## <a name="use-extended-events-in-the-sql-database-engine"></a>SQL veritabanı altyapısında genişletilmiş olayları kullanma

Ek olarak, Gelişmiş izleme ve sorun giderme işlemleri için SQL 'de [Genişletilmiş olayları](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) da kullanabilirsiniz. Genişletilmiş olaylar mimarisi, kullanıcıların sorunları gidermek veya bir performans sorunu tanımlamak için gereken kadar çok veya az veri toplamasına olanak sağlar. SQL veritabanında genişletilmiş olayları kullanma hakkında daha fazla bilgi için bkz. [SQL veritabanı 'Nda genişletilmiş olaylar](sql-database-xevent-db-diff-from-svr.md).

## <a name="next-steps"></a>Sonraki adımlar

- Tek ve havuza alınmış veritabanlarının akıllı performans önerileri hakkında daha fazla bilgi için bkz. [veritabanı Danışmanı performans önerileri](sql-database-advisor.md).
- Performans sorunlarının otomatik tanılama ve kök neden analizine sahip veritabanı performansını otomatik olarak izleme hakkında daha fazla bilgi için bkz. [Azure SQL akıllı içgörüler](sql-database-intelligent-insights.md).
'''''''''