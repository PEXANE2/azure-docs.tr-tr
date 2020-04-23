---
title: 'Öğretici: Apache Spark Akış & Apache Kafka - Azure HDInsight'
description: Apache Spark akışını kullanarak Apache Kafka içine veya dışına veri almayı öğrenin. Bu öğreticide, HDInsight üzerinde Spark’tan bir Jupyter not defterini kullanarak verilerinizi akışla aktaracaksınız.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,seodec18
ms.date: 04/22/2020
ms.openlocfilehash: 5fa25f54faecbc7caf130ffeb0d24c3d8fef7e09
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084813"
---
# <a name="tutorial-use-apache-spark-structured-streaming-with-apache-kafka-on-hdinsight"></a>Öğretici: HDInsight üzerinde Apache Spark Yapılandırılmış Akışını Apache Kafka ile kullanma

Bu öğretici, Azure HDInsight'ta [Apache Kafka](./kafka/apache-kafka-introduction.md) ile veri okumak ve yazmak için [Apache Spark Structured Streaming'in](https://spark.apache.org/docs/latest/structured-streaming-programming-guide) nasıl kullanılacağını göstermektedir.

Spark Structured Streaming, Spark SQL üzerine kurulmuş bir akış işleme motorudur. Bu altyapıyı kullanarak, statik veriler üzerinde toplu hesaplamayla aynı şekilde akış hesaplamalarını ifade edebilirsiniz.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kümeler oluşturmak için Azure Kaynak Yöneticisi şablonu kullanma
> * Kafka ile Kıvılcım Yapılandırılmış Akış Kullanın

Bu belgedeki adımları bitirdiğinizde, fazla ücretleri önlemek için kümeleri silmeyi unutmayın.

## <a name="prerequisites"></a>Önkoşullar

* jq, bir komut satırı JSON işlemci.  Bkz. [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).

* HDInsight'ta Spark ile [Jupyter Notebook'ları](https://jupyter.org/) kullanma aşinalığı. Daha fazla bilgi için [Verileri Yükle'ye bakın ve HDInsight belgesinde Apache Spark ile sorguları çalıştırın.](spark/apache-spark-load-data-run-query.md)

* Scala programlama dilini bilme. Bu öğreticide kullanılan kod, Scala dilinde yazılmıştır.

* Kafka konuları oluşturmayı bilme. Daha fazla bilgi için [HDInsight hızlı başlatma belgesindeki Apache Kafka'ya](kafka/apache-kafka-get-started.md) bakın.

> [!IMPORTANT]  
> Bu belgede yer alan adımlar hem HDInsight üzerinde Spark hem de HDInsight kümesi üzerinde Kafka içeren bir Azure kaynak grubu gerektirir. Bu kümelerin her ikisi de Spark kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanıyan bir Azure Sanal Ağı içinde bulunur.
>
> Size kolaylık sağlamak için bu belgede, tüm gerekli Azure kaynaklarını oluşturabilecek bir şablonun bağlantıları sağlanır.
>
> Sanal ağda HDInsight kullanımı hakkında daha fazla bilgi için, [HDInsight](hdinsight-plan-virtual-network-deployment.md) belgesi için sanal ağ Planı'na bakın.

## <a name="structured-streaming-with-apache-kafka"></a>Apache Kafka ile Yapılandırılmış Akış

Spark Yapılandırılmış Akışı, Spark SQL altyapısı üzerinde derlenen bir akış işleme altyapısıdır. Yapılandırılmış Akış'ı kullanırken, toplu sorguları yazdığınız gibi akış sorguları da yazabilirsiniz.

Aşağıdaki kod parçacıkları, Kafka’dan okuma ve dosyaya depolama işlemlerini gösterir. Birincisi bir toplu iş işlemi, ikincisiyse bir akış işlemidir:

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

Her iki kod parçacığında Kafka’dan veriler okunur ve dosyaya yazılır. Örnekler arasındaki farklar şunlardır:

| Batch | Akış |
| --- | --- |
| `read` | `readStream` |
| `write` | `writeStream` |
| `save` | `start` |

Akış işlemi, `awaitTermination(30000)`30.000 ms'den sonra akışı durduran akışı da kullanır.

Kafka ile Yapılandırılmış Akışı kullanmak için projenizin `org.apache.spark : spark-sql-kafka-0-10_2.11` paketinde bir bağımlılığı olmalıdır. Bu paketin sürümü, HDInsight üzerinde Spark sürümüyle eşleşmelidir. Spark 2.2.0 için (HDInsight 3.6'da mevcuttur), farklı proje türlerine ait bağımlılık bilgilerini [https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar](https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar).

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

HDInsight'taki Apache Kafka, Kafka brokerlarına halka açık internet üzerinden erişim sağlamaz. Kafka kullanan her özellik aynı Azure sanal ağı içinde olmalıdır. Bu öğreticide hem Kafka hem de Spark kümeleri aynı Azure sanal ağı içinde yer alır.

Aşağıdaki diyagramda Spark ile Kafka arasındaki iletişimin nasıl aktığı gösterilmektedir:

![Bir Azure sanal ağında Spark ve Kafka kümeleri diyagramı](./media/hdinsight-apache-kafka-spark-structured-streaming/apache-spark-kafka-vnet.png)

> [!NOTE]  
> Kafka hizmeti, sanal ağ içindeki iletişimle sınırlıdır. SSH ve Ambari gibi küme üzerindeki diğer hizmetlere internet üzerinden erişilebilir. HDInsight üzerinde kullanılabilir olan genel bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight Tarafından Kullanılan Bağlantı Noktaları ve URI’ler](hdinsight-hadoop-port-settings-for-services.md).

Bir Azure Sanal Ağı oluşturmak ve sonra bunun içinde Kafka ve Spark kümeleri oluşturmak için aşağıdaki adımları kullanın:

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-kafka-spark-structured-streaming/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

    Azure Kaynak Yöneticisi şablonu **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json**' nda yer alır.

    Bu şablon aşağıdaki kaynakları oluşturur:

   * HDInsight 3.6 kümesi üzerinde bir Kafka.
   * HDInsight 3.6 kümesi üzerinde bir Spark 2.2.0.
   * HDInsight kümeleri içeren bir Azure Sanal Ağı.

     > [!IMPORTANT]  
     > Bu öğreticide kullanılan yapılandırılmış akış not defteri için HDInsight 3.6 üzerinde Spark 2.2.0 gerekir. HDInsight üzerinde Spark’ın daha önceki bir sürümünü kullanıyorsanız, not defterini kullanırken hatalarla karşılaşırsınız.

2. **Özelleştirilmiş şablon** bölümündeki girişleri doldurmak için aşağıdaki bilgileri kullanın:

    | Ayar | Değer |
    | --- | --- |
    | Abonelik | Azure aboneliğiniz |
    | Kaynak grubu | Kaynakları içeren kaynak grubu. |
    | Konum | İçinde kaynakların oluşturulduğu Azure bölgesi. |
    | Spark Kümesi Adı | Spark kümesinin adı. İlk altı karakter Kafka küme adından farklı olmalıdır. |
    | Kafka Kümesi Adı | Kafka kümesinin adı. İlk altı karakter Spark küme adından farklı olmalıdır. |
    | Küme Oturum Açma Kullanıcı Adı | Kümeler için yönetici kullanıcı adı. |
    | Küme Oturum Açma Parolası | Kümeler için yönetici kullanıcı parolası. |
    | SSH Kullanıcı Adı | Kümeler için oluşturulacak SSH kullanıcısı. |
    | SSH Parolası | SSH kullanıcısı için parola. |

    ![Özelleştirilmiş şablonun ekran görüntüsü](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

3. Şartlar **ve Koşullar**okuyun , sonra **ben yukarıda belirtilen hüküm ve koşulları kabul seçin.**

4. **Satın al**'ı seçin.

> [!NOTE]  
> Kümelerin oluşturulması 20 dakikaya kadar sürebilir.

## <a name="use-spark-structured-streaming"></a>Kıvılcım Yapılandırılmış Akış'ı Kullanma

Bu örnek, HDInsight'ta Kafka ile Spark Structured Streaming'in nasıl kullanılacağını göstermektedir. New York city tarafından sağlanan taksi gezileri, verileri kullanır.  Bu not defteri tarafından kullanılan veri seti [2016 Yeşil Taksi Gezi Verileri'nden](https://data.cityofnewyork.us/Transportation/2016-Green-Taxi-Trip-Data/hvrh-b6nb)dir.

1. Ana bilgisayar bilgilerini toplayın. Kafka ZooKeeper ve broker hosts bilgi almak için aşağıdaki curl ve [jq](https://stedolan.github.io/jq/) komutları kullanın. Komutlar bir Windows komut istemi için tasarlanmıştır, diğer ortamlar için küçük varyasyonlar gerekir. Kafka `KafkaCluster` kümenizin adı ve `KafkaPassword` küme giriş parolası ile değiştirin. Ayrıca, `C:\HDI\jq-win64.exe` jq yükleme için gerçek yol ile değiştirin. Windows komut istemindeki komutları girin ve çıktıyı sonraki adımlarda kullanmak üzere kaydedin.

    ```cmd
    REM Enter cluster name in lowercase

    set CLUSTERNAME=KafkaCluster
    set PASSWORD=KafkaPassword

    curl -u admin:%PASSWORD% -G "https://%CLUSTERNAME%.azurehdinsight.net/api/v1/clusters/%CLUSTERNAME%/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | C:\HDI\jq-win64.exe -r "["""\(.host_components[].HostRoles.host_name):2181"""] | join(""",""")"

    curl -u admin:%PASSWORD% -G "https://%CLUSTERNAME%.azurehdinsight.net/api/v1/clusters/%CLUSTERNAME%/services/KAFKA/components/KAFKA_BROKER" | C:\HDI\jq-win64.exe -r "["""\(.host_components[].HostRoles.host_name):9092"""] | join(""",""")"
    ```

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/jupyter`kümenizin adı nerede' `CLUSTERNAME` ye gidin. Sorulduğunda, kümeyi oluştururken kullanılan küme kullanıcı adı (yönetici) ve parolasını girin.

1. Not defteri oluşturmak için **Yeni > Kıvılcım'ı** seçin.

1. Kıvılcım akışı mikrobatching vardır, bu da veri toplu olarak gelir ve yürütücüler veri toplu çalışır. Uygulayıcının toplu iş işleme süresinden daha az boşta zaman acısı varsa, uygulayıcılar sürekli olarak eklenir ve kaldırılır. Uygulayıcılar boşta zaman dilimi toplu iş süresinden büyükse, uygulayıcı asla kaldırılmaz. Bu **nedenle, akış uygulamalarını çalıştırırken spark.dynamicAllocation.enabled'i yanlış olarak ayarlayarak dinamik ayırmayı devre dışı bırakmamanızı öneririz.**

    Not Defteri hücresine aşağıdaki bilgileri girerek Not Defteri tarafından kullanılan paketleri yükleyin. **CTRL + ENTER**kullanarak komutu çalıştırın.

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

1. Kafka konusunu oluşturun. İlk adımda çıkarılan Zookeeper ana bilgisayar bilgilerini değiştirerek `YOUR_ZOOKEEPER_HOSTS` aşağıdaki komutu düzenleyin. Konuyu oluşturmak için Jupyter Notebook'unuza düzenlenmiş komutu `tripdata` girin.

    ```scala
    %%bash
    export KafkaZookeepers="YOUR_ZOOKEEPER_HOSTS"

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic tripdata --zookeeper $KafkaZookeepers
    ```

1. Taksi yolculukları ile ilgili verileri alın. New York'taki taksi yolculuklarına veri yüklemek için bir sonraki hücredeki komutu girin. Veriler bir veri çerçevesine yüklenir ve ardından veri çerçevesi hücre çıktısı olarak görüntülenir.

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

1. Kafka aracısını ayarlayın bilgi barındırı. 1. adımda çıkardığınız broker ana bilgisayar bilgileriyle değiştirin. `YOUR_KAFKA_BROKER_HOSTS`  Düzenlenen komutu sonraki Jupyter Notebook hücresine girin.

    ```scala
    // The Kafka broker hosts and topic used to write to Kafka
    val kafkaBrokers="YOUR_KAFKA_BROKER_HOSTS"
    val kafkaTopic="tripdata"

    println("Finished setting Kafka broker and topic configuration.")
    ```

1. Verileri Kafka'ya gönder. Aşağıdaki komutta, `vendorid` alan Kafka iletisi için anahtar değer olarak kullanılır. Anahtar Kafka tarafından veri bölümleme kullanılır. Tüm alanlar Kafka iletisinde JSON dize değeri olarak depolanır. Bir toplu iş sorgusu kullanarak verileri Kafka'ya kaydetmek için Jupyter'da aşağıdaki komutu girin.

    ```scala
    // Select the vendorid as the key and save the JSON string as the value.
    val query = taxiDF.selectExpr("CAST(vendorid AS STRING) as key", "to_JSON(struct(*)) AS value").write.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("topic", kafkaTopic).save()

    println("Data sent to Kafka")
    ```

1. Şema bildir. Aşağıdaki komut, Kafka'dan JSON verilerini okurken şema nın nasıl kullanılacağını göstermektedir. Bir sonraki Jupyter hücrenizdeki komutu girin.

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

1. Verileri seçin ve akışı başlatın. Aşağıdaki komut, toplu sorgu kullanarak Kafka'dan nasıl veri alınır gösteriş gösterir. Ve sonra Sonuçları Spark kümesindeki HDFS'ye yazın. Bu örnekte, `select` Kafka'dan iletiyi (değer alanı) alır ve şemayı ona uygular. Veriler daha sonra PARKE FORMATInDA HDFS 'ye (WASB veya ADL) yazılır. Bir sonraki Jupyter hücrenizdeki komutu girin.

    ```scala
    // Read a batch from Kafka
    val kafkaDF = spark.read.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("subscribe", kafkaTopic).option("startingOffsets", "earliest").load()

    // Select data and write to file
    val query = kafkaDF.select(from_json(col("value").cast("string"), schema) as "trip").write.format("parquet").option("path","/example/batchtripdata").option("checkpointLocation", "/batchcheckpoint").save()

    println("Wrote data to file")
    ```

1. Dosyaların bir sonraki Jupyter hücrenizdeki komutu girerek oluşturulduğunu doğrulayabilirsiniz. `/example/batchtripdata` Dizindeki dosyaları listeler.

    ```scala
    %%bash
    hdfs dfs -ls /example/batchtripdata
    ```

1. Önceki örnekte toplu iş sorgusu kullanılırken, aşağıdaki komut akış sorgusu kullanarak aynı şeyin nasıl yapılacağını gösterir. Bir sonraki Jupyter hücrenizdeki komutu girin.

    ```scala
    // Stream from Kafka
    val kafkaStreamDF = spark.readStream.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("subscribe", kafkaTopic).option("startingOffsets", "earliest").load()

    // Select data from the stream and write to file
    kafkaStreamDF.select(from_json(col("value").cast("string"), schema) as "trip").writeStream.format("parquet").option("path","/example/streamingtripdata").option("checkpointLocation", "/streamcheckpoint").start.awaitTermination(30000)
    println("Wrote data to file")
    ```

1. Dosyaların akış sorgusu tarafından yazıldığını doğrulamak için aşağıdaki hücreyi çalıştırın.

    ```scala
    %%bash
    hdfs dfs -ls /example/streamingtripdata
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunu silmek, ilişkili HDInsight kümesini de siler. Ve kaynak grubuyla ilişkili diğer kaynaklar.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure [portalında,](https://portal.azure.com/)hizmetler menüsünü açmak için sol taraftaki menüyü genişletin ve ardından kaynak gruplarınızın listesini görüntülemek için __Kaynak Grupları'nı__ seçin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

> [!WARNING]  
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz.
>
> HDInsight üzerinde Kafka kümesinin silinmesi Kafka’da depolanmış tüm verileri siler.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Apache Spark Structured Streaming'in nasıl kullanılacağını öğrendiniz. HDInsight'ta Apache Kafka'dan veri yazmak ve okumak için. Kafka ile Apaçi Fırtınası'nın nasıl kullanılacağını öğrenmek için aşağıdaki bağlantıyı kullanın.

> [!div class="nextstepaction"]
> [Apaçi Kafka ile Apaçi Fırtınası kullanın](hdinsight-apache-storm-with-kafka.md)
