---
title: Çoğaltılan tablolar için tasarım kılavuzu
description: Synapse SQL'de çoğaltılan tabloların tasarlanması için öneriler
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/19/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 0b240c45afcb2374f41eb26e86e46b106e314e76
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80582237"
---
# <a name="design-guidance-for-using-replicated-tables-in-synapse-sql"></a>Synapse SQL'de çoğaltılan tabloları kullanmak için tasarım kılavuzu

Bu makalede, Synapse SQL şemanızda çoğaltılan tablolar tasarlamak için öneriler verebisi bulunmaktadır. Veri hareketi ve sorgu karmaşıklığını azaltarak sorgu performansını artırmak için bu önerileri kullanın.

> [!VIDEO https://www.youtube.com/embed/1VS_F37GI9U]

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, Synapse SQL'deki veri dağıtımı ve veri hareketi kavramlarına aşina olduğunuzu varsayar.Daha fazla bilgi için [mimari](massively-parallel-processing-mpp-architecture.md) makaleye bakın. 

Tablo tasarımının bir parçası olarak, verileriniz ve verilerinizin nasıl sorgulandırışları hakkında mümkün olduğunca çok bilgi edinin.Örneğin, şu soruları göz önünde bulundurun:

- Masa ne kadar büyük?   
- Tablo ne sıklıkta yenilenir?   
- Synapse SQL veritabanında gerçek ve boyut tabloları var mı?   

## <a name="what-is-a-replicated-table"></a>Çoğaltılmış tablo nedir?

Çoğaltılan tablo, her İşlem düğümünde erişilebilen tablonun tam kopyasına sahiptir. Tablo çoğaltıldığında bir birleştirme veya toplama öncesinde İşlem düğümleri arasında verileri aktarma gereksinimi ortadan kalkar. Tablobirden çok kopyaya sahip olduğundan, çoğaltılan tablolar tablo boyutu 2 GB'tan az sıkıştırıldığında en iyi şekilde çalışır.  2 GB sabit bir sınır değildir.  Veriler statikse ve değişmezse, daha büyük tabloları çoğaltabilirsiniz.

Aşağıdaki diyagram, her İşlem düğümünde erişilebilen çoğaltılmış bir tabloyu gösterir. Synapse SQL'de, çoğaltılan tablo her işlem düğümünde bir dağıtım veritabanına tam olarak kopyalanır. 

![Çoğaltılmış tablo](./media/design-guidance-for-replicated-tables/replicated-table.png "Çoğaltılmış tablo")  

Çoğaltılan tablolar yıldız şemasındaki boyut tabloları için iyi çalışır. Boyut tabloları genellikle boyut tablosundan farklı olarak dağıtılan olgu tablolarına birleştirilir.  Boyutlar genellikle birden çok kopyayı depolamayı ve korumayı mümkün kılan boyuttadır. Boyutlar, müşteri adı ve adresi ve ürün ayrıntıları gibi yavaş yavaş değişen açıklayıcı verileri depolar. Verilerin yavaş yavaş değişen yapısı, çoğaltılan tablonun daha az bakımını sağlar. 

Şu anda çoğaltılmış bir tablo kullanmayı düşünün:

- Diskteki tablo boyutu, satır sayısına bakılmaksızın 2 GB'dan küçüktür. Tablo boyutunu bulmak için [DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) komutunu kullanabilirsiniz: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- Tablo, aksi takdirde veri hareketi gerektiren birleştirmelerde kullanılır. Karma dağıtılmış tablo gibi aynı sütunda dağıtılmayan tabloları bir round-robin tablosuna birleştiriyorsak, sorguyu tamamlamak için veri hareketi gereklidir.  Tablolardan biri küçükse, çoğaltılan bir tablo düşünün. Çoğu durumda yuvarlanan tablolar yerine çoğaltılmış tablolar kullanmanızı öneririz. Sorgu planlarında veri hareketi işlemlerini görüntülemek için [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)kullanın.  BroadcastMoveOperation çoğaltılmış bir tablo kullanılarak ortadan kaldırılabilir tipik bir veri hareketi işlemidir.  
 
Çoğaltılan tablolar aşağıdaki anda en iyi sorgu performansını vermeyebilir:

- Tabloda sık sık ekleme, güncelleştirme ve silme işlemleri vardır.Veri işleme dili (DML) işlemleri, çoğaltılan tablonun yeniden oluşturulmasını gerektirir.Sık sık yeniden oluşturma daha yavaş performansa neden olabilir.
- Synapse SQL veritabanı sık sık ölçeklendirilir. Veritabanını ölçekleme, yinelenen tabloyu yeniden oluşturmaya neden olan işlem düğümlerinin sayısını değiştirir.
- Tabloda çok sayıda sütun vardır, ancak veri işlemleri genellikle yalnızca az sayıda sütuna erişir. Bu senaryoda, tablonun tamamını çoğaltmak yerine, tabloyu dağıtmak ve ardından sık erişilen sütunlarda bir dizin oluşturmak daha etkili olabilir. Bir sorgu veri hareketi gerektiriyorsa, yalnızca istenen sütunlar için veri taşınır.

## <a name="use-replicated-tables-with-simple-query-predicates"></a>Basit sorgu yüklemleri ile çoğaltılmış tabloları kullanma

Bir tablodağıtmayı veya çoğaltmayı seçmeden önce, tabloya karşı çalıştırmayı planladığınız sorgu türlerini düşünün. Mümkün olduğu

- Eşitlik veya eşitsizlik gibi basit sorgu yüklemlerine sahip sorgular için çoğaltılan tabloları kullanın.
- LIKE veya NOT LIKE gibi karmaşık sorgu yüklemleri olan sorgular için dağıtılmış tabloları kullanın.

CPU yoğun sorgular, çalışma tüm İşlem lezlerine dağıtıldığında en iyi performansı gösterir. Örneğin, bir tablonun her satırında hesaplamaları çalıştıran sorgular, yinelenen tablolara göre dağıtılmış tablolarda daha iyi performans gösterir. Çoğaltılan bir tablo her İşlemçi düğümünde tam olarak depolandığından, yinelenen tabloya karşı CPU yoğun bir sorgu, her İşlemci düğümündeki tüm tabloya karşı çalışır. Ek hesaplama sorgu performansını yavaşlatabilir.

Örneğin, bu sorgukarmaşık bir yüklem vardır.  Veriler çoğaltılmış bir tablo yerine dağıtılmış bir tabloda olduğunda daha hızlı çalışır. Bu örnekte, veriler round-robin dağıtılabilir.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'
       
```       
           
## <a name="convert-existing-round-robin-tables-to-replicated-tables"></a>Varolan yuvarlak robin tablolarını çoğaltılan tablolara dönüştürme
Zaten round-robin tabloları varsa, bu makalede özetlenen ölçütleri karşılıyorsa bunları çoğaltılmış tablolara dönüştürmenizi öneririz. Çoğaltılan tablolar, veri hareketi gereksinimini ortadan kaldırdığından, yuvarlanma tabloları üzerinden performansı artırır.  Bir yuvarlak-robin tablo her zaman birleştirmeler için veri hareketi gerektirir. 

Bu örnek, DimSalesTerritory tablosunu çoğaltılmış bir tabloyla değiştirmek için [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanır. Bu örnek, DimSalesTerritory'nin karma dağıtılmış mı yoksa yuvarlak-robin mi olduğuna bakılmaksızın çalışır.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
``` 
    
### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Yinelenen karşı round-robin için sorgu performans örneği 
    
Çoğaltılan tablo, her İşlemleme düğümünde zaten mevcut olduğundan, birleşimler için herhangi bir veri hareketi gerektirmez. Boyut tabloları round-robin dağıtılırsa, birleştirme boyut tablosunu her İşlemle'nin düğümüne tam olarak kopyalar. Verileri taşımak için sorgu planı BroadcastMoveOperation adlı bir işlem içerir. Bu tür veri hareketi işlemi sorgu performansını yavaşlatır ve çoğaltılan tablolar kullanılarak ortadan kaldırılır. Sorgu planı adımlarını görüntülemek için [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) sistem kataloğu görünümünü kullanın.  

Örneğin, AdventureWorks şemasına `FactInternetSales` karşı sorguyu izleyen tablo karma dağıtılır. Ve `DimDate` `DimSalesTerritory` tablolar daha küçük boyutlu tablolardır. Bu sorgu, 2004 mali yılı için Kuzey Amerika'daki toplam satışları döndürür:

```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
Biz yeniden `DimDate` oluşturduk ve `DimSalesTerritory` yuvarlak-robin tablolar olarak. Sonuç olarak, sorgu, birden çok yayın taşıma işlemi olan aşağıdaki sorgu planını gösterdi: 
 
![Round-robin sorgu planı](./media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

Yeniden oluşturduk `DimDate` `DimSalesTerritory` ve çoğaltılmış tablolar olarak, ve sorguyu yeniden çalıştırdık. Ortaya çıkan sorgu planı çok daha kısadır ve herhangi bir yayın hareketi yoktur.

![Çoğaltılan sorgu planı](./media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Çoğaltılan tabloları değiştirmek için performans hususları

Çoğaltılan tablo, tablonun ana sürümünü koruyarak uygulanır. Ana sürümü her İşlemçi düğümünde bir dağıtım veritabanına kopyalar. Bir değişiklik olduğunda, önce ana tablo güncelleştirilir. Sonra her İşlem düğümündeki tablo yeniden oluşturulur. Çoğaltılan tablonun yeniden oluşturulması, tabloyu her İşlem düğümüne kopyalamayı ve dizinleri oluşturmayı içerir.  Örneğin, Bir DW400 üzerinde çoğaltılan bir tablo verilerin 5 kopyası vardır.  Her İşlemçi düğümünde bir ana kopya ve tam bir kopya.  Tüm veriler, daha hızlı veri değişiklik deyimlerini ve esnek ölçekleme işlemlerini desteklemek için dağıtım veritabanlarında depolanır. 

Yeniden oluşturmalar sonra gereklidir:
- Veriler yüklenir veya değiştirilir
- Synapse SQL örneği farklı bir düzeye ölçeklendirilir
- Tablo tanımı güncelleştirildi

Daha sonra yeniden oluşturma lar gerekli değildir:
- İşlemi duraklatma
- Devam işlemi

Yeniden oluşturma, veriler değiştirildikten hemen sonra gerçekleşmez. Bunun yerine, yeniden oluşturma, bir sorgu tablodan ilk kez seçtiğinde tetiklenir.  Yeniden oluşturmayı tetikleyen sorgu, veriler her İşlemçi düğümüne eş senkronize olarak kopyalanırken tablonun ana sürümünden hemen okur. Veri kopyası tamamlanana kadar, sonraki sorgular tablonun ana sürümünü kullanmaya devam eder.  Başka bir yeniden oluşturmayı zorlayan çoğaltılan tabloya karşı herhangi bir etkinlik gerçekleşirse, veri kopyası geçersiz kılınır ve bir sonraki seçili deyim verileri yeniden kopyalanması için tetikler. 

### <a name="use-indexes-conservatively"></a>Dizinleri konservatif olarak kullanma

Standart dizin oluşturma uygulamaları çoğaltılan tablolar için geçerlidir. Çoğaltılan her tablo dizini, dizin yeniden oluşturmanın bir parçası olarak yeniden oluşturulur. Yalnızca performans kazancı dizinleri yeniden oluşturma maliyetinden ağır bastığında dizinleri kullanın.  
 
### <a name="batch-data-loads"></a>Toplu veri yükleri
Verileri çoğaltılan tablolara yüklerken, yükleri bir araya getirerek yeniden oluşturmayı en aza indirmeye çalışın. Seçili ifadeleri çalıştırmadan önce toplu tüm yükleri gerçekleştirin.

Örneğin, bu yük deseni dört kaynaktan veri yükler ve dört yeniden çağırır. 

        Load from source 1.
- Select deyimi yeniden 1 tetikler.
        Kaynak 2'den yük.
- Select deyimi yeniden 2 tetikler.
- Kaynaktan yük 3.
- Select deyimi yeniden 3 tetikler.
- Kaynak tan yük 4.
- Select deyimi tetikleyiciler yeniden 4.

Örneğin, bu yük deseni verileri dört kaynaktan yükler, ancak yalnızca bir yeniden oluşturma yı çağırır.

- Kaynak 1'den yük.
- Kaynak 2'den yük.
- Kaynaktan yük 3.
- Kaynak tan yük 4.
- İfadeyi seçin yeniden tetikler.

### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Toplu işlem yükünden sonra çoğaltılan tabloyu yeniden oluşturma

Tutarlı sorgu yürütme süreleri sağlamak için, toplu iş yükünden sonra çoğaltılan tabloların oluşturulmasını zorlamayı düşünün. Aksi takdirde, ilk sorgu yine de sorguyu tamamlamak için veri hareketini kullanır. 

Bu sorgu, değiştirilen ancak yeniden oluşturulmayan yinelenen tabloları listelemek için [sys.pdw_replicated_table_cache_state](/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) DMV'yi kullanır.

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
Yeniden oluşturmayı tetiklemek için, önceki çıktıdaki her tabloda aşağıdaki deyimi çalıştırın. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
```

## <a name="next-steps"></a>Sonraki adımlar

Çoğaltılan bir tablo oluşturmak için şu ifadelerden birini kullanın:

- [TABLO OLUŞTUR](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)
- [SEÇOLARAK TABLO OLUŞTUR](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

Dağıtılmış tablolara genel bakış için [dağıtılmış tablolara](sql-data-warehouse-tables-distribute.md)bakın.
