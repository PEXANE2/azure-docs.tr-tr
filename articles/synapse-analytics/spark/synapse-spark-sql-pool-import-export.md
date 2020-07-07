---
title: Spark havuzları (Önizleme) ve SQL havuzları arasında verileri içeri ve dışarı aktarma
description: Bu makalede, SQL havuzları ve Spark havuzları (Önizleme) arasında verileri geri ve geriye taşımak için özel bağlayıcının nasıl kullanılacağına ilişkin bilgiler sağlanmaktadır.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: spark
ms.date: 04/15/2020
ms.author: prgomata
ms.reviewer: euang
ms.openlocfilehash: ebf948fdb1df76cb7bcb03ee5d85f581d856524f
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85918729"
---
# <a name="introduction"></a>Giriş

Azure SYNAPSE Apache Spark to SYNAPSE SQL Connector, Azure SYNAPSE 'te Spark havuzları (Önizleme) ve SQL havuzları arasında verileri verimli bir şekilde aktarmak için tasarlanmıştır. Azure SYNAPSE Apache Spark to SYNAPSE SQL Connector yalnızca SQL havuzlarında çalışır, isteğe bağlı SQL ile çalışmaz.

## <a name="design"></a>Tasarım

Spark havuzları ve SQL havuzları arasında veri aktarımı, JDBC kullanılarak yapılabilir. Bununla birlikte, Spark ve SQL havuzları gibi iki Dağıtılmış Sistem, JDBC, seri veri aktarımı ile ilgili bir performans sorunu olduğunu eğilimi gösterir.

Azure SYNAPSE Apache Spark havuzu SYNAPSE SQL Bağlayıcısı Apache Spark için bir veri kaynağı uygulamasıdır. Spark kümesi ve SYNAPSE SQL örneği arasında verileri verimli bir şekilde aktarmak için SQL havuzlarındaki Azure Data Lake Storage 2. ve PolyBase 'i kullanır.

![Bağlayıcı mimarisi](./media/synapse-spark-sqlpool-import-export/arch1.png)

## <a name="authentication-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te kimlik doğrulaması

Sistemler arasında kimlik doğrulaması, Azure SYNAPSE Analytics 'te sorunsuz hale getirilir. Depolama hesabına veya veri ambarı sunucusuna erişirken kullanılacak güvenlik belirteçlerini almak için Azure Active Directory ile bağlanan bir belirteç hizmeti vardır.

Bu nedenle, depolama hesabında ve veri ambarı sunucusunda AAD kimlik doğrulaması yapılandırıldığı sürece kimlik bilgileri oluşturmanız veya bunları bağlayıcı API 'de belirtmeniz gerekmez. Aksi takdirde, SQL kimlik doğrulaması belirlenebilir. [Kullanım](#usage) bölümünde daha fazla ayrıntı bulun.

## <a name="constraints"></a>Kısıtlamalar

- Bu bağlayıcı yalnızca Scala 'da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

- Veri aktarmak istediğiniz veritabanında/SQL havuzunda **db_exporter** rolün bir üyesi olmalıdır.
- Varsayılan depolama hesabındaki Depolama Blobu veri katılımcısı rolünün bir üyesi olmalıdır.

Kullanıcı oluşturmak için SQL havuzu veritabanına bağlanın ve aşağıdaki örnekleri izleyin:

```sql
--SQL User
CREATE USER Mary FROM LOGIN Mary;

--Azure Active Directory User
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Rol atamak için:

```sql
--SQL User
EXEC sp_addrolemember 'db_exporter', 'Mary';

--Azure Active Directory User
EXEC sp_addrolemember 'db_exporter',[mike@contoso.com]
```

## <a name="usage"></a>Kullanım

İçeri aktarma deyimleri gerekli değildir, Not defteri deneyimi için önceden içeri aktarırlar.

### <a name="transferring-data-to-or-from-a-sql-pool-attached-with-the-workspace"></a>Çalışma alanıyla eklenmiş bir SQL havuzuna veya veri aktarımı

> [!NOTE]
> **Not defteri deneyiminde içeri aktarmalar gerekmez**

```scala
 import com.microsoft.spark.sqlanalytics.utils.Constants
 import org.apache.spark.sql.SqlAnalyticsConnector._
