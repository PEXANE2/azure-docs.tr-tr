---
title: Azure SQL Veritabanı'ndaki sorgu performansı sorunları türleri
description: Bu makalede, Azure SQL Veritabanı'ndaki sorgu performansı sorunları türleri hakkında bilgi edinin ve bu sorunlarla sorguları nasıl tanımlayıp çözümlemeyi de öğrenin
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
ms.openlocfilehash: e155321c2727bf4ee871ef7be7b61b6a523ec1fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256139"
---
# <a name="detectable-types-of-query-performance-bottlenecks-in-azure-sql-database"></a>Azure SQL Veritabanı'nda algılanabilir sorgu performans sorunları türleri

Performans darboğazını çözmeye çalışırken, sorgu çalışan bir durumdayken veya bekleme durumundayken darboğaz oluşup oluşmadığını belirleyerek başlayın. Bu kararlılığa bağlı olarak farklı kararlar uygulanır. Çalışanla ilgili bir soruna veya beklemeyle ilgili bir soruna neden olabilecek etkenleri anlamada yardımcı olmak için aşağıdaki diyagramı kullanın. Bu makalede, her sorun türüyle ilgili sorunlar ve çözümler ele alınmıştır.

Bu tür performans sorunlarını algılamak için Azure SQL Database [Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md#detectable-database-performance-patterns) veya SQL Server [DMV'leri](sql-database-monitoring-with-dmvs.md) kullanabilirsiniz.

![İş yükü durumları](./media/sql-database-monitor-tune-overview/workload-states.png)

**Çalışmayla ilgili sorunlar**: Çalışanla ilgili sorunlar genellikle yetersiz veya aşırı kullanılan kaynaklarla ilgili bir alt optimal sorgu planı veya yürütme sorunlarıyla sonuçlanan derleme sorunlarıyla ilişkilidir.
**Bekleme yle ilgili sorunlar**: Beklemeyle ilgili sorunlar genellikle:

- Kilitler (engelleme)
- G/Ç
- TempDB kullanımı ile ilgili çekişme
- Bellek hibe bekler

## <a name="compilation-problems-resulting-in-a-suboptimal-query-plan"></a>Suboptimal sorgu planı ile sonuçlanan derleme sorunları

SQL Query Optimizer tarafından oluşturulan bir suboptimal planı yavaş sorgu performansı nedeni olabilir. SQL Query Optimizer eksik bir dizin, eski istatistikler, işlenecek satır sayısının yanlış tahmini veya gerekli belleğin yanlış tahmini nedeniyle bir alt optimal plan üretebilir. Sorgunun geçmişte veya başka bir örnekte (yönetilen bir örnek veya SQL Server örneği) daha hızlı yürütüldedildiğini biliyorsanız, farklı olup olmadıklarını görmek için gerçek yürütme planlarını karşılaştırın.

- Bu yöntemlerden birini kullanarak eksik dizinleri tanımlayın:

  - [Akıllı Öngörüler](sql-database-intelligent-insights-troubleshoot-performance.md#missing-index)kullanın.
  - Tek ve havuzlu veritabanları için [Veritabanı Danışmanı.](sql-database-advisor.md)
  - Dmvs. Bu örnek, eksik bir dizinin etkisini, DMV'ler kullanarak [eksik dizinlerin](sql-database-performance-guidance.md#identifying-and-adding-missing-indexes) nasıl algılanacağını ve eksik dizin önerisini uygulamanın etkisini gösterir.
- Daha iyi bir plan almak için [sorgu ipuçlarını,](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) [güncelleştirme istatistiklerini](https://docs.microsoft.com/sql/t-sql/statements/update-statistics-transact-sql)veya [dizinleri yeniden uygulamaya](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes) çalışın. Bu sorunları otomatik olarak azaltmak için Azure SQL Veritabanı'nda [otomatik plan düzeltmeyi](sql-database-automatic-tuning.md) etkinleştirin.

  Bu [örnek,](sql-database-performance-guidance.md#query-tuning-and-hinting) parametreli bir sorgu nedeniyle bir alt optimal sorgu planının etkisini, bu koşulun nasıl algılandığını ve çözümlemek için bir sorgu ipucunun nasıl kullanılacağını gösterir.

- Veritabanı uyumluluk düzeyini değiştirmeyi ve akıllı sorgu işlemeyi uygulamayı deneyin. SQL Query Optimizer veritabanınızın uyumluluk düzeyine bağlı olarak farklı bir sorgu planı oluşturabilir. Daha yüksek uyumluluk düzeyleri daha [akıllı sorgu işleme yetenekleri](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing)sağlar.

  - Sorgu işleme hakkında daha fazla bilgi için sorgu [işleme mimarisi kılavuzuna](https://docs.microsoft.com/sql/relational-databases/query-processing-architecture-guide)bakın.
  - Veritabanı uyumluluk düzeylerini değiştirmek ve uyumluluk düzeyleri arasındaki farklar hakkında daha fazla bilgi için [ALTER DATABASE'e](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level)bakın.
  - Kardinallik tahmini hakkında daha fazla bilgi için [Kardinallik Tahmini'ne](https://docs.microsoft.com/sql/relational-databases/performance/cardinality-estimation-sql-server) bakın

## <a name="resolving-queries-with-suboptimal-query-execution-plans"></a>Suboptimal sorgu yürütme planları ile sorguları çözme

Aşağıdaki bölümlerde, en uygun olmayan sorgu yürütme planıyla sorguların nasıl çözüleceği tartışılır.

### <a name="queries-that-have-parameter-sensitive-plan-psp-problems"></a><a name="ParamSniffing"></a>Parametre duyarlı plan (PSP) sorunları olan sorgular

Parametre duyarlı plan (PSP) sorunu, sorgu optimize edici yalnızca belirli bir parametre değeri (veya değer kümesi) için en uygun olan bir sorgu yürütme planı oluşturduğunda ve önbelleğe alınmış plan ardışık olarak kullanılan parametre değerleri için en uygun olmadığında ortaya çıkar Infaz. En iyi olmayan planlar sorgu performansı sorunlarına neden olabilir ve genel iş yükü çıktısı azaltabilir.

Parametre algılama ve sorgu işleme hakkında daha fazla bilgi için [Sorgu işleme mimarisi kılavuzuna](/sql/relational-databases/query-processing-architecture-guide#ParamSniffing)bakın.

Çeşitli geçici çözüm, PSP sorunlarını azaltabilir. Her geçici çözüm ilişkili tradeoffs ve dezavantajları vardır:

- Her sorgu yürütülmesinde [RECOMPILE](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu kullanın. Bu geçici çözüm, derleme süresini ve daha iyi plan kalitesi için CPU'nun artırılmasını sağlar. Bu `RECOMPILE` seçenek genellikle yüksek iş yükü gerektiren iş yükleri için mümkün değildir.
- Çoğu parametre değeri olasılığı için yeterince iyi bir plan üreten tipik bir parametre değeriyle gerçek parametre değerini geçersiz kılmak için [OPTION (İçİn OPTIMIZE ETME...)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu kullanın. Bu seçenek, en iyi parametre değerlerinin ve ilişkili plan özelliklerinin iyi anlaşılmasını gerektirir.
- Gerçek parametre değerini geçersiz kılmak ve bunun yerine yoğunluk vektörü ortalamasını kullanmak [için OPTION (MEÇHUL İçİn OPTIMIZE)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu kullanın. Bunu, yerel değişkenlerde gelen parametre değerlerini yakalayarak ve parametreleri kendileri kullanmak yerine yüklemler içindeki yerel değişkenleri kullanarak da yapabilirsiniz. Bu düzeltme için, ortalama yoğunluk *yeterince iyi*olmalıdır.
- [DISABLE_PARAMETER_SNIFFING](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu kullanarak parametre sniffing'i tamamen devre dışı bırak.
- Önbellekteki derlemeleri önlemek için [KEEPFIXEDPLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu kullanın. Bu geçici çözüm, yeterli olan iyi ortak planın zaten önbellekte olan olduğunu varsayar. Ayrıca, iyi planın tahliye edilme ve yeni bir kötü planın derlenme olasılığını azaltmak için otomatik istatistik güncelleştirmelerini devre dışı bırakabilirsiniz.
- Sorguyu yeniden yazarak ve sorgu metnine ipucu ekleyerek [USE PLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu açıkça kullanarak planı zorlayın. Veya Query Store'u kullanarak veya [otomatik ayarlamayı](sql-database-automatic-tuning.md)etkinleştirerek belirli bir plan ayarlayın.
- Tek yordamı, koşullu mantık ve ilişkili parametre değerlerine göre kullanılabilecek iç içe bir yordam kümesiyle değiştirin.
- Statik yordam tanımına dinamik dize yürütme alternatifleri oluşturun.

PSP sorunlarını çözme hakkında daha fazla bilgi için aşağıdaki blog gönderilerine bakın:

- [Parametre kokusu alıyorum.](https://docs.microsoft.com/archive/blogs/queryoptteam/i-smell-a-parameter)
- [Conor vs dinamik SQL ve parametreli sorgular için plan kalitesi](https://blogs.msdn.microsoft.com/conor_cunningham_msft/2009/06/03/conor-vs-dynamic-sql-vs-procedures-vs-plan-quality-for-parameterized-queries/)
- [SQL Server'da SQL sorgu optimizasyonu teknikleri: Parametre koklama](https://www.sqlshack.com/query-optimization-techniques-in-sql-server-parameter-sniffing/)

### <a name="compile-activity-caused-by-improper-parameterization"></a>Yanlış parametreizasyondan kaynaklanan etkinliği derleme

Bir sorgunun gerçek sayıları olduğunda, veritabanı altyapısı deyimi otomatik olarak parametreye ler veya kullanıcı derleme sayısını azaltmak için ifadeyi açıkça parametreleştirir. Aynı deseni kullanan bir sorgu için çok sayıda derleme, ancak farklı gerçek değerler yüksek CPU kullanımına neden olabilir. Benzer şekilde, yalnızca kısmen literals devam eden bir sorgu parametreize, veritabanı altyapısı daha fazla sorgu parametrelendirmez.

Aşağıda kısmen parametreli bir sorgu örneği verilmiştir:

```sql
SELECT *
FROM t1 JOIN t2 ON t1.c1 = t2.c1
WHERE t1.c1 = @p1 AND t2.c2 = '961C3970-0E54-4E8E-82B6-5545BE897F8F'
```

Bu `t1.c1` örnekte, `@p1`alır `t2.c2` , ama edebi olarak GUID almaya devam ediyor. Bu durumda, `c2`,sorgunun değerini değiştirirseniz, sorgu farklı bir sorgu olarak kabul edilir ve yeni bir derleme gerçekleşir. Bu örnekteki derlemeleri azaltmak için GUID'i de parametrenize eleştirirsiniz.

Aşağıdaki sorgu, sorgunun düzgün parametreli olup olmadığını belirlemek için sorgu karmasına göre sorgu sayısını gösterir:

```sql
SELECT TOP 10
  q.query_hash
  , count (distinct p.query_id ) AS number_of_distinct_query_ids
  , min(qt.query_sql_text) AS sampled_query_text
FROM sys.query_store_query_text AS qt
  JOIN sys.query_store_query AS q
     ON qt.query_text_id = q.query_text_id
  JOIN sys.query_store_plan AS p
     ON q.query_id = p.query_id
  JOIN sys.query_store_runtime_stats AS rs
     ON rs.plan_id = p.plan_id
  JOIN sys.query_store_runtime_stats_interval AS rsi
     ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE
  rsi.start_time >= DATEADD(hour, -2, GETUTCDATE())
  AND query_parameterization_type_desc IN ('User', 'None')
GROUP BY q.query_hash
ORDER BY count (distinct p.query_id) DESC
```

### <a name="factors-that-affect-query-plan-changes"></a>Sorgu planı değişikliklerini etkileyen etkenler

Sorgu yürütme planı yeniden derlemesi, özgün önbelleğe alınmış plandan farklı olarak oluşturulan bir sorgu planıyla sonuçlanabilir. Varolan özgün bir plan çeşitli nedenlerle otomatik olarak yeniden derlenebilir:

- Şemadaki değişiklikler sorgu tarafından başvurulmaktadır
- Tablolardaki veri değişiklikleri sorgu tarafından başvurulmaktadır
- Sorgu bağlamı seçenekleri değiştirildi

Derlenmiş bir plan, aşağıdakiler gibi çeşitli nedenlerle önbellekten çıkarılabilir:

- Örnek yeniden başlatılır
- Veritabanı kapsamı nagöre yapılandırma değişiklikleri
- Bellek baskısı
- Önbelleği temizlemek için açık istekler

RECOMPILE ipucu kullanırsanız, bir plan önbelleğe alınmıyor.

Yeniden derleme (veya önbellek tahliyesi sonrasında taze derleme) yine de orijinalle aynı olan bir sorgu yürütme planının oluşmasına neden olabilir. Plan önceki veya özgün plandan değiştiğinde, bu açıklamalar olasıdır:

- **Değiştirilen fiziksel tasarım**: Örneğin, yeni oluşturulan dizinler sorgugereksinimlerini daha etkili bir şekilde kapsar. Sorgu en iyiduruması, bu yeni dizin kullanarak sorgu yürütme ilk sürümü için başlangıçta seçilen veri yapısını kullanarak daha optimal olduğuna karar verirse yeni bir derleme üzerinde yeni dizinler kullanılabilir. Başvurulan nesnelerdeki herhangi bir fiziksel değişiklik derleme zamanında yeni bir plan seçimiyle sonuçlanabilir.

- **Sunucu kaynak farklılıkları**: Bir sistemdeki bir plan başka bir sistemdeki plandan farklıysa, kullanılabilir işlemci sayısı gibi kaynak kullanılabilirliği hangi planın oluşturulduğunu etkileyebilir. Örneğin, bir sistemde daha fazla işlemci varsa, paralel bir plan seçilebilir.

- **Farklı istatistikler**: Başvurulan nesnelerle ilişkili istatistikler değişmiş veya özgün sistemin istatistiklerinden önemli ölçüde farklı olabilir. İstatistikler değişir ve yeniden derleme gerçekleşirse, sorgu en iyiduruması değiştikleri andan itibaren istatistikleri kullanır. Gözden geçirilen istatistiklerin veri dağılımları ve frekansları özgün derlemeninkinden farklı olabilir. Bu değişiklikler, kardinallik tahminleri oluşturmak için kullanılır. *(Kardinallik tahminleri,* mantıksal sorgu ağacından akması beklenen satır sayısıdır.) Kardinallik tahminlerinde yapılan değişiklikler, farklı fiziksel işleçler ve ilişkili işlem emirlerini seçmenize neden olabilir. İstatistiklerde yapılan küçük değişiklikler bile sorgu yürütme planının değiştirilmesine neden olabilir.

- **Veritabanı uyumluluk düzeyi veya kardinallik tahmincisi sürümü değiştirildi**: Veritabanı uyumluluk düzeyinde yapılan değişiklikler, farklı bir sorgu yürütme planıyla sonuçlanacak yeni stratejiler ve özellikler sağlayabilir. Veritabanı uyumluluk düzeyinin ötesinde, devre dışı bırakılmış veya etkin leştirilmiş izleme bayrağı 4199 veya veritabanı kapsamı QUERY_OPTIMIZER_HOTFIXES yapılandırmanın değişen durumu da derleme zamanında sorgu yürütme planı seçeneklerini etkileyebilir. İzleme bayrakları 9481 (zorlama eski CE) ve 2312 (kuvvet varsayılan CE) de planı etkiler.

## <a name="resource-limits-issues"></a>Kaynak sınırlamaları sorunları

Suboptimal sorgu planları ve eksik dizinler ile ilgili olmayan yavaş sorgu performansı genellikle yetersiz veya aşırı kullanılan kaynaklarla ilişkilidir. Sorgu planı en uygunsa, sorgu (ve veritabanı) veritabanı, elastik havuz veya yönetilen örnek için kaynak sınırlarına isabet ediyor olabilir. Bir örnek, hizmet düzeyi için fazla günlük yazma iş düzeyi olabilir.

- Azure portalını kullanarak kaynak sorunlarını algılama: Kaynak sınırlarının sorun olup olmadığını görmek için [SQL Veritabanı kaynak izleme](sql-database-monitor-tune-overview.md#sql-database-resource-monitoring)konusuna bakın. Tek veritabanları ve esnek havuzlar için [Veritabanı Danışmanı performans önerileri](sql-database-advisor.md) ve Sorgu Performans [İstatistikleri'ne](sql-database-query-performance.md)bakın.
- [Akıllı Öngörüler](sql-database-intelligent-insights-troubleshoot-performance.md#reaching-resource-limits) kullanarak kaynak sınırlarını algılama
- [DMV'ler](sql-database-monitoring-with-dmvs.md)kullanarak kaynak sorunlarının algılanması:

  - [Sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) DMV, bir SQL veritabanı için CPU, G/Ç ve bellek tüketimini döndürür. Veritabanında etkinlik olmasa bile, her 15 saniyelik aralık için bir satır vardır. Geçmiş veriler bir saat boyunca korunur.
  - [Sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) DMV, Azure SQL Veritabanı için CPU kullanımını ve depolama verilerini döndürür. Veriler toplanır ve beş dakikalık aralıklarla toplanır.
  - [Kümülatif olarak yüksek CPU tüketen birçok ayrı sorgu](sql-database-monitoring-with-dmvs.md#many-individual-queries-that-cumulatively-consume-high-cpu)
  - 

Sorunu yetersiz kaynak olarak tanımlarsanız, CPU gereksinimlerini absorbe etmek için SQL veritabanınızın kapasitesini artırmak için kaynakları yükseltebilirsiniz. Daha fazla bilgi için bkz: [Azure SQL Veritabanı'nda tek veritabanı kaynaklarını ölçeklendirin](sql-database-single-database-scale.md) ve [Azure SQL Veritabanı'ndaki esnek havuz kaynaklarını ölçeklendirin.](sql-database-elastic-pool-scale.md) Yönetilen bir örneği ölçekleme hakkında bilgi için [Hizmet katmanı kaynak sınırlarına](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)bakın.

## <a name="performance-problems-caused-by-increased-workload-volume"></a>Artan iş yükü hacminden kaynaklanan performans sorunları

Uygulama trafiği ve iş yükü hacmindeki artış CPU kullanımının artmasına neden olabilir. Ama düzgün bu sorunu teşhis etmek için dikkatli olmalısınız. Yüksek CPU sorunu gördüğünüzde, artışın iş yükü birimindeki değişikliklerden kaynaklanıp kaynaklanmadığını belirlemek için aşağıdaki soruları yanıtlayın:

- Uygulamadan gelen sorgular yüksek CPU sorununun nedeni midir?
- [Tanımlayabileceğiniz en iyi CPU tüketen sorgular](sql-database-monitoring-with-dmvs.md#the-cpu-issue-occurred-in-the-past)için:

  - Birden çok yürütme planı aynı sorguyla ilişkili miydi? Eğer öyleyse, neden?
  - Aynı yürütme planına sahip sorgular için yürütme süreleri tutarlı mıydı? İnfaz sayısı arttı mı? Bu nedenle, iş yükü artışı büyük olasılıkla performans sorunlarına neden olabilir.

Özetle, sorgu yürütme planı farklı yürütülmediyse, ancak yürütme sayısıyla birlikte CPU kullanımı arttıysa, performans sorunu büyük olasılıkla iş yükü artışıyla ilişkilidir.

CPU sorununa neden olan iş yükü hacmi değişikliğini tanımlamak her zaman kolay değildir. Bu faktörleri göz önünde bulundurun:

- **Değiştirilen kaynak kullanımı**: Örneğin, CPU kullanımının uzun bir süre için yüzde 80'e yükseldiği bir senaryo düşünün. CPU kullanımı tek başına iş yükü hacminin değiştiği anlamına gelmez. Sorgu yürütme planındaki gerilemeler ve veri dağıtımındaki değişiklikler, uygulama aynı iş yükünü yürütse bile daha fazla kaynak kullanımına da katkıda bulunabilir.

- **Yeni bir sorgunun görünümü**: Uygulama, farklı zamanlarda yeni bir sorgu kümesini yönlendirebilir.

- **İstek sayısında artış veya azalma**: Bu senaryo, iş yükünün en belirgin ölçüsüdür. Sorgu sayısı her zaman daha fazla kaynak kullanımına karşılık gelmez. Ancak, bu metrik hala önemli bir sinyal, diğer faktörler değişmeden varsayarak.

[İş yükü artışlarını](sql-database-intelligent-insights-troubleshoot-performance.md#workload-increase) algılamak ve [gerilemeler planlamak](sql-database-intelligent-insights-troubleshoot-performance.md#plan-regression)için Intelligent Insights'ı kullanın.

## <a name="waiting-related-problems"></a>Beklemeyle ilgili sorunlar

Yürütme sorunlarıyla ilgili bir alt optimal planı ve *Bekleme yle ilgili* sorunları ortadan kaldırdıktan sonra, performans sorunu genellikle sorguların büyük olasılıkla bazı kaynak lar beklediğidir. Beklemeyle ilgili sorunlar neden olabilir:

- **Engelleme**:

  Diğerleri aynı nesnelere erişmeye çalışırken bir sorgu veritabanındaki nesnelerüzerinde kilit tutabilir. [DMVs](sql-database-monitoring-with-dmvs.md#monitoring-blocked-queries) veya [Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md#locking)kullanarak engelleme sorguları belirleyebilirsiniz.
- **IO sorunları**

  Sorgular, sayfaların verilere veya günlük dosyalarına yazılmasını bekliyor olabilir. Bu durumda, DMV'deki `INSTANCE_LOG_RATE_GOVERNOR` `WRITE_LOG`istatistikleri denetleyin veya `PAGEIOLATCH_*` bekleyin. [IO performans sorunlarını tanımlamak](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)için DMV kullanma konusuna bakın.
- **TempDB sorunları**

  İş yükü geçici tablolar kullanıyorsa veya planlarda TempDB dökülmeleri varsa, sorguların TempDB iş yüküyle ilgili bir sorunu olabilir. [TempDB sorunlarını kimlik kullanmak](sql-database-monitoring-with-dmvs.md#identify-tempdb-performance-issues)için DMV'ler kullanma konusuna bakın.
- **Bellekle ilgili sorunlar**

  İş yükü yeterli belleğe sahip değilse, sayfa yaşam beklentisi düşebilir veya sorgular gereksinim duyduklarından daha az bellek alabilir. Bazı durumlarda, Sorgu Optimize Edici'deki yerleşik zeka bellekle ilgili sorunları giderir. Bellek hibe sorunlarını [tanımlamak](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)için DMV kullanma konusuna bakın.

### <a name="methods-to-show-top-wait-categories"></a>En iyi bekleme kategorilerini gösterme yöntemleri

Bu yöntemler genellikle bekleme türlerinin üst kategorilerini göstermek için kullanılır:

- Artan beklemeler nedeniyle performans düşüşüne sahip sorguları tanımlamak için Akıllı [Öngörüler'i](sql-database-intelligent-insights-troubleshoot-performance.md#increased-wait-statistic) kullanın
- Zaman içinde her sorgu için bekleme istatistiklerini bulmak için [Sorgu Mağazası'nı](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) kullanın. Sorgu Deposu'nda bekleme türleri bekleme kategorilerinde birleştirilir. [Sys.query_store_wait_stats'da](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql#wait-categories-mapping-table)beklemek için bekleme kategorilerinin eşlemesini bulabilirsiniz.
- Sorgu işlemi sırasında çalıştırılan iş parçacıklarının karşılaştığı tüm beklemelerle ilgili bilgileri döndürmek için [sys.dm_db_wait_stats'yi](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) kullanın. Bu toplu görünümü, Azure SQL Veritabanı ile ve ayrıca belirli sorgular ve toplu işlerle performans sorunlarını tanılamak için kullanabilirsiniz. Sorgular kaynaklarda, sıra beklemede veya dış beklemelerde bekliyor olabilir.
- Bazı kaynaklarda bekleyen görevler in sırası hakkında bilgi döndürmek için [sys.dm_os_waiting_tasks'ı](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) kullanın.

Yüksek CPU senaryolarında, Sorgu Lehebi ve bekleme istatistikleri cpu kullanımını yansıtmayabilir:

- Yüksek CPU tüketen sorgular hala yürütülme.
- Bir hata olduğunda, yüksek CPU tüketen sorgular çalışıyor.

Sorgu Deposu'nun ve bekleme istatistiklerinin izini süren DMV'ler, yalnızca başarıyla tamamlanmış ve zamanlanmış sorguların sonuçlarını gösterir. İfadeler bitene kadar şu anda çalıştırılamakta olan deyimler için veri göstermezler. Şu anda çalıştırılamakta olan sorguları ve ilişkili alt çalışma süresini izlemek için dinamik yönetim görünümünü [sys.dm_exec_requests'ı](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) kullanın.

> [!TIP]
> Ek araçlar:
>
> - [TigerToolbox bekler ve mandallar](https://github.com/Microsoft/tigertoolbox/tree/master/Waits-and-Latches)
> - [TigerToolbox usp_whatsup](https://github.com/Microsoft/tigertoolbox/tree/master/usp_WhatsUp)

## <a name="next-steps"></a>Sonraki adımlar

[SQL Veritabanı izleme ve ayarlama genel bakış](sql-database-monitor-tune-overview.md)