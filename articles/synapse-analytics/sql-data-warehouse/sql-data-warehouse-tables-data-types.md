---
title: Veri türlerini tanımlama
description: Synapse SQL havuzunda tablo veri türlerini tanımlama önerileri.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 5b3d24232c26bb4d483d360eb593bc361190ccfe
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631254"
---
# <a name="table-data-types-in-synapse-sql-pool"></a>Synapse SQL havuzunda tablo veri türleri
Bu makalede, SQL havuzunda tablo veri türlerini tanımlamak için öneriler yer alıyor. 

## <a name="supported-data-types"></a>Desteklenen veri türleri

SQL havuzu en sık kullanılan veri türlerini destekler. Desteklenen veri türlerinin listesi için CREATE TABLE deyimindeki [veri türlerine](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) bakın. 

## <a name="minimize-row-length"></a>Satır uzunluğunu en aza indirin
Veri türlerinin boyutunu en aza indirmek satır uzunluğunu kısaltır ve bu da daha iyi sorgu performansı sağlar. Verileriniz için çalışan en küçük veri türünü kullanın. 

- Büyük varsayılan uzunlukta karakter sütunları tanımlamaktan kaçının. Örneğin, en uzun değer 25 karakterse, sütununuzu VARCHAR(25) olarak tanımlayın. 
- Yalnızca VARCHAR'a ihtiyacınız olduğunda [NVARCHAR](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql) kullanmaktan kaçının.
- Mümkün olduğunda, NVARCHAR(MAX) veya VARCHAR(MAX) yerine NVARCHAR(4000) veya VARCHAR(8000) kullanın.

Tablolarınızı yüklemek için PolyBase harici tabloları kullanıyorsanız, tablo satırının tanımlanan uzunluğu 1 MB'ı geçemez. Değişken uzunlukta veri içeren bir satır 1 MB'ı aştığında, satırı BCP ile yükleyebilirsiniz, ancak PolyBase ile yükleyemezsiniz.

## <a name="identify-unsupported-data-types"></a>Desteklenmeyen veri türlerini belirleme
Veritabanınızı başka bir SQL veritabanından geçiriyorsanız, SQL havuzunda desteklenmeyen veri türlerini bulabilirsiniz. Varolan SQL şemanızda desteklenmeyen veri türlerini bulmak için aşağıdaki sorguyu kullanın:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="workarounds-for-unsupported-data-types"></a><a name="unsupported-data-types"></a>Desteklenmeyen veri türleri için geçici çözüm

Aşağıdaki liste, SQL havuzunun desteklemediği veri türlerini gösterir ve desteklenmeyen veri türleri için yararlı alternatifler sağlar.

| Desteklenmeyen veri türü | Geçici çözüm |
| --- | --- |
| [Geometri](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql) |[Varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [Coğrafya](/sql/t-sql/spatial-geography/spatial-types-geography) |[Varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [Hierarchyid](/sql/t-sql/data-types/hierarchyid-data-type-method-reference) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql)(4000) |
| [Görüntü](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[Varbinary](/sql/t-sql/data-types/binary-and-varbinary-transact-sql) |
| [Metin](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [Ntext](/sql/t-sql/data-types/ntext-text-and-image-transact-sql) |[nvarchar](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql) |
| [Sql_variant](/sql/t-sql/data-types/sql-variant-transact-sql) |Sütunu güçlü bir şekilde yazılan birkaç sütuna bölün. |
| [Tablo](/sql/t-sql/data-types/table-transact-sql) |Geçici tablolara dönüştürün. |
| [timestamp](/sql/t-sql/data-types/date-and-time-types) |[Datetime2](/sql/t-sql/data-types/datetime2-transact-sql) ve [CURRENT_TIMESTAMP](/sql/t-sql/functions/current-timestamp-transact-sql) işlevini kullanmak için kodu yeniden çalışır. Yalnızca sabitler varsayılan olarak desteklenir, bu nedenle current_timestamp varsayılan kısıtlama olarak tanımlanamaz. Zaman damgası yazılan sütundan satır sürüm değerlerini geçirmeniz gerekiyorsa, NULL veya NULL satır sürüm değerleri için [IKILI](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) veya [VARBINARY](/sql/t-sql/data-types/binary-and-varbinary-transact-sql)(8) kullanın. |
| [Xml](/sql/t-sql/xml/xml-transact-sql) |[varchar](/sql/t-sql/data-types/char-and-varchar-transact-sql) |
| [kullanıcı tanımlı türü](/sql/relational-databases/native-client/features/using-user-defined-types) |Mümkün olduğunda yerel veri türüne geri dönüştürün. |
| varsayılan değerler | Varsayılan değerler yalnızca gerçek ve sabitleri destekler. |


## <a name="next-steps"></a>Sonraki adımlar

Tablo geliştirme hakkında daha fazla bilgi için [Tabloya Genel Bakış'a](sql-data-warehouse-tables-overview.md)bakın.
