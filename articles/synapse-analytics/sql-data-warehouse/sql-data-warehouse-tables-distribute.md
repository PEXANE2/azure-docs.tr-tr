---
title: Dağıtılmış tablolar tasarım kılavuzu
description: Synapse SQL havuzunda karma dağıtılmış ve yuvarlak robin dağıtılmış tablolar tasarlamaönerileri.
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
ms.openlocfilehash: 04255fb6fdf83e7249fad01c75425943b580393c
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80742872"
---
# <a name="guidance-for-designing-distributed-tables-in-synapse-sql-pool"></a>Synapse SQL havuzunda dağıtılmış tablolar tasarlama kılavuzu

Synapse SQL havuzlarında karma dağıtılmış ve yuvarlak robin dağıtılmış tablolar tasarlamaönerileri.

Bu makalede, Synapse SQL havuzunda veri dağıtımı ve veri hareketi kavramları aşina olduğunuzu varsayar.Daha fazla bilgi için Azure [Synapse Analytics'e büyük ölçüde paralel işleme (MPP) mimarisi](massively-parallel-processing-mpp-architecture.md)bölümüne bakın.

## <a name="what-is-a-distributed-table"></a>Dağıtılmış tablo nedir?

Dağıtılmış bir tablo tek bir tablo olarak görünür, ancak satırlar gerçekte 60 dağıtım arasında depolanır. Satırlar karma veya round-robin algoritması ile dağıtılır.  

**Karma dağıtılmış tablolar,** büyük olgu tablolarında sorgu performansını artırır ve bu makalenin odak noktasıdır. **Round-robin tablolar** yükleme hızını artırmak için yararlıdır. Bu tasarım seçenekleri, sorgu ve yükleme performansını artırmada önemli bir etkiye sahiptir.

Başka bir tablo depolama seçeneği, tüm İşlem düğümleri arasında küçük bir tablo çoğaltmaktır. Daha fazla bilgi için, [çoğaltılan tablolar için Tasarım kılavuzuna](design-guidance-for-replicated-tables.md)bakın. Üç seçenek arasından hızlı bir şekilde seçim yapmak için [tablolara genel bakışta](sql-data-warehouse-tables-overview.md)Dağıtılmış tablolara bakın.

Tablo tasarımının bir parçası olarak, verileriniz ve verilerinizin nasıl sorgulandırışları hakkında mümkün olduğunca çok bilgi edinin.Örneğin, şu soruları göz önünde bulundurun:

- Masa ne kadar büyük?
- Tablo ne sıklıkta yenilenir?
- Synapse SQL havuzunda gerçek ve boyut tabloları var mı?

### <a name="hash-distributed"></a>Karma dağıtıldı

Karma dağıtılmış bir tablo, her satırı bir [dağıtıma](massively-parallel-processing-mpp-architecture.md#distributions)atamak için deterministik karma işlevi kullanarak Tablo satırlarını İşlem düğümleri arasında dağıtır.

![Dağıtılmış tablo](./media/sql-data-warehouse-tables-distribute/hash-distributed-table.png "Dağıtılmış tablo")  

Aynı değerler her zaman aynı dağıtıma göre karma olduğundan, veri ambarı satır konumları hakkında yerleşik bilgiye sahiptir. Synapse SQL havuzunda bu bilgi sorgu sırasında veri hareketini en aza indirmek için kullanılır ve bu da sorgu performansını artırır.

Haşhaş dağıtılan tablolar, yıldız şemasındaki büyük gerçek tablolar için iyi çalışır. Onlar satır çok sayıda olabilir ve hala yüksek performans elde. Elbette, dağıtılmış sistemin sağlamak üzere tasarladığı performansı elde etmenize yardımcı olan bazı tasarım konuları vardır. İyi bir dağıtım sütunu seçmek, bu makalede açıklanan bu tür bir husustur.

Şu anda karma dağıtılmış bir tablo kullanmayı düşünün:

- Diskteki tablo boyutu 2 GB'tan fazladır.
- Tabloda sık sık ekleme, güncelleştirme ve silme işlemleri vardır.

### <a name="round-robin-distributed"></a>Round-robin dağıtıldı

Round-robin dağıtılmış tablo tüm dağıtımlar arasında eşit tablo satırları dağıtır. Satırların dağılımlara atanması rasgeledir. Karma dağıtılmış tabloların aksine, eşit değerlere sahip satırların aynı dağıtıma atanması garanti edilmez.

Sonuç olarak, sistem bazen bir sorgu çözebilir önce verilerinizi daha iyi düzenlemek için bir veri hareketi işlemi çağırmak gerekir.  Bu ek adım sorgularınızı yavaşlatabilir. Örneğin, bir round-robin tablosuna katılmak genellikle bir performans isabet satırları, reshuffling gerektirir.

Aşağıdaki senaryolarda tablonuz için round-robin dağılımını kullanmayı düşünün:

- Varsayılan olduğu için basit bir başlangıç noktası olarak başlarken
- Bariz bir birleştirme anahtarı yoksa
- Tablodağıtmak için karma için iyi bir aday sütunu yoksa
- Tablo diğer tablolarla ortak bir birleştirme anahtarını paylaşmazsa
- Birleştirme sorgudaki diğer birleştirmelerden daha az önemliyse
- Tablo geçici bir evreleme tablosu olduğunda

Öğretici [Load New York taksi verileri,](load-data-from-azure-blob-storage-using-polybase.md#load-the-data-into-your-data-warehouse) verileri yuvarlak-robin evreleme tablosuna yüklemenin bir örneğini verir.

## <a name="choosing-a-distribution-column"></a>Dağıtım sütunu seçme

Karma dağıtılmış bir tabloda karma anahtar olan bir dağıtım sütunu vardır. Örneğin, aşağıdaki kod, dağıtım sütunu olarak ProductKey ile karma dağıtılmış bir tablo oluşturur.

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

Bu sütundaki değerler satırların nasıl dağıtılacağını belirlediğinden, dağıtım sütunu seçmek önemli bir tasarım kararıdır. En iyi seçim çeşitli faktörlere bağlıdır ve genellikle tradeoffs içerir. Ancak, en iyi sütunu ilk kez seçmezseniz, tabloyu farklı bir dağıtım sütunuyla yeniden oluşturmak için [CREATE TABLE AS SELECT (CTAS)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) 'yi kullanabilirsiniz.

### <a name="choose-a-distribution-column-that-does-not-require-updates"></a>Güncelleştirme gerektirmeyen bir dağıtım sütunu seçin

Satırı silip güncelleştirilmiş değerlere yeni bir satır eklemediğiniz sürece dağıtım sütununa güncelleştiremezsiniz. Bu nedenle, statik değerleri olan bir sütun seçin.

### <a name="choose-a-distribution-column-with-data-that-distributes-evenly"></a>Eşit dağıtan verileri içeren bir dağıtım sütunu seçin

En iyi performans için, tüm dağıtımların yaklaşık olarak aynı sayıda satıra sahip olması gerekir. Bir veya daha fazla dağıtım orantısız sayıda satıra sahipse, bazı dağıtımlar paralel sorgunun kendi bölümünü diğerlerinden önce tamamlar. Tüm dağıtımlar işleme tamamlanana kadar sorgu tamamlanamadığından, her sorgu yalnızca en yavaş dağıtım kadar hızlıdır.

- Veri eğriltme, verilerin dağıtımlar arasında eşit olarak dağıtılmaması anlamına gelir
- Eğriltme işlemi, bazı dağıtımların paralel sorguları çalıştırırken diğerlerinden daha uzun sürdüğü anlamına gelir. Veriler eğriltildiğinde bu durum olabilir.
  
Paralel işlemedengelemek için, aşağıdakileri içeren bir dağıtım sütunu seçin:

- **Birçok benzersiz değerlere sahiptir.** Sütunun bazı yinelenen değerleri olabilir. Ancak, aynı değere sahip tüm satırlar aynı dağıtıma atanır. 60 dağıtım olduğundan, sütunun en az 60 benzersiz değeri olmalıdır.  Genellikle benzersiz değerlerin sayısı çok daha fazladır.
- **NULLs yok, ya da sadece birkaç NULLs vardır.** Ekstrem bir örnek için, sütundaki tüm değerler NULL ise, tüm satırlar aynı dağıtıma atanır. Sonuç olarak, sorgu işleme tek bir dağıtıma eğriltilir ve paralel işlemeden yararlanamaz.
- **Tarih sütunu değil.** Aynı tarihe ait tüm veriler aynı dağıtıma düşer. Birkaç kullanıcı aynı tarihte filtreleme yapıyorsa, tüm işleme işlerini 60 dağıtımdan yalnızca 1'i yapar.

### <a name="choose-a-distribution-column-that-minimizes-data-movement"></a>Veri hareketini en aza indiren bir dağıtım sütunu seçin

Doğru sorgu sonucunu almak için sorgular verileri bir İşlem düğümünden diğerine taşıyabilir. Veri hareketi genellikle sorgular dağıtılmış tablolarda birleşince ve toplandıklarında gerçekleşir. Veri hareketini en aza indirmeye yardımcı olan bir dağıtım sütunu seçmek, Synapse SQL havuzunuzun performansını optimize etmek için en önemli stratejilerden biridir.

Veri hareketini en aza indirmek için, şu larına sahip bir dağıtım sütunu seçin:

- `JOIN`, `GROUP BY`, `DISTINCT`, `OVER`ve `HAVING` yan tümcelerinde kullanılır. İki büyük olgu tablosunda sık sık birleşim olduğunda, her iki tabloyu birleştirme sütunlarından birine dağıttığınızda sorgu performansı artar.  Birleştirmelerde bir tablo kullanılmadığında, tabloyu yan tümcede sık kullanılan `GROUP BY` bir sütunda dağıtmayı düşünün.
- Yan tümcelerde `WHERE` *kullanılmaz.* Bu, sorguyu tüm dağıtımlarda çalışmamak için daraltabilir.
- Tarih sütunu *değildir.* WHERE yan tümceleri genellikle tarihe göre filtre.  Bu durumda, tüm işleme yalnızca birkaç dağıtım üzerinde çalıştırılabilir.

### <a name="what-to-do-when-none-of-the-columns-are-a-good-distribution-column"></a>Sütunların hiçbiri iyi bir dağıtım sütunu olmadığında ne yapmalı?

Sütunlarınızın hiçbirinde bir dağıtım sütunu için yeterli farklı değerler yoksa, bir veya daha fazla değerin bileşimi olarak yeni bir sütun oluşturabilirsiniz. Sorgu yürütme sırasında veri hareketinden kaçınmak için, sorgularda birleştirme sütunu olarak bileşik dağıtım sütunu kullanın.

Karma dağıtılmış bir tablo tasarladıktan sonra, bir sonraki adım tabloya veri yüklemektir.  Yükleme kılavuzu için [yüklemeye genel bakış'a](design-elt-data-loading.md)bakın.

## <a name="how-to-tell-if-your-distribution-column-is-a-good-choice"></a>Dağıtım sütununuzun iyi bir seçim olup olmadığını nasıl anlayabilirsiniz?

Veriler karma dağıtılmış bir tabloya yüklendikten sonra, satırların 60 dağıtıma nasıl eşit dağıtıldığını kontrol edin. Dağıtım başına satırlar performans üzerinde gözle görülür bir etki olmadan %10'a kadar değişebilir.

### <a name="determine-if-the-table-has-data-skew"></a>Tabloda veri eğrilimi olup olmadığını belirleme

Veri çarpıklık kontrol etmek için hızlı bir yolu [PDW_SHOWSPACEUSED DBCC](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)kullanmaktır. Aşağıdaki SQL kodu, 60 dağıtımın her birinde depolanan tablo satırlarının sayısını döndürür. Dengeli performans için, dağıtılmış tablonuzdaki satırlar tüm dağıtımlara eşit olarak dağıtılmalıdır.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Hangi tabloların %10'dan fazla veri eğrilisi olduğunu belirlemek için:

1. [Tablolar genel bakış](sql-data-warehouse-tables-overview.md#table-size-queries) makalesinde gösterilen görünüm dbo.vTableSizes'ı oluşturun.  
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

### <a name="check-query-plans-for-data-movement"></a>Veri hareketi için sorgu planlarını denetleme

İyi bir dağıtım sütunu birleştirmelerin ve toplamaların en az veri hareketine sahip olmasını sağlar. Bu, birleştirmelerin yazılması nı etkiler. Karma dağıtılmış iki tabloda birbirletmek için en az veri hareketi elde etmek için, birleştirme sütunlarından birinin dağıtım sütunu olması gerekir.  Karma dağıtılmış iki tablo aynı veri türündeki bir dağıtım sütununda birleştiğinde, birleştirme veri hareketi gerektirmez. Birleştirmeler, veri hareketine maruz kalmadan ek sütunlar kullanabilir.

Birleştirme sırasında veri hareketinden kaçınmak için:

- Birleştirmeile ilgili tablolar, birleştirme de katılan **sütunlardan birinde** karma dağıtılmalıdır.
- Birleştirme sütunlarının veri türleri her iki tablo arasında eşleşmelidir.
- Sütunlar eşit bir işleçle birleştirilmelidir.
- Birleştirme türü bir `CROSS JOIN`.

Sorguların veri hareketi yle karşılaşıp yaşanıp yaşanıp yaşanmamasını görmek için sorgu planına bakabilirsiniz.  

## <a name="resolve-a-distribution-column-problem"></a>Dağıtım sütunu sorununu çözme

Tüm veri eğrilme durumlarını çözmek gerekli değildir. Veri dağıtmak, veri eğriliğini en aza indirmek ile veri hareketi arasındaki doğru dengeyi bulma meselesidir. Hem veri eğriliğini hem de veri hareketini en aza indirmek her zaman mümkün değildir. Bazen en az veri hareketine sahip olmanın yararı, veri çarpıklığı olan etkisinden daha ağır basabilir.

Bir tablodaki veri eğriliğini çözmeniz gerekip gerekmeden karar vermek için, iş yüklerinizdeki veri birimleri ve sorgular hakkında mümkün olduğunca çok şey anlamanız gerekir. Eğrilme'nin sorgu performansı üzerindeki etkisini izlemek için [Sorgu izleme](sql-data-warehouse-manage-monitor.md) makalesindeki adımları kullanabilirsiniz. Özellikle, tek tek dağıtımlarda büyük sorguların tamamlanmasının ne kadar sürdüğünü görün.

Varolan bir tablodaki dağıtım sütununu değiştiremeyeceğiniz için, veri eğriliğini çözmenin tipik yolu tabloyu farklı bir dağıtım sütunuyla yeniden oluşturmaktır.  

### <a name="re-create-the-table-with-a-new-distribution-column"></a>Tabloyu yeni bir dağıtım sütunuyla yeniden oluşturma

Bu örnek, farklı karma dağıtım sütununa sahip bir tabloyu yeniden oluşturmak için [CREATE TABLE AS SELECT'i](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanır.

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

Dağıtılmış bir tablo oluşturmak için şu ifadelerden birini kullanın:

- [CREATE TABLE (Synapse SQL havuzu)](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [SELECT OLARAK TABLO OLUŞTUR (Synapse SQL havuzu)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
