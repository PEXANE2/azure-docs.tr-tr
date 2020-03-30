---
title: Uygulamalar ve veritabanları için performans alamı kılavuzu
description: Azure SQL Veritabanı'nda performans için veritabanı uygulamalarını ve veritabanlarını alabilmem hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: carlrab; jrasnick
ms.date: 03/10/2020
ms.openlocfilehash: 4f30ebe39d86db7076baa8c29b2a5cf060b07bf5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255957"
---
# <a name="tune-applications-and-databases-for-performance-in-azure-sql-database"></a>Azure SQL Veritabanı'nda performans için uygulamaları ve veritabanlarını ayarlayın

SQL Veritabanı ile karşı karşıya olduğunuz bir performans sorununu tanımladıktan sonra, bu makale size yardımcı olmak üzere tasarlanmıştır:

- Uygulamanızı ayarlayın ve performansı artırabilecek bazı en iyi uygulamaları uygulayın.
- Verilerle daha verimli çalışmak için dizinleri ve sorguları değiştirerek veritabanını ayarlayın.

Bu makalede, Azure SQL [Veritabanı veritabanı danışmanı önerileri](sql-database-advisor.md) ve Azure SQL Veritabanı otomatik [ayarı önerileri](sql-database-automatic-tuning.md)üzerinde zaten çalıştığınızvarsayar. Ayrıca, izleme ve ayarlama [ve](sql-database-monitor-tune-overview.md) sorun giderme performansı sorunları yla ilgili ilgili makalelere genel bir bakış gözden geçirdiğinizvarsa varsayıyor. Ayrıca, bu makalede, veritabanınıza daha fazla kaynak sağlamak için bilgi işlem boyutunu veya hizmet katmanını artırarak çözülebilecek bir CPU kaynaklarınız, çalışmayla ilgili performans sorununa sahip olmadığını varsayar.

## <a name="tune-your-application"></a>Uygulamanızı ayarlayın

Geleneksel şirket içi SQL Server'da, ilk kapasite planlama işlemi genellikle üretimde bir uygulamayı çalıştırma işleminden ayrılır. Donanım ve ürün lisansları önce satın alınır ve performans daha sonra yapılır. Azure SQL Veritabanı'nı kullandığınızda, bir uygulamayı çalıştırma ve atürleme işlemini iç içe yürütmek iyi bir fikirdir. İsteğe bağlı kapasite için ödeme modeliyle, uygulamanızı, genellikle yanlış olan bir uygulama için gelecekteki büyüme planlarının tahminlerine dayalı olarak donanıma aşırı sağlama yerine, gereken minimum kaynakları kullanacak şekilde ayarlayabilirsiniz. Bazı müşteriler bir uygulamayı ayarlamamayı seçebilir ve bunun yerine donanım kaynaklarını aşırı sağlamayı seçebilir. Yoğun bir dönemde önemli bir uygulamayı değiştirmek istemiyorsanız, bu yaklaşım iyi bir fikir olabilir. Ancak, Azure SQL Veritabanı'ndaki hizmet katmanlarını kullandığınızda, bir uygulamayı ayarlayama, kaynak gereksinimlerini en aza indirebilir ve aylık faturaları düşürebilir.

### <a name="application-characteristics"></a>Uygulama özellikleri

Azure SQL Veritabanı hizmet katmanları bir uygulama için performans kararlılığını ve öngörülebilirliği artırmak için tasarlanmış olsa da, bazı en iyi uygulamalar uygulamanızı bilgi işlem boyutundaki kaynaklardan daha iyi yararlanmak için ayarlamanıza yardımcı olabilir. Birçok uygulama yalnızca daha yüksek bir bilgi işlem boyutuna veya hizmet katmanına geçerek önemli performans kazançlarına sahip olsa da, bazı uygulamaların daha yüksek bir hizmet düzeyinden yararlanmak için ek aparata ihtiyacı vardır. Daha yüksek performans için, bu özelliklere sahip uygulamalar için ek uygulama atonlama düşünün:

