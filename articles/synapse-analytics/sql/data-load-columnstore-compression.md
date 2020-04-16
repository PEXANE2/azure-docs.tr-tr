---
title: Sütun deposu dizini performansını artırma
description: Bir sütun deposu dizininin her satır grubuna sıkıştırtığı satır sayısını en üst düzeye çıkarmak için bellek gereksinimlerini azaltın veya kullanılabilir belleği artırın.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: f1f3667c088c5f7300317ea02ca19a72e4e62905
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431039"
---
# <a name="maximizing-rowgroup-quality-for-columnstore"></a>Sütun mağazası için satır grubu kalitesini en üst düzeye çıkarma

Satır grubu kalitesi, bir satır grubundaki satır sayısına göre belirlenir. Kullanılabilir belleği artırmak, bir sütun deposu dizininin her satır grubuna sıkıştıran satır sayısını en üst düzeye çıkarabilir.  Sıkıştırma oranlarını ve sütun mağazası dizinleri için sorgu performansını artırmak için bu yöntemleri kullanın.

## <a name="why-the-rowgroup-size-matters"></a>Satır grubu boyutu neden önemlidir?

Bir sütun deposu dizini tek tek satır gruplarının sütun bölümlerini tarayarak bir tabloyu taradığından, her satır grubundaki satır sayısını en üst düzeye çıkarmak sorgu performansını artırır. Satır grupları nın satır sayısı yüksekolduğunda, veri sıkıştırma geliştirir, bu da diskten okunacak daha az veri olduğu anlamına gelir.

