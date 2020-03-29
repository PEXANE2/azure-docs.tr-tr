---
title: Azure HDInsight 3.6 Apache Storm'u HDInsight 4.0 Apache Spark'a geçirin
description: Apache Storm iş yüklerinin Spark Streaming'e veya Yapılandırılmış Akışı Kıvılcımla yönetmesi için farklar ve geçiş akışı.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/16/2019
ms.openlocfilehash: 916c54c3739d1164e4e9c1db67aa1f4e0dbd0c6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76157798"
---
# <a name="migrate-azure-hdinsight-36-apache-storm-to-hdinsight-40-apache-spark"></a>Azure HDInsight 3.6 Apache Storm'u HDInsight 4.0 Apache Spark'a geçirin

Bu belge, HDInsight 3.6'daki Apache Storm iş yüklerinin nasıl HDInsight 4.0'a geçirilen yollardan açıklanmaktadır. HDInsight 4.0, Apache Storm küme türünü desteklemez ve başka bir akış veri platformuna geçiş yapmanız gerekir. Apache Spark Streaming ve Spark Structured Streaming olmak için iki uygun seçenek vardır. Bu belge, bu platformlar arasındaki farkları açıklar ve ayrıca Apache Storm iş yüklerini geçirmek için bir iş akışı önerir.

## <a name="storm-migration-paths-in-hdinsight"></a>HDInsight'ta fırtına geçiş yolları

HDInsight 3.6'da Apache Storm'dan geçiş yapmak istiyorsanız birden çok seçeneğiniz vardır:

* HDInsight 4.0'da Kıvılcım Akışı
* HDInsight 4.0'da Kıvılcım Yapılandırılmış Akış
* Azure Stream Analytics

Bu belge, Apache Storm'dan Spark Streaming ve Spark Structured Streaming'e geçiş için bir kılavuz sağlar.

> [!div class="mx-imgBorder"]
> ![HDInsight Fırtına geçiş yolu](./media/migrate-storm-to-spark/storm-migration-path.png)

## <a name="comparison-between-apache-storm-and-spark-streaming-spark-structured-streaming"></a>Apache Storm ve Spark Streaming, Kıvılcım Yapılandırılmış Akış arasında karşılaştırma

Apache Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir Fırtına uygulaması en az bir kez işlemeyi garanti edebilir ve [Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html) tam olarak bir kez işlemeyi garanti edebilir. Kıvılcım Akışı ve Spark Structured Streaming, düğüm hatası oluşsa bile herhangi bir giriş olayının tam olarak bir kez işlenmeyi garanti eder. Storm'un her bir olayı işleyen bir modeli vardır ve Trident ile Micro Batch modelini de kullanabilirsiniz. Kıvılcım Akışı ve Kıvılcım Yapılandırılmış Akış Micro-Batch işleme modeli sağlar.

