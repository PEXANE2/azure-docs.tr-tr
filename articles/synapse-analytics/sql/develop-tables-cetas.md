---
title: CETAS in Synapse SQL
description: Synapse SQL ile CETAS kullanma
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 71bc20680467d270436e28190bb49db5b9313ca0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424030"
---
# <a name="cetas-with-synapse-sql"></a>Synapse SQL ile CETAS

Sql havuzunda veya isteğe bağlı SQL'de (önizleme), aşağıdaki görevleri tamamlamak için CREATE EXTERNAL TABLE AS SELECT (CETAS) seçeneğini kullanabilirsiniz:  

- Harici tablo oluşturma
- Dışa aktarma, buna paralel olarak, bir Transact-SQL SELECT deyiminin sonuçlarını

  - Hadoop
  - Azure Depolama Blob
  - Azure Data Lake Storage Gen2

## <a name="cetas-in-sql-pool"></a>CETAS SQL havuzunda

SQL havuzu, CETAS kullanımı ve sözdizimi için SELECT makalesi [olarak CREATE EXTERNAL TABLE'ı](/sql/t-sql/statements/create-external-table-as-select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işaretleyin. Ayrıca, SQL havuzunu kullanarak CTAS'da kılavuzluk için [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) makalesine bakın.

## <a name="cetas-in-sql-on-demand"></a>CETAS SQL on-demand

CETAS, isteğe bağlı SQL kaynağını kullanırken harici bir tablo oluşturmak ve sorgu sonuçlarını Azure Depolama Blob'una veya Azure Veri Gölü Depolama Gen2'ye aktarmak için kullanılır.

## <a name="syntax"></a>Sözdizimi

```syntaxsql
CREATE EXTERNAL TABLE [ [database_name  . [ schema_name ] . ] | schema_name . ] table_name
    WITH (
        LOCATION = 'path_to_folder',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name  
)
    AS <select_statement>  
[;]

<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT <select_criteria>
```

## <a name="arguments"></a>Bağımsız Değişkenler

*[ [ *database_name* . [ *schema_name* ] . ] | *schema_name* . ] *table_name**

Oluşturmak için tablonun bir ila üç bölümlü adı. Harici bir tablo için, SQL isteğe bağlı olarak yalnızca tablo meta verilerini depolar. Hiçbir gerçek veri SQL'de isteğe bağlı olarak taşınamaz veya depolanır.

YER = *'path_to_folder'*

SELECT deyiminin sonuçlarının dış veri kaynağına nerede yazAcağını belirtir. Kök klasör, dış veri kaynağında belirtilen veri konumudur. KONUM bir klasörü işaret etmeli ve bir iz /. Örnek: aggregated_data/

DATA_SOURCE = *external_data_source_name*

Dış verilerin depolandığı konumu içeren dış veri kaynağı nesnesinin adını belirtir. Harici bir veri kaynağı oluşturmak için [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](develop-tables-external-tables.md#create-external-data-source)kullanın.

FILE_FORMAT = *external_file_format_name*

Dış veri dosyasının biçimini içeren dış dosya biçimi nesnesinin adını belirtir. Harici dosya biçimi oluşturmak için [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](develop-tables-external-tables.md#create-external-file-format)kullanın. Şu anda yalnızca FORMAT='PARKE' ile harici dosya biçimleri desteklenmektedir.

*<common_table_expression>*

Ortak tablo ifadesi (CTE) olarak bilinen geçici adlandırılmış sonuç kümesini belirtir. Daha fazla bilgi için [WITH common_table_expression (Transact-SQL)](/sql/t-sql/queries/with-common-table-expression-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)bakın.

<select_criteria> SEÇIN

Yeni tabloyu SELECT deyiminin sonuçlarıyla doldurur. *select_criteria,* yeni tabloya hangi verilerin kopyalanamasını belirleyen SELECT deyiminin gövdesidir. SELECT deyimleri hakkında bilgi için [SELECT (Transact-SQL)](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)'ye bakın.

## <a name="permissions"></a>İzinler

Klasör içeriğini listelemek ve CETAS'ın çalışması için KONUM klasörüne yazma izniniz olması gerekir.

## <a name="examples"></a>Örnekler

Bu örnekler, yıllara göre toplanan toplam popülasyonu kaydetmek ve population_ds veri kaynağında bulunan aggregated_data klasörüne durumu kaydetmek için CETAS'ı kullanır.

Bu örnek, daha önce oluşturulan kimlik bilgisine, veri kaynağına ve dış dosya biçimine dayanır. [Dış tablolar](develop-tables-external-tables.md) belgesine bakın. Sorgu sonuçlarını aynı veri kaynağında farklı bir klasöre kaydetmek için KONUM bağımsız değişkenini değiştirin. Sonuçları farklı bir depolama hesabına kaydetmek için, DATA_SOURCE bağımsız değişken için farklı bir veri kaynağı oluşturun ve kullanın.

> [!NOTE]
> İzleyen örnekler, herkese açık bir Azure Açık Veri depolama hesabı kullanır. Salt okunur. Bu sorguları yürütmek için, yazma izinleri olan veri kaynağını sağlamanız gerekir.

```sql
-- use CETAS to export select statement with OPENROWSET result to  storage
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM
    OPENROWSET(BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=*/*.parquet',
    FORMAT='PARQUET') AS [r]
GROUP BY decennialTime, stateName
GO

-- you can query created external table
SELECT * FROM population_by_year_state
```

Aşağıdaki örnekte CETAS'ın kaynağı olarak harici bir tablo kullanınız. Daha önce oluşturulan kimlik bilgisine, veri kaynağına, dış dosya biçimine ve dış tabloya dayanır. [Dış tablolar](develop-tables-external-tables.md) belgesine bakın.

```sql
-- use CETAS with select from external table
CREATE EXTERNAL TABLE population_by_year_state
WITH (
    LOCATION = 'aggregated_data/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)  
AS
SELECT decennialTime, stateName, SUM(population) AS population
FROM census_external_table
GROUP BY decennialTime, stateName
GO

-- you can query created external table
SELECT * FROM population_by_year_state
```

## <a name="supported-data-types"></a>Desteklenen veri türleri

CETAS, aşağıdaki SQL veri türleri ile sonuç kümelerini depolamak için kullanılabilir:

- ikili
- Varbinary
- char
- varchar
- date
- time
- datetime2
- decimal
- sayısal
- float
- gerçek
- bigint
- int
- smallint
- tinyint
- bit

LOB'lar CETAS ile kullanılamaz.

Aşağıdaki veri türleri CETAS'ın SELECT bölümünde kullanılamaz:

- Nchar
- nvarchar
- datetime
- Smalldatetime
- Datetimeoffset
- Para
- Smallmoney
- uniqueidentifier

## <a name="next-steps"></a>Sonraki adımlar

[Spark tablolarını](develop-storage-files-spark-tables.md)sorgulamayı deneyebilirsiniz.
