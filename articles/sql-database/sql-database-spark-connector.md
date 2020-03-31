---
title: Azure SQL Veritabanı ve SQL Server ile Kıvılcım Konektörü
description: Azure SQL Veritabanı ve SQL Server için Kıvılcım Bağlayıcısını nasıl kullanacağınızı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: denzilribeiro
ms.author: denzilr
ms.reviewer: carlrab
ms.date: 09/25/2018
ms.openlocfilehash: b22ec475c0281a54d65921bc450b35723aa23219
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471661"
---
# <a name="accelerate-real-time-big-data-analytics-with-spark-connector-for-azure-sql-database-and-sql-server"></a>Azure SQL Veritabanı ve SQL Server için Spark konektörü ile gerçek zamanlı büyük veri analitiğini hızlandırın

Azure SQL Veritabanı ve SQL Server için Spark bağlayıcısı, Azure SQL Veritabanı ve SQL Server da dahil olmak üzere SQL veritabanlarının Spark işleri için giriş veri kaynağı veya çıktı veri stoğu görevi görebilmesini sağlar. Büyük veri analitiğinde gerçek zamanlı işlem verilerini kullanmanıza ve geçici sorgular veya raporlama için sonuçları kalıcı hale getirmek için izin verir. Yerleşik JDBC bağlayıcısı ile karşılaştırıldığında, bu bağlayıcı SQL veritabanlarına veri toplu ekleme yeteneği sağlar. 10x ila 20x daha hızlı performans ile satır satır ekleme daha iyi performans gösterebilirsiniz. Azure SQL Veritabanı ve SQL Server için Kıvılcım bağlayıcısı da AAD kimlik doğrulamasını destekler. AAD hesabınızı kullanarak Azure Databricks'ten Azure SQL veritabanınıza güvenli bir şekilde bağlanmanızı sağlar. Yerleşik JDBC konektörü ile benzer arabirimler sağlar. Bu yeni bağlayıcıyı kullanmak için varolan Spark işlerinizi geçirmek kolaydır.

## <a name="download"></a>İndirme
Başlamak için GitHub'daki [azure-sqldb-spark deposundan](https://github.com/Azure/azure-sqldb-spark) Sql DB konektörüne Spark'ı indirin.

## <a name="official-supported-versions"></a>Resmi Desteklenen Sürümler

| Bileşen                            | Sürüm                  |
| :----------------------------------- | :----------------------- |
| Apache Spark                         | 2.0.2 veya sonrası           |
| Scala                                | 2.10 veya sonrası            |
| SQL Server için Microsoft JDBC sürücüsü | 6.2 veya sonrası             |
| Microsoft SQL Server                 | SQL Server 2008 veya üstü |
| Azure SQL Veritabanı                   | Destekleniyor                |

Azure SQL Veritabanı ve SQL Server için Spark bağlayıcısı, verileri Spark alt düğümleri ve SQL veritabanları arasında taşımak için SQL Server için Microsoft JDBC Driver'ı kullanır:
 
Veri akışı aşağıdaki gibidir:
1. Spark ana düğümü SQL Server veya Azure SQL Veritabanı'na bağlanır ve belirli bir tablodan veya belirli bir SQL sorgusukullanarak verileri yükler
2. Spark ana düğümü dönüştürme için alt düğümlere veri dağıtır. 
3. İşçi düğümü SQL Server veya Azure SQL Veritabanı'na bağlanır ve veritabanına veri yazar. Kullanıcı satır satır ekleme veya toplu ekleme kullanmayı seçebilir.

Aşağıdaki diyagram veri akışını göstermektedir.

   ![architecture](./media/sql-database-spark-connector/architecture.png)

### <a name="build-the-spark-to-sql-db-connector"></a>Kıvılcımdan SQL DB konektörüne oluşturun
Şu anda, bağlayıcı proje maven kullanır. Bağlayıcıyı bağımlılık olmadan oluşturmak için şunları çalıştırabilirsiniz:

- mvn temiz paket
- JAR'ın en son sürümlerini sürüm klasöründen indirin
- SQL DB Kıvılcım JAR'ı ekleyin

