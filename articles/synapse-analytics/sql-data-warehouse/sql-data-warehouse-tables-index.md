---
title: Dizine tablo ekleme
description: SQL Analytics'te tabloları dizine ekleme önerileri ve örnekleri.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/18/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: ced965f94808bdc672f694bede5c239178891f97
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351280"
---
# <a name="indexing-tables-in-sql-analytics"></a>SQL Analytics'te tabloları dizinleme

SQL Analytics'te tabloları dizine ekleme önerileri ve örnekleri.

## <a name="index-types"></a>Dizin türleri

SQL [Analytics, kümelenmiş sütun deposu dizinleri,](/sql/relational-databases/indexes/columnstore-indexes-overview) [kümelenmiş dizinler ve kümelenmemiş dizinler](/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described)ve [yığın](/sql/relational-databases/indexes/heaps-tables-without-clustered-indexes)olarak da bilinen dizin dışı bir seçenek gibi çeşitli dizin oluşturma seçenekleri sunar.  

Dizin içeren bir tablo oluşturmak için [CREATE TABLE (SQL Analytics)](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) belgelerine bakın.

## <a name="clustered-columnstore-indexes"></a>Kümelenmiş sütun deposu dizinleri

Varsayılan olarak, SQL Analytics tabloda dizin seçeneği belirtilmediğinde kümelenmiş bir sütun mağazası dizini oluşturur. Kümelenmiş sütun deposu tabloları hem en yüksek veri sıkıştırma düzeyini hem de en iyi genel sorgu performansını sunar.  Kümelenmiş sütun deposu tabloları genellikle kümelenmiş dizin veya yığın tabloları daha iyi performans ve genellikle büyük tablolar için en iyi seçimdir.  Bu nedenlerden dolayı, clustered columnstore, tablonuzu nasıl dizine ekeceğinden emin değilseniz başlamak için en iyi yerdir.  

Kümelenmiş bir sütun deposu tablosu oluşturmak için, WITH yan tümcesinde CLUSTERED COLUMNSTORE INDEX'i belirtmeniz veya WITH yan tümcesini kapalı bırakmanız yeterlidir:

```SQL
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Kümelenmiş sütun mağazasının iyi bir seçenek olmayabileceği birkaç senaryo vardır:

- Sütun deposu tabloları varchar(max), nvarchar(max) ve varbinary(max) desteklemez. Bunun yerine yığın veya kümelenmiş dizin düşünün.
- Sütun deposu tabloları geçici veriler için daha az verimli olabilir. Yığın ve hatta belki de geçici tablolar düşünün.
- 60 milyondan az sıralı küçük masalar. Yığın masaları düşünün.

## <a name="heap-tables"></a>Yığın tablolar

SQL Analytics'te verileri geçici olarak indirdiğinizde, yığın tablosu kullanmanın genel işlemi hızlandırdığını görebilirsiniz. Bunun nedeni, yığınlara yüklenen yüklerin tabloları dizine göre daha hızlı olması ve bazı durumlarda sonraki okumanın önbellekten yapılabilecek olmasıdır.  Verileri yalnızca daha fazla dönüşüm çalıştırmadan önce aşamalı olarak dolduruyorsanız, tabloyu yığın tablosuna yüklemek, verileri kümelenmiş bir sütun deposu tablosuna yüklemekten çok daha hızlıdır. Buna ek olarak, verileri geçici bir [tabloya](sql-data-warehouse-tables-temporary.md) yüklemek, tabloyu kalıcı depolamaya yüklemekten daha hızlı yükler.  

Küçük arama tabloları için, 60 milyondan az satır, genellikle yığın tablolar mantıklı.  Küme sütun deposu tabloları, 60 milyondan fazla satır olduğunda en iyi sıkıştırmayı elde etmeye başlar.

Yığın tablosu oluşturmak için WITH yan tümcesinde HEAP belirtmeniz yeterlidir:

```SQL
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Kümelenmiş ve kümesiz dizinler

