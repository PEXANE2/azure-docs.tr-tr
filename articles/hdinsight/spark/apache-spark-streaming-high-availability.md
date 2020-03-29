---
title: İplik'te yüksek kullanılabilir Kıvılcım Akış işleri - Azure HDInsight
description: Azure HDInsight'ta yüksek kullanılabilirlik senaryosu için Apache Spark Streaming nasıl ayarlanır?
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: ac51b77e1ffc2b476b0a73dac9b6917552a86ce4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74807162"
---
# <a name="create-high-availability-apache-spark-streaming-jobs-with-yarn"></a>YARN ile yüksek kullanılabilirlikte Apache Spark Akış işleri oluşturun

[Apaçi Kıvılcım](https://spark.apache.org/) Akış, veri akışları işleme için ölçeklenebilir, yüksek iş letme, hataya dayanıklı uygulamalar uygulamanızı sağlar. HDInsight Spark kümesindeki Spark Streaming uygulamalarını Azure Event Hub'ları, Azure IoT Hub'ı, [Apache Kafka, Apache](https://kafka.apache.org/) [Flume,](https://flume.apache.org/)Twitter, [ZeroMQ,](http://zeromq.org/)ham TCP soketleri gibi farklı veri kaynaklarına bağlayabilir veya değişiklikler için [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html) dosya sistemini izleyebilirsiniz. Kıvılcım Akışı, herhangi bir olayın düğüm hatasında bile tam olarak bir kez işlendiğinin garantisi ile hata toleransını destekler.

Kıvılcım Akışı, verilere dönüşümler uygulayabileceğiniz ve ardından sonuçları dosya sistemlerine, veritabanlarına, panolara ve konsola itebildiğiniz uzun soluklu işler oluşturur. Kıvılcım Akışı, ilk olarak belirli bir zaman aralığında bir dizi olay toplayarak mikro veri yığınlarını işler. Daha sonra, bu toplu işlem ve çıktı için gönderilir. Toplu işlem zaman aralıkları genellikle saniyenin kesirleri olarak tanımlanır.

![Kıvılcım Akışı](./media/apache-spark-streaming-high-availability/apache-spark-streaming.png)

## <a name="dstreams"></a>DStreams

Spark Akışı, *ayrık* bir akış (DStream) kullanarak sürekli bir veri akışını temsil eder. Bu DStream, Olay Hub'ları veya Kafka gibi giriş kaynaklarından veya dönüşümleri başka bir DStream'e uygulayarak oluşturulabilir. Bir olay Spark Streaming uygulamanıza ulaştığında, olay güvenilir bir şekilde depolanır. Diğer bir süre, olay verileri çoğaltılır, böylece birden çok düğüm bunun bir kopyasına sahip olur. Bu, tek bir düğümün arızalanmasının etkinliğinizin kaybına neden olmamasını sağlar.

Spark çekirdeği *esnek dağıtılmış veri kümeleri* (RDD' ler) kullanır. RDD'ler verileri kümedeki birden çok düğüme dağıtır ve her düğüm genellikle en iyi performans için verilerini tamamen bellekte tutar. Her RDD, toplu iş aralığı nda toplanan olayları temsil eder. Toplu iş aralığı sona erdiğinde, Spark Streaming bu aralıktaki tüm verileri içeren yeni bir RDD üretir. Bu sürekli RDD kümesi bir DStream'de toplanır. Bir Kıvılcım Akış uygulaması, her bir toplu iş partisinin RDD'sinde depolanan verileri işler.

![Kıvılcım DStream](./media/apache-spark-streaming-high-availability/apache-spark-dstream.png)

## <a name="spark-structured-streaming-jobs"></a>Yapılandırılmış Akış işleri kıvılcım

Spark Structured Streaming, Spark 2.0'da akış yapılandırılmış verilerde kullanılmak üzere bir analitik motor olarak tanıtıldı. Spark Structured Streaming, SparkSQL toplu iş motoru API'lerini kullanır. Spark Streaming'de olduğu gibi, Spark Structured Streaming de hesaplamalarını sürekli olarak gelen mikro yığınlar üzerinden çalıştırır. Spark Structured Streaming, sınırsız satıriçeren bir Giriş Tablosu olarak veri akışını temsil eder. Diğer bir deyişle, Giriş Tablosu yeni veriler geldikçe büyümeye devam ediyor. Bu Giriş Tablosu sürekli olarak uzun süren bir sorgu tarafından işlenir ve sonuçlar bir Çıktı Tablosuna yazılır.

![Kıvılcım Yapılandırılmış Akış](./media/apache-spark-streaming-high-availability/structured-streaming.png)

Yapılandırılmış Akış'ta veriler sisteme gelir ve hemen Giriş Tablosuna yutulrülir. Bu Giriş Tablosuna karşı işlem gerçekleştiren sorgular yazarsınız. Sorgu çıktısı, Sonuç Tablosu adı verilen başka bir tablo verir. Sonuçlar Tablosu, dış veri deposuna göndermek için veri çizdiğiniz sorgunuzun sonuçlarını içerir. *Tetikleme aralığı,* Giriş Tablosu'ndan verilerin işlenmesi nin zamanlamasını ayarlar. Varsayılan olarak, Yapılandırılmış Akış verileri gelir gelmez işler. Ancak, tetikleyiciyi daha uzun bir aralıkta çalışacak şekilde de yapılandırabilirsiniz, böylece akış verileri zaman tabanlı toplu işlenmeler için işlenir. Sonuç Tablosundaki veriler, akış sorgusu başladığından beri tüm çıktı verilerini içerecek şekilde yeni veriler olduğunda her yeni lendirilebilir *(tam mod),* veya sorgunun en son işlendiği zamandan beri yalnızca yeni olan verileri içerebilir *(ek modu).*

## <a name="create-fault-tolerant-spark-streaming-jobs"></a>Hataya dayanıklı Kıvılcım Akış işleri oluşturma

Kıvılcım Akış işleriniz için son derece kullanılabilir bir ortam oluşturmak için, başarısız olması durumunda kurtarma için ayrı işlerinizi kodlayarak başlayın. Bu tür kendini iyiletiren işler hataya dayanıklıdır.

RDD'ler, kullanılabilir ve hataya dayanıklı Kıvılcım Akış işlerine yardımcı olan çeşitli özelliklere sahiptir:

* DStream olarak RDD'lerde depolanan giriş verilerinin toplu işserileri, hata toleransı için bellekte otomatik olarak çoğaltılır.
* İşçi hatası nedeniyle kaybedilen veriler, bu işçi düğümleri kullanılabilir olduğu sürece, farklı çalışanlardaki çoğaltılan giriş verilerinden yeniden hesaplanabilir.
* Hatalardan/straggler'lardan kurtarma bellekte hesaplama yoluyla gerçekleştiğinden, hızlı hata kurtarma bir saniye içinde oluşabilir.

### <a name="exactly-once-semantics-with-spark-streaming"></a>Kıvılcım Streaming ile tam olarak bir kez semantik

Her olayı bir kez (ve yalnızca bir kez) işleyen bir uygulama oluşturmak için, bir sorun la karşılaştıktan sonra tüm sistem hata noktalarının nasıl yeniden başlatılabildiğini ve veri kaybını nasıl önleyebileceğinizi göz önünde bulundurun. Tam olarak bir kez semantik hiçbir veri herhangi bir noktada kaybolur gerektirir ve bu ileti işleme, ne olursa olsun hata oluşur yeniden başlatılabilir. Bkz. [Tam olarak bir kez olay işleme ile Spark Akış işleri oluşturun.](apache-spark-streaming-exactly-once.md)

## <a name="spark-streaming-and-apache-hadoop-yarn"></a>Kıvılcım Akış ve Apache Hadoop İplik

HDInsight'ta küme çalışması *Yet Another Resource Negotiator* (YARN) tarafından koordine edilir. Kıvılcım Akış için yüksek kullanılabilirlik tasarımı, Kıvılcım Akış teknikleri ve ayrıca İPLik bileşenleri için içerir.  YARN kullanılarak örnek bir yapılandırma aşağıda gösterilmiştir.

![İplik Mimarisi](./media/apache-spark-streaming-high-availability/hdi-yarn-architecture.png)

Aşağıdaki bölümlerde bu yapılandırma için tasarım konuları açıklayınız.

### <a name="plan-for-failures"></a>Hatalar için plan

Yüksek kullanılabilirlik için bir İplik yapılandırması oluşturmak için olası bir yürütücü veya sürücü hatası planlamanız gerekir. Bazı Kıvılcım Akış işleri, ek yapılandırma ve kurulum gerektiren veri garanti gereksinimlerini de içerir. Örneğin, bir akış uygulaması, barındırma akış sisteminde veya HDInsight kümesinde oluşan herhangi bir hataya rağmen sıfır veri kaybı garantisi için bir iş gereksinimi olabilir.

Bir **uygulayıcı** başarısız olursa, görevleri ve alıcıları Spark tarafından otomatik olarak yeniden başlatılır, bu nedenle yapılandırma değişikliği gerekmez.

Ancak, bir **sürücü** başarısız olursa, ilişkili tüm uygulayıcıları başarısız olur ve alınan tüm bloklar ve hesaplama sonuçları kaybolur. Sürücü hatasından kurtarmak için, [Tam olarak bir kez olay işleme ile Spark Streaming işleri oluştur'da](apache-spark-streaming-exactly-once.md#use-checkpoints-for-drivers)açıklandığı gibi *DStream denetim noktası* kullanın. DStream denetim noktası, DStream'lerin *yönlendirilmiş asiklik grafiğini* (DAG) Azure Depolama gibi hataya dayanıklı depolama alanına düzenli olarak kaydeder.  Denetim noktası, Spark Structured Streaming'in başarısız sürücüyü denetim noktası bilgisinden yeniden başlatmasına olanak tanır.  Bu sürücü yeniden başlatma yeni yürütücüler başlatıyor ve aynı zamanda alıcıları yeniden başlatır.

DStream denetim noktası olan sürücüleri kurtarmak için:

* Otomatik sürücü yeniden yarn yapılandırma ayarı `yarn.resourcemanager.am.max-attempts`ile yapılandırın.
* HDFS uyumlu bir dosya sisteminde bir denetim `streamingContext.checkpoint(hdfsDirectory)`noktası dizini ayarlayın.
* Kurtarma için denetim noktalarını kullanmak için kaynak kodunu yeniden yapılandırma:

    ```scala
        def creatingFunc() : StreamingContext = {
            val context = new StreamingContext(...)
            val lines = KafkaUtils.createStream(...)
            val words = lines.flatMap(...)
            ...
            context.checkpoint(hdfsDir)
        }

        val context = StreamingContext.getOrCreate(hdfsDir, creatingFunc)
        context.start()
    ```

* Kayıp veri kurtarma işlemini, önden yazma günlüğünü `sparkConf.set("spark.streaming.receiver.writeAheadLog.enable","true")`(WAL) ile etkinleştirerek ve dstreams girişi `StorageLevel.MEMORY_AND_DISK_SER`için bellek içi çoğaltmayı devre dışı bırakarak yapılandırın.

Özetlemek gerekirse, denetim noktası + WAL + güvenilir alıcıları kullanarak, "en az bir kez" veri kurtarma sunmak mümkün olacak:

* Tam olarak bir kez, alınan veriler kaybolmadığı ve çıktılar ya idempotent ya da işlemsel olduğu sürece.
* Tam olarak bir kez, yeni Kafka Direct yaklaşımı ile, hangi bir çoğaltılmış günlük olarak Kafka kullanır, yerine alıcıları veya WALs kullanarak.

### <a name="typical-concerns-for-high-availability"></a>Yüksek kullanılabilirlik için tipik endişeler

* Akışlı işleri izlemek, toplu işlerden daha zordur. Kıvılcım Akış işleri genellikle uzun sürelidir ve YARN, bir iş bitene kadar günlükleri toplamaz.  Spark denetim noktaları uygulama veya Spark yükseltmeleri sırasında kaybolur ve bir yükseltme sırasında kontrol noktası dizinini temizlemeniz gerekir.

* Bir istemci başarısız olsa bile, İPLik küme modunuzu sürücüleri çalıştırmak için yapılandırın. Sürücüler için otomatik yeniden başlatmayı ayarlamak için:

    ```
    spark.yarn.maxAppAttempts = 2
    spark.yarn.am.attemptFailuresValidityInterval=1h
    ```

* Kıvılcım ve Kıvılcım Akış UI'si yapılandırılabilir bir ölçüm sistemine sahiptir. Grafit/Grafana gibi ek kitaplıkları kullanarak 'num records işlenmiş', 'sürücü & uygulayıcılarında bellek/GC kullanımı', 'toplam gecikme', 'kümenin kullanımı' gibi pano ölçümlerini indirebilirsiniz. Yapılandırılmış Akış sürüm 2.1 veya daha `StreamingQueryListener` büyük, ek ölçümler toplamak için kullanabilirsiniz.

* Uzun soluklu işleri bölümlere ayırmalısın.  Kümeye bir Kıvılcım Akışı uygulaması gönderildiğinde, işin çalıştığı İplik sırası tanımlanmalıdır. Uzun süren işleri ayrı kuyruklara göndermek için [BIR İpLik Kapasite Zamanlayıcısı](https://hadoop.apache.org/docs/stable/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html) kullanabilirsiniz.

* Akış uygulamanızı incelikle kapatın. İçkileriniz biliniyorsa ve tüm uygulama durumu harici olarak depolanırsa, akış uygulamanızı uygun yerde programlı olarak durdurabilirsiniz. Bir teknik, her *n* saniyede harici bir bayrak için kontrol ederek, Kıvılcım "iplik kancaları" kullanmaktır. Uygulamayı başlatırken HDFS'de oluşturulan ve ardından durdurmak istediğinizde kaldırılan bir *işaretçi dosyası* da kullanabilirsiniz. İşaretçi dosyası yaklaşımı için, Spark uygulamanızda buna benzer kod çağıran ayrı bir iş parçacığı kullanın:

    ```scala
    streamingContext.stop(stopSparkContext = true, stopGracefully = true)
    // to be able to recover on restart, store all offsets in an external database
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark Streaming Genel Bakış](apache-spark-streaming-overview.md)
* [Tam olarak bir kez olay işleme ile Apache Spark Akış işleri oluşturma](apache-spark-streaming-exactly-once.md)
* [Uzun soluklu Apache Kıvılcım Akış İşleri İplik üzerinde](https://mkuthan.github.io/blog/2016/09/30/spark-streaming-on-yarn/)
* [Yapılandırılmış Akış: Hataya Dayanıklı Semantik](https://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html#fault-tolerance-semantics)
* [Ayrık Akışlar: Ölçeklenebilir Akış İşleme için Hataya Dayanıklı Model](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2012/EECS-2012-259.pdf)
