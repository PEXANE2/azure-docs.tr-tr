---
title: Sıralı kümelenmiş columnstore dizini ile performans ayarlama
description: Sorgu performansınızı artırmak için sıralı kümelenmiş sütun mağazası dizini kullanırken bilmeniz gereken öneriler ve hususlar.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: a5bb048a2368f60a83e70dcd6d1ce663ce70a885
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350928"
---
# <a name="performance-tuning-with-ordered-clustered-columnstore-index"></a>Sıralı kümelenmiş columnstore dizini ile performans ayarlama  

Kullanıcılar SQL Analytics'te bir sütun mağazası tablosunu sorguladığında, en iyi duruma getirici her segmentte depolanan minimum ve maksimum değerleri denetler.  Sorgu yükleminin sınırları dışında olan segmentler diskten belleğe okunmaz.  Okunacak kesim sayısı ve toplam boyutları küçükse, sorgu daha hızlı performans elde edebilir.   

## <a name="ordered-vs-non-ordered-clustered-columnstore-index"></a>Sıralı vs sıralı olmayan kümelenmiş sütun deposu dizini 
Varsayılan olarak, dizin seçeneği olmadan oluşturulan her SQL Analytics tablosu için, bir iç bileşen (dizin oluşturucusu) üzerinde sıralı olmayan kümelenmiş sütun deposu dizini (CCI) oluşturur.  Her sütundaki veriler ayrı bir CCI satır grubu segmentine sıkıştırılır.  Her kesimin değer aralığında meta veriler vardır, bu nedenle sorgu yükleminin sınırları dışında olan segmentler sorgu yürütme sırasında diskten okunmaz.  CCI en yüksek düzeyde veri sıkıştırma sunar ve sorguların daha hızlı çalıştırabilmesi için okumak için segmentlerin boyutunu azaltır. Ancak, dizin oluşturucusu verileri segmentlere sıkıştırmadan önce sıralamadığından, çakışan değer aralıklarına sahip segmentler oluşabilir ve sorguların diskten daha fazla kesim okumasına ve tamamlanmasının daha uzun sürmesine neden olabilir.  

Sıralı bir CCI oluştururken, SQL Analytics altyapısı bellekteki varolan verileri dizin oluşturucusu bunları dizin segmentlerine sıkıştırmadan önce sıra anahtarına göre sıralar.  Sıralanmış verilerle, segment çakışması, sorguların daha verimli bir segment eliminasyonuna ve böylece diskten okunacak kesim sayısı daha küçük olduğundan daha hızlı performansa sahip olmasını sağlayarak azalır.  Tüm veriler aynı anda bellekte sıralanabilirse, segment çakışması önlenebilir.  SQL Analytics tablolarındaki büyük veri boyutu göz önüne alındığında, bu senaryo sık sık gerçekleşmez.  

Bir sütunun segment aralıklarını denetlemek için bu komutu tablo adınız ve sütun adınız ile çalıştırın:

```sql
SELECT o.name, pnp.index_id, 
cls.row_count, pnp.data_compression_desc, 
pnp.pdw_node_id, pnp.distribution_id, cls.segment_id, 
cls.column_id, 
cls.min_data_id, cls.max_data_id, 
cls.max_data_id-cls.min_data_id as difference
FROM sys.pdw_nodes_partitions AS pnp
   JOIN sys.pdw_nodes_tables AS Ntables ON pnp.object_id = NTables.object_id AND pnp.pdw_node_id = NTables.pdw_node_id
   JOIN sys.pdw_table_mappings AS Tmap  ON NTables.name = TMap.physical_name AND substring(TMap.physical_name,40, 10) = pnp.distribution_id
   JOIN sys.objects AS o ON TMap.object_id = o.object_id
   JOIN sys.pdw_nodes_column_store_segments AS cls ON pnp.partition_id = cls.partition_id AND pnp.distribution_id  = cls.distribution_id
JOIN sys.columns as cols ON o.object_id = cols.object_id AND cls.column_id = cols.column_id
WHERE o.name = '<Table Name>' and cols.name = '<Column Name>'  and TMap.physical_name  not like '%HdTable%'
ORDER BY o.name, pnp.distribution_id, cls.min_data_id 


```

