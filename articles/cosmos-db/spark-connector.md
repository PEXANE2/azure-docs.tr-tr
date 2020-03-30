---
title: Apache Spark'ı Azure Cosmos DB'ye bağlayın
description: Apache Spark'ı Azure Cosmos DB'ye bağlamanızı sağlayan Azure Cosmos DB Spark konektörü hakkında bilgi edinin.
author: tknandu
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: ramkris
ms.openlocfilehash: bbc97489f0c7045040dd0189b97946c2bd8fb3c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481645"
---
# <a name="accelerate-big-data-analytics-by-using-the-apache-spark-to-azure-cosmos-db-connector"></a>Apache Spark to Azure Cosmos DB konektörünü kullanarak büyük veri analitiğini hızlandırın

Cosmos DB Spark konektörünü kullanarak Azure Cosmos DB'de depolanan verilerle [Spark](https://spark.apache.org/) işlerini çalıştırabilirsiniz. Cosmos toplu iş ve akış işleme için ve düşük gecikme cezası erişimi için bir hizmet katmanı olarak kullanılabilir.

Azure'da yönetilen Spark kümeleri sağlayan [Azure Databricks](https://azure.microsoft.com/services/databricks) veya [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/)ile bağlayıcıyı kullanabilirsiniz. Aşağıdaki tablodesteklenen Spark sürümlerini gösterir.

| Bileşen | Sürüm |
|---------|-------|
| Apache Spark | 2.4.x, 2.3.x, 2.2.x ve 2.1.x |
| Scala | 2.11 |
| Azure Databricks çalışma zamanı sürümü | > 3.4 |