## <a name="connect-spark-to-sql-db-using-the-connector"></a>Konektörü kullanarak Spark'ı SQL DB'ye bağlayın
Spark işlerinden Azure SQL Veritabanı'na veya SQL Server'a bağlanabilir, veri okuyabilir veya yazabilirsiniz. Bir Azure SQL veritabanında veya SQL Server veritabanında bir DML veya DDL sorgusu da çalıştırabilirsiniz.

### <a name="read-data-from-azure-sql-database-or-sql-server"></a>Azure SQL Veritabanı veya SQL Server'daki verileri okuma

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
### <a name="reading-data-from-azure-sql-database-or-sql-server-with-specified-sql-query"></a>Belirtilen SQL sorgusuyla Azure SQL Veritabanı veya SQL Server'dan veri okuma
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

### <a name="write-data-to-azure-sql-database-or-sql-server"></a>Azure SQL Veritabanı'na veya SQL Server'a veri yazma
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

### <a name="run-dml-or-ddl-query-in-azure-sql-database-or-sql-server"></a>Azure SQL Veritabanı veya SQL Server'da DML veya DDL sorgusu nu çalıştırma
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

## <a name="connect-spark-to-azure-sql-database-using-aad-authentication"></a>AAD kimlik doğrulamasını kullanarak Spark'ı Azure SQL Veritabanına bağlayın
Azure Active Directory (AAD) kimlik doğrulamasını kullanarak Azure SQL Veritabanı'na bağlanabilirsiniz. Veritabanı kullanıcılarının kimliklerini merkezi olarak yönetmek ve SQL Server kimlik doğrulamasına alternatif olarak AAD kimlik doğrulamasını kullanın.
### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>ActiveDirectoryPassword Kimlik Doğrulama Modu kullanarak bağlanma
#### <a name="setup-requirement"></a>Kurulum Gereksinimi
ActiveDirectoryPassword kimlik doğrulama modunu kullanıyorsanız, [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) ve bağımlılıklarını indirmeniz ve bunları Java yapı yoluna eklemeniz gerekir.

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

### <a name="connecting-using-access-token"></a>Access Token kullanarak bağlanma
#### <a name="setup-requirement"></a>Kurulum Gereksinimi
Access token tabanlı kimlik doğrulama modunu kullanıyorsanız, [azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) ve bağımlılıklarını indirmeniz ve bunları Java yapı yoluna eklemeniz gerekir.

Azure SQL veritabanınıza nasıl erişim elde edileceksiniz öğrenmek [için SQL Veritabanı ile kimlik doğrulama için Azure Active Directory Authentication'ı kullanın'](sql-database-aad-authentication.md) a bakın.

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

## <a name="write-data-to-azure-sql-database-or-sql-server-using-bulk-insert"></a>Toplu Ekleme'yi kullanarak verileri Azure SQL veritabanına veya SQL Server'a yazma
Geleneksel jdbc bağlayıcısı, satır satır ekleme kullanarak verileri Azure SQL veritabanına veya SQL Server'a yazar. Toplu ekleme yi kullanarak SQL veritabanına veri yazmak için Spark to SQL DB bağlayıcısı kullanabilirsiniz. Büyük veri kümeleri yüklerken veya bir sütun deposu dizininin kullanıldığı tablolara veri yüklerken yazma performansını önemli ölçüde artırır.

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
Henüz yapmadıysanız, Azure SQL Veritabanı ve SQL Server için Spark konektörünü [azure-sqldb-spark GitHub deposundan](https://github.com/Azure/azure-sqldb-spark) indirin ve repo'daki ek kaynakları keşfedin:

- [Azure Databricks dizüstü bilgisayarlarını örnekle](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [Örnek komut dosyaları (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

[Ayrıca Apache Spark SQL, DataFrames ve Datasets Kılavuzu](https://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure Databricks belgelerini](https://docs.microsoft.com/azure/azure-databricks/)de gözden geçirmek isteyebilirsiniz.

