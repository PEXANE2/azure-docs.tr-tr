---
title: Spark'tan Azure Cosmos DB Cassandra API'de DDL işlemleri
description: Bu makalede, Spark'tan Azure Cosmos DB Cassandra API'ye karşı anahtar alanı ve tablo DDL işlemleri ayrıntıları.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: c0df05eff5dc84ef24e1ed5afcaf705d99f447ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77622570"
---
# <a name="ddl-operations-in-azure-cosmos-db-cassandra-api-from-spark"></a>Spark'tan Azure Cosmos DB Cassandra API'de DDL işlemleri

Bu makalede, Spark'tan Azure Cosmos DB Cassandra API'ye karşı anahtar alanı ve tablo DDL işlemleri ayrıntıları.

## <a name="cassandra-api-related-configuration"></a>Cassandra API ile ilgili yapılandırma 

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

## <a name="keyspace-ddl-operations"></a>Keyspace DDL işlemleri

### <a name="create-a-keyspace"></a>Bir anahtar alanı oluşturma

```scala
//Cassandra connector instance
val cdbConnector = CassandraConnector(sc)

// Create keyspace
cdbConnector.withSessionDo(session => session.execute("CREATE KEYSPACE IF NOT EXISTS books_ks WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1 } "))
```

#### <a name="validate-in-cqlsh"></a>Cqlsh içinde doğrulayın

Cqlsh aşağıdaki komutu çalıştırın ve daha önce oluşturduğunuz boşluk görmeniz gerekir.

```bash
DESCRIBE keyspaces;
```

### <a name="drop-a-keyspace"></a>Bir boşluk bırak

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP KEYSPACE books_ks"))
```

#### <a name="validate-in-cqlsh"></a>Cqlsh içinde doğrulayın

```bash
DESCRIBE keyspaces;
```
## <a name="table-ddl-operations"></a>Tablo DDL işlemleri

**Dikkat edilmesi gerekenler:**  

- Oluşturma tablosu deyimi kullanılarak tablo düzeyinde iş lenebilir.  
- Bir bölme anahtarı 20 GB veri depolayabilir.  
- Bir kayıt en fazla 2 MB veri depolayabilir.  
- Bir bölüm anahtar aralığı birden çok bölüm anahtarı depolayabilir.

### <a name="create-a-table"></a>Bir tablo oluşturma

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000 , WITH default_time_to_live=630720000;"))
```

#### <a name="validate-in-cqlsh"></a>Cqlsh içinde doğrulayın

Cqlsh aşağıdaki komutu çalıştırın ve "kitaplar" adlı tabloyu görmeniz gerekir: 

```bash
USE books_ks;
DESCRIBE books;
```

Sağlanan iş ve varsayılan TTL değerleri önceki komutun çıktısında gösterilmez, bu değerleri portaldan alabilirsiniz.

### <a name="alter-table"></a>Tabloyu değiştir

Alter table komutunu kullanarak aşağıdaki değerleri değiştirebilirsiniz:

* sağlanan iş girdisi 
* zaman-to-live değeri
<br>Sütun değişiklikleri şu anda desteklenmiyor.

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("ALTER TABLE books_ks.books WITH cosmosdb_provisioned_throughput=8000, WITH default_time_to_live=0;"))
```

### <a name="drop-table"></a>Açılan tablo

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP TABLE IF EXISTS books_ks.books;"))
```

#### <a name="validate-in-cqlsh"></a>Cqlsh içinde doğrulayın

Cqlsh aşağıdaki komutu çalıştırın ve "kitaplar" tablosu artık kullanılabilir olduğunu görmeniz gerekir:

```bash
USE books_ks;
DESCRIBE tables;
```

## <a name="next-steps"></a>Sonraki adımlar

Anahtar alanı ve tablo oluşturduktan sonra, CRUD işlemleri ve daha fazlası için aşağıdaki makalelere devam edin:
 
* [Oluşturma/ekleme işlemleri](cassandra-spark-create-ops.md)  
* [İşlemleri okuma](cassandra-spark-read-ops.md)  
* [İşlemleri yukarı yaslar](cassandra-spark-upsert-ops.md)  
* [İşlemleri silme](cassandra-spark-delete-ops.md)  
* [Toplama işlemleri](cassandra-spark-aggregation-ops.md)  
* [Tablo kopyalama işlemleri](cassandra-spark-table-copy-ops.md)  