```

#### <a name="read-api"></a>API 'YI oku

```scala
val df = spark.read.sqlanalytics("<DBName>.<Schema>.<TableName>")
```

Yukarıdaki API, hem Iç (yönetilen) hem de SQL havuzundaki dış tablolar için çalışacaktır.

#### <a name="write-api"></a>Yazma API 'SI

```scala
df.write.sqlanalytics("<DBName>.<Schema>.<TableName>", <TableType>)
```

Yazma API 'SI, tabloyu SQL havuzunda oluşturur ve sonra verileri yüklemek için PolyBase 'i çağırır.  Tablo SQL havuzunda yer almıyor veya "zaten var olan bir nesne var.." bildiren hata döndürülür.

TableType değerleri

- Sabitler. SQL havuzundaki Iç yönetilen tablo
- SQL havuzundaki sabitler. EXTERNAL-External tablosu

SQL havuzu yönetilen tablosu

```scala
df.write.sqlanalytics("<DBName>.<Schema>.<TableName>", Constants.INTERNAL)
```

SQL havuzu dış tablosu

SQL havuzu dış tablosuna yazmak için, SQL havuzunda bir dış VERI kaynağı ve bir dış dosya BIÇIMI bulunmalıdır.  Daha fazla bilgi için SQL havuzunda [dış veri kaynağı](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ve [Harici dosya biçimleri](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) oluşturma makalesini okuyun.  Aşağıda, SQL havuzunda dış veri kaynağı ve harici dosya biçimleri oluşturma örnekleri verilmiştir.

```sql
--For an external table, you need to pre-create the data source and file format in SQL pool using SQL queries:
CREATE EXTERNAL DATA SOURCE <DataSourceName>
WITH
  ( LOCATION = 'abfss://...' ,
    TYPE = HADOOP
  ) ;

CREATE EXTERNAL FILE FORMAT <FileFormatName>
WITH (  
    FORMAT_TYPE = PARQUET,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);
```

Depolama hesabına Azure Active Directory geçişli kimlik doğrulaması kullanılırken bir dış KIMLIK BILGISI nesnesi gerekli değildir.  Depolama hesabındaki "Depolama Blobu verileri katılımcısı" rolünün bir üyesi olduğunuzdan emin olun.

```scala

df.write.
    option(Constants.DATA_SOURCE, <DataSourceName>).
    option(Constants.FILE_FORMAT, <FileFormatName>).
    sqlanalytics("<DBName>.<Schema>.<TableName>", Constants.EXTERNAL)

```

### <a name="if-you-are-transferring-data-to-or-from-a-sql-pool-or-database-outside-the-workspace"></a>Bir SQL havuzuna veya çalışma alanı dışında bir veritabanına veri aktardıysanız

> [!NOTE]
> Not defteri deneyiminde içeri aktarmalar gerekmez

```scala
 import com.microsoft.spark.sqlanalytics.utils.Constants
 import org.apache.spark.sql.SqlAnalyticsConnector._