> [!NOTE] 
> Sıralı bir CCI tablosunda, aynı DML veya veri yükleme işlemlerinden kaynaklanan yeni veriler bu toplu iş içinde sıralanır, tablodaki tüm veriler arasında genel sıralama yoktur.  Kullanıcılar tablodaki tüm verileri sıralamak için sıralanan CCI'yı YENIDEN OLUŞTURAbAbilir.  SQL Analytics'te, sütun mağazası dizini REBUILD çevrimdışı bir işlemdir.  Bölümlenmiş bir tablo için REBUILD her seferinde bir bölüm yapılır.  Yeniden oluşturulmakta olan bölümdeki veriler "çevrimdışı" olur ve REBUILD bu bölüm için tamamlanana kadar kullanılamaz. 

## <a name="query-performance"></a>Sorgu performansı

Bir sorgunun sıralı cci'den elde ettiği performans kazancı, sorgu desenlerine, verilerin boyutuna, verilerin ne kadar iyi sıralanmasına, kesimlerin fiziksel yapısına ve sorgu yürütmesi için seçilen DWU ve kaynak sınıfına bağlıdır.  Kullanıcılar, sıralı bir CCI tablosu tasarlarken sipariş sütunlarını seçmeden önce tüm bu etkenleri gözden geçirmelidir.

Tüm bu desenlere sahip sorgular genellikle sıralı CCI ile daha hızlı çalışır.  
1. Sorgularda eşitlik, eşitsizlik veya aralık yüklemleri vardır
1. Yüklem sütunları ve sıralı CCI sütunları aynıdır.  
1. Yüklem sütunları, sıralı CCI sütunlarının sütun ordinal'ı ile aynı sırada kullanılır.  
 
Bu örnekte, tablo T1 Col_C, Col_B ve Col_A sırasına göre sıralanmış kümelenmiş bir sütun deposu dizini vardır.

```sql

CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON  T1
ORDER (Col_C, Col_B, Col_A)

```

Sorgu 1'in performansı, sipariş edilen CCI'dan diğer üç sorgudan daha fazla yararlanabilir. 

```sql
-- Query #1: 

SELECT * FROM T1 WHERE Col_C = 'c' AND Col_B = 'b' AND Col_A = 'a';

-- Query #2

SELECT * FROM T1 WHERE Col_B = 'b' AND Col_C = 'c' AND Col_A = 'a';

-- Query #3
SELECT * FROM T1 WHERE Col_B = 'b' AND Col_A = 'a';

-- Query #4
SELECT * FROM T1 WHERE Col_A = 'a' AND Col_C = 'c';

```

## <a name="data-loading-performance"></a>Veri yükleme performansı

Sıralı bir CCI tablosuna veri yükleme performansı bölümlenmiş tabloya benzer.  Verileri sıralı bir CCI tablosuna yüklemek, veri sıralama işlemi nedeniyle sipariş edilmeyen bir CCI tablosundan daha uzun sürebilir, ancak sorgular daha sonra sipariş edilen CCI ile daha hızlı çalıştırılabilir.  

Burada farklı şema ile tablolara veri yükleme örnek bir performans karşılaştırması.

![Performance_comparison_data_loading](./media/performance-tuning-ordered-cci/cci-data-loading-performance.png)


CcI ve sıralı CCI arasında örnek bir sorgu performans karşılaştırması aşağıda verilmiştir.

![Performance_comparison_data_loading](./media/performance-tuning-ordered-cci/occi_query_performance.png)

 
## <a name="reduce-segment-overlapping"></a>Segment çakışma azaltın

Çakışan kesimlerin sayısı, sıralı CCI oluşturma sırasında sıralı verilerin boyutuna, kullanılabilir belleğe ve en yüksek paralellik derecesine (MAXDOP) bağlıdır. Aşağıda, sıralı CCI oluştururken segment çakışmasını azaltma seçenekleri verilmiştir.

- Dizin oluşturucu verileri segmentlere sıkıştırmadan önce veri sıralama için daha fazla bellek sağlamak için daha yüksek bir DWU üzerinde xbiggerc kaynak sınıfını kullanın.  Dizin segmentinde bir kez, verilerin fiziksel konumu değiştirilemez.  Bir segment içinde veya segmentler arasında veri sıralaması yoktur.  

