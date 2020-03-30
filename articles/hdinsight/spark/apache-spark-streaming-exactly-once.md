---
title: Kıvılcım Akışı tam olarak bir kez olay işleme & - Azure HDInsight
description: Bir olayı bir kez ve yalnızca bir kez işlemek için Apache Spark Streaming nasıl ayarlanır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/15/2018
ms.openlocfilehash: ee4f9b84e822cb370e5fe3d55fcceb9c8a9f2ab9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74228983"
---
# <a name="create-apache-spark-streaming-jobs-with-exactly-once-event-processing"></a>Tam olarak bir kez olay işleme ile Apache Spark Akış işleri oluşturma

Akış işleme uygulamaları, sistemdeki bazı hatalardan sonra iletileri yeniden işleme şekline farklı yaklaşımlar benimser:

* En az bir kez: Her iletinin işlenmesi garanti edilir, ancak birden fazla kez işlenebilir.
* En fazla bir kerede: Her ileti işlenebilir veya işlenmeyebilir. İleti işlenirse, yalnızca bir kez işlenir.
* Tam olarak bir kez: Her iletinin bir kez ve yalnızca bir kez işlenmesi garanti edilir.

Bu makalede, tam olarak bir kez işleme elde etmek için Spark Streaming yapılandırmak için nasıl gösterir.

## <a name="exactly-once-semantics-with-apache-spark-streaming"></a>Apache Spark Streaming ile tam olarak bir kez semantik

İlk olarak, bir sorun dan sonra tüm sistem hata noktalarının nasıl yeniden başlatılabildiğini ve veri kaybını nasıl önleyebileceğinizi düşünün. Bir Kıvılcım Akış uygulaması vardır:

* Bir giriş kaynağı.
* Giriş kaynağından veri çeken bir veya daha fazla alıcı işlemi.
* Verileri işleyen görevler.
* Çıkış lavabosu.
* Uzun süren işi yöneten bir sürücü işlemi.

Tam olarak bir kez semantik hiçbir noktada hiçbir veri kayıp gerektirir ve ileti işleme, ne olursa olsun hata oluşur yeniden başlatılabilir.

### <a name="replayable-sources"></a>Yeniden oynatılabilir kaynaklar

Kıvılcım Akış uygulamanızın etkinliklerinizi okuduğu kaynak *yeniden oynatılabilir*olmalıdır. Bu, iletinin alındığı ancak iletinin kalıcı veya işlenmeden önce sistemin başarısız olduğu durumlarda, kaynağın aynı iletiyi yeniden sağlaması gerektiği anlamına gelir.

Azure'da, HDInsight'taki Azure Etkinlik Hub'ları ve [Apache Kafka](https://kafka.apache.org/) yeniden oynatılabilir kaynaklar sağlar. Yeniden oynatılabilir bir kaynağa başka bir örnek, tüm verilerin sonsuza dek tutulduğu ve herhangi bir noktada verileri bütünüyle yeniden okuyabileceğiniz [Apache Hadoop HDFS,](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html)Azure Depolama lekeleri veya Azure Veri Gölü Depolama gibi hataya dayanıklı bir dosya sistemidir.

### <a name="reliable-receivers"></a>Güvenilir alıcılar

