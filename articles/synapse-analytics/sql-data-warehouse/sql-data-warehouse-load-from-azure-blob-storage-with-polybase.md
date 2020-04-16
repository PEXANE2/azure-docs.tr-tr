---
title: Contoso perakende verilerini Synapse SQL veri ambarına yükleyin
description: Contoso perakende verilerinden Synapse SQL'e iki tablo yüklemek için PolyBase ve T-SQL komutlarını kullanın.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 118653efc8829ac5ef6287bb36fb5595cff1147b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416120"
---
# <a name="load-contoso-retail-data-to-synapse-sql"></a>Contoso perakende verilerini Synapse SQL'e yükleyin 

Bu eğitimde, Contoso perakende verilerinden iki tabloyu Synapse SQL veri ambarına yüklemek için PolyBase ve T-SQL komutlarını kullanmayı öğrenirsiniz.

Bu eğitimde olacak:

1. PolyBase'i Azure blob depolamasından yükedecek şekilde yapılandırın
2. Genel verileri veritabanınıza yükleme
3. Yük bittikten sonra optimizasyonlar gerçekleştirin.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticiyi çalıştırmak için, synapse SQL veri ambarı olan bir Azure hesabına ihtiyacınız vardır. Veri ambarı sağlanmış değilseniz, [bkz.](create-data-warehouse-portal.md)

## <a name="configure-the-data-source"></a>Veri kaynağını yapılandırma

PolyBase, dış verilerin konumunu ve özniteliklerini tanımlamak için T-SQL dış nesneleri kullanır. Dış nesne tanımları Synapse SQL veri ambarınızda depolanır. Veriler harici olarak depolanır.

## <a name="create-a-credential"></a>Kimlik bilgisi oluşturma

Contoso genel verilerini yüklüyorsanız **bu adımı atlayın.** Zaten herkes tarafından erişilebildiğinden, herkese açık verilere güvenli erişime ihtiyacınız yoktur.

Bu öğreticiyi kendi verilerinizi yüklemek için bir şablon olarak kullanıyorsanız **bu adımı atlamayın.** Bir kimlik bilgisi aracılığıyla verilere erişmek için veritabanı kapsamında bir kimlik bilgisi oluşturmak için aşağıdaki komut dosyasını kullanın. Ardından, veri kaynağının konumunu tanımlarken kullanın.

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

## <a name="create-the-external-data-source"></a>Dış veri kaynağını oluşturma

Verilerin konumunu ve veri türünü depolamak için bu [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) komutunu kullanın.

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH
(  
    TYPE = Hadoop
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
);
```

> [!IMPORTANT]
> Azure blob depolama kaplarınızı herkese açık hale getirmeyi seçerseniz, veri sahibi olarak veri merkezinden ayrıldığında veri çıkış ücretleri için ücretlendirileceğinizi unutmayın.

## <a name="configure-the-data-format"></a>Veri biçimini yapılandırma

Veriler Azure blob depolama metin dosyalarında depolanır ve her alan bir sınırlayıcı ile ayrılır. SSMS'te, metin dosyalarındaki verilerin biçimini belirtmek için aşağıdaki CREATE EXTERNAL FILE FORMAT komutunu çalıştırın. Contoso verileri sıkıştırılmamış ve boru sınırlandırılmış.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-schema-for-the-external-tables"></a>Dış tablolar için şema oluşturma

Artık veri kaynağını ve dosya biçimini belirttiğinize göre, dış tablolar için şemayı oluşturmaya hazırsınız.

Contoso verilerini veritabanınızda depolamak için bir yer oluşturmak için bir şema oluşturun.

```sql
CREATE SCHEMA [asb]
GO
```

## <a name="create-the-external-tables"></a>Dış tabloları oluşturma

DimProduct ve FactOnlineSales dış tabloları oluşturmak için aşağıdaki komut dosyasını çalıştırın. Burada yaptığınız tek şey sütun adlarını ve veri türlerini tanımlamak ve bunları Azure blob depolama dosyalarının konumuna ve biçimine bağlamaktır. Tanım veri ambarında depolanır ve veriler hala Azure Depolama Blob'dadır.

**KONUM** parametresi, Azure Depolama Blob'undaki kök klasörün altındaki klasördür. Her tablo farklı bir klasördedir.

```sql
--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/'
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="load-the-data"></a>Verileri yükleme

