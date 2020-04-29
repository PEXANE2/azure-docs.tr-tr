---
title: Azure Cosmos DB ve Apache Spark lambda mimarisi
description: Bu makalede Azure Cosmos DB, HDInsight ve Spark kullanarak bir lambda mimarisinin nasıl uygulanacağı açıklanır.
ms.service: cosmos-db
author: tknandu
ms.author: ramkris
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 68ce06d8a2904bf99f58a53817444b2992b23501
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76719747"
---
# <a name="azure-cosmos-db-implement-a-lambda-architecture-on-the-azure-platform"></a>Azure Cosmos DB: Azure platformunda bir lambda mimarisi uygulama 

Lambda mimarileri, çok büyük veri kümelerinin verimli veri işlemesini sağlar. Lambda mimarileri, büyük verileri sorgulamak için kullanılan gecikme süresini en aza indirmek için toplu işleme, akış işleme ve bir hizmet katmanı kullanır. 

Azure 'da bir lambda mimarisi uygulamak için, gerçek zamanlı büyük veri analizlerini hızlandırmak üzere aşağıdaki teknolojileri birleştirebilirsiniz:
* Sektörün ilk küresel olarak dağıtılan, çok modelli veritabanı hizmeti [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). 
* Büyük ölçekli veri analizi uygulamalarını çalıştıran bir işleme çerçevesi olan [Azure HDInsight için Apache Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/)
* HDInsight 'ın işlemesi için toplu iş katmanına yeni verileri akıp Azure Cosmos DB [değişiklik akışı](change-feed.md)
* [Spark to Azure Cosmos DB Bağlayıcısı](spark-connector.md)

Bu makalede, ilk çok katmanlı tasarıma dayalı bir lambda mimarisinin temelleri ve işlemleri basitleştiren "yeniden tasarlanmış" bir lambda mimarisinin avantajları açıklanmaktadır.  

