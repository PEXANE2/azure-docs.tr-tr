---
title: Vekil anahtarları oluşturmak için KİmLİk kullanma
description: Synapse SQL havuzundaki tablolarda vekil anahtarlar oluşturmak için KİmLİk özelliğini kullanmak için öneriler ve örnekler.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/30/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: d4a9880ed7ab26d0127026f49c0bc781cfc2a941
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80586326"
---
# <a name="using-identity-to-create-surrogate-keys-in-synapse-sql-pool"></a>Synapse SQL havuzunda vekil anahtarları oluşturmak için KİmLİk kullanma

Synapse SQL havuzundaki tablolarda vekil anahtarlar oluşturmak için KİmLİk özelliğini kullanmak için öneriler ve örnekler.

## <a name="what-is-a-surrogate-key"></a>Vekil anahtar nedir

Tablodaki vekil anahtarı, her satır için benzersiz bir tanımlayıcısı olan bir sütundur. Anahtar tablo verilerinden oluşturulmadı. Veri modelleyicileri, veri ambarı modellerini tasarlarken tablolarında vekil anahtarları oluşturmayı severler. Yük performansını etkilemeden bu amaca ulaşmak için KİmLİk özelliğini kullanabilirsiniz.  

## <a name="creating-a-table-with-an-identity-column"></a>KİmLİk sütunu içeren tablo oluşturma

IDENTITY özelliği, yük performansını etkilemeden Synapse SQL havuzundaki tüm dağıtımları ölçeklendirmek üzere tasarlanmıştır. Bu nedenle, KİmLİk uygulaması bu hedeflere ulaşmaya yöneliktir.

Aşağıdaki ifadeye benzer sözdizimi kullanarak tabloyu ilk oluşturduğunuzda, tabloyu KİmLİk özelliğine sahip olarak tanımlayabilirsiniz:

