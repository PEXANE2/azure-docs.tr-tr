---
title: Dağıtılmış tablolar Tasarım Kılavuzu
description: SYNAPSE SQL havuzunda karma olarak dağıtılan ve hepsini bir kez deneme dağıtılmış tablolar tasarlamaya yönelik öneriler.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 599514f6e7b97208194fc4c1660712f4d5e0c4cb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585360"
---
# <a name="guidance-for-designing-distributed-tables-in-synapse-sql-pool"></a>SYNAPSE SQL havuzunda dağıtılmış tablo tasarlamaya yönelik kılavuz

SYNAPSE SQL havuzlarında karma olarak dağıtılan ve hepsini bir kez deneme dağıtılmış tablolar tasarlamaya yönelik öneriler.

Bu makalede, SYNAPSE SQL havuzundaki veri dağıtımı ve veri taşıma kavramlarıyla ilgili bilgi sahibi olduğunuz varsayılır.Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics, yüksek düzeyde paralel işleme (MPP) mimarisi](massively-parallel-processing-mpp-architecture.md).

## <a name="what-is-a-distributed-table"></a>Dağıtılmış tablo nedir?

Dağıtılmış bir tablo, tek tablo olarak görünür, ancak satırlar aslında 60 dağıtımları genelinde depolanır. Satırlar karma veya hepsini bir kez deneme algoritması ile dağıtılır.  

**Karma Dağıtılmış tablolar** , büyük olgu tablolarında sorgu performansını geliştirir ve bu makalenin odaklanmaktadır. Hepsini bir **kez deneme tablosu** , yükleme hızını iyileştirmek için faydalıdır. Bu tasarım seçimlerinin sorgu geliştirme ve performansı yükleme konusunda önemli bir etkisi vardır.

Başka bir tablo depolama seçeneği, küçük bir tabloyu tüm Işlem düğümlerinde çoğaltmalıdır. Daha fazla bilgi için bkz. [çoğaltılan tablolar Için tasarım kılavuzu](design-guidance-for-replicated-tables.md). Üç seçenekten hızlıca seçim yapmak için bkz. tablolardaki dağıtılmış tablolar [genel bakış](sql-data-warehouse-tables-overview.md).

Tablo tasarımının bir parçası olarak, verileriniz ve verilerin nasıl sorgulandığı hakkında mümkün olduğunca fazla bilgi edinin.Örneğin, bu soruları göz önünde bulundurun:

- Tablo ne kadar büyük?
- Tablo ne sıklıkta yenilenir?
- SYNAPSE SQL havuzunda olgu ve boyut tabloları var mı?

### <a name="hash-distributed"></a>Karma dağıtıldı