Spark Streaming'de, Event Hub'ları ve Kafka gibi kaynaklar, her alıcının kaynağı okuyarak kaydettiği ilerlemeyi takip ettiği *güvenilir alıcılara*sahiptir. Güvenilir bir alıcı, [Apache ZooKeeper](https://zookeeper.apache.org/) içinde veya HDFS'ye yazılmış Spark Streaming kontrol noktalarında hataya dayanıklı depolama ya da durumunu devam etti. Böyle bir alıcı başarısız olursa ve daha sonra yeniden başlatılırsa, kaldığı yerden devam edebilir.

### <a name="use-the-write-ahead-log"></a>İleri Ye yaz günlüğünü kullanma

Spark Streaming, alınan her olayın önce Hataya dayanıklı depolama alanında Spark'ın denetim noktası dizinine yazıldığı ve daha sonra Esnek Dağıtılmış Veri Kümesi'nde (RDD) depolandığı Bir Write-Ahead Log kullanımını destekler. Azure'da hataya dayanıklı depolama, Azure Depolama veya Azure Veri Gölü Depolama tarafından desteklenen HDFS'dir. Kıvılcım Akış uygulamanızda, yapılandırma ayarını `spark.streaming.receiver.writeAheadLog.enable` `true`' da ayarlayarak Tüm alıcılar için İleri Yaz Günlüğü etkinleştirilir. Write-Ahead Log hem sürücü hem de uygulayıcıların hataları için hata toleransı sağlar.

Olay verilerine karşı görevleri çalıştıran işçiler için, her RDD tanım gereği hem çoğaltılır hem de birden çok çalışan arasında dağıtılır. Bir görev, çalıştıran alt çalışma çöktüğu için başarısız olursa, olay verilerinin bir kopyasına sahip başka bir alt alt çalışanda görev yeniden başlatılır, böylece olay kaybolmaz.

### <a name="use-checkpoints-for-drivers"></a>Sürücüler için denetim noktalarını kullanma

İş sürücülerinin yeniden başlatılabilir olması gerekir. Kıvılcım Akış uygulamanızı çalıştıran sürücü çökerse, tüm çalışan alıcıları, görevleri ve olay verilerini depolayan RDD'leri de onunla birlikte aşağı çeker. Bu durumda, daha sonra devam edebilirsiniz böylece işin ilerleme kaydetmek gerekir. Bu, DStream'in Yönlendirilmiş Asiklik Grafiğini (DAG) periyodik olarak hataya dayanıklı depolamaya yönlendirilerek gerçekleştirilir. DAG meta verileri, akış uygulamasını oluşturmak için kullanılan yapılandırmayı, uygulamayı tanımlayan işlemleri ve sıraya alınan ancak henüz tamamlanmamış toplu işlemleri içerir. Bu meta veri, başarısız bir sürücünün denetim noktası bilgisinden yeniden başlatılmasını sağlar. Sürücü yeniden başlatıldığında, olay verilerini Yazma-İleri Günlüğü'nden RDD'lere geri kaydeden yeni alıcılar başlatacaktır.

Denetim noktaları Spark Streaming'de iki adımda etkinleştirilir.

1. Akış Bağlamı nesnesinde, denetim noktaları için depolama yolunu yapılandırın:

    ```Scala
    val ssc = new StreamingContext(spark, Seconds(1))
    ssc.checkpoint("/path/to/checkpoints")
    ```

    HDInsight'ta, bu denetim noktaları kümenize bağlı varsayılan depolama alanına kaydedilmelidir( Azure Depolama veya Azure Veri Gölü Depolama).

2. Ardından, DStream üzerinde bir denetim noktası aralığı (saniye cinsinden) belirtin. Her aralıkta, giriş olayından türetilen durum verileri depolamaya devam eder. Kalıcı durum verileri, durumu kaynak olaydan yeniden inşa ederken gereken hesaplamayı azaltabilir.

    ```Scala
    val lines = ssc.socketTextStream("hostname", 9999)
    lines.checkpoint(30)
    ssc.start()
    ssc.awaitTermination()
    ```

### <a name="use-idempotent-sinks"></a>Idempotent lavabolar kullanın

İşinizin sonuç yazdığı hedef, aynı sonucu birden çok kez verildiği durumu ele alabilmeli. Lavabo bu tür yinelenen sonuçları algılamak ve bunları yoksaymak gerekir. Bir *idempotent* lavabo durum değişikliği ile aynı veri ile birden çok kez çağrılabilir.

Datastore'da gelen sonucun varlığını ilk olarak denetleyen mantığı uygulayarak idempotent lavabolar oluşturabilirsiniz. Sonuç zaten varsa, yazma Spark iş perspektifinden başarılı görünmelidir, ancak gerçekte veri deposu yinelenen verileri yoksayılır. Sonuç yoksa, lavabo bu yeni sonucu depolama alanına eklemelidir.

Örneğin, Azure SQL Veritabanı'nda olayları tabloya ekleyen bir saklı yordam kullanabilirsiniz. Bu depolanan yordam önce olayı anahtar alanlara göre arar ve yalnızca eşleşen bir olay bulunamadığında tabloya eklenen kayıt bulunur.

Başka bir örnek, Azure Depolama lekeleri veya Azure Veri Gölü Depolama gibi bölümlenmiş bir dosya sistemi kullanmaktır. Bu durumda, lavabo mantığınızın bir dosyanın varlığını denetlemesi gerekmez. Olayı temsil eden dosya varsa, yalnızca aynı verilerle üzerine yazılır. Aksi takdirde, hesaplanan yolda yeni bir dosya oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark Streaming Genel Bakış](apache-spark-streaming-overview.md)
* [Apache Hadoop İplik'te yüksek oranda kullanılabilir Apache Spark Akış işleri oluşturma](apache-spark-streaming-high-availability.md)