- MAXDOP = 1 ile sipariş cci oluşturun.  Sıralı CCI oluşturma için kullanılan her iş parçacığı bir veri alt kümesi üzerinde çalışır ve yerel olarak sıralar.  Farklı iş parçacıklarına göre sıralanmış veriler arasında genel sıralama yoktur.  Paralel iş parçacığı kullanmak, sıralı bir CCI oluşturma süresini azaltabilir, ancak tek bir iş parçacığı kullanmaktan daha çakışan parçalar oluşturur.  Şu anda, MAXDOP seçeneği yalnızca CREATE TABLE AS SELECT komutunu kullanarak sıralı bir CCI tablosu oluştururken desteklenir.  CREATE INDEX veya CREATE TABLE komutları aracılığıyla sıralanan bir CCI oluşturmak MAXDOP seçeneğini desteklemez. Örneğin,

```sql
CREATE TABLE Table1 WITH (DISTRIBUTION = HASH(c1), CLUSTERED COLUMNSTORE INDEX ORDER(c1) )
AS SELECT * FROM ExampleTable
OPTION (MAXDOP 1);
```
- Verileri SQL Analytics tablolarına yüklemeden önce sıralama anahtarına göre önceden sıralayın.


Aşağıda, yukarıdaki önerilerin ardından çakışan sıfır segmente sahip sıralı bir CCI tablo dağılımı örneği verilmiştir. Sipariş edilen CCI tablosu, MAXDOP 1 ve xbiggerc kullanılarak 20 GB'lık bir yığın tablodan CTAS aracılığıyla DWU1000c veritabanında oluşturulur.  CCI, çoğaltmasız bir BIGINT sütununda sıralanır.  

![Segment_No_Overlapping](./media/performance-tuning-ordered-cci/perfect-sorting-example.png)

## <a name="create-ordered-cci-on-large-tables"></a>Büyük tablolarda sıralı CCI oluşturma
Sıralı bir CCI oluşturmak çevrimdışı bir işlemdir.  Bölümü olmayan tablolar için, sipariş edilen CCI oluşturma işlemi tamamlanana kadar veriler kullanıcılar tarafından erişilemez.   Bölümlenmiş tablolar için, motor bölüm tarafından sıralanan CCI bölümlerini oluşturduğundan, kullanıcılar sıralanan CCI oluşturma işleminde olmayan bölümlerdeki verilere erişebilir.   Büyük tablolarda sipariş edilen CCI oluşturma sırasında kapalı kalma süresini en aza indirmek için bu seçeneği kullanabilirsiniz: 

1.    Hedef büyük tabloda (Table_A olarak adlandırılır) bölümler oluşturun.
2.    Tablo A ile aynı tablo ve bölüm şeması ile boş bir sıralı CCI tablosu (Table_B olarak adlandırılır) oluşturun.
3.    Bir bölümü Tablo A'dan Tablo B'ye değiştirin.
4.    ALTER INDEX <Ordered_CCI_Index> <Table_B <> Table_B yeniden oluşturma bölüm = <Partition_ID> Tablo B'de yeniden anahtarlanan bölümü yeniden oluşturmak için çalıştırın.  
5.    Table_A her bölüm için adım 3 ve 4'ü yineleyin.
6.    Tüm bölümler Table_A'den Table_B değiştirildikten ve yeniden inşa edildikten sonra Table_A bırakın ve Table_B Table_A olarak yeniden adlandırın. 

## <a name="examples"></a>Örnekler

**A. Sıralı sütunlar ve sipariş ordinal kontrol etmek için:**
```sql
SELECT object_name(c.object_id) table_name, c.name column_name, i.column_store_order_ordinal 
FROM sys.index_columns i 
JOIN sys.columns c ON i.object_id = c.object_id AND c.column_id = i.column_id
WHERE column_store_order_ordinal <>0
```

**B. Sütun ordinalini değiştirmek, sipariş listesinden sütun eklemek veya kaldırmak veya CCI'dan sipariş edilen CCI'ya değiştirmek için:**
```sql
CREATE CLUSTERED COLUMNSTORE INDEX InternetSales ON  InternetSales
ORDER (ProductKey, SalesAmount)
WITH (DROP_EXISTING = ON)
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.
