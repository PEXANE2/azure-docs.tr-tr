---
title: Spark bağlayıcısını SQL Microsoft Azure ve SQL Server kullanma
description: Spark bağlayıcısını Microsoft Azure SQL ve SQL Server ile nasıl kullanacağınızı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: denzilribeiro
ms.author: denzilr
ms.reviewer: carlrab
ms.date: 09/25/2018
ms.openlocfilehash: b2e042f2c3a7c6e1528ff96fb4fb96f392274855
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84041230"
---
# <a name="accelerate-real-time-big-data-analytics-using-the-spark-connector"></a>Spark bağlayıcısını kullanarak gerçek zamanlı büyük veri analizlerini hızlandırma 
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Spark Bağlayıcısı, Azure SQL veritabanı, Azure SQL yönetilen örneği ve SQL Server veritabanlarının, Spark işleri için giriş veri kaynağı veya çıkış veri havuzu olarak davranmasını sağlar. Büyük veri analizindeki gerçek zamanlı işlem verilerini kullanmanızı ve geçici sorgular veya raporlama sonuçlarını kalıcı hale getirme olanağı sağlar. Yerleşik JDBC Bağlayıcısı ile karşılaştırıldığında, bu bağlayıcı Microsoft Azure SQL ve SQL Server veritabanlarına toplu veri ekleme olanağı sunar. Satır ekleyerek 10 kat daha hızlı performans ile 20 kat daha hızlı performans sağlayabilir. Spark Bağlayıcısı, Azure SQL veritabanlarına bağlanmak için AAD kimlik doğrulamasını destekler. AAD hesabınızı kullanarak Azure Databricks Azure SQL veritabanınıza güvenli bir şekilde bağlanmanızı sağlar. Yerleşik JDBC Bağlayıcısı ile benzer arabirimler sağlar. Mevcut Spark işlerinizin bu yeni bağlayıcıyı kullanmak için geçirilmesi kolaydır.

## <a name="download-and-build-spark-connector"></a>Spark bağlayıcısını indirme ve derleme

Başlamak için, GitHub 'daki [Azure-SQLDB-Spark deposundan](https://github.com/Azure/azure-sqldb-spark) Spark bağlayıcısını indirin.

### <a name="official-supported-versions"></a>Resmi desteklenen sürümler

| Bileşen                             | Sürüm                  |
| :-----------------------------------  | :----------------------- |
| Apache Spark                          | 2.0.2 veya üzeri           |
| Scala                                 | 2,10 veya üzeri            |
| SQL Server için Microsoft JDBC sürücüsü  | 6,2 veya üzeri             |
| Microsoft SQL Server                  | SQL Server 2008 veya üstü |
| Azure SQL Veritabanı                    | Destekleniyor                |
| Azure SQL Yönetilen Örnek            | Destekleniyor                |

Spark Bağlayıcısı, verileri Spark çalışan düğümleri ve SQL veritabanları arasında taşımak için Microsoft JDBC sürücüsü SQL Server kullanır:

Veri akışı aşağıdaki gibidir:

1. Spark ana düğümü bir Azure SQL veya SQL Server veritabanına bağlanır ve verileri belirli bir tablodan veya belirli bir SQL sorgusunu kullanarak yükler
2. Spark ana düğümü, dönüştürme için verileri çalışan düğümlerine dağıtır.
3. Çalışan düğümü bir Azure SQL veya SQL Server veritabanına bağlanır ve veritabanına veri yazar. Kullanıcı, satır satır ekleme veya toplu ekleme kullanmayı seçebilir.

Aşağıdaki diyagramda veri akışı gösterilmektedir.

   ![architecture](./media/spark-connector/architecture.png)

### <a name="build-the-spark-connector"></a>Spark bağlayıcısını oluşturma

Şu anda bağlayıcı projesi Maven kullanır. Bağlayıcıyı bağımlılıklar olmadan derlemek için şunu çalıştırabilirsiniz:

- MVN Temizleme paketi
- Yayın klasöründen JAR 'nin en son sürümlerini indirin
- SQL veritabanı Spark JAR 'i ekleme

## <a name="connect-and-read-data-using-the-spark-connector"></a>Spark bağlayıcısını kullanarak verileri bağlama ve okuma

Spark işinden bir Azure SQL veya SQL Server veritabanına bağlanabilirsiniz, verileri okuyabilir veya yazabilirsiniz. Ayrıca, Azure SQL ve SQL Server veritabanında bir DML veya DDL sorgusu da çalıştırabilirsiniz.

### <a name="read-data-from-azure-sql-and-sql-server"></a>Azure SQL ve SQL Server verilerini okuyun

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "dbTable"        -> "dbo.Clients",
  "user"           -> "username",
  "password"       -> "*********",
  "connectTimeout" -> "5", //seconds
  "queryTimeout"   -> "5"  //seconds
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="read-data-from-azure-sql-and-sql-server-with-specified-sql-query"></a>Azure SQL ve SQL Server belirtilen SQL sorgusuyla verileri okuma

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "queryCustom"  -> "SELECT TOP 100 * FROM dbo.Clients WHERE PostalCode = 98074" //Sql query
  "user"         -> "username",
  "password"     -> "*********",
))

//Read all data in table dbo.Clients
val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="write-data-to-azure-sql-and-sql-server"></a>Verileri Azure SQL 'e ve SQL Server yazma

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