|  |Storm |Spark Streaming | Kıvılcım yapılandırılmış akış|
|---|---|---|---|
|**Olay işleme garantisi**|En az bir kez <br> Tam Bir Kez (Trident) |[Tam Bir Kez](https://spark.apache.org/docs/latest/streaming-programming-guide.html)|[Tam Bir Kez](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)|
|**İşleme Modeli**|Gerçek zamanlı <br> Mikro Toplu İşlem (Trident) |Mikro Toplu İş |Mikro Toplu İş |
|**Etkinlik zamanı desteği**|[Evet](https://storm.apache.org/releases/2.0.0/Windowing.html)|Hayır|[Evet](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)|
|**Diller**|Java, vb.|Scala, Java, Python|Python, R, Scala, Java, SQL|

### <a name="spark-streaming-vs-spark-structured-streaming"></a>Kıvılcım akışı vs Spark yapılandırılmış akış

Spark Structured Streaming, Spark Streaming'in (DStreams) yerini alıyor. Yapılandırılmış Akış geliştirmeler ve bakım almaya devam ederken, DStreams yalnızca bakım modunda olacaktır. **Not: bu noktayı vurgulamak için bağlantılara ihtiyacınız var.** Yapılandırılmış Akış, kutunun dışında desteklediği kaynaklar ve lavabolar için DStreams kadar fazla özelliğe sahip değildir, bu nedenle uygun Spark akışı işleme seçeneğini seçmek için gereksinimlerinizi değerlendirin.

## <a name="streaming-single-event-processing-vs-micro-batch-processing"></a>Akış (Tek olay) işleme vs Micro-Batch işleme

Storm, her bir olayı işleyen bir model sağlar. Bu, gelen tüm kayıtların geldikleri anda işleneceği anlamına gelir. Kıvılcım Akış uygulamaları, bu toplu işlemi için göndermeden önce her bir mikro dizi olayı toplamak için saniyenin bir kısmını beklemelidir. Buna karşılık, olay odaklı bir uygulama her olayı hemen işler. Kıvılcım Akışı gecikmesi genellikle birkaç saniyenin altındadır. Mikro-toplu yaklaşımın yararları daha verimli veri işleme ve basit toplam hesaplamalar vardır.

> [!div class="mx-imgBorder"]
> ![akış ve mikro toplu işleme](./media/migrate-storm-to-spark/streaming-and-micro-batch-processing.png)

## <a name="storm-architecture-and-components"></a>Fırtına mimarisi ve bileşenleri

Storm topolojileri döngüsel olmayan yönlü grafikte (DAG) düzenlenmiş birden fazla bileşenden oluşur. Veriler grafikteki bileşenler arasında akar. Her bileşen bir veya daha fazla veri akışı kullanır ve isteğe bağlı olarak bir veya daha fazla akış yayar.

|Bileşen |Açıklama |
|---|---|
|Emzik|Verileri bir topolojiye getirir. Bu bileşenler topolojiye bir veya daha fazla akış yayar.|
|Cıvata|Emzitlerden veya diğer cıvatalardan yayılan akışları tüketir. Boltlar topolojiye isteğe bağlı olarak akışlar yayabilir. Boltlar ayrıca HDFS, Kafka veya HBase gibi dış hizmetlere veya depolama alanlarına veri yazmaktan sorumludur.|

> [!div class="mx-imgBorder"]
> ![fırtına bileşenlerinin etkileşimi](./media/migrate-storm-to-spark/apache-storm-components.png)

Fırtına, Fırtına kümesinin çalışmasını sağlayan aşağıdaki üç daemondan oluşur.

|Daemon |Açıklama |
|---|---|
|Nimbus|Hadoop JobTracker'a benzer şekilde, küme nin etrafında kod dağıtmakve makinelere görev atamak ve hataları izlemekten sorumludur.|
|Zookeeper|Küme koordinasyonu için kullanılır.|
|Gözetmen|Makinesine atanan işleri dinler ve Nimbus'tan gelen direktiflere göre işçi işlemlerini başlatır ve durdurur. Her alt işlem bir topolojinin bir alt kümesini yürütür. Kullanıcının uygulama mantığı (Spouts ve Bolt) burada çalışır.|

> [!div class="mx-imgBorder"]
> ![nimbus, zookeeper ve süpervizör daemons](./media/migrate-storm-to-spark/nimbus-zookeeper-supervisor.png)

## <a name="spark-streaming-architecture-and-components"></a>Kıvılcım Akış mimarisi ve bileşenleri

Aşağıdaki adımlar, bileşenlerin Spark Streaming (DStreams) ve Spark Structured Streaming'de birlikte nasıl çalıştığını özetler:

* Kıvılcım Akışı başlatıldığında, sürücü yürütme de görev başlattı.
* Uygulayıcı, akış lı bir veri kaynağından bir akış alır.
* Uygulayıcı veri akışları aldığında, akışı bloklara böler ve bellekte tutar.
* Veri blokları diğer uygulayıcılara çoğaltılır.
* İşlenen veriler daha sonra hedef veri deposunda depolanır.

> [!div class="mx-imgBorder"]
> ![çıkışa akış yolu kıvılcım](./media/migrate-storm-to-spark/spark-streaming-to-output.png)

## <a name="spark-streaming-dstream-workflow"></a>Kıvılcım Akışı (DStream) iş akışı

Her toplu işlem aralığı sona ererken, bu aralıktaki tüm verileri içeren yeni bir RDD üretilir. Sürekli RDD kümeleri bir DStream'de toplanır. Örneğin, toplu iş aralığı bir saniye uzunluğundaysa, DStream'iniz her saniye, o saniye boyunca alınan tüm verileri içeren bir RDD içeren bir toplu iş partisi yayır. DStream işlenirken, sıcaklık olayı bu toplu işlerden birinde görünür. Bir Kıvılcım Akış uygulaması, olayları içeren toplu işlemleri işler ve sonuçta her RDD'de depolanan veriler üzerinde hareket eder.

> [!div class="mx-imgBorder"]
> ![kıvılcım akış işleme toplu](./media/migrate-storm-to-spark/spark-streaming-batches.png)

Kıvılcım Akışı ile kullanılabilen farklı dönüşümler hakkında ayrıntılı bilgi için [DStreams'deki Dönüşümler'e](https://spark.apache.org/docs/latest/streaming-programming-guide.html#transformations-on-dstreams)bakın.

## <a name="spark-structured-streaming"></a>Kıvılcım Yapılandırılmış Akış

Spark Structured Streaming, derinlemesine sınırlandırılmamış bir tablo olarak veri akışını temsil eder. Yeni veriler geldikçe tablo büyümeye devam ediyor. Bu giriş tablosu sürekli olarak uzun süren bir sorgu tarafından işlenir ve sonuçlar bir çıktı tablosuna gönderilir.

Yapılandırılmış Akış'ta, veriler sisteme gelir ve hemen bir giriş tablosuna yutulrülir. Bu giriş tablosuna karşı işlem gerçekleştiren sorgular (DataFrame ve Dataset API'lerini kullanarak) yazarsınız.

Sorgu çıktısı, sorgunuzun sonuçlarını içeren bir *sonuç tablosu*verir. Dış veri deposu, böyle bir ilişkisel veritabanı için sonuç tablosundan veri çizebilirsiniz.

Giriş tablosundan verilerin işlenmesinin zamanlaması tetikleme aralığı tarafından denetlenir. Varsayılan olarak, tetikleme aralığı sıfırdır, bu nedenle Yapılandırılmış Akış verileri gelir gelmez işlemeye çalışır. Uygulamada, bu, Yapılandırılmış Akış önceki sorgunun çalışmasını işleme yapılır yapılmaz, yeni alınan verilere karşı başka bir işleme çalıştırma başlattığı anlamına gelir. Etkine, akış verilerinin zaman tabanlı toplu işlenmeleri için bir aralıkta çalışacak şekilde yapılandırabilirsiniz.

> [!div class="mx-imgBorder"]
> ![yapılandırılmış akışta verilerin işlenmesi](./media/migrate-storm-to-spark/structured-streaming-data-processing.png)

> [!div class="mx-imgBorder"]
> ![yapılandırılmış akış için programlama modeli](./media/migrate-storm-to-spark/structured-streaming-model.png)

## <a name="general-migration-flow"></a>Genel geçiş akışı

Fırtına'dan Kıvılcım'a önerilen geçiş akışı aşağıdaki ilk mimariyi varsayar:

* Kafka akış veri kaynağı olarak kullanılır
* Kafka ve Storm aynı sanal ağda konuşlandırıldı
* Storm tarafından işlenen veriler, Azure Depolama veya Azure Veri Gölü Depolama Gen2 gibi bir veri lavabosu için yazılır.

    > [!div class="mx-imgBorder"]
    > ![tahmin edilen geçerli ortamın diyagramı](./media/migrate-storm-to-spark/presumed-current-environment.png)

Uygulamanızı Storm'dan Spark akış API'lerinden birine geçirmek için aşağıdakileri yapın:

1. **Yeni bir küme dağıtın.** Aynı sanal ağa yeni bir HDInsight 4.0 Kıvılcım kümesi dağıtın ve Spark Streaming veya Spark Yapılandırılmış Akış uygulamanızı üzerine dağıtın ve iyice test edin.

    > [!div class="mx-imgBorder"]
    > ![HDInsight'ta yeni kıvılcım dağıtımı](./media/migrate-storm-to-spark/new-spark-deployment.png)

1. **Eski Fırtına kümesini tüketmeyi bırak.** Varolan Fırtına'da, akış veri kaynağından veri tüketmeyi durdurun ve verilerin hedef lavaboya yazmayı bitirmesini bekleyin.

    > [!div class="mx-imgBorder"]
    > ![geçerli kümeüzerinde tüketmeyi durdurma](./media/migrate-storm-to-spark/stop-consuming-current-cluster.png)

1. **Yeni Kıvılcım kümesini tüketmeye başlayın.** Yeni dağıtılan HDInsight 4.0 Spark kümesinden veri akışına başlayın. Şu anda, süreç en son Kafka ofset tüketerek devralınır.

    > [!div class="mx-imgBorder"]
    > ![yeni kümeüzerinde tüketmeye başlayın](./media/migrate-storm-to-spark/start-consuming-new-cluster.png)

1. **Gerektiğinde eski kümeyi kaldırın.** Anahtar tamamlandıktan ve düzgün çalıştığında, eski HDInsight 3.6 Fırtına kümesini gerektiği gibi kaldırın.

    > [!div class="mx-imgBorder"]
    > ![gerektiğinde eski HDInsight kümelerini kaldırma](./media/migrate-storm-to-spark/remove-old-clusters1.png)

## <a name="next-steps"></a>Sonraki adımlar

Storm, Spark Streaming ve Spark Structured Streaming hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Apache Spark Streaming'e Genel Bakış](../spark/apache-spark-streaming-overview.md)
* [Apache Spark Yapılandırılmış Akışa Genel Bakış](../spark/apache-spark-structured-streaming-overview.md)