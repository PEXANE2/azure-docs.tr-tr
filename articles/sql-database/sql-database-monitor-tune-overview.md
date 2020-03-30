---
title: İzleme ve performans ayarlama
description: Azure SQL Veritabanı'nda izleme ve performans ayarlama yeteneklerine ve metodolojiye genel bakış.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268736"
---
# <a name="monitoring-and-performance-tuning-in-azure-sql-database"></a>Azure SQL Veritabanı'nda izleme ve performans alamı

Azure SQL Veritabanı'ndaki bir veritabanının performansını izlemek için, iş yükünüz tarafından kullanılan CPU ve IO kaynaklarını belirli bir hizmet katmanı ve performans düzeyini seçerken seçtiğiniz veritabanı performansı düzeyine göre izleyerek başlayın. Bunu başarmak için Azure SQL Veritabanı, Azure portalında veya şu SQL yönetim araçlarından birini kullanarak görüntülenebilen kaynak ölçümleri yayar: [Azure Veri Stüdyosu](https://docs.microsoft.com/sql/azure-data-studio/what-is) veya SQL Server Management [Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS).

Azure SQL Veritabanı, tek ve birleştirilmiş veritabanları için, performansı artırmak için akıllı performans ayarı önerileri ve otomatik ayar seçenekleri sağlamak için bir dizi Veritabanı Danışmanı sağlar. Ayrıca, Sorgu Performans Öngörüsü, tek ve havuzlu veritabanları için en çok CPU ve IO kullanımından sorumlu sorgular hakkındaki ayrıntıları gösterir.

Azure SQL Veritabanı, veritabanlarınızın ve çözümlerinizin performansını en üst düzeye çıkarmak ve sorun gidermede size yardımcı olmak için yapay zeka tarafından desteklenen ek izleme ve atoraking özellikleri sağlar. Bu [Intelligent Insights](sql-database-intelligent-insights.md) ve diğer SQL Veritabanı kaynak günlüklerinin ve ölçümlerinin [akış lı dışa](sql-database-metrics-diag-logging.md) aktarımını, özellikle [SQL Analytics'i](../azure-monitor/insights/azure-sql.md)kullanarak tüketim ve analiz için çeşitli noktalardan birine yapılandırmayı seçebilirsiniz). Azure SQL Analytics, tüm Azure SQL veritabanlarınızın performansını ölçekte ve birden çok abonelik genelinde tek bir görünümde izlemek için gelişmiş bir bulut izleme çözümüdür. Dışa aktarabileceğiniz günlüklerin ve ölçümlerin listesi [için, dışa aktarma için tanılama telemetrisine](sql-database-metrics-diag-logging.md#diagnostic-telemetry-for-export-for-azure-sql-database) bakın

Son olarak, SQL SQL Server Query [Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) ve dinamik yönetim [görünümleri (DMVs)](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)ile kendi izleme ve tanılama yetenekleri vardır. Çeşitli performans sorunlarını izlemek için komut dosyaları için [DMV'leri kullanarak İzleme](sql-database-monitoring-with-dmvs.md) bölümüne bakın.

## <a name="monitoring-and-tuning-capabilities-in-the-azure-portal"></a>Azure portalındaki izleme ve aetme özellikleri

Azure portalında, Azure SQL Veritabanı tüm dağıtım türleri için kaynak ölçümlerinin izlenmesini sağlar. Ek olarak, tek ve havuzlu veritabanları için veritabanı danışmanları ve Query Performance Insight sorgu atonlama önerileri ve sorgu performans çözümlemesi sağlar. Son olarak, Azure portalında, mantıksal sunucular ve bunların tek ve birleştirilmiş veritabanları için otomatik etkinleştirebilirsiniz.

### <a name="sql-database-resource-monitoring"></a>SQL Veritabanı kaynak izleme

Azure portalında aşağıdaki kaynak ölçümlerini **Ölçümler** görünümünde hızlı bir şekilde izleyebilirsiniz:

- **DTU kullanımı**

  Bir veritabanının veya esnek havuzun uzun bir süre dtu kullanımının yüzde 100'üne ulaşıp ulaşmadığını kontrol edin. Yüksek DTU kullanımı, iş yükünüzün daha fazla CPU veya IO kaynağına ihtiyaç duyabileceğini gösterir. Ayrıca, en iyi duruma getirilmesi gereken sorguları da gösterebilir.
- **CPU kullanımı**

  Veritabanının, elastik havuzun veya yönetilen örneğin uzun bir süre cpu kullanımının yüzde 100'üne ulaşıp ulaşmadığını denetleyin. Yüksek CPU, iş yükünüzün daha fazla CPU veya IO kaynağına ihtiyaç duyabileceğini gösterir. Ayrıca, en iyi duruma getirilmesi gereken sorguları da gösterebilir.
- **IO kullanımı**

  Veritabanının, elastik havuzun veya yönet'in örneğinin temel depolama alanının IO sınırlarına ulaşıp ulaşmayolmadığını denetleyin. Yüksek IO kullanımı, iş yükünüzün daha fazla CPU veya IO kaynağına ihtiyaç duyabileceğini gösterir. Ayrıca, en iyi duruma getirilmesi gereken sorguları da gösterebilir.

  ![Kaynak ölçümleri](./media/sql-database-monitor-tune-overview/resource-metrics.png)

### <a name="database-advisors"></a>Veritabanı danışmanları
' Azure SQL Veritabanı, tek ve havuzlu veritabanları için performans alabilen öneriler sağlayan [veritabanı danışmanlarını](sql-database-advisor.md) içerir. Bu öneriler Azure portalında ve [PowerShell'i](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseadvisor)kullanarak kullanılabilir. SQL Veritabanı'nın bu ayar önerilerini otomatik olarak uygulayabilmesi için [otomatik tuning'i](sql-database-automatic-tuning.md) de etkinleştirebilirsiniz.

