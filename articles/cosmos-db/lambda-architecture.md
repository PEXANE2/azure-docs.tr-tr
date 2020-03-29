---
title: Azure Cosmos DB ve Apache Spark ile Lambda mimarisi
description: Bu makalede, Azure Cosmos DB, HDInsight ve Spark kullanarak bir lambda mimarisinin nasıl uygulanacağı açıklanmaktadır
ms.service: cosmos-db
author: tknandu
ms.author: ramkris
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 68ce06d8a2904bf99f58a53817444b2992b23501
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76719747"
---
# <a name="azure-cosmos-db-implement-a-lambda-architecture-on-the-azure-platform"></a>Azure Cosmos DB: Azure platformunda bir lambda mimarisi uygulayın 

Lambda mimarileri, büyük veri kümelerinin verimli bir şekilde işlenmesini sağlar. Lambda mimarileri, büyük verileri sorgulamada yer alan gecikme yi en aza indirmek için toplu işleme, akış işleme ve servis katmanı kullanır. 

Azure'da lambda mimarisi uygulamak için, gerçek zamanlı büyük veri analitiğini hızlandırmak için aşağıdaki teknolojileri birleştirebilirsiniz:
* [Azure Cosmos DB,](https://azure.microsoft.com/services/cosmos-db/)endüstrinin ilk küresel olarak dağıtılan, çok modelli veritabanı hizmeti. 
* Büyük ölçekli veri analizi uygulamaları çalıştıran bir işleme çerçevesi olan [Azure HDInsight için Apache Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/)
* Azure Cosmos [DB,](change-feed.md)HDInsight'ın işlemesi için toplu katmana yeni veriler aktaran akış değiştirme
* [Azure Cosmos DB Bağlayıcısına Kıvılcım](spark-connector.md)

Bu makalede, orijinal çok katmanlı tasarıma dayanan bir lambda mimarisinin temelleri ve işlemleri kolaylaştıran "yeniden yapılanmış" lambda mimarisinin yararları anlatılmaktadır.  

## <a name="what-is-a-lambda-architecture"></a>Lambda mimarisi nedir?
Lambda mimarisi, [Nathan Marz](https://twitter.com/nathanmarz)tarafından açıklandığı gibi toplu iş ve hız gecikme senaryoları adrese genel, ölçeklenebilir ve hataya dayanıklı veri işleme mimarisidir.

![Lambda mimarisini gösteren diyagram](./media/lambda-architecture/lambda-architecture-intro.png)

Kaynak: http://lambda-architecture.net/

Lambda mimarisinin temel ilkeleri yukarıdaki şemada aşağıdaki [http://lambda-architecture.net](http://lambda-architecture.net/)gibi açıklanmıştır.

 1. Tüm **veriler** hem *toplu işlem katmanına* hem *de* hız *katmanına*itilir.
 2. **Toplu işlem katmanında** bir ana veri kümesi (değişmez, yalnızca ek ham veri kümesi) vardır ve toplu iş görünümleri önceden hesaplar.
 3. **Servis katmanı,** hızlı sorgular için toplu görünümlere sahiptir. 
 4. **Hız katmanı** işlem süresini (servis katmanına) dengeler ve yalnızca son verilerle ilgilenir.
 5. Tüm sorgular, toplu görünümlerden ve gerçek zamanlı görünümlerden elde edilen sonuçları birleştirerek veya tek tek pingleyerek yanıtlanabilir.

Daha fazla okuduktan sonra, bu mimariyi yalnızca aşağıdakileri kullanarak uygulayabileceğiz:

* Azure Cosmos konteyner(ler)
* HDInsight (Apache Spark 2.1) kümesi
* Kıvılcım Konektörü [1.0](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.1.0_2.11/1.2.6/jar)

## <a name="speed-layer"></a>Hız katmanı

İşlemler açısından bakıldığında, verilerin doğru durumunu sağlarken iki veri akışı sağlamak karmaşık bir çaba olabilir. İşlemleri basitleştirmek için, toplu *katman* için durumu korumak için [Azure Cosmos DB değişiklik akışı desteğini](change-feed.md) kullanırken, hız *katmanınız*için *Feed API'sini Değiştir* üzerinden Azure Cosmos DB değişiklik günlüğünü açıklayın.  
![Lambda mimarisinin yeni veri, hız katmanı ve ana veri kümesi bölümünü vurgulayan diyagram](./media/lambda-architecture/lambda-architecture-change-feed.png)

Bu katmanlarda önemli olan:

 1. Tüm **veriler** *yalnızca* Azure Cosmos DB'ye itilir, böylece çoklu döküm sorunlarını önleyebilirsiniz.
 2. **Toplu işlem katmanında** bir ana veri kümesi (değişmez, yalnızca ek ham veri kümesi) vardır ve toplu iş görünümleri önceden hesaplar.
 3. **Hizmet katmanı** sonraki bölümde ele alınmıştır.
 4. **Hız katmanı,** Azure Cosmos DB değişiklik akışını okumak için HDInsight 'ı (Apache Spark) kullanır. Bu, verilerinizi kalıcı hale getirmek ve aynı anda sorgulayıp işlemenizi sağlar.
 5. Tüm sorgular, toplu görünümlerden ve gerçek zamanlı görünümlerden elde edilen sonuçları birleştirerek veya tek tek pingleyerek yanıtlanabilir.
 
### <a name="code-example-spark-structured-streaming-to-an-azure-cosmos-db-change-feed"></a>Kod Örneği: Azure Cosmos DB değişiklik akışına yapılandırılmış akışı kıvılcımla
**Hız katmanının**bir parçası olarak Azure Cosmos DB değiştirme akışının hızlı bir prototipini çalıştırmak için, [Azure Cosmos DB Değişiklik Akışı ve Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark) örneğini kullanarak Akış İşleme Değişiklikleri'nin bir parçası olarak Twitter verilerini kullanarak bunu test edebilirsiniz. Twitter çıktınızı başlatmak [için, Twitter'dan Cosmos DB'ye stream akışındaki](https://github.com/tknandu/TwitterCosmosDBFeed)kod örneğine bakın. Önceki örnekte, Twitter verilerini Azure Cosmos DB'ye yüklüyorsunuz ve daha sonra değişiklik akışına bağlanmak için HDInsight (Apache Spark) kümenizi ayarlayabilirsiniz. Bu yapılandırmanın nasıl ayarlandığı hakkında daha fazla bilgi için [Apache Spark to Azure Cosmos DB Bağlayıcı Kurulumu'na](https://github.com/Azure/azure-cosmosdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup)bakın.  

Aşağıdaki kod snippet', çalışan `spark-shell` aralık sayımı gerçekleştirmek için gerçek zamanlı Twitter veri akışını inceleyen Azure Cosmos DB değişiklik akışına bağlanmak için yapılandırılmış bir akış işini çalıştırmak için nasıl yapılandırılacağını gösterir.

```
// Import Libraries
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.codehaus.jackson.map.ObjectMapper
import com.microsoft.azure.cosmosdb.spark.streaming._
import java.time._


// Configure connection to Azure Cosmos DB Change Feed
val sourceConfigMap = Map(
"Endpoint" -> "[COSMOSDB ENDPOINT]",
"Masterkey" -> "[MASTER KEY]",
"Database" -> "[DATABASE]",
"Collection" -> "[COLLECTION]",
"ConnectionMode" -> "Gateway",
"ChangeFeedCheckpointLocation" -> "checkpointlocation",
"changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Interval Count")

// Start reading change feed as a stream
var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()

// Start streaming query to console sink
val query = streamData.withColumn("countcol", streamData.col("id").substr(0, 0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
```

Tam kod örnekleri için [azure-cosmosdb-spark/lambda/samples](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda)(bkz:
* [Cosmos DB Change Feed.scala'dan Akış Sorgusu](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala)
* [Cosmos DB Change Feed.scala'dan Akış Etiketleri Sorgusu](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala)

Bunun çıktısı, `spark-shell` Azure Cosmos DB değişiklik akışından gelen Twitter verilerine karşı aralıklı bir sayı gerçekleştiren yapılandırılmış bir akış işini sürekli olarak çalıştıran bir konsoldur. Aşağıdaki resimde akış işinin çıktısı ve aralık sayar.

![Azure Cosmos DB değiştirme akışından Twitter verilerine karşı aralık sayısını gösteren akış çıktısı](./media/lambda-architecture/lambda-architecture-speed-layer-twitter-count.png) 

Azure Cosmos DB değişiklik akışı hakkında daha fazla bilgi için bkz:

* [Azure Cosmos DB'de değişiklik akışı desteğiyle çalışma](change-feed.md)
* [Azure CosmosDB Değişiklik Akışı İşlemcisi Kitaplığı Tanıtımı](https://azure.microsoft.com/blog/introducing-the-azure-cosmosdb-change-feed-processor-library/)
* [Akış İşleme Değişiklikleri: Azure CosmosDB değişiklik akışı + Apache Spark](https://azure.microsoft.com/blog/stream-processing-changes-azure-cosmosdb-change-feed-apache-spark/)

## <a name="batch-and-serving-layers"></a>Toplu iş ve servis katmanları
Yeni veriler Azure Cosmos DB'ye yüklendiğinden (değişiklik akışı hız katmanı için **kullanılıyorsa), ana veri kümesinin** (değişmez, yalnızca ekham veri kümesi) bulunduğu yerdir. Bu noktadan itibaren, aşağıdaki resimde gösterildiği **gibi, toplu işlem katmanından** **hizmet katmanına**ön işlem işlevlerini gerçekleştirmek için HDInsight 'ı (Apache Spark) kullanın:

![Lambda mimarisinin toplu katmanını ve servis katmanını vurgulayan diyagram](./media/lambda-architecture/lambda-architecture-batch-serve.png)

Bu katmanlarda önemli olan:

 1. Tüm **veriler** yalnızca Azure Cosmos DB'ye itilir (çoklu döküm sorunlarını önlemek için).
 2. **Toplu işlem katmanı,** Azure Cosmos DB'de depolanan bir ana veri kümesine (değişmez, yalnızca ek ham veri kümesi) sahiptir. HDI Spark'ı kullanarak, toplulaştırmalarınızı hesaplanmış toplu görünümlerinizde depolanacak şekilde önceden hesaplayabilirsiniz.
 3. **Hizmet katmanı,** ana veri kümesi ve hesaplanmış toplu görünüm için koleksiyonlara sahip bir Azure Cosmos veritabanıdır.
 4. **Hız katmanı** bu makalede daha sonra ele alınmıştır.
 5. Tüm sorgular, toplu iş görünümlerinden ve gerçek zamanlı görünümlerden elde edilen sonuçları birleştirerek veya bunları tek tek pingleyerek yanıtlanabilir.

### <a name="code-example-pre-computing-batch-views"></a>Kod örneği: Ön hesaplama toplu görünümleri
Apache Spark'tan Azure Cosmos DB'ye kadar **ana veri kümenize** karşı önceden hesaplanmış görünümlerin nasıl yürütüleceğinizi göstermek için [Lambda Architecture Rearchitected - Batch Layer](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) and [Lambda Architecture Rearchitected - Batch to Serving Layer](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb)dizüstü bilgisayarlardaki aşağıdaki kod parçacıklarını kullanın. Bu senaryoda, Azure Cosmos DB'de depolanan Twitter verilerini kullanın.

Aşağıdaki PySpark kodunu kullanarak Azure Cosmos DB içindeki Twitter verilerine yapılandırma bağlantısı oluşturarak başlayalım.

```
# Configuration to connect to Azure Cosmos DB
tweetsConfig = {
  "Endpoint" : "[Endpoint URL]",
  "Masterkey" : "[Master Key]",
  "Database" : "[Database]",
  "Collection" : "[Collection]", 
  "preferredRegions" : "[Preferred Regions]",
  "SamplingRatio" : "1.0",
  "schema_samplesize" : "200000",
  "query_custom" : "[Cosmos DB SQL Query]"
}

# Create DataFrame
tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()

# Create Temp View (to run Spark SQL statements)
tweets.createOrReplaceTempView("tweets")
```

Ardından, tweet kümesinin en iyi 10 hashtag'ini belirlemek için aşağıdaki Spark SQL deyimini çalıştıralım. Bu Spark SQL sorgusu için, bu kodu doğrudan bu kod snippet aşağıdaki çıkış çubuğu grafik olmadan bir Jupyter dizüstü bilgisayarda bu çalıştırıyoruz.

```
%%sql
select hashtags.text, count(distinct id) as tweets
from (
  select 
    explode(hashtags) as hashtags,
    id
  from tweets
) a
group by hashtags.text
order by tweets desc
limit 10
```

![Hashtag başına tweet sayısını gösteren grafik](./media/lambda-architecture/lambda-architecture-batch-hashtags-bar-chart.png)

Artık sorgunuz olduğuna göre, çıktı verilerini farklı bir koleksiyona kaydetmek için Spark Bağlayıcısı'nı kullanarak bunu bir koleksiyona geri kaydedelim.  Bu örnekte, bağlantıyı sergilemek için Scala'yı kullanın. Önceki örneğe benzer şekilde, Apache Spark DataFrame'i farklı bir Azure Cosmos kapsayıcısına kaydetmek için yapılandırma bağlantısı oluşturun.

```
val writeConfigMap = Map(
    "Endpoint" -> "[Endpoint URL]",
    "Masterkey" -> "[Master Key]",
    "Database" -> "[Database]",
    "Collection" -> "[New Collection]", 
    "preferredRegions" -> "[Preferred Regions]",
    "SamplingRatio" -> "1.0",
    "schema_samplesize" -> "200000"
)

// Configuration to write
val writeConfig = Config(writeConfigMap)

```

(Belge `SaveMode` `Overwrite` mi yoksa belirtin) `Append` belirttikten `tweets_bytags` sonra, önceki örnekteki Spark SQL sorgusuna benzer bir DataFrame oluşturun.  Oluşturulan `tweets_bytags` DataFrame ile, daha önce `write` belirtilen `writeConfig`yöntemi kullanarak kaydedebilirsiniz.

```
// Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
import org.apache.spark.sql.{Row, SaveMode, SparkSession}

// Create new DataFrame of tweets tags
val tweets_bytags = spark.sql("select hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc")

// Save to Cosmos DB (using Append in this case)
tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

Bu son bildirim şimdi Spark DataFrame'inizi yeni bir Azure Cosmos kapsayıcısına kaydetmiştir; lambda mimarisi açısından bakıldığında, bu hizmet **katmanı**içinde **toplu görünümüdür.**
 
#### <a name="resources"></a>Kaynaklar

Tam kod örnekleri için şunları içeren [azure-cosmosdb-spark/lambda/örneklerine](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda) bakın:
* Lambda Mimarlık Rearchitected - Toplu Katman [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb)
* Lambda Mimarlık Rearchitected - Hizmet Katmanı [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) Toplu

## <a name="speed-layer"></a>Hız katmanı
Daha önce de belirtildiği gibi, Azure Cosmos DB Değişiklik Akışı Kitaplığı'nı kullanmak, toplu iş ve hız katmanları arasındaki işlemleri basitleştirmenize olanak tanır. Bu mimaride, verilere karşı *yapılandırılmış akış* sorgularını gerçekleştirmek için Apache Spark'ı (HDInsight üzerinden) kullanın. Ayrıca, diğer sistemlerin bu verilere erişebilmeleri için yapılandırılmış akış sorgularınızın sonuçlarını geçici olarak sürdürmek isteyebilirsiniz.

![Lambda mimarisinin hız tabakasını vurgulayan diyagram](./media/lambda-architecture/lambda-architecture-speed.png)

Bunu yapmak için, yapılandırılmış akış sorgularınızın sonuçlarını kaydetmek için ayrı bir Azure Cosmos kapsayıcısı oluşturun.  Bu, diğer sistemlerin bu bilgilere erişmenizi sağlar, sadece Apache Spark değil. Cosmos DB Time-to-Live (TTL) özelliğinde olduğu gibi, belgelerinizi belirli bir süre sonra otomatik olarak silinecek şekilde yapılandırabilirsiniz.  Azure Cosmos DB TTL özelliği hakkında daha fazla bilgi için, [azure cosmos kaplarında otomatik olarak süresi dolan verilere bakın.](time-to-live.md)

```
// Import Libraries
import com.microsoft.azure.cosmosdb.spark._
import com.microsoft.azure.cosmosdb.spark.schema._
import com.microsoft.azure.cosmosdb.spark.config.Config
import org.codehaus.jackson.map.ObjectMapper
import com.microsoft.azure.cosmosdb.spark.streaming._
import java.time._


// Configure connection to Azure Cosmos DB Change Feed
val sourceCollectionName = "[SOURCE COLLECTION NAME]"
val sinkCollectionName = "[SINK COLLECTION NAME]"

val configMap = Map(
"Endpoint" -> "[COSMOSDB ENDPOINT]",
"Masterkey" -> "[COSMOSDB MASTER KEY]",
"Database" -> "[DATABASE NAME]",
"Collection" -> sourceCollectionName,
"ChangeFeedCheckpointLocation" -> "changefeedcheckpointlocation")

val sourceConfigMap = configMap.+(("changefeedqueryname", "Structured Stream replication streaming test"))

// Start to read the stream
var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
val sinkConfigMap = configMap.-("collection").+(("collection", sinkCollectionName))

// Start the stream writer to new collection
val streamingQueryWriter = streamData.writeStream.format(classOf[CosmosDBSinkProvider].getName).outputMode("append").options(sinkConfigMap).option("checkpointLocation", "streamingcheckpointlocation")
var streamingQuery = streamingQueryWriter.start()

```

## <a name="lambda-architecture-rearchitected"></a>Lambda mimarisi: Yeniden yapılandı
Önceki bölümlerde belirtildiği gibi, aşağıdaki bileşenleri kullanarak orijinal lambda mimarisini basitleştirebilirsiniz:
* Azure Cosmos DB
* Verilerinizi toplu iş ve hız katmanları arasında çoklu döküm gereksinimini önlemek için Azure Cosmos DB Değiştirme Akışı Kitaplığı
* HDInsight'ta Apache Spark
* Azure Cosmos DB için Kıvılcım Bağlayıcısı

![Azure Cosmos DB, Spark ve Azure Cosmos DB Değişiklik Akışı API'sini kullanarak lambda mimarisinin yeniden mimarisini gösteren diyagram](./media/lambda-architecture/lambda-architecture-re-architected.png)

Bu tasarımla, yalnızca iki yönetilen hizmete, Azure Cosmos DB ve HDInsight'a ihtiyacınız vardır. Birlikte, onlar toplu adres, hizmet, ve lambda mimarisinin hız katmanları. Bu, yalnızca işlemleri değil, aynı zamanda veri akışını da kolaylaştırır. 
 1. Tüm veriler işlenmek için Azure Cosmos DB'ye itilir
 2. Toplu işlem katmanında bir ana veri kümesi vardır (değişmez, yalnızca eklenen ham veri kümesi) ve toplu iş görünümleri önceden hesaplar
 3. Hizmet katmanı, hızlı sorgular için toplu veri görünümlerine sahiptir.
 4. Hız katmanı işlem süresini (servis katmanına) dengeler ve yalnızca son verilerle ilgilenir.
 5. Tüm sorgular, toplu iş görünümlerinden ve gerçek zamanlı görünümlerden elde edilen sonuçları birleştirerek yanıtlanabilir.

### <a name="resources"></a>Kaynaklar

* **Yeni veriler**: [Twitter'dan CosmosDB'ye akış akışı,](https://github.com/tknandu/TwitterCosmosDBFeed)azure cosmos DB'ye yeni verileri itme mekanizmasıdır.
* **Toplu katman:** Toplu işlem katmanı *ana veri kümesinden* (değişmez, yalnızca eklenen ham veri kümesi) ve **hizmet katmanına**itilen verilerin toplu görünümlerini önceden hesaplama yeteneğinden oluşur.
   * **Lambda Architecture Rearchitected - Batch Layer** dizüstü [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) toplu görünümlerin ana *veri kümesini* sorgular.
* **Hizmet katmanı:** **Servis katmanı,** hızlı sorgular için toplu görünümlerle (örneğin toplamalar, belirli dilimleyiciler, vb.) sonuçlanan önceden hesaplanmış verilerden oluşur.
  * **Lambda Architecture Rearchitected - Batch to Serving Layer** dizüstü bilgisayar [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) | [html](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) toplu verileri servis katmanına iter; diğer bir de, Spark tweet'lerden oluşan bir toplu koleksiyonu sorgular, işler ve başka bir koleksiyona (hesaplanmış toplu iş) saklar.
    * **Hız katmanı:** **Hız katmanı,** Azure Cosmos DB değişiklik akışını kullanarak hemen okumak ve harekete geçerek Spark'tan oluşur. Veriler, gerçek zamanlı sorguyu kendileri çalıştırmak yerine diğer sistemlerin işlenen gerçek zamanlı verileri sorgulayabilmesi *için, hesaplanan RT'ye* de kaydedilebilir.
  * [Cosmos DB Change Feed](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala) scala komut dosyasından Akış Sorgusu, Kıvılcım kabuğundan bir aralık sayımı hesaplamak için Azure Cosmos DB değişiklik akışından bir akış sorgusu yürütür.
  * Cosmos DB Change Feed scala komut [dosyasından Akış Etiketleri Sorgusu,](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala) Kıvılcım kabuğundan bir etiket aralığı sayısını hesaplamak için Azure Cosmos DB değişiklik akışından bir akış sorgusu yürütür.
  
## <a name="next-steps"></a>Sonraki adımlar
Henüz yapmadıysanız, [Azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub deposundan Azure Cosmos DB konektörüne Spark'ı indirin ve repo'daki ek kaynakları keşfedin:
* [Lambda mimarisi](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda)
* [Dağıtılmış toplama örnekleri](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)
* [Örnek komut dosyaları ve not defterleri](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)
* [Yapılandırılmış akış demoları](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)
* [Özet akışı demolarını değiştirme](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [Azure Cosmos DB Değişiklik Akışı ve Apache Spark kullanarak akış işlemleri değişiklikleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)

[Ayrıca Apache Spark SQL, DataFrames ve Datasets Kılavuzu](https://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure HDInsight makalesinde Apache Spark'ı](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) da gözden geçirmek isteyebilirsiniz.
