---
title: Azure HDInsight'ta Apache Spark için OutOfMemoryError özel durumları
description: Azure HDInsight'ta Apache Spark kümesi için çeşitli OutOfMemoryError özel durumları
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/15/2019
ms.openlocfilehash: 31cdef281b1cb26d01a4690c815e3d3621e2c053
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271973"
---
# <a name="outofmemoryerror-exceptions-for-apache-spark-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Spark için OutOfMemoryError özel durumları

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="scenario-outofmemoryerror-exception-for-apache-spark"></a>Senaryo: Apache Spark için OutOfMemoryError özel durumu

### <a name="issue"></a>Sorun

Apache Spark uygulamanız, OutOfMemoryError işlenmemiş bir özel durumla başarısız oldu. Şuna benzer bir hata iletisi alabilirsiniz:

```error
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439)

java.lang.OutOfMemoryError
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source)
    at java.io.ByteArrayOutputStream.grow(Unknown Source)
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source)
    at java.io.ByteArrayOutputStream.write(Unknown Source)
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source)
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source)
    at java.io.ObjectOutputStream.writeObject0(Unknown Source)
    at java.io.ObjectOutputStream.writeObject(Unknown Source)
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44)
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101)
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
    at java.lang.Thread.run(Unknown Source)
```

```error
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main]

java.lang.OutOfMemoryError
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source)
    ...
```

### <a name="cause"></a>Nedeni

Bu özel durum en olası nedeni yeterli yığın bellek Java sanal makineler (JVM) tahsis olmasıdır. Bu JVM'ler Apache Spark uygulamasının bir parçası olarak uygulayıcıveya sürücü olarak başlatılır.

### <a name="resolution"></a>Çözüm

1. Spark uygulamasının işleyeceği verilerin boyut üst sınırını belirleyin. Girdi verilerinin boyutunun maksimumuna, girdi verilerinin dönüştürülmesiyle üretilen ara verilerin ve ara verilerin daha da dönüştürülmesiyle üretilen çıktı verilerine göre boyutu tahmin edin. İlk tahmin yeterli değilse, boyutu biraz artırın ve bellek hataları yatışana kadar yineleyin.

1. Kullanılacak HDInsight kümesinin, Spark uygulamasını barındırmak için yeterli kaynağa (bellek ve ayrıca çekirdek olarak) sahip olduğundan emin olun. Bu, kullanılan **Memory Total** ve **VCores'un** Toplam'a karşı **kullanılan Bellek** değerleri için kümenin İplik **VCores Total**UI'sinin Küme Ölçümleri bölümünü görüntüleyerek belirlenebilir.

    ![iplik çekirdek bellek görünümü](./media/apache-spark-ts-outofmemory/yarn-core-memory-view.png)

1. Aşağıdaki Spark yapılandırmalarını uygun değerlere ayarlayın. Uygulama gereksinimlerini kümedeki kullanılabilir kaynaklarla dengeleyin. Bu değerler, İplik tarafından görüntülenen kullanılabilir bellek ve çekirdeklerin %90'ını geçmemeli ve Aynı zamanda Kıvılcım uygulamasının minimum bellek gereksinimini karşılamalıdır:

    ```
    spark.executor.instances (Example: 8 for 8 executor count)
    spark.executor.memory (Example: 4g for 4 GB)
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB)
    spark.executor.cores (Example: 2 for 2 cores per executor)
    spark.driver.memory (Example: 8g for 8GB)
    spark.driver.cores (Example: 4 for 4 cores)
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB)
    ```

    Tüm uygulayıcılar tarafından kullanılan toplam bellek =

    ```
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```

    Sürücü tarafından kullanılan toplam bellek =

    ```
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

---

## <a name="scenario-java-heap-space-error-when-trying-to-open-apache-spark-history-server"></a>Senaryo: Apache Spark tarih sunucusu açmaya çalışırken Java yığın alanı hatası

### <a name="issue"></a>Sorun

Spark History sunucusunda olayları açarken aşağıdaki hatayı alırsınız:

```
scala.MatchError: java.lang.OutOfMemoryError: Java heap space (of class java.lang.OutOfMemoryError)
```

### <a name="cause"></a>Nedeni

Bu sorun genellikle büyük kıvılcım olay dosyaları açarken kaynak eksikliği neden olur. Spark yığını boyutu varsayılan olarak 1 GB olarak ayarlanır, ancak büyük Spark olay dosyaları bundan daha fazlasını gerektirebilir.

Yüklemeye çalıştığınız dosyaların boyutunu doğrulamak istiyorsanız, aşağıdaki komutları gerçekleştirebilirsiniz:

```bash
hadoop fs -du -s -h wasb:///hdp/spark2-events/application_1503957839788_0274_1/
**576.5 M**  wasb:///hdp/spark2-events/application_1503957839788_0274_1