### <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu Performans Öngörüsü,](sql-database-query-performance.md) tek ve havuzlu veritabanları için en çok tüketen ve en uzun süre çalışan sorguların Azure portalındaki performansı gösterir.

## <a name="generate-intelligent-assessments-of-performance-issues"></a>Performans sorunlarının akıllı değerlendirmelerini oluşturun

Azure SQL Database [Intelligent Insights,](sql-database-intelligent-insights.md) yapay zeka aracılığıyla veritabanı kullanımını sürekli olarak izlemek ve düşük performansa neden olan yıkıcı olayları tespit etmek için yerleşik zekayı kullanır. Intelligent Insights, sorgu yürütme bekleme sürelerine, hatalara veya zaman çıkışlarına bağlı olarak Azure SQL Veritabanı'ndaki veritabanlarındaki performans sorunlarını otomatik olarak algılar. Algılandıktan sonra, [sorunların akıllı](sql-database-intelligent-insights-troubleshoot-performance.md)bir değerlendirmesi yle bir kaynak günlüğü (SQLInsights olarak adlandırılır) oluşturan ayrıntılı bir analiz gerçekleştirilir. Bu değerlendirme, veritabanı performans sorununun temel neden çözümlemesi ve mümkünse performans iyileştirmeleri için önerilerden oluşur.

Intelligent Insights, Azure yerleşik zekasının aşağıdaki değeri sağlayan benzersiz bir yeteneğidir:

- Öngörülebilir izleme
- Özel performans öngörüleri
- Veritabanı performans bozulmasının erken saptanması
- Algılanan sorunların kök neden analizi
- Performans geliştirme önerileri
- Yüz binlerce veritabanındaki yeteneği ölçeklendirin
- DevOps kaynaklarına olumlu etki ve toplam sahip olma maliyeti

## <a name="enable-the-streaming-export-of-metrics-and-resource-logs"></a>Ölçümlerin ve kaynak günlüklerinin akış dışa aktarılmasını etkinleştirme

Akıllı İstatistikler kaynak günlüğü de dahil olmak üzere çeşitli noktalardan birine [tanısal telemetri akışı dışa aktarma](sql-database-metrics-diag-logging.md) yı etkinleştirebilir ve yapılandırabilirsiniz. Performans sorunlarını tanımlamak ve çözmek için bu ek tanısal telemetriyi kullanmak için [SQL Analytics](../azure-monitor/insights/azure-sql.md) ve diğer yetenekleri kullanın.

