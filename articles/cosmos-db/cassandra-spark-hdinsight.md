---
title: HDInsight ile Azure Cosmos DB Cassandra API'ye İplik'te Kıvılcım'dan erişin
description: Bu makale, HDInsight ile İplik'teki Spark'tan Azure Cosmos DB Cassandra API ile nasıl çalışılabildiğini kapsar
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: bef4ee14cb4a7d64d80dc5776d8ecea0f831881a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887641"
---
# <a name="access-azure-cosmos-db-cassandra-api-from-spark-on-yarn-with-hdinsight"></a>HDInsight ile Azure Cosmos DB Cassandra API'ye İplik'te Kıvılcım'dan erişin

Bu makalede, Spark-shell'den HDInsight-Spark ile Yarn'daki Spark'tan Azure Cosmos DB Cassandra API'ye nasıl erişilenler ele allanmıştır. HDInsight, Microsoft'un Azure'daki Hortonworks Hadoop PaaS'ıdır ve HDFS için nesne depolamadan yararlanır ve [Spark](../hdinsight/spark/apache-spark-overview.md)da dahil olmak üzere çeşitli tatlarda sunulur.  Bu belgedeki içerik HDInsight-Spark'a atıfta bulunulsa da, tüm Hadoop dağıtımları için geçerlidir.  

## <a name="prerequisites"></a>Ön koşullar

