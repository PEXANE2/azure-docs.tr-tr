---
title: Eğitim Apache Spark Apache Kafka ile yapılandırılmış akış-Azure HDInsight
description: Apache Spark akışı kullanarak Apache Kafka verileri alma veya dışarı aktarma hakkında bilgi edinin. Bu öğreticide, HDInsight üzerinde Spark 'tan bir Jupyter Not defteri kullanarak veri akışı yapabilirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: tutorial
ms.date: 10/08/2019
ms.openlocfilehash: db2174451f01ef38dc69e4e14561175203e075c3
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72264270"
---
# <a name="tutorial-use-apache-spark-structured-streaming-with-apache-kafka-on-hdinsight"></a>Öğretici: HDInsight üzerinde Apache Kafka ile yapılandırılmış Apache Spark akışı kullanma

Bu öğreticide, Azure HDInsight üzerinde [Apache Kafka](https://kafka.apache.org/) ile verileri okumak ve yazmak Için [Apache Spark yapılandırılmış akışın](https://spark.apache.org/docs/latest/structured-streaming-programming-guide) nasıl kullanılacağı gösterilmektedir.

Spark yapılandırılmış akış, Spark SQL üzerinde oluşturulmuş bir akış işleme motorudur. Statik verilerde toplu hesaplama ile aynı akış hesaplamaları ifade etmenize olanak tanır.  

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Küme oluşturmak için bir Azure Resource Manager şablonu kullanma
> * Spark yapısal akışını Kafka ile kullanma

Bu belgedeki adımlarla işiniz bittiğinde, fazla ücretlerden kaçınmak için kümeleri silmeyi unutmayın.

## <a name="prerequisites"></a>Prerequisites

* bir komut satırı JSON işlemcisi olan JQ.  Bkz. [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).

* HDInsight 'ta Spark ile [Jupyıter not defterlerini](https://jupyter.org/) kullanma hakkında benzerlik. Daha fazla bilgi için bkz. [HDInsight 'ta verileri yükleme ve sorguları çalıştırma Apache Spark](spark/apache-spark-load-data-run-query.md) .

* [Scala](https://www.scala-lang.org/) programlama diliyle benzerlik. Bu öğreticide kullanılan kod Scala 'da yazılmıştır.

* Kafka konu başlıkları oluşturma hakkında benzerlik. Daha fazla bilgi için bkz. [HDInsight hızlı başlangıç belgesi Apache Kafka](kafka/apache-kafka-get-started.md) .

> [!IMPORTANT]  
> Bu belgedeki adımlarda hem HDInsight üzerinde Spark hem de HDInsight kümesinde Kafka içeren bir Azure Kaynak grubu gerekir. Bu kümeler her ikisi de bir Azure sanal ağı içinde bulunur. Bu, Spark kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanır.
>
> Kolaylık olması için, bu belge tüm gerekli Azure kaynaklarını oluşturabileceğiniz bir şablona bağlantı sağlar.
>
> Bir sanal ağda HDInsight kullanma hakkında daha fazla bilgi için bkz. [HDInsight için sanal ağ planı](hdinsight-plan-virtual-network-deployment.md) belgesi.

## <a name="structured-streaming-with-apache-kafka"></a>Apache Kafka ile yapılandırılmış akış

Spark yapılandırılmış akış, Spark SQL altyapısında oluşturulmuş bir akış işleme motorudur. Yapılandırılmış akışı kullanırken, akış sorgularını toplu sorguları yazarken aynı şekilde yazabilirsiniz.

Aşağıdaki kod parçacıkları Kafka öğesinden okumayı ve dosyaya depolamayı gösterir. Birincisi bir toplu işlemdir, ikincisi ise bir akış işlemidir:

```scala
// Read a batch from Kafka
val kafkaDF = spark.read.format("kafka")
                .option("kafka.bootstrap.servers", kafkaBrokers)
                .option("subscribe", kafkaTopic)
                .option("startingOffsets", "earliest")
                .load()

// Select data and write to file
kafkaDF.select(from_json(col("value").cast("string"), schema) as "trip")
                .write
                .format("parquet")
                .option("path","/example/batchtripdata")
                .option("checkpointLocation", "/batchcheckpoint")
                .save()
```

```scala
// Stream from Kafka
val kafkaStreamDF = spark.readStream.format("kafka")
                .option("kafka.bootstrap.servers", kafkaBrokers)
                .option("subscribe", kafkaTopic)
                .option("startingOffsets", "earliest")
                .load()

// Select data from the stream and write to file
kafkaStreamDF.select(from_json(col("value").cast("string"), schema) as "trip")
                .writeStream
                .format("parquet")
                .option("path","/example/streamingtripdata")
                .option("checkpointLocation", "/streamcheckpoint")
                .start.awaitTermination(30000)
```

Her iki kod parçacıklarında de veriler Kafka öğesinden okunmakta ve dosyaya yazılır. Örnekler arasındaki farklılıklar şunlardır:

| İşlemini | Akış |
| --- | --- |
| `read` | `readStream` |
| `write` | `writeStream` |
| `save` | `start` |

Akış işlemi ayrıca, 30.000 MS 'den sonra akışı durduran `awaitTermination(30000)` kullanır.

Kafka ile yapılandırılmış akışı kullanmak için projenizin `org.apache.spark : spark-sql-kafka-0-10_2.11` paketine bağımlılığı olmalıdır. Bu paketin sürümü HDInsight 'ta Spark sürümüyle eşleşmelidir. Spark 2.2.0 için (HDInsight 3,6 ' de mevcuttur) [https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar](https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar)' de farklı proje türleri için bağımlılık bilgilerini bulabilirsiniz.