hadoop fs -du -s -h wasb:///hdp/spark2-events/application_1503957839788_0264_1/
**2.1 G**  wasb:///hdp/spark2-events/application_1503957839788_0264_1
```

### <a name="resolution"></a>Çözüm

Özelliği Spark yapılandırmasında düzenleyerek `SPARK_DAEMON_MEMORY` ve tüm hizmetleri yeniden başlatarak Spark History Server belleği artırabilirsiniz.

Bunu Ambari tarayıcı kullanıcı arabirimi içinden Spark2/Config/Advanced spark2-env bölümünü seçerek yapabilirsiniz.

![Gelişmiş spark2-env bölümü](./media/apache-spark-ts-outofmemory-heap-space/apache-spark-image01.png)

Spark History Server belleği 1g'den 4g'ye `SPARK_DAEMON_MEMORY=4g`değiştirmek için aşağıdaki özelliği ekleyin: .

![Kıvılcım özelliği](./media/apache-spark-ts-outofmemory-heap-space/apache-spark-image02.png)

Ambari'den etkilenen tüm hizmetleri yeniden başlattığınızdan emin olun.

---

## <a name="scenario-livy-server-fails-to-start-on-apache-spark-cluster"></a>Senaryo: Livy Server Apache Spark kümesinde başlatılamıyor

### <a name="issue"></a>Sorun

Livy Server bir Apache Spark [(Spark 2.1 Linux (HDI 3.6)]tarihinde başlatılamaz. Livy günlüklerinden aşağıdaki hata yığınında sonuçları yeniden başlatmaya çalışAlım:

```log
17/07/27 17:52:50 INFO CuratorFrameworkImpl: Starting
17/07/27 17:52:50 INFO ZooKeeper: Client environment:zookeeper.version=3.4.6-29--1, built on 05/15/2017 17:55 GMT
17/07/27 17:52:50 INFO ZooKeeper: Client environment:host.name=10.0.0.66
17/07/27 17:52:50 INFO ZooKeeper: Client environment:java.version=1.8.0_131
17/07/27 17:52:50 INFO ZooKeeper: Client environment:java.vendor=Oracle Corporation
17/07/27 17:52:50 INFO ZooKeeper: Client environment:java.home=/usr/lib/jvm/java-8-openjdk-amd64/jre
17/07/27 17:52:50 INFO ZooKeeper: Client environment:java.class.path= <DELETED>
17/07/27 17:52:50 INFO ZooKeeper: Client environment:java.library.path= <DELETED>
17/07/27 17:52:50 INFO ZooKeeper: Client environment:java.io.tmpdir=/tmp
17/07/27 17:52:50 INFO ZooKeeper: Client environment:java.compiler=<NA>
17/07/27 17:52:50 INFO ZooKeeper: Client environment:os.name=Linux
17/07/27 17:52:50 INFO ZooKeeper: Client environment:os.arch=amd64
17/07/27 17:52:50 INFO ZooKeeper: Client environment:os.version=4.4.0-81-generic
17/07/27 17:52:50 INFO ZooKeeper: Client environment:user.name=livy
17/07/27 17:52:50 INFO ZooKeeper: Client environment:user.home=/home/livy
17/07/27 17:52:50 INFO ZooKeeper: Client environment:user.dir=/home/livy
17/07/27 17:52:50 INFO ZooKeeper: Initiating client connection, connectString=zk2-kcspark.cxtzifsbseee1genzixf44zzga.gx.internal.cloudapp.net:2181,zk3-kcspark.cxtzifsbseee1genzixf44zzga.gx.internal.cloudapp.net:2181,zk6-kcspark.cxtzifsbseee1genzixf44zzga.gx.internal.cloudapp.net:2181 sessionTimeout=60000 watcher=org.apache.curator.ConnectionState@25fb8912
17/07/27 17:52:50 INFO StateStore$: Using ZooKeeperStateStore for recovery.
17/07/27 17:52:50 INFO ClientCnxn: Opening socket connection to server 10.0.0.61/10.0.0.61:2181. Will not attempt to authenticate using SASL (unknown error)
17/07/27 17:52:50 INFO ClientCnxn: Socket connection established to 10.0.0.61/10.0.0.61:2181, initiating session
17/07/27 17:52:50 INFO ClientCnxn: Session establishment complete on server 10.0.0.61/10.0.0.61:2181, sessionid = 0x25d666f311d00b3, negotiated timeout = 60000
17/07/27 17:52:50 INFO ConnectionStateManager: State change: CONNECTED
17/07/27 17:52:50 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
17/07/27 17:52:50 INFO AHSProxy: Connecting to Application History server at headnodehost/10.0.0.67:10200
Exception in thread "main" java.lang.OutOfMemoryError: unable to create new native thread
  at java.lang.Thread.start0(Native Method)
  at java.lang.Thread.start(Thread.java:717)
  at com.cloudera.livy.Utils$.startDaemonThread(Utils.scala:98)
  at com.cloudera.livy.utils.SparkYarnApp.<init>(SparkYarnApp.scala:232)
  at com.cloudera.livy.utils.SparkApp$.create(SparkApp.scala:93)
  at com.cloudera.livy.server.batch.BatchSession$$anonfun$recover$2$$anonfun$apply$4.apply(BatchSession.scala:117)
  at com.cloudera.livy.server.batch.BatchSession$$anonfun$recover$2$$anonfun$apply$4.apply(BatchSession.scala:116)
  at com.cloudera.livy.server.batch.BatchSession.<init>(BatchSession.scala:137)
  at com.cloudera.livy.server.batch.BatchSession$.recover(BatchSession.scala:108)
  at com.cloudera.livy.sessions.BatchSessionManager$$anonfun$$init$$1.apply(SessionManager.scala:47)
  at com.cloudera.livy.sessions.BatchSessionManager$$anonfun$$init$$1.apply(SessionManager.scala:47)
  at scala.collection.TraversableLike$$anonfun$map$1.apply(TraversableLike.scala:244)
  at scala.collection.TraversableLike$$anonfun$map$1.apply(TraversableLike.scala:244)
  at scala.collection.mutable.ResizableArray$class.foreach(ResizableArray.scala:59)
  at scala.collection.mutable.ArrayBuffer.foreach(ArrayBuffer.scala:47)
  at scala.collection.TraversableLike$class.map(TraversableLike.scala:244)
  at scala.collection.AbstractTraversable.map(Traversable.scala:105)
  at com.cloudera.livy.sessions.SessionManager.com$cloudera$livy$sessions$SessionManager$$recover(SessionManager.scala:150)
  at com.cloudera.livy.sessions.SessionManager$$anonfun$1.apply(SessionManager.scala:82)
  at com.cloudera.livy.sessions.SessionManager$$anonfun$1.apply(SessionManager.scala:82)
  at scala.Option.getOrElse(Option.scala:120)
  at com.cloudera.livy.sessions.SessionManager.<init>(SessionManager.scala:82)
  at com.cloudera.livy.sessions.BatchSessionManager.<init>(SessionManager.scala:42)
  at com.cloudera.livy.server.LivyServer.start(LivyServer.scala:99)
  at com.cloudera.livy.server.LivyServer$.main(LivyServer.scala:302)
  at com.cloudera.livy.server.LivyServer.main(LivyServer.scala)
  
  ## using "vmstat" found  we had enough free memory
