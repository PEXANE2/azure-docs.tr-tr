---
title: Spark kullanarak Cassandra API tablo verilerini okuyun
titleSufix: Azure Cosmos DB
description: Bu makalede, Azure Cosmos DB'deki Cassandra API tablolarından alınan verilerin nasıl okunduğu açıklanmaktadır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 01a9582062d8eb0d039473a03901fc83fe179020
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60893419"
---
# <a name="read-data-from-azure-cosmos-db-cassandra-api-tables-using-spark"></a>Azure Cosmos DB Cassandra API tablolarından verileri Spark'ı kullanarak okuyun

 Bu makalede, Azure Cosmos DB Cassandra API'de depolanan verilerin Spark'tan nasıl okunduğu açıklanmaktadır.

## <a name="cassandra-api-configuration"></a>Cassandra API yapılandırması
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
## <a name="dataframe-api"></a>Veri çerçevesi API'si

### <a name="read-table-using-sessionreadformat-command"></a>session.read.format komutunu kullanarak tabloyu okuma

```scala
val readBooksDF = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load

readBooksDF.explain
readBooksDF.show
```
### <a name="read-table-using-sparkreadcassandraformat"></a>spark.read.cassandraFormat kullanarak tabloyu okuyun 

```scala
val readBooksDF = spark.read.cassandraFormat("books", "books_ks", "").load()
```

### <a name="read-specific-columns-in-table"></a>Tabloda belirli sütunları okuma

```scala
val readBooksDF = spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load
  .select("book_name","book_author", "book_pub_year")

readBooksDF.printSchema
readBooksDF.explain
readBooksDF.show
```

### <a name="apply-filters"></a>Filtreleri uygulama

Şu anda yüklem pushdown desteklenmez, aşağıdaki örnekler istemci tarafı filtreleme yansıtır. 

```scala
val readBooksDF = spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load
  .select("book_name","book_author", "book_pub_year")
  .filter("book_pub_year > 1891")
//.filter("book_name IN ('A sign of four','A study in scarlet')")
//.filter("book_name='A sign of four' OR book_name='A study in scarlet'")
//.filter("book_author='Arthur Conan Doyle' AND book_pub_year=1890")
//.filter("book_pub_year=1903")  

readBooksDF.printSchema
readBooksDF.explain
readBooksDF.show
```

## <a name="rdd-api"></a>RDD API

### <a name="read-table"></a>Tabloyu oku
```scala
val bookRDD = sc.cassandraTable("books_ks", "books")
bookRDD.take(5).foreach(println)
```

### <a name="read-specific-columns-in-table"></a>Tabloda belirli sütunları okuma

```scala
val booksRDD = sc.cassandraTable("books_ks", "books").select("book_id","book_name").cache
booksRDD.take(5).foreach(println)
```

## <a name="sql-views"></a>SQL Görünümleri 

### <a name="create-a-temporary-view-from-a-dataframe"></a>Veri çerçevesinden geçici görünüm oluşturma

```scala
spark
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load.createOrReplaceTempView("books_vw")
```

### <a name="run-queries-against-the-view"></a>Sorguları görünüme karşı çalıştırma

```sql
select * from books_vw where book_pub_year > 1891
```

## <a name="next-steps"></a>Sonraki adımlar

Spark'tan Azure Cosmos DB Cassandra API ile çalışma yla ilgili ek makaleler şunlardır:
 
 * [İşlemleri yukarı yaslar](cassandra-spark-upsert-ops.md)
 * [İşlemleri silme](cassandra-spark-delete-ops.md)
 * [Toplama işlemleri](cassandra-spark-aggregation-ops.md)
 * [Tablo kopyalama işlemleri](cassandra-spark-table-copy-ops.md)