> [!WARNING]
> Bu bağlayıcı, Azure Cosmos DB'nin çekirdek (SQL) API'sini destekler.
> MongoDB API için Cosmos DB için [MongoDB Kıvılcım konektörünü](https://docs.mongodb.com/spark-connector/master/)kullanın.
> Cosmos DB Cassandra API için [Cassandra Spark konektörünü](https://github.com/datastax/spark-cassandra-connector)kullanın.
>

## <a name="quickstart"></a>Hızlı Başlangıç

* Cosmos DB hesabı kurmak ve bazı verileri doldurmak için [Java SDK ile başlatılan](sql-api-async-java-get-started.md) adımları izleyin.
* [Azure Databricks'in](/azure/azure-databricks/quickstart-create-databricks-workspace-portal) bir Azure Databricks çalışma alanı ve kümesi kurmaya başladığı adımları izleyin.
* Artık yeni Not Defterleri oluşturabilir ve Cosmos DB bağlayıcı kitaplığını içe aktarabilirsiniz. Çalışma alanınızı nasıl ayarlayacağınızla ilgili ayrıntılar için [Cosmos DB konektörüyle çalışmaya](#bk_working_with_connector) devam edin.
* Aşağıdaki bölümde konektör kullanılarak okuma ve yazma ile ilgili parçacıklar bulunmaktadır.

### <a name="batch-reads-from-cosmos-db"></a>Toplu Cosmos DB okur

Aşağıdaki parçacık, PySpark'taki Cosmos DB'den okunacak bir Spark DataFrame'in nasıl oluşturulduğunu gösterir.

```python
# Read Configuration
readConfig = {
    "Endpoint": "https://doctorwho.documents.azure.com:443/",
    "Masterkey": "YOUR-KEY-HERE",
    "Database": "DepartureDelays",
    "Collection": "flights_pcoll",
    "query_custom": "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'" // Optional
}

# Connect via azure-cosmosdb-spark to create Spark DataFrame
flights = spark.read.format(
    "com.microsoft.azure.cosmosdb.spark").options(**readConfig).load()
flights.count()
```

Ve Scala'daki aynı kod parçacığı:

```scala
// Import Necessary Libraries
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Read Configuration
val readConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_pcoll",
  "query_custom" -> "SELECT c.date, c.delay, c.distance, c.origin, c.destination FROM c WHERE c.origin = 'SEA'" // Optional
))

// Connect via azure-cosmosdb-spark to create Spark DataFrame
val flights = spark.read.cosmosDB(readConfig)
flights.count()
```

### <a name="batch-writes-to-cosmos-db"></a>Toplu Cosmos DB yazıyor

Aşağıdaki parçacık PySpark'taki Cosmos DB'ye nasıl bir veri çerçevesi yazılabildiğini gösterir.

```python
# Write configuration
writeConfig = {
    "Endpoint": "https://doctorwho.documents.azure.com:443/",
    "Masterkey": "YOUR-KEY-HERE",
    "Database": "DepartureDelays",
    "Collection": "flights_fromsea",
    "Upsert": "true"
}

# Write to Cosmos DB from the flights DataFrame
flights.write.format("com.microsoft.azure.cosmosdb.spark").options(
    **writeConfig).save()
```

Ve Scala'daki aynı kod parçacığı:

```scala
// Write configuration

val writeConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_fromsea",
  "Upsert" : "true"
))

// Write to Cosmos DB from the flights DataFrame
import org.apache.spark.sql.SaveMode
flights.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

### <a name="streaming-reads-from-cosmos-db"></a>Cosmos DB'den akış okumaları

Aşağıdaki snippet, Azure Cosmos DB Change Feed'e nasıl bağlanıp okuneceğini gösterir.

```python
# Read Configuration
readConfig = {
    "Endpoint": "https://doctorwho.documents.azure.com:443/",
    "Masterkey": "YOUR-KEY-HERE",
    "Database": "DepartureDelays",
    "Collection": "flights_pcoll",
    "ReadChangeFeed": "true",
    "ChangeFeedQueryName": "Departure-Delays",
    "ChangeFeedStartFromTheBeginning": "false",
    "InferStreamSchema": "true",
    "ChangeFeedCheckpointLocation": "dbfs:/Departure-Delays"
}


# Open a read stream to the Cosmos DB Change Feed via azure-cosmosdb-spark to create Spark DataFrame
changes = (spark
           .readStream
           .format("com.microsoft.azure.cosmosdb.spark.streaming.CosmosDBSourceProvider")
           .options(**readConfig)
           .load())
```
Ve Scala'daki aynı kod parçacığı:

```scala
// Import Necessary Libraries
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.config.Config

// Read Configuration
val readConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_pcoll",
  "ReadChangeFeed" -> "true",
  "ChangeFeedQueryName" -> "Departure-Delays",
  "ChangeFeedStartFromTheBeginning" -> "false",
  "InferStreamSchema" -> "true",
  "ChangeFeedCheckpointLocation" -> "dbfs:/Departure-Delays"
))

// Open a read stream to the Cosmos DB Change Feed via azure-cosmosdb-spark to create Spark DataFrame
val df = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(readConfig).load()
```

### <a name="streaming-writes-to-cosmos-db"></a>Akış Cosmos DB yazıyor

Aşağıdaki parçacık PySpark'taki Cosmos DB'ye nasıl bir veri çerçevesi yazılabildiğini gösterir.

```python
# Write configuration
writeConfig = {
    "Endpoint": "https://doctorwho.documents.azure.com:443/",
    "Masterkey": "YOUR-KEY-HERE",
    "Database": "DepartureDelays",
    "Collection": "flights_fromsea",
    "Upsert": "true",
    "WritingBatchSize": "500",
    "CheckpointLocation": "/checkpointlocation_write1"
}

# Write to Cosmos DB from the flights DataFrame
changeFeed = (changes
              .writeStream
              .format("com.microsoft.azure.cosmosdb.spark.streaming.CosmosDBSinkProvider")
              .outputMode("append")
              .options(**writeconfig)
              .start())
```

Ve Scala'daki aynı kod parçacığı:

```scala
// Write configuration

val writeConfig = Config(Map(
  "Endpoint" -> "https://doctorwho.documents.azure.com:443/",
  "Masterkey" -> "YOUR-KEY-HERE",
  "Database" -> "DepartureDelays",
  "Collection" -> "flights_fromsea",
  "Upsert" -> "true",
  "WritingBatchSize" -> "500",
  "CheckpointLocation" -> "/checkpointlocation_write1"
))

// Write to Cosmos DB from the flights DataFrame
df
.writeStream
.format(classOf[CosmosDBSinkProvider].getName)
.options(writeConfig)
.start()
```
Daha fazla snippets ve sonuna kadar örnekleri, [Jupyter](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/notebooks)bakın.

## <a name="working-with-the-connector"></a><a name="bk_working_with_connector"></a>Konektörle çalışma

Konektörü GitHub'da kaynaktan oluşturabilir veya aşağıdaki bağlantılardan Maven'den uber kavanozlarını indirebilirsiniz.

| Spark | Scala | En son sürüm |
|---|---|---|
| 2.4.0 | 2.11 | [azure-cosmosdb-spark_2.4.0_2.11_1.4.0](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11/1.4.0/jar)
| 2.3.0 | 2.11 | [azure-cosmosdb-spark_2.3.0_2.11_1.3.3](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.3.0_2.11/1.3.3/jar)
| 2.2.0 | 2.11 | [azure-cosmosdb-spark_2.2.0_2.11_1.1.1](https://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-cosmosdb-spark_2.2.0_2.11%7C1.1.1%7Cjar)
| 2.1.0 | 2.11 | [azure-cosmosdb-spark_2.1.0_2.11_1.2.2](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.1.0_2.11/1.2.2/jar)

### <a name="using-databricks-notebooks"></a>Databricks not defterlerini kullanma

Azure Veri Tuğlaları Kılavuzu'ndaki kılavuzu izleyerek > [Azure Cosmos DB Spark konektörünü kullanarak](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html) Databricks çalışma alanınızı kullanarak bir kitaplık oluşturun

> [!NOTE]
> Azure **Cosmos DB Spark Bağlayıcısı'nı Kullan** sayfası şu anda güncel değil. Altı ayrı kavanozu altı farklı kütüphaneye indirmek yerine, uber kavanozu maven'den https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11/1.4.0/jar) indirebilir ve bu kavanozu/kütüphaneyi yükleyebilirsiniz.
> 

### <a name="using-spark-cli"></a>Kıvılcım-cli kullanma

Kıvılcım-cli kullanarak bağlayıcı ile çalışmak için `spark-shell`(yani, `pyspark` `spark-submit`, , ), `--packages` bağlayıcının [maven koordinatları](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.4.0_2.11)ile parametre kullanabilirsiniz.

```sh
spark-shell --master yarn --packages "com.microsoft.azure:azure-cosmosdb-spark_2.4.0_2.11:1.4.0"

```

### <a name="using-jupyter-notebooks"></a>Jupyter dizüstü bilgisayarları kullanma

HDInsight içinde Jupyter dizüstü bilgisayarları kullanıyorsanız, konektörün maven koordinatlarını belirtmek için kıvılcım sihirli `%%configure` hücrek kullanabilirsiniz.

```python
{ "name":"Spark-to-Cosmos_DB_Connector",
  "conf": {
    "spark.jars.packages": "com.microsoft.azure:azure-cosmosdb-spark_2.4.0_2.11:1.4.0",
    "spark.jars.excludes": "org.scala-lang:scala-reflect"
   }
   ...
}
```

> Ek olarak, bağlayıcı, Apache Spark ve Livy arasındaki olası çakışmaları kaldırmak için `spark.jars.excludes` özeldir.

### <a name="build-the-connector"></a>Konektörü oluşturun

Şu anda, bu `maven` bağlayıcı proje bağımlılıkları olmadan oluşturmak için kullanır, çalıştırabilirsiniz:

```sh
mvn clean package
```

## <a name="working-with-our-samples"></a>Örneklerimizle çalışma

[Cosmos DB Spark GitHub deposunda](https://github.com/Azure/azure-cosmosdb-spark) deneyebileceğiniz aşağıdaki örnek not defterleri ve komut dosyaları vardır.

* **Spark ve Cosmos DB (Seattle)** [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/On-Time%20Flight%20Performance%20with%20Spark%20and%20Cosmos%20DB%20-%20Seattle.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/On-Time%20Flight%20Performance%20with%20Spark%20and%20Cosmos%20DB%20-%20Seattle.html)ile Zamanında Uçuş Performansı : Spark SQL, GraphFrames vitrin hdInsight Jupyter dizüstü bilgisayar hizmeti kullanarak Cosmos DB spark bağlayın ve ML boru hatları kullanarak uçuş gecikmeleri tahmin.
* **Apache Spark ve Azure Cosmos DB Change Feed ile Twitter Kaynak**: [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Twitter%20with%20Spark%20and%20Azure%20Cosmos%20DB%20Change%20Feed.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Twitter%20with%20Spark%20and%20Azure%20Cosmos%20DB%20Change%20Feed.html)
* **Cosmos DB Grafikleri sorgulamak için Apache Spark**kullanma : [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Using%20Apache%20Spark%20to%20query%20Cosmos%20DB%20Graphs.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/notebooks/Using%20Apache%20Spark%20to%20query%20Cosmos%20DB%20Graphs.html)
* **[Azure Veri Tuğlalarını Azure Cosmos DB'ye bağlama.](https://docs.databricks.com/spark/latest/data-sources/azure/cosmosdb-connector.html)** `azure-cosmosdb-spark`  Burada bağlantılı da Zamanında Uçuş Performansı [dizüstü bir](https://github.com/dennyglee/databricks/tree/master/notebooks/Users/denny%40databricks.com/azure-databricks)Azure Databricks sürümüdür.
* **[Azure Cosmos DB ve HDInsight (Apache Spark) ile Lambda Architecture](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/readme.md)**: Cosmos DB ve Spark kullanarak büyük veri boru hatlarını korumanın operasyonel ek yükü azaltabilirsiniz.

## <a name="more-information"></a>Daha Fazla Bilgi

Biz de dahil `azure-cosmosdb-spark` olmak üzere [wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki) daha fazla bilgi var:

* [Azure Cosmos DB Kıvılcım Konektörü Kullanım Kılavuzu](https://github.com/Azure/azure-documentdb-spark/wiki/Azure-Cosmos-DB-Spark-Connector-User-Guide)
* [Toplama Örnekleri](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)

### <a name="configuration-and-setup"></a>Yapılandırma ve Kurulum

* [Kıvılcım Konektörü Yapılandırması](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuration-references)
* [Cosmos DB Bağlayıcı Kurulumu kıvılcım](https://github.com/Azure/azure-documentdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup) (devam ediyor)
* [Apache Spark (HDI) ile Power BI Direct Query'yi Azure Cosmos DB'ye yapılandırma](https://github.com/Azure/azure-cosmosdb-spark/wiki/Configuring-Power-BI-Direct-Query-to-Azure-Cosmos-DB-via-Apache-Spark-(HDI))

### <a name="troubleshooting"></a>Sorun giderme

* [Cosmos DB Agregalarını Kullanma](https://github.com/Azure/azure-documentdb-spark/wiki/Troubleshooting:-Using-Cosmos-DB-Aggregates)
* [Bilinen Sorunlar](https://github.com/Azure/azure-cosmosdb-spark/wiki/Known-Issues)

### <a name="performance"></a>Performans

* [Performans İpuçları](https://github.com/Azure/azure-cosmosdb-spark/wiki/Performance-tips)
* [Sorgu Test Çalışır](https://github.com/Azure/azure-documentdb-spark/wiki/Query-Test-Runs)
* [Yazma Test Çalıştırmaları](https://github.com/Azure/azure-cosmosdb-spark/wiki/Writing-Test-Runs)

### <a name="change-feed"></a>Değişiklik Akışı

* [Azure Cosmos DB Değişiklik Akışı ve Apache Spark kullanarak Akış İşleme Değişiklikleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)
* [Yem Demolarını Değiştir](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [Yapılandırılmış Akış Demoları](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)

### <a name="monitoring"></a>İzleme

* [Uygulama öngörüleri ile Kıvılcım işlerinin izlenmesi](https://github.com/Azure/azure-cosmosdb-spark/tree/2.3/samples/monitoring)

## <a name="next-steps"></a>Sonraki adımlar

Henüz yapmadıysanız, [Azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub deposundan Azure Cosmos DB konektörüne Kıvılcım'ı indirin. Repo'da aşağıdaki ek kaynakları keşfedin:

* [Toplama örnekleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Örnek komut dosyaları ve not defterleri](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

[Ayrıca Apache Spark SQL, DataFrames ve Datasets Kılavuzu'nu ve](https://spark.apache.org/docs/latest/sql-programming-guide.html)Azure [HDInsight'taki Apache Spark makalesini](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) de gözden geçirmek isteyebilirsiniz.
