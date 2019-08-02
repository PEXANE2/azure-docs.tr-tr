---
title: Azure HDInsight 'ta Apache Spark kümesinde Livy sunucusu başlatılamıyor
description: Azure HDInsight 'ta Apache Spark kümesinde Livy sunucusu başlatılamıyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 540b5d2a20764daf2a7f762ac7381f72ce4aabcd
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68668917"
---
# <a name="scenario-livy-server-fails-to-start-on-apache-spark-cluster-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Spark kümesinde Livy sunucusu başlatılamıyor

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Livy sunucu bir Apache Spark [(Linux üzerinde Spark 2,1 (HDI 3,6)] üzerinde başlatılamaz. Aşağıdaki hata yığınında, tüm sonuçları yeniden başlatmaya çalışmak, Livy günlüklerinden:

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

## <a name="cause"></a>Nedeni

`java.lang.OutOfMemoryError: unable to create new native thread`Ana işletim sistemi, JVM 'lere daha fazla yerel iş parçacığı atayamaz. Bu özel durumun, işlem başına iş parçacığı sayısı sınırının ihlalinden kaynaklanmış olduğunu doğrulamıştır.

Livy sunucusu beklenmedik şekilde sonlandırıldığında, Spark kümelerine yapılan tüm bağlantılar da sonlandırılır. Bu, tüm işlerin ve ilgili verilerin kaybedildiği anlamına gelir. HDP 2,6 oturum kurtarma mekanizması tanıtılmıştı, Livy sunucu geri alındıktan sonra kurtarılacak Zookeeper içinde oturum ayrıntılarını depolar.

Livy aracılığıyla çok sayıda iş gönderildiğinde, Livy sunucusu için yüksek kullanılabilirlik parçası olan bu oturum durumlarını ZK 'de (HDInsight kümelerinde) depolar ve Livy hizmeti yeniden başlatıldığında bu oturumları kurtarır. Beklenmedik sonlandırma sonrasında, Livy oturum başına bir iş parçacığı oluşturur ve bu, çok fazla iş parçacığı oluşturulmasını neden olan belirli bir sayıda kurtarılabilir oturumu biriktirir.

## <a name="resolution"></a>Çözüm

Aşağıda açıklanan adımları kullanarak tüm girdileri silin.

1. Kullanarak Zookeeper düğümlerinin IP adresini al

    ```bash
    grep -R zk /etc/hadoop/conf  
    ```

1. Yukarıdaki komut, Kümem için tüm zookeepers listelendi

    ```bash
    /etc/hadoop/conf/core-site.xml:      <value>zk1-hwxspa.lnuwp5akw5ie1j2gi2amtuuimc.dx.internal.cloudapp.net:2181,zk2-      hwxspa.lnuwp5akw5ie1j2gi2amtuuimc.dx.internal.cloudapp.net:2181,zk4-hwxspa.lnuwp5akw5ie1j2gi2amtuuimc.dx.internal.cloudapp.net:2181</value>
    ```

1. Ping kullanarak Zookeeper düğümlerinin tüm IP adreslerini alın veya Ayrıca ZK adını kullanarak baş düğümüne 'dan Zookeeper 'e bağlanabilirsiniz

    ```bash
    /usr/hdp/current/zookeeper-client/bin/zkCli.sh -server zk2-hwxspa:2181
    ```

1. Zookeeper 'e bağlandıktan sonra, yeniden başlatılmaya çalışılan tüm oturumları listelemek için aşağıdaki komutu yürütün.

    1. Çoğu durumda bu durum 8000 ' den fazla oturumu bir liste olabilir ####

        ```bash
        ls /livy/v1/batch
        ```

    1. Aşağıdaki komut, tüm kurtarılabilir oturumları kaldırmalıdır. #####

        ```bash
        rmr /livy/v1/batch
        ```

1. Yukarıdaki komutun tamamlanmasını bekleyin ve imleci bir süre sonra yeniden başlatın ve bu işlemin başarılı olması gerekir.

> [!NOTE]
> `DELETE`Yürütme tamamlandıktan sonra tüm oturum. Tek tek toplu oturumlar, bir tasarıma göre olan Spark uygulaması tamamlandıktan hemen sonra otomatik olarak silinmez. Livy oturumu, Livy Rest sunucusuna yönelik bir POST isteği tarafından oluşturulan bir varlıktır. Varlığı `DELETE` silmek için bir çağrı gerekir. Ya da GC 'nin başlatılmasını beklememiz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
