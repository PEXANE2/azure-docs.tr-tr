---
title: Spark havuzları (Önizleme) ve SQL havuzları arasında verileri içeri ve dışarı aktarma
description: Bu makalede, SQL havuzları ve Spark havuzları (Önizleme) arasında verileri geri ve geriye taşımak için özel bağlayıcının nasıl kullanılacağına ilişkin bilgiler sağlanmaktadır.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: prgomata
ms.reviewer: euang
ms.openlocfilehash: f562c195e90f2356568530b9b618ae9e6610fa56
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83201470"
---
# <a name="introduction"></a>Giriş

Spark SQL Analytics Bağlayıcısı, Azure SYNAPSE 'te Spark Havuzu (Önizleme) ve SQL havuzları arasında verileri verimli bir şekilde aktarmak için tasarlanmıştır. Spark SQL Analytics Bağlayıcısı yalnızca SQL havuzlarında çalışır, Isteğe bağlı SQL ile çalışmaz.

## <a name="design"></a>Tasarlama

Spark havuzları ve SQL havuzları arasında veri aktarımı, JDBC kullanılarak yapılabilir. Bununla birlikte, Spark ve SQL havuzları gibi iki Dağıtılmış Sistem, JDBC, seri veri aktarımı ile ilgili bir performans sorunu olduğunu eğilimi gösterir.

SQL Analytics bağlayıcısıyla Spark havuzları, Apache Spark için bir veri kaynağı uygulamasıdır. Spark kümesi ve SQL Analytics örneği arasında verileri verimli bir şekilde aktarmak için SQL havuzlarındaki Azure Data Lake Storage Gen 2 ve PolyBase 'i kullanır.

![Bağlayıcı mimarisi](./media/synapse-spark-sqlpool-import-export/arch1.png)

## <a name="authentication-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te kimlik doğrulaması

