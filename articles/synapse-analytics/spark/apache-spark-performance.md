---
title: Azure Synapse Analytics'te performans için Kıvılcım işlerini optimize edin
description: Bu makale, Azure Synapse Analytics'te Apache Spark'a ve farklı kavramlara giriş sağlar.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 6ffe7f3d9faf82c892975e9ffa03b383d3610c36
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424625"
---
# <a name="optimize-apache-spark-jobs-preview-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te Apache Spark işlerini (önizleme) optimize edin

[Apache Spark](https://spark.apache.org/) küme yapılandırmasını belirli iş yükünüz için nasıl optimize edeceğinizi öğrenin.  En yaygın zorluk, yanlış yapılandırmalar (özellikle yanlış boyutlu yürütücüler), uzun süren işlemler ve Kartezyen işlemleriyle sonuçlanan görevler nedeniyle bellek basıncıdır. Uygun önbelleğe alma ile işleri hızlandırabilir ve [veri eğriltme](#optimize-joins-and-shuffles)sine izin verebilirsiniz. En iyi performans için, uzun süredir çalışan ve kaynak tüketen Spark iş yürütmelerini izleyin ve gözden geçirin.

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

Performans için en iyi format, Spark 2.x'te varsayılan olan *hızlı sıkıştırmalı*parkedir. Parke verileri sütun biçiminde depolar ve Spark'ta son derece optimize edilmiştir. Buna ek *olarak, hızlı sıkıştırma* gzip sıkıştırma demek daha büyük dosyalarneden olabilir. Bu dosyaların bölünebilen yapısı nedeniyle daha hızlı dekompresyon olur]

## <a name="use-the-cache"></a>Önbelleği kullanma

Kıvılcım gibi `.persist()`farklı yöntemlerle kullanılabilir kendi yerli önbelleğe alma `.cache()`mekanizmaları `CACHE TABLE`sağlar , ve . Bu yerel önbelleğe alma, küçük veri kümelerinin yanı sıra ara sonuçları önbelleğe almanız gereken ETL ardışık hatlarında da etkilidir. Ancak, önbelleğe alınmış bir tablo bölümleme verilerini tutmadığından, Spark yerel önbelleğe alma şu anda bölümleme ile iyi çalışmıyor.

## <a name="use-memory-efficiently"></a>Belleği verimli kullanma

Spark, verileri belleğe yerleştirerek çalışır, bu nedenle bellek kaynaklarını yönetmek, Spark işlerinin yürütülmesini en iyi duruma getirilmesinin önemli bir yönüdür.  Kümenizin belleği verimli bir şekilde kullanmak için uygulayabileceğiniz çeşitli teknikler vardır.