## <a name="what-is-a-lambda-architecture"></a>Lambda mimarisi nedir?
Lambda mimarisi, [Nathan Marz](https://twitter.com/nathanmarz)tarafından açıklandığı şekilde Batch ve hız gecikme senaryolarına yönelik genel, ölçeklenebilir ve hataya dayanıklı bir veri işleme mimarisidir.

![Lambda mimarisini gösteren diyagram](./media/lambda-architecture/lambda-architecture-intro.png)

Kaynak: http://lambda-architecture.net/

Bir lambda mimarisinin temel ilkeleri, Yukarıdaki diyagramda, temelinde [http://lambda-architecture.net](http://lambda-architecture.net/)açıklanmıştır.

 1. Tüm **veriler** , hem *Batch katmanına* hem *de* *hız katmanına*gönderilir.
 2. **Batch katmanının** ana veri kümesi (sabit, salt ekleme ham veri kümesi) vardır ve Batch görünümlerini önceden hesaplar.
 3. **Hizmet veren katmanda** hızlı sorgular için Batch görünümleri vardır. 
 4. **Hız katmanı** , işleme süresini (sunan katmana) dengeler ve yalnızca son verilerle ilgilenir.
 5. Tüm sorgular, Batch görünümlerinden ve gerçek zamanlı görünümlerde sonuçları birleştirerek veya tek tek ping işlemi yaparak cevaplanır.

Daha fazla okuma sonrasında, bu mimariyi yalnızca aşağıdakileri kullanarak uygulayacağız:

* Azure Cosmos kapsayıcıları
* HDInsight (Apache Spark 2,1) kümesi
* Spark Bağlayıcısı [1,0](https://search.maven.org/artifact/com.microsoft.azure/azure-cosmosdb-spark_2.1.0_2.11/1.2.6/jar)

## <a name="speed-layer"></a>Hız katmanı

Bir işlem perspektifinden, verilerin doğru durumunu güvence altına alırken iki veri akışının saklanması karmaşık bir Endeavor olabilir. İşlemleri basitleştirmek için, Azure Cosmos DB değişiklik günlüğünü, *hız katmanınız*Için *değişiklik akışı API 'si* aracılığıyla alırken *toplu iş katmanının* durumunu tutmak için [Azure Cosmos DB akış desteğini değiştirin](change-feed.md) .  
![Lambda mimarisinin yeni verileri, hız katmanını ve ana veri kümesi bölümünü vurgulayan diyagram](./media/lambda-architecture/lambda-architecture-change-feed.png)

Bu katmanlarda önemli olanlar:

 1. Tüm **veriler** *yalnızca* Azure Cosmos DB gönderilir, böylece çok sayıda atama sorunlarından kaçınabilirsiniz.
 2. **Batch katmanının** ana veri kümesi (sabit, salt ekleme ham veri kümesi) vardır ve Batch görünümlerini önceden hesaplar.
 3. **Hizmet düzeyi** , sonraki bölümde ele alınmıştır.
 4. **Hız katmanı** , Azure Cosmos DB değişiklik akışını okumak için hdınsight (Apache Spark) kullanır. Bu, verilerinizi kalıcı hale getirmenizi ve aynı anda sorgulamak ve işlemek için de sağlar.
 5. Tüm sorgular, Batch görünümlerinden ve gerçek zamanlı görünümlerde sonuçları birleştirerek veya tek tek ping işlemi yaparak cevaplanır.
 
### <a name="code-example-spark-structured-streaming-to-an-azure-cosmos-db-change-feed"></a>Kod örneği: Azure Cosmos DB değişiklik akışına Spark yapılandırılmış akışı
**Hız katmanının**bir parçası olarak Azure Cosmos DB değişiklik akışının hızlı bir örneğini çalıştırmak Için, akış işleme değişikliklerinin bir parçası olarak Twitter verilerini kullanarak, [Azure Cosmos DB değişiklik akışı ve Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Stream-Processing-Changes-using-Azure-Cosmos-DB-Change-Feed-and-Apache-Spark) örneği kullanarak test edebilir. Twitter çıktınızdan geçiş yapmak için [Twitter 'Dan akış akışı](https://github.com/tknandu/TwitterCosmosDBFeed)içindeki kod örneğine Cosmos DB bakın. Yukarıdaki örnekte, Twitter verilerini Azure Cosmos DB içine yüklüyorsunuz ve daha sonra HDInsight (Apache Spark) kümenizi, değişiklik akışına bağlanacak şekilde ayarlayabilirsiniz. Bu yapılandırmayı ayarlama hakkında daha fazla bilgi için, [Azure Cosmos DB bağlayıcı kurulumuna Apache Spark](https://github.com/Azure/azure-cosmosdb-spark/wiki/Spark-to-Cosmos-DB-Connector-Setup)bakın.  

Aşağıdaki kod parçacığı, çalışan bir Aralık sayımı `spark-shell` gerçekleştirmek üzere gerçek zamanlı Twitter veri akışını inceleyen Azure Cosmos DB değişiklik akışına bağlanmak üzere yapılandırılmış bir akış işi çalıştıracak şekilde nasıl yapılandırılacağını gösterir.

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

Bu, Azure Cosmos DB değişiklik akışından Twitter `spark-shell` verilerine göre Aralık sayısı gerçekleştiren bir yapılandırılmış akış işini sürekli çalıştıran bir konsoldur. Aşağıdaki görüntüde akış işinin çıktısı ve Aralık sayıları gösterilmektedir.

![Azure Cosmos DB değişiklik akışındaki Twitter verilerine göre Aralık sayısını gösteren akış çıkışı](./media/lambda-architecture/lambda-architecture-speed-layer-twitter-count.png) 

Azure Cosmos DB değişiklik akışı hakkında daha fazla bilgi için bkz.:

* [Azure Cosmos DB değişiklik akışı desteğiyle çalışma](change-feed.md)
* [Azure CosmosDB değişiklik akışı Işlemci kitaplığına giriş](https://azure.microsoft.com/blog/introducing-the-azure-cosmosdb-change-feed-processor-library/)
* [Akış Işleme değişiklikleri: Azure CosmosDB değişiklik akışı + Apache Spark](https://azure.microsoft.com/blog/stream-processing-changes-azure-cosmosdb-change-feed-apache-spark/)

## <a name="batch-and-serving-layers"></a>Toplu ve hizmet sunma katmanları
Yeni veriler Azure Cosmos DB yüklendi (hız katmanı için değişiklik akışı kullanıldığı), bu, **ana veri kümesinin** (sabit, yalnızca bir ham veri kümesi) bulunduğu yerdir. Bu noktadan itibaren, aşağıdaki görüntüde gösterildiği gibi, **toplu iş katmanından** **katmana hizmet**vermek üzere işlem öncesi işlevleri gerçekleştirmek için HDInsight (Apache Spark) kullanın:

![Batch katmanını vurgulama ve lambda mimarisinin katmanına sunma diyagramı](./media/lambda-architecture/lambda-architecture-batch-serve.png)

Bu katmanlarda önemli olanlar:

 1. Tüm **veriler** yalnızca Azure Cosmos DB gönderilir (çok noktaya yayın sorunlarından kaçınmak için).
 2. **Batch katmanının** , Azure Cosmos DB depolanan ana veri kümesi (değişmez, yalnızca satır içi ham veri kümesi) vardır. HDI Spark kullanarak, toplamaların hesaplanmış Batch görünümleriniz içinde saklanmasını sağlamak için önceden işlem yapabilirsiniz.
 3. **Hizmet sunma katmanı** , ana veri kümesi ve hesaplanan toplu iş görünümü için koleksiyonlara sahip bir Azure Cosmos veritabanıdır.
 4. **Hız katmanı** Bu makalenin ilerleyen kısımlarında ele alınmıştır.
 5. Tüm sorgular, Batch görünümleri ve gerçek zamanlı görünümlerin sonuçları birleştirilerek veya ayrı ayrı ping bir şekilde yanıtlanarak yanıtlanır.

### <a name="code-example-pre-computing-batch-views"></a>Kod örneği: ön bilgi işlem Batch görünümleri
Apache Spark **ana veri kümeniz** Azure Cosmos DB için önceden hesaplanmış görünümlerin nasıl yürütüleceğini göstermek için, Not defterleri lambda mimarisinden aşağıdaki kod parçacıklarını kullanın. bu Işlem, [katmana hizmet vermek için](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb), çalıştırılabilir [-Batch katmanı](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) ve lambda mimarisi yeniden tasarlanmıştır. Bu senaryoda, Azure Cosmos DB depolanan Twitter verilerini kullanın.

Aşağıdaki PySpark kodunu kullanarak Azure Cosmos DB içindeki Twitter verileriyle yapılandırma bağlantısı oluşturarak başlayalım.

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

Daha sonra, yer işaretleri kümesinin ilk 10 diyez etiketini öğrenmek için aşağıdaki Spark SQL ifadesini çalıştıralim. Bu Spark SQL sorgusu için, bu kod parçacığını doğrudan takip eden çıkış çubuğu grafiği olmadan bir Jupyter not defterinde çalıştırıyoruz.

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

![Diyez etiketi başına doldurulabilir sayısını gösteren grafik](./media/lambda-architecture/lambda-architecture-batch-hashtags-bar-chart.png)

Sorgunuz olduğuna göre, şimdi de, çıktı verilerini farklı bir koleksiyona kaydetmek için Spark bağlayıcısını kullanarak bir koleksiyona geri kaydedelim.  Bu örnekte, bağlantıyı göstermek için Scala ' yı kullanın. Önceki örneğe benzer şekilde, Apache Spark veri çerçevesini farklı bir Azure Cosmos kapsayıcısına kaydetmek için yapılandırma bağlantısını oluşturun.

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

`SaveMode` ( `Overwrite` Veya `Append` belgeleri belirterek) öğesini BELIRTTIKTEN sonra, önceki örnekteki Spark SQL `tweets_bytags` sorgusuna benzer bir dataframe oluşturun.  `tweets_bytags` Veri çerçevesi oluşturulduğunda, daha önce belirtilen `write` `writeConfig`öğesini kullanarak yöntemi kullanarak kaydedebilirsiniz.

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
* Lambda mimarisi yeniden tasarlanmıştır-katman [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) | [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) 'ye hizmet vermeye yönelik toplu işlem

## <a name="speed-layer"></a>Hız katmanı
Daha önce belirtildiği gibi, Azure Cosmos DB değişiklik akışı kitaplığını kullanmak, Batch ve Speed katmanları arasındaki işlemleri basitleştirmenizi sağlar. Bu mimaride, verilere karşı *yapılandırılmış akış* sorgularını gerçekleştirmek için Apache Spark (HDInsight aracılığıyla) kullanın. Ayrıca, yapılandırılmış akış sorgularınızın sonuçlarını geçici olarak kalıcı hale getirmek isteyebilirsiniz, böylece diğer sistemler bu verilere erişebilir.

![Lambda mimarisinin hız katmanını vurgulayan diyagram](./media/lambda-architecture/lambda-architecture-speed.png)

Bunu yapmak için, yapılandırılmış akış sorgularınızın sonuçlarını kaydetmek üzere ayrı bir Azure Cosmos kapsayıcısı oluşturun.  Bu bilgilere yalnızca Apache Spark değil, diğer sistemlerin erişmesini sağlar. Ayrıca, yaşam süresi (TTL) özelliği Cosmos DB, belgelerinizi bir ayarlama süresinden sonra otomatik olarak silinecek şekilde yapılandırabilirsiniz.  Azure Cosmos DB TTL özelliği hakkında daha fazla bilgi için bkz. [Azure Cosmos kapsayıcılarındaki verileri yaşam süresi ile otomatik olarak bitirme](time-to-live.md)

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

## <a name="lambda-architecture-rearchitected"></a>Lambda mimarisi: yeniden tasarlanmıştır
Önceki bölümlerde belirtildiği gibi, aşağıdaki bileşenleri kullanarak özgün lambda mimarisini basitleştirebilirsiniz:
* Azure Cosmos DB
* Toplu iş ve hız katmanları arasında verilerinize Çok noktaya yayın ihtiyacını önlemek için Azure Cosmos DB akış kitaplığını değiştirin
* HDInsight üzerinde Apache Spark
* Azure Cosmos DB için Spark Bağlayıcısı

![Azure Cosmos DB, Spark ve Azure Cosmos DB değişiklik akışı API 'sini kullanarak lambda mimarisinin yeniden mimarisini gösteren diyagram](./media/lambda-architecture/lambda-architecture-re-architected.png)

Bu tasarıma sahip olarak yalnızca iki adet yönetilen hizmet, Azure Cosmos DB ve HDInsight gerekir. Birlikte, lambda mimarisinin toplu işlem, hizmet sunma ve hız katmanlarını ele alırlar. Bu, yalnızca işlemleri değil, veri akışını da basitleştirir. 
 1. Tüm veriler işleme için Azure Cosmos DB gönderilir
 2. Batch katmanının ana veri kümesi (sabit, salt ekleme ham veri kümesi) vardır ve Batch görünümlerini önceden hesaplar
 3. Hizmet veren katmanda hızlı sorgulara yönelik veri toplu görünümleri bulunur.
 4. Hız katmanı, işleme süresini (sunan katmana) dengeler ve yalnızca son verilerle ilgilenir.
 5. Toplu iş görünümlerindeki ve gerçek zamanlı görünümlerdeki sonuçlar birleştirilerek tüm sorgular yanıtlanamaz.

### <a name="resources"></a>Kaynaklar

* **Yeni veriler**: [Twitter 'dan cosmosdb 'ye](https://github.com/tknandu/TwitterCosmosDBFeed), yeni verileri Azure Cosmos DB gönderme mekanizması olan akış akışı.
* **Batch katmanı:** Batch katmanı, *ana veri kümesinden* (değişmez, yalnızca bir ham veri kümesi) oluşur ve **hizmet katmanına**gönderilen verilerin toplu iş görünümlerini önceden hesaplamanıza olanak sağlar.
   * Mimari **mimarisi yeniden tasarlanmıştır-Batch katmanı** Not defteri [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.ipynb) | [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20Layer.html) , Batch görünümlerinin *ana veri kümesi* kümesini sorgular.
* **Katman sunma:** **Hizmet veren katman** , hızlı sorgularda toplu görünümlere (örneğin toplamalar, belirli Dilimleyiciler vb.) neden olan önceden hesaplanan verilerden oluşur.
  * Çalışan **lambda mimarisi, katman** Not defteri 'ne hizmet veren [ipynb](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.ipynb) | [HTML](https://github.com/Azure/azure-cosmosdb-spark/blob/master/samples/lambda/Lambda%20Architecture%20Re-architected%20-%20Batch%20to%20Serving%20Layer.html) , toplu verileri sunan katmana iter; diğer bir deyişle, Spark, toplu iş koleksiyonunu sorgular, işler ve onu başka bir koleksiyonda (hesaplanan bir Batch) depolar.
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
