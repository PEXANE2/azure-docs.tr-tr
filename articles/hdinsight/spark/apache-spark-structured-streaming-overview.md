---
title: Azure HDInsight'ta Yapılandırılmış Akış Kıvılcım
description: HDInsight Spark kümelerinde Spark Yapılandırılmış Akış uygulamaları nasıl kullanılır?
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/24/2019
ms.openlocfilehash: 19cfd5d8ed4100048c270fb41e5e54a920c61516
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75548845"
---
# <a name="overview-of-apache-spark-structured-streaming"></a>Apache Spark Yapılandırılmış Akışa Genel Bakış

[Apaçi Kıvılcım](https://spark.apache.org/) Yapılandırılmış Akış, veri akışlarını işlemek için ölçeklenebilir, yüksek iş letme, hataya dayanıklı uygulamalar uygulamanızı sağlar. Yapılandırılmış Akış, Spark SQL motoru üzerine kuruludur ve Toplu sorguları yazdığınız şekilde akış sorguları yazabilmeniz için Spark SQL Veri Çerçeveleri ve Veri Kümeleri'ndeki yapıları geliştirir.  

Yapılandırılmış Akış uygulamaları HDInsight Spark kümelerinde çalışır ve Bir TCP soketi [(hata](https://kafka.apache.org/)ayıklama amacıyla), Azure Depolama veya Azure Veri Gölü Depolama'dan gelen akış verilerine bağlanır. Harici depolama hizmetlerine dayanan son iki seçenek, depolamaya eklenen yeni dosyaları izlemenize ve içeriklerini akışlı yatmaktadır gibi işlemenize olanak tanır.

Yapılandırılmış Akış, seçim, projeksiyon, toplama, pencereleme ve akış DataFrame'ine başvuru DataFrame'leri ile katılma gibi giriş verilerine işlemleri uyguladığınız uzun soluklu bir sorgu oluşturur. Ardından, özel kod (SQL Veritabanı veya Power BI gibi) kullanarak sonuçları dosya depolamasına (Azure Depolama Lekeleri veya Veri Gölü Depolama) veya herhangi bir veri deposuna çıktınız. Yapılandırılmış Akış, HDInsight'ta hata ayıklama için oluşturulan verileri görebilmeniz için konsola yerel olarak hata ayıklama için çıktı ve bellek içi tabloya da çıkış sağlar.

![HDInsight ve Kıvılcım Yapılandırılmış Akış ile Akış İşleme](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming.png)

> [!NOTE]  
> Spark Structured Streaming, Spark Streaming'in (DStreams) yerini alıyor. İleriye dönük olarak, Yapılandırılmış Akış geliştirmeler ve bakım lar alırken, DStream'ler yalnızca bakım modunda olacaktır. Yapılandırılmış Akış şu anda kutunun dışında desteklediği kaynaklar ve lavabolar için DStreams kadar özellik tam değildir, bu nedenle uygun Spark akışı işleme seçeneğini seçmek için gereksinimlerinizi değerlendirin.

## <a name="streams-as-tables"></a>Tablo olarak akışlar

Spark Structured Streaming, derinlemesine sınırsız bir tablo olarak bir veri akışını temsil eder, yani yeni veriler geldikçe tablo büyümeye devam eder. Bu *giriş tablosu* sürekli olarak uzun süren bir sorgu tarafından işlenir ve sonuçlar bir çıktı *tablosuna*gönderilir:

![Yapılandırılmış Akış Kavramı](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-concept.png)

Yapılandırılmış Akış'ta, veriler sisteme gelir ve hemen bir giriş tablosuna yutulrülir. Bu giriş tablosuna karşı işlem gerçekleştiren sorgular (DataFrame ve Dataset API'lerini kullanarak) yazarsınız. Sorgu çıktısı başka bir tablo, *sonuç tablosu*verir. Sonuç tablosu, örneğin ilişkisel bir veritabanı olan harici bir veri deposu için veri çizdiğiniz sorgunuzun sonuçlarını içerir. Giriş tablosundan verilerin işlenmesi nin zamanlaması *tetikleme aralığı*tarafından denetlenir. Varsayılan olarak, tetikleme aralığı sıfırdır, bu nedenle Yapılandırılmış Akış verileri gelir gelmez işlemeye çalışır. Uygulamada, bu, Yapılandırılmış Akış önceki sorgunun çalışmasını işleme yapılır yapılmaz, yeni alınan verilere karşı başka bir işleme çalıştırma başlattığı anlamına gelir. Etkine, akış verilerinin zaman tabanlı toplu işlenmeleri için bir aralıkta çalışacak şekilde yapılandırabilirsiniz.

Sonuç tablolarında yer alan veriler, sorgunun en son işlenmesinden bu yana yalnızca yeni olan verileri içerebilir *(ek modu),* veya tablo her yeni veri olduğunda yenilenebilir, böylece tablo akış sorgusu başladığından beri tüm çıktı verilerini içerir *(tam mod).*

### <a name="append-mode"></a>Ek modu

Ek modunda, sonuç tablosunda yalnızca son sorgu çalıştırılmasından sonra sonuç tablosuna eklenen satırlar, sonuç tablosunda bulunur ve dış depolama alanına yazılır. Örneğin, en basit sorgu, giriş tablosundaki tüm verileri değiştirilmeden sonuç tablosuna kopyalar. Tetikleyici aralığı her zaman yeni veriler işlenir ve bu yeni verileri temsil eden satırlar sonuç tablosunda görünür.

Termostat gibi sıcaklık sensörlerinden telemetri işlediğiniz bir senaryo düşünün. 95 derece sıcaklık okuması ile cihaz 1 için saat 00:01'de işlenen ilk tetikleyiciyi varsayalım. Sorgunun ilk tetikleyicisinde, sonuçlar tablosunda yalnızca saat 00:01 satırı görüntülenir. Saat 00:02'de başka bir olay geldiğinde, tek yeni satır saat0:02'li satırdır ve böylece sonuçlar tablosu yalnızca bir satır içerir.

![Yapılandırılmış Akış Ek Modu](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-append-mode.png)

Ek modu kullanırken, sorgunuz projeksiyonlar (önemsediği sütunları seçmek), filtreleme (yalnızca belirli koşullarla eşleşen satırları seçmek) veya birleştirme (verileri statik bir arama tablosundaki verilerle artırma) uygular. Ek modu, yalnızca ilgili yeni verilerin dış depolamaya işaret etmesini kolaylaştırır.

### <a name="complete-mode"></a>Tam mod

Tam modu kullanarak bu kez, aynı senaryoyu düşünün. Tam modda, tüm çıkış tablosu her tetikleyicide yenilenir, bu nedenle tablo yalnızca en son tetikleyici çalıştırmadan değil, tüm çalıştırmalardan gelen verileri içerir. Giriş tablosundan sonuç tablosuna değiştirilmemiş verileri kopyalamak için tam modu kullanabilirsiniz. Tetiklenen her çalıştırmada, yeni sonuç satırları önceki tüm satırlarla birlikte görüntülenir. Çıktı sonuçları tablosu, sorgu başladığından beri toplanan tüm verileri depolamaya başlar ve sonunda bellek biter. Tam mod, gelen verileri bir şekilde özetleyen toplu sorgularla kullanılmak üzere tasarlanmıştır, bu nedenle her tetikleyicide sonuç tablosu yeni bir özetle güncelleştirilir.

Şimdiye kadar zaten işlenmiş beş saniye değerinde veri olduğunu varsayalım, ve altıncı saniye için veri işleme zamanı. Giriş tablosunda saat 00:01 ve saat 00:03 olayları vardır. Bu örnek sorgunun amacı, her beş saniyede bir aygıtın ortalama sıcaklığını vermektir. Bu sorgunun uygulanması, her 5 saniyelik pencerede düşen tüm değerleri alan, sıcaklığı nortalamaya çıkaran ve bu aralıktaki ortalama sıcaklık için bir satır üreten bir agrega uygular. İlk 5 saniyelik pencerenin sonunda iki tupül vardır: (00:01, 1, 95) ve (00:03, 1, 98). Yani pencere için 00:00-00:05 toplama 96,5 derece ortalama sıcaklık ile bir tuple üretir. Sonraki 5 saniyelik pencerede, saat 00:06'da sadece bir veri noktası var, yani ortaya çıkan ortalama sıcaklık 98 derece. Saat 00:10'da, tam modu kullanarak, sonuç tablosunda her iki pencere için de satırlar vardır 00:00-00:05 ve 00:05-00:10 sorgu, sadece yeni satırları değil, tüm toplu satırları çıkar. Bu nedenle, yeni pencereler eklendikçe sonuç tablosu büyümeye devam ediyor.

![Yapılandırılmış Akış Tam Modu](./media/apache-spark-structured-streaming-overview/hdinsight-spark-structured-streaming-complete-mode.png)

Tam modu kullanan tüm sorgular tablonun sınırsız büyümesine neden olmaz.  Önceki örnekte, sıcaklığın zaman penceresine göre ortalamasını almak yerine, bunun yerine aygıt kimliğiyle ortalama sını göz önünde bulundurun. Sonuç tablosu, bu aygıttan alınan tüm veri noktaları arasında aygıtın ortalama sıcaklığına sahip sabit sayıda satır (aygıt başına bir) içerir. Yeni sıcaklıklar alındıkça, sonuçlar tablosu güncelleştirilir, böylece tablodaki ortalamalar her zaman geçerlidir.

## <a name="components-of-a-spark-structured-streaming-application"></a>Kıvılcım Yapılandırılmış Akış uygulamasının bileşenleri

Basit bir örnek sorgu, sıcaklık okumalarını saat süren pencerelere göre özetleyebilir. Bu durumda, veriler Azure Depolama'daki JSON dosyalarında depolanır (HDInsight kümesi için varsayılan depolama alanı olarak eklenir):

    {"time":1469501107,"temp":"95"}
    {"time":1469501147,"temp":"95"}
    {"time":1469501202,"temp":"95"}
    {"time":1469501219,"temp":"95"}
    {"time":1469501225,"temp":"95"}

Bu JSON dosyaları HDInsight `temps` kümesinin kapsayıcısının altındaki alt klasörde depolanır.

### <a name="define-the-input-source"></a>Giriş kaynağını tanımlama

Önce, verilerin kaynağını ve bu kaynağın gerektirdiği ayarları açıklayan bir DataFrame yapılandırın. Bu örnek, Azure Depolama'daki JSON dosyalarından yararlanır ve okuma zamanında bunlara bir şema uygular.

    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._

    //Cluster-local path to the folder containing the JSON files
    val inputPath = "/temps/" 

    //Define the schema of the JSON files as having the "time" of type TimeStamp and the "temp" field of type String
    val jsonSchema = new StructType().add("time", TimestampType).add("temp", StringType)

    //Create a Streaming DataFrame by calling readStream and configuring it with the schema and path
    val streamingInputDF = spark.readStream.schema(jsonSchema).json(inputPath) 

#### <a name="apply-the-query"></a>Sorguyu uygulama

Ardından, Akış Veri Çerçevesi'ne karşı istenen işlemleri içeren bir sorgu uygulayın. Bu durumda, bir toplama tüm satırları 1 saatlik pencerelere gruplanır ve ardından bu 1 saatlik penceredeki minimum, ortalama ve maksimum sıcaklıkları hesaplar.

    val streamingAggDF = streamingInputDF.groupBy(window($"time", "1 hour")).agg(min($"temp"), avg($"temp"), max($"temp"))

### <a name="define-the-output-sink"></a>Çıkış lavabosu tanımlayın

Ardından, her tetikleyici aralıkiçinde sonuç tablosuna eklenen satırların hedefini tanımlayın. Bu örnek, tüm satırları daha sonra SparkSQL ile sorgulayabileceğiniz bir bellek içi tabloya `temps` çıkar. Tam çıkış modu, tüm pencereler için tüm satırların her seferinde çıktı olmasını sağlar.

    val streamingOutDF = streamingAggDF.writeStream.format("memory").queryName("temps").outputMode("complete") 

### <a name="start-the-query"></a>Sorguyu başlat

Akış sorgusunu başlatın ve sonlandırma sinyali alınana kadar çalıştırın.

    val query = streamingOutDF.start()  

### <a name="view-the-results"></a>Sonuçları görüntüleme

Sorgu çalışırken, aynı SparkSession'da, sorgu sonuçlarının depolandığı tabloya `temps` karşı bir SparkSQL sorgusu çalıştırabilirsiniz.

    select * from temps

Bu sorgu aşağıdakilere benzer sonuçlar verir:

| pencere |  min(temp) | avg(temp) | max(temp) |
| --- | --- | --- | --- |
|{u'start': u'2016-07-26T02:00:00.000Z', u'end'... |    95 |    95.231579 | 99 |
|{u'start': u'2016-07-26T03:00:00.000Z', u'end'...  |95 |   96.023048 | 99 |
|{u'start': u'2016-07-26T04:00:00.000Z', u'end'...  |95 |   96.797133 | 99 |
|{u'start': u'2016-07-26T05:00:00.000Z', u'end'...  |95 |   96.984639 | 99 |
|{u'start': u'2016-07-26T06:00:00.000Z', u'end'...  |95 |   97.014749 | 99 |
|{u'start': u'2016-07-26T07:00:00.000Z', u'end'...  |95 |   96.980971 | 99 |
|{u'start': u'2016-07-26T08:00:00.000Z', u'end'...  |95 |   96.965997 | 99 |  

Spark Structured Stream API'si ile ilgili ayrıntılar ve desteklediği giriş veri kaynakları, işlemler ve çıktı lavaboları için [Apache Spark Structured Streaming Programlama Kılavuzu'na](https://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html)bakın.

## <a name="checkpointing-and-write-ahead-logs"></a>Denetim noktası ve yazma günlükleri

Yapılandırılmış Akış, esneklik ve hata toleransı sağlamak için, akış işleminin düğüm hatalarına rağmen kesintisiz olarak devam etmesini sağlamak için *denetim noktasına* dayanır. HDInsight'ta Spark, Azure Depolama veya Veri Gölü Depolama sı dahil olmak üzere dayanıklı depolama için denetim noktaları oluşturur. Bu denetim noktaları, akış sorgusu yla ilgili ilerleme bilgilerini depolar. Buna ek olarak, Yapılandırılmış Akış bir *yazma-ahead günlük* (WAL) kullanır. WAL alınan ancak henüz bir sorgu tarafından işlenmemiş sindirilen verileri yakalar. Bir hata oluşursa ve işleme WAL yeniden başlatılırsa, kaynaktan alınan olaylar kaybolmaz.

## <a name="deploying-spark-streaming-applications"></a>Kıvılcım Akış uygulamalarını dağıtma

Genellikle bir JAR dosyasına yerel olarak bir Kıvılcım Akış uygulaması oluşturur sunuz ve ardından JAR dosyasını HDInsight kümenize bağlı varsayılan depolama alanına kopyalayarak HDInsight'ta Spark'a dağıtirsınız. Bir POST işlemi kullanarak kümenizden edinebileceğiniz [Apache Livy](https://livy.incubator.apache.org/) REST API'leri ile başvurunuzu başlatabilirsiniz. POST'un gövdesi, JAR'ınıza giden yolu, ana yöntemi akış uygulamasını tanımlayan ve çalıştıran sınıfın adını ve isteğe bağlı olarak işin kaynak gereksinimlerini (uygulayıcıların, bellek ve çekirdek sayısı gibi) sağlayan bir JSON belgesi içerir. ve uygulama kodunuzu gerektiren yapılandırma ayarları.

![Bir Kıvılcım Akış uygulaması dağıtma](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Tüm uygulamaların durumu, LIVY bitiş noktasına karşı get isteğiyle de kontrol edilebilir. Son olarak, LIVY bitiş noktasına karşı bir DELETE isteği vererek çalışan bir uygulamayı sonlandırabilirsiniz. LIVY API hakkında ayrıntılı bilgi için, [Apache LIVY ile Uzak işleri](apache-spark-livy-rest-interface.md) görün

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta Bir Apache Spark kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Apache Spark Yapılandırılmış Akış Programlama Kılavuzu](https://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html)
* [Apache LIVY ile Apache Spark işlerini uzaktan başlatın](apache-spark-livy-rest-interface.md)
