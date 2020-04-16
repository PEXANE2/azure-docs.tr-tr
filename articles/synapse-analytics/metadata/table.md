---
title: Azure Synapse Analytics'in paylaşılan meta veri tabloları
description: Azure Synapse Analytics, Apache Spark'ta bir tablo oluşturmanın verileri çoğaltmadan isteğe bağlı SQL (önizleme) ve SQL havuz motorlarından erişilebilir hale getireceği paylaşılan bir meta veri modeli sağlar.
services: sql-data-warehouse
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: 7c1951c772dcd2f49f4f7c09021f69193af0a87e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424583"
---
# <a name="azure-synapse-analytics-shared-metadata-tables"></a>Azure Synapse Analytics paylaşılan meta veri tabloları

[!INCLUDE [synapse-analytics-preview-terms](../../../includes/synapse-analytics-preview-terms.md)]

Azure Synapse Analytics, farklı çalışma alanı hesaplama lı motorlarının veritabanlarını ve Parke destekli tabloları Apache Spark havuzları (önizleme), isteğe bağlı (önizleme) altyapısı ve SQL havuzları arasında paylaşmasına olanak tanır.

Bir Veritabanı bir Spark işi tarafından oluşturulduktan sonra, depolama biçimi olarak Parke kullanan Spark ile içinde tablolar oluşturabilirsiniz. Bu tablolar, Azure Synapse çalışma alanı Kıvılcım havuzlarından herhangi biri tarafından sorgulanmak üzere hemen kullanılabilir hale gelir. Ayrıca, izinlere tabi olan Kıvılcım işlerinden herhangi birinden de kullanılabilirler.

