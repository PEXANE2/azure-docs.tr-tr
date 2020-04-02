---
title: Bellek içi örnek
description: OLTP ve columnstore örneğiyle Azure SQL Veritabanı Bellek İçi teknolojilerini deneyin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: ''
ms.date: 12/18/2018
ms.openlocfilehash: b707d67c88eb550d397134b2294c1c5b0e1f7f7d
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80528208"
---
# <a name="in-memory-sample"></a>Bellek içi örnek

Azure SQL Veritabanı'ndaki Bellek İçi teknolojiler, uygulamanızın performansını artırmanıza ve veritabanınızın maliyetini düşürmenize olanak tanır. Azure SQL Veritabanı'nda Bellek Içi teknolojileri kullanarak, çeşitli iş yükleriyle performans iyileştirmeleri elde edebilirsiniz.

Bu makalede, Azure SQL Veritabanı'nda Bellek Içi OLTP'nin yanı sıra sütun deposu dizinlerinin kullanımını gösteren iki örnek görürsünüz.

Daha fazla bilgi için bkz.
- [Bellek İçi OLTP Genel Bakış ve Kullanım Senaryoları](https://msdn.microsoft.com/library/mt774593.aspx) (müşteri vaka çalışmaları ve başlamak için bilgi referansları içerir)
- [Bellek IÇI OLTP için Dokümantasyon](https://msdn.microsoft.com/library/dn133186.aspx)
- [Columnstore Dizinler Kılavuzu](https://msdn.microsoft.com/library/gg492088.aspx)
- [Gerçek zamanlı operasyonel analitik](https://msdn.microsoft.com/library/dn817827.aspx) olarak da bilinen karma işlemsel/analitik işleme (HTAP)

<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a>1. Bellek IÇI OLTP örneğini yükleyin

[Azure portalında](https://portal.azure.com/)birkaç tıklamayla AdventureWorksLT örnek veritabanını oluşturabilirsiniz. Bu bölümdeki adımlar, AdventureWorksLT veritabanınızı Bellek Içi OLTP nesneleri ile nasıl zenginleştirebileceğinizi ve performans avantajları gösterebileceğinizi açıklar.

In-Memory OLTP için daha basit, ancak görsel olarak daha çekici bir performans demosu için bkz:

- Yayın: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Kaynak kodu: [in-memory-oltp-demo-kaynak kodu](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/in-memory-database)

#### <a name="installation-steps"></a>Yükleme adımları

1. Azure [portalında,](https://portal.azure.com/)bir sunucuda Premium veya İş Açısından Kritik veritabanı oluşturun. **Kaynağı** AdventureWorksLT örnek veritabanına ayarlayın. Ayrıntılı talimatlar için [bkz.](sql-database-single-database-get-started.md)

2. SQL Server Management Studio [(SSMS.exe)](https://msdn.microsoft.com/library/mt238290.aspx)ile veritabanına bağlanın.

3. Bellek [IÇI OLTP Transact-SQL komut dosyasını](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/sql_in-memory_oltp_sample.sql) panonuza kopyalayın. T-SQL komut dosyası, adım 1'de oluşturduğunuz AdventureWorksLT örnek veritabanında gerekli Bellek içi nesneleri oluşturur.

4. T-SQL komut dosyasını SSMS'e yapıştırın ve ardından komut dosyasını çalıştırın. CREATE `MEMORY_OPTIMIZED = ON` TABLE deyimleri çok önemlidir. Örnek:


```sql
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Hata 40536


T-SQL komut dosyasını çalıştırdığınızda hata 40536 alırsanız, veritabanının Bellek İçi'ni destekleyip desteklemediğini doğrulamak için aşağıdaki T-SQL komut dosyasını çalıştırın:


```sql
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


**0'ın** sonucu, Bellek Içi'nin desteklenmediğini ve **1'in** desteklenmediği anlamına gelir. Sorunu tanılamak için veritabanının Premium hizmet katmanında olduğundan emin olun.


#### <a name="about-the-created-memory-optimized-items"></a>Oluşturulan bellek için en iyi duruma getirilmiş öğeler hakkında

**Tablolar**: Örnekte bellek için en iyi duruma getirilmiş aşağıdaki tablolar yer almaktadır:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


SSMS'teki **Object Explorer** aracılığıyla bellek için optimize edilmiş tabloları inceleyebilirsiniz. Sağ tıkla **Tablolar** > **Filtre** > **Filtresi Ayarları** > **Bellek Optimize edilmiştir.** Değer 1'e eşittir.


Veya aşağıdakiler gibi katalog görünümlerini sorgulayabilirsiniz:


```sql
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Yerel olarak derlenen depoyordam**: SalesLT.usp_InsertSalesOrder_inmem'ı katalog görünümü sorgusu aracılığıyla inceleyebilirsiniz:


```sql
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a>Örnek OLTP iş yükünü çalıştırma

Aşağıdaki iki *depolanan yordamlar* arasındaki tek fark, ilk yordamın tabloların bellek için en iyi duruma getirilmiş sürümlerini kullanması, ikinci yordamın ise normal disk üzerindeki tabloları kullanmasıdır:

- SalesLT **.** usp_InsertSalesOrder **_inmem**
- SalesLT **.** usp_InsertSalesOrder **_ondisk**


Bu bölümde, stresli düzeylerde iki depolanan yordamları yürütmek için kullanışlı **ostress.exe** yardımcı programı nasıl kullanılacağını görmek. İki gerilim koşusunun tamamlanmasının ne kadar sürdüğünü karşılaştırabilirsiniz.


ostress.exe çalıştırdığınızda, aşağıdakilerin her ikisi için tasarlanmış parametre değerlerini geçmenizi öneririz:

- -n100 kullanarak çok sayıda eşzamanlı bağlantı çalıştırın.
- -r500 kullanarak her bağlantı halkası yüzlerce kez var.


Ancak, her şeyin çalıştığından emin olmak için -n10 ve -r50 gibi çok daha küçük değerlerle başlamak isteyebilirsiniz.


### <a name="script-for-ostressexe"></a>Ostress.exe için script


Bu bölümde, ostress.exe komut satırına gömülü Olan T-SQL komut dosyası görüntülenir. Komut dosyası, daha önce yüklediğiniz T-SQL komut dosyası tarafından oluşturulan öğeleri kullanır.


Aşağıdaki komut dosyası, beş satır öğesi içeren örnek bir satış siparişini aşağıdaki bellek için optimize edilmiş *tablolara*ekler:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```sql
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


ostress.exe için önceki T-SQL komut dosyasının *_ondisk* sürümünü yapmak için, *_inmem* alt dizesinin her iki oluşumunu da *_ondisk.* Bu değiştirmeler tabloların ve depolanan yordamların adlarını etkiler.


### <a name="install-rml-utilities-and-ostress"></a>RML yardımcı programları yükleyin ve`ostress`


İdeal olarak, ostress.exe'yi bir Azure sanal makinesinde (VM) çalıştırmayı planlıyorsunuz. AdventureWorksLT veritabanınızın bulunduğu aynı Azure coğrafi bölgesinde bir [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) oluşturursunuz. Ama onun yerine dizüstü bilgisayarında ostress.exe çalıştırabilirsiniz.


VM'de veya seçtiğiniz ana bilgisayarda Yeniden Biçimlendirme Dili (RML) yardımcı hizmetlerini yükleyin. Kamu hizmetleri ostress.exe içerir.

Daha fazla bilgi için bkz.
- [In-Memory OLTP için Örnek Veritabanı'nda](https://msdn.microsoft.com/library/mt465764.aspx)ostress.exe tartışma .
- [Bellek IÇI OLTP için Örnek Veritabanı.](https://msdn.microsoft.com/library/mt465764.aspx)
- [ostress.exe yüklemek için blog](https://blogs.msdn.com/b/psssql/archive/20../../cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(https://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(https://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-_inmem-stress-workload-first"></a>Önce *_inmem* stres yükünü çalıştırma


Ostress.exe komut satırımızı çalıştırmak için *RML Cmd İstem penceresi* kullanabilirsiniz. Komut satırı parametreleri `ostress` doğrudan:

- 100 bağlantıyı aynı anda çalıştırın (-n100).
- Her bağlantının T-SQL komut dosyasını 50 kez (-r50) çalıştırması gerekir.


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Önceki ostress.exe komut satırını çalıştırmak için:


1. Önceki çalıştırmalar tarafından eklenen tüm verileri silmek için SSMS'te aşağıdaki komutu çalıştırarak veritabanı veri içeriğini sıfırla:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. Önceki ostress.exe komut satırının metnini panonuza kopyalayın.

3. -S `<placeholders>` -U -P -d parametrelerini doğru gerçek değerlerle değiştirin.

4. Düzenlenen komut satırınızı RML Cmd penceresinde çalıştırın.


#### <a name="result-is-a-duration"></a>Sonuç bir süredir


`ostress.exe` Tamamlandığında, çalışma süresini RML Cmd penceresinde son çıktı satırı olarak yazar. Örneğin, daha kısa bir test çalışması yaklaşık 1,5 dakika sürdü:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-_ondisk-then-rerun"></a>Sıfırlama, *_ondisk*için yeniden ayarlama, sonra yeniden


*_inmem* çalışmasından sonucu aldıktan sonra, *_ondisk* çalıştırın için aşağıdaki adımları gerçekleştirin:


1. Önceki çalıştırmatarafından eklenen tüm verileri silmek için SSMS'te aşağıdaki komutu çalıştırarak veritabanını sıfırla:
   ```sql
   EXECUTE Demo.usp_DemoReset;
   ```

2. Tüm *_inmem* *_ondisk*değiştirmek için ostress.exe komut satırını düzenle.

3. İkinci kez ostress.exe rerun ve süre sonucu yakalamak.

4. Yine, veritabanını sıfırlama (büyük miktarda test verisi olabilecek verileri sorumlu bir şekilde sildim).


#### <a name="expected-comparison-results"></a>Beklenen karşılaştırma sonuçları

Bellek Içi testlerimiz, bu basit iş yükü için performansın dokuz `ostress` **kat** arttığını ve veritabanıyla aynı Azure bölgesinde bir Azure VM'de çalıştığını göstermiştir.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a>2. Bellek İçi Analiz örneğini yükleme


Bu bölümde, bir sütun deposu dizini kullanırken IO ve istatistik sonuçlarını geleneksel b-ağacı dizini ile karşılaştırın.


OLTP iş yüküyle ilgili gerçek zamanlı analizler için, kümeslenmiş olmayan bir sütun mağazası dizini kullanmak genellikle en iyisidir. Ayrıntılar için bkz: [Columnstore Indexes Açıklanan.](https://msdn.microsoft.com/library/gg492088.aspx)



### <a name="prepare-the-columnstore-analytics-test"></a>Sütun mağazası analiz testini hazırlama


1. Örnekten yeni bir AdventureWorksLT veritabanı oluşturmak için Azure portalını kullanın.
   - Tam olarak şu ismi kullan.
   - Herhangi bir Premium hizmet katmanı seçin.

2. sql_in [memory_analytics_sample](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/sql_in-memory_analytics_sample.sql) panonuza kopyalayın.
   - T-SQL komut dosyası, adım 1'de oluşturduğunuz AdventureWorksLT örnek veritabanında gerekli Bellek içi nesneleri oluşturur.
   - Komut dosyası Boyut tablosunu ve iki olgu tablosunu oluşturur. Aslında tablolar her biri 3,5 milyon satır ile doldurulur.
   - Komut dosyasının tamamlanması 15 dakika sürebilir.

3. T-SQL komut dosyasını SSMS'e yapıştırın ve ardından komut dosyasını çalıştırın. **CREATE INDEX** deyimindeki **COLUMNSTORE** anahtar kelimesi çok önemlidir:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. AdventureWorksLT'ü uyumluluk düzeyi 130'a ayarlayın:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Düzey 130, Bellek Içi özelliklerle doğrudan ilişkili değildir. Ancak düzey 130 genellikle 120'den daha hızlı sorgu performansı sağlar.


#### <a name="key-tables-and-columnstore-indexes"></a>Anahtar tabloları ve sütun deposu dizinleri


- Dbo. FactResellerSalesXL_CCI, *veri* düzeyinde gelişmiş sıkıştırma olan kümelenmiş sütun deposu dizini olan bir tablodur.

- Dbo. FactResellerSalesXL_PageCompressed, yalnızca *sayfa* düzeyinde sıkıştırılmış eşdeğer normal kümelenmiş dizine sahip bir tablodur.


#### <a name="key-queries-to-compare-the-columnstore-index"></a>Sütun deposu dizini karşılaştırmak için anahtar sorguları


Performans geliştirmelerini görmek için [çalıştırabileceğiniz birkaç T-SQL sorgu türü](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/clustered_columnstore_sample_queries.sql) vardır. T-SQL komut dosyasındaki adım 2'de, bu sorgu çiftine dikkat edin. Onlar sadece bir satırda farklı:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Kümelenmiş sütun mağaza dizini FactResellerSalesXL\_CCI tablosundadır.

Aşağıdaki T-SQL komut dosyası alıntısı, her tablonun sorgusu için IO ve TIME istatistiklerini yazdırır.


```sql
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a clustered columnstore index CCI
-- The comparison numbers are even more dramatic the larger the table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

P2 fiyatlandırma katmanına sahip bir veritabanında, geleneksel diziyle karşılaştırıldığında kümelenmiş sütun deposu dizini kullanarak bu sorguiçin performans kazancının yaklaşık dokuz katını bekleyebilirsiniz. P15 ile, sütun mağazası dizini kullanarak performans kazancının yaklaşık 57 katını bekleyebilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar

- [Quickstart 1: Daha hızlı T-SQL Performansı için Bellek içi OLTP Teknolojileri](https://msdn.microsoft.com/library/mt694156.aspx)

- [Varolan bir Azure SQL uygulamasında Bellek Içi OLTP'yi kullanma](sql-database-in-memory-oltp-migration.md)

- Bellek [IÇI OLTP için Bellek İçi OLTP depolamasını izleyin](sql-database-in-memory-oltp-monitoring.md)


## <a name="additional-resources"></a>Ek kaynaklar

#### <a name="deeper-information"></a>Daha derin bilgiler

- [SQL Veritabanı'nda In-Memory OLTP ile DTU'nun %70 oranında azaltılarken Quorum'un anahtar veritabanının iş yükünü nasıl ikikatına çıkardığını öğrenin](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [Azure SQL Veritabanı Blog Gönderisinde Bellek Içi OLTP](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Bellek IÇI OLTP hakkında bilgi edinin](https://msdn.microsoft.com/library/dn133186.aspx)

- [Sütun mağaza dizinleri hakkında bilgi edinin](https://msdn.microsoft.com/library/gg492088.aspx)

- [Gerçek zamanlı operasyonel analitik hakkında bilgi edinin](https://msdn.microsoft.com/library/dn817827.aspx)

- Bkz. [Ortak İş Yükü Desenleri ve Geçiş Hususları](https://msdn.microsoft.com/library/dn673538.aspx) (Bellek içi OLTP'nin sıklıkla önemli performans kazançları sağladığı iş yükü modellerini açıklar)

#### <a name="application-design"></a>Uygulama tasarımı

- [Bellek Içi OLTP (Bellek Içi Optimizasyon)](https://msdn.microsoft.com/library/dn133186.aspx)

- [Varolan bir Azure SQL uygulamasında Bellek Içi OLTP'yi kullanma](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Araçlar

- [Azure portalı](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Veri Araçları (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx)