- **"Geveze" davranışı nedeniyle yavaş performansa sahip uygulamalar**

  Geveze uygulamalar, ağ gecikmesine duyarlı aşırı veri erişim işlemleri yapar. SQL veritabanına veri erişim işlemleri sayısını azaltmak için bu tür uygulamaları değiştirmeniz gerekebilir. Örneğin, geçici sorguları toplu çıkarma veya sorguları depolanan yordamlara taşıma gibi teknikler kullanarak uygulama performansını artırabilirsiniz. Daha fazla bilgi için [Toplu iş sorgularına](#batch-queries)bakın.

- **Tek bir makinenin tamamı tarafından desteklenemez yoğun bir iş yüküne sahip veritabanları**

   En yüksek Premium işlem boyutundaki kaynakları aşan veritabanları iş yükünü ölçeklemenin avantajını sağlayabilir. Daha fazla bilgi için [bkz.](#cross-database-sharding) [Functional partitioning](#functional-partitioning)

- **Alt en uygun sorguları olan uygulamalar**

  Özellikle veri erişim katmanındaki, sorguları kötü ayarlanmış olan uygulamalar daha yüksek bir bilgi işlem boyutundan fayda sağlamayabilir. Buna WHERE yan tümcesi olmayan, eksik dizinleri olan veya eski istatistikleri olan sorgular dahildir. Bu uygulamalar standart sorgu performans tuning tekniklerinden yararlanır. Daha fazla bilgi için [Eksik dizinler](#identifying-and-adding-missing-indexes) ve [Sorgu atonlama ve ipucu](#query-tuning-and-hinting)na bakın.

- **En uygun olmayan veri erişim tasarımına sahip uygulamalar**

   İçsel veri erişimi eşzamanlılık sorunları olan uygulamalar (örneğin kilitlenme), daha yüksek bir bilgi işlem boyutundan fayda sağlamayabilir. Azure Önbelleğe Alma hizmeti veya başka bir önbelleğe alma teknolojisiyle istemci tarafındaki verileri önbelleğe alarak Azure SQL Veritabanı'na yönelik gidiş-dönüş yolculukları azaltmayı düşünün. Bkz. [Uygulama katmanı önbelleğe alma.](#application-tier-caching)

## <a name="tune-your-database"></a>Veritabanınızı ayarlayın

Bu bölümde, uygulamanız için en iyi performansı elde etmek ve mümkün olan en düşük bilgi işlem boyutunda çalıştırmak için Azure SQL Veritabanı'nı ayarlamak için kullanabileceğiniz bazı tekniklere bakıyoruz. Bu tekniklerden bazıları geleneksel SQL Server atonlama en iyi uygulamaları eşleşir, ancak diğerleri Azure SQL Veritabanı'na özgüdir. Bazı durumlarda, geleneksel SQL Server tekniklerini Azure SQL Veritabanı'nda çalışacak şekilde daha fazla ayar yapacak ve genişletecek alanları bulmak için bir veritabanı için tüketilen kaynakları inceleyebilirsiniz.

### <a name="identifying-and-adding-missing-indexes"></a>Eksik dizinleri tanımlama ve ekleme

OLTP veritabanı performansında sık karşılaşılan bir sorun, fiziksel veritabanı tasarımıyla ilgilidir. Genellikle, veritabanı şemaları ölçekte (yük veya veri hacmi nde) sınmadan tasarlanır ve sevk edilir. Ne yazık ki, bir sorgu planının performansı küçük ölçekte kabul edilebilir, ancak üretim düzeyindeveri hacimleri altında önemli ölçüde bozulabilir. Bu sorunun en yaygın kaynağı, bir sorgudaki filtreleri veya diğer kısıtlamaları karşılamak için uygun dizinlerin olmamasıdır. Genellikle, bir dizin arama yeterli olabilir bir tablo tarar olarak eksik dizinler tezahür.

Bu örnekte, seçili sorgu planı, arama yeterli olduğunda bir taramayı kullanır:

```sql
DROP TABLE dbo.missingindex;
CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
DECLARE @a int = 0;
SET NOCOUNT ON;
BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;
```

![Eksik dizinleri olan bir sorgu planı](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL Veritabanı, sık karşılaşılan eksik dizin koşullarını bulmanıza ve düzeltmenize yardımcı olabilir. Azure SQL Veritabanı'nda yerleşik olan DMVs, bir dizinin sorguyu çalıştırmak için tahmini maliyeti önemli ölçüde azaltacağı sorgu derlemelerine bakar. Sorgu yürütme sırasında, SQL Veritabanı her sorgu planının ne sıklıkta yürütüldüğüni izler ve yürütülen sorgu planı ile bu dizinin bulunduğu hayal edilen plan arasındaki tahmini boşluğu izler. Fiziksel veritabanı tasarımınızda yapılan değişikliklerin bir veritabanının genel iş yükü maliyetini ve gerçek iş yükünü artırabileceğini hızlı bir şekilde tahmin etmek için bu DMV'leri kullanabilirsiniz.

Bu sorgu, olası eksik dizinleri değerlendirmek için kullanabilirsiniz:

```sql
SELECT
   CONVERT (varchar, getdate(), 126) AS runtime
   , mig.index_group_handle
   , mid.index_handle
   , CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
        (migs.user_seeks + migs.user_scans)) AS improvement_measure
   , 'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
        CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
        (' + ISNULL (mid.equality_columns,'')
        + CASE WHEN mid.equality_columns IS NOT NULL
        AND mid.inequality_columns IS NOT NULL
        THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '') + ')'
        + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement
   , migs.*
   , mid.database_id
   , mid.[object_id]
FROM sys.dm_db_missing_index_groups AS mig
   INNER JOIN sys.dm_db_missing_index_group_stats AS migs
      ON migs.group_handle = mig.index_group_handle
   INNER JOIN sys.dm_db_missing_index_details AS mid
      ON mig.index_handle = mid.index_handle
 ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC
```

Bu örnekte, sorgu bu öneriyle sonuçlandı:

```sql
CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  
```

Oluşturulduktan sonra, aynı SELECT deyimi, tarama yerine arama kullanan farklı bir plan seçer ve sonra planı daha verimli bir şekilde yürütür:

![Düzeltilmiş dizinleri olan bir sorgu planı](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Anahtar içgörü, paylaşılan, emtia sisteminin IO kapasitesinin özel bir sunucu makinesinden daha sınırlı olduğudur. Azure SQL Veritabanı hizmet katmanlarının her işlem boyutundaki DTU'daki sistemden maksimum şekilde yararlanmak için gereksiz IO'yu en aza indirmenin bir avantajı vardır. Uygun fiziksel veritabanı tasarım seçenekleri, tek tek sorguların gecikme süresini önemli ölçüde artırabilir, ölçek birimi başına işlenen eşzamanlı isteklerin işlenebilirliğini artırabilir ve sorguyu karşılamak için gereken maliyetleri en aza indirebilir. Eksik dizin DMVs hakkında daha fazla bilgi için, [bkz sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Sorgu aparatve ipucu

Azure SQL Veritabanı'ndaki sorgu optimize edicisi geleneksel SQL Server sorgu optimize edicisine benzer. Sorguları alabilmek ve sorgu optimize ediciiçin mantık modeli sınırlamalarını anlamak için en iyi uygulamaların çoğu Azure SQL Veritabanı için de geçerlidir. Azure SQL Veritabanı'ndaki sorguları ayarlarsanız, toplu kaynak taleplerini azaltmanın ek avantajını elde edebilirsiniz. Uygulamanız ayarlanmamış bir eşdeğerden daha düşük bir maliyetle çalışabilir, çünkü daha düşük bir bilgi işlem boyutunda çalışabilir.

SQL Server'da yaygın olan ve Azure SQL Veritabanı için de geçerli olan bir örnek, sorgu optimize edicinin parametreleri nasıl "kokladığı"dır. Derleme sırasında sorgu optimize edicisi, daha uygun bir sorgu planı oluşturup oluşturamayacağını belirlemek için bir parametrenin geçerli değerini değerlendirir. Bu strateji genellikle bilinen parametre değerleri olmadan derlenen bir plandan önemli ölçüde daha hızlı bir sorgu planına yol açsa da, şu anda hem SQL Server'da hem de Azure SQL Veritabanı'nda mükemmel şekilde çalışır. Bazen parametre koklanmaz ve bazen parametre koklanır, ancak oluşturulan plan iş yükündeki parametre değerlerinin tam kümesi için en uygun un altındadır. Microsoft, amacı daha kasıtlı olarak belirtebilmeniz ve parametre algılamanın varsayılan davranışını geçersiz ksayamanız için sorgu ipuçları (yönergeleri) içerir. Genellikle, ipuçları kullanırsanız, varsayılan SQL Server veya Azure SQL Veritabanı davranışının belirli bir müşteri iş yükü için kusurlu olduğu durumları düzeltebilirsiniz.

Sonraki örnek, sorgu işlemcisinin hem performans hem de kaynak gereksinimleri için en uygun un altında bir planı nasıl oluşturabileceğini gösterir. Bu örnek, bir sorgu ipucu kullanırsanız, SQL veritabanınız için sorgu çalıştırma süresini ve kaynak gereksinimlerini azaltabileceğinizi de gösterir:

```sql
DROP TABLE psptest1;
CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));
DECLARE @a int = 0;
SET NOCOUNT ON;
BEGIN TRANSACTION
   WHILE @a < 20000
   BEGIN
     INSERT INTO psptest1(col2) values (1);
     INSERT INTO psptest1(col2) values (@a);
     SET @a += 1;
   END
   COMMIT TRANSACTION
   CREATE INDEX i1 on psptest1(col2);
GO

CREATE PROCEDURE psp1 (@param1 int)
   AS
   BEGIN
      INSERT INTO t1 SELECT * FROM psptest1
      WHERE col2 = @param1
      ORDER BY col2;
    END
    GO

CREATE PROCEDURE psp2 (@param2 int)
   AS
   BEGIN
      INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
      ORDER BY col2
      OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
   END
   GO

CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
GO
```

Kurulum kodu, veri dağıtımını çarpıtmış bir tablo oluşturur. En iyi sorgu planı, hangi parametrenin seçildiğine bağlı olarak değişir. Ne yazık ki, plan önbelleğe alma davranışı her zaman en yaygın parametre değerine göre sorguyu yeniden derlemez. Bu nedenle, farklı bir plan ortalama olarak daha iyi bir plan seçimi olsa bile, en uygun altı bir planın önbelleğe alınması ve birçok değer için kullanılması mümkündür. Daha sonra sorgu planı, özel bir sorgu ipucu olması dışında, aynı olan iki depolanmış yordam oluşturur.

```sql
-- Prime Procedure Cache with scan plan
EXEC psp1 @param1=1;
TRUNCATE TABLE t1;

-- Iterate multiple times to show the performance difference
DECLARE @i int = 0;
WHILE @i < 1000
   BEGIN
      EXEC psp1 @param1=2;
      TRUNCATE TABLE t1;
      SET @i += 1;
    END
```

Sonuçların ortaya çıkan telemetri verilerinde farklı olması için, örneğin 2.

```sql
EXEC psp2 @param2=1;
TRUNCATE TABLE t1;

DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END
```

Bu örneğin her bölümü parametreli bir insert deyimini 1.000 kez çalıştırmayı dener (test veri kümesi olarak kullanmak için yeterli yük oluşturmak için). Depolanan yordamları çalıştırdığında, sorgu işlemcisi ilk derlemesi sırasında yordama geçirilen parametre değerini inceler (parametre "koklama"). İşlemci, parametre değeri farklı olsa bile, ortaya çıkan planı önbelleğe alınır ve sonraki çağırmalar için kullanır. En uygun plan her durumda kullanılmayabilir. Bazen, sorgunun ilk derlendiğinden itibaren belirli bir durum için daha iyi bir plan seçmek için optimize ediciye yol açmanız gerekir. Bu örnekte, ilk plan, parametreyle eşleşen her değeri bulmak için tüm satırları okuyan bir "tetkik" planı oluşturur:

![Bir tcan planı kullanarak tuning sorgusu](./media/sql-database-performance-guidance/query_tuning_1.png)

Yordamı 1 değerini kullanarak uyguladığımız için, ortaya çıkan plan 1 değeri için en uygun uydu, ancak tablodaki diğer tüm değerler için en uygunun altındaydı. Plan daha yavaş performans gösterdiğinden ve daha fazla kaynak kullandığından, her planı rasgele seçseniz, sonuç büyük olasılıkla isteyeceğiniz şey değildir.

Bu örnekte mantıksal `SET STATISTICS IO` tbmkif çalışması perde arkasında `ON`yapılır, "set" ile testi çalıştırın. Plan tarafından yapılan 1.148 okuma olduğunu görebilirsiniz (ortalama durum tek bir satır döndürmek için ise, verimsiz):

![Mantıksal bir tarak kullanarak alamı sorgulama](./media/sql-database-performance-guidance/query_tuning_2.png)

Örneğin ikinci bölümü derleme işlemi sırasında belirli bir değer kullanmak için en iyi duruma getirici söylemek için bir sorgu ipucu kullanır. Bu durumda, sorgu işlemcisini parametre olarak geçirilen değeri yok saymaya `UNKNOWN`ve bunun yerine . Bu, tabloda ortalama frekansa sahip bir değeri ifade eder (eğriltme). Ortaya çıkan plan, bu örneğin bölüm 1'deki plandan ortalama olarak daha hızlı olan ve ortalama olarak daha az kaynak kullanan arama tabanlı bir plandır:

![Sorgu ipucu kullanarak sorgu ala](./media/sql-database-performance-guidance/query_tuning_3.png)

**Sys.resource_stats** tablosundaki efekti görebilirsiniz (testi yürüttüğün tarihten ve verilerin tabloyu doldurmasından itibaren bir gecikme olur). Bu örnekte, bölüm 1 22:25:00 zaman penceresinde yürütülür ve bölüm 2 22:35:00'de çalıştırılan. Önceki zaman penceresi, bu zaman penceresinde daha sonraki (plan verimliliği iyileştirmeleri nedeniyle) daha fazla kaynak kullanılır.

```sql
SELECT TOP 1000 *
FROM sys.resource_stats
WHERE database_name = 'resource1'
ORDER BY start_time DESC
```

![Sorgu alamı örnek sonuçları](./media/sql-database-performance-guidance/query_tuning_4.png)

> [!NOTE]
> Bu örnekteki birim kasıtlı olarak küçük olsa da, özellikle daha büyük veritabanları üzerinde alt optimal parametrelerin etkisi önemli olabilir. Fark, aşırı durumlarda, hızlı durumlarda saniye ve yavaş durumlarda için saat arasında olabilir.

Bir test için kaynağın başka bir testten daha fazla veya daha az kaynak kullanıp kullanmadığını belirlemek için **sys.resource_stats'ı** inceleyebilirsiniz. Verileri karşılaştırdığınızda, **sys.resource_stats** görünümünde aynı 5 dakikalık pencerede olmayacak şekilde testlerin zamanlamasını ayırın. Alıştırmanın amacı, kullanılan toplam kaynak miktarını en aza indirmek ve en yüksek kaynakları en aza indirmek değildir. Genellikle, gecikme için bir kod parçasını optimize etmek de kaynak tüketimini azaltır. Uygulamada yaptığınız değişikliklerin gerekli olduğundan ve değişikliklerin uygulamada sorgu ipuçlarını kullanan bir kişi için müşteri deneyimini olumsuz etkilemediğinden emin olun.

İş yükünde yinelenen sorgular kümesi varsa, veritabanını barındırmak için gereken minimum kaynak boyutu birimini kullandığından plan seçimlerinizin en uygun olup olmadığını yakalamak ve doğrulamak genellikle mantıklıdır. Bunu doğruladıktan sonra, zaman zaman bozulmadığından emin olmak için planları yeniden gözden geçirin. [Sorgu ipuçları (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx)hakkında daha fazla bilgi edinebilirsiniz.

### <a name="very-large-database-architectures"></a>Çok büyük veritabanı mimarileri

Azure SQL Veritabanı'ndaki tek veritabanları için [Hyperscale](sql-database-service-tier-hyperscale.md) hizmet katmanı yayımlanmadan önce, müşteriler tek tek veritabanları için kapasite sınırlarına çarpar. Bu kapasite sınırları, yönetilen örneklerde elastik havuzlarda ve örnek veritabanında birleştirilmiş veritabanları için hala vardır. Aşağıdaki iki bölümde, Hyperscale hizmet katmanını kullanamadığınızda Azure SQL Veritabanı'nda çok büyük veritabanlarıyla ilgili sorunları çözmek için iki seçenek tartışılır.

### <a name="cross-database-sharding"></a>Çapraz veritabanı parçalama

Azure SQL Veritabanı emtia donanımıyla çalıştığından, tek bir veritabanının kapasite sınırları geleneksel şirket içi SQL Server yüklemesinden daha düşüktür. Bazı müşteriler, işlemler Azure SQL Veritabanı'ndaki tek bir veritabanının sınırlarına sığmazken veritabanı işlemlerini birden çok veritabanına yaymak için parçalama tekniklerini kullanır. Azure SQL Veritabanı'nda parçalama teknikleri kullanan çoğu müşteri, verilerini tek bir boyutta birden çok veritabanına böler. Bu yaklaşım için, OLTP uygulamalarının genellikle yalnızca bir satıra veya şemadaki küçük bir satır grubuna uygulanan işlemleri gerçekleştirdiğini anlamanız gerekir.

> [!NOTE]
> SQL Veritabanı artık parçalama ile yardımcı olmak için bir kitaplık sağlar. Daha fazla bilgi için [Bkz. Elastik Veritabanı istemci kitaplığı genel bakışı.](sql-database-elastic-database-client-library.md)

Örneğin, bir veritabanında müşteri adı, sipariş ve sipariş ayrıntıları varsa (SQL Server ile birlikte giden geleneksel örnek Northwind veritabanı gibi), bir müşteriyi ilgili sipariş ve sipariş ayrıntılarıyla gruplandırmak için bu verileri birden çok veritabanına bölebilirsiniz Bilgi. Müşterinin verilerinin tek bir veritabanında kalacağını garanti edebilirsiniz. Uygulama, farklı müşterileri veritabanları arasında bölerek yükü birden çok veritabanına etkili bir şekilde yayılacak. Bu parçalama ile müşteriler yalnızca maksimum veritabanı boyutu sınırını önlemekle kalmamış, aynı zamanda Azure SQL Veritabanı, her bir veritabanı DTU'ya uyduğu sürece farklı işlem boyutlarının sınırlarından önemli ölçüde daha büyük iş yüklerini de işleyebilir.

Veritabanı parçalama bir çözüm için toplam kaynak kapasitesini azaltmasa da, birden çok veritabanına yayılan çok büyük çözümleri desteklemede son derece etkilidir. Her veritabanı, yüksek kaynak gereksinimlerine sahip çok büyük, "etkili" veritabanlarını desteklemek için farklı bir bilgi işlem boyutunda çalıştırılabilir.

#### <a name="functional-partitioning"></a>İşlevsel bölümleme

SQL Server kullanıcıları genellikle tek bir veritabanında birçok işlevi birleştirir. Örneğin, bir uygulamanın bir mağazanın envanterini yönetme mantığı varsa, bu veritabanının stokla ilişkili mantığı, satınalma siparişlerini izleme, depolanan yordamlar ve ay sonu raporlamasını yöneten dizine veya maddeleştirilmiş görünümlere sahip olabilir. Bu teknik, yedekleme gibi işlemler için veritabanının yönetilmesini kolaylaştırır, ancak aynı zamanda bir uygulamanın tüm işlevleri arasında en yüksek yükü işlemek için donanımı boyutlandırmanızı da gerektirir.

Azure SQL Veritabanı'nda ölçeklendirilen bir mimari kullanıyorsanız, bir uygulamanın farklı işlevlerini farklı veritabanlarına bölmek iyi bir fikirdir. Bu tekniği kullanarak, her uygulama bağımsız olarak ölçeklendirilir. Bir uygulama daha işlek hale geldikçe (ve veritabanındaki yük arttıkça), yönetici uygulamadaki her işlev için bağımsız bilgi işlem boyutları seçebilir. Bu mimaride, yük birden çok makineye yayıldığı için, bir uygulama tek bir emtia makinesinin işleyebilir daha büyük olabilir.

### <a name="batch-queries"></a>Toplu sorgular

Yüksek hacimli, sık, geçici sorgu kullanarak verilere erişen uygulamalar için, uygulama katmanı ile Azure SQL Veritabanı katmanı arasındaki ağ iletişimine önemli miktarda yanıt süresi harcanıyor. Hem uygulama hem de Azure SQL Veritabanı aynı veri merkezinde olsa bile, ikisi arasındaki ağ gecikmesi çok sayıda veri erişim işlemiyle büyütülebilir. Veri erişim işlemleri için ağ tur larını azaltmak için, geçici sorguları toplu olarak toplama veya bunları depolanmış yordamlar olarak derleme seçeneğini kullanmayı düşünün. Geçici sorguları toplu olarak toplarsanız, Azure SQL Veritabanı'na tek bir yolculukta birden çok sorguyu tek bir büyük toplu işlem olarak gönderebilirsiniz. Depolanan bir yordamda geçici sorgular derlerseniz, toplu işlemle aynı sonucu elde edebilirsiniz. Depolanmış yordamı kullanmak, depolanan yordamı yeniden kullanabilmeniz için Azure SQL Veritabanı'ndaki sorgu planlarını önbelleğe alma şansını artırma nızı da sağlar.

Bazı uygulamalar yazma yoğun. Bazen toplu olarak nasıl birlikte yazılanın gerektiğini göz önünde bulundurarak veritabanındaki toplam IO yükünü azaltabilirsiniz. Genellikle bu, depolanan yordamlarda ve geçici toplu işlerle otomatik işleme işlemleri yerine açık hareketleri kullanmak kadar basittir. Kullanabileceğiniz farklı tekniklerin değerlendirilmesi [için Azure'daki SQL Veritabanı uygulamaları için Toplu İşlem teknikleribölümüne](sql-database-use-batching-to-improve-performance.md)bakın. Toplu iş için doğru modeli bulmak için kendi iş yükünüzü deneyin. Bir modelin biraz farklı işlem tutarlılık garantileri olabileceğini anladığınızdan emin olun. Kaynak kullanımını en aza indiren doğru iş yükünü bulmak, doğru tutarlılık ve performans dengelerinin doğru birleşimini bulmayı gerektirir.

### <a name="application-tier-caching"></a>Uygulama katmanı önbelleğe alma

Bazı veritabanı uygulamaları okuma ağırlıklı iş yükleri vardır. Önbelleğe alma katmanları veritabanındaki yükü azaltabilir ve Azure SQL Veritabanı'nı kullanarak bir veritabanını desteklemek için gereken işlem boyutunu azaltabilir. [Redis için Azure Önbelleği](https://azure.microsoft.com/services/cache/)ile, okuma ağırlıklı bir iş yükünüz varsa, verileri bir kez okuyabilir (veya belki de nasıl yapılandırıldığına bağlı olarak uygulama katmanı makine başına bir kez) okuyabilir ve bu verileri SQL veritabanınızın dışında depolayabilirsiniz. Bu, veritabanı yükünü azaltmanın (CPU ve IO'yu okuma) bir yoludur, ancak önbellekten okunan veriler veritabanındaki verilerle eşitlenmemiş olabileceğinden işlem tutarlılığı üzerinde bir etkisi vardır. Birçok uygulamada bazı tutarsızlık düzeyi kabul edilebilir olsa da, bu tüm iş yükleri için geçerli değildir. Uygulama katmanı önbelleğe alma stratejisini uygulamadan önce tüm uygulama gereksinimlerini tam olarak anlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- DTU tabanlı hizmet katmanları hakkında daha fazla bilgi için [DTU tabanlı satın alma modeline](sql-database-service-tiers-dtu.md)bakın.
- vCore tabanlı hizmet katmanları hakkında daha fazla bilgi için [vCore tabanlı satın alma modeline](sql-database-service-tiers-vcore.md)bakın.
- Esnek havuzlar hakkında daha fazla bilgi için Azure [elastik havuzu nedir?](sql-database-elastic-pool.md)
- Performans ve elastik havuzlar hakkında bilgi için, [elastik havuzu ne zaman göz önünde bulundurabilirsiniz](sql-database-elastic-pool-guidance.md)