Sistemler arasında kimlik doğrulaması, Azure SYNAPSE Analytics 'te sorunsuz hale getirilir. Depolama hesabına veya veri ambarı sunucusuna erişirken kullanılacak güvenlik belirteçlerini almak için Azure Active Directory ile bağlanan bir belirteç hizmeti vardır. Bu nedenle, depolama hesabında ve veri ambarı sunucusunda AAD kimlik doğrulaması yapılandırıldığı sürece kimlik bilgileri oluşturmanız veya bunları bağlayıcı API 'de belirtmeniz gerekmez. Aksi takdirde, SQL kimlik doğrulaması belirlenebilir. [Kullanım](#usage) bölümünde daha fazla ayrıntı bulun.

## <a name="constraints"></a>Kısıtlamalar

- Bu bağlayıcı yalnızca Scala 'da kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

- Veri aktarmak istediğiniz veritabanında/SQL havuzunda **db_exporter** rolün olması gerekir.

Kullanıcı oluşturmak için veritabanına bağlanın ve aşağıdaki örnekleri izleyin:

```Sql
CREATE USER Mary FROM LOGIN Mary;
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Rol atamak için:

```Sql
EXEC sp_addrolemember 'db_exporter', 'Mary';
```

## <a name="usage"></a>Kullanım

İçeri aktarma deyimleri gerekli değildir, Not defteri deneyimi için önceden içeri aktarırlar.

### <a name="transferring-data-to-or-from-a-sql-pool-in-the-logical-server-dw-instance-attached-with-the-workspace"></a>Çalışma alanıyla eklenen mantıksal sunucudaki (DW örneği) bir SQL havuzundan veri aktarma

> [!NOTE]
> **Not defteri deneyiminde içeri aktarmalar gerekmez**

```Scala
 import com.microsoft.spark.sqlanalytics.utils.Constants
 import org.apache.spark.sql.SqlAnalyticsConnector._
```

#### <a name="read-api"></a>API 'YI oku

```Scala
val df = spark.read.sqlanalytics("[DBName].[Schema].[TableName]")
```

Yukarıdaki API, hem Iç (yönetilen) hem de SQL havuzundaki dış tablolar için çalışacaktır.

#### <a name="write-api"></a>Yazma API 'SI

```Scala
df.write.sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

Burada TableType sabit olabilir. Iç veya sabitler. EXTERNAL

```Scala
df.write.sqlanalytics("[DBName].[Schema].[TableName]", Constants.INTERNAL)
df.write.sqlanalytics("[DBName].[Schema].[TableName]", Constants.EXTERNAL)
```

Depolama ve SQL Server kimlik doğrulaması yapılır

### <a name="if-you-are-transferring-data-to-or-from-a-sql-pool-or-database-in-a-logical-server-outside-the-workspace"></a>Çalışma alanı dışındaki bir mantıksal sunucudaki bir SQL havuzuna veya veritabanına veri aktardıysanız

> [!NOTE]
> Not defteri deneyiminde içeri aktarmalar gerekmez

```Scala
 import com.microsoft.spark.sqlanalytics.utils.Constants
 import org.apache.spark.sql.SqlAnalyticsConnector._
```

#### <a name="read-api"></a>API 'YI oku

```Scala
val df = spark.read.
option(Constants.SERVER, "samplews.database.windows.net").
sqlanalytics("<DBName>.<Schema>.<TableName>")
```

#### <a name="write-api"></a>Yazma API 'SI

```Scala
df.write.
option(Constants.SERVER, "[samplews].[database.windows.net]").
sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

### <a name="using-sql-auth-instead-of-aad"></a>AAD yerine SQL auth kullanma

#### <a name="read-api"></a>API 'YI oku

Şu anda bağlayıcı, çalışma alanının dışında olan bir SQL havuzuna belirteç tabanlı kimlik doğrulamasını desteklemiyor. SQL kimlik doğrulaması kullanmanız gerekir.

```Scala
val df = spark.read.
option(Constants.SERVER, "samplews.database.windows.net").
option(Constants.USER, [SQLServer Login UserName]).
option(Constants.PASSWORD, [SQLServer Login Password]).
sqlanalytics("<DBName>.<Schema>.<TableName>")
```

#### <a name="write-api"></a>Yazma API 'SI

```Scala
df.write.
option(Constants.SERVER, "[samplews].[database.windows.net]").
option(Constants.USER, [SQLServer Login UserName]).
option(Constants.PASSWORD, [SQLServer Login Password]).
sqlanalytics("[DBName].[Schema].[TableName]", [TableType])
```

### <a name="using-the-pyspark-connector"></a>PySpark bağlayıcısını kullanma

> [!NOTE]
> Bu örnek yalnızca akılda tutulan Not defteri deneyimiyle verilmiştir.

DW içine yazmak istediğiniz bir "pyspark_df" veri çerçevesine sahip olduğunu varsayalım.

PySpark içinde dataframe kullanarak geçici tablo oluşturma

```Python
pyspark_df.createOrReplaceTempView("pysparkdftemptable")
```

Mıknatıc kullanarak PySpark not defterinde bir Scala hücresi çalıştırma

```Scala
%%spark
val scala_df = spark.sqlContext.sql ("select * from pysparkdftemptable")

pysparkdftemptable.write.sqlanalytics("sqlpool.dbo.PySparkTable", Constants.INTERNAL)
```

Benzer şekilde, okuma senaryosunda, Scala kullanarak verileri okuyun ve geçici bir tabloya yazın ve geçici tabloyu bir veri çerçevesinde sorgulamak için PySpark içinde Spark SQL kullanın.

## <a name="allowing-other-users-to-use-the-dw-connector-in-your-workspace"></a>Diğer kullanıcıların çalışma alanınızda DW bağlayıcısını kullanmasına izin verme

Başkalarının eksik izinlerini değiştirmek için, çalışma alanına bağlı ADLS 2. depolama hesabında Depolama Blobu veri sahibi olmanız gerekir. Kullanıcının çalışma alanına erişimi olduğundan ve not defterlerini çalıştırma izinlerine sahip olduğundan emin olun.

### <a name="option-1"></a>Seçenek 1

- Kullanıcıya Depolama Blobu verileri katılımcısı/Owner yapın

### <a name="option-2"></a>2. Seçenek

- Klasör yapısında aşağıdaki ACL 'Leri belirtin:

| Klasör | / | synapse | çalışma alanı  | <workspacename> | Mini veri havuzları | <sparkpoolname>  | parlak havuzörnekleri  |
|--|--|--|--|--|--|--|--|
| Erişim Izinleri |--X |--X |--X |--X |--X |--X |-WX |
| Varsayılan Izinler |---|---|---|---|---|---|---|

- "SYNAPSE" ve Azure portal 'den aşağı doğru tüm klasörlere ACL 'leri sağlayabilmelisiniz. "/" Kök dizinini ACL 'ye eklemek için aşağıdaki yönergeleri izleyin.

- AAD kullanarak Depolama Gezgini çalışma alanıyla bağlantılı depolama hesabına bağlanma
- Hesabınızı seçin ve çalışma alanı için ADLS 2. URL 'sini ve varsayılan dosya sistemini verin
- Listelenen depolama hesabını gördüğünüzde, liste çalışma alanına sağ tıklayın ve "erişimi yönet" i seçin
- Kullanıcıyı "Yürüt" erişim Izniyle/klasörüne ekleyin. "Tamam" ı seçin

**Şunları yapmayı düşünmüyorsanız "varsayılan" seçeneğini seçmeyin emin olun.**

## <a name="next-steps"></a>Sonraki adımlar

- [SQL havuzu oluşturma](../../synapse-analytics/quickstart-create-apache-spark-pool.md))
- [Azure SYNAPSE Analytics çalışma alanı için yeni bir Apache Spark havuzu oluşturma](../../synapse-analytics/quickstart-create-apache-spark-pool.md) 