Tanılama ayarlarını, tek veritabanları, havuzlu veritabanları, elastik havuzlar, yönetilen örnekler ve örnek veritabanları için ölçüm ve kaynak günlükleri kategorilerini aşağıdaki Azure kaynaklarından birine akış ayarı yapacak şekilde yapılandırırsınız.

### <a name="log-analytics-workspace-in-azure-monitor"></a>Azure monitöründe Günlük Analytics çalışma alanı

Azure Monitor'da ölçümleri ve kaynak günlüklerini Günlük [Analizi çalışma alanına](../azure-monitor/platform/resource-logs-collect-workspace.md)aktarabilirsiniz. Burada aktarılan veriler, performans raporları, uyarılar ve azaltma önerileri içeren veritabanlarınızın akıllı izlemesini sağlayan bir bulut yalnızca izleme çözümü olan [SQL Analytics](../azure-monitor/insights/azure-sql.md)tarafından tüketilebilir. Log Analytics çalışma alanına aktarılan veriler, toplanan diğer izleme verileriyle analiz edilebilir ve uyarılar ve görselleştirmeler gibi diğer Azure Monitör özelliklerinden yararlanmanıza olanak tanır.

### <a name="azure-event-hubs"></a>Azure Event Hubs

Ölçümleri ve kaynak günlüklerini [Azure Etkinlik Hub'larına](../azure-monitor/platform/resource-logs-stream-event-hubs.md)aktarabilirsiniz. Aşağıdaki işlevselliği sağlamak için olay hub'larına tanısal telemetri akışı:

- **3. taraf günlük ve telemetri sistemlerine akış günlükleri**

  Günlük verilerini üçüncü taraf bir SIEM veya günlük analizi aracına aktarmak için tüm ölçümlerinizi ve kaynak günlüklerinizi tek bir olay hub'ına aktarın.
- **Özel bir telemetri ve günlük platformu oluşturun**

  Etkinlik hub'larının yüksek ölçeklenebilir yayımlama-abone lik yapısı, ölçümleri ve kaynak günlüklerini esnek bir şekilde özel bir telemetri platformuna yutmanızı sağlar. Ayrıntılar [için Azure Etkinlik Hub'larında Küresel Ölçekli Bir Telemetri Platformu Tasarlama ve Boyutlandırma](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/) bölümüne bakın.
- **Power BI'ye veri akışı sağlayarak hizmet durumunu görüntüleme**

  Tanılama verilerinizi Azure hizmetlerinizle ilgili neredeyse gerçek zamanlı öngörülere dönüştürmek için Etkinlik Hub'larını, Akış Analizini ve Power BI'yi kullanın. Bkz. Akış Analizi ve Power BI: Bu çözümle ilgili ayrıntılar [için veri akışı için gerçek zamanlı analiz panosu.](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-power-bi-dashboard)

### <a name="azure-storage"></a>Azure Storage

[Azure Depolama'ya](../azure-monitor/platform/resource-logs-collect-storage.md)akış ölçümleri ve kaynak günlükleri. Önceki iki akış seçeneğinin maliyetinin çok küçük bir kısmı için büyük miktarda tanısal telemetriyi arşivlemek için Azure depolama alanını kullanın.

## <a name="use-extended-events-in-the-sql-database-engine"></a>SQL veritabanı altyapısında genişletilmiş olayları kullanma

Ayrıca, ek gelişmiş izleme ve sorun giderme için SQL'de [genişletilmiş olayları](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) kullanabilirsiniz. Genişletilmiş olaylar mimarisi, kullanıcıların bir performans sorununu gidermek veya tanımlamak için gerekli olduğu kadar çok veya az veri toplamasına olanak tanır. SQL Veritabanı'nda genişletilmiş olayların kullanımı hakkında bilgi [için](sql-database-xevent-db-diff-from-svr.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar

- Tek ve havuzlu veritabanları için akıllı performans önerileri hakkında daha fazla bilgi için [Veritabanı danışmanı performans önerilerine](sql-database-advisor.md)bakın.
- Otomatik tanılama ve performans sorunlarının kök neden analiziyle veritabanı performansını otomatik olarak izleme hakkında daha fazla bilgi için [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md)bölümüne bakın.
'''''''''