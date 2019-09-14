---
title: Azure SQL veri ambarı sıralı kümelenmiş columnstore dizini ile performans ayarı | Microsoft Docs
description: Sorgu performansınızı geliştirmek için sıralı kümelenmiş columnstore dizini kullanırken bilmeniz gereken öneriler ve önemli noktalar.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 09/05/2019
ms.author: xiaoyul
ms.reviewer: nibruno; jrasnick
ms.openlocfilehash: 41fbebcf4b85f6e48babba30c2d05fedb3e7a5c7
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985304"
---
# <a name="performance-tuning-with-ordered-clustered-columnstore-index"></a>Sıralı kümelenmiş columnstore diziniyle performans ayarı  

Kullanıcılar Azure SQL veri ambarı 'nda bir columnstore tablosunu sorgudığında, iyileştirici her kesimde depolanan en düşük ve en yüksek değerleri denetler.  Sorgu koşulunun sınırları dışında kalan segmentler diskten belleğe okunamaz.  Okunan parçaların sayısı ve toplam boyutu küçük olduğunda sorgu daha hızlı bir performans alabilir.   

## <a name="ordered-vs-non-ordered-clustered-columnstore-index"></a>Sıralı ve sıralı olmayan kümelenmiş columnstore dizini 
Varsayılan olarak, bir dizin seçeneği olmadan oluşturulan her Azure veri ambarı tablosu için, bir iç bileşen (Dizin Oluşturucu), üzerinde sıralı olmayan bir kümelenmiş columnstore dizini (CCı) oluşturur.  Her sütundaki veriler ayrı bir CCI satır grubu segmentinde sıkıştırılır.  Her bir segmentin değer aralığında meta veriler bulunur, bu nedenle sorgu koşulunun sınırları dışında kalan segmentler sorgu yürütme sırasında diskten okunmazlar.  CCı, en yüksek düzeyde veri sıkıştırması sağlar ve sorguların daha hızlı çalışabilmesi için okunacak parçaların boyutunu azaltır. Ancak, Dizin Oluşturucu verileri kesimlere sıkıştırmadan önce sıralamadığından, çakışan değer aralıklarına sahip kesimler meydana gelebilir ve sorguların diskten daha fazla kesim okumasına ve daha uzun sürmesine neden olabilir.  

Sıralı bir CCı oluştururken, Azure SQL veri ambarı altyapısı bellekteki verileri Dizin Oluşturucu tarafından dizin kesimlerine sıkıştırmadan önce sıralama anahtarına göre sıralar.  Sıralanmış verilerle, çakışan bölüm, sorguların daha verimli bir kesim yok etme ve bu nedenle diskten okunan segmentlerin sayısı daha az olduğundan daha hızlı performans sağlar.  Tüm veriler bellekte aynı anda sıralanmışsa, çakışan segmentden kaçınılabilir.  Veri ambarı tablolarında verilerin büyük boyutu verildiğinde, bu senaryo genellikle gerçekleşmez.  

Bir sütunun kesim aralıklarını denetlemek için şu komutu tablo adınızla ve sütun adınızla çalıştırın:

```sql
SELECT o.name, pnp.index_id, pnp.rows, pnp.data_compression_desc, pnp.pdw_node_id, 
pnp.distribution_id, cls.segment_id, cls.column_id, cls.min_data_id, cls.max_data_id, cls.max_data_id-cls.min_data_id as difference
FROM sys.pdw_nodes_partitions AS pnp
   JOIN sys.pdw_nodes_tables AS Ntables ON pnp.object_id = NTables.object_id AND pnp.pdw_node_id = NTables.pdw_node_id
   JOIN sys.pdw_table_mappings AS Tmap  ON NTables.name = TMap.physical_name AND substring(TMap.physical_name,40, 10) = pnp.distribution_id
   JOIN sys.objects AS o ON TMap.object_id = o.object_id
   JOIN sys.pdw_nodes_column_store_segments AS cls ON pnp.partition_id = cls.partition_id AND pnp.distribution_id  = cls.distribution_id
   JOIN sys.columns as cols ON o.object_id = cols.object_id AND cls.column_id = cols.column_id
WHERE o.name = '<table_name>' and c.name = '<column_name>'
ORDER BY o.name, pnp.distribution_id, cls.min_data_id
```

## <a name="data-loading-performance"></a>Veri yükleme performansı