Dış verilere erişmenin farklı yolları vardır.  Verileri doğrudan dış tablolardan sorgulayabilir, verileri veri ambarındaki yeni tablolara yükleyebilir veya varolan veri ambarı tablolarına dış veri ekleyebilirsiniz.  

### <a name="create-a-new-schema"></a>Yeni bir şema oluşturma

CTAS veri içeren yeni bir tablo oluşturur.  İlk olarak, contoso verileri için bir şema oluşturun.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="load-the-data-into-new-tables"></a>Verileri yeni tablolara yükleme

Azure blob depolamadan verileri veri ambarı tablosuna yüklemek için [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) deyimini kullanın. [CTAS](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) ile yükleme, oluşturduğunuz güçlü bir şekilde yazılan harici tablolardan yararlanır. Verileri yeni tablolara yüklemek için tablo başına bir CTAS deyimi kullanın.

CTAS yeni bir tablo oluşturur ve onu seçili bir deyimin sonuçlarıyla doldurur. CTAS, yeni tabloyu seçifadenin sonuçlarıyla aynı sütunlara ve veri türlerine sahip olacak şekilde tanımlar. Harici bir tablodaki tüm sütunları seçerseniz, yeni tablo dış tablodaki sütunların ve veri türlerinin bir kopyası olur.

Bu örnekte, karma dağıtılmış tablolar olarak hem boyut hem de olgu tablosunu oluştururuz.

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="track-the-load-progress"></a>Yük ilerlemesini izleme

Dinamik yönetim görünümlerini (DMV) kullanarak yükünüzün ilerlemesini izleyebilirsiniz.

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files,
    sum(s.bytes_processed)/1024/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="optimize-columnstore-compression"></a>Sütun deposu sıkıştırmayı optimize edin

Varsayılan olarak, Synapse SQL veri ambarı tabloyu kümelenmiş sütun deposu dizini olarak depolar. Yükleme tamamlandıktan sonra, bazı veri satırları sütun deposuna sıkıştırılmamış olabilir.  Bunun olmasının farklı nedenleri vardır. Daha fazla bilgi edinmek için [sütun mağazası dizinlerini yönet'e](sql-data-warehouse-tables-index.md)bakın.

Bir yükten sonra sorgu performansını ve sütun mağazası sıkıştırmasını en iyi duruma getirmek için, sütun deposu dizini tüm satırları sıkıştırmaya zorlamak için tabloyu yeniden oluşturun.

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Sütun mağazası dizinlerini koruma hakkında daha fazla bilgi için [sütun mağazası dizinlerini yönet](sql-data-warehouse-tables-index.md) makalesine bakın.

## <a name="optimize-statistics"></a>İstatistikleri optimize edin

Yüklemeden hemen sonra tek sütunlu istatistikler oluşturmak en iyisidir. Belirli sütunların sorgu yüklemlerinde olmayacağını biliyorsanız, bu sütunlar üzerinde istatistik oluşturmayı atlayabilirsiniz. Her sütunda tek sütunlu istatistikler oluşturursanız, tüm istatistikleri yeniden oluşturmak uzun sürebilir.

Her tablonun her sütununda tek sütunlu istatistikler oluşturmaya karar verirseniz, `prc_sqldw_create_stats` [istatistik](sql-data-warehouse-tables-statistics.md) makalesinde depolanan yordam kodu örneğini kullanabilirsiniz.

Aşağıdaki örnek, istatistik oluşturmak için iyi bir başlangıç noktasıdır. Boyut tablosundaki her sütunda ve olgu tablolarında her birleştirme sütununda tek sütunlu istatistikler oluşturur. Daha sonra diğer olgu tablosu sütunlarına her zaman tek veya çok sütunlu istatistikleri ekleyebilirsiniz.

```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Başarı kilidi!

Genel verileri veri ambarınıza başarıyla yüklediniz. Harika iş!

Artık verilerinizi keşfetmek için tabloları sorgulamaya başlayabilirsiniz. Marka başına toplam satışları bulmak için aşağıdaki sorguyu çalıştırın:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Sonraki adımlar

Tam veri kümesini yüklemek için, microsoft SQL Server örnek deposundan [contoso perakende veri ambarına tam contoso perakende veri ambarını yükleyin.](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md)
Daha fazla geliştirme ipucu için, [veri ambarları için Tasarım kararları ve kodlama tekniklerine](sql-data-warehouse-overview-develop.md)bakın.