```

### <a name="cause"></a>Nedeni

`java.lang.OutOfMemoryError: unable to create new native thread`OS'nin JVM'lere daha fazla yerel iş parçacığı atayamayacağını vurgular. Bu Özel Durum'un işlem başına iş parçacığı sayısı sınırının ihlalinden kaynaklandığı doğrulandı.

Livy Server beklenmedik bir şekilde sonlandığında, Kıvılcım Kümeleri'ne olan tüm bağlantılar da sonlandırılır, bu da tüm işlerin ve ilgili verilerin kaybolacağı anlamına gelir. HDP'de 2.6 oturumkurtarma mekanizması uygulamaya kondu, Livy, Livy Server geri döndükten sonra kurtarılmak üzere Seans detaylarını Zookeeper'da saklar.

Livy üzerinden çok sayıda iş gönderildiğinde, Livy Server için Yüksek Kullanılabilirlik'in bir parçası olarak bu oturum durumları ZK'da (HDInsight kümelerinde) saklar ve Livy hizmeti yeniden başlatıldığında bu oturumları kurtarır. Beklenmeyen sonlandırmadan sonra yeniden başlatılırken, Livy oturum başına bir iş parçacığı oluşturur ve bu, çok fazla iş parçacığı nın oluşturulmasına neden olacak belirli sayıda kurtarılacak oturumlar birikir.

### <a name="resolution"></a>Çözüm

Aşağıda ayrıntılı olarak belirtilen adımları kullanarak tüm girişleri silin.

1. Zookeeper Düğümlerinin IP adresini kullanarak alın

    ```bash
    grep -R zk /etc/hadoop/conf  
    ```

1. Yukarıdaki komut benim küme için tüm zookeepers listelenen

    ```bash
    /etc/hadoop/conf/core-site.xml:      <value>zk1-hwxspa.lnuwp5akw5ie1j2gi2amtuuimc.dx.internal.cloudapp.net:2181,zk2-      hwxspa.lnuwp5akw5ie1j2gi2amtuuimc.dx.internal.cloudapp.net:2181,zk4-hwxspa.lnuwp5akw5ie1j2gi2amtuuimc.dx.internal.cloudapp.net:2181</value>
    ```

1. Ping kullanarak zookeeper düğümlerinin tüm IP adreslerini alın Veya zk adını kullanarak headnode zookeeper bağlanabilirsiniz

    ```bash
    /usr/hdp/current/zookeeper-client/bin/zkCli.sh -server zk2-hwxspa:2181
    ```

1. Zookeeper'a bağlandıktan sonra yeniden başlatmaya çalışılmaya çalışılan tüm oturumları listelemek için aşağıdaki komutu uygulayın.

    1. Vakaların çoğu bu 8000'den fazla oturumları bir liste olabilir ####

        ```bash
        ls /livy/v1/batch
        ```

    1. Aşağıdaki komut, kurtarılacak tüm oturumları kaldırmaktır. #####

        ```bash
        rmr /livy/v1/batch
        ```

1. Yukarıdaki komutun tamamlanmasını ve imlecin komut istemini döndürmesini bekleyin ve sonra Ambari'den Livy hizmetini yeniden başlatın, bu da başarılı olmalıdır.

> [!NOTE]
> `DELETE`yürütülmesi tamamlandıktan sonra livy oturumu. Livy toplu iş oturumları, kıvılcım uygulaması tamamlanır tamamlanmaz otomatik olarak silinmez, ki bu da tasarım gereğidir. Livy oturumu, Livy Rest sunucusuna karşı bir POST isteği tarafından oluşturulan bir varlıktır. Bu `DELETE` varlığı silmek için bir arama gereklidir. Ya da GC'nin etkisini beklemeli.

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Kıvılcım bellek yönetimi genel bakış](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview).

* [HDInsight kümelerinde Hata Ayıklama Spark uygulaması.](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