Bu öğreticide kullanılan Jupyter Notebook için aşağıdaki hücre bu paket bağımlılığını yükler:

```
%%configure -f
{
    "conf": {
        "spark.jars.packages": "org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0",
        "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.11"
    }
}
```

## <a name="create-the-clusters"></a>Kümeleri oluşturma

HDInsight üzerinde Apache Kafka, genel İnternet üzerinden Kafka aracıları için erişim sağlamaz. Kafka kullanan her şey aynı Azure sanal ağında olmalıdır. Bu öğreticide, hem Kafka hem de Spark kümeleri aynı Azure sanal ağında bulunur.

Aşağıdaki diyagramda Spark ve Kafka arasında iletişimin nasıl akagösterdiği gösterilmektedir:

![Bir Azure sanal ağında Spark ve Kafka kümelerinin diyagramı](./media/hdinsight-apache-kafka-spark-structured-streaming/apache-spark-kafka-vnet.png)

> [!NOTE]  
> Kafka hizmeti, sanal ağ içindeki iletişim ile sınırlıdır. Kümedeki SSH ve ambarı gibi diğer hizmetlere internet üzerinden erişilebilir. HDInsight ile kullanılabilen ortak bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight tarafından kullanılan bağlantı noktaları ve URI 'ler](hdinsight-hadoop-port-settings-for-services.md).

Bir Azure sanal ağı oluşturmak ve sonra bunun içinde Kafka ve Spark kümelerini oluşturmak için aşağıdaki adımları kullanın:

1. Azure 'da oturum açmak ve Azure portal şablonu açmak için aşağıdaki düğmeyi kullanın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-kafka-spark-structured-streaming/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

    Azure Resource Manager şablonu **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json** ' de bulunur.

    Bu şablon aşağıdaki kaynakları oluşturur:

   * HDInsight 3,6 kümesinde bir Kafka.
   * HDInsight 3,6 kümesinde Spark 2.2.0.
   * HDInsight kümelerini içeren bir Azure sanal ağı.

     > [!IMPORTANT]  
     > Bu öğreticide kullanılan yapılandırılmış akış Not defteri, HDInsight 3,6 üzerinde Spark 2.2.0 gerektirir. HDInsight 'ta Spark 'ın daha önceki bir sürümünü kullanıyorsanız, Not defterini kullanırken hata alırsınız.

