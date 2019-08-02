---
title: '& Performans ayarlamayı izleme-Azure SQL veritabanı | Microsoft Docs'
description: Değerlendirme ve iyileştirme aracılığıyla Azure SQL veritabanı 'nda performans ayarlama ipuçları.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnik, carlrab
ms.date: 01/25/2019
ms.openlocfilehash: c52b41c4e6d0618b4df9b2aed985bbd22d89f419
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567202"
---
# <a name="monitoring-and-performance-tuning"></a>İzleme ve performans ayarlama

Azure SQL veritabanı, kullanımı kolayca izlemek, kaynak eklemek veya kaldırmak (CPU, bellek, g/ç), olası sorunları gidermek ve bir veritabanının performansını iyileştirebilecek öneriler bulmak için araçlar ve yöntemler sağlar. Azure SQL veritabanı, veritabanlarının iş yüküne uyum sağlayacak ve performansı otomatik olarak iyileştirmesine imkan tanıyan, veritabanlarındaki sorunları otomatik olarak giderebilecek birçok özelliğe sahiptir. Ancak, sorun gidermeye gerekebilecek bazı özel sorunlar vardır. Bu makalede, performans sorunlarını gidermek için kullanılabilen bazı en iyi yöntemler ve araçlar açıklanmaktadır.

