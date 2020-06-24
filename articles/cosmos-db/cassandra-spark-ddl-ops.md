---
title: Spark 'dan Azure Cosmos DB Cassandra API DDL işlemleri
description: Bu makalede, Spark 'dan Azure Cosmos DB Cassandra API karşı anahtar alanı ve tablo DDL işlemleri ayrıntılı olarak açıklanır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: 30cac5894998ca2bb9c37217820e1000ed97ba5d
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85260584"
---
# <a name="ddl-operations-in-azure-cosmos-db-cassandra-api-from-spark"></a>Spark 'dan Azure Cosmos DB Cassandra API DDL işlemleri

Bu makalede, Spark 'dan Azure Cosmos DB Cassandra API karşı anahtar alanı ve tablo DDL işlemleri ayrıntılı olarak açıklanır.

## <a name="cassandra-api-related-configuration"></a>Cassandra API ilgili yapılandırma 

```scala
import org.apache.spark.sql.cassandra._

//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

## <a name="keyspace-ddl-operations"></a>Anahtar uzayı DDL işlemleri

### <a name="create-a-keyspace"></a>Anahtar alanı oluştur

```scala
//Cassandra connector instance
val cdbConnector = CassandraConnector(sc)

// Create keyspace
cdbConnector.withSessionDo(session => session.execute("CREATE KEYSPACE IF NOT EXISTS books_ks WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1 } "))
```

#### <a name="validate-in-cqlsh"></a>Csqlsh 'te doğrulama

Csqlsh içinde aşağıdaki komutu çalıştırın ve daha önce oluşturduğunuz anahtar alanı 'i görmeniz gerekir.

```bash
DESCRIBE keyspaces;
```

### <a name="drop-a-keyspace"></a>Anahtar alanını bırakma

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP KEYSPACE books_ks"))
```

#### <a name="validate-in-cqlsh"></a>Csqlsh 'te doğrulama

```bash
DESCRIBE keyspaces;
```
## <a name="table-ddl-operations"></a>Tablo DDL işlemleri

**Konuları**  

- Üretilen iş, tablo düzeyinde create table deyimleri kullanılarak atanabilir.  
- Bir bölüm anahtarı 20 GB veri saklayabilir.  
- Bir kayıt, en fazla 2 MB veri saklayabilir.  
- Bir bölüm anahtarı aralığı, birden çok bölüm anahtarını saklayabilir.

### <a name="create-a-table"></a>Bir tablo oluşturma

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000 , WITH default_time_to_live=630720000;"))
```

#### <a name="validate-in-cqlsh"></a>Csqlsh 'te doğrulama

Csqlsh içinde aşağıdaki komutu çalıştırın ve "kitaplar:" adlı tabloyu görmeniz gerekir. 

```bash
USE books_ks;
DESCRIBE books;
```

Sağlanan aktarım hızı ve varsayılan TTL değerleri önceki komutun çıkışında gösterilmez, portaldan bu değerleri alabilirsiniz.

### <a name="alter-table"></a>Alter table

Alter table komutunu kullanarak aşağıdaki değerleri değiştirebilirsiniz:

* sağlanan aktarım hızı 
* yaşam süresi değeri
<br>Sütun değişiklikleri şu anda desteklenmiyor.

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("ALTER TABLE books_ks.books WITH cosmosdb_provisioned_throughput=8000, WITH default_time_to_live=0;"))
```

### <a name="drop-table"></a>Tabloyu bırak

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP TABLE IF EXISTS books_ks.books;"))
```

#### <a name="validate-in-cqlsh"></a>Csqlsh 'te doğrulama

Csqlsh içinde aşağıdaki komutu çalıştırın ve "Kitaplar" tablosunun artık kullanılabilir olmadığını görmeniz gerekir:

```bash
USE books_ks;
DESCRIBE tables;
```

## <a name="next-steps"></a>Sonraki adımlar

Anahtar alanı ve tablo oluşturduktan sonra CRUD işlemleri ve daha fazlası için aşağıdaki makalelere ilerleyin:
 
* [Oluşturma/ekleme işlemleri](cassandra-spark-create-ops.md)  
* [Okuma işlemleri](cassandra-spark-read-ops.md)  
* [Upsert işlemler](cassandra-spark-upsert-ops.md)  
* [Silme işlemleri](cassandra-spark-delete-ops.md)  
* [Toplama işlemleri](cassandra-spark-aggregation-ops.md)  
* [Tablo kopyalama işlemleri](cassandra-spark-table-copy-ops.md)  