```sql
CREATE TABLE dbo.T1
(    C1 INT IDENTITY(1,1) NOT NULL
,    C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Daha sonra `INSERT..SELECT` tabloyu doldurmak için kullanabilirsiniz.

Bu bölümün geri kalanı, bunları daha tam olarak anlamanıza yardımcı olmak için uygulamanın nüanslarını vurgular.  

### <a name="allocation-of-values"></a>Değerlerin tahsisi

KİmLİk özelliği, SQL Server ve Azure SQL Veritabanı'nın davranışını yansıtan vekil değerlerinin ayrılma sırasını garanti etmez. Ancak, Synapse SQL havuzunda, bir garantinin yokluğu daha belirgindir.

Aşağıdaki örnek bir örnektir:

```sql
CREATE TABLE dbo.T1
(    C1 INT IDENTITY(1,1)    NOT NULL
,    C2 VARCHAR(30)                NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

Önceki örnekte, iki satır dağılım 1 indi. İlk satır sütunda `C1`1'in vekil değerine, ikinci satırın ise 61'in vekil değerine sahiptir. Bu değerlerin her ikisi de KİmLİk özelliği tarafından oluşturuldu. Ancak, değerlerin tahsisi bitişik değildir. Bu davranış tasarım gereğidir.

### <a name="skewed-data"></a>Çarpık veriler

Veri türü için değer aralığı dağıtımlar arasında eşit olarak yayılır. Dağıtılmış bir tabloda çarpık veriler varsa, veri türü için kullanılabilir değer aralığı zamanından önce tükenebilir. Örneğin, tüm veriler tek bir dağıtımda biterse, tablo etkili bir şekilde veri türünün değerlerinin yalnızca altıda birine erişebilir. Bu nedenle, KİmLİk özelliği `INT` `BIGINT` yalnızca veri türleri ile sınırlıdır.

### <a name="selectinto"></a>Seçin.. INTO

Varolan bir KİmLİk sütunu yeni bir tabloya seçildiğinde, aşağıdaki koşullardan biri doğru olmadığı sürece, yeni sütun KİmLİk özelliğini devralır:

- SELECT deyimi bir birleştirme içerir.
- Birden çok SELECT deyimleri UNION kullanılarak birleştirilir.
- KİmLİk sütunu SELECT listesinde birden fazla kez listelenir.
- KİmLİk sütunu bir ifadenin parçasıdır.

Bu koşullardan herhangi biri doğruysa, sütun KİmLİk özelliğini devralmak yerine NULL Değİl oluşturulur.

### <a name="create-table-as-select"></a>CREATE TABLE AS SELECT

CREATE TABLE AS SELECT (CTAS) SELECT için belgelenen aynı SQL Server davranışını izler... IÇINE. Ancak, deyimin `CREATE TABLE` sütun tanımında bir KİmLİk özelliği belirtemezsiniz. CTAS `SELECT` bölümünde ki KİmLİk işlevini de kullanamazsınız. Bir tabloyu doldurmak `CREATE TABLE` `INSERT..SELECT` için, tabloyu doldurmak için izleyen tabloyu tanımlamak için kullanmanız gerekir.

## <a name="explicitly-inserting-values-into-an-identity-column"></a>Kimlik sütununa değerleri açıkça ekleme

Synapse SQL `SET IDENTITY_INSERT <your table> ON|OFF` havuzu sözdizimini destekler. Bu sözdizimini, kimlik sütununa açıkça değer eklemek için kullanabilirsiniz.

Birçok veri modelleyicisi, boyutlarındaki belirli satırlar için önceden tanımlanmış negatif değerleri kullanmayı sever. Örnek -1 veya "bilinmeyen üye" satırıdır.

Sonraki komut dosyası, SET IDENTITY_INSERT kullanarak bu satırın nasıl açıkça eklendirilebildiğini gösterir:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT     *
FROM    dbo.T1
;
```

## <a name="loading-data"></a>Veri yükleme

KİmLİk özelliğinin varlığı, veri yükleme kodunuz üzerinde bazı etkilere sahiptir. Bu bölümde, Identity kullanarak tablolara veri yüklemek için bazı temel desenler vurgulanır.

Bir tabloya veri yüklemek ve KİmLİk kullanarak bir vekil anahtarı oluşturmak için, tablo oluşturmak ve sonra INSERT kullanın... SELECT veya INSERT... YÜKÜ gerçekleştirmek için DEĞERLER.

Aşağıdaki örnek, temel deseni vurgular:

```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(    C1 INT IDENTITY(1,1)
,    C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT     C2
FROM    ext.T1
;

SELECT *
FROM   dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE]
> Kimlik sütunu olan `CREATE TABLE AS SELECT` bir tabloya veri yüklerken şu anda kullanmak mümkün değildir.
>

Veri yükleme hakkında daha fazla bilgi için, [Synapse SQL havuzu için Ekstresi, Yükleme ve Dönüştürme (ELT) Tasarlama](design-elt-data-loading.md) ve [en iyi uygulamaları yükleme](guidance-for-loading-data.md)bölümüne bakın.

## <a name="system-views"></a>Sistem görünümleri

KİmLİk özelliğine sahip bir sütunu tanımlamak için [sys.identity_columns](/sql/relational-databases/system-catalog-views/sys-identity-columns-transact-sql) kataloğu görünümünü kullanabilirsiniz.

Veritabanı şemasını daha iyi anlamanıza yardımcı olmak için, bu örnek, sys.identity_column'nin diğer sistem kataloğu görünümleriyle nasıl tümleştirilebildiğini gösterir:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Sınırlamalar

KİmLİk özelliği kullanılamaz:

- Sütun veri türü INT veya BIGINT olmadığında
- Sütun da dağıtım anahtarı olduğunda
- Tablo harici bir tablo olduğunda

Synapse SQL havuzunda aşağıdaki ilgili işlevler desteklenmez:

- [KİmLİk()](/sql/t-sql/functions/identity-function-transact-sql)
- [@@IDENTITY](/sql/t-sql/functions/identity-transact-sql)
- [Scope_ıdentıty](/sql/t-sql/functions/scope-identity-transact-sql)
- [ıdent_current](/sql/t-sql/functions/ident-current-transact-sql)
- [IDENT_INCR](/sql/t-sql/functions/ident-incr-transact-sql)
- [IDENT_SEED](/sql/t-sql/functions/ident-seed-transact-sql)

## <a name="common-tasks"></a>Genel görevler

Bu bölümde, KİmLİk sütunları ile çalışırken ortak görevleri gerçekleştirmek için kullanabileceğiniz bazı örnek kodlar bulunur.

C1 sütunu, aşağıdaki tüm görevlerde kİmLİkTIR.

### <a name="find-the-highest-allocated-value-for-a-table"></a>Tablo için ayrılan en yüksek değeri bulma

Dağıtılmış `MAX()` tablo için ayrılan en yüksek değeri belirlemek için işlevi kullanın:

```sql
SELECT MAX(C1)
FROM dbo.T1
```

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>KİmLİk özelliği için tohum ve artış bulun

Aşağıdaki sorguyu kullanarak bir tablonun kimlik artışını ve tohum yapılandırma değerlerini keşfetmek için katalog görünümlerini kullanabilirsiniz:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Sonraki adımlar

- [Tabloya genel bakış](/azure/sql-data-warehouse/sql-data-warehouse-tables-overview)
- [CREATE TABLE (Transact-SQL) KİmLİğİ (Özellik)](/sql/t-sql/statements/create-table-transact-sql-identity-property?view=azure-sqldw-latest)
- [DBCC CHECKINDENT](/sql/t-sql/database-console-commands/dbcc-checkident-transact-sql)
