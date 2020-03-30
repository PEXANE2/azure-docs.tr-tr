---
title: Intelligent Insights performans tanılama günlüğü
description: Intelligent Insights, Azure SQL Veritabanı performans sorunlarının tanılama günlüğünü sağlar
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: bb62b087451140261aee7aaa2fab0de14ea36283
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79209455"
---
# <a name="use-the-intelligent-insights-azure-sql-database-performance-diagnostics-log"></a>Intelligent Insights Azure SQL Veritabanı performans tanılama günlüğünü kullanma

Bu sayfa, [Intelligent Insights](sql-database-intelligent-insights.md)tarafından oluşturulan Azure SQL Veritabanı performans tanılama günlüğünün nasıl kullanılacağı, biçimi ve özel geliştirme gereksinimleriniz için içerdiği veriler hakkında bilgi sağlar. Bu tanılama günlüğünü [Azure Monitor günlüklerine,](../azure-monitor/insights/azure-sql.md) [Azure Etkinlik Hub'larına,](../azure-monitor/platform/resource-logs-stream-event-hubs.md) [Azure Depolama'ya](sql-database-metrics-diag-logging.md#stream-into-azure-storage)veya özel DevOps uyarı ve raporlama yetenekleri için üçüncü taraf çözümüne gönderebilirsiniz.

## <a name="log-header"></a>Günlük üstbilgi

Tanılama günlüğü, Intelligent Insights bulgularını elde etmek için JSON standart biçimini kullanır. Intelligent Insights günlüğüne erişmek için tam kategori özelliği sabit değer "SQLInsights"tır.

Günlüğün üstbilgisi yaygındır ve bir girişin ne zaman oluşturulduğunu gösteren zaman damgası (TimeGenerated) oluşur. Ayrıca, girişin ilgili olduğu belirli SQL Veritabanı'na atıfta bulunan bir kaynak kimliği (ResourceId) içerir. Kategori (Kategori), düzey (Düzey) ve işlem adı (OperationName) değerleri değişmeyen sabit özelliklerdir. Günlük girişinin bilgilendirsel olduğunu ve Intelligent Insights 'tan (SQLInsights) geldiğini belirtirler.

```json
"TimeGenerated" : "2017-9-25 11:00:00", // time stamp of the log entry
"ResourceId" : "database identifier", // value points to a database resource
"Category": "SQLInsights", // fixed property
"Level" : "Informational", // fixed property
"OperationName" : "Insight", // fixed property
```

## <a name="issue-id-and-database-affected"></a>Sorun kimliği ve veritabanı etkilenen

Sorun tanımlama özelliği (issueId_d) çözülene kadar performans sorunlarını benzersiz bir şekilde izlemenin bir yolunu sağlar. Aynı sorunun günlük raporlama durumundaki birden çok olay kaydı aynı kuruluş kimliğini paylaşır.

Tanılama kimliğiyle birlikte, tanılama günlüğü, tanılama günlüğünde bildirilen bir sorunla ilgili belirli olayın başlangıç (intervalStartTime_t) ve bitiş (intervalEndTme_t) zaman damgalarını bildirir.

Elastik havuz (elasticPoolName_s) özelliği, bir sorunla veritabanının hangi elastik havuza ait olduğunu gösterir. Veritabanı elastik bir havuzun parçası değilse, bu özelliğin değeri yoktur. Bir sorunun algılandığı veritabanı veritabanı adı (databaseName_s) özelliğinde açıklanır.

```json
"intervalStartTime_t": "2017-9-25 11:00", // start of the issue reported time stamp
"intervalEndTme_t":"2017-9-25 12:00", // end of the issue reported time stamp
"elasticPoolName_s" : "", // resource elastic pool (if applicable)
"databaseName_s" : "db_name", // database name
"issueId_d" : 1525, // unique ID of the issue detected
"status_s" : "Active" // status of the issue – possible values: "Active", "Verifying", and "Complete"
```

## <a name="detected-issues"></a>Algılanan sorunlar

Intelligent Insights performans günlüğünün bir sonraki bölümünde yerleşik yapay zeka ile algılanan performans sorunları yer almaktadır. Algılamalar JSON tanılama günlüğündeki özelliklerde açıklanır. Bu algılamalar, bir sorunun kategorisi, sorunun etkisi, etkilenen sorgular ve ölçümlerden oluşur. Algılama özellikleri, algılanan birden çok performans sorunu içerebilir.

Algılanan performans sorunları aşağıdaki algılamalar özellik yapısı ile bildirilir:

```json
"detections_s" : [{
"impact" : 1 to 3, // impact of the issue detected, possible values 1-3 (1 low, 2 moderate, 3 high impact)
"category" : "Detectable performance pattern", // performance issue detected, see the table
"details": <Details outputted> // details of an issue (see the table)
}]
```

Algılanabilir performans desenleri ve tanılama günlüğüne çıktılanan ayrıntılar aşağıdaki tabloda sağlanır.

### <a name="detection-category"></a>Algılama kategorisi

Kategori (kategori) özelliği, algılanabilir performans desenleri kategorisini açıklar. Algılanabilir performans desenleri olası tüm kategoriler için aşağıdaki tabloya bakın. Daha fazla bilgi için [Intelligent Insights ile sorun giderme veritabanı performansı sorunlarına](sql-database-intelligent-insights-troubleshoot-performance.md)bakın.

Algılanan performans sorununa bağlı olarak, tanılama günlüğü dosyasında çıktı alınan ayrıntılar buna göre farklılık gösterir.

| Algılanabilir performans desenleri | Çıktılar |
| :------------------- | ------------------- |
| Kaynak sınırlarına ulaşma | <li>Etkilenen kaynaklar</li><li>Sorgu ları</li><li>Kaynak tüketim yüzdesi</li> |
| İş Yükü Artışı | <li>Yürütmesi artan sorgu sayısı</li><li>İş yükü artışına en büyük katkıyı içeren sorguların sorgularını sorgula</li> |
| Bellek Baskısı | <li>Bellek katibi</li> |
| Kilitleme | <li>Etkilenen sorgu sorgu ları</li><li>Sorgu işlerini engelleme</li> |
| Artan MAXDOP | <li>Sorgu ları</li><li>CXP bekleme süreleri</li><li>Bekleme süreleri</li> |
| Pagelatch Çekişme | <li>Çekişmeye neden olan sorguların sorgu larını sorgula</li> |
| Eksik Dizini | <li>Sorgu ları</li> |
| Yeni Sorgu | <li>Yeni sorguların sorgu karma</li> |
| Olağandışı Bekleme İstatistikleri | <li>Olağandışı bekleme türleri</li><li>Sorgu ları</li><li>Bekleme sürelerini sorgula</li> |
| TempDB Çekişme | <li>Çekişmeye neden olan sorguların sorgu larını sorgula</li><li>Genel veritabanı pagelatch çekişme bekleme süresi sorgula atıf [%]</li> |
| Elastik havuz DTU Sıkıntısı | <li>Elastik havuz</li><li>En iyi DTU tüketen veritabanı</li><li>Üst tüketici tarafından kullanılan havuz DTU yüzdesi</li> |
| Regresyon Planı | <li>Sorgu ları</li><li>İyi planlı t.c.</li><li>Kötü plan lı t'ler</li> |
| Veritabanı Kapsamı Değer Değişikliği | <li>Varsayılan değerlerle karşılaştırıldığında veritabanı kapsamı nagöre yapılandırma değişiklikleri</li> |
| Yavaş İstemci | <li>Sorgu ları</li><li>Bekleme süreleri</li> |
| Fiyatlandırma Tier Downgrade | <li>Metin bildirimi</li> |

### <a name="impact"></a>Etki

Etki (etki) özelliği, algılanan bir davranışın veritabanının sahip olduğu soruna ne kadar katkıda bulunduğunu açıklar. Etkiler 1 ile 3 arasında değişirken, en yüksek katkı 3, 2 orta, en düşük katkı ise 1. Etki değeri, özel gereksinimlerinize bağlı olarak özel uyarı otomasyonu için bir giriş olarak kullanılabilir. Etkilenen özellik sorguları (QueryHashes) belirli bir algılama etkilenen sorgu işlenme bir listesini sağlar.

### <a name="impacted-queries"></a>Etkilenen sorgular

Intelligent Insights günlüğünün bir sonraki bölümü, algılanan performans sorunlarından etkilenen belirli sorgular hakkında bilgi sağlar. Bu bilgiler, impact_s özelliğine katıştırılmış bir nesne dizisi olarak açıklanır. Etki özelliği varlıklar ve ölçümlerden oluşur. Varlıklar belirli bir sorguya başvurur (Tür: Sorgu). Benzersiz sorgu karma değeri (Değer) özelliği altında açıklanır. Ayrıca, açıklanan sorguların her biri, algılanan bir performans sorununu gösteren bir metrik ve bir değer tarafından izlenir.

Aşağıdaki günlük örneğinde, karma 0x9102EXZ4 ile sorgu yürütme (Metrik: DurationIncreaseSeconds) artan bir süre olduğu tespit edildi. 110 saniyenin değeri, bu özel sorgunun yürütülmesinin 110 saniye daha uzun sürdüğünü gösterir. Birden çok sorgu algılanadığından, bu belirli günlük bölümü birden çok sorgu girişi içerebilir.

```json
"impact" : [{
"entity" : {
"Type" : "Query", // type of entity - query
"Value" : "query hash value", // for example "0x9102EXZ4" query hash value },
"Metric" : "DurationIncreaseSeconds", // measured metric and the measurement unit (in this case seconds)
"Value" : 110 // value of the measured metric (in this case seconds)
}]
```

### <a name="metrics"></a>Ölçümler

Bildirilen her metrik için ölçüm birimi, saniye, sayı ve yüzde olası değerleri ile metrik (metrik) özelliği altında sağlanır. Ölçülen bir ölçümün değeri değer (değer) özelliğinde bildirilir.

DurationIncreaseSeconds özelliği saniye cinsinden ölçüm birimini sağlar. CriticalErrorCount ölçüm birimi, hata sayısını temsil eden bir sayıdır.

```json
"metric" : "DurationIncreaseSeconds", // issue metric type – possible values: DurationIncreaseSeconds, CriticalErrorCount, WaitingSeconds
"value" : 102 // value of the measured metric (in this case seconds)
```

## <a name="root-cause-analysis-and-improvement-recommendations"></a>Kök neden analizi ve iyileştirme önerileri

Intelligent Insights performans günlüğünün son bölümü, tanımlanan performans bozulması sorununun otomatik kök neden çözümlemesi ile ilgilidir. Bu bilgiler, kök neden analizi (rootCauseAnalysis_s) özelliğinde insan dostu bir şekilde görünür. İyileştirme önerileri mümkünse günlüğe dahil edilir.

```json
// example of reported root cause analysis of the detected performance issue, in a human-readable format

"rootCauseAnalysis_s" : "High data IO caused performance to degrade. It seems that this database is missing some indexes that could help."
```

Azure [Monitor günlükleriyle]( https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql) Akıllı Öngörüler performans günlüğünü veya özel DevOps uyarı ve raporlama özellikleri için bir üçüncü taraf çözümü kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Intelligent [Insights](sql-database-intelligent-insights.md) kavramları hakkında bilgi edinin.
- [Intelligent Insights ile Azure SQL Veritabanı performans sorunlarını](sql-database-intelligent-insights-troubleshoot-performance.md)nasıl gidereceklerini öğrenin.
- [Azure SQL Analytics'i kullanarak Azure SQL Veritabanı'nı nasıl izleyeceğinizi öğrenin.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql)
- Azure kaynaklarınızdan günlük verilerini nasıl [toplayıp tükettiğinizi](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)öğrenin.
