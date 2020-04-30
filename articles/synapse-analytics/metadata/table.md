---
title: Azure SYNAPSE Analytics ' paylaşılan meta veri tabloları
description: Azure SYNAPSE Analytics, verileri çoğaltmadan SQL 'in isteğe bağlı (Önizleme) ve SQL havuzu altyapılarından erişilebilir hale getirmek için Apache Spark bir tablo oluşturmanın paylaşılan bir meta veri modeli sağlar.
services: sql-data-warehouse
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: 7c1951c772dcd2f49f4f7c09021f69193af0a87e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424583"
---
# <a name="azure-synapse-analytics-shared-metadata-tables"></a>Azure SYNAPSE Analytics paylaşılan meta veri tabloları

[!INCLUDE [synapse-analytics-preview-terms](../../../includes/synapse-analytics-preview-terms.md)]

Azure SYNAPSE Analytics, farklı çalışma alanı hesaplama altyapılarının, Apache Spark havuzları (Önizleme), SQL isteğe bağlı (Önizleme) altyapısı ve SQL havuzları arasında veritabanlarını ve Parquet tarafından desteklenen tabloları paylaşmasına izin verir.

Bir Spark işi tarafından bir veritabanı oluşturulduktan sonra, depolama biçimi olarak Parquet kullanan Spark ile birlikte tablo oluşturabilirsiniz. Bu tablolar, Azure SYNAPSE çalışma alanı Spark havuzlarından herhangi biri tarafından sorgulanarak hemen kullanılabilir hale gelir. Bunlar ayrıca, izinlerle ilgili Spark işlerinin herhangi birinden de kullanılabilir.

Oluşturulan, yönetilen ve dış tablolar, SQL 'de isteğe bağlı eşitlenmiş veritabanında ve meta veri eşitlemesi etkinleştirilmiş SQL havuzlarındaki karşılık gelen `$`ön ek şemalarda aynı ada sahip harici tablolar olarak da kullanılabilir hale getirilir. [SQL 'de Spark tablosunun kullanıma](#exposing-a-spark-table-in-sql) sunulması tablo eşitlemesi hakkında daha fazla ayrıntı sağlar.

Tablolar, istek üzerine SQL ve SQL havuzlarının zaman uyumsuz olarak eşitlendiğinden, görünene kadar bir gecikme olur.

Tabloların dış tablolara, veri kaynaklarına ve dosya biçimlerine eşlenmesi.

## <a name="manage-a-spark-created-table"></a>Spark tarafından oluşturulan bir tabloyu yönetme

Spark tarafından oluşturulan veritabanlarını yönetmek için Spark 'ı kullanın. Örneğin, bir Spark havuzu işi aracılığıyla silin ve Spark ' dan tablo oluşturun.

Bu tür bir veritabanında SQL isteğe bağlı olarak nesne oluşturursanız veya veritabanını bırakmaya çalışırsanız, işlem başarılı olur ancak özgün Spark veritabanı değiştirilmez.

Eşitlenmiş şemayı bir SQL havuzunda bırakmayı veya bir tablo oluşturmayı denerseniz, Azure bir hata döndürür.

## <a name="exposing-a-spark-table-in-sql"></a>SQL 'de Spark tablosu gösterme

### <a name="which-spark-tables-are-shared"></a>Hangi Spark tablolarının paylaşıldığı

Spark, Azure SYNAPSE 'in SQL 'de otomatik olarak sunduğu iki tür tablo sağlar:

- Yönetilen tablolar

  Spark, metın, CSV, JSON, JDBC, PARQUET, ORC, HIVE, DELTA ve LIBSVM gibi yönetilen tablolarda verilerin depolanması için birçok seçenek sunar. Bu dosyalar normalde yönetilen tablo verilerinin depolandığı `warehouse` dizinde depolanır.

- Dış tablolar

  Spark ayrıca, `LOCATION` ya da Hive biçimini kullanarak var olan veriler üzerinde dış tablo oluşturmak için yollar sağlar. Bu tür dış tablolar, Parquet dahil çeşitli veri biçimlerinin üzerinde olabilir.

Azure SYNAPSE Şu anda yalnızca SQL altyapılarıyla verileri Parquet formatında depolayan yönetilen ve harici Spark tablolarını paylaşır. Diğer biçimler tarafından desteklenen tablolar otomatik olarak eşitlenmez. SQL altyapısı tablonun temel biçimini destekliyorsa, bu tür tabloları kendi SQL veritabanınızda bir dış tablo olarak doğrudan eşitleyebilirsiniz.

### <a name="how-are-spark-tables-shared"></a>Spark tabloları nasıl paylaşılır

SQL altyapılarında, aşağıdaki özelliklere sahip dış tablo olarak sunulan paylaşılabilir yönetilen ve dış Spark tabloları:

- SQL dış tablosunun veri kaynağı Spark tablosunun konum klasörünü temsil eden veri kaynağıdır.
- SQL dış tablosunun dosya biçimi Parquet 'dir.
- SQL dış tablosunun erişim kimlik bilgileri geçişdir.

Tüm Spark tablosu adları geçerli SQL tablo adları olduğundan ve Spark sütun adları geçerli SQL sütun adları olduğundan, SQL dış tablosu için Spark tablosu ve sütun adları kullanılacaktır.

Spark tabloları, SYNAPSE SQL altyapılarından farklı veri türleri sağlar. Aşağıdaki tabloda Spark tablosu veri türleri SQL türleriyle eşlenir:

| Spark veri türü | SQL veri türü | Açıklamalar |
|---|---|---|
| `byte`      | `smallint`       ||
| `short`     | `smallint`       ||
| `integer`   |    `int`            ||
| `long`      |    `bigint`         ||
| `float`     | `real`           |<!-- need precision and scale-->|
| `double`    | `float`          |<!-- need precision and scale-->|
| `decimal`      | `decimal`        |<!-- need precision and scale-->|
| `timestamp` |    `datetime2`      |<!-- need precision and scale-->|
| `date`      | `date`           ||
| `string`    |    `varchar(max)`   | Harmanlama ile`Latin1_General_CP1_CI_AS_UTF8` |
| `binary`    |    `varbinary(max)` ||
| `boolean`   |    `bit`            ||
| `array`     |    `varchar(max)`   | Harmanlama ile JSON içine dizileştirir`Latin1_General_CP1_CI_AS_UTF8` |
| `map`       |    `varchar(max)`   | Harmanlama ile JSON içine dizileştirir`Latin1_General_CP1_CI_AS_UTF8` |
| `struct`    |    `varchar(max)`   | Harmanlama ile JSON içine dizileştirir`Latin1_General_CP1_CI_AS_UTF8` |

<!-- TODO: Add precision and scale to the types mentioned above -->

## <a name="security-model"></a>Güvenlik modeli

Spark veritabanlarının ve tablolarının yanı sıra SQL altyapılarındaki eşitlenmiş temsilleri, temel alınan depolama düzeyinde güvenli hale getirilir. Şu anda nesnelerin kendileri üzerinde izinleri olmadığından, nesneler Nesne Gezgini 'nde görülebilir.

Yönetilen bir tablo oluşturan güvenlik sorumlusu, bu tablonun sahibi olarak değerlendirilir ve tablodaki tüm haklara ve temel klasör ve dosyalara sahip olur. Ayrıca, veritabanının sahibi otomatik olarak tablonun ikincil sahibi olur.

Kimlik doğrulaması geçişli bir Spark veya SQL dış tablosu oluşturursanız, veriler yalnızca klasör ve dosya düzeylerinde güvenlidir. Birisi bu tür bir dış tabloyu sorgullarsa, sorgu gönderenin güvenlik kimliği dosya sistemine geçirilir ve bu da erişim haklarını kontrol eder.

Klasörler ve dosyalar üzerinde izinlerin nasıl ayarlanacağı hakkında daha fazla bilgi için bkz. [Azure SYNAPSE Analytics Shared Database](database.md).

## <a name="examples"></a>Örnekler

### <a name="create-a-managed-table-backed-by-parquet-in-spark-and-query-from-sql-on-demand"></a>Spark ve SQL isteğe bağlı sorgu 'da Parquet tarafından desteklenen yönetilen bir tablo oluşturma

Bu senaryoda adlı `mytestdb`bir Spark veritabanınız vardır. Bkz. [Spark veritabanına bağlantı oluşturma & bağlanma-isteğe bağlı SQL](database.md#create--connect-to-spark-database---sql-on-demand).

Aşağıdaki komutu çalıştırarak, mini bir SQL ile yönetilen Spark tablosu oluşturun:

```sql
    CREATE TABLE mytestdb.myParquetTable(id int, name string, birthdate date) USING Parquet
```

Bu, tabloyu `myParquetTable` veritabanında `mytestdb`oluşturur. Kısa bir gecikmeden sonra, tabloyu SQL isteğe bağlı olarak görebilirsiniz. Örneğin, isteğe bağlı SQL 'de aşağıdaki ifadeyi çalıştırın.

```sql
    USE mytestdb;
    SELECT * FROM sys.tables;
```

`myParquetTable` Sonuçlara dahil edildiğini doğrulayın.

>[!NOTE]
>Depolama biçimi olarak Parquet kullanmayan bir tablo eşitlenmeyecektir.

Sonra, Spark 'dan tabloya bazı değerler ekleyin, örneğin bir C# not defterinde aşağıdaki C# Spark deyimleriyle.

```csharp
using Microsoft.Spark.Sql.Types;

var data = new List<GenericRow>();

data.Add(new GenericRow(new object[] { 1, "Alice", new Date(2010, 1, 1)}));
data.Add(new GenericRow(new object[] { 2, "Bob", new Date(1990, 1, 1)}));

var schema = new StructType
    (new List<StructField>()
        {
            new StructField("id", new IntegerType()),
            new StructField("name", new StringType()),
            new StructField("birthdate", new DateType())
        }
    );

var df = spark.CreateDataFrame(data, schema);
df.Write().Mode(SaveMode.Append).InsertInto("mytestdb.myParquetTable");
```

Artık SQL isteğe bağlı verileri şu şekilde okuyabilirsiniz:

```sql
SELECT * FROM mytestdb.dbo.myParquetTable WHERE name = 'Alice';
```

Sonuç olarak aşağıdaki satırı almalısınız:

```
id | name | birthdate
---+-------+-----------
1 | Alice | 2010-01-01
```

### <a name="creating-an-external-table-backed-by-parquet-in-spark-and-querying-it-from-sql-on-demand"></a>Spark 'ta Parquet tarafından desteklenen bir dış tablo oluşturma ve bunları isteğe bağlı olarak sorgulama

Bu örnekte, yönetilen tablo için önceki örnekte oluşturulan Parquet veri dosyaları üzerinde bir dış Spark tablosu oluşturun.

Örneğin, Mini SQL çalıştırması ile:

```sql
CREATE TABLE mytestdb.myExternalParquetTable
    USING Parquet
    LOCATION "abfss://<fs>@arcadialake.dfs.core.windows.net/synapse/workspaces/<synapse_ws>/warehouse/mytestdb.db/myparquettable/"
```

Yer tutucusunu `<fs>` , çalışma alanı varsayılan dosya sistemi olan dosya sistemi adıyla ve bu örneği çalıştırmak için kullandığınız `<synapse_ws>` SYNAPSE çalışma alanının adı ile birlikte yer tutucu ile değiştirin.

Önceki örnekte, tablosu `myExtneralParquetTable` veritabanında `mytestdb`oluşturulur. Kısa bir gecikmeden sonra, tabloyu SQL isteğe bağlı olarak görebilirsiniz. Örneğin, isteğe bağlı SQL 'de aşağıdaki ifadeyi çalıştırın.

```sql
USE mytestdb;
SELECT * FROM sys.tables;
```

`myExternalParquetTable` Sonuçlara dahil edildiğini doğrulayın.

Artık SQL isteğe bağlı verileri şu şekilde okuyabilirsiniz:

```sql
SELECT * FROM mytestdb.dbo.myExternalParquetTable WHERE name = 'Alice';
```

Sonuç olarak aşağıdaki satırı almalısınız:

```
id | name | birthdate
---+-------+-----------
1 | Alice | 2010-01-01
```

### <a name="querying-spark-tables-in-a-sql-pool"></a>SQL havuzunda Spark tablolarını sorgulama

Önceki örneklerde oluşturulan tablolarla, artık adlı `mysqlpool` çalışma alanınızda meta veri eşitlemeye izin veren bir SQL havuzu oluşturun (veya zaten oluşturulan havuzu [bir SQL havuzunda Spark veritabanını açığa çıkarmadan](database.md#exposing-a-spark-database-in-a-sql-pool)kullanın.

`mysqlpool` SQL havuzunda aşağıdaki ifadeyi çalıştırın:

```sql
SELECT * FROM sys.tables;
```

Tabloların `myParquetTable` ve `myExternalParquetTable` şemada `$mytestdb`görünür olduğunu doğrulayın.

Artık SQL isteğe bağlı verileri şu şekilde okuyabilirsiniz:

```sql
SELECT * FROM [$mytestdb].myParquetTable WHERE name = 'Alice';
SELECT * FROM [$mytestdb].myExternalParquetTable WHERE name = 'Alice';
```

Yukarıdaki isteğe bağlı SQL ile aynı sonuçları almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SYNAPSE Analytics 'te paylaşılan meta veriler hakkında daha fazla bilgi edinin](overview.md)
- [Azure SYNAPSE Analytics ' paylaşılan meta veri tabloları hakkında daha fazla bilgi edinin](table.md)


