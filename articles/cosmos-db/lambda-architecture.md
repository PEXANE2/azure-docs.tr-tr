---
title: Azure Cosmos DB ve Apache Spark lambda mimarisi
description: Bu makalede Azure Cosmos DB, HDInsight ve Spark'ı kullanarak bir lambda mimarisi uygulama
ms.service: cosmos-db
author: tknandu
ms.author: ramkris
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 68ce06d8a2904bf99f58a53817444b2992b23501
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719747"
---
# <a name="azure-cosmos-db-implement-a-lambda-architecture-on-the-azure-platform"></a>Azure Cosmos DB: Azure platformunda lambda mimarisi uygulama 

Lambda mimarisi etkin veri işleme, büyük hacimli veri kümeleri etkinleştirin. Lambda mimarisi, büyük verileri Sorgulama ilgili gecikme süresini en aza indirmek için toplu işlem, akış işleme ve Hizmet katmanını kullanın. 

Lambda mimarisi Azure'da uygulamak için gerçek zamanlı büyük veri analizi hızlandırmak için aşağıdaki teknolojileri birleştirebilirsiniz:
* Sektörün ilk küresel olarak dağıtılan, çok modelli veritabanı hizmeti [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 
* Büyük ölçekli veri analizi uygulamalarını çalıştıran bir işleme çerçevesi olan [Azure HDInsight için Apache Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/)
* HDInsight 'ın işlemesi için toplu iş katmanına yeni verileri akıp Azure Cosmos DB [değişiklik akışı](change-feed.md)
* [Spark to Azure Cosmos DB Bağlayıcısı](spark-connector.md)

Bu makalede, özgün çok katmanlı tasarımı ve işlemlerini kolaylaştıran bir "rearchitected" lambda mimarisinin avantajları temel alan bir lambda mimarisinin temelleri açıklanır.  

## <a name="what-is-a-lambda-architecture"></a>Lambda mimarisi nedir?
Lambda mimarisi, [Nathan Marz](https://twitter.com/nathanmarz)tarafından açıklandığı şekilde Batch ve hız gecikme senaryolarına yönelik genel, ölçeklenebilir ve hataya dayanıklı bir veri işleme mimarisidir.

![Lambda mimarisi gösteren diyagram](./media/lambda-architecture/lambda-architecture-intro.png)

Kaynak: http://lambda-architecture.net/

Bir lambda mimarisinin temel ilkeleri, Yukarıdaki diyagramda [http://lambda-architecture.net](http://lambda-architecture.net/)göre açıklanmıştır.

 1. Tüm **veriler** , hem *Batch katmanına* hem *de* *hız katmanına*gönderilir.
 2. **Batch katmanının** ana veri kümesi (sabit, salt ekleme ham veri kümesi) vardır ve Batch görünümlerini önceden hesaplar.
 3. **Hizmet veren katmanda** hızlı sorgular için Batch görünümleri vardır. 
 4. **Hız katmanı** , işleme süresini (sunan katmana) dengeler ve yalnızca son verilerle ilgilenir.
 5. Tüm sorguları birleştirme sonuçları toplu görünümler ve gerçek zamanlı bir görünüm veya ayrı ayrı ping yanıtlanması gereken.

Daha fazla okuma sırasında biz yalnızca şunları kullanarak bu mimariyi uygulamak şunları yapabilir:

* Azure Cosmos kapsayıcıları
* HDInsight (Apache Spark 2.1) kümesi
* Spark Bağlayıcısı [1,0](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.1.0_2.11/1.2.6/jar)

## <a name="speed-layer"></a>Hızı katmanı

İşlemler açısından bakıldığında, iki veri akışlarını veri doğru durumu sağlarken koruma karmaşık bir çaba olabilir. İşlemleri basitleştirmek için, Azure Cosmos DB değişiklik günlüğünü, *hız katmanınız*Için *değişiklik akışı API 'si* aracılığıyla alırken *toplu iş katmanının* durumunu tutmak için [Azure Cosmos DB akış desteğini değiştirin](change-feed.md) .  
Lambda mimarisinin yeni verileri, hız katmanını ve ana veri kümesi bölümünü vurgulayan diyagram ![](./media/lambda-architecture/lambda-architecture-change-feed.png)

Bu katmanda önemli nedir:

 1. Tüm **veriler** *yalnızca* Azure Cosmos DB gönderilir, böylece çok sayıda atama sorunlarından kaçınabilirsiniz.
 2. **Batch katmanının** ana veri kümesi (sabit, salt ekleme ham veri kümesi) vardır ve Batch görünümlerini önceden hesaplar.
 3. **Hizmet düzeyi** , sonraki bölümde ele alınmıştır.
 4. **Hız katmanı** , Azure Cosmos DB değişiklik akışını okumak için hdınsight (Apache Spark) kullanır. Bu, sorgulama ve eşzamanlı olarak işlemek için verilerinizin kalıcı hale getirmek sağlar.
 5. Tüm sorguları birleştirme sonuçları toplu görünümler ve gerçek zamanlı bir görünüm veya ayrı ayrı ping yanıtlanması gereken.
 
### <a name="code-example-spark-structured-streaming-to-an-azure-cosmos-db-change-feed"></a>Kod örneği: Spark yapılandırılmış bir Azure Cosmos DB değişiklik akışı akış
**Hız katmanının**bir parçası olarak Azure Cosmos DB değişiklik akışının hızlı bir örneğini çalıştırmak Için, akış işleme değişikliklerinin bir parçası olarak Twitter verilerini kullanarak, [Azure Cosmos DB değişiklik akışı ve Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark) örneği kullanarak test edebilir. Twitter çıktınızdan geçiş yapmak için [Twitter 'Dan akış akışı](https://github.com/tknandu/TwitterCosmosDBFeed)içindeki kod örneğine Cosmos DB bakın. Önceki örnekte ile Twitter verilerini Azure Cosmos DB'ye yüklüyorsunuz ve değişiklik akışı bağlanmak için (Apache Spark) HDInsight kümenizi daha sonra ayarlayabilirsiniz. Bu yapılandırmayı ayarlama hakkında daha fazla bilgi için, [Azure Cosmos DB bağlayıcı kurulumuna Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup)bakın.  

Aşağıdaki kod parçacığı, çalışan bir Aralık sayımı gerçekleştirmek üzere gerçek zamanlı Twitter veri akışını inceleyen Azure Cosmos DB değişiklik akışına bağlanmak üzere `spark-shell` nasıl yapılandırılacağını gösterir.

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

Tam kod örnekleri için bkz. [Azure-cosmosdb-Spark/lambda/örnekler](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda), örneğin:
* [Cosmos DB değişiklik akışından akış sorgusu. Scala](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala)
* [Cosmos DB değişiklik akışı. Scala 'dan akış etiketleri sorgusu](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala)

Bu, Azure Cosmos DB değişiklik akışından Twitter verilerine karşı bir Aralık sayısı gerçekleştiren yapısal bir akış işini sürekli çalıştıran bir `spark-shell` konsoludur. Aşağıdaki görüntüde akışı tanımlı işlemin çıktısını gösterir ve aralığı sayar.

![Azure Cosmos DB değişiklik akışı Twitter verilerini karşı aralık sayısı gösteren akış çıkışı](./media/lambda-architecture/lambda-architecture-speed-layer-twitter-count.png) 

Değişiklik akışını Azure Cosmos DB hakkında daha fazla bilgi için bkz:

* [Azure Cosmos DB değişiklik akışı desteğiyle çalışma](change-feed.md)
* [Azure CosmosDB değişiklik akışı Işlemci kitaplığına giriş](https://azure.microsoft.com/blog/introducing-the-azure-cosmosdb-change-feed-processor-library/)
* [Akış Işleme değişiklikleri: Azure CosmosDB değişiklik akışı + Apache Spark](https://azure.microsoft.com/blog/stream-processing-changes-azure-cosmosdb-change-feed-apache-spark/)

## <a name="batch-and-serving-layers"></a>Batch ve hizmet katmanları
Yeni veriler Azure Cosmos DB yüklendi (hız katmanı için değişiklik akışı kullanıldığı), bu, **ana veri kümesinin** (sabit, yalnızca bir ham veri kümesi) bulunduğu yerdir. Bu noktadan itibaren, aşağıdaki görüntüde gösterildiği gibi, **toplu iş katmanından** **katmana hizmet**vermek üzere işlem öncesi işlevleri gerçekleştirmek için HDInsight (Apache Spark) kullanın:

![Toplu iş katmanı ve Hizmet katmanını lambda mimarisinin vurgulama diyagramı](./media/lambda-architecture/lambda-architecture-batch-serve.png)

Bu katmanda önemli nedir:

 1. Tüm **veriler** yalnızca Azure Cosmos DB gönderilir (çok noktaya yayın sorunlarından kaçınmak için).
 2. **Batch katmanının** , Azure Cosmos DB depolanan ana veri kümesi (değişmez, yalnızca satır içi ham veri kümesi) vardır. HDI Spark'ı kullanarak hesaplanan batch görünümlerinizde depolanacak işlemlerinizi önceden hesaplayabilirsiniz.
 3. **Hizmet sunma katmanı** , ana veri kümesi ve hesaplanan toplu iş görünümü için koleksiyonlara sahip bir Azure Cosmos veritabanıdır.
 4. **Hız katmanı** Bu makalenin ilerleyen kısımlarında ele alınmıştır.
 5. Tüm sorguların sonuçlarını toplu görünümler ve gerçek zamanlı görünümleri birleştirme veya ayrı ayrı ping yanıtlanması.

### <a name="code-example-pre-computing-batch-views"></a>Kod örneği: batch görünümleri önceden bilgi işlem
Apache Spark **ana veri kümeniz** Azure Cosmos DB için önceden hesaplanmış görünümlerin nasıl yürütüleceğini göstermek için, Not defterleri lambda mimarisinden aşağıdaki kod parçacıklarını kullanın. bu Işlem, [katmana hizmet vermek için](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb), çalıştırılabilir [-Batch katmanı](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) ve lambda mimarisi yeniden tasarlanmıştır. Bu senaryoda, Azure Cosmos DB'de depolanan Twitter verilerini kullanın.

Twitter verilerini aşağıdaki PySpark kodu kullanarak Azure Cosmos DB içinde yapılandırma bağlantısı oluşturarak başlayalım.

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

Yanında, ilk 10 diyez etiketlerini tweetleri kümesini belirlemek için aşağıdaki Spark SQL deyimi çalıştıralım. Bu bir Spark SQL sorgusu için Biz bu bir Jupyter not defteri doğrudan bu kod parçacığı aşağıdaki çıkış çubuk grafik çalıştırıyorsunuz.

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

![Tweet diyez etiketi başına sayısını gösteren grafik](./media/lambda-architecture/lambda-architecture-batch-hashtags-bar-chart.png)

Şimdi sorgunuzu olduğuna göre çıktı verilerini farklı bir koleksiyona kaydetmek için Spark Bağlayıcısı'nı kullanarak bir koleksiyon için geri kaydedin.  Bu örnekte, Scala bağlantıyı göstermek için kullanın. Önceki örneğe benzer şekilde, Apache Spark veri çerçevesini farklı bir Azure Cosmos kapsayıcısına kaydetmek için yapılandırma bağlantısını oluşturun.

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

`SaveMode` (`Overwrite` veya `Append` belgeler) belirttikten sonra, önceki örnekteki Spark SQL sorgusuna benzer bir `tweets_bytags` DataFrame oluşturun.  `tweets_bytags` veri çerçevesi oluşturulduktan sonra, daha önce belirtilen `writeConfig`kullanarak `write` yöntemi kullanarak kaydedebilirsiniz.

```
// Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
import org.apache.spark.sql.{Row, SaveMode, SparkSession}

// Create new DataFrame of tweets tags
val tweets_bytags = spark.sql("select hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc")

// Save to Cosmos DB (using Append in this case)
tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
```

Bu son bildiri artık Spark DataFrame 'i yeni bir Azure Cosmos kapsayıcısına kaydettiniz. bir lambda mimarisi perspektifinden, bu, **hizmet veren katmandaki** **toplu iş görünümüdür** .
 
#### <a name="resources"></a>Kaynaklar

Tam kod örnekleri için bkz. [Azure-cosmosdb-Spark/lambda/Samples](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda) şunlar:
* Lambda mimarisi yeniden tasarlanmıştır-Batch katmanı [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb)
* Lambda mimarisi yeniden tasarlanmıştır-katman [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) 'si | [Ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) 'ye sunma toplu işi

## <a name="speed-layer"></a>Hızı katmanı
Daha önce not ettiğiniz, kullanarak Azure Cosmos DB değişiklik akışı kitaplığı, batch ve hız Katmanlar arasındaki işlemleri basitleştirmek sağlar. Bu mimaride, verilere karşı *yapılandırılmış akış* sorgularını gerçekleştirmek için Apache Spark (HDInsight aracılığıyla) kullanın. Bu verileri diğer sistemlere erişebilmesi için geçici olarak yapılandırılmış akış sorgularınızın sonuçlarını kalıcı hale getirmek isteyebilirsiniz.

![Lambda mimarisinin hız katmanına vurgulama diyagramı](./media/lambda-architecture/lambda-architecture-speed.png)

Bunu yapmak için, yapılandırılmış akış sorgularınızın sonuçlarını kaydetmek üzere ayrı bir Azure Cosmos kapsayıcısı oluşturun.  Bu, diğer sistemler erişim sağlamak için bu bilgileri sağlar yalnızca Apache Spark. De Cosmos DB için-yaşam süresi (TTL) özelliği ile kullanarak, belgeleriniz otomatik olarak ayarlanmış bir süre sonra silinecek şekilde yapılandırabilirsiniz.  Azure Cosmos DB TTL özelliği hakkında daha fazla bilgi için bkz. [Azure Cosmos kapsayıcılarındaki verileri yaşam süresi ile otomatik olarak bitirme](time-to-live.md)

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

## <a name="lambda-architecture-rearchitected"></a>Lambda mimarisi: Rearchitected
Önceki bölümlerde belirtildiği gibi aşağıdaki bileşenleri'ni kullanarak özgün lambda mimarisinin basitleştirebilirsiniz:
* Azure Cosmos DB
* Çok noktaya yayın için gereken toplu ve hız Katmanlar arasındaki verilerinizi önlemek için Azure Cosmos DB değişiklik akışı kitaplığı
* HDInsight üzerinde Apache Spark
* Azure Cosmos DB için Spark Bağlayıcısı

![Azure Cosmos DB, Spark ve Azure Cosmos DB değişiklik akışı API'si kullanarak lambda mimarisinin rearchitecture gösteren diyagram](./media/lambda-architecture/lambda-architecture-re-architected.png)

Bu tasarımla, yalnızca iki yönetilen hizmetler, Azure Cosmos DB ile HDInsight gerekir. Bunlar birlikte, batch, sunma ve lambda mimarisinin hız katmanı adres. Bu, yalnızca işlem aynı zamanda veri akışı kolaylaştırır. 
 1. Tüm veri işleme için Azure Cosmos DB'ye gönderildi
 2. Toplu iş katmanı ana veri kümesinde (sabit, salt ham veri kümesi) ve toplu görünümler önceden hesaplar
 3. Hizmet katmanını, veri hızlı sorgular için toplu görünümler sahiptir.
 4. Hızı katman, işleme süresi (için Hizmet katmanını) dengeler ve yalnızca yeni verilerle ilgilidir.
 5. Tüm sorguların sonuçlarını toplu görünümler ve gerçek zamanlı görünümleri birleştirerek yanıtlanması gereken.

### <a name="resources"></a>Kaynaklar

* **Yeni veriler**: [Twitter 'dan cosmosdb 'ye](https://github.com/tknandu/TwitterCosmosDBFeed), yeni verileri Azure Cosmos DB gönderme mekanizması olan akış akışı.
* **Batch katmanı:** Batch katmanı, *ana veri kümesinden* (değişmez, yalnızca bir ham veri kümesi) oluşur ve **hizmet katmanına**gönderilen verilerin toplu iş görünümlerini önceden hesaplamanıza olanak sağlar.
   * Yinelenen **lambda mimarisi** , toplu iş görünümü [IP](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) 'leri | [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) , Batch görünümlerinin *ana veri* kümesini sorgular.
* **Katman sunma:** **Hizmet veren katman** , hızlı sorgularda toplu görünümlere (örneğin toplamalar, belirli Dilimleyiciler vb.) neden olan önceden hesaplanan verilerden oluşur.
  * Çalışan lambda mimarisi, [ıp](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) | katman Not defteri 'Ne **hizmet vermeye yönelik toplu işlem** , [](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) toplu verileri sunan katmana iter; diğer bir deyişle, Spark, toplu iş koleksiyonunu sorgular, işler ve onu başka bir koleksiyonda (hesaplanan bir Batch) depolar.
    * **Hız katmanı:** **Hız katmanı** , okuma ve hemen işlem yapması için Azure Cosmos DB değişiklik akışını kullanan Spark 'dan oluşur. Veriler ayrıca, diğer sistemlerin gerçek zamanlı bir sorgunun kendisini çalıştırmak yerine işlenen gerçek zamanlı verileri sorgulayabilmesi için *hesaplanan RT* 'ye kaydedilebilir.
  * [Cosmos DB değişiklik akışı Scala betiği Içindeki akış sorgusu](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Query%20from%20Cosmos%20DB%20Change%20Feed.scala) , Spark-Shell öğesinden bir Aralık sayısını hesaplamak için Azure Cosmos DB değişiklik akışından bir akış sorgusu yürütür.
  * [Cosmos DB değişiklik akışı Scala betiği Içindeki akış etiketleri sorgusu](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Streaming%20Tags%20Query%20from%20Cosmos%20DB%20Change%20Feed%20.scala) , Spark-Shell ' e ait etiketlerin Aralık sayısını hesaplamak için Azure Cosmos DB değişiklik akışından bir akış sorgusu yürütür.
  
## <a name="next-steps"></a>Sonraki adımlar
Henüz yapmadıysanız, Spark 'ı [Azure-cosmosdb-Spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub deposundan Azure Cosmos DB bağlayıcıya indirin ve depodaki ek kaynakları araştırın:
* [Lambda mimarisi](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples/lambda)
* [Dağıtılmış toplamalar örnekleri](https://github.com/Azure/azure-documentdb-spark/wiki/Aggregations-Examples)
* [Örnek betikler ve Not defterleri](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)
* [Yapılandırılmış akış gösterileri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Structured-Stream-demos)
* [Akış tanıtımları değiştirme](https://github.com/Azure/azure-cosmosdb-spark/wiki/Change-Feed-demos)
* [Azure Cosmos DB değişiklik akışını ve Apache Spark kullanarak akış işleme değişiklikleri](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark)

Ayrıca, [SQL, DataFrames ve veri kümeleri kılavuzunu](https://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure hdınsight 'ta Apache Spark](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) Apache Spark gözden geçirmek isteyebilirsiniz.