2. **Özelleştirilmiş şablon** bölümündeki girişleri doldurmak için aşağıdaki bilgileri kullanın:

    | Ayar | Değer |
    | --- | --- |
    | Aboneliğiniz | Azure aboneliğiniz |
    | Kaynak grubu | Kaynakları içeren kaynak grubu. |
    | Konum | Kaynakların oluşturulduğu Azure bölgesi. |
    | Spark kümesi adı | Spark kümesinin adı. İlk altı karakter Kafka küme adından farklı olmalıdır. |
    | Kafka küme adı | Kafka kümesinin adı. İlk altı karakter Spark kümesi adından farklı olmalıdır. |
    | Küme oturum açma Kullanıcı adı | Kümeler için Yönetici Kullanıcı adı. |
    | Küme oturum açma parolası | Kümeler için Yönetici Kullanıcı parolası. |
    | SSH Kullanıcı adı | Kümeler için oluşturulacak SSH kullanıcısı. |
    | SSH parolası | SSH kullanıcısının parolası. |

    ![Özelleştirilmiş şablonun ekran görüntüsü](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

3. **Hüküm ve koşulları**okuyun ve ardından **yukarıda belirtilen hüküm ve koşulları kabul ediyorum**' u seçin.

4. **Satın al**' ı seçin.

> [!NOTE]  
> Kümelerin oluşturulması 20 dakikaya kadar sürebilir.

## <a name="use-spark-structured-streaming"></a>Spark yapılandırılmış akışını kullanma

Bu örnek, HDInsight üzerinde Kafka ile Spark yapısal akışının nasıl kullanılacağını gösterir. Bu, New York City tarafından sunulan TAXI seyahatlerinde verileri kullanır.  Bu not defteri tarafından kullanılan veri kümesi [2016 yeşil TAXI veri yolculuğuna](https://data.cityofnewyork.us/Transportation/2016-Green-Taxi-Trip-Data/hvrh-b6nb)ait.

1. Konak bilgilerini toplayın. Kafka ZooKeeper ve Broker Konakları bilgilerini almak için aşağıdaki kıvrımlı ve [JQ](https://stedolan.github.io/jq/) komutlarını kullanın. Komutlar bir Windows komut istemi için tasarlanan diğer ortamlar için hafif Çeşitlemeler gerekecektir. @No__t-0 ' ı Kafka Kümenizin adı ile değiştirin ve küme oturum açma parolasıyla-1 @no__t. Ayrıca, `C:\HDI\jq-win64.exe` değerini JQ yüklemenizin gerçek yoluyla değiştirin. Komutları bir Windows komut istemine girin ve sonraki adımlarda kullanmak için çıktıyı kaydedin.

    ```cmd
    set CLUSTERNAME=KafkaCluster
    set PASSWORD=KafkaPassword
    
    curl -u admin:%PASSWORD% -G "https://%CLUSTERNAME%.azurehdinsight.net/api/v1/clusters/%CLUSTERNAME%/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | C:\HDI\jq-win64.exe -r "["""\(.host_components[].HostRoles.host_name):2181"""] | join(""",""")"
    
    curl -u admin:%PASSWORD% -G "https://%CLUSTERNAME%.azurehdinsight.net/api/v1/clusters/%CLUSTERNAME%/services/KAFKA/components/KAFKA_BROKER" | C:\HDI\jq-win64.exe -r "["""\(.host_components[].HostRoles.host_name):9092"""] | join(""",""")"
    ```

2. Web tarayıcınızda, Spark kümenizdeki Jupyter not defterine bağlanın. Aşağıdaki URL 'de, `CLUSTERNAME` ' ı __Spark__ kümenizin adıyla değiştirin:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    İstendiğinde, kümeyi oluştururken kullanılan küme oturum açma (yönetici) ve parolayı girin.

3. **Yeni > Spark** ' ı seçerek bir not defteri oluşturun.

4. Spark akışında mikro işleme vardır. Bu, verilerin toplu işler ve yürüticiler veri yığınlarıyla birlikte çalıştırılmasıdır. Yürütücü, toplu işi işlemek için gereken süreden daha az boşta kalırsa, yürütmeleri sürekli olarak eklenir ve kaldırılır. Yürütme boşta kalma zaman aşımı, toplu iş süresinden fazlaysa, yürütücü hiçbir zaman kaldırılmaz. Bu nedenle **, akış uygulamalarını çalıştırırken spark. dynamicAllocation. Enabled ayarını false olarak ayarlayarak dinamik ayırmayı devre dışı bırakmanızı öneririz.**

    Aşağıdaki bilgileri bir not defteri hücresine girerek Not defteri tarafından kullanılan paketleri yükleyin. **CTRL + ENTER**kullanarak komutu çalıştırın.

    ```configuration
    %%configure -f
    {
        "conf": {
            "spark.jars.packages": "org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0",
            "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.11",
            "spark.dynamicAllocation.enabled": false
        }
    }
    ```

5. Kafka konusunu oluşturun. @No__t-0 ' i ilk adımda ayıklanan Zookeeper ana bilgisayar bilgileriyle değiştirerek aşağıdaki komutu düzenleyin. @No__t-0 konusunu oluşturmak için Jupyter Notebook düzenlenen komutu girin.

    ```scala
    %%bash
    export KafkaZookeepers="YOUR_ZOOKEEPER_HOSTS"

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic tripdata --zookeeper $KafkaZookeepers
    ```

6. Vergi seyahatlerinde veri alın. Yeni York şehrinde vergi seyahatleri üzerine veri yüklemek için sonraki hücreye komutu girin. Veriler bir veri çerçevesine yüklenir ve ardından dataframe hücre çıktısı olarak görüntülenir.

    ```scala
    import spark.implicits._

    // Load the data from the New York City Taxi data REST API for 2016 Green Taxi Trip Data
    val url="https://data.cityofnewyork.us/resource/pqfs-mqru.json"
    val result = scala.io.Source.fromURL(url).mkString

    // Create a dataframe from the JSON data
    val taxiDF = spark.read.json(Seq(result).toDS)

    // Display the dataframe containing trip data
    taxiDF.show()
    ```

7. Kafka Broker ana bilgisayar bilgilerini ayarlayın. @No__t-0 ' yı, 1. Adımda ayıkladığınız aracı ana bilgisayar bilgileri ile değiştirin.  Sonraki Jupyter Notebook hücresinde düzenlenen komutunu girin.

    ```scala
    // The Kafka broker hosts and topic used to write to Kafka
    val kafkaBrokers="YOUR_KAFKA_BROKER_HOSTS"
    val kafkaTopic="tripdata"

    println("Finished setting Kafka broker and topic configuration.")
    ```

8. Verileri Kafka 'ye gönderin. Aşağıdaki komutta, `vendorid` alanı Kafka iletisi için anahtar değer olarak kullanılır. Anahtar, verileri Bölümlemede Kafka tarafından kullanılır. Tüm alanlar, Kafka iletisinde JSON dizesi değeri olarak depolanır. Bir Batch sorgusu kullanarak verileri Kafka 'e kaydetmek için jupi 'de aşağıdaki komutu girin.

    ```scala
    // Select the vendorid as the key and save the JSON string as the value.
    val query = taxiDF.selectExpr("CAST(vendorid AS STRING) as key", "to_JSON(struct(*)) AS value").write.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("topic", kafkaTopic).save()

    println("Data sent to Kafka")
    ```

9. Bir şema bildirin. Aşağıdaki komut, Kafka öğesinden JSON verileri okurken bir şemanın nasıl kullanılacağını göstermektedir. Sonraki Jupyıter hücresindeki komutu girin.

    ```scala
    // Import bits useed for declaring schemas and working with JSON data
    import org.apache.spark.sql._
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._

    // Define a schema for the data
    val schema = (new StructType).add("dropoff_latitude", StringType).add("dropoff_longitude", StringType).add("extra", StringType).add("fare_amount", StringType).add("improvement_surcharge", StringType).add("lpep_dropoff_datetime", StringType).add("lpep_pickup_datetime", StringType).add("mta_tax", StringType).add("passenger_count", StringType).add("payment_type", StringType).add("pickup_latitude", StringType).add("pickup_longitude", StringType).add("ratecodeid", StringType).add("store_and_fwd_flag", StringType).add("tip_amount", StringType).add("tolls_amount", StringType).add("total_amount", StringType).add("trip_distance", StringType).add("trip_type", StringType).add("vendorid", StringType)
    // Reproduced here for readability
    //val schema = (new StructType)
    //   .add("dropoff_latitude", StringType)
    //   .add("dropoff_longitude", StringType)
    //   .add("extra", StringType)
    //   .add("fare_amount", StringType)
    //   .add("improvement_surcharge", StringType)
    //   .add("lpep_dropoff_datetime", StringType)
    //   .add("lpep_pickup_datetime", StringType)
    //   .add("mta_tax", StringType)
    //   .add("passenger_count", StringType)
    //   .add("payment_type", StringType)
    //   .add("pickup_latitude", StringType)
    //   .add("pickup_longitude", StringType)
    //   .add("ratecodeid", StringType)
    //   .add("store_and_fwd_flag", StringType)
    //   .add("tip_amount", StringType)
    //   .add("tolls_amount", StringType)
    //   .add("total_amount", StringType)
    //   .add("trip_distance", StringType)
    //   .add("trip_type", StringType)
    //   .add("vendorid", StringType)

    println("Schema declared")
    ```

10. Verileri seçin ve akışı başlatın. Aşağıdaki komut, bir Batch sorgusu kullanılarak Kafka 'ten nasıl veri alınacağını ve sonra Spark kümesinde bir işlem için sonuçları ne şekilde yazılacağını gösterir. Bu örnekte, `select`, iletiyi (değer alanı) Kafka adresinden alır ve şemayı buna uygular. Daha sonra bu veriler, Parquet biçiminde bir. Sonraki Jupyıter hücresindeki komutu girin.

    ```scala
    // Read a batch from Kafka
    val kafkaDF = spark.read.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("subscribe", kafkaTopic).option("startingOffsets", "earliest").load()

    // Select data and write to file
    val query = kafkaDF.select(from_json(col("value").cast("string"), schema) as "trip").write.format("parquet").option("path","/example/batchtripdata").option("checkpointLocation", "/batchcheckpoint").save()

    println("Wrote data to file")
    ```

11. Sonraki jupi hücresine komutu girerek dosyaların oluşturulduğunu doğrulayabilirsiniz. @No__t-0 dizinindeki dosyaları listeler.

    ```scala
    %%bash
    hdfs dfs -ls /example/batchtripdata
    ```

12. Önceki örnekte bir toplu iş sorgusu kullanılırken aşağıdaki komut, akış sorgusu kullanarak aynı şeyi nasıl yapabileceğinizi gösterir. Sonraki Jupyıter hücresindeki komutu girin.

    ```scala
    // Stream from Kafka
    val kafkaStreamDF = spark.readStream.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("subscribe", kafkaTopic).option("startingOffsets", "earliest").load()

    // Select data from the stream and write to file
    kafkaStreamDF.select(from_json(col("value").cast("string"), schema) as "trip").writeStream.format("parquet").option("path","/example/streamingtripdata").option("checkpointLocation", "/streamcheckpoint").start.awaitTermination(30000)
    println("Wrote data to file")
    ```

13. Dosyaların akış sorgusu tarafından yazıldığını doğrulamak için aşağıdaki hücreyi çalıştırın.

    ```scala
    %%bash
    hdfs dfs -ls /example/streamingtripdata
    ```

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Bu öğretici tarafından oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portal kullanarak kaynak grubunu kaldırmak için:

1. Azure portal, sol taraftaki menüyü genişleterek hizmet menüsünü açın ve kaynak __grupları__ ' nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve ardından listenin sağ tarafındaki __daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__' i seçip onaylayın.

> [!WARNING]  
> HDInsight kümesi faturalandırma, küme oluşturulduğunda başlar ve küme silindiğinde duraklar. Faturalandırma, dakika başına dağıtılır, bu nedenle artık kullanımda olmadığında kümenizi silmeniz gerekir.
>
> HDInsight kümesinde Kafka silme, Kafka ' de depolanan tüm verileri siler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, HDInsight üzerinde [Apache Kafka](https://kafka.apache.org/) verileri yazmak ve okumak Için [Apache Spark yapılandırılmış akışın](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) nasıl kullanılacağını öğrendiniz. Kafka ile [Apache Storm](https://storm.apache.org/) nasıl kullanacağınızı öğrenmek için aşağıdaki bağlantıyı kullanın.

> [!div class="nextstepaction"]
> [Apache Kafka ile Apache Storm kullanma](hdinsight-apache-storm-with-kafka.md)