```

#### <a name="read-api"></a>API 'YI oku

```scala
val df = spark.read.
option(Constants.SERVER, "samplews.database.windows.net").
sqlanalytics("<DBName>.<Schema>.<TableName>")
```

#### <a name="write-api"></a>Yazma API 'SI

```scala
df.write.
option(Constants.SERVER, "samplews.database.windows.net").
sqlanalytics("<DBName>.<Schema>.<TableName>", <TableType>)
```

### <a name="using-sql-auth-instead-of-aad"></a>AAD yerine SQL auth kullanma

#### <a name="read-api"></a>API 'YI oku

Şu anda bağlayıcı, belirteç tabanlı kimlik doğrulamasını çalışma alanının dışında olan bir SQL havuzuna desteklemez. SQL kimlik doğrulaması kullanmanız gerekir.

```scala
val df = spark.read.
option(Constants.SERVER, "samplews.database.windows.net").
option(Constants.USER, <SQLServer Login UserName>).
option(Constants.PASSWORD, <SQLServer Login Password>).
sqlanalytics("<DBName>.<Schema>.<TableName>")
```

#### <a name="write-api"></a>Yazma API 'SI

```scala
df.write.
option(Constants.SERVER, "samplews.database.windows.net").
option(Constants.USER, <SQLServer Login UserName>).
option(Constants.PASSWORD, <SQLServer Login Password>).
sqlanalytics("<DBName>.<Schema>.<TableName>", <TableType>)
```

### <a name="using-the-pyspark-connector"></a>PySpark bağlayıcısını kullanma

> [!NOTE]
> Bu örnek yalnızca akılda tutulan Not defteri deneyimiyle verilmiştir.

DW içine yazmak istediğiniz bir "pyspark_df" veri çerçevesine sahip olduğunu varsayalım.

PySpark içindeki dataframe kullanarak geçici bir tablo oluşturun:

```py
pyspark_df.createOrReplaceTempView("pysparkdftemptable")
```

Mıknatıccs kullanarak PySpark not defterinde bir Scala hücresi çalıştırın:

```scala
%%spark
val scala_df = spark.sqlContext.sql ("select * from pysparkdftemptable")

pysparkdftemptable.write.sqlanalytics("sqlpool.dbo.PySparkTable", Constants.INTERNAL)
```

Benzer şekilde, okuma senaryosunda, Scala kullanarak verileri okuyun ve geçici bir tabloya yazın ve geçici tabloyu bir veri çerçevesinde sorgulamak için PySpark içinde Spark SQL kullanın.

## <a name="allowing-other-users-to-use-the-azure-synapse-apache-spark-to-synapse-sql-connector-in-your-workspace"></a>Diğer kullanıcıların, Azure SYNAPSE Apache Spark, çalışma alanınızdaki SQL bağlayıcısını SYNAPSE için kullanmasına izin verme

Diğer kullanıcıların eksik izinlerini değiştirmek için, çalışma alanına bağlı ADLS 2. depolama hesabında Depolama Blobu veri sahibi olmanız gerekir. Kullanıcının çalışma alanına erişimi olduğundan ve not defterlerini çalıştırma izinlerine sahip olduğundan emin olun.

### <a name="option-1"></a>1\. Seçenek

- Kullanıcıya Depolama Blobu verileri katılımcısı/Owner yapın

### <a name="option-2"></a>2\. Seçenek

- Klasör yapısında aşağıdaki ACL 'Leri belirtin:

| Klasör | / | synapse | çalışma alanı  | \<workspacename> | Mini veri havuzları | \<sparkpoolname>  | parlak havuzörnekleri  |
|--|--|--|--|--|--|--|--|
| Erişim Izinleri | --X | --X | --X | --X | --X | --X | -WX |
| Varsayılan Izinler | ---| ---| ---| ---| ---| ---| ---|

- "SYNAPSE" ve Azure portal 'den aşağı doğru tüm klasörlere ACL 'leri sağlayabilmelisiniz. Kök "/" klasörüne ACL eklemek için lütfen aşağıdaki yönergeleri izleyin.

- AAD kullanarak Depolama Gezgini çalışma alanıyla bağlantılı depolama hesabına bağlanma
- Hesabınızı seçin ve çalışma alanı için ADLS 2. URL 'sini ve varsayılan dosya sistemini verin
- Listelenen depolama hesabını gördüğünüzde, liste çalışma alanına sağ tıklayın ve "erişimi yönet" i seçin
- Kullanıcıyı "Yürüt" erişim Izniyle/klasörüne ekleyin. "Tamam" ı seçin

> [!IMPORTANT]
> ' Yi planlamıyorsanız "varsayılan" seçeneğini belirlediğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal kullanarak bir SQL havuzu oluşturma](../../synapse-analytics/quickstart-create-apache-spark-pool-portal.md)
- [Azure portal kullanarak yeni bir Apache Spark havuzu oluşturma](../../synapse-analytics/quickstart-create-apache-spark-pool-portal.md) 