Satır grupları hakkında daha fazla bilgi için [Columnstore Dizinler Kılavuzu'na](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.

## <a name="target-size-for-rowgroups"></a>Satır grupları için hedef boyutu

En iyi sorgu performansı için amaç, sütun deposu dizininde satır başına satır başına satır sayısını en üst düzeye çıkarmaktır. Bir satır grubunun en fazla 1.048.576 satırı olabilir. Satır grubu başına en fazla satır sayısına sahip olmamak sorun değil. Sütun grupları en az 100.000 satır olduğunda sütun mağazası dizinleri iyi performans elde eder.

## <a name="rowgroups-can-get-trimmed-during-compression"></a>Satır grupları sıkıştırma sırasında kırpılabilir

Toplu yük veya sütun deposu dizini yeniden oluşturma sırasında, bazen her satır grubu için atanan tüm satırları sıkıştırmak için yeterli bellek yoktur. Bellek basıncı olduğunda, sütun deposu dizinleri satır grubu boyutlarını kırparak sütun deposuna sıkıştırmanın başarılı olabileceğini gösterir.

Her satır grubuna en az 10.000 satır sıkıştırmak için yeterli bellek olmadığında, bir hata oluşturulur.

Toplu yükleme hakkında daha fazla bilgi için, [kümelenmiş sütun deposu dizinine toplu yükleme](/sql/relational-databases/indexes/columnstore-indexes-data-loading-guidance?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#Bulk )bölümüne bakın.

## <a name="how-to-monitor-rowgroup-quality"></a>Rowgroup kalitesi nasıl izlenir?

DMV sys.dm_pdw_nodes_db_column_store_row_group_physical_stats[(sys.dm_db_column_store_row_group_physical_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) sql DB eşleşen görünüm tanımı içerir) satır gruplarında satır sayısı ve kırpma varsa kırpma nedeni gibi yararlı bilgileri ortaya çıkarır. Satır grubu kırpma hakkında bilgi almak için bu DMV'yi sorgulamanın kullanışlı bir yolu olarak aşağıdaki görünümü oluşturabilirsiniz.

```sql
create view dbo.vCS_rg_physical_stats
as
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[distribution_id]    = nt.[distribution_id]
)
select *
from cte;
```

trim_reason_desc, satır grubunun kırpılıp kırpılmadığını söyler (trim_reason_desc = NO_TRIM kırpma olmadığını ve satır grubunun en uygun kalitede olduğunu gösterir). Aşağıdaki kırpma nedenleri, satır grubunun erken kırpılmış olduğunu gösterir:

- BULKLOAD: Bu kırpma nedeni, yük için gelen satır toplu 1 milyondan az satır olduğunda kullanılır. 100.000'den fazla satır takılıysa (delta deposuna eklemek yerine) sıkıştırılmış satır grupları oluşturur, ancak kırpma nedenini BULKLOAD olarak ayarlar. Bu senaryoda, toplu iş yükünüzü daha fazla satır içerecek şekilde artırmayı düşünün. Ayrıca, satır grupları bölüm sınırlarını genişletemediğinden çok parçalı olmadığından emin olmak için bölümleme düzeninizi yeniden değerlendirin.
- MEMORY_LIMITATION: 1 milyon satırlık satır grupları oluşturmak için, motor tarafından belirli bir miktar çalışma belleği gereklidir. Yükleme oturumunun kullanılabilir belleği gerekli çalışma belleğinden daha az olduğunda, satır grupları zamanından önce kırpılır. Aşağıdaki bölümlerde, gerekli bellek nasıl tahmin ve daha fazla bellek ayırmak açıklayınız.
- DICTIONARY_SIZE: Bu kırpma nedeni, geniş ve/veya yüksek kardinallik dizelerine sahip en az bir dize sütunu olduğundan satır grubu kırpma sının oluştuğunu gösterir. Sözlük boyutu bellekte 16 MB ile sınırlıdır ve bu sınıra ulaşıldıktan sonra satır grubu sıkıştırılır. Bu durumla karşılaştıysanız, sorunlu sütunu ayrı bir tabloya yalıtmayı düşünün.

## <a name="how-to-estimate-memory-requirements"></a>Bellek gereksinimleri nasıl tahmin edilir?

Bir satır grubunu sıkıştırmak için gereken maksimum bellek yaklaşık olarak

- 72 MB +
- \#satır \* \#sütunları \* 8 bayt +
- \#satırkısa \* \#dize-sütunlar \* 32 bayt +
- \#sıkıştırma sözlüğü için \* uzun dize sütunları 16 MB

kısa dize sütunları <= 32 bayt ve uzun dize-sütunlar > 32 bayt dize veri türleri string veri türleri kullanın dize veri türleri kullanın.

Uzun dizeleri metin sıkıştırma için tasarlanmış bir sıkıştırma yöntemi ile sıkıştırılır. Bu sıkıştırma yöntemi metin desenleri depolamak için bir *sözlük* kullanır. Sözlüğün maksimum boyutu 16 MB'dır. Satır grubundaki her uzun dize sütunu için yalnızca bir sözlük vardır.

Sütun deposu bellek gereksinimlerini derinlemesine tartışmak için [synapse SQL ölçekleme](https://channel9.msdn.com/Events/Ignite/2016/BRK3291)videosuna bakın: yapılandırma ve kılavuz.

## <a name="ways-to-reduce-memory-requirements"></a>Bellek gereksinimlerini azaltmanın yolları

Satır gruplarını sütun deposu dizinlerine sıkıştırmak için bellek gereksinimlerini azaltmak için aşağıdaki teknikleri kullanın.

### <a name="use-fewer-columns"></a>Daha az sütun kullanma

Mümkünse, tabloyu daha az sütuniçeren tasarlayın. Bir satır grubu sütun deposuna sıkıştırıldığında, sütun deposu dizini her sütun kesimini ayrı ayrı sıkıştırır. Bu nedenle, sütun sayısı arttıkça bir satır grubunu sıkıştırmak için bellek gereksinimleri artar.

### <a name="use-fewer-string-columns"></a>Daha az dize sütunu kullanma

Dize veri türlerinin sütunları sayısal ve tarih veri türlerinden daha fazla bellek gerektirir. Bellek gereksinimlerini azaltmak için dize sütunlarını olgu tablolarından kaldırmayı ve bunları daha küçük boyut tablolarına koymayı düşünün.

Dize sıkıştırma için ek bellek gereksinimleri:

- 32 karaktere kadar dize veri türleri, değer başına 32 ek bayt gerektirebilir.
- 32'den fazla karaktere sahip dize veri türleri sözlük yöntemleri kullanılarak sıkıştırılır.  Satır grubundaki her sütunun sözlüğü oluşturmak için 16 MB'a kadar ek bir bilgi gerekir.

### <a name="avoid-over-partitioning"></a>Aşırı bölümlemeden kaçının

Sütun mağazası dizinleri bölüm başına bir veya daha fazla satır grubu oluşturur. Azure Synapse Analytics'te veri depolama için, veriler dağıtıldıkça ve her dağıtım bölümlendirildikçe bölüm sayısı hızla artar. Tabloda çok fazla bölüm varsa, satır gruplarını doldurmak için yeterli satır olmayabilir. Satır eksikliği sıkıştırma sırasında bellek basıncı oluşturmaz, ancak en iyi sütun mağazası sorgu performansını elde olmayan satır gruplarına yol açar.

Aşırı bölümleme önlemek için başka bir nedeni bölümlenmiş bir tabloüzerinde bir sütun deposu dizini içine satır yükleme için bir bellek yükü olmasıdır. Bir yük sırasında, her bölüm sıkıştırılmak için yeterli satır alanına kadar bellekte tutulan birçok bölüm gelen satırları alabilir. Çok fazla bölüm olması ek bellek basıncı oluşturur.

### <a name="simplify-the-load-query"></a>Yük sorgusunu basitleştirin

Veritabanı, sorgudaki tüm işleçler arasında bir sorgu için bellek hibesini paylaşır. Bir yük sorgusu karmaşık sıralamaları ve birleştirir, sıkıştırma için kullanılabilir bellek azalır.

Yalnızca sorguyu yüklemeye odaklanmak için yük sorgusunu tasarla. Verilerdeki dönüşümleri çalıştırmanız gerekiyorsa, bunları yük sorgusundan ayrı çalıştırın. Örneğin, verileri yığın tablosunda sahneleyin, dönüşümleri çalıştırın ve ardından evreleme tablosunu sütun deposu dizinine yükleyin. 

### <a name="adjust-maxdop"></a>MAXDOP'u ayarla

Her dağıtım, her dağıtım için birden fazla CPU çekirdeği olduğunda satır gruplarını paralel olarak sütun deposuna sıkıştırır. Paralellik, bellek basıncıve satır grubu kırpma yol açabilir ek bellek kaynakları gerektirir.

Bellek basıncını azaltmak için, yük işlemini her dağıtım da seri modunda çalıştırmaya zorlamak için MAXDOP sorgu ipucunu kullanabilirsiniz.

```sql
CREATE TABLE MyFactSalesQuota
WITH (DISTRIBUTION = ROUND_ROBIN)
AS SELECT * FROM FactSalesQuota
OPTION (MAXDOP 1);
```

## <a name="ways-to-allocate-more-memory"></a>Daha fazla bellek ayırmanın yolları

DWU boyutu ve kullanıcı kaynak sınıfı birlikte bir kullanıcı sorgusu için ne kadar bellek kullanılabilir belirler. Bir yük sorgusu için bellek hibesini artırmak için, DWUs sayısını artırabilir veya kaynak sınıfını artırabilirsiniz.

- DWUs'ları artırmak için [performansı nasıl ölçeklendirebilirim bakın?](../sql-data-warehouse/quickstart-scale-compute-portal.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- Sorgunun kaynak sınıfını değiştirmek için [bkz.](../sql-data-warehouse/resource-classes-for-workload-management.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#change-a-users-resource-class)

## <a name="next-steps"></a>Sonraki adımlar

Synapse SQL'de performansı artırmak için daha fazla yol bulmak için [Performans'a genel bakış'a](../overview-cheat-sheet.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

 