Kümelenmiş dizinler, tek bir satırın hızla alınması gerektiğinde kümelenmiş sütun deposu tablolarından daha iyi performans gösterebilir. Tek veya çok az satır araması aşırı hızda gerçekleştirmek için gerekli olan sorgular için, bir küme dizini veya nonclustered ikincil dizini düşünün. Kümelenmiş dizin kullanmanın dezavantajı, yalnızca kümelenmiş dizin sütununda yüksek seçici bir filtre kullanan sorguların olmasıdır. Diğer sütunlarda filtreyi geliştirmek için diğer sütunlara kümelenmemiş bir dizin eklenebilir. Ancak, bir tabloya eklenen her dizin yüklere hem alan hem de işlem süresi ekler.

Kümelenmiş dizin tablosu oluşturmak için WITH yan tümcesinde CLUSTERED INDEX belirtmeniz yeterlidir:

```SQL
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Tabloya kümelenmiş olmayan bir dizin eklemek için aşağıdaki sözdizimini kullanın:

```SQL
CREATE INDEX zipCodeIndex ON myTable (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Kümelenmiş sütun deposu dizinlerini en iyi duruma alma

Kümelenmiş sütun deposu tabloları veriler halinde bölümler halinde düzenlenir.  Yüksek segment kalitesine sahip olmak, sütun mağazası tablosunda en iyi sorgu performansını elde etmek için çok önemlidir.  Segment kalitesi sıkıştırılmış bir satır grubundaki satır sayısıyla ölçülebilir.  Segment kalitesi, sıkıştırılmış satır grubu başına en az 100K satır olduğu ve satır başına satır başına satır sayısı 1.048.576 satıra yaklaştıkça performans artışının olduğu ve bir satır grubunun içerebileceği en fazla satır olan en uygun satırdır.

Aşağıdaki görünüm oluşturulabilir ve sisteminizde satır grubu başına ortalama satırları hesaplamak ve herhangi bir alt-optimal küme sütun deposu dizinlerini tanımlamak için kullanılabilir.  Bu görünümdeki son sütun, dizinlerinizi yeniden oluşturmak için kullanılabilecek bir SQL deyimi oluşturur.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,    COUNT(DISTINCT rg.[partition_number])                    AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,    CEILING    ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Görünümü oluşturduğunuza göre, 100K'dan az satıra sahip satır gruplarına sahip tabloları tanımlamak için bu sorguyu çalıştırın. Tabii ki, daha uygun segment kalitesi arıyorsanız 100K eşiğini artırmak isteyebilirsiniz.

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Sorguyu çalıştırdıktan sonra verilere bakmaya ve sonuçlarınızı çözümlemeye başlayabilirsiniz. Bu tablo, satır grubu çözümlemenizde nelere bakMası gerektiğini açıklar.

| Sütun | Bu veriler nasıl kullanılır? |
| --- | --- |
| [table_partition_count] |Tablo bölümlenmişse, daha yüksek Açık satır grubu sayımları görmeyi bekleyebilirsiniz. Dağıtımdaki her bölüm teoride onunla ilişkili açık bir satır grubuna sahip olabilir. Bunu analizinize dahil et. Bölümlenmiş küçük bir tablo, sıkıştırmayı artıracağı için bölümleme tamamen kaldırılarak en iyi duruma getirilebilir. |
| [row_count_total] |Tablo için toplam satır sayısı. Örneğin, sıkıştırılmış durumdaki satıryüzdesini hesaplamak için bu değeri kullanabilirsiniz. |
| [row_count_per_distribution_MAX] |Tüm satırlar eşit olarak dağıtılırsa, bu değer dağıtım başına hedef satır sayısı olacaktır. Bu değeri compressed_rowgroup_count ile karşılaştırın. |
| [COMPRESSED_rowgroup_rows] |Tablo için sütun deposu biçimindeki toplam satır sayısı. |
| [COMPRESSED_rowgroup_rows_AVG] |Ortalama satır sayısı bir satır grubu için satırların maksimum #'undan önemli ölçüde daha azsa, verileri yeniden sıkıştırmak için CTAS veya ALTER INDEX REBUILD'ı kullanmayı düşünün |
| [COMPRESSED_rowgroup_count] |Sütun deposu biçimindesatır grup sayısı. Tabloya göre bu sayı çok yüksekse, kolon deposu yoğunluğunun düşük olduğunun bir göstergesidir. |
| [COMPRESSED_rowgroup_rows_DELETED] |Satırlar, sütun mağazası biçiminde mantıksal olarak silinir. Sayı tablo boyutuna göre yüksekse, bölümü yeniden oluşturmayı veya dizini fiziksel olarak kaldırarak yeniden oluşturmayı düşünün. |
| [COMPRESSED_rowgroup_rows_MIN] |Sütun mağazanızdaki satır gruplarının değer aralığını anlamak için Bunu AVG ve MAX sütunlarıyla birlikte kullanın. Yük eşiğinin üzerindeki düşük bir sayı (bölüm hizalı dağılım başına 102.400) veri yükünde optimizasyonların mevcut olduğunu gösterir |
| [COMPRESSED_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [OPEN_rowgroup_count] |Açık satır grupları normaldir. Bir makul tablo dağılımı (60) başına bir OPEN satır grubu beklenebilir. Aşırı sayılar, bölümler arasında veri yüklemesi olduğunu gösterir. Ses olduğundan emin olmak için bölümleme stratejisini iki kez kontrol edin |
| [OPEN_rowgroup_rows] |Her satır grubunda en fazla 1.048.576 satır olabilir. Açık satır gruplarının şu anda ne kadar dolu olduğunu görmek için bu değeri kullanın |
| [OPEN_rowgroup_rows_MIN] |Açık gruplar, verilerin tabloya yüklendiğini veya önceki yükün kalan satırgruplarına döküldüğünü gösterir. OPEN satır gruplarında ne kadar veri olduğunu görmek için MIN, MAX, AVG sütunlarını kullanın. Küçük tablolar için tüm verilerin% 100 olabilir! Bu durumda ALTER INDEX REBUILD verileri sütun deposuna zorlamak için. |
| [OPEN_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [OPEN_rowgroup_rows_AVG] |Yukarıdaki gibi |
| [CLOSED_rowgroup_rows] |Kapalı satır grup satırlarına akıl sağlığı denetimi olarak bakın. |
| [CLOSED_rowgroup_count] |Hiç görülen kapalı satır gruplarının sayısı düşük olmalıdır. Kapalı satır grupları ALTER INDEX kullanılarak sıkıştırılmış satır gruplarına dönüştürülebilir ... REORGANIZE komutu. Ancak, bu normalde gerekli değildir. Kapalı gruplar, arka plan "tuple mover" işlemi yle otomatik olarak sütun deposu satır gruplarına dönüştürülür. |
| [CLOSED_rowgroup_rows_MIN] |Kapalı satır grupları çok yüksek bir dolum oranına sahip olmalıdır. Kapalı bir satır grubunun dolgu oranı düşükse, sütun deposunun daha fazla analizi gerekir. |
| [CLOSED_rowgroup_rows_MAX] |Yukarıdaki gibi |
| [CLOSED_rowgroup_rows_AVG] |Yukarıdaki gibi |
| [Rebuild_Index_SQL] |SQL bir tablo için sütun deposu dizini yeniden oluşturmak için |

## <a name="causes-of-poor-columnstore-index-quality"></a>Columnstore dizin kalitesinin düşük olmasının nedenleri

Segment kalitesi düşük tabloları tanımladıysanız, temel nedenini tanımlamak istiyorsunuz.  Aşağıda düşük segment kalitesi diğer bazı yaygın nedenleri şunlardır:

1. Dizin oluşturulunca bellek basıncı
2. Yüksek hacimli DML işlemleri
3. Küçük veya damla yük işlemleri
4. Çok fazla bölüm

Bu etkenler, bir sütun deposu dizininin satır grubu başına en uygun 1 milyon satırdan önemli ölçüde daha az olmasına neden olabilir. Ayrıca, satırların sıkıştırılmış bir satır grubu yerine delta satır grubuna gitmesine neden olabilir.

### <a name="memory-pressure-when-index-was-built"></a>Dizin oluşturulunca bellek basıncı

Sıkıştırılmış satır grubu başına satır sayısı, satır genişliği ve satır grubunu işlemek için kullanılabilir bellek miktarıyla doğrudan ilişkilidir.  Satırlar columnstore tablolarına bellek baskısı altında yazıldığında, segment kalitesi düşebilir.  Bu nedenle, en iyi yöntem, sütun mağaza dizin tablolarımümkün olduğunca çok belleğe erişim yazmakta olan oturumu vermektir.  Bellek ve eşzamanlılık arasında bir denge olduğundan, doğru bellek ayırma kılavuzu tablonuzun her satırındaki verilere, sisteminize ayrılan SQL Analytics birimlerine ve oturuma verebileceğiniz eşzamanlılık yuvası sayısına bağlıdır. tablonuza veri yazma.

### <a name="high-volume-of-dml-operations"></a>Yüksek hacimli DML işlemleri

Satırları güncelleştiren ve silen yüksek hacimli DML işlemleri sütun deposunda verimsizliğe neden olabilir. Bu, özellikle bir satır grubundaki satırların çoğunluğu değiştirildiğinde geçerlidir.

- Sıkıştırılmış bir satır grubundan bir satır ı silerken, satırı yalnızca mantıksal olarak silinmiş olarak işaretler. Bölüm veya tablo yeniden oluşturulana kadar satır sıkıştırılmış satır grubunda kalır.
- Satır eklemek, satırı delta satır grubu olarak adlandırılan dahili bir satır deposu tablosuna ekler. Eklenen satır, delta satır grubu dolana ve kapalı olarak işaretlenene kadar sütun deposuna dönüştürülmez. Satır grupları, maksimum 1.048.576 satır kapasiteye ulaştıklarında kapatılır.
- Sütun mağazası biçiminde bir satırın güncelleştirilmesi mantıksal silme ve ardından bir ekleme olarak işlenir. Eklenen satır delta deposunda depolanabilir.

Toplu güncelleştirme ve ekleme işlemleri, bölüm hizalı dağıtım başına 102.400 satırlık toplu eşiğini aşan işlemleri doğrudan sütun deposu biçimine gider. Ancak, eşit bir dağılım varsayarsak, bunun gerçekleşmesi için tek bir işlemde 6.144 milyondan fazla satırı değiştirmeniz gerekir. Belirli bir bölüm hizalanmış dağıtım için satır sayısı 102.400'den azsa, satırlar delta deposuna gider ve satır grubunu kapatmak için yeterli satır eklenene veya değiştirilene veya dizin yeniden oluşturulana kadar orada kalır.

### <a name="small-or-trickle-load-operations"></a>Küçük veya damla yük işlemleri

SQL Analytics veritabanlarına akan küçük yükler bazen damla yükler olarak da bilinir. Bunlar genellikle sistem tarafından yutulan neredeyse sabit bir veri akışını temsil eder. Ancak, bu akış sürekliyakın olduğu için satırların hacmi özellikle büyük değildir. Çoğu zaman veriler, sütun deposu biçimine doğrudan yük yükleme için gereken eşiğin altındadır.

Bu gibi durumlarda, verileri önce Azure blob depolamasına yüklemek ve yüklemeden önce birikmesine izin vermek genellikle daha iyidir. Bu teknik genellikle *mikro-toplu olarak*bilinir.

### <a name="too-many-partitions"></a>Çok fazla bölüm

Göz önünde bulundurulması gereken başka bir şey kümelenmiş sütun deposu tabloları üzerinde bölümleme etkisidir.  Bölümlemeden önce, SQL Analytics verilerinizi zaten 60 veritabanına böler.  Bölümleme verilerinizi daha da böler.  Verilerinizi bölümlere ayırırsanız, **kümelenmiş** sütun deposu dizininden yararlanmak için her bölümün en az 1 milyon satıra ihtiyacı olduğunu göz önünde bulundurun.  Tablonuzu 100 bölüme bölümlere ayırırsanız, tablonuzun kümelenmiş sütun deposu dizininden (60 dağılım *100 bölüm 1* milyon satır) yararlanabilmesi için en az 6 milyar satıra ihtiyacı vardır. 100 bölmeli tablonuzda 6 milyar satır yoksa, bölüm sayısını azaltın veya bunun yerine yığın tablo kullanmayı düşünün.

Tablolarınız bazı verilerle yüklendikten sonra, tabloları en uygun un altında kümelenmiş sütun deposu dizinleriyle tanımlamak ve yeniden oluşturmak için aşağıdaki adımları izleyin.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Segment kalitesini artırmak için dizinleri yeniden oluşturma

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Adım 1: Doğru kaynak sınıfını kullanan kullanıcıyı tanımlama veya oluşturma

Segment kalitesini hemen geliştirmenin hızlı bir yolu, dizini yeniden oluşturmaktır.  Yukarıdaki görünümtarafından döndürülen SQL, dizinlerinizi yeniden oluşturmak için kullanılabilecek bir ALTER INDEX REBUILD deyimi döndürür. Dizinlerinizi yeniden yaparken, dizininizi yeniden oluşturan oturuma yeterli bellek ayırdığınızdan emin olun.  Bunu yapmak için, bu tablodaki dizini önerilen minimuma yeniden oluşturma izinleri olan bir kullanıcının kaynak sınıfını artırın.

Aşağıda, kaynak sınıfını artırarak kullanıcıya daha fazla bellek ayırmanın bir örneği verilmiştir. Kaynak sınıflarıyla çalışmak [için iş yükü yönetimi için Kaynak sınıflarına](resource-classes-for-workload-management.md)bakın.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Adım 2: Daha yüksek kaynak sınıfı kullanıcısıyla kümelenmiş sütun mağazası dizinlerini yeniden oluşturma

Şu anda daha yüksek bir kaynak sınıfı kullanan adım 1'den (örn. LoadUser) kullanıcı olarak oturum açın ve ALTER INDEX deyimlerini çalıştırın. Bu kullanıcının dizinin yeniden oluşturulmakta olduğu tablolariçin ALTER izni olduğundan emin olun. Bu örnekler, sütun deposu dizininin tamamının nasıl yeniden oluşturulacak veya tek bir bölümün nasıl yeniden oluşturulabildiğini gösterir. Büyük tablolarda, dizinleri bir defada tek bir bölümü yeniden oluşturmak daha pratiktir.

Alternatif olarak, dizini yeniden oluşturmak yerine, [ctas kullanarak](sql-data-warehouse-develop-ctas.md)tabloyu yeni bir tabloya kopyalayabilirsiniz. Hangi yol daha iyi? Büyük hacimli veriler için CTAS genellikle [ALTER INDEX'ten](/sql/t-sql/statements/alter-index-transact-sql)daha hızlıdır. Daha küçük veri hacimleri için ALTER INDEX'in kullanımı daha kolaydır ve tabloyu değiştirmenizi gerektirmez.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

SQL Analytics'te bir dizini yeniden oluşturmak çevrimdışı bir işlemdir.  Dizinleri yeniden oluşturma hakkında daha fazla bilgi [için, Sütun deposu Dizinleri Parçalanma](/sql/relational-databases/indexes/columnstore-indexes-defragmentation)ve [ALTER INDEX](/sql/t-sql/statements/alter-index-transact-sql)ALTER INDEX ALTER INDEX rebuild bölümüne bakın.

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Adım 3: Kümelenmiş sütun deposu segment kalitesinin iyileştiğini doğrulayın

Düşük segment kalitesiyle tablotanımlayan sorguyu yeniden çalıştırın ve segment kalitesinin iyileştiğini doğrulayın.  Segment kalitesi iyileşmediyse, tablonuzdaki satırlar ekstra geniş olabilir.  Dizinlerinizi yeniden yaparken daha yüksek bir kaynak sınıfı veya DWU kullanmayı düşünün.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>CTAS ve bölüm anahtarlama ile dizinleri yeniden oluşturma

Bu örnek, [create table AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) deyimini ve tablo bölümünü yeniden oluşturmak için bölüm geçişini kullanır.

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Switch IN the rebuilt data with TRUNCATE_TARGET option
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2 WITH (TRUNCATE_TARGET = ON);
```

CTAS kullanarak bölümleri yeniden oluşturma hakkında daha fazla bilgi [için](sql-data-warehouse-tables-partition.md)bkz.

## <a name="next-steps"></a>Sonraki adımlar

Tablo geliştirme hakkında daha fazla bilgi için tablo [geliştirme'ye](sql-data-warehouse-tables-overview.md)bakın.