* [Sağlama Azure Cosmos DB Cassandra API](create-cassandra-dotnet.md#create-a-database-account)

* [Azure Cosmos DB Cassandra API'ye bağlanmanın temellerini gözden geçirin](cassandra-spark-generic.md)

* [HDInsight-Spark kümesini sağlama](../hdinsight/spark/apache-spark-jupyter-spark-sql.md)

* [Cassandra API ile çalışmak için kod örneklerini gözden geçirin](cassandra-spark-generic.md#next-steps)

* [Eğer çok tercih ederseniz doğrulama için cqlsh kullanın](cassandra-spark-generic.md#connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Spark2'deki Cassandra API yapılandırması** - Cassandra için Kıvılcım konektörü, Cassandra bağlantı ayrıntılarının Spark bağlamının bir parçası olarak başlatılmasını gerektirir. Bir Jupyter dizüstü bilgisayar başlattığınızda, kıvılcım oturumu ve bağlam zaten başlatılır ve HDInsight varsayılan Jupyter dizüstü bilgisayar başlatmanın bir parçası olarak ayarlanmış her yapılandırma ile tamamlanmadığı sürece Spark bağlamını durdurmak ve yeniden başlatmanız tavsiye edilmez. Bir geçici çözüm Ambari, Spark2 hizmet yapılandırmadoğrudan Cassandra örnek ayrıntıları eklemektir. Bu, spark2 hizmet yeniden başlatmagerektiren küme başına tek seferlik bir etkinliktir.
 
  1. Ambari, Spark2 servisine gidin ve configs seçin

  2. Ardından özel spark2 varsayılanlarına gidin ve aşağıdakilerle yeni bir özellik ekleyin ve Spark2 hizmetini yeniden başlatın:

  ```scala
  spark.cassandra.connection.host=YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com<br>
  spark.cassandra.connection.port=10350<br>
  spark.cassandra.connection.ssl.enabled=true<br>
  spark.cassandra.auth.username=YOUR_COSMOSDB_ACCOUNT_NAME<br>
  spark.cassandra.auth.password=YOUR_COSMOSDB_KEY<br>
  ```

## <a name="access-azure-cosmos-db-cassandra-api-from-spark-shell"></a>Spark kabuğundan Azure Cosmos DB Cassandra API'ye erişin

Kıvılcım kabuğu test/keşif amacıyla kullanılır.

* Kümenizin Spark sürümüyle uyumlu gerekli maven bağımlılıklarıyla kıvılcım kabuğunu başlatın.

  ```scala
  spark-shell --packages "com.datastax.spark:spark-cassandra-connector_2.11:2.3.0,com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0"
  ```

* Bazı DDL ve DML işlemlerini yürütme

  ```scala
  import org.apache.spark.rdd.RDD
  import org.apache.spark.{SparkConf, SparkContext}

  import spark.implicits._
  import org.apache.spark.sql.functions._
  import org.apache.spark.sql.Column
  import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType,LongType,FloatType,DoubleType, TimestampType}
  import org.apache.spark.sql.cassandra._

  //Spark connector
  import com.datastax.spark.connector._
  import com.datastax.spark.connector.cql.CassandraConnector

  //CosmosDB library for multiple retry
  import com.microsoft.azure.cosmosdb.cassandra

  // Specify connection factory for Cassandra
  spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

  // Parallelism and throughput configs
  spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
  spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
  spark.conf.set("spark.cassandra.output.concurrent.writes", "100")
  spark.conf.set("spark.cassandra.concurrent.reads", "512")
  spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
  spark.conf.set("spark.cassandra.connection.keep_alive_ms", "60000000") //Increase this number as needed
  ```

* CRUD işlemlerini çalıştırın

  ```scala
  //1) Create table if it does not exist
  val cdbConnector = CassandraConnector(sc)
  cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000;"))

  //2) Delete data from potential prior runs
  cdbConnector.withSessionDo(session => session.execute("DELETE FROM books_ks.books WHERE book_id IN ('b00300','b00001','b00023','b00501','b09999','b01001','b00999','b03999','b02999','b000009');"))

  //3) Generate a few rows
  val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887,11.33),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890,22.45),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892,19.83),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893,14.22),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901,12.25)
  ).toDF("book_id", "book_author", "book_name", "book_pub_year","book_price")

  //4) Persist
  booksDF.write.mode("append").format("org.apache.spark.sql.cassandra").options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000")).save()

  //5) Read the data in the table
  spark.read.format("org.apache.spark.sql.cassandra").options(Map( "table" -> "books", "keyspace" -> "books_ks")).load.show
  ```

## <a name="access-azure-cosmos-db-cassandra-api-from-jupyter-notebooks"></a>Jupyter dizüstü bilgisayarlardan Azure Cosmos DB Cassandra API'ye erişin

HDInsight-Spark, Zeppelin ve Jupyter dizüstü bilgisayar hizmetleriyle birlikte gelir. Her ikisi de Scala ve Python'u destekleyen web tabanlı dizüstü bilgisayar ortamlarıdır. Dizüstü bilgisayarlar etkileşimli keşif analizleri ve işbirliği için mükemmeldir, ancak operasyonel/üretime dayalı süreçler için değildir.

Aşağıdaki Jupyter dizüstü bilgisayarlar HDInsight Spark kümenize yüklenebilir ve Azure Cosmos DB Cassandra API ile çalışmak için hazır örnekler sağlayabilir. Azure Cosmos DB `1.0-ReadMe.ipynb` Cassandra API'ye bağlanmak için Spark hizmet yapılandırmasını inceleyen ilk dizüstü bilgisayarı gözden geçirdiğinizden emin olun.

Bu not defterlerini [azure-cosmos-db-cassandra-api-spark-notebooks-jupyter](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-jupyter/blob/master/scala/) altında makinenize indirin.
  
### <a name="how-to-upload"></a>Nasıl yüklenir:
Jupyter'ı fırlattığında, Scala'ya git. Önce bir dizin oluşturun ve ardından not defterlerini dizine yükleyin. Yükleme düğmesi üstte, sağ taraftadır.  

### <a name="how-to-run"></a>Nasıl çalıştırılabilen:
Not defterlerinde ve her not defteri hücresinde sırayla çalıştırın.  Tüm hücreleri çalıştırmak için her not defterinin üst kısmındaki çalıştır düğmesini tıklatın veya her hücre için shift+enter'ı tıklatın.

## <a name="access-with-azure-cosmos-db-cassandra-api-from-your-spark-scala-program"></a>Spark Scala programınızdan Azure Cosmos DB Cassandra API ile erişin

Üretimdeki otomatik işlemler için, Kıvılcım programları [spark-gönder](https://spark.apache.org/docs/latest/submitting-applications.html)yoluyla kümeye gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Nasıl bir IDE bir Spark Scala programı oluşturmak ve yürütme için Livy üzerinden HDInsight Spark kümesine göndermek](../hdinsight/spark/apache-spark-create-standalone-application.md)

* [Bir Spark Scala programından Azure Cosmos DB Cassandra API'ye bağlanma](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/master/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)

* [Cassandra API ile çalışmak için kod örneklerinin tam listesi](cassandra-spark-generic.md)