Karma olarak dağıtılan bir tablo, her bir satırı bir [dağıtıma](massively-parallel-processing-mpp-architecture.md#distributions)atamak için belirleyici bir karma Işlevi kullanarak işlem düğümleri arasında tablo satırları dağıtır.

![Dağıtılmış tablo](./media/sql-data-warehouse-tables-distribute/hash-distributed-table.png "Dağıtılmış tablo")  

Özdeş değerler her zaman aynı dağıtıma karma olduğundan, veri ambarı satır konumlarına yönelik yerleşik bilgiye sahiptir. SYNAPSE SQL havuzunda bu bilgi, sorgu performansını artıran sorgular sırasında veri hareketini en aza indirmek için kullanılır.

Karma Dağıtılmış tablolar, bir yıldız şemasında büyük olgu tabloları için iyi çalışır. Bunlar çok fazla sayıda satıra sahip olabilir ve yine de yüksek performans elde edebilir. Kuşkusuz, dağıtılmış sistemin sağlamak üzere tasarlandığı performansı almanıza yardımcı olacak bazı tasarım konuları vardır. İyi bir dağıtım sütunu seçmek, bu makalede açıklanan bir noktadır.

Karma olarak dağıtılan bir tablo kullanmayı göz önünde bulundurun:

- Diskteki tablo boyutu 2 GB 'tan fazla.
- Tabloda, sık kullanılan ekleme, güncelleştirme ve silme işlemleri bulunur.

### <a name="round-robin-distributed"></a>Hepsini bir kez deneme dağıtıldı

Hepsini bir kez deneme dağıtılan tablo, tablo satırlarını tüm dağıtımların arasına eşit dağıtır. Dağıtım için satırların atanması rastgele olur. Karma dağıtılan tablolardan farklı olarak, eşit değerlere sahip satırların aynı dağıtıma atanması garanti edilmez.

Sonuç olarak, sistemin bir sorguyu çözebilmesi için bazen verilerinizi daha iyi düzenlemek üzere bir veri taşıma işlemini çağırması gerekir.  Bu ek adım, sorgularınızı yavaşlatabilir. Örneğin, hepsini bir kez deneme tablosunun katılması, genellikle bir performans isabeti olan satırları reshuffling gerektirir.

Aşağıdaki senaryolarda tablonuz için hepsini bir kez deneme dağılımı kullanmayı düşünün:

- Varsayılan değer olduğundan, basit bir başlangıç noktası olarak Başlarken
- Belirgin bir katılım anahtarı yoksa
- Karma için tablo dağıtmak üzere iyi bir aday sütunu yoksa
- Tablo, diğer tablolarla ortak bir JOIN anahtarını paylaşmaz
- Birleştirme, sorgudaki diğer birleşimlerden daha az önemse
- Tablo geçici bir hazırlama tablosu olduğunda

[Yeni York taksi verileri](load-data-from-azure-blob-storage-using-polybase.md#load-the-data-into-your-data-warehouse) için öğretici yükleme, hepsini bir kez deneme için bir hazırlama tablosuna veri yükleme örneği sağlar.

## <a name="choosing-a-distribution-column"></a>Dağıtım sütunu seçme

Karma dağıtılan bir tablo, karma anahtar olan bir dağıtım sütununa sahiptir. Örneğin, aşağıdaki kod, dağıtım sütunu olarak ProductKey ile karma dağıtılmış bir tablo oluşturur.

```sql
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

Dağıtım sütununda depolanan veriler güncelleştirilebilen olabilir. Dağıtım sütunundaki verilerin güncelleştirmeleri, veri karıştırma işlemine neden olabilir.

Bu sütundaki değerler satırların nasıl dağıtıldığını belirlemediğinden, bir dağıtım sütunu seçilmesi önemli bir tasarım kararıdır. En iyi seçenek çeşitli faktörlere bağlıdır ve genellikle avantajları içerir. Bir dağıtım sütunu seçildikten sonra, bunu değiştiremezsiniz.  

En iyi sütunu ilk kez seçmediyseniz, farklı bir dağıtım sütunuyla tabloyu yeniden oluşturmak için [Create Table Select (CTAS) olarak](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanabilirsiniz.

### <a name="choose-a-distribution-column-with-data-that-distributes-evenly"></a>Eşit olarak dağıtan verilerle bir dağıtım sütunu seçin

En iyi performansı elde etmek için tüm dağıtımların, yaklaşık olarak aynı sayıda satıra sahip olması gerekir. Bir veya daha fazla dağıtımda orantısız sayıda satır olduğunda, bazı dağıtımlar diğerlerinden önce paralel bir sorgunun parçalarını bitirir. Tüm dağıtımlar işlemeyi tamamlayana kadar sorgu tamamlantamadığından, her sorgu yalnızca en yavaş dağıtım kadar hızlıdır.

- Veri eğriltme, veriler dağıtımlar arasında eşit olarak dağıtılmayacağı anlamına gelir
- Eğriliği işlemek, bazı dağıtımların paralel sorgular çalıştırılırken başkalarının daha uzun sürme anlamına gelir. Bu, veriler eğriltilmiş olduğunda meydana gelebilir.
  
Paralel işlemeyi dengelemek için şu şekilde bir dağıtım sütunu seçin:

- **Birçok benzersiz değere sahiptir.** Sütunda yinelenen değerler olabilir. Ancak aynı değere sahip tüm satırlar aynı dağıtıma atanır. 60 dağıtımları olduğundan, sütun en az 60 benzersiz değere sahip olmalıdır.  Genellikle benzersiz değer sayısı çok daha büyüktür.
- **Null değerleri yok veya yalnızca birkaç null içeriyor.** Sıradışı bir örnek için, sütundaki tüm değerler NULL ise, tüm satırlar aynı dağıtıma atanır. Sonuç olarak, sorgu işleme bir dağıtıma eğtilir ve paralel işlemeden faydalanır.
- **Bir tarih sütunu değil**. Aynı tarih için tüm veriler aynı dağıtım içinde yer vermez. Birden çok kullanıcının hepsi aynı tarihte filtrelendiğinde, tüm işleme işlerini yalnızca 1 60 dağıtımlardan oluşur.

### <a name="choose-a-distribution-column-that-minimizes-data-movement"></a>Veri hareketini en aza indiren bir dağıtım sütunu seçin

Doğru sorgu sonucu sorgularının elde etmek için verileri bir Işlem düğümünden diğerine taşıyabilirler. Veri taşıma genellikle sorguları dağıtılmış tablolar üzerinde birleştirmeler ve toplamalar olduğunda oluşur. Veri hareketini en aza indirmeye yardımcı olacak bir dağıtım sütunu seçmek, SYNAPSE SQL havuzunuzun performansını iyileştirmeye yönelik en önemli stratejilerden biridir.

Veri hareketini en aza indirmek için şu şekilde bir dağıtım sütunu seçin:

- ,,, `JOIN` `GROUP BY` `DISTINCT` `OVER` Ve `HAVING` yan tümcelerinde kullanılır. İki büyük olgu tablosunun sık birleşme durumunda, her iki tabloyu da birleştirme sütunlarından birine dağıttığınızda sorgu performansı artar.  Bir tablo birleşimlerde kullanılmazsa, tabloyu yan tümcesindeki sık kullanılan bir sütuna dağıtmayı düşünün `GROUP BY` .
- *not* `WHERE` Yan tümcelerde kullanılmaz. Bu, sorguyu tüm dağıtımların çalıştırılmadığından daraltabilirdi.
- Bir tarih sütunu *değil* . WHERE yan tümceleri genellikle tarihe göre filtreleyerek.  Bu durumda, tüm işleme yalnızca birkaç dağıtımda çalıştırılabilir.

### <a name="what-to-do-when-none-of-the-columns-are-a-good-distribution-column"></a>Sütunlardan hiçbiri iyi bir dağıtım sütunu olmadığında yapmanız gerekenler

Sütunlarınızın hiçbirinde bir dağıtım sütunu için yeterli sayıda farklı değer yoksa, bir veya daha fazla değerin bileşik olarak yeni bir sütun oluşturabilirsiniz. Sorgu yürütme sırasında veri hareketini önlemek için, sorgularda bir JOIN sütunu olarak bileşik dağıtım sütununu kullanın.

Karma olarak dağıtılan bir tablo tasarladıktan sonra, sonraki adım tabloya veri yüklemek olur.  Yükleme Kılavuzu için bkz. [yüklemeye genel bakış](design-elt-data-loading.md).

## <a name="how-to-tell-if-your-distribution-column-is-a-good-choice"></a>Dağıtım sütunlarınızın iyi bir seçenek olup olmadığını nasıl söyleyeceğinizi

Veriler karma olarak dağıtılmış bir tabloya yüklendikten sonra, satırların 60 dağıtımları arasında ne kadar eşit dağıtıldığını görmek için işaretleyin. Dağıtım başına satır sayısı, performans üzerinde belirgin bir etki olmadan %10 ' a kadar farklılık gösterebilir.

### <a name="determine-if-the-table-has-data-skew"></a>Tabloda veri eğriliği olup olmadığını belirleme

Veri eğriliğini denetlemeye yönelik hızlı bir yol, [DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)kullanmaktır. Aşağıdaki SQL kodu, 60 dağıtımların her birinde depolanan tablo satırı sayısını döndürür. Dengeli performans için, dağıtılmış tablodaki satırların tüm dağıtımlarla eşit olarak yayılmalıdır.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Hangi tabloların %10 ' dan fazla veri eğriliğini olduğunu belirlemek için:

1. [Tablolara genel bakış](sql-data-warehouse-tables-overview.md#table-size-queries) makalesinde gösterilen dbo. vtablesize görünümünü oluşturun.  
2. Aşağıdaki sorguyu çalıştırın:

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having (max(row_count * 1.000) - min(row_count * 1.000))/max(row_count * 1.000) >= .10
    )
order by two_part_name, row_count
;
```

### <a name="check-query-plans-for-data-movement"></a>Veri taşıma için sorgu planlarını denetleme

İyi bir dağıtım sütunu, birleşimlerin ve toplamaların en az veri hareketine sahip olmasını sağlar. Bu, birleştirmeleri yazma yöntemini etkiler. Karma olarak dağıtılan iki tabloda bir birleşime yönelik en düşük veri hareketini almak için, JOIN sütunlarından birinin dağıtım sütunu olması gerekir.  Karma olarak dağıtılan iki tablo aynı veri türünde bir dağıtım sütununa katılılırsa, JOIN veri hareketi gerektirmez. Birleşimler, veri hareketini oluşturmadan ek sütunları kullanabilir.

Bir JOIN sırasında veri hareketini önlemek için:

- Birleşime dahil olan tabloların, birleşime katılan sütunlardan **birinde** karma dağıtılmış olması gerekir.
- JOIN sütunlarının veri türleri her iki tablo arasında aynı olmalıdır.
- Sütunlar bir Equals işleci ile birleştirilmelidir.
- JOIN türü bir olamaz `CROSS JOIN` .

Sorguların veri hareketi yaşamadığını görmek için sorgu planına bakabilirsiniz.  

## <a name="resolve-a-distribution-column-problem"></a>Dağıtım sütunu sorununu çözme

Veri eğçalışmasının tüm çalışmalarını çözümlemek gerekli değildir. Verilerin dağıtılması, en aza indirmeyi ve veri hareketini arasında doğru dengeyi bulmanın bir önemi. Veri eğriltme ve veri hareketini en aza indirmek her zaman mümkün değildir. Bazen en az veri hareketine sahip olmanın avantajı, veri eğriltinin etkilerini olumsuz etkileyebilir.

Bir tablodaki veri eğimlerini çözmeniz gerekip gerekmediğini belirlemek için iş yükünüzün veri birimleri ve sorguları hakkında mümkün olduğunca fazla bilgi almalısınız. Sorgu performansı üzerinde eğizlemenin etkilerini izlemek için [sorgu izleme](sql-data-warehouse-manage-monitor.md) makalesindeki adımları kullanabilirsiniz. Özellikle, bireysel dağıtımlarda ne kadar büyük bir sorgu tamamlandığını arayın.

Varolan bir tablodaki dağıtım sütununu değiştirebileceğinizden, veri eğriliğini çözecek tipik yol, tabloyu farklı bir dağıtım sütunuyla yeniden oluşturmaktır.  

### <a name="re-create-the-table-with-a-new-distribution-column"></a>Tabloyu yeni bir dağıtım sütunuyla yeniden oluşturun

Bu örnekte CREATE TABLE, farklı bir karma dağıtım sütunuyla bir tabloyu yeniden oluşturmak için [SEÇIM olarak](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanılmaktadır.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

## <a name="next-steps"></a>Sonraki adımlar

Dağıtılmış bir tablo oluşturmak için aşağıdaki deyimlerden birini kullanın:

- [CREATE TABLE (SYNAPSE SQL havuzu)](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [SELECT (SYNAPSE SQL Pool) olarak CREATE TABLE](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