Oluşturulan, yönetilen ve dış tablolar, SQL on-demand'de ilgili senkronize veritabanında ve meta veri eşitlemesi `$`etkinleştirilmiş SQL havuzlarındaki karşılık gelen -önceden belirlenmiş şemalarda aynı ada sahip dış tablolar olarak da kullanılabilir hale getirilir. [SQL'de bir Spark tablosunun açığa çıkarılması,](#exposing-a-spark-table-in-sql) tablo eşitlemesinde daha fazla ayrıntı sağlar.

Tablolar isteğe bağlı SQL'e eşitlendive SQL havuzları eşzamanlı olarak eşitlendi, bunlar ortaya çıkana kadar gecikme olacaktır.

Tabloların dış tablolara, veri kaynaklarına ve dosya biçimlerine eşleneme.

## <a name="manage-a-spark-created-table"></a>Spark oluşturulan tabloyu yönetme

Spark oluşturulan veritabanlarını yönetmek için Spark'ı kullanın. Örneğin, bir Spark havuz işi aracılığıyla silin ve Içinde Spark tablolar oluşturun.

Böyle bir veritabanında SQL'den isteğe bağlı nesneler oluşturursanız veya veritabanını düşürmeye çalışırsanız, işlem başarılı olur, ancak özgün Spark veritabanı değiştirilmez.

Senkronize şemayı BIR SQL havuzuna düşürmeyi veya içinde tablo oluşturmaya çalışırsanız, Azure bir hata döndürür.

## <a name="exposing-a-spark-table-in-sql"></a>SQL'de Bir Kıvılcım tablosunu açığa çıkarma

### <a name="which-spark-tables-are-shared"></a>Hangi Kıvılcım tabloları paylaşılır

Spark, Azure Synapse'nin SQL'de otomatik olarak ortaya çıkardığı iki tür tablo sağlar:

- Yönetilen tablolar

  Spark, TEXT, CSV, JSON, JDBC, PARKE, ORC, HIVE, DELTA ve LIBSVM gibi yönetilen tablolarda verilerin nasıl depolanır? Bu dosyalar normalde yönetilen tablo `warehouse` verilerinin depolandığı dizinde depolanır.

- Dış tablolar

  Spark, seçeneği sağlayarak veya Kovan biçimini kullanarak varolan veriler üzerinde harici tablolar oluşturmanın `LOCATION` yollarını da sağlar. Bu tür harici tablolar Parke dahil olmak üzere çeşitli veri biçimleri üzerinde olabilir.

Azure Synapse şu anda yalnızca, verilerini PARKe biçiminde DEPOLAYAN yönetilen ve harici Spark tablolarını SQL motorları ile paylaşır. Diğer biçimlerle desteklenen tablolar otomatik olarak eşitlenmez. SQL altyapısı tablonun temel biçimini destekliyorsa, bu tür tabloları kendi SQL veritabanınızda dış tablo olarak açıkça senkronize edebilirsiniz.

### <a name="how-are-spark-tables-shared"></a>Spark tabloları nasıl paylaşılır?

SQL motorlarında dış tablolar olarak aşağıdaki özelliklere sahip paylaşımlı yönetilen ve harici Spark tabloları:

- SQL dış tablosunun veri kaynağı, Spark tablosunun konum klasörünü temsil eden veri kaynağıdır.
- SQL harici tablonun dosya biçimi Parke'dir.
- SQL dış tablosunun erişim kimlik bilgisi geçer.

Tüm Kıvılcım tablo adları geçerli SQL tablo adları olduğundan ve tüm Kıvılcım sütun adları geçerli SQL sütun adları olduğundan, Spark tablosu ve sütun adları SQL dış tablosu için kullanılır.

Kıvılcım tabloları Synapse SQL motorlarından farklı veri türleri sağlar. Aşağıdaki tablo eşlerini Sql türleri için tablo veri türleri eşlem:

| Veri türünü kıvılcımla | SQL veri türü | Yorumlar |
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
| `array`     |    `varchar(max)`   | Harmanlama ile JSON içine Serializes`Latin1_General_CP1_CI_AS_UTF8` |
| `map`       |    `varchar(max)`   | Harmanlama ile JSON içine Serializes`Latin1_General_CP1_CI_AS_UTF8` |
| `struct`    |    `varchar(max)`   | Harmanlama ile JSON içine Serializes`Latin1_General_CP1_CI_AS_UTF8` |

<!-- TODO: Add precision and scale to the types mentioned above -->

## <a name="security-model"></a>Güvenlik modeli

Spark veritabanları ve tablolarının yanı sıra SQL motorlarındaki senkronize gösterimleri temel depolama düzeyinde güvence altına alınacaktır. Şu anda nesnelerin kendileri üzerinde izinleri olmadığından, nesneler nesne gezgininde görülebilir.

Yönetilen bir tablo oluşturan güvenlik ilkesi, bu tablonun sahibi olarak kabul edilir ve tablonun tüm haklarına ve temel klasör ve dosyalara sahiptir. Ayrıca, veritabanının sahibi otomatik olarak tablonun ortak sahibi olur.

Kimlik doğrulama geçişi ne olursa, bir Kıvılcım veya SQL harici tablo oluşturursanız, veriler yalnızca klasör ve dosya düzeylerinde güvenli hale verilir. Birisi bu tür bir dış tabloyu sorgularsa, sorgu gönderenin güvenlik kimliği erişim haklarını denetleyecek olan dosya sistemine aktarılır.

Klasörler ve dosyalarda izinlerin nasıl ayarlanılabilenhakkında daha fazla bilgi için [Azure Synapse Analytics paylaşılan veritabanına](database.md)bakın.

## <a name="examples"></a>Örnekler

### <a name="create-a-managed-table-backed-by-parquet-in-spark-and-query-from-sql-on-demand"></a>Kıvılcım'da Parke tarafından desteklenen yönetilen bir tablo oluşturma ve isteğe bağlı SQL'den sorgu

Bu senaryoda, bir Spark `mytestdb`veritabanı adlı var. Bkz. [Create & Spark veritabanına bağlan - SQL isteğe bağlı.](database.md#create--connect-to-spark-database---sql-on-demand)

Aşağıdaki komutu çalıştırarak SparkSQL ile yönetilen bir Spark tablosu oluşturun:

```sql
    CREATE TABLE mytestdb.myParquetTable(id int, name string, birthdate date) USING Parquet
```

Bu veritabanında `mytestdb` `myParquetTable` tablo oluşturur. Kısa bir gecikmeden sonra, sql on-demand tablosunu görebilirsiniz. Örneğin, isteğe bağlı OLARAK SQL'den aşağıdaki deyimi çalıştırın.

```sql
    USE mytestdb;
    SELECT * FROM sys.tables;
```

Sonuçlara `myParquetTable` dahil olduğunu doğrulayın.

>[!NOTE]
>Parke'yi depolama biçimi olarak kullanmayan bir tablo eşitlenmez.

Ardından, örneğin C# not defterinde aşağıdaki C# Spark deyimleri ile Spark'tan tabloya bazı değerler ekleyin:

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

Şimdi SQL on-demand verileri aşağıdaki gibi okuyabilirsiniz:

```sql
SELECT * FROM mytestdb.dbo.myParquetTable WHERE name = 'Alice';
```

Sonuç olarak aşağıdaki satırı almalısınız:

```
id | name | birthdate
---+-------+-----------
1 | Alice | 2010-01-01
```

### <a name="creating-an-external-table-backed-by-parquet-in-spark-and-querying-it-from-sql-on-demand"></a>Parke tarafından Parke tarafından desteklenen harici bir tablo oluşturma ve isteğe bağlı SQL'den sorgulama

Bu örnekte, yönetilen tablo için önceki örnekte oluşturulan Parke veri dosyaları üzerinde harici bir Kıvılcım tablosu oluşturun.

Örneğin, SparkSQL çalıştırın:

```sql
CREATE TABLE mytestdb.myExternalParquetTable
    USING Parquet
    LOCATION "abfss://<fs>@arcadialake.dfs.core.windows.net/synapse/workspaces/<synapse_ws>/warehouse/mytestdb.db/myparquettable/"
```

Yer tutucuyu `<fs>` çalışma alanı varsayılan dosya sistemi olan dosya sistemi `<synapse_ws>` adı yla ve yer tutucuyu bu örneği çalıştırmak için kullandığınız sinaps çalışma alanının adıyla değiştirin.

Önceki örnek veritabanında `myExtneralParquetTable` `mytestdb`tablo oluşturur. Kısa bir gecikmeden sonra, sql on-demand tablosunu görebilirsiniz. Örneğin, isteğe bağlı OLARAK SQL'den aşağıdaki deyimi çalıştırın.

```sql
USE mytestdb;
SELECT * FROM sys.tables;
```

Sonuçlara `myExternalParquetTable` dahil olduğunu doğrulayın.

Şimdi SQL on-demand verileri aşağıdaki gibi okuyabilirsiniz:

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

Önceki örneklerde oluşturulan tablolarla, artık çalışma alanınızda meta `mysqlpool` veri eşitlemesini sağlayan (veya [SQL havuzunda Bir Kıvılcım veritabanını açığa çıkarmaktan](database.md#exposing-a-spark-database-in-a-sql-pool)zaten oluşturulmuş havuzu kullanan ) adlı bir SQL havuzu oluşturun.

AŞAĞıDAKI deyimi SQL `mysqlpool` havuzuna karşı çalıştırın:

```sql
SELECT * FROM sys.tables;
```

Tabloların `myParquetTable` şemada görünür olduğunu `myExternalParquetTable` `$mytestdb`doğrulayın.

Şimdi SQL on-demand verileri aşağıdaki gibi okuyabilirsiniz:

```sql
SELECT * FROM [$mytestdb].myParquetTable WHERE name = 'Alice';
SELECT * FROM [$mytestdb].myExternalParquetTable WHERE name = 'Alice';
```

Yukarıdaki SQL on-demand ile aynı sonuçları almalısınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics'in paylaşılan meta verileri hakkında daha fazla bilgi edinin](overview.md)
- [Azure Synapse Analytics'in paylaşılan meta veri Tabloları hakkında daha fazla bilgi edinin](table.md)


