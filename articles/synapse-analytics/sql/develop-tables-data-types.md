---
title: Veri türlerini tanımlama
description: Synapse SQL'de tablo veri türlerini tanımlama önerileri.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 312c58a7df09ebe3e0bdf0749f902199723a331b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429011"
---
# <a name="table-data-types-in-synapse-sql"></a>Synapse SQL'de tablo veri türleri

Synapse SQL'de tablo veri türlerini tanımlama önerileri. 

## <a name="what-are-the-data-types"></a>Veri türleri nelerdir?

Synapse SQL en sık kullanılan veri türlerini destekler. Desteklenen veri türlerinin listesi için CREATE TABLE deyimindeki [veri türlerine](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) bakın. 

## <a name="minimize-row-length"></a>Satır uzunluğunu en aza indirin

Veri türlerinin boyutunu en aza indirmek satır uzunluğunu kısaltır ve bu da daha iyi sorgu performansı sağlar. Verileriniz için çalışan en küçük veri türünü kullanın.

- Büyük varsayılan uzunlukta karakter sütunları tanımlamaktan kaçının. Örneğin, en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın.
- Yalnızca VARCHAR'a ihtiyacınız olduğunda [NVARCHAR][NVARCHAR] kullanmaktan kaçının.
- Mümkün olduğunda, NVARCHAR(MAX) veya VARCHAR(MAX) yerine NVARCHAR(4000) veya VARCHAR(8000) kullanın.

> [!NOTE]
> SQL bilardo tablolarınızı yüklemek için PolyBase harici tabloları kullanıyorsanız, tablo satırının tanımlanan uzunluğu 1 MB'ı geçemez. Değişken uzunlukta veri içeren bir satır 1 MB'ı aştığında, satırı BCP ile yükleyebilirsiniz, ancak PolyBase ile yükleyemezsiniz.

## <a name="identify-unsupported-data-types"></a>Desteklenmeyen veri türlerini belirleme

Veritabanınızı başka bir SQL veritabanından geçiriyorsanız, Synapse SQL'de desteklenmeyen veri türleri ile karşılaşabilirsiniz. Varolan SQL şemanızda desteklenmeyen veri türlerini bulmak için bu sorguyunuzu kullanın.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>Desteklenmeyen veri türleri için geçici çözüm

Aşağıdaki liste, Synapse SQL'in desteklemediği veri türlerini gösterir ve desteklenmeyen veri türleri yerine kullanabileceğiniz alternatifler sunar.

| Desteklenmeyen veri türü | Geçici çözüm |
| --- | --- |
| [Geometri](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[Varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Coğrafya](/sql/t-sql/spatial-geography/spatial-types-geography) |[Varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(4000) |
| [Görüntü](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[Varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Metin](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [Sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |Sütunu güçlü bir şekilde yazılan birkaç sütuna bölün. |
| [Tablo](/sql/t-sql/data-types/table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |SQL havuzu kullanıyorsanız, geçici tablolara dönüştürebilirsiniz. SQL (önizleme) kullanıyorsanız, [CETAS](../sql/develop-tables-cetas.md)kullanarak depolama alanına veri depolamayı düşünebilirsiniz. |
| [timestamp](/sql/t-sql/data-types/date-and-time-types) |[Datetime2](/sql/t-sql/data-types/datetime2-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ve [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işlevini kullanmak için kodu yeniden çalışır. Yalnızca sabitler varsayılan olarak desteklenir, bu nedenle current_timestamp varsayılan kısıtlama olarak tanımlanamaz. Zaman damgası yazılan sütundan satır sürüm değerlerini geçirmeniz gerekiyorsa, NULL veya NULL satır sürüm değerleri için [IKILI](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(8) veya [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)(8) kullanın. |
| [Xml](/sql/t-sql/xml/xml-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |
| [kullanıcı tanımlı türü](/sql/relational-databases/native-client/features/using-user-defined-types) |Mümkün olduğunda yerel veri türüne geri dönüştürün. |
| varsayılan değerler | Varsayılan değerler yalnızca gerçek ve sabitleri destekler. |

## <a name="next-steps"></a>Sonraki adımlar

Tablo geliştirme hakkında daha fazla bilgi için [Tabloya Genel Bakış'a](develop-overview.md)bakın.
