---
title: Performans için Kıvılcım işlerini optimize edin - Azure HDInsight
description: Azure HDInsight'ta Apache Spark kümelerinin en iyi performansı için ortak stratejiler gösterin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/12/2020
ms.openlocfilehash: 3d8f4a28961be7e0ece517e00026d9711d8f67e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198880"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>HDInsight'ta Apache Spark işlerini optimize edin

[Apache Spark](https://spark.apache.org/) küme yapılandırmasını belirli iş yükünüz için nasıl optimize edeceğinizi öğrenin.  En yaygın zorluk, yanlış yapılandırmalar (özellikle yanlış boyutlu yürütücüler), uzun süren işlemler ve Kartezyen işlemleriyle sonuçlanan görevler nedeniyle bellek basıncıdır. Uygun önbelleğe alma ile işleri hızlandırabilir ve [veri eğriltme](#optimize-joins-and-shuffles)sine izin verebilirsiniz. En iyi performans için, uzun süredir çalışan ve kaynak tüketen Spark iş yürütmelerini izleyin ve gözden geçirin. HDInsight'ta Apache Spark'a başlama hakkında daha fazla bilgi için [Azure portalını kullanarak Apache Spark kümesi oluştur'a](apache-spark-jupyter-spark-sql-use-portal.md)bakın.

Aşağıdaki bölümlerde ortak Kıvılcım iş optimizasyonları ve öneriler açıklayınız.

## <a name="choose-the-data-abstraction"></a>Veri soyutlamasını seçin

Önceki Spark sürümleri, özet veriler, Spark 1.3 ve 1.6'yı sırasıyla DataFrame'ler ve DataSets'leri tanıtmak için RDD'leri kullanır. Aşağıdaki göreceli yararları göz önünde bulundurun:

* **Veri çerçeveleri**
    * Çoğu durumda en iyi seçim.
    * Catalyst aracılığıyla sorgu optimizasyonu sağlar.
    * Tüm aşama kod oluşturma.
    * Doğrudan bellek erişimi.
    * Düşük çöp toplama (GC) yükü.
    * Derleme zamanı denetimleri veya etki alanı nesnesi programlaması olmadığı için, DataSets kadar geliştirici dostu değildir.
* **Dataset**
    * Performans etkisinin kabul edilebilir olduğu karmaşık ETL boru hatlarında iyi.
    * Performans etkisinin önemli olabileceği toplamalarda iyi değildir.
    * Catalyst aracılığıyla sorgu optimizasyonu sağlar.
    * Etki alanı nesneprogramlama ve derleme-zaman denetimleri sağlayarak geliştirici dostu.
    * Serileştirme/deserialization yükü ekler.
    * Yüksek GC havai.
    * Tüm aşama kod oluşturmayı bozar.
* **RDD'ler**
    * Yeni bir özel RDD oluşturmanız gerekmediği sürece RDD kullanmanız gerekmez.
    * Catalyst üzerinden sorgu optimizasyonu yok.
    * Tüm aşama kod oluşturma yok.
    * Yüksek GC havai.
    * Spark 1.x eski API'leri kullanmalıdır.

## <a name="use-optimal-data-format"></a>En uygun veri biçimini kullanma

Kıvılcım csv, json, xml, parke, ork ve avro gibi birçok biçimi destekler. Spark dış veri kaynakları ile çok daha fazla biçimleri desteklemek için genişletilebilir - daha fazla bilgi için, [Apache Spark paketleri](https://spark-packages.org)bakın.

Performans için en iyi format, Spark 2.x'te varsayılan olan *hızlı sıkıştırmalı*parkedir. Parke verileri sütun biçiminde depolar ve Spark'ta son derece optimize edilmiştir.

## <a name="select-default-storage"></a>Varsayılan depolama alanını seçin

Yeni bir Kıvılcım kümesi oluşturduğunuzda, kümenizin varsayılan depolama alanı olarak Azure Blob Depolama veya Azure Veri Gölü Depolama'yı seçebilirsiniz. Her iki seçenek de geçici kümeler için uzun süreli depolama avantajı sağlar, böylece kümenizi sildiğinizde verileriniz otomatik olarak silinmez. Geçici bir küme oluşturabilir ve verilerinize erişebilirsiniz.

| Mağaza Türü | Dosya Sistemi | Hız | Geçi -ci | Kullanım Örnekleri |
| --- | --- | --- | --- | --- |
| Azure Blob Depolama | **wasb:**//url/ | **Standart** | Evet | Geçici küme |
| Azure Blob Depolama (güvenli) | **isbs:**//url/ | **Standart** | Evet | Geçici küme |
| Azure Data Lake Storage 2. Nesil| **abfs:**//url/ | **Hızlı** | Evet | Geçici küme |
| Azure Veri Gölü Depolama Gen 1| **adl:**//url/ | **Hızlı** | Evet | Geçici küme |
| Yerel HDFS | **hdfs:**//url/ | **Hızlı** | Hayır | İnteraktif 7/24 küme |

HDInsight kümeleri için kullanılabilen depolama seçeneklerinin tam açıklaması için, [kullanılmak üzere depolama seçeneklerini Azure HDInsight kümeleriyle karşılaştır'a](../hdinsight-hadoop-compare-storage-options.md)bakın.

## <a name="use-the-cache"></a>Önbelleği kullanma

Kıvılcım gibi `.persist()`farklı yöntemlerle kullanılabilir kendi yerli önbelleğe alma `.cache()`mekanizmaları `CACHE TABLE`sağlar , ve . Bu yerel önbelleğe alma, küçük veri kümelerinin yanı sıra ara sonuçları önbelleğe almanız gereken ETL ardışık hatlarında da etkilidir. Ancak, önbelleğe alınmış bir tablo bölümleme verilerini tutmadığından, Spark yerel önbelleğe alma şu anda bölümleme ile iyi çalışmıyor. Daha genel ve güvenilir önbelleğe alma tekniği *depolama katmanı önbelleğe almadır.*

* Yerel Kıvılcım önbelleğe alma (önerilmez)
    * Küçük veri kümeleri için iyi.
    * Gelecekteki Spark sürümlerinde değişebilir bölümleme ile çalışmıyor.

* Depolama düzeyi önbelleğe alma (önerilir)
    * [IO Önbellek](apache-spark-improve-performance-iocache.md) özelliği kullanılarak HDInsight'ta uygulanabilir.
    * Bellek ve SSD önbelleğe alma kullanır.

* Yerel HDFS (önerilir)
    * `hdfs://mycluster`Yolu.
    * SSD önbelleğe alma kullanır.
    * Önbelleğe alınan veriler, önbelleğe alma alanı yeniden oluşturma gerektiren kümeyi sildiğinizde kaybolur.

## <a name="use-memory-efficiently"></a>Belleği verimli kullanma

Spark, verileri belleğe yerleştirerek çalışır, bu nedenle bellek kaynaklarını yönetmek, Spark işlerinin yürütülmesini en iyi duruma getirilmesinin önemli bir yönüdür.  Kümenizin belleği verimli bir şekilde kullanmak için uygulayabileceğiniz çeşitli teknikler vardır.

* Bölümleme stratejinizde daha küçük veri bölümlerini ve veri boyutu, türleri ve dağıtımı için hesap yapmayı tercih edin.
* Varsayılan Java serileştirmeyerine daha yeni, daha verimli [Kryo veri serileştirmedüşünün.](https://github.com/EsotericSoftware/kryo)
* Toplu olarak ayrılır `spark-submit` gibi, IPLIK kullanmayı tercih edin.
* Spark yapılandırma ayarlarını izleyin ve ayarlayın.

Başvurunuz için, Spark bellek yapısı ve bazı önemli yürütme bellek parametreleri sonraki resimde gösterilir.

### <a name="spark-memory-considerations"></a>Bellek hususlarını kıvılcımla

[Apache Hadoop İPLik](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)kullanıyorsanız, o zaman İPLik her Kıvılcım düğümündeki tüm kaplar tarafından kullanılan maksimum bellek toplamını kontrol eder.  Aşağıdaki diyagram, anahtar nesneleri ve ilişkilerini gösterir.

![İplik Kıvılcım Bellek Yönetimi](./media/apache-spark-perf/apache-yarn-spark-memory.png)

'Bellek dışı' iletileri adreslemek için şunları deneyin:

* DAG Yönetim Shuffles gözden geçirin. Harita tarafı indirgeme, ön bölümleme (veya kovaize) kaynak verileri ile azaltın, tek shuffles maksimize ve gönderilen veri miktarını azaltmak.
* Sabit `ReduceByKey` bellek sınırı `GroupByKey`ile tercih , hangi toplama, pencereleme ve diğer işlevleri sağlar ama ann sınırsız bellek sınırı vardır.
* Tercih `TreeReduce`, hangi uygulayıcıları veya bölümleri üzerinde daha `Reduce`fazla iş yapar, için , sürücü üzerinde tüm çalışır.
* Alt düzey RDD nesneleri yerine DataFrame'lerden yararlanın.
* "Üst N", çeşitli toplamalar veya pencereleme işlemleri gibi eylemleri kapsülleyen ComplexTypes oluşturun.

Ek sorun giderme adımları için Azure [HDInsight'ta Apache Spark için OutOfMemoryError özel durumları'na](apache-spark-troubleshoot-outofmemory.md)bakın.

## <a name="optimize-data-serialization"></a>Veri serileştirmeyi optimize edin

Kıvılcım işleri dağıtılır, bu nedenle en iyi performans için uygun veri serileştirmesi önemlidir.  Spark için iki serileştirme seçeneği vardır:

* Java serileştirme varsayılandır.
* Kryo serileştirme yeni bir biçimdir ve Java daha hızlı ve daha kompakt serileştirme neden olabilir.  Kryo, programınızdaki sınıfları kaydetmenizi gerektirir ve henüz tüm Serializable türlerini desteklemez.

## <a name="use-bucketing"></a>Kovalama yı kullanma

Kovalama veri bölümleme benzer, ancak her kova yerine sadece bir sütun değerleri kümesi tutabilir. Kovalama, ürün tanımlayıcıları gibi büyük (milyonlarca veya daha fazla) değer sayılarında bölümleme için iyi çalışır. Bir kova, satırın kova anahtarının karma ile belirlenir. Kovalanan tablolar, nasıl kovalandıklarına ve sıralandıklarına ilişkin meta verileri depoladıkları için benzersiz optimizasyonlar sunar.

Bazı gelişmiş kovalama özellikleri şunlardır:

* Meta-bilgileri kovalama temel alınarak optimizasyon sorgusu.
* Optimize edilmiş toplamalar.
* Optimize edilmiş birleştirmeler.

Bölümleme ve kovalamayı aynı anda kullanabilirsiniz.

## <a name="optimize-joins-and-shuffles"></a>Birleştirmeleri ve karıştırmaları optimize edin

Bir Join veya Shuffle'da yavaş işleriniz varsa, bunun nedeni büyük olasılıkla iş verilerinizdeki asimetri olan *veri çarpıklıktır.* Örneğin, bir harita işi 20 saniye sürebilir, ancak verilerin birleştiği veya karıştırıldığı bir işi çalıştırmak saatler sürer. Veri eğriliğini düzeltmek için, anahtarın tamamını tuzlamalı veya yalnızca bazı anahtar alt kümesi için *yalıtılmış bir tuz* kullanmalısınız. İzole edilmiş bir tuz kullanıyorsanız, harita birleştirmelerinde tuzlu anahtaralt kümenizi izole etmek için daha fazla filtre uygulamanız gerekir. Başka bir seçenek, önce kovalarda bir kova sütunu ve önceden agrega sunmaktır.

Yavaş birleşimlere neden olan bir diğer faktör de birleştirme türü olabilir. Varsayılan olarak, Spark `SortMerge` birleştirme türünü kullanır. Bu tür birleştirme, büyük veri kümeleri için en uygun olandır, ancak birleştirmeden önce verilerin sol ve sağ kenarlarını sıralaması gerektiğinden, başka bir şekilde hesaplama açısından pahalıdır.

Birleştirme `Broadcast` en küçük veri kümeleri için uygundur veya birleşimin bir tarafının diğer taraftan çok daha küçük olduğu durumlarda. Bu tür birleştirme, tüm uygulayıcılara bir tarafı yayınlar ve bu nedenle genel olarak yayınlar için daha fazla bellek gerektirir.

Yapılandırmanızdaki birleştirme türünü ayarlayarak `spark.sql.autoBroadcastJoinThreshold`değiştirebilir veya DataFrame API'lerini kullanarak bir`dataframe.join(broadcast(df2))`birleştirme ipucu ayarlayabilirsiniz .

```scala
// Option 1
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", 1*1024*1024*1024)

// Option 2
val df1 = spark.table("FactTableA")
val df2 = spark.table("dimMP")
df1.join(broadcast(df2), Seq("PK")).
    createOrReplaceTempView("V_JOIN")

sql("SELECT col1, col2 FROM V_JOIN")
```

Kovalanmış tablolar kullanıyorsanız, üçüncü bir birleştirme türü, `Merge` birleştirme var. Doğru önceden bölümlenmiş ve önceden sıralanmış bir veri kümesi, birbirme `SortMerge` pahalı sıralama aşamasını atlar.

Birleştirme sırası, özellikle daha karmaşık sorgularda önemlidir. En seçici birleştirmelerle başlayın. Ayrıca, move, mümkün olduğunda toplamalardan sonra satır sayısını artıran birleştirmeleri sağlar.

Kartezyen birleştirmeleri için paralelliği yönetmek için iç içe yapılar, pencereleme ekleyebilir ve Belki de Kıvılcım İşinizde bir veya daha fazla adımı atlayabilirsiniz.

## <a name="customize-cluster-configuration"></a>Küme yapılandırması özelleştirme

Spark kümesi iş yükünüze bağlı olarak, varsayılan olmayan bir Spark yapılandırması daha iyi leştirilmiş Spark iş yürütmeneden olacağını belirleyebilirsiniz.  Varsayılan olmayan küme yapılandırmalarını doğrulamak için örnek iş yükleriyle kıyaslama testi gerçekleştirin.

Ayarlayabileceğiniz bazı yaygın parametreler şunlardır:

* `--num-executors`uygun sayıda uygulayıcı ayarlar.
* `--executor-cores`her uygulayıcı için çekirdek sayısını ayarlar. Diğer işlemler kullanılabilir belleğin bazılarını tükettiği için genellikle orta ölçekli yürütücüler olmalıdır.
* `--executor-memory`HER uygulayıcı için bellek boyutunu ayarlar, bu da İPLik'teki yığın boyutunu denetler. Yürütme yükü için biraz bellek bırakmalısın.

### <a name="select-the-correct-executor-size"></a>Doğru uygulayıcı boyutunu seçin

Uygulayıcı yapılandırmanıza karar verirken, Java çöp toplama (GC) ek yükü göz önünde bulundurun.

* Uygulayıcı boyutunu azaltmak için faktörler:
    1. GC havai <%10'u korumak için yığın boyutunu 32 GB'ın altına indirin.
    2. GC genel <%10'u tutmak için çekirdek sayısını azaltın.

* Uygulayıcı boyutunu artırmak için faktörler:
    1. Uygulayıcılar arasındaki iletişim yükü azaltın.
    2. Büyük kümelerde (>100 uygulayıcılar) uygulayıcılar (N2) arasındaki açık bağlantı sayısını azaltın.
    3. Bellek yoğun görevler için karşılamak için yığın boyutunu artırın.
    4. İsteğe bağlı: Çalıştırıcı başına bellek ek yükü azaltın.
    5. İsteğe bağlı: CPU'yu aşırı abone ederek kullanımı ve eşzamanlılığı artırın.

Uygulayıcı boyutu seçerken genel bir başparmak kuralı olarak:

1. Uygulayıcı başına 30 GB ile başlayın ve kullanılabilir makine çekirdeklerini dağıtın.
2. Büyük kümeler için uygulayıcı çekirdek sayısını artırın (> 100 uygulayıcısı).
3. Hem deneme çalıştırmalarını hem de GC yükü gibi önceki etkenlere göre boyutu değiştirin.

Eşzamanlı sorguları çalıştırırken, aşağıdakileri göz önünde bulundurun:

1. Çalıştırıcı başına 30 GB ve tüm makine çekirdekleri ile başlayın.
2. CPU'yu (yaklaşık %30 gecikme süresi iyileştirme) abone olarak birden çok paralel Spark uygulaması oluşturun.
3. Sorguları paralel uygulamalar arasında dağıtın.
4. Hem deneme çalıştırmalarını hem de GC yükü gibi önceki etkenlere göre boyutu değiştirin.

Ambari'yi kullanarak uygulayıcıları yapılandırmak için daha fazla bilgi için [Apache Spark ayarlarına bakın - Spark uygulayıcıları](apache-spark-settings.md#configuring-spark-executors).

Zaman çizelgesi görünümüne, SQL grafiğine, iş istatistiklerine vb. bakarak, aykırılar veya diğer performans sorunları için sorgu performansınızı izleyin. İplik ve Spark Geçmişi sunucusunu kullanarak Spark işlerinin hata ayıklanması hakkında bilgi için Azure [HDInsight'ta çalışan Hata Ayıklama Apache Spark işleri'ne](apache-spark-job-debugging.md)bakın. İPLik Zaman Çizelgesi Sunucusu'nun kullanımı yla ilgili ipuçları için [Access Apache Hadoop IPN uygulama günlüklerine](../hdinsight-hadoop-access-yarn-app-logs-linux.md)bakın.

Bazen yürütücülerden biri veya birkaçı diğerlerinden daha yavaş olur ve görevlerin yürütülmesi çok daha uzun sürer. Bu genellikle büyük kümelerde (> 30 düğüm) olur. Bu durumda, zamanlayıcının yavaş görevleri telafi edebilsin diye çalışmayı daha fazla sayıda görevsayısına bölün. Örneğin, uygulamadaki yürütücü çekirdek sayısının en az iki katı sayıda görev vardır. Ayrıca, `conf: spark.speculation = true`'' ile görevlerin spekülatif yürütülmesini de etkinleştirebilirsiniz.

## <a name="optimize-job-execution"></a>İş yürütmeyi optimize edin

* Gerektiğinde önbelleğe para verin, örneğin verileri iki kez kullanıyorsanız, önbelleğe verin.
* Değişkenleri tüm uygulayıcılara yayın. Değişkenler yalnızca bir kez serihale getirilir ve bu da daha hızlı aramalarla sonuçlanır.
* Sürücüdeki iş parçacığı havuzunu kullanın ve bu da birçok görev için daha hızlı çalışma yla sonuçlanır.

Performans sorunları için çalışan işlerinizi düzenli olarak izleyin. Belirli konular hakkında daha fazla bilgi edinmeniz gerekiyorsa, aşağıdaki performans profil oluşturma araçlarından birini göz önünde bulundurun:

* [Intel PAL Tool](https://github.com/intel-hadoop/PAT) CPU, depolama ve ağ bant genişliği kullanımını izler.
* [Oracle Java 8 Görev Kontrol](https://www.oracle.com/technetwork/java/javaseproducts/mission-control/java-mission-control-1998576.html) profilleri Kıvılcım ve uygulayıcı kodu.

Spark 2.x sorgu performansının anahtarı, tüm aşama kod oluşumuna bağlı olan Tungsten motorudur. Bazı durumlarda, tüm aşama kod oluşturma devre dışı olabilir. Örneğin, toplama ifadesinde değişmez bir tür`string`( kullanıyorsanız, `SortAggregate` `HashAggregate`' ın yerine . Örneğin, daha iyi performans için aşağıdakileri deneyin ve ardından kod oluşturmayı yeniden etkinleştirin:

```sql
MAX(AMOUNT) -> MAX(cast(AMOUNT as DOUBLE))
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight üzerinde çalışan Apache Spark işlerinin hatasını ayıklama](apache-spark-job-debugging.md)
* [HDInsight'ta bir Apache Spark kümesinin kaynaklarını yönetme](apache-spark-resource-manager.md)
* [Bir Apache Spark kümesine uzak işler göndermek için Apache Spark REST API'sini kullanın](apache-spark-livy-rest-interface.md)
* [Apache Kıvılcımı](https://spark.apache.org/docs/latest/tuning.html)
* [Nasıl Aslında Tune Your Apache Spark İşler Böylece Onlar İş](https://www.slideshare.net/ilganeli/how-to-actually-tune-your-spark-jobs-so-they-work)
* [Kryo Serileştirme](https://github.com/EsotericSoftware/kryo)
