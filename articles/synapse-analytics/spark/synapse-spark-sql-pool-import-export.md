---
title: Spark havuzları (önizleme) ve SQL havuzları arasında veri alma ve dışa aktarma
description: Bu makalede, SQL havuzları ve Kıvılcım havuzları arasında ileri ve geri veri taşımak için özel bağlayıcı nasıl kullanılacağı hakkında bilgi sağlar (önizleme).
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: prgomata
ms.reviewer: euang
ms.openlocfilehash: f92c05476c9e85690fdeacade5463a43d0a4af42
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424296"
---
# <a name="introduction"></a>Giriş

Spark SQL Analytics Bağlayıcısı, Azure Sinaps'taki Spark havuzu (önizleme) ve SQL havuzları arasındaki verileri verimli bir şekilde aktarmak için tasarlanmıştır. Spark SQL Analytics Bağlayıcısı yalnızca SQL havuzlarında çalışır, SQL on-Demand ile çalışmaz.

## <a name="design"></a>Tasarım

Spark havuzları ve SQL havuzları arasında veri aktarımı JDBC kullanılarak yapılabilir. Ancak, Spark ve SQL havuzları gibi iki dağıtılmış sistemler göz önüne alındığında, JDBC seri veri aktarımı ile bir darboğaz olma eğilimindedir.

SQL Analytics Connector'a spark havuzları Apache Spark için bir veri kaynağı uygulamasıdır. Spark kümesi ve SQL Analytics örneği arasındaki verileri verimli bir şekilde aktarmak için SQL havuzlarında Azure Veri Gölü Depolama Gen 2 ve SQL havuzlarında Polybase kullanır.

![Konektör Mimarisi](./media/synapse-spark-sqlpool-import-export/arch1.png)

## <a name="authentication-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te kimlik doğrulama

Sistemler arasındaki kimlik doğrulama, Azure Synapse Analytics'te sorunsuz bir şekilde gerçekleştirilir. Depolama hesabına veya veri ambarı sunucusuna erişirken kullanılmak üzere güvenlik belirteçleri elde etmek için Azure Etkin Dizin'e bağlanan bir Belirteç Hizmeti vardır. Bu nedenle, AAD-Auth depolama hesabında ve veri ambarı sunucusunda yapılandırıldığı sürece kimlik bilgileri oluşturmaya veya bağlayıcı API'de belirtmeye gerek yoktur. Değilse, SQL Auth belirtilebilir. [Kullanım](#usage) bölümünde daha fazla ayrıntı bulabilirsiniz.

## <a name="constraints"></a>Kısıtlamalar

- Bu konektör sadece Scala'da çalışır.

## <a name="prerequisites"></a>Ön koşullar

- Veri aktarmak istediğiniz veritabanı/SQL havuzunda **db_exporter** rolü var.

Kullanıcı oluşturmak için veritabanına bağlanın ve aşağıdaki örnekleri izleyin:

```Sql
CREATE USER Mary FROM LOGIN Mary;
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Bir rol atamak için:

```Sql
EXEC sp_addrolemember 'db_exporter', 'Mary';
```

## <a name="usage"></a>Kullanım

İthalat ekstrelerinin sağlanması gerekmez, not defteri deneyimi için önceden içe aktarılır.

### <a name="transferring-data-to-or-from-a-sql-pool-in-the-logical-server-dw-instance-attached-with-the-workspace"></a>Çalışma alanına bağlı Mantıksal Sunucu'daki (DW Örneği) bir SQL havuzuna veya bu havuzdan veri aktarma

> [!NOTE]
> **Not defteri deneyiminde gerek olmayan içeri almalar**

```Scala
 import com.microsoft.spark.sqlanalytics.utils.Constants
 import org.apache.spark.sql.SqlAnalyticsConnector._
```

#### <a name="read-api"></a>API'yi okuyun

```Scala
val df = spark.read.sqlanalytics("[DBName].[Schema].[TableName]")
```

Yukarıdaki API, SQL havuzunda hem İç (Yönetilen) hem de Harici Tablolar için çalışacaktır.

#### <a name="write-api"></a>API Yaz

```Scala
df.write.sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

TableType'ın Constants.INTERNAL veya Constants olabileceği yer.EXTERNAL

```Scala
df.write.sqlanalytics("[DBName].[Schema].[TableName]", Constants.INTERNAL)
df.write.sqlanalytics("[DBName].[Schema].[TableName]", Constants.EXTERNAL)
```

Depolama ve SQL Server kimlik doğrulaması yapılır

### <a name="if-you-are-transferring-data-to-or-from-a-sql-pool-or-database-in-a-logical-server-outside-the-workspace"></a>Çalışma alanı dışında mantıksal bir sunucuda bir SQL havuzuna veya veritabanına veri aktarılıyorsanız

> [!NOTE]
> Not defteri deneyiminde gerek olmayan içeri almalar

```Scala
 import com.microsoft.spark.sqlanalytics.utils.Constants
 import org.apache.spark.sql.SqlAnalyticsConnector._
```

#### <a name="read-api"></a>API'yi okuyun

```Scala
val df = spark.read.
option(Constants.SERVER, "samplews.database.windows.net").
sqlanalytics("<DBName>.<Schema>.<TableName>")
```

#### <a name="write-api"></a>API Yaz

```Scala
df.write.
option(Constants.SERVER, "[samplews].[database.windows.net]").
sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

### <a name="using-sql-auth-instead-of-aad"></a>AAD yerine SQL Auth kullanma

#### <a name="read-api"></a>API'yi okuyun

Şu anda bağlayıcı, çalışma alanının dışında bir SQL havuzuna belirteç tabanlı auth'u desteklemez. SQL Auth kullanmanız gerekir.

```Scala
val df = spark.read.
option(Constants.SERVER, "samplews.database.windows.net").
option(Constants.USER, [SQLServer Login UserName]).
option(Constants.PASSWORD, [SQLServer Login Password]).
sqlanalytics("<DBName>.<Schema>.<TableName>")
```

#### <a name="write-api"></a>API Yaz

```Scala
df.write.
option(Constants.SERVER, "[samplews].[database.windows.net]").
option(Constants.USER, [SQLServer Login UserName]).
option(Constants.PASSWORD, [SQLServer Login Password]).
sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

### <a name="using-the-pyspark-connector"></a>PySpark konektörünü kullanma

> [!NOTE]
> Bu örnek, yalnızca göz önünde bulundurulan not defteri deneyimi ile verilir.

DW'ye yazmak istediğiniz bir veri çerçeveniz "pyspark_df" olduğunu varsayalım.

PySpark'taki veri çerçevesini kullanarak geçici tablo oluşturma

```Python
pyspark_df.createOrReplaceTempView("pysparkdftemptable")
```

Sihirli kullanarak PySpark dizüstü bir Scala hücre çalıştırın

```Scala
%%spark
val scala_df = spark.sqlContext.sql ("select * from pysparkdftemptable")

pysparkdftemptable.write.sqlanalytics("sqlpool.dbo.PySparkTable", Constants.INTERNAL)
```
Benzer şekilde, okuma senaryosunda, Scala kullanarak verileri okuyun ve geçici bir tabloya yazın ve geçici tabloyu bir veri çerçevesi halinde sorgulamak için PySpark'taki Spark SQL'i kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL havuzu oluşturma]([Create a new Apache Spark pool for an Azure Synapse Analytics workspace](../../synapse-analytics/quickstart-create-apache-spark-pool.md))
- [Azure Synapse Analytics çalışma alanı için yeni bir Apache Spark havuzu oluşturun](../../synapse-analytics/quickstart-create-apache-spark-pool.md) 