Sıralı bir CCı tablosuna yükleme verilerinin performansı bölümlenmiş bir tabloya veri yüklemeye benzer.  
Verilerin sıralı bir CCı tablosuna yüklenmesi, veri sıralaması nedeniyle sıralı olmayan bir CCı tablosuna veri yüklemeden daha fazla zaman alabilir.  

Aşağıda, verileri farklı şemalara sahip tablolara yüklemenin bir örnek performans karşılaştırması verilmiştir.
![Performance_comparison_data_loading](media/performance-tuning-ordered-cci/cci-data-loading-performance.png)
 
## <a name="reduce-segment-overlapping"></a>Çakışan kesimi azalt
Aşağıda, CTAS aracılığıyla veya mevcut bir tabloda veri içeren bir tablo üzerinde sıralı CCı oluştururken çakışan segmenti daha fazla azaltma seçenekleri verilmiştir:

- Dizin Oluşturucu onları kesimlere sıkıştırmadan önce bellekte bir kez daha fazla verinin sıralanmasını sağlamak için daha büyük bir kaynak sınıfı kullanın.  Dizin segmentinde, verilerin fiziksel konumu değiştirilemez.  Bir kesim içinde veya kesimlerde veri sıralaması yoktur.  

- Daha düşük bir paralellik derecesi kullanın (örneğin, DOP = 1).  Sıralı CCı oluşturma için kullanılan her iş parçacığı verilerin bir alt kümesinde çalışarak yerel olarak sıralar.  Farklı iş parçacıkları tarafından sıralanan veriler arasında genel sıralama yoktur.  Paralel iş parçacıklarının kullanılması, sıralı bir CCı oluşturma süresini azaltabilir, ancak tek bir iş parçacığı kullanmaktan daha fazla çakışan kesim oluşturur. 
- Verileri Azure SQL veri ambarı tablolarına yüklemeden önce sıralama anahtarına göre önceden sıralayın.

## <a name="create-ordered-cci-on-large-tables"></a>Büyük tablolarda sıralı CCı oluşturma
Sıralı bir CCı oluşturma, çevrimdışı bir işlemdir.  Bölüm içermeyen tablolar için, sıralı CCı oluşturma işlemi tamamlanana kadar verilere kullanıcılar erişemez.   Bölümlenmiş tablolar için, altyapı sıralı CCı bölümünü bölüm tarafından oluşturduğundan, kullanıcılar sıralı CCı oluşturma işleminin işlem içinde olmadığı bölümlerdeki verilere erişmeye devam edebilir.   Büyük tablolarda sıralı CCı oluşturma sırasında kapalı kalma süresini en aza indirmek için bu seçeneği kullanabilirsiniz: 

1.  Hedef büyük tabloda (Tablo A olarak adlandırılır) bölümler oluşturun.
2.  Tablo A ile aynı tablo ve bölüm şemasına sahip boş bir sıralı CCı tablosu (Tablo B olarak adlandırılır) oluşturun.
3.  A tablosundan tablo B 'ye bir bölüm geçirin.
4.  Değiştirilmiş bölümü yeniden derlemek için ALTER INDEX < Ordered_CCI_Index > yeniden oluşturma ' yı Tablo B üzerinde çalıştırın.  
5.  A tablosundaki her bölüm için 3. ve 4. adımı yineleyin.
6.  Tüm bölümler tablo A 'dan tablo B 'ye geçildiğinde ve yeniden oluşturulduktan sonra tabloyu bir kez bırakın ve tablo B 'yi yeniden adlandırın. 

## <a name="examples"></a>Örnekler

**A. Sıralı sütunları ve sıra sıra sayısını denetlemek için:**
```sql
SELECT object_name(c.object_id) table_name, c.name column_name, i.column_store_order_ordinal 
FROM sys.index_columns i 
JOIN sys.columns c ON i.object_id = c.object_id AND c.column_id = i.column_id
WHERE column_store_order_ordinal <>0
```

**KENARI. Sütun sırasını değiştirmek için, sipariş listesinden sütun ekleyin veya kaldırın ya da CCı 'dan sıralı CCı 'ya geçiş yapın:**
```sql
CREATE CLUSTERED COLUMNSTORE INDEX InternetSales ON  InternetSales
ORDER (ProductKey, SalesAmount)
WITH (DROP_EXISTING = ON)
```

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Data Warehouse geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