// Aquire a DataFrame collection (val collection)

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "dbTable"      -> "dbo.Clients",
  "user"         -> "username",
  "password"     -> "*********"
))

import org.apache.spark.sql.SaveMode
collection.write.mode(SaveMode.Append).sqlDB(config)
```

### <a name="run-dml-or-ddl-query-in-azure-sql-and-sql-server"></a>Azure SQL ve SQL Server 'de DML veya DDL sorgusu çalıştırın

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.query._
val query = """
              |UPDATE Customers
              |SET ContactName = 'Alfred Schmidt', City = 'Frankfurt'
              |WHERE CustomerID = 1;
            """.stripMargin

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "user"         -> "username",
  "password"     -> "*********",
  "queryCustom"  -> query
))

sqlContext.sqlDBQuery(config)
```

## <a name="connect-from-spark-to-azure-sql-using-aad-authentication"></a>AAD kimlik doğrulaması kullanarak Spark 'tan Azure SQL 'e bağlanma

Azure SQL 'e Azure Active Directory (AAD) kimlik doğrulaması kullanarak bağlanabilirsiniz. Veritabanı kullanıcılarının kimliklerini merkezi olarak yönetmek için AAD kimlik doğrulamasını kullanın ve SQL Server kimlik doğrulamasına alternatif olarak.

### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>ActiveDirectoryPassword kimlik doğrulama modunu kullanarak bağlanma

#### <a name="setup-requirement"></a>Kurulum gereksinimi

ActiveDirectoryPassword kimlik doğrulama modunu kullanıyorsanız, [Azure-ActiveDirectory-Library-for-Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) ve bağımlılıklarını Indirmeniz ve Java derleme yoluna dahil etmeniz gerekir.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "user"           -> "username",
  "password"       -> "*********",
  "authentication" -> "ActiveDirectoryPassword",
  "encrypt"        -> "true"
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

### <a name="connecting-using-access-token"></a>Erişim belirteci kullanarak bağlanma

#### <a name="setup-requirement"></a>Kurulum gereksinimi

Erişim belirteci tabanlı kimlik doğrulama modunu kullanıyorsanız, [Azure-ActiveDirectory-Library-for-Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) ve bağımlılıklarını indirmeniz ve bunları Java derleme yoluna eklemeniz gerekir.

Azure SQL veritabanı veya Azure SQL yönetilen örneği 'nde veritabanınıza erişim belirteci alma hakkında bilgi edinmek için bkz. [kimlik doğrulaması için Azure Active Directory kimlik doğrulaması kullanma](authentication-aad-overview.md) .

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"                   -> "mysqlserver.database.windows.net",
  "databaseName"          -> "MyDatabase",
  "accessToken"           -> "access_token",
  "hostNameInCertificate" -> "*.database.windows.net",
  "encrypt"               -> "true"
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```

## <a name="write-data-to-azure-sql-and-sql-server-using-bulk-insert"></a>Azure SQL 'e veri yazma ve toplu ekleme kullanarak SQL Server

Geleneksel JDBC Bağlayıcısı verileri Azure SQL 'e ve satır satır ekleme kullanarak SQL Server yazar. Azure SQL 'e veri yazmak ve toplu ekleme kullanarak SQL Server Spark bağlayıcısını kullanabilirsiniz. Büyük veri kümelerini yüklerken yazma performansını önemli ölçüde geliştirir veya bir sütun deposu dizininin kullanıldığı tablolara veri yükleme.

```scala
import com.microsoft.azure.sqldb.spark.bulkcopy.BulkCopyMetadata
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

/**
  Add column Metadata.
  If not specified, metadata is automatically added
  from the destination table, which may suffer performance.
*/
var bulkCopyMetadata = new BulkCopyMetadata
bulkCopyMetadata.addColumnMetadata(1, "Title", java.sql.Types.NVARCHAR, 128, 0)
bulkCopyMetadata.addColumnMetadata(2, "FirstName", java.sql.Types.NVARCHAR, 50, 0)
bulkCopyMetadata.addColumnMetadata(3, "LastName", java.sql.Types.NVARCHAR, 50, 0)

val bulkCopyConfig = Config(Map(
  "url"               -> "mysqlserver.database.windows.net",
  "databaseName"      -> "MyDatabase",
  "user"              -> "username",
  "password"          -> "*********",
  "dbTable"           -> "dbo.Clients",
  "bulkCopyBatchSize" -> "2500",
  "bulkCopyTableLock" -> "true",
  "bulkCopyTimeout"   -> "600"
))

df.bulkCopyToSqlDB(bulkCopyConfig, bulkCopyMetadata)
//df.bulkCopyToSqlDB(bulkCopyConfig) if no metadata is specified.
```

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, [Azure-SQLDB-Spark GitHub deposundan](https://github.com/Azure/azure-sqldb-spark) Spark bağlayıcısını indirin ve depodaki ek kaynakları keşfedebilirsiniz:

- [Örnek Azure Databricks Not defterleri](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [Örnek betikler (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

Ayrıca, [Apache Spark SQL, DataFrames ve veri kümeleri kılavuzunu](https://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure Databricks belgelerini](https://docs.microsoft.com/azure/azure-databricks/)gözden geçirmek isteyebilirsiniz.
