---
title: SYNAPSE SQL havuzundaki tablo veri türleri
description: SYNAPSE SQL havuzunda tablo veri türlerini tanımlamaya yönelik öneriler.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 01/06/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 8118329564bbf8193797be0db619f821c064429a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089167"
---
# <a name="table-data-types-in-synapse-sql-pool"></a>SYNAPSE SQL havuzundaki tablo veri türleri

Bu makaleye dahil, SQL havuzundaki tablo veri türlerini tanımlamaya yönelik önerilerdir.

## <a name="supported-data-types"></a>Desteklenen veri türleri

SQL havuzu en yaygın kullanılan veri türlerini destekler. Desteklenen veri türlerinin bir listesi için, bkz. CREATE TABLE deyimindeki [veri türleri](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest#DataTypes) .

## <a name="minimize-row-length"></a>Satır uzunluğunu Küçült

Veri türlerinin boyutunu en aza indirmek, satır uzunluğunun kısaltaştırarak daha iyi sorgu performansına yol açar. Verileriniz için en küçük veri türünü kullanın.

- Büyük bir varsayılan uzunluğa sahip karakter sütunları tanımlamaktan kaçının. Örneğin, en uzun değer 25 karakter ise sütununuzu VARCHAR (25) olarak tanımlayın.
- Yalnızca VARCHAR gerektiren bir [nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanmaktan kaçının.
- Mümkün olduğunda, NVARCHAR (MAX) veya VARCHAR (MAX) yerine NVARCHAR (4000) veya VARCHAR (8000) kullanın.

Tablolarınızı yüklemek için PolyBase dış tabloları kullanıyorsanız, tablo satırının tanımlanan uzunluğu 1 MB 'ı aşamaz. Değişken uzunluklu verileri olan bir satır 1 MB 'yi aştığında satırı BCP ile yükleyebilirsiniz ancak PolyBase ile birlikte kullanamazsınız.

## <a name="identify-unsupported-data-types"></a>Desteklenmeyen veri türlerini tanımla

Veritabanınızı başka bir SQL veritabanından geçiriyorsanız, SQL havuzunda desteklenmeyen veri türleri bulabilirsiniz. Mevcut SQL şemanızda desteklenmeyen veri türlerini saptamak için aşağıdaki sorguyu kullanın:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>Desteklenmeyen veri türleri için geçici çözümler

Aşağıdaki listede SQL havuzunun desteklemediği veri türleri ve desteklenmeyen veri türleri için yararlı alternatifler verilmiştir.

| Desteklenmeyen veri türü | Geçici çözüm |
| --- | --- |
| [geometrisi](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[ikili](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Coğrafya](/sql/t-sql/spatial-geography/spatial-types-geography) |[ikili](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)(4000) |
| [görüntüyle](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[ikili](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [metinleri](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [n](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Sütunu kesin olarak belirlenmiş birkaç sütuna Böl. |
| [tablosundan](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Geçici tablolara Dönüştür. |
| [ilişkin](/sql/t-sql/data-types/date-and-time-types) |[Datetime2](/sql/t-sql/data-types/datetime2-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ve [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) işlevini kullanmak için yeniden çalışma kodu. Varsayılan olarak yalnızca sabitler desteklenir, bu nedenle current_timestamp varsayılan kısıtlama olarak tanımlanamaz. Zaman damgası türünde bir sütundan satır sürümü değerlerini geçirmeniz gerekiyorsa, NULL veya NULL satır sürüm değerleri için [BINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)(8) veya [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)(8) kullanın. |
| ['sini](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Kullanıcı tanımlı tür](/sql/relational-databases/native-client/features/using-user-defined-types?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) |Mümkün olduğunda yerel veri türüne geri dönüştür. |
| Varsayılan değerler | Varsayılan değerler yalnızca sabit değerleri ve sabitleri destekler. |

## <a name="next-steps"></a>Sonraki adımlar

Tablo geliştirme hakkında daha fazla bilgi için bkz. [tabloya genel bakış](sql-data-warehouse-tables-overview.md).
