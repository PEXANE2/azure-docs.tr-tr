---
title: Azure HDInsight'ta Kıvılcım Akışı
description: HDInsight Spark kümelerinde Apache Spark Streaming uygulamaları nasıl kullanılır?
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/20/2019
ms.openlocfilehash: 521d72642a27995d096402a4ca0e4af632b0788c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74406292"
---
# <a name="overview-of-apache-spark-streaming"></a>Apache Spark Streaming'e Genel Bakış

[Apaçi Kıvılcım](https://spark.apache.org/) Akış, bir düğüm hatası oluşsa bile, herhangi bir giriş olayının tam olarak bir kez işlenir garantisi ile HDInsight Spark kümelerinde veri akışı işleme sağlar. Spark Akışı, Azure Etkinlik Hub'ları, Azure IoT Hub'ı, [Apache Kafka, Apache](https://kafka.apache.org/) [Flume,](https://flume.apache.org/)Twitter, [ZeroMQ,](http://zeromq.org/)ham TCP soketleri veya [Apache Hadoop IPN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) dosya sistemlerini izleme gibi çok çeşitli kaynaklardan giriş verileri alan uzun soluklu bir iştir. Yalnızca olay odaklı bir işlemin aksine, Bir Kıvılcım Akışı 2 saniyelik dilim gibi zaman pencerelerine veri girişi ve ardından harita, azaltma, birleştirme ve ayıklama işlemlerini kullanarak her veri toplu işlemini dönüştürür. Spark Akışı daha sonra dönüştürülmüş verileri dosya sistemlerine, veritabanlarına, panolara ve konsola yazar.

![HDInsight ve Spark Streaming ile Akış İşleme](./media/apache-spark-streaming-overview/hdinsight-spark-streaming.png)

Kıvılcım Akış uygulamaları, bu toplu işlemi için göndermeden önce her *bir mikro dizi* olayı toplamak için saniyenin bir kısmını beklemelidir. Buna karşılık, olay odaklı bir uygulama her olayı hemen işler. Kıvılcım Akışı gecikmesi genellikle birkaç saniyenin altındadır. Mikro-toplu yaklaşımın yararları daha verimli veri işleme ve basit toplam hesaplamalar vardır.

## <a name="introducing-the-dstream"></a>DStream tanıtımı

Spark Streaming, DStream adı verilen *ayrık* bir akış kullanarak sürekli gelen veri akışını temsil eder. DStream, Olay Hub'ları veya Kafka gibi giriş kaynaklarından veya dönüşümleri başka bir DStream'e uygulayarak oluşturulabilir.

DStream, ham olay verilerinin üzerine bir soyutlama katmanı sağlar.

Tek bir olay ile başlayın, bağlı bir termostat bir sıcaklık okuma söylüyorlar. Bu olay Spark Streaming uygulamanıza ulaştığında, olay güvenilir bir şekilde depolanır ve burada birden çok düğümde çoğaltılır. Bu hata toleransı, tek bir düğümün arızalanmasının etkinliğinizin kaybına neden olmamasını sağlar. Spark çekirdeği, her düğümün genellikle en iyi performans için bellekte kendi verilerini koruduğu kümedeki birden çok düğüme veri dağıtan bir veri yapısı kullanır. Bu veri *yapısına esnek dağıtılmış veri kümesi* (RDD) adı verilir.

Her *RDD, toplu iş aralığı*adı verilen kullanıcı tanımlı bir zaman dilimi boyunca toplanan olayları temsil eder. Her toplu işlem aralığı sona ererken, bu aralıktaki tüm verileri içeren yeni bir RDD üretilir. Sürekli RDD kümesi bir DStream'de toplanır. Örneğin, toplu iş aralığı bir saniye uzunluğundaysa, DStream'iniz her saniye, o saniye boyunca alınan tüm verileri içeren bir RDD içeren bir toplu iş partisi yayır. DStream işlenirken, sıcaklık olayı bu toplu işlerden birinde görünür. Bir Kıvılcım Akış uygulaması, olayları içeren toplu işlemleri işler ve sonuçta her RDD'de depolanan veriler üzerinde hareket eder.

![Sıcaklık Olayları ile Örnek DStream](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Kıvılcım Akış uygulamasının yapısı

Kıvılcım Akış uygulaması, yutulan kaynaklardan veri alan, verileri işlemek için dönüşümler uygulayan ve verileri bir veya daha fazla hedefe iten uzun soluklu bir uygulamadır. Kıvılcım Akış uygulamasının yapısı statik bir parçaya ve dinamik bir parçaya sahiptir. Statik kısım, verilerin nereden geldiğini, veriler üzerinde hangi işlemenin yapılacağı ve sonuçların nereye gitmesi gerektiğini tanımlar. Dinamik kısım uygulamayı süresiz olarak çalıştırıyor ve durdurma sinyalini bekliyor.

Örneğin, aşağıdaki basit uygulama bir TCP soketi üzerinde bir metin satırı alır ve her sözcüğün kaç kez göründüğünü sayar.

### <a name="define-the-application"></a>Uygulamayı tanımlama

Uygulama mantığı tanımının dört adımı vardır:

1. Akış Bağlamı oluşturun.
2. Akış Bağlamından bir DStream oluşturun.
3. DStream dönüşümleri uygulayın.
4. Sonuçları çıktın.

Bu tanım statiktir ve siz uygulamayı çalıştırana kadar hiçbir veri işlenmez.

#### <a name="create-a-streamingcontext"></a>Akış Bağlamı Oluşturma

Kümenizi işaret eden SparkContext'tan bir Akış Bağlamı oluşturun. Akışbağlamoluştururken, toplu iş akışının boyutunu saniyeler içinde belirtirsiniz, örneğin:  

```
import org.apache.spark._
import org.apache.spark.streaming._

val ssc = new StreamingContext(sc, Seconds(1))
```

#### <a name="create-a-dstream"></a>DStream oluşturma

StreamingContext örneğinde, giriş kaynağınız için bir giriş DStream'i oluşturun. Bu durumda, uygulama HDInsight kümesine bağlı varsayılan depolama yeni dosyaların görünümünü izliyor.

```
val lines = ssc.textFileStream("/uploads/Test/")
```

#### <a name="apply-transformations"></a>Dönüşümleri uygulama

DStream üzerinde dönüşümler uygulayarak işleme uygularsınız. Bu uygulama, dosyadan aynı anda bir metin satırı alır, her satırı sözcüklere böler ve sonra her sözcüğün kaç kez göründüğünü saymak için bir harita azaltma deseni kullanır.

```
val words = lines.flatMap(_.split(" "))
val pairs = words.map(word => (word, 1))
val wordCounts = pairs.reduceByKey(_ + _)
```

#### <a name="output-results"></a>Çıktı sonuçları

Çıktı işlemleri uygulayarak dönüşüm sonuçlarını hedef sistemlere itin. Bu durumda, hesaplama yoluyla her çalıştırmanın sonucu konsol çıkışında yazdırılır.

```
wordCounts.print()
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Akış uygulamasını başlatın ve sonlandırma sinyali alınana kadar çalıştırın.

```
ssc.start()
ssc.awaitTermination()
```

Spark Stream API'si hakkındaki ayrıntılar ve desteklediği etkinlik kaynakları, dönüşümler ve çıktı işlemleri için [Bkz.](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)

Aşağıdaki örnek uygulama bağımsızdır, böylece [bir Jupyter Notebook](apache-spark-jupyter-notebook-kernels.md)içinde çalıştırabilirsiniz. Bu örnek, DummySource sınıfında sayaç değerini ve geçerli zamanı her beş saniyede milisaniyede çıkan sahte bir veri kaynağı oluşturur. Yeni bir Akış Bağlam nesnesinin toplu işlem aralığı 30 saniyedir. Akış uygulaması, toplu iş her oluşturulduğunda, üretilen RDD'yi inceler, RDD'yi Bir Spark DataFrame'e dönüştürür ve DataFrame üzerinde geçici bir tablo oluşturur.

```
class DummySource extends org.apache.spark.streaming.receiver.Receiver[(Int, Long)](org.apache.spark.storage.StorageLevel.MEMORY_AND_DISK_2) {

    /** Start the thread that simulates receiving data */
    def onStart() {
        new Thread("Dummy Source") { override def run() { receive() } }.start()
    }

    def onStop() {  }

    /** Periodically generate a random number from 0 to 9, and the timestamp */
    private def receive() {
        var counter = 0  
        while(!isStopped()) {
            store(Iterator((counter, System.currentTimeMillis)))
            counter += 1
            Thread.sleep(5000)
        }
    }
}

// A batch is created every 30 seconds
val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

// Set the active SQLContext so that we can access it statically within the foreachRDD
org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

// Create the stream
val stream = ssc.receiverStream(new DummySource())

// Process RDDs in the batch
stream.foreachRDD { rdd =>

    // Access the SQLContext and create a table called demo_numbers we can query
    val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
    _sqlContext.createDataFrame(rdd).toDF("value", "time")
        .registerTempTable("demo_numbers")
}

// Start the stream processing
ssc.start()
```

Yukarıdaki uygulamayı başladıktan sonra yaklaşık 30 saniye bekleyin.  Daha sonra, örneğin bu SQL sorgusunu kullanarak toplu iş teki geçerli değer kümesini görmek için DataFrame'i düzenli aralıklarla sorgulayabilirsiniz:

```sql
%%sql
SELECT * FROM demo_numbers
```

Elde edilen çıktı aşağıdaki gibi görünür:

| value | time |
| --- | --- |
|10 | 1497314465256 |
|11 | 1497314470272 |
|12 | 1497314475289 |
|13 | 1497314480310 |
|14 | 1497314485327 |
|15 | 1497314490346 |

DummySource her 5 saniyede bir değer oluşturduğundan ve uygulama her 30 saniyede bir toplu iş yayıdığından, altı değer vardır.

## <a name="sliding-windows"></a>Sürgülü pencereler

Bir süre boyunca DStream'inizde toplu hesaplamalar yapmak için (örneğin, son iki saniye içinde ortalama sıcaklık elde etmek için, Kıvılcım Akışı ile birlikte gelen *kayan pencere işlemlerini* kullanabilirsiniz. Kayan pencerenin bir süresi (pencere uzunluğu) ve pencereiçeriğinin değerlendirildiği aralık (slayt aralığı) vardır.

Sürgülü pencereler çakışabilir, örneğin, her saniye kayan iki saniye uzunluğunda bir pencere tanımlayabilirsiniz. Bu, bir toplama hesaplaması her yaptığınızda, pencerenin önceki pencerenin son bir saniyesinden gelen verileri ve sonraki bir saniyedeki yeni verileri içereceği anlamına gelir.

![Örnek Sıcaklık Olayları ile İlk Pencere](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Kayar Sonra Sıcaklık Olayları ile Örnek Pencere](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

Aşağıdaki örnek, toplu iş bir dakika süresi ve bir dakikalık slayt ile bir pencereye toplu toplamak için, DummySource kullanan kodu güncelleştirir.

```
class DummySource extends org.apache.spark.streaming.receiver.Receiver[(Int, Long)](org.apache.spark.storage.StorageLevel.MEMORY_AND_DISK_2) {

    /** Start the thread that simulates receiving data */
    def onStart() {
        new Thread("Dummy Source") { override def run() { receive() } }.start()
    }

    def onStop() {  }

    /** Periodically generate a random number from 0 to 9, and the timestamp */
    private def receive() {
        var counter = 0  
        while(!isStopped()) {
            store(Iterator((counter, System.currentTimeMillis)))
            counter += 1
            Thread.sleep(5000)
        }
    }
}

// A batch is created every 30 seconds
val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

// Set the active SQLContext so that we can access it statically within the foreachRDD
org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

// Create the stream
val stream = ssc.receiverStream(new DummySource())

// Process batches in 1 minute windows
stream.window(org.apache.spark.streaming.Minutes(1)).foreachRDD { rdd =>

    // Access the SQLContext and create a table called demo_numbers we can query
    val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
    _sqlContext.createDataFrame(rdd).toDF("value", "time")
    .registerTempTable("demo_numbers")
}

// Start the stream processing
ssc.start()
```

İlk dakikadan sonra, pencerede toplanan iki toplu iş her altı girişleri - 12 girişleri vardır.

| value | time |
| --- | --- |
| 1 | 1497316294139 |
| 2 | 1497316299158
| 3 | 1497316304178
| 4 | 1497316309204
| 5 | 1497316314224
| 6 | 1497316319243
| 7 | 1497316324260
| 8 | 1497316329278
| 9 | 1497316334293
| 10 | 1497316339314
| 11 | 1497316344339
| 12 | 1497316349361

Spark Streaming API'sinde bulunan kayar pencere işlevleri pencere, countByWindow, reduceByWindow ve countByValueAndWindow'u içerir. Bu işlevler hakkında ayrıntılı bilgi için [DStreams'deki Dönüşümler'e](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams)bakın.

## <a name="checkpointing"></a>Denetim noktası oluşturma

Spark Streaming, esneklik ve hata toleransı sağlamak için, düğüm hataları karşısında bile akış işleminin kesintisiz olarak devam etmesini sağlamak için kontrol noktasına dayanır. HDInsight'ta Spark, dayanıklı depolama (Azure Depolama veya Veri Gölü Depolama) için denetim noktaları oluşturur. Bu denetim noktaları yapılandırma, uygulama tarafından tanımlanan işlemler ve sıraya alınan ancak henüz işlenmemiş toplu işlemler gibi akış uygulamasıyla ilgili meta verileri depolar. Bazı durumlarda, denetim noktaları, Spark tarafından yönetilen RDD'lerde bulunan verilerin durumunu daha hızlı bir şekilde yeniden oluşturmak için RDD'lerde verileri kaydetmeyi de içerir.

## <a name="deploying-spark-streaming-applications"></a>Kıvılcım Akış uygulamalarını dağıtma

Genellikle bir JAR dosyasına yerel olarak bir Kıvılcım Akış uygulaması oluşturur sunuz ve ardından JAR dosyasını HDInsight kümenize bağlı varsayılan depolama alanına kopyalayarak HDInsight'ta Spark'a dağıtirsınız. Bir POST işlemi kullanarak kümenizden edinebileceğiniz LIVY REST API'leri ile başvurunuzu başlatabilirsiniz. POST'un gövdesi, JAR'ınıza giden yolu, ana yöntemi akış uygulamasını tanımlayan ve çalıştıran sınıfın adını ve isteğe bağlı olarak işin kaynak gereksinimlerini (uygulayıcıların, bellek ve çekirdek sayısı gibi) sağlayan bir JSON belgesi içerir. ve uygulama kodunuzu gerektiren yapılandırma ayarları.

![Bir Kıvılcım Akış uygulaması dağıtma](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Tüm uygulamaların durumu, LIVY bitiş noktasına karşı get isteğiyle de kontrol edilebilir. Son olarak, LIVY bitiş noktasına karşı bir DELETE isteği vererek çalışan bir uygulamayı sonlandırabilirsiniz. LIVY API hakkında ayrıntılı bilgi için, [Apache LIVY ile Uzak işleri](apache-spark-livy-rest-interface.md) görün

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Bir Apache Spark kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Apache Spark Akış Programlama Kılavuzu](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Apache LIVY ile Apache Spark işlerini uzaktan başlatın](apache-spark-livy-rest-interface.md)