Veritabanının sorunsuz çalıştığından emin olmak için yapılması gereken iki ana etkinlik vardır:
- Veritabanına atanan kaynakların iş yükünü işleyebileceğini doğrulamak için [veritabanı performansını izleme](#monitoring-database-performance) . Bir veritabanının kaynak sınırlarını vurmasını görürseniz, üst kaynak kullanma sorguları tanımlanmalıdır ve en iyi duruma getirilmeli ya da hizmet katmanını yükselterek daha fazla kaynak eklenmelidir.
- Olası bir sorunun neden oluştuğunu belirlemek için [performans sorunlarını giderin](#troubleshoot-performance-issues) , sorunun kök nedenini belirleyip sorunu giderecek eylemi gerçekleştirin.

## <a name="monitoring-database-performance"></a>Veritabanı performansını izleme

Azure SQL veritabanı performansını izlemeye, seçtiğiniz veritabanı performans düzeyiyle ilgili kaynak kullanımını izleyerek başlarsınız. Aşağıdaki kaynaklar aynı şekilde izlenmelidir:
 - **CPU kullanımı** -gelişmiş bir süre IÇIN veritabanının CPU kullanımının% 100 ' ine ulaşmakta olup olmadığını denetleyin. Bu, veritabanının veya örneğin daha yüksek bir hizmet katmanına yükseltilmesi gerektiğini veya işlem gücünün çoğunu kullanan sorguların tanımlanması ve ayarlanmış olması gerektiğini gösteriyor olabilir.
 - **Bekleme istatistikleri** -sorguların neden bazı kaynakları beklediğini denetleyin. Sorgular verilerin veritabanı dosyalarına alınmasını veya kaydedilmesini bekler, bazı kaynak sınırına ulaşıldığı için bekliyor.
 - **GÇ kullanımı** -veritabanının TEMELDEKI depolamanın GÇ sınırlarına ulaşmakta olup olmadığını denetleyin.
 - **Bellek kullanımı** -veritabanı veya örnek için kullanılabilir bellek miktarı, sanal çekirdek sayısıyla orantılıdır ve iş yükü için yeterli olup olmadığını kontrol edin. Sayfa ömrü erkeklerin sayfaların bellekten ne kadar hızlı bir şekilde kaldırıldığını gösterebilen parametrelerden biridir.

Azure SQL veritabanı hizmeti **, olası performans sorunlarını gidermeye ve gidermeye yardımcı olacak araçlar ve kaynaklar içerir**. Fırsatlar, [performans ayarlama önerilerini](sql-database-advisor.md)inceleyerek kaynakları değiştirmeden sorgu performansını iyileştirmek ve iyileştirmek için kolayca tanımlanabilir. Veritabanı performansının düşük olmasına yol açan yaygın nedenler, dizinlerin eksik olması ve sorguların hatalı bir şekilde iyileştirilmesidir. Bu ayarlama önerileri, iş yükünün performansını artırmak için uygulanabilir. Ayrıca, Azure SQL veritabanı 'nın, tüm tanımlanan önerileri uygulayarak ve bunun veritabanı performansını artırdığı için [sorguların performansını otomatik olarak iyileştirmelerine](sql-database-automatic-tuning.md) de izin veririz.

Veritabanı performansını izlemek ve sorunlarını gidermek için aşağıdaki seçenekler kullanılabilir:

- [Azure Portal](https://portal.azure.com), **SQL veritabanları**' na tıklayın, veritabanını seçin ve ardından izleme grafiğini kullanarak en yüksek kullanımlarına yaklaştığı kaynakları arayın. DTU tüketimi varsayılan olarak gösterilir. Gösterilen zaman aralığını ve değerleri değiştirmek için **Düzenle** ' ye tıklayın.
- SQL Server Management Studio gibi araçlar, kaynak kullanımını izlemek ve en üstteki kaynak kullanan sorguları belirlemek için bir [performans panosu](https://docs.microsoft.com/sql/relational-databases/performance/performance-dashboard?view=sql-server-2017) gibi çok sayıda faydalı rapor sağlar ve gerileyen sorguları tanımlamak Için [sorgu deposu](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store#Regressed) ' nu sorgular mının.
- Kaynakların en iyi şekilde harcadıkları sorguları belirlemek için [Azure portal](https://portal.azure.com) [sorgu performansı içgörüleri](sql-database-query-performance.md) kullanın. Bu özellik yalnızca Tek Veritabanı ve elastik havuzlarda kullanılabilir.
- Dizin oluşturma ve bırakma, sorguları parametreleme ve şema sorunlarını düzeltme önerilerini görüntülemek için [SQL veritabanı Danışmanı](sql-database-advisor-portal.md) kullanın. Bu özellik yalnızca Tek Veritabanı ve elastik havuzlarda kullanılabilir.
- Veritabanı performansını otomatik olarak izlemek için [Azure SQL akıllı içgörüler](sql-database-intelligent-insights.md) kullanın. Bir performans sorunu algılandığında, sorunun ayrıntıları ve kök neden analizi (RCA) ile bir tanılama günlüğü oluşturulur. Mümkün olduğunda performans iyileştirme önerisi sunulmaktadır.
- [Otomatik ayarlamayı etkinleştirin](sql-database-automatic-tuning-enable.md) ve Azure SQL veritabanı 'nın belirlenen performans sorunlarını otomatik olarak çözmesine izin verin.
- Performans sorunları hakkında daha ayrıntılı sorun giderme için [dinamik yönetim görünümlerini (DMVs)](sql-database-monitoring-with-dmvs.md), [Genişletilmiş olayları](sql-database-xevent-db-diff-from-svr.md)ve [sorgu deposunu](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) kullanın.

> [!TIP]
> Yukarıdaki yöntemlerden bir veya daha fazlasını kullanarak performans sorununu tanımladıktan sonra Azure SQL veritabanı 'nın performansını artırmak için kullanabileceğiniz teknikleri bulmak için [performans Kılavuzu](sql-database-performance-guidance.md) ' na bakın.

## <a name="troubleshoot-performance-issues"></a>Performans sorunlarını giderme

Performans sorunlarını tanılamak ve çözümlemek için, her bir etkin sorgunun durumunu ve her bir iş yükü durumuyla ilgili performans sorunlarına neden olan koşulları anlamak için başlayın. Azure SQL veritabanı performansını geliştirmek için, uygulamadan gelen her etkin sorgu isteğinin çalışan veya bekleme durumunda olduğunu anlayın. Azure SQL veritabanında bir performans sorununu giderirken, performans sorunlarını tanılamak ve çözmek için bu makaleyi okuduğumuz sırada aşağıdaki grafiği aklınızda tutun.

![İş yükü durumları](./media/sql-database-monitor-tune-overview/workload-states.png)

Performans sorunlarına sahip bir iş yükü için, performans sorunu CPU çekişmesine ( **çalışırken çalışan** bir koşul) veya tek tek sorguların bir şeyi ( **bekleme ile ilgili** bir koşul) bekliyor olabilir.

**Çalıştırmaya** ilişkin sorunların nedenleri şu olabilir:
- **Derleme sorunları** -SQL sorgu İdealleştirici, eski istatistikler, işlenecek satır sayısının yanlış tahmin edilmesi veya gerekli belleğin tahmini olması nedeniyle en iyi alt plana sahip olabilir. Sorgunun geçmişte veya başka bir örnekte (yönetilen örnek veya SQL Server örneği) daha hızlı yürütüldüğünü biliyoruz, gerçek yürütme planlarını alın ve farklı olup olmadığını görmek için bunları karşılaştırın. Daha iyi planı almak için sorgu ipuçları uygulamayı veya istatistikleri yeniden oluşturmayı veya dizinleri yeniden oluşturmayı deneyin. Bu sorunları otomatik olarak azaltmak için Azure SQL veritabanı 'nda otomatik plan düzeltmesini etkinleştirin.
- **Yürütme sorunları** -sorgu planının en iyi durumda olması, veritabanında günlük yazma işlemi gibi bazı kaynak limitlerine ulaşarak veya yeniden oluşturulması gereken birleştirilmiş dizinleri kullanıyor olması olabilir. Kaynakları harcama halinde olan çok sayıda eşzamanlı sorgu, yürütme sorunları nedeniyle de olabilir. Çoğu durumda, etkili bir şekilde yürütülmediği sorgular büyük olasılıkla bazı kaynakları beklerken, çoğu durumda **bekleyen ilgili** sorunlar yürütme sorunlarıyla ilgilidir.

**Bekleme ile ilgili** sorunların nedenleri şu olabilir:
- **Engelleme** -bir sorgu, diğerleri aynı nesnelere erişmeye çalışırken veritabanındaki bazı nesneler üzerindeki kilidi tutabilir. Sorgu engelleme, DMV veya izleme araçları kullanılarak kolayca tanımlanabilir.
- **GÇ sorunları** -sorgular, sayfaların verilere veya günlük dosyalarına yazılmasını bekliyor olabilir. Bu durumda, DMV `WRITE_LOG`'de, `PAGEIOLATCH_*` veya bekleme istatistiklerini görün `INSTANCE_LOG_RATE_GOVERNOR`.
- **Tempdb sorunları** -iş yükü çok sayıda geçici tablo kullanıyorsa veya planlarda çok sayıda tempdb kopyası varsa, sorgular tempdb aktarım hızı ile ilgili bir sorun olabilir. 
- **Bellekle ilgili sorunlar** -sayfa ömrü erkeklerin veya sorgular gerekenden daha az bellek alıyor olması için iş yükü için yeterli bellek olmayabilir. Bazı durumlarda, sorgu Iyileştiricinizdeki yerleşik zeka bu sorunları çözmeyecektir.
 
 Aşağıdaki bölümler, bu sorunlardan bazılarının nasıl tanımlanacağına ve giderileceğine ilişkin açıklanacaktır.

## <a name="running-related-performance-issues"></a>Çalışırken ilgili performans sorunları

Genel bir kılavuz olarak, CPU kullanımı sürekli olarak% 80 veya daha yüksek ise, çalışırken ilgili bir performans sorunu vardır. Çalışırken ilgili bir sorun varsa, bu, yetersiz CPU kaynaklarından kaynaklanıyor olabilir veya aşağıdaki koşullardan biri ile ilişkili olabilir:

- Çok fazla çalışan sorgu
- Çok fazla derleme sorgusu
- Bir veya daha fazla çalışan sorgu en iyi bir sorgu planı kullanıyor

Güvenlikle ilgili bir performans sorunu olduğunu tespit ediyorsanız, amaç bir veya daha fazla yöntem kullanarak kesin sorunu belirlemektir. Çalıştırmaya ilişkin sorunları belirlemek için en yaygın yöntemler şunlardır:

- CPU yüzdesi kullanımını izlemek için [Azure Portal](sql-database-manage-after-migration.md#monitor-databases-using-the-azure-portal) kullanın.
- Aşağıdaki [dinamik yönetim görünümlerini](sql-database-monitoring-with-dmvs.md)kullanın:

  - [sys. DM _db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) , BIR Azure SQL VERITABANı için CPU, g/ç ve bellek tüketimi döndürür. Veritabanında etkinlik olmasa bile, her 15 saniyelik Aralık için bir satır vardır. Geçmiş verileri bir saat boyunca tutulur.
  - [sys. resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) , BIR Azure SQL VERITABANı için CPU kullanımı ve depolama verileri döndürür. Veriler, beş dakikalık aralıklar içinde toplanır ve toplanır.

> [!IMPORTANT]
> Bu DMV 'leri kullanarak bir T-SQL sorguları ayarlama CPU kullanımı sorunlarını gidermek için bkz. [CPU performans sorunlarını belirleme](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues).

### <a name="ParamSniffing"></a>Parametreye duyarlı sorgu yürütme planı sorunlarıyla olan sorgularda sorun giderme

Parametre duyarlı plan (PSP) sorunu, sorgu iyileştiricinin yalnızca belirli bir parametre değeri (veya değerler kümesi) için ideal olan bir sorgu yürütme planı oluşturduğu bir senaryoya başvurur ve önbelleğe alınmış plan daha sonra ' de kullanılan parametre değerleri için en iyi duruma getirilmemiş ardışık yürütmeler. En iyi durumda olmayan planlar sorgu performans sorunlarına ve genel iş yükü verimlilik düşüşüne neden olabilir. Parametre algılaması ve sorgu işleme hakkında daha fazla bilgi için bkz. [sorgu Işleme Mimarisi Kılavuzu](/sql/relational-databases/query-processing-architecture-guide#ParamSniffing).

Her biri ilişkili avantajları ve dezavantajları olan bu sorunları azaltmak için kullanılan birkaç geçici çözüm vardır:

- Her sorgu yürütmesinde sorgu ipucunu yeniden [Derle](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) ' i kullanın. Bu geçici çözüm, daha iyi plan kalitesi için derleme süresini ve CPU 'YU artırabilir. `RECOMPILE` Seçeneğinin kullanılması, genellikle yüksek bir işleme gerektiren iş yükleri için mümkün değildir.
- Gerçek parametre değerini, çoğu parametre değeri olasılıklarından daha iyi bir plan üreten tipik bir parametre değeri ile geçersiz kılmak için [seçeneğini kullanın (...)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucu.   Bu seçenek, en iyi parametre değerlerinin ve ilişkili plan özelliklerinin iyi bir şekilde anlaşılmasına gerek duyar.
- Yoğunluk vektörü ortalamasını kullanarak Exchange 'teki gerçek parametre değerini geçersiz kılmak için [OPTION (bılınmıyor IÇIN iyileştirin)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu kullanın. Bunu yapmanın başka bir yolu da, gelen parametre değerlerini yerel değişkenlere yakalayıp, sonra parametrelerin kendileri yerine koşulların içindeki yerel değişkenleri kullanarak kullanmaktır. Bu düzeltmeyle, ortalama yoğunluğu *yeterince iyi* olmalıdır.
- [DISABLE_PARAMETER_SNIFFING](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu kullanarak parametre algılama özelliğini tamamen devre dışı bırakın.
- Önbellekte yeniden derleme yapılmasını engellemek için [KeepFixedPlan](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) sorgu ipucunu kullanın. Bu geçici çözüm, ön önbellekte bulunan, *uygun* olan genel planı kabul eder. Ayrıca, çıkarılan bir planın ve yeni bir hatalı planın derlenme olasılığını azaltmak için istatistiklerin otomatik güncelleştirmelerini devre dışı bırakabilirsiniz.
- Planı sorgu ipucunu [kullanın](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) (sorgu deposu kullanarak belirli bir planı ayarlayarak veya [otomatik ayarlamayı](sql-database-automatic-tuning.md)etkinleştirerek) planı açık olarak belirterek zorla.
- Tek yordamı, her biri koşullu Logic ve ilişkili parametre değerlerine göre kullanılabilecek, iç içe geçmiş bir yordamlar kümesiyle değiştirin.
- Statik yordam tanımına dinamik dize yürütme alternatifleri oluşturun.

Bu tür sorunları çözme hakkında daha fazla bilgi için bkz.:

- Bu, [bir parametre](https://blogs.msdn.microsoft.com/queryoptteam/2006/03/31/i-smell-a-parameter/) blog gönderisini
- [Parametreli sorgular için bu dinamik SQL ve plan kalitesi](https://blogs.msdn.microsoft.com/conor_cunningham_msft/2009/06/03/conor-vs-dynamic-sql-vs-procedures-vs-plan-quality-for-parameterized-queries/) blog gönderisi
- SQL Server [içindeki bu SQL sorgusu iyileştirme teknikleri: Parameter algılaması](https://www.sqlshack.com/query-optimization-techniques-in-sql-server-parameter-sniffing/) blog gönderisi

### <a name="troubleshooting-compile-activity-due-to-improper-parameterization"></a>Yanlış Parametreleştirme nedeniyle derleme etkinliği sorunlarını giderme

Bir sorguda değişmez değerler olduğunda, veritabanı motoru otomatik olarak Parametreleştirme yapmayı seçer veya bir Kullanıcı, derleme sayısını azaltmak için açıkça parametreleştirebilirsiniz. Aynı model kullanılarak bir sorgunun yüksek sayıda derleme, ancak farklı değişmez değer değerleri yüksek CPU kullanımına neden olabilir. Benzer şekilde, yalnızca sabit değerlere sahip olmaya devam eden bir sorguyu kısmen parametreleştirmeye devam ederseniz, veritabanı altyapısı onu daha fazla parametrelemez.  Kısmen parametreli bir sorgunun örneği aşağıda verilmiştir:

```sql
SELECT * FROM t1 JOIN t2 ON t1.c1 = t2.c1
WHERE t1.c1 = @p1 AND t2.c2 = '961C3970-0E54-4E8E-82B6-5545BE897F8F'
```

Önceki örnekte, alır `t1.c1` `@p1` , ancak `t2.c2` GUID 'yi hazır değer olarak alma devam eder. Bu durumda, için `c2`değeri değiştirirseniz, sorgu farklı bir sorgu olarak değerlendirilir ve yeni bir derleme gerçekleşmeyecektir. Önceki örnekteki derlemeleri azaltmak için, çözüm GUID de parametreleştirilemez.

Aşağıdaki sorgu sorgunun doğru parametreli olup olmadığını anlamak için sorgu karmamına göre sorguların sayısını gösterir:

```sql
SELECT  TOP 10  
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
### <a name="factors-influencing-query-plan-changes"></a>Sorgu planı değişikliklerini etkileyen faktörler

Sorgu yürütme planı yeniden derleme işlemi, ilk olarak önbelleğe alınmış olandan farklı olan üretilmiş bir sorgu planına neden olabilir. Mevcut bir Orijinal planın otomatik olarak yeniden derlenmesi için çeşitli nedenler vardır:
- Sorgu tarafından başvurulan şemadaki değişiklikler
- Sorgu tarafından başvurulan tablolardaki veri değişiklikleri 
- Sorgu bağlamı seçeneklerindeki değişiklikler 

Derlenmiş bir plan, örnek yeniden başlatmaları, veritabanı kapsamlı yapılandırma değişiklikleri, bellek baskısı ve önbelleği temizlemek için açık istekler dahil çeşitli nedenlerle önbellekten çıkarılabilir. Ayrıca, yeniden derleme ipucu kullanmak bir planın önbelleğe alınmayacağı anlamına gelir.

Yeniden derleme (veya önbellek çıkartım sonrasında yeni derleme), ilk olarak gözlemlenen bir tane aynı sorgu yürütme planının oluşturulmasına neden olabilir.  Ancak, planda önceki veya orijinal planla karşılaştırılan değişiklikler varsa, sorgu yürütme planının neden değiştiği hakkında en yaygın açıklamalar aşağıda verilmiştir:

- **Değiştirilen fiziksel tasarım**. Örneğin, bir sorgunun gereksinimlerini daha etkin bir şekilde kapsayan yeni dizinler oluşturulmuştur. Sorgu iyileştiricisi, sorgu yürütmenin ilk sürümü için başlangıçta seçili olan veri yapısını kullanlandan bu yeni bir derlemede kullanılabilir.  Başvurulan nesnelerde yapılan tüm fiziksel değişiklikler, derleme zamanında yeni bir plan seçimine neden olabilir.

- **Sunucu kaynak farkları**. Bir planın "Sistem A" ve "sistem B" ile farklı olduğu bir senaryoda, kullanılabilir işlemci sayısı gibi kaynakların kullanılabilirliği, hangi planın oluşturulduğunu etkileyebilir.  Örneğin, bir sistem daha yüksek sayıda işlemciye sahipse, paralel bir plan seçilebilir. 

- **Farklı istatistikler**. Başvurulan nesnelerle ilişkili istatistikler değişir veya özgün sistemin istatistikleriyle göre farklılık gösterilmiyor.  İstatistik değişikliği ve bir yeniden derleme gerçekleşirse, sorgu iyileştiricisi o zaman içindeki belirli bir noktaya göre istatistikleri kullanır. Düzeltilen istatistikler önemli ölçüde farklı veri dağıtımlarını ve özgün derlemede durum olmayan frekansları içerebilir.  Bu değişiklikler, kardinalite tahminlerini tahmin etmek için kullanılır (mantıksal sorgu ağacı üzerinden akış için beklenen satır sayısı).  Kardinalite tahminlerinde yapılan değişiklikler, farklı fiziksel işleçler ve ilişkili işlemler sırası seçmemize neden olabilir.  İstatistikte küçük değişiklikler bile değiştirilen bir sorgu yürütme planına yol açabilir.

- **Veritabanı uyumluluk düzeyi veya kardinalite tahmin aracı sürümü değiştirildi**.  Veritabanı uyumluluk düzeyinde yapılan değişiklikler, farklı bir sorgu yürütme planına neden olabilecek yeni stratejileri ve özellikleri etkinleştirebilir.  Veritabanı uyumluluk düzeyinin ötesinde, 4199 izleme bayrağını devre dışı bırakma veya etkinleştirme ya da veritabanı kapsamlı yapılandırma QUERY_OPTIMIZER_HOTFIXES durumunu değiştirme, derleme zamanında sorgu yürütme planı seçimlerini de etkileyebilir.  İzleme bayrakları 9481 (eski CE 'yi zorla) ve 2312 (varsayılan CE 'yi zorla) Ayrıca, ' i de etkiliyor. 

### <a name="resolve-problem-queries-or-provide-more-resources"></a>Sorun sorgularını çözümleyin veya daha fazla kaynak sağlayın

Sorunu belirledikten sonra, Azure SQL veritabanınızın kapasitesini artışlarını devralarak CPU gereksinimlerine yükseltmek için sorun sorgularını ayarlayabilir veya işlem boyutunu ya da hizmet katmanını yükseltebilirsiniz. Tek veritabanlarına yönelik kaynakları ölçeklendirmeyle ilgili bilgi için bkz. [Azure SQL veritabanı 'nda tek veritabanı kaynaklarını](sql-database-single-database-scale.md) ölçeklendirme ve elastik havuzlara yönelik ölçek kaynakları için bkz. [Azure SQL veritabanı 'nda elastik havuz kaynaklarını](sql-database-elastic-pool-scale.md)ölçeklendirme. Yönetilen örneği ölçeklendirme hakkında daha fazla bilgi için bkz. [örnek düzeyi kaynak sınırları](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).

### <a name="determine-if-running-issues-due-to-increase-workload-volume"></a>İş yükü birimi arttırması nedeniyle çalışan sorunlar olup olmadığını belirleme

Uygulama trafiği ve iş yükündeki artış, daha fazla CPU kullanımı için hesap oluşturabilir, ancak bu sorunu doğru bir şekilde tanılamak için dikkatli olmanız gerekir. Yüksek CPU senaryosunda, gerçekten iş yükü birimi değişikliklerinden kaynaklanan bir CPU artışının olup olmadığını öğrenmek için bu soruları yanıtlayın:

1. Uygulamanın sorguları yüksek CPU sorununun nedeni mi?
2. En yüksek CPU kullanan sorgular için (tanımlanabilecek):

   - Aynı sorguyla ilişkili birden fazla yürütme planı olup olmadığını belirleme. Öyleyse, nedenini tespit edin.
   - Aynı yürütme planına sahip sorgular için, yürütme sürelerinin tutarlı olup olmadığını ve yürütme sayısı arttırılmadığını saptayın. Yanıt Evet ise, iş yükü artışı nedeniyle olası performans sorunları vardır.

Özetlemek gerekirse, sorgu yürütme planı farklı şekilde yürütülemedi ancak yürütme sayısı ile birlikte CPU kullanımı arttıkça, iş yükü artışı ile ilgili performans sorunu büyük olasılıkla artar.

Bir CPU sorunu eden bir iş yükü birimi değişikliği olması her zaman kolay değildir.   Göz önünde bulundurulması gereken etmenler: 

- **Kaynak kullanımı değişti**

  Örneğin, CPU 'nun uzun süre% 80 ' e arttığı bir senaryo düşünün.  CPU kullanımı tek başına iş yükü birimi değişti.  Sorgu yürütme planı gerilemeleri ve veri dağıtımı değişiklikleri, uygulama aynı tam iş yükünü yürütüyor olsa da daha fazla kaynak kullanımına katkıda bulunabilir.

- **Yeni sorgu görüntülendi**

   Bir uygulama, farklı zamanlarda yeni bir sorgu kümesi oluşturabilir.

- **Artırılan veya azaltılan istek sayısı**

   Bu senaryo, iş yükünün en belirgin ölçüdür. Sorgu sayısı, her zaman daha fazla kaynak kullanımına karşılık gelmez. Ancak, bu ölçüm, diğer faktörlerin değiştirilmemiş olduğu varsayıldığında önemli bir sinyaldir.

## <a name="waiting-related-performance-issues"></a>Bekleme ile ilgili performans sorunları

Yüksek CPU, çalıştırmaya yönelik bir performans sorununa bağlı kalmadıktan sonra, beklenmeden ilgili bir performans sorunuyla karşı karşıya görürsünüz. Yani, CPU kaynakları başka bir kaynakta beklediği için CPU kaynaklarınız etkili bir şekilde kullanılmaz. Bu durumda, bir sonraki adımınız, CPU kaynaklarınızın ne kadar beklediğini belirlemektir. En sık kullanılan bekleme türü kategorilerini göstermek için en yaygın Yöntemler:

- [Sorgu deposu](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) , zaman içinde sorgu başına bekleme istatistikleri sağlar. Sorgu deposunda, bekleme türleri bekleme kategorilerine birleştirilir. Bekleme kategorilerinin bekleme türleri eşleştirmesi [sys. query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql#wait-categories-mapping-table)içinde kullanılabilir.
- [sys. DM _db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) , işlem sırasında yürütülen iş parçacıklarının karşılaştığı tüm bekler hakkındaki bilgileri döndürür. Azure SQL veritabanı ve ayrıca belirli sorgular ve toplu işlerle ilgili performans sorunlarını tanılamak için bu toplanmış görünümü kullanabilirsiniz.
- [sys. DM _os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) , bazı kaynakları bekleyen görevlerin bekleme kuyruğu hakkında bilgi döndürür.

Yüksek CPU senaryolarında, sorgu deposu ve bekleme istatistikleri her zaman CPU kullanımını Bu iki nedenden dolayı yansıtmaz:

- Yüksek CPU kullanan sorgular hala yürütülüyor olabilir ve sorgular bitmedi
- Yük devretme sırasında yüksek CPU kullanan sorgular çalışıyor

Sorgu deposu ve bekleme istatistikleri-dinamik yönetim görünümlerinin izlenmesi yalnızca başarıyla tamamlanan ve zaman aşımına uğrayan sorgular için sonuçları gösterir ve şu anda yürütülmekte olan deyimler için verileri göstermez (tamamlanana kadar). Dinamik yönetim görünümü [sys. DM _exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) , şu anda yürütülen sorguları ve ilişkili çalışan saatini izlemenize olanak sağlar.

Önceki grafikte gösterildiği gibi en sık görülen bekler şunlardır:

- Kilitler (engelleme)
- G/Ç
- `tempdb`-ilgili çekişme
- Bellek izni bekler

> [!IMPORTANT]
> Bu ve ilgili sorunları gidermek için bu DMV 'leri kullanarak T-SQL sorguları ayarlama bölümüne bakın:
>
> - [G/ç performans sorunlarını tanımla](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Performans `tempdb` sorunlarını tanımla](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Bellek verme süresi istemini tanımla](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)
> - [TigerToolbox-wait ve Ladallar](https://github.com/Microsoft/tigertoolbox/tree/master/Waits-and-Latches)
> - [TigerToolbox-usp_whatsup](https://github.com/Microsoft/tigertoolbox/tree/master/usp_WhatsUp)

## <a name="improving-database-performance-with-more-resources"></a>Daha fazla kaynakla veritabanı performansını artırma

Son olarak, veritabanınızın performansını iyileştirebilecek bir işlem yapılabilir öğe yoksa, Azure SQL veritabanı 'nda kullanılabilir kaynak miktarını değiştirebilirsiniz. Tek bir veritabanının [DTU hizmeti katmanını](sql-database-service-tiers-dtu.md) değiştirerek veya elastik bir havuzun eDTU 'sunu dilediğiniz zaman artırarak daha fazla kaynak atayabilirsiniz. Alternatif olarak, [sanal çekirdek tabanlı satın alma modelini](sql-database-service-tiers-vcore.md)kullanıyorsanız, hizmet katmanını değiştirebilir veya veritabanınıza ayrılan kaynakları artırabilirsiniz.

1. Tek veritabanları için, [Hizmet katmanlarını](sql-database-single-database-scale.md) veya [işlem kaynaklarını](sql-database-single-database-scale.md) isteğe bağlı olarak değiştirerek veritabanı performansını artırabilirsiniz.
2. Birden çok veritabanı için, kaynakları otomatik olarak ölçeklendirmek için [Esnek havuzlar](sql-database-elastic-pool-guidance.md) kullanmayı düşünün.

## <a name="tune-and-refactor-application-or-database-code"></a>Uygulama veya veritabanı kodunu ayarlama ve yeniden düzenleme

Veritabanını daha en iyi şekilde kullanmak, dizinleri değiştirmek, planları zorlamak veya veritabanını iş yükünüze el ile uyarlamak için ipuçları kullanmak için uygulama kodunu değiştirebilirsiniz. El ile ayarlama ve [performans Kılavuzu konu](sql-database-performance-guidance.md) makalesindeki kodu yeniden yazma için bazı kılavuz ve ipuçları bulun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı 'nda otomatik ayarlamayı etkinleştirmek ve otomatik ayarlama özelliğinin iş yükünüzü tamamen yönetmesine izin vermek için bkz. [otomatik ayarlamayı etkinleştirme](sql-database-automatic-tuning-enable.md).
- El ile ayarlamayı kullanmak için [Azure Portal ayarlama önerilerini](sql-database-advisor-portal.md) gözden geçirebilir ve Sorgularınızın performansını geliştiren olanları el ile uygulayabilirsiniz.
- [Azure SQL veritabanı hizmet katmanlarını](sql-database-performance-guidance.md) değiştirerek veritabanınızda kullanılabilir kaynakları değiştirme