* Bölümleme stratejinizde daha küçük veri bölümlerini ve veri boyutu, türleri ve dağıtımı için hesap yapmayı tercih edin.
* Varsayılan Java serileştirmeyerine daha yeni, daha verimli [Kryo veri serileştirmedüşünün.](https://github.com/EsotericSoftware/kryo)
* Spark yapılandırma ayarlarını izleyin ve ayarlayın.

Başvurunuz için, Spark bellek yapısı ve bazı önemli yürütme bellek parametreleri sonraki resimde gösterilir.

### <a name="spark-memory-considerations"></a>Bellek hususlarını kıvılcımla

Azure Sinaps'taki Apache Spark, İplik [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)kullanır, YARN her Kıvılcım düğümündeki tüm kapsayıcılar tarafından kullanılan maksimum bellek toplamını kontrol eder.  Aşağıdaki diyagram, anahtar nesneleri ve ilişkilerini gösterir.

![İplik Kıvılcım Bellek Yönetimi](./media/apache-spark-perf/apache-yarn-spark-memory.png)

'Bellek dışı' iletileri adreslemek için şunları deneyin:

* DAG Yönetim Shuffles gözden geçirin. Kaynak verilerini harita tarafında azaltarak, ön bölümle (veya kovaya indirerek) azaltın, tek shuffle'ları en üst düzeye çıkarın ve gönderilen veri miktarını azaltın.
* Sabit `ReduceByKey` bellek sınırı `GroupByKey`ile tercih , hangi toplama, pencereleme ve diğer işlevleri sağlar ama ann sınırsız bellek sınırı vardır.
* Tercih `TreeReduce`, hangi uygulayıcıları veya bölümleri üzerinde daha `Reduce`fazla iş yapar, için , sürücü üzerinde tüm çalışır.
* Alt düzey RDD nesneleri yerine DataFrame'lerden yararlanın.
* "Üst N", çeşitli toplamalar veya pencereleme işlemleri gibi eylemleri kapsülleyen ComplexTypes oluşturun.

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

### <a name="select-the-correct-executor-size"></a>Doğru uygulayıcı boyutunu seçin

Uygulayıcı yapılandırmanıza karar verirken, Java çöp toplama (GC) ek yükü göz önünde bulundurun.

* Uygulayıcı boyutunu azaltmak için faktörler:
  * GC havai <%10'u korumak için yığın boyutunu 32 GB'ın altına indirin.
  * GC genel <%10'u tutmak için çekirdek sayısını azaltın.

* Uygulayıcı boyutunu artırmak için faktörler:
  * Uygulayıcılar arasındaki iletişim yükü azaltın.
  * Büyük kümelerde (>100 uygulayıcılar) uygulayıcılar (N2) arasındaki açık bağlantı sayısını azaltın.
  * Bellek yoğun görevler için karşılamak için yığın boyutunu artırın.
  * İsteğe bağlı: Çalıştırıcı başına bellek ek yükü azaltın.
  * İsteğe bağlı: CPU'yu aşırı abone ederek kullanımı ve eşzamanlılığı artırın.

Uygulayıcı boyutu seçerken genel bir başparmak kuralı olarak:

* Uygulayıcı başına 30 GB ile başlayın ve kullanılabilir makine çekirdeklerini dağıtın.
* Büyük kümeler için uygulayıcı çekirdek sayısını artırın (> 100 uygulayıcısı).
* Hem deneme çalıştırmalarını hem de GC yükü gibi önceki etkenlere göre boyutu değiştirin.

Eşzamanlı sorguları çalıştırırken, aşağıdakileri göz önünde bulundurun:

* Çalıştırıcı başına 30 GB ve tüm makine çekirdekleri ile başlayın.
* CPU'yu (yaklaşık %30 gecikme süresi iyileştirme) abone olarak birden çok paralel Spark uygulaması oluşturun.
* Sorguları paralel uygulamalar arasında dağıtın.
* Hem deneme çalıştırmalarını hem de GC yükü gibi önceki etkenlere göre boyutu değiştirin.

Zaman çizelgesi görünümüne, SQL grafiğine, iş istatistiklerine vb. bakarak, aykırılar veya diğer performans sorunları için sorgu performansınızı izleyin. Bazen yürütücülerden biri veya birkaçı diğerlerinden daha yavaş olur ve görevlerin yürütülmesi çok daha uzun sürer. Bu genellikle büyük kümelerde (> 30 düğüm) olur. Bu durumda, zamanlayıcının yavaş görevleri telafi edebilsin diye çalışmayı daha fazla sayıda görevsayısına bölün. 

Örneğin, uygulamadaki yürütücü çekirdek sayısının en az iki katı sayıda görev vardır. Ayrıca, `conf: spark.speculation = true`'' ile görevlerin spekülatif yürütülmesini de etkinleştirebilirsiniz.

## <a name="optimize-job-execution"></a>İş yürütmeyi optimize edin

* Gerektiğinde önbelleğe para verin, örneğin verileri iki kez kullanıyorsanız, önbelleğe verin.
* Değişkenleri tüm uygulayıcılara yayın. Değişkenler yalnızca bir kez serihale getirilir ve bu da daha hızlı aramalarla sonuçlanır.
* Sürücüdeki iş parçacığı havuzunu kullanın ve bu da birçok görev için daha hızlı çalışma yla sonuçlanır.

Spark 2.x sorgu performansının anahtarı, tüm aşama kod oluşumuna bağlı olan Tungsten motorudur. Bazı durumlarda, tüm aşama kod oluşturma devre dışı olabilir. 

Örneğin, toplama ifadesinde değişmez bir tür`string`( kullanıyorsanız, `SortAggregate` `HashAggregate`' ın yerine . Örneğin, daha iyi performans için aşağıdakileri deneyin ve ardından kod oluşturmayı yeniden etkinleştirin:

```sql
MAX(AMOUNT) -> MAX(cast(AMOUNT as DOUBLE))
```

## <a name="next-steps"></a>Sonraki adımlar

- [Apache Kıvılcımı](https://spark.apache.org/docs/latest/tuning.html)
- [Nasıl Aslında Tune Your Apache Spark İşler Böylece Onlar İş](https://www.slideshare.net/ilganeli/how-to-actually-tune-your-spark-jobs-so-they-work)
- [Kryo Serileştirme](https://github.com/EsotericSoftware/kryo)
