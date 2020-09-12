---
title: Uygulamalar ve veritabanları için performans ayarlama Kılavuzu
titleSuffix: Azure SQL Database & Azure SQL Managed Instance
description: Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde performans için veritabanı uygulamalarını ve veritabanlarını ayarlama hakkında bilgi edinin.
services: sql-database
ms.service: sql-db-mi
ms.subservice: performance
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: jrasnick
ms.date: 03/10/2020
ms.openlocfilehash: 54a6293a29a407a7014aafb66587dcb01fc13337
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89645797"
---
# <a name="tune-applications-and-databases-for-performance-in-azure-sql-database-and-azure-sql-managed-instance"></a>Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde performans için uygulamaları ve veritabanlarını ayarlama
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL veritabanı ve Azure SQL yönetilen örneği ile ilgili bir performans sorunu tanımladıktan sonra, bu makale size yardımcı olacak şekilde tasarlanmıştır:

- Uygulamanızı ayarlayın ve performansı iyileştirebilecek bazı en iyi yöntemleri uygulayın.
- Dizinleri ve sorguları verilerle daha verimli çalışacak şekilde değiştirerek veritabanını ayarlayın.

Bu makalede, Azure SQL veritabanı [veritabanı Danışmanı önerileri](database-advisor-implement-performance-recommendations.md) ve varsa Azure SQL veritabanı [otomatik ayarlama önerileri](automatic-tuning-overview.md)aracılığıyla zaten çalıştık varsayılmaktadır. Ayrıca, [izleme ve ayarlamaya Ilişkin bir genel bakış](monitor-tune-overview.md) ve performans sorunlarını giderme ile ilgili makalelere göz atın. Ayrıca, bu makalede, veritabanınıza daha fazla kaynak sağlamak üzere işlem boyutu veya hizmet katmanını artırarak çözülebildiğiniz bir CPU kaynaklarınız, çalıştırmaya ilişkin bir performans sorunu olmadığı varsayılır.

## <a name="tune-your-application"></a>Uygulamanızı ayarlama

Geleneksel şirket içi SQL Server, ilk kapasite planlaması süreci genellikle üretimde bir uygulama çalıştırma süreciyle ayrılır. İlk olarak donanım ve ürün lisansları satın alınır ve performans ayarlaması daha sonra yapılır. Azure SQL kullandığınızda, bir uygulamayı çalıştırma ve ayarlama sürecini interweave iyi bir fikir olabilir. İsteğe bağlı kapasite için ödeme modeliyle, uygulamanızı, genellikle yanlış olan bir uygulama için gelecekteki büyüme planlarının tahminlerini temel alarak donanım üzerinde sağlama yerine, şimdi gereken minimum kaynakları kullanacak şekilde ayarlayabilirsiniz. Bazı müşteriler bir uygulamayı ayarlamayı tercih edebilir, bunun yerine donanım kaynakları sağlamayı tercih edebilir. Bu yaklaşım, yoğun bir dönemde anahtar uygulamayı değiştirmek istemediğiniz durumlarda iyi bir fikir olabilir. Ancak, bir uygulamanın ayarlanması, Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde hizmet katmanlarını kullandığınızda kaynak gereksinimlerini en aza indirebilir ve aylık faturaları azaltır.

### <a name="application-characteristics"></a>Uygulama özellikleri

Azure SQL veritabanı ve Azure SQL yönetilen örnek hizmet katmanları bir uygulama için performans kararlılığını ve öngörülebilirlik sağlamak üzere tasarlansa da, bazı en iyi yöntemler, bir işlem boyutundaki kaynaklardan daha iyi faydalanmak için uygulamanızı ayarlamanıza yardımcı olabilir. Birçok uygulamanın daha yüksek bir işlem boyutuna veya hizmet katmanına geçerek önemli performans kazançları olsa da, bazı uygulamalara daha yüksek bir hizmet düzeyinden yararlanmak için ek ayarlamalar yapmanız gerekir. Daha yüksek performans için, bu özelliklere sahip uygulamalar için ek uygulama ayarlamayı düşünün:

- **"Geveze" davranışı nedeniyle performansının yavaşlamasına neden olan uygulamalar**

  Geveze uygulamaları, ağ gecikmesinden duyarlı aşırı veri erişimi işlemleri yapar. Veritabanına veri erişim işlemlerinin sayısını azaltmak için bu tür uygulamaları değiştirmeniz gerekebilir. Örneğin, geçici sorguları toplu olarak oluşturma veya sorguları saklı yordamlara taşıma gibi teknikleri kullanarak uygulama performansını artırabilirsiniz. Daha fazla bilgi için bkz. [Batch sorguları](#batch-queries).

- **Tek bir makine tarafından desteklenmeyen yoğun iş yüküne sahip veritabanları**

   En yüksek Premium işlem boyutu kaynaklarını aşan veritabanları iş yükünü ölçeklendirmeden faydalanabilir. Daha fazla bilgi için bkz. [veritabanları arası](#cross-database-sharding) parçalar ve [işlevsel bölümlendirme](#functional-partitioning).

- **En iyi alt sorguları olan uygulamalar**

  Özellikle, veri erişim katmanındaki, kötü ayarlı sorgulara sahip olan uygulamalar daha yüksek bir işlem boyutundan faydalanmayabilir. Bu, WHERE yan tümcesinin olmadığı, dizinleri eksik olan veya güncel olmayan istatistiklerdeki sorguları içerir. Bu uygulamalar standart sorgu performansı ayarlama tekniklerinden yararlanır. Daha fazla bilgi için bkz. [eksik dizinler](#identifying-and-adding-missing-indexes) ve [sorgu ayarlama ve ipucu](#query-tuning-and-hinting).

- **En uygun alt veri erişimi tasarımına sahip uygulamalar**

   Devralınan veri erişimi eşzamanlılık sorunlarına sahip uygulamalar, örneğin, kilitlenmeme, daha yüksek bir işlem boyutundan faydalanabilir. İstemci tarafındaki verileri Azure önbellek hizmeti ya da başka bir önbelleğe alma teknolojisiyle önbelleğe alarak veritabanına karşı gidiş dönüş işlemlerini azaltmayı göz önünde bulundurun. Bkz. [uygulama katmanı önbelleğe alma](#application-tier-caching).

## <a name="tune-your-database"></a>Veritabanınızı ayarlama

Bu bölümde, uygulamanız için en iyi performansı elde etmek üzere veritabanını ayarlamak ve mümkün olan en düşük işlem boyutunda çalıştırmak için kullanabileceğiniz bazı tekniklere bakacağız. Bu tekniklerin bazıları geleneksel SQL Server ayarlama en iyi uygulamaları ile eşleşir, ancak diğerleri Azure SQL veritabanı ve Azure SQL yönetilen örneği 'ne özgüdür. Bazı durumlarda, Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde çalışmak üzere geleneksel SQL Server tekniklerini daha fazla ayarlamak ve genişletmek için bir veritabanının tüketilen kaynaklarını inceleyebilirsiniz.

### <a name="identifying-and-adding-missing-indexes"></a>Eksik dizinleri tanımlama ve ekleme

OLTP veritabanı performansında yaygın bir sorun, fiziksel veritabanı tasarımıyla ilgilidir. Genellikle veritabanı şemaları, ölçeğe göre test etmeden tasarlanır ve gönderilir (yükleme ya da veri biriminde). Ne yazık ki, bir sorgu planının performansı küçük ölçekte kabul edilebilir, ancak üretim düzeyi veri birimlerinde önemli ölçüde düşebilir. Bu sorunun en yaygın kaynağı, bir sorgudaki filtreleri veya diğer kısıtlamaları karşılamak için uygun dizinlerin olmamasıdır. Genellikle, Dizin arama yeterli olduğunda tablo tarama olarak dizinler bildirimleri eksik olur.

Bu örnekte, seçili sorgu planı bir arama yeterli olduğunda bir tarama kullanır:

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

![Dizinleri eksik olan bir sorgu planı](./media/performance-guidance/query_plan_missing_indexes.png)

Azure SQL veritabanı ve Azure SQL yönetilen örneği, yaygın eksik dizin koşullarını bulup düzeltmenize yardımcı olabilir. Azure SQL veritabanı 'nda yerleşik olarak bulunan DMV ve Azure SQL yönetilen örneği, bir dizinin bir sorgu çalıştırmak için tahmin edilen maliyeti önemli ölçüde azaltacağından sorgu derlemelerinin göz atalım. Sorgu yürütme sırasında, veritabanı altyapısı her bir sorgu planının ne sıklıkta yürütüleceğini izler ve yürütülen sorgu planı arasındaki tahmini boşluğu ve dizinin bulunduğu yerde bir tane olduğunu izler. Bu DMVs 'yi, fiziksel veritabanı tasarımınızda hangi değişikliklerin bir veritabanının ve gerçek iş yükünün genel iş yükü maliyetini iyileştirebileceğini hızlı bir şekilde tahmin etmek için kullanabilirsiniz.

Bu sorguyu, olası eksik dizinleri değerlendirmek için kullanabilirsiniz:

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

Bu örnekte, sorgu şu öneriye neden oldu:

```sql
CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  
```

Oluşturulduktan sonra, aynı SELECT açıklaması tarama yerine bir arama kullanan farklı bir plan seçer ve daha sonra planı daha verimli bir şekilde yürütür:

![Düzeltilen dizinler içeren bir sorgu planı](./media/performance-guidance/query_plan_corrected_indexes.png)

Temel Öngörüler, paylaşılan bir emtia sisteminin GÇ kapasitesinin, ayrılmış bir sunucu makinesinden daha fazla sınırlı olması. Hizmet katmanlarının her işlem boyutu için kaynakların en büyük avantajlarından yararlanmak üzere gereksiz GÇ 'yi en aza indirmeden bir Premium vardır. Uygun fiziksel veritabanı tasarım seçimleri, bireysel sorguların gecikmesini önemli ölçüde iyileştirebilir, ölçek birimi başına işlenen eşzamanlı isteklerin verimini artırır ve sorguyu karşılamak için gereken maliyetleri en aza indirir. Eksik dizin DMVs hakkında daha fazla bilgi için bkz. [sys. dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Sorgu ayarlama ve ipuçcu

Azure SQL veritabanı ve Azure SQL yönetilen örneği 'ndeki sorgu iyileştiricisi geleneksel SQL Server sorgu iyileştiriciye benzerdir. Sorgu iyileştiricilerini ayarlama ve sorgu iyileştiricilerini anlamak için en iyi yöntemler, Azure SQL veritabanı ve Azure SQL yönetilen örneği için de geçerlidir. Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde sorguları ayarlarsanız, toplu kaynak taleplerini azaltmanın ek avantajını elde edebilirsiniz. Uygulamanız, daha düşük bir işlem boyutunda çalıştırılabildiğinden, daha düşük bir maliyetle çalıştırılabilir.

SQL Server ' de yaygın olan ve Azure SQL veritabanı için de geçerli olan ve Azure SQL yönetilen örneği, sorgu iyileştiricinin "en iyi duruma getirme" parametrelerinden oluşan bir örnektir. Derleme sırasında sorgu iyileştiricisi, daha iyi bir sorgu planı oluşturulup oluşturulmayacağını anlamak için bir parametrenin geçerli değerini değerlendirir. Bu strateji genellikle bilinen parametre değerleri olmadan derlenen bir plandan önemli ölçüde daha hızlı bir sorgu planına yol açabilse de şu anda SQL Server, Azure SQL veritabanı ve Azure SQL yönetilen örneği ile sorunsuz bir şekilde çalışır. Bazen parametre bir ön plana uygulanmaz ve bazen parametre bir yük dengelenir ancak oluşturulan plan, bir iş yükünde bulunan parametre değerlerinin tam kümesi için en uygun alt düzeyde olur. Microsoft, sorgu ipuçları (yönergeler) içerir, böylece amacı daha bilinçli olarak belirtebilir ve parametre algılaması varsayılan davranışını geçersiz kılabilirsiniz. Genellikle ipuçları kullanıyorsanız, varsayılan SQL Server, Azure SQL veritabanı ve Azure SQL yönetilen örnek davranışının belirli bir müşteri iş yükü için de eksik olduğu durumları çözebilirsiniz.

Sonraki örnekte, sorgu işlemcisinin performans ve kaynak gereksinimleri için her ikisi de en uygun olan bir planı nasıl oluşturabileceği gösterilmektedir. Bu örnek ayrıca bir sorgu ipucu kullanırsanız, veritabanınız için sorgu çalışma süresini ve kaynak gereksinimlerini azaltabilirsiniz.

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

Kurulum kodu, asimetrik veri dağıtımına sahip bir tablo oluşturur. En iyi sorgu planı, hangi parametrenin seçildiğinden farklıdır. Ne yazık ki, plan önbelleği davranışı en sık kullanılan parametre değerine göre sorguyu her zaman yeniden deretmez. Bu nedenle, bir alt en iyi planın önbelleğe alınması ve çok sayıda değer için kullanılması, farklı bir plan ortalama üzerinde daha iyi bir plan seçeneği olabileceğinden bile mümkündür. Ardından sorgu planı aynı olan iki saklı yordam oluşturur, bunun dışında özel bir sorgu ipucu vardır.

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

Sonucun 2. bölümüne başlamadan önce en az 10 dakika beklemeniz önerilir, böylece sonuçlar elde edilen telemetri verilerinde ayrı olur.

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

Bu örneğin her bir bölümü parametreli INSERT ifadesini 1.000 kez çalıştırmayı dener (test veri kümesi olarak kullanmak için yeterli yük oluşturmak için). Saklı yordamları yürüttüğünde sorgu işlemcisi, ilk derlemesi sırasında yordama geçirilen parametre değerini inceler (parametre "algılaması"). İşlemci sonuç planını önbelleğe alır ve parametre değeri farklı olsa bile daha sonra çağırmaları için kullanır. En iyi plan, her durumda kullanılmamış olabilir. Bazen, sorgunun ilk derlenmesi sırasında belirli bir durum yerine ortalama durum için daha iyi bir plan seçmek üzere iyileştiriciye kılavuzluk etmeniz gerekir. Bu örnekte, ilk plan, parametresiyle eşleşen her değeri bulmak için tüm satırları okuyan bir "tarama" planı oluşturur:

![Tarama planı kullanarak sorgu ayarlama](./media/performance-guidance/query_tuning_1.png)

Yordamı 1 değerini kullanarak yürütüyoruz, sonuçta elde edilen plan 1 değeri için idealdir, ancak tablodaki diğer tüm değerler için en uygun alt değerdir. Sonuç büyük olasılıkla, her bir planı daha yavaş gerçekleştirdiğinden ve daha fazla kaynak kullandığından, her planı rastgele olarak seçmeniz durumunda istediğiniz şeydir.

Testini `SET STATISTICS IO` olarak ayarla `ON` ' yı çalıştırırsanız, bu örnekteki mantıksal tarama işi arka planda yapılır. Plan tarafından gerçekleştirilen 1.148 okuma olduğunu (ortalama durum yalnızca bir satır döndürmek ise verimsiz olduğunu) görebilirsiniz:

![Mantıksal tarama kullanarak sorgu ayarlama](./media/performance-guidance/query_tuning_2.png)

Örneğin ikinci bölümü, derleme işlemi sırasında iyileştiricinin belirli bir değeri kullanmasını söylemek için bir sorgu ipucu kullanır. Bu durumda, sorgu işlemcisini parametre olarak geçirilen değeri yok saymaya zorlar ve bunun yerine varsayabilirsiniz `UNKNOWN` . Bu, tabloda ortalama sıklık değeri olan bir değere başvurur (eğriliği yok sayar). Elde edilen plan, bu örnekte daha hızlı olan ve ortalama olarak daha az kaynak kullanan bir arama tabanlı plandır:

![Sorgu ipucu kullanarak sorgu ayarlama](./media/performance-guidance/query_tuning_3.png)

Bu etkiyi **sys. resource_stats** tablosunda görebilirsiniz (testi yürüttüğünüzde ve veriler tabloyu doldurışınızda bir gecikme vardır). Bu örnekte, 1. Bölüm 22:25:00 zaman penceresi sırasında yürütülür ve Bölüm 2 22:35:00 ' de yürütülür. Önceki zaman penceresi, daha sonraki bir zaman penceresinde daha fazla kaynak kullandı (plan verimlilik geliştirmeleri nedeniyle).

```sql
SELECT TOP 1000 *
FROM sys.resource_stats
WHERE database_name = 'resource1'
ORDER BY start_time DESC
```

![Sorgu ayarlama örnek sonuçları](./media/performance-guidance/query_tuning_4.png)

> [!NOTE]
> Bu örnekteki birim bilinçli olarak küçük olsa da, alt optimum parametrelerin etkisi özellikle daha büyük veritabanlarında çok önemli olabilir. Büyük durumlarda farklılık, yavaş durumlar için hızlı durumlar ve saatler için saniyeler arasında olabilir.

Bir testin kaynağının başka bir testten daha fazla veya daha az kaynak kullanıp kullanmadığını anlamak için, **sys. resource_stats** ' i inceleyebilirsiniz. Verileri karşılaştırdığınızda, test zamanlamasını, **sys. resource_stats** görünümündeki 5 dakikalık bir pencerede kalmayacak şekilde ayırın. Alıştırma hedefi, en yoğun kaynakları en aza indirmek için kullanılan toplam kaynak miktarını en aza indirmektir. Genellikle, gecikme için kod parçasını iyileştirmek kaynak tüketimini de azaltır. Bir uygulamada yaptığınız değişikliklerin gerekli olduğundan ve değişikliklerin uygulamada sorgu ipuçları kullanıyor olabilecek bir kişiye ait müşteri deneyimini olumsuz şekilde etkilemediğinden emin olun.

Bir iş yükünün yinelenen sorgular kümesi varsa, genellikle, veritabanını barındırmak için gereken en düşük kaynak boyutu birimini barındıracağından plan seçimlerinizin optimizasyonu ve doğrulanması mantıklı olur. Bunu doğruladıktan sonra, bunların düşürülmediğinden emin olmanıza yardımcı olacak planları bazen yeniden inceleyin. [Sorgu ipuçları (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx)hakkında daha fazla bilgi edinebilirsiniz.

### <a name="very-large-database-architectures"></a>Çok büyük veritabanı mimarileri

Azure SQL veritabanı 'ndaki tek veritabanlarına ait [hiper ölçek](service-tier-hyperscale.md) hizmeti katmanının yayımlanmasından önce, müşteriler ayrı veritabanları için kapasite sınırlarına isabet etmek üzere kullanılır. Azure SQL veritabanı elastik havuzlarında ve Azure SQL yönetilen örneklerinde örnek veritabanlarında, havuza alınmış veritabanları için bu kapasite sınırları hala mevcut. Aşağıdaki iki bölümde, hiper ölçek hizmet katmanını kullanıdığınızda, Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde çok büyük veritabanlarıyla ilgili sorunları çözmek için iki seçenek ele alınmaktadır.

### <a name="cross-database-sharding"></a>Veritabanları arası parçalama

Azure SQL veritabanı ve Azure SQL yönetilen örneği, emtia donanımında çalıştığından, tek bir veritabanı için kapasite sınırları geleneksel bir şirket içi SQL Server yüklemesine göre daha düşüktür. Bazı müşteriler, Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde tek bir veritabanının sınırlarına uygun olmayan işlemler, birden çok veritabanı üzerinde veritabanı işlemlerini yaymak için parçalı teknikler kullanır. Azure SQL veritabanı ve Azure SQL yönetilen örneği üzerinde parçalama tekniklerini kullanan çoğu müşteri, verilerini birden çok veritabanı arasında tek bir boyutta böler. Bu yaklaşım için, OLTP uygulamalarının genelde yalnızca bir satıra veya şemadaki küçük bir satır grubuna uygulanan işlemler yaptığını anlamanız gerekir.

> [!NOTE]
> Azure SQL veritabanı artık, parçalı oluşturmaya yardımcı olacak bir kitaplık sağlıyor. Daha fazla bilgi için bkz. [elastik veritabanı istemci kitaplığına genel bakış](elastic-database-client-library.md).

Örneğin, bir veritabanında müşteri adı, sipariş ve sipariş ayrıntıları varsa (SQL Server ile birlikte gelen geleneksel örnek Northwind veritabanı gibi), bir müşteriyi ilgili sipariş ve sipariş ayrıntısı bilgileriyle gruplayarak, bu verileri birden çok veritabanına bölebilirsiniz. Müşterinin verilerinin tek bir veritabanında kalmasını garanti edebilirsiniz. Uygulama, farklı müşterileri veritabanları genelinde bölerek yük yükünü etkin bir şekilde birden çok veritabanı arasında bölüşyordu. Parçalama sayesinde, müşteriler yalnızca en büyük veritabanı boyutu sınırını önlemez, ancak Azure SQL veritabanı ve Azure SQL yönetilen örneği aynı zamanda farklı işlem boyutlarının limitlerinden önemli ölçüde daha büyük olan iş yüklerini işleyebilir, çünkü her bir veritabanı hizmet katmanı sınırlarına uygun olur.

Veritabanı parçaları bir çözüm için toplam kaynak kapasitesini azaltmasa da, birden fazla veritabanına yayılan çok büyük çözümlerin desteklenmesi oldukça etkilidir. Her veritabanı, yüksek kaynak gereksinimleriyle çok büyük, "etkili" veritabanlarını desteklemek için farklı bir işlem boyutuyla çalıştırılabilir.

#### <a name="functional-partitioning"></a>İşlevsel bölümleme

Kullanıcılar genellikle tek bir veritabanında birçok işlevi birleştirir. Örneğin, bir uygulamanın bir mağaza için envanteri yönetme mantığı varsa, bu veritabanı envanterle ilişkili mantığa sahip olabilir, satın alma siparişleri, saklı yordamlar ve aylık son raporlamayı yöneten dizinli veya gerçekleştirilmiş görünümler olabilir. Bu teknik, yedekleme gibi işlemler için veritabanını yönetmeyi kolaylaştırır, ancak aynı zamanda bir uygulamanın tüm işlevlerinde en yüksek yükü işlemek için donanımı boyutlandırmanıza da gerek duyar.

Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde bir genişleme mimarisi kullanıyorsanız, bir uygulamanın farklı işlevlerini farklı veritabanlarına bölmek iyi bir fikirdir. Bu tekniği kullanarak her bir uygulama bağımsız olarak ölçeklendirilir. Bir uygulama, busier olur (ve veritabanındaki Yük arttıkça), yönetici uygulamadaki her bir işlev için bağımsız işlem boyutları seçebilir. Bu mimaride, bu mimariye sahip bir uygulama, yük birden çok makineye yayıldığı için tek bir emtia makinesinden daha büyük olabilir.

### <a name="batch-queries"></a>Toplu iş sorguları

Yüksek hacimli, sık sık, geçici sorgulama kullanarak verilere erişen uygulamalar için, uygulama katmanı ve veritabanı katmanı arasındaki ağ iletişiminde önemli miktarda yanıt süresi harcanmıştır. Hem uygulama hem de veritabanı aynı veri merkezinde olsa bile, ikisi arasındaki ağ gecikmesi çok sayıda veri erişim işlemi ile büyütülebilir. Veri erişimi işlemlerine yönelik ağ gidiş dönüşlerini azaltmak için, geçici sorguları toplu olarak toplu olarak veya saklı yordamlar olarak derlemek için seçeneğini kullanmayı düşünün. Geçici sorguları toplu olarak oluşturursanız, veritabanına tek bir seyahat halinde birden çok sorgu gönderebilirsiniz. Saklı yordamda geçici sorgular derlerseniz, bu sonuçları toplu olarak elde edebilirsiniz. Saklı yordam kullanmak ayrıca, saklı yordamı yeniden kullanabilmeniz için veritabanında sorgu planlarını önbelleğe alma olasılığını artırmanın avantajını de sağlar.

Bazı uygulamalar yazma yoğunluğu vardır. Bazen bir veritabanındaki toplam GÇ yükünü, toplu olarak nasıl bir araya yazabileceğini göz önünde bulundurarak azaltabilirsiniz. Genellikle bu, saklı yordamlarda ve geçici toplu işlemlerde işlemleri otomatik olarak yürütmek yerine açık işlemler kullanmanın en kolay bir işlemdir. Kullanabileceğiniz farklı tekniklerin değerlendirmesi için bkz. [Azure 'da veritabanı uygulamaları Için toplu işlem teknikleri](../performance-improve-use-batching.md). Toplu işleme yönelik doğru modeli bulmak için kendi iş yükünüzü deneyin. Bir modelin biraz farklı işlem tutarlılığı garantisi olabileceğini anladığınızdan emin olun. Kaynak kullanımını en aza indiren doğru iş yükünü bulmak için tutarlılık ve performans dengelerinin doğru birleşimini bulmanız gerekir.

### <a name="application-tier-caching"></a>Uygulama katmanı önbelleğe alma

Bazı veritabanı uygulamalarında okuma ağır iş yükleri vardır. Önbelleğe alma katmanları, veritabanının yükünü azaltabilir ve Azure SQL veritabanı ve Azure SQL yönetilen örneği kullanarak bir veritabanını desteklemek için gereken işlem boyutunu azaltabilir. [Redin Için Azure Cache](https://azure.microsoft.com/services/cache/)ile, okuma ağır bir iş yükünüz varsa, verileri bir kez (veya belki de her bir uygulama katmanı makinesi için bir kez) okuyabilir ve daha sonra bu verileri veritabanınızın dışına saklayabilirsiniz. Bu, veritabanı yükünü (CPU ve Okuma GÇ) azaltmanın bir yoludur, ancak önbellekten okunan veriler veritabanındaki verilerle eşitlenmemiş olabileceğinden işlem tutarlılığı üzerinde bir etkisi vardır. Birçok uygulamada, bazı tutarsızlık düzeyi kabul edilebilir, ancak bu değer tüm iş yükleri için geçerli değildir. Uygulama katmanı önbelleğe alma stratejisini uygulamadan önce herhangi bir uygulama gereksinimini tam olarak anlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- DTU tabanlı hizmet katmanları hakkında daha fazla bilgi için bkz. [DTU tabanlı satın alma modeli](service-tiers-dtu.md).
- Sanal çekirdek tabanlı hizmet katmanları hakkında daha fazla bilgi için bkz. [sanal çekirdek tabanlı satın alma modeli](service-tiers-vcore.md).
- Elastik havuzlar hakkında daha fazla bilgi için bkz. [Azure elastik havuzu nedir?](elastic-pool-overview.md)
- Performans ve elastik havuzlar hakkında daha fazla bilgi için bkz. [elastik havuzun ne zaman dikkate alınması](elastic-pool-overview.md)
