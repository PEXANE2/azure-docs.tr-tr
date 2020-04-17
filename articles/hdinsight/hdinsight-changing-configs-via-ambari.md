---
title: Küme yapılandırmalarını optimize etmek için Apache Ambari - Azure HDInsight
description: Azure HDInsight kümelerini yapılandırmak ve optimize etmek için Apache Ambari web Kullanıcı Arabirimi'ni kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/16/2020
ms.openlocfilehash: c88882175ff256300dee486e680a9b63e9a65c99
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81532541"
---
# <a name="use-apache-ambari-to-optimize-hdinsight-cluster-configurations"></a>HDInsight kümesi yapılandırmalarını iyileştirmek için Apache Ambari'yi kullanma

HDInsight, büyük ölçekli veri işleme uygulamaları için [Apache Hadoop](./hadoop/apache-hadoop-introduction.md) kümeleri sağlar. Bu karmaşık çok düğümlü kümeleri yönetmek, izlemek ve en iyi duruma etmek zor olabilir. [Apache Ambari,](https://ambari.apache.org/) HDInsight Linux kümelerini yönetmek ve izlemek için bir web arabirimidir.  Windows kümeleri için [Ambari REST API'yi](hdinsight-hadoop-manage-ambari-rest-api.md)kullanın.

Ambari Web UI'sini kullanmaya giriş için [Apache Ambari Web UI'yi kullanarak HDInsight kümelerini yönet'e](hdinsight-hadoop-manage-ambari.md) bakın

Küme kimlik bilgilerinizle `https://CLUSTERNAME.azurehdidnsight.net` Ambari'ye giriş yapın. İlk ekranda genel bir pano görüntülenir.

![Apache Ambari kullanıcı panosu görüntülendi](./media/hdinsight-changing-configs-via-ambari/apache-ambari-dashboard.png)

Ambari web Web Web Web UI, ana bilgisayarları, hizmetleri, uyarıları, yapılandırmaları ve görünümleri yönetmek için kullanılır. Ambari bir HDInsight kümesi oluşturmak veya hizmetleri yükseltmek için kullanılamaz. Ayrıca yığınları ve sürümleri yönetemez, ana bilgisayarları devre dışı kaldıramaz veya yeniden devreye alamaz veya kümeye hizmet ekemez.

## <a name="manage-your-clusters-configuration"></a>Kümenizin yapılandırmayı yönetme

Yapılandırma ayarları belirli bir hizmeti ayarlamaya yardımcı olur. Bir hizmetin yapılandırma ayarlarını değiştirmek için **Hizmetleri Kenar** Çubuğu'ndan (solda) seçin. Ardından, hizmet ayrıntı sayfasındaki **Configs** sekmesine gidin.

![Apache Ambari Hizmetleri kenar çubuğu](./media/hdinsight-changing-configs-via-ambari/ambari-services-sidebar.png)

### <a name="modify-namenode-java-heap-size"></a>NameNode Java yığın boyutunu değiştirin

NameNode Java yığın boyutu küme üzerindeki yük gibi birçok faktöre bağlıdır. Ayrıca, dosya sayısı ve blok sayısı. Bazı iş yükleri daha fazla veya daha az bellek gerektirebilir, ancak 1 GB varsayılan boyutu çoğu kümeleri ile iyi çalışır.

NameNode Java yığın boyutunu değiştirmek için:

1. Hizmetler kenar çubuğundan **HDFS'yi** seçin ve **Configs** sekmesine gidin.

    ![Apache Ambari HDFS yapılandırması](./media/hdinsight-changing-configs-via-ambari/ambari-apache-hdfs-config.png)

1. Ayarı **NameNode Java yığın boyutunu**bulun. Belirli bir ayarı yazmak ve bulmak için **filtre** metin kutusunu da kullanabilirsiniz. Ayar adının yanındaki **kalem** simgesini seçin.

    ![Apache Ambari NameNode Java yığın boyutu](./media/hdinsight-changing-configs-via-ambari/ambari-java-heap-size.png)

1. Metin kutusuna yeni değeri yazın ve değişikliği kaydetmek için **Enter** tuşuna basın.

    ![Ambari Edit NameNode Java yığın boyutu1](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit1.png)

1. NameNode Java yığın boyutu 2 GB 1 GB olarak değiştirilir.

    ![Düzenlenen NameNode Java yığın boyutu2](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

1. Yapılandırma ekranının üst kısmındaki yeşil **Kaydet** düğmesine tıklayarak değişikliklerinizi kaydedin.

    !['Apache Ambari yapılandırmaları kaydedin'](./media/hdinsight-changing-configs-via-ambari/ambari-save-changes1.png)

## <a name="apache-hive-optimization"></a>Apache Hive optimizasyonu

Aşağıdaki bölümlerde, genel Apache Hive performansını optimize etmek için yapılandırma seçenekleri açıklayınız.

1. Hive yapılandırma parametrelerini **Hive** değiştirmek için Hizmetler kenar çubuğundan Kovan'ı seçin.
1. **Configs** sekmesine gidin.

### <a name="set-the-hive-execution-engine"></a>Kovan yürütme motorini ayarlama

Kovan iki yürütme motoru sağlar: [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) ve [Apache TEZ](https://tez.apache.org/). Tez MapReduce'den daha hızlıdır. HDInsight Linux kümeleri tez varsayılan yürütme motoru olarak var. Yürütme altyapısını değiştirmek için:

1. Hive **Configs** sekmesinde, filtre kutusuna **yürütme altyapısı** yazın.

    ![Apache Ambari Arama yürütme motoru](./media/hdinsight-changing-configs-via-ambari/ambari-search-execution.png)

1. **Optimizasyon** özelliğinin varsayılan değeri **Tez'dir.**

    ![Optimizasyon - Apache Tez motoru](./media/hdinsight-changing-configs-via-ambari/optimization-apache-tez.png)

### <a name="tune-mappers"></a>Haritaperları ayarlayın

Hadoop, tek bir dosyayı birden çok dosyaya bölmeye *(eşle)* ve elde edilen dosyaları paralel olarak işlemeye çalışır. Haritapers sayısı bölmelerin sayısına bağlıdır. Aşağıdaki iki yapılandırma parametresi Tez yürütme motoru için bölme sayısını sürücü:

* `tez.grouping.min-size`: Varsayılan değeri 16 MB (16.777.216 bayt) olan gruplanmış bölmenin boyutunda alt sınır.
* `tez.grouping.max-size`: Varsayılan değeri 1 GB olan gruplanmış bölmenin boyutundaki üst sınır (1.073.741.824 bayt).

Performans kılavuzu olarak, gecikme yi artırmak için bu parametrelerin her ikisini de düşürün, daha fazla iş elde etmek için artırın.

Örneğin, 128 MB'lık bir veri boyutu için dört mapper görevi ayarlamak için her iki parametreyi de 32 MB (33.554.432 bayt) olarak ayarlarsınız.

1. Sınır parametrelerini değiştirmek için Tez hizmetinin **Configs** sekmesine gidin. **Genel** paneli genişletin ve `tez.grouping.max-size` `tez.grouping.min-size` parametreleri ve parametreleri bulun.

1. Her iki parametreyi de **33,554,432** bayt (32 MB) olarak ayarlayın.

    ![Apache Ambari Tez gruplandırma boyutları](./media/hdinsight-changing-configs-via-ambari/apache-tez-grouping-size.png)

Bu değişiklikler sunucudaki tüm Tez işlerini etkiler. En iyi sonucu elde etmek için uygun parametre değerlerini seçin.

### <a name="tune-reducers"></a>Ayar redüktörleri

[Apache ORC](https://orc.apache.org/) ve [Snappy](https://google.github.io/snappy/) her ikisi de yüksek performans sunuyor. Ancak, Hive varsayılan olarak çok az indirgeyiciye sahip olabilir ve bu da darboğazlara neden olabilir.

Örneğin, 50 GB'lık bir giriş veri boyutuna sahip olduğunuzu varsabilirsiniz. Snappy sıkıştırma ile ORC formatında bu veri 1 GB'dır. Kovan olarak gerekli azaltıcıların sayısını tahmin eder: (mappers bayt girişi sayısı / `hive.exec.reducers.bytes.per.reducer`).

Varsayılan ayarlarda, bu örnek dört indirgeyicidir.

Parametre, `hive.exec.reducers.bytes.per.reducer` azaltıcı başına işlenen bayt sayısını belirtir. Varsayılan değer 64 MB'dır. Bu değeri aşağı alalır paralelliği artırır ve performansı artırabilir. Çok düşük atoklama da çok fazla redüktör üretebilir, potansiyel olarak performansı olumsuz etkileyen. Bu parametre, belirli veri gereksinimlerinize, sıkıştırma ayarlarına ve diğer çevresel faktörlere dayanır.

1. Parametreyi değiştirmek için Kovan **Configs** sekmesine gidin ve Ayarlar sayfasında **Azaltıcı başına Veri** parametresini bulun.

    ![Apache Ambari Veri Redüktör başına](./media/hdinsight-changing-configs-via-ambari/ambari-data-per-reducer.png)

1. Değeri 128 MB (134.217.728 bayt) olarak değiştirmek için **Edet'i** seçin ve kaydetmek için **Enter** tuşuna basın.

    ![Azaltıcı başına Ambari Veri - düzenlenmiş](./media/hdinsight-changing-configs-via-ambari/data-per-reducer-edited.png)
  
    1.024 MB'lık bir giriş boyutu ve redüktör başına 128 MB veri ile sekiz redüktör vardır (1024/128).

1. **Azaltıcı** parametresi başına Veri için yanlış bir değer, çok sayıda redüktöre neden olabilir ve bu da sorgu performansını olumsuz etkileyebilir. En fazla sayıda indiriciyi `hive.exec.reducers.max` sınırlamak için uygun bir değere ayarlayın. Varsayılan değer 1009'dur.

### <a name="enable-parallel-execution"></a>Paralel yürütmeyi etkinleştirme

Kovan sorgusu bir veya daha fazla aşamada yürütülür. Bağımsız aşamalar paralel olarak çalıştırılabiliyorsa, bu sorgu performansını artırır.

1. Paralel sorgu yürütmesini etkinleştirmek için Hive **Config** `hive.exec.parallel` sekmesine gidin ve özelliği arayın. Varsayılan değer false'tur. Değeri true'ya değiştirin ve değeri kaydetmek için **Enter** tuşuna basın.

1. Paralel olarak çalıştırılabilen iş sayısını `hive.exec.parallel.thread.number` sınırlamak için özelliği değiştirin. Varsayılan değer 8'dir.

    ![Apache Hive exec paralel ekran](./media/hdinsight-changing-configs-via-ambari/apache-hive-exec-parallel.png)

### <a name="enable-vectorization"></a>Vektörelleştirme

Kovan verileri satır satır işler. Vektörelleştirme, Hive'ı verileri bir defada bir satır yerine 1.024 satırlık bloklar halinde işlemeye yönlendirir. Vektörelleştirme yalnızca ORC dosya biçimi için geçerlidir.

1. Vektörel sorgu yürütmesini etkinleştirmek için Hive **Configs** sekmesine gidin ve parametreyi `hive.vectorized.execution.enabled` arayın. Varsayılan değer Kovan 0.13.0 veya sonraki için geçerlidir.

1. Sorgunun azaltma tarafı için vektörel yürütmeyi `hive.vectorized.execution.reduce.enabled` etkinleştirmek için parametreyi doğru ayarlayın. Varsayılan değer false'tur.

    ![Apache Hive vektörel yürütme](./media/hdinsight-changing-configs-via-ambari/hive-vectorized-execution.png)

### <a name="enable-cost-based-optimization-cbo"></a>Maliyet tabanlı optimizasyon (CBO) etkinleştirme

Varsayılan olarak, Hive en iyi sorgu yürütme planı bulmak için kurallar kümesi izler. Maliyet tabanlı optimizasyon (CBO), sorguyu yürütmek için birden çok planı değerlendirir. Ve her plana bir maliyet atar, sonra bir sorgu yürütmek için en ucuz planı belirler.

CBO'yu etkinleştirmek **için, Hive** > **Configs** > **Ayarları'na** gidin ve Maliyet Tabanlı Optimize **Et'i Etkinleştir'i**bulun, ardından geçiş düğmesini **Açık'a**çevirin.

![HDInsight maliyet tabanlı optimize edici](./media/hdinsight-changing-configs-via-ambari/hdinsight-cbo-config.png)

Aşağıdaki ek yapılandırma parametreleri CBO etkinleştirildiğinde Kovan sorgu performansını artırır:

* `hive.compute.query.using.stats`

    Doğru ayarlandığında, Hive gibi `count(*)`basit sorguları yanıtlamak için metadeposunda depolanan istatistikleri kullanır.

    ![İstatistikleri kullanarak Apache Hive hesaplama sorgusu](./media/hdinsight-changing-configs-via-ambari/hive-compute-query-using-stats.png)

* `hive.stats.fetch.column.stats`

    CBO etkinleştirildiğinde sütun istatistikleri oluşturulur. Kovan sorguları optimize etmek için metastore'da depolanan sütun istatistiklerini kullanır. Sütun sayısı yüksek olduğunda her sütun için sütun istatistiklerialma daha uzun sürer. Yanlış olarak ayarlandığında, bu ayar metastore'dan sütun istatistiklerini getirmeyi devre dışı kılabilir.

    ![Apache Hive istatistikleri sütun istatistikleri ayarlamak](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-column-stats.png)

* `hive.stats.fetch.partition.stats`

    Satır sayısı, veri boyutu ve dosya boyutu gibi temel bölüm istatistikleri metastore'da depolanır. Doğru olarak ayarlanırsa, bölüm istatistikleri metastore'dan getirilir. Yanlış olduğunda, dosya boyutu dosya sisteminden getirilir. Ve satır sayısı satır şema getirilir.

    ![Kovan istatistikleri bölüm istatistikleri ayarlamak](./media/hdinsight-changing-configs-via-ambari/hive-stats-fetch-partition-stats.png)

### <a name="enable-intermediate-compression"></a>Ara sıkıştırmayı etkinleştirme

Harita görevleri, azaltıcı görevler tarafından kullanılan ara dosyalar oluşturur. Ara sıkıştırma ara dosya boyutunu küçültür.

Hadoop işleri genellikle I / O darboğaz vardır. Verileri sıkıştırmak G/Ç'yi ve genel ağ aktarımını hızlandırabilir.

Kullanılabilir sıkıştırma türleri şunlardır:

| Biçimlendir | Araç | Algoritma | Dosya Uzantısı | Bölünene mi? |
| --- | --- | --- | --- | --- |
| Gzip | Gzip | Deflate | `.gz` | Hayır |
| Bzip2 | Bzip2 | Bzip2 |`.bz2` | Evet |
| LZO | `Lzop` | LZO | `.lzo` | Evet, dizine eklenmişse |
| Çabuk | Yok | Çabuk | Çabuk | Hayır |

Genel bir kural olarak, sıkıştırma yöntemi splittable olması önemlidir, aksi takdirde birkaç mappers oluşturulur. Giriş verileri metinise, `bzip2` en iyi seçenektir. ORC biçimi için Snappy en hızlı sıkıştırma seçeneğidir.

1. Ara sıkıştırmayı etkinleştirmek için Kovan **Configs** sekmesine `hive.exec.compress.intermediate` gidin ve sonra parametreyi doğru olarak ayarlayın. Varsayılan değer false'tur.

    !['Kovan exec kompres ara'](./media/hdinsight-changing-configs-via-ambari/hive-exec-compress-intermediate.png)

    > [!NOTE]  
    > Ara dosyaları sıkıştırmak için, codec'in yüksek sıkıştırma çıktısı olmasa bile, daha düşük CPU maliyetine sahip bir sıkıştırma codec'i seçin.

1. Ara sıkıştırma codec'ini ayarlamak `mapred.map.output.compression.codec` için `hive-site.xml` özel `mapred-site.xml` özelliği veya dosyayı ekleyin.

1. Özel ayar eklemek için:

    a. **Hive** > **Configs** > **Gelişmiş** > Özel**kovan-site**gidin.

    b. Özel kovan sitesi bölmesinin altındaki **Özellik Ekle...'yi** seçin.

    c. Özellik Ekle penceresinde, `mapred.map.output.compression.codec` anahtar ve `org.apache.hadoop.io.compress.SnappyCodec` değer olarak girin.

    d. **Add (Ekle)** seçeneğini belirleyin.

    !['Apache Hive özel özellik eklemek'](./media/hdinsight-changing-configs-via-ambari/hive-custom-property.png)

    Bu ayar, Snappy sıkıştırma kullanarak ara dosyayı sıkıştırır. Özellik eklendikten sonra, Özel kovan sitesi bölmesinde görünür.

    > [!NOTE]  
    > Bu yordam dosyayı `$HADOOP_HOME/conf/hive-site.xml` değiştirir.

### <a name="compress-final-output"></a>Son çıkışı sıkıştırın

Son Hive çıkışı da sıkıştırılabilir.

1. Son Hive çıktısını sıkıştırmak için Hive **Configs** sekmesine `hive.exec.compress.output` gidin ve parametreyi doğru şekilde ayarlayın. Varsayılan değer false'tur.

1. Çıktı sıkıştırma codec'ini `mapred.output.compression.codec` seçmek için, önceki bölümün adım 3'te açıklandığı gibi özel özelliği Özel kovan sitesi bölmesine ekleyin.

    ![Apache Hive özel özellik add2](./media/hdinsight-changing-configs-via-ambari/hive-custom-property2.png)

### <a name="enable-speculative-execution"></a>Spekülatif yürütmeyi etkinleştirme

Spekülatif yürütme, yavaş çalışan görev izleyicisini algılamak ve reddetmek için belirli sayıda yinelenen görev başlatir. Tek tek görev sonuçlarını optimize ederek genel iş yürütmeyi geliştirirken.

Spekülatif yürütme, büyük miktarda girdiiçeren uzun soluklu MapReduce görevleri için açık olmamalıdır.

* Spekülatif yürütmeyi etkinleştirmek için Hive **Configs** sekmesine gidin ve parametreyi `hive.mapred.reduce.tasks.speculative.execution` doğru olarak ayarlayın. Varsayılan değer false'tur.

    !['Kovan mapred görevleri spekülatif yürütme azaltmak'](./media/hdinsight-changing-configs-via-ambari/hive-mapred-reduce-tasks-speculative-execution.png)

### <a name="tune-dynamic-partitions"></a>Dinamik bölümleri ayarlayın

Hive, her bölümü önceden tanımlamadan bir tabloya kayıt eklerken dinamik bölümler oluşturmanızı sağlar. Bu yetenek güçlü bir özelliktir. Çok sayıda bölüm oluşturulmasına neden olsa da. Ve her bölüm için çok sayıda dosya.

1. Hive'ın dinamik bölümler yapması `hive.exec.dynamic.partition` için parametre değerinin (varsayılan) doğru olması gerekir.

1. Dinamik bölüm modunu katı olarak *değiştirin.* Sıkı modda, en az bir bölüm statik olmalıdır. Bu ayar WHERE yan tümcesinde bölüm filtresi olmadan sorguları önler, yani tüm bölümleri tarayın sorguları *kesin* engeller. Hive **Configs** sekmesine gidin ve `hive.exec.dynamic.partition.mode` sonra **katı**olarak ayarlayın. Varsayılan değer **katı değildir.**

1. Oluşturulacak dinamik bölüm sayısını sınırlamak için parametreyi değiştirin. `hive.exec.max.dynamic.partitions` Varsayılan değer 5000'dir.

1. Düğüm başına dinamik bölüm toplam sayısını sınırlamak `hive.exec.max.dynamic.partitions.pernode`için değiştirin. Varsayılan değer 2000'dir.

### <a name="enable-local-mode"></a>Yerel modu etkinleştirme

Yerel mod, Hive'ın tek bir makinede bir işin tüm görevlerini yapmasını sağlar. Ya da bazen tek bir süreçte. Giriş verileri küçükse, bu ayar sorgu performansını artırır. Ve sorgular için görevleri başlatma yükü genel sorgu yürütme önemli bir yüzdesi tüketir.

Yerel modu etkinleştirmek `hive.exec.mode.local.auto` için, [ara sıkıştırmayı etkinleştir](#enable-intermediate-compression) bölümünün 3.

![Apache Hive exec modu yerel otomatik](./media/hdinsight-changing-configs-via-ambari/hive-exec-mode-local-auto.png)

### <a name="set-single-mapreduce-multigroup-by"></a>Tek MapReduce MultiGROUP BY'yi ayarlama

Bu özellik doğru ayarlandığında, ortak grup tabanlı anahtarlara sahip bir MultiGROUP BY sorgusu tek bir MapReduce işi oluşturur.  

Bu davranışı etkinleştirmek `hive.multigroupby.singlereducer` için, [ara sıkıştırmayı etkinleştir](#enable-intermediate-compression) bölümünün 3.

![Kovan seti tek MapReduce MultiGROUP BY](./media/hdinsight-changing-configs-via-ambari/hive-multigroupby-singlereducer.png)

### <a name="additional-hive-optimizations"></a>Ek Kovan optimizasyonları

Aşağıdaki bölümlerde ayarlayabileceğiniz kovan ile ilgili ek optimizasyonlar açıklanabilir.

#### <a name="join-optimizations"></a>Optimizasyonlara katılma

Kovan'daki varsayılan birleştirme türü bir *shuffle birleştirmedir.* Kovan'da, özel haritalayıcılar girişi okur ve bir ara dosyaya birleştirme anahtarı/değer çiftini yarayar. Hadoop sıralar ve bir shuffle aşamasında bu çiftleri birleştirir. Bu karıştırma aşaması pahalıdır. Verilerinize göre doğru birleştirme'yi seçmek performansı önemli ölçüde artırabilir.

| Birleştirme Türü | Tesis | Nasıl | Kovan ayarları | Yorumlar |
| --- | --- | --- | --- | --- |
| Karıştırma Birleştirme | <ul><li>Varsayılan seçim</li><li>Her zaman çalışır</li></ul> | <ul><li>Tablolardan birinin bir kısmından okuma</li><li>Birleştirme tuşundaki kovalar ve sıralamalar</li><li>Her azaltmak için bir kova gönderir</li><li>Birleştirme Azalt tarafında yapılır</li></ul> | Önemli bir Kovan ayarı gerekli değil | Her zaman çalışır |
| Haritaya Katıl | <ul><li>Bir tablo belleğe sığabilir</li></ul> | <ul><li>Bellek karma tablosuna küçük tablo okur</li><li>Büyük dosyanın bir bölümünden akışlar</li><li>Karma tablodaki her kaydı birleştirir</li><li>Birleştirmeler sadece mapper tarafından</li></ul> | `hive.auto.confvert.join=true` | Hızlı, ancak sınırlı |
| Birleştirme Kovası'nı Sırala | Her iki tablo da: <ul><li>Aynı sıralanmış</li><li>Kovalı aynı</li><li>Sıralanmış/kovalanmış sütunda katılma</li></ul> | Her işlem: <ul><li>Her tablodan bir kova okur</li><li>Satırı en düşük değerle işler</li></ul> | `hive.auto.convert.sortmerge.join=true` | Verimli |

#### <a name="execution-engine-optimizations"></a>Yürütme motoru optimizasyonları

Hive yürütme motoru optimize etmek için ek öneriler:

| Ayar | Önerilen | HDInsight Varsayılan |
| --- | --- | --- |
| `hive.mapjoin.hybridgrace.hashtable` | True = daha güvenli, daha yavaş; false = daha hızlı | yanlış |
| `tez.am.resource.memory.mb` | Çoğu için 4 GB üst sınır | Otomatik Ayarlı |
| `tez.session.am.dag.submit.timeout.secs` | 300+ | 300 |
| `tez.am.container.idle.release-timeout-min.millis` | 20000+ | 10000 |
| `tez.am.container.idle.release-timeout-max.millis` | 40000+ | 20000 |

## <a name="apache-pig-optimization"></a>Apache Pig optimizasyonu

[Apache Pig](https://pig.apache.org/) özellikleri Ambari web UI'den Pig sorgularını ayarlamak için değiştirilebilir. Ambari'deki Pig özelliklerini değiştirmek, dosyadaki `/etc/pig/2.4.2.0-258.0/pig.properties` Pig özelliklerini doğrudan değiştirir.

1. Pig özelliklerini değiştirmek için Pig **Configs** sekmesine gidin ve ardından **Gelişmiş domuz özellikleri** bölmesini genişletin.

1. Değiştirmek istediğiniz özelliğin değerini bulun, açıklamayapmayı bırakın ve değiştirin.

1. Yeni değeri kaydetmek için pencerenin sağ üst tarafında **Kaydet'i** seçin. Bazı özellikler bir hizmet yeniden gerektirebilir.

    ![Gelişmiş Apaçi domuz özellikleri](./media/hdinsight-changing-configs-via-ambari/advanced-pig-properties.png)

> [!NOTE]  
> Oturum düzeyindeki ayarlar `pig.properties` dosyadaki özellik değerlerini geçersiz kılar.

### <a name="tune-execution-engine"></a>Yürütme motorini ayarlayın

Pig komut dosyalarını çalıştırmak için iki yürütme motoru mevcuttur: MapReduce ve Tez. Tez optimize edilmiş bir motordur ve MapReduce'dan çok daha hızlıdır.

1. Yürütme altyapısını değiştirmek için, **Gelişmiş domuz özellikleri** bölmesinde, özelliği `exectype`bulun.

1. Varsayılan değer **MapReduce'dir.** **Tez**olarak değiştirin.

### <a name="enable-local-mode"></a>Yerel modu etkinleştirme

Hive'a benzer şekilde, yerel mod nispeten daha küçük miktarda veriiçeren işleri hızlandırmak için kullanılır.

1. Yerel modu etkinleştirmek `pig.auto.local.enabled` **için, doğru**ayarlayın. Varsayılan değer false'tur.

1. Girdi veri boyutu özellik değerinden `pig.auto.local.input.maxbytes` daha az olan işler küçük işler olarak kabul edilir. Varsayılan değer 1 GB'dır.

### <a name="copy-user-jar-cache"></a>Kullanıcı jar önbelleğini kopyalama

Domuz, UDF'ler tarafından gerekli olan JAR dosyalarını görev düğümleri için kullanılabilir hale getirmek için dağıtılmış bir önbelleğe kopyalar. Bu kavanozlar sık sık değişmez. Etkinleştirilirse, `pig.user.cache.enabled` ayar kavanozların aynı kullanıcı tarafından işletilen işler için yeniden kullanmak üzere önbelleğe yerleştirilmesini sağlar. Bu ayar, iş performansında küçük bir artışa neden olabilir.

1. Etkinleştirmek için, doğru ayarlayın. `pig.user.cache.enabled` Varsayılan değer false.

1. Önbelleğe alınmış kavanozların taban yolunu ayarlamak `pig.user.cache.location` için temel yola ayarlayın. Varsayılan değer: `/tmp`.

### <a name="optimize-performance-with-memory-settings"></a>Bellek ayarlarıyla performansı optimize edin

Aşağıdaki bellek ayarları Pig komut dosyası performansını optimize yardımcı olabilir.

* `pig.cachedbag.memusage`: Bir çantaya verilen hafıza miktarı. Bir çanta tuples koleksiyonudur. Tuple sıralı bir alan kümesidir ve alan bir veri parçasıdır. Bir torbadaki veriler verilen belleğin ötesindeyse, diske dökülür. Varsayılan değer, kullanılabilir belleğin yüzde 20'sini temsil eden 0,2'dir. Bu bellek bir uygulamadaki tüm torbalar arasında paylaşılır.

* `pig.spill.size.threshold`: Bu dökülme boyutu eşiğinin (baytlarda) daha büyük torbalar diske dökülür. Varsayılan değer 5 MB'dır.

### <a name="compress-temporary-files"></a>Geçici dosyaları sıkıştırma

Domuz iş yürütme sırasında geçici dosyalar oluşturur. Geçici dosyaları sıkıştırmak, dosyaları okurken veya diske yazarken performans artışına neden olabilir. Geçici dosyaları sıkıştırmak için aşağıdaki ayarlar kullanılabilir.

* `pig.tmpfilecompression`: Doğru olduğunda, geçici dosya sıkıştırma sağlar. Varsayılan değer false'tur.

* `pig.tmpfilecompression.codec`: Geçici dosyaları sıkıştırmak için kullanılacak sıkıştırma codec.. Önerilen sıkıştırma codec'leri düşük CPU kullanımı için [LZO](https://www.oberhumer.com/opensource/lzo/) ve Snappy'dir.

### <a name="enable-split-combining"></a>Bölme birleştirme yi etkinleştirme

Etkinleştirildiğinde, küçük dosyalar daha az eşlemi görevi için birleştirilir. Bu ayar, birçok küçük dosyayla işlerin verimliliğini artırır. Etkinleştirmek için, doğru ayarlayın. `pig.noSplitCombination` Varsayılan değer false'tur.

### <a name="tune-mappers"></a>Haritaperları ayarlayın

Mappers sayısı özelliği `pig.maxCombinedSplitSize`değiştirerek kontrol edilir. Bu özellik, tek bir harita görevi tarafından işlenecek verilerin boyutunu belirtir. Varsayılan değer, dosya sisteminin varsayılan blok boyutudur. Bu değeri artırmak, mapper görevlerinin sayısının azalmasına neden olabilir.

### <a name="tune-reducers"></a>Ayar redüktörleri

Azaltıcı sayısı parametreye `pig.exec.reducers.bytes.per.reducer`göre hesaplanır. Parametre, varsayılan olarak 1 GB olarak, azaltıcı başına işlenen bayt sayısını belirtir. En fazla indirici sayısını sınırlamak `pig.exec.reducers.max` için, varsayılan olarak 999 özelliği ayarlayın.

## <a name="apache-hbase-optimization-with-the-ambari-web-ui"></a>Ambari web UI ile Apache HBase optimizasyonu

[Apache HBase](https://hbase.apache.org/) yapılandırması **HBase Configs** sekmesinden değiştirilir. Aşağıdaki bölümlerde HBase performansını etkileyen bazı önemli yapılandırma ayarları açıklayınız.

### <a name="set-hbase_heapsize"></a>HBASE_HEAPSIZE ayarlayın

HBase yığın *boyutu, bölgeye* ve *ana* sunuculara göre megabaytlarda kullanılacak maksimum yığın miktarını belirtir. Varsayılan değer 1.000 MB'dır. Bu değer küme iş yükü için ayarlanmalıdır.

1. Değiştirmek için, HBase **Configs** sekmesindeki **Gelişmiş HBase-env** bölmesine gidin `HBASE_HEAPSIZE` ve ardından ayarı bulun.

1. Varsayılan değeri 5.000 MB olarak değiştirin.

    !['Apache Ambari HBase bellek yığını'](./media/hdinsight-changing-configs-via-ambari/ambari-hbase-heapsize.png)

### <a name="optimize-read-heavy-workloads"></a>Okuma ağırlıklı iş yüklerini optimize edin

Aşağıdaki yapılandırmalar, okuma ağırlıklı iş yüklerinin performansını artırmak için önemlidir.

#### <a name="block-cache-size"></a>Önbellek boyutunu engelle

Blok önbelleği okundu önbelleğidir. Boyutu `hfile.block.cache.size` parametre tarafından kontrol edilir. Varsayılan değer 0,4'tür ve bu da toplam bölge sunucusu belleğinin yüzde 40'ıdır. Blok önbellek boyutu ne kadar büyükse, rasgele okumalar o kadar hızlı olur.

1. Bu parametreyi değiştirmek için HBase **Configs** sekmesindeki **Ayarlar** sekmesine gidin ve ardından **Ayırıcıları Okumak için Ayrılan RegionServer'ın %'sini**bulun.

    ![Apache HBase bellek bloğu önbellek boyutu](./media/hdinsight-changing-configs-via-ambari/hbase-block-cache-size.png)

1. Değeri değiştirmek için **Edit** simgesini seçin.

#### <a name="memstore-size"></a>Memstore boyutu

Tüm depolar, *Memstore*adı verilen bellek arabelleğinde depolanır. Bu arabellek, tek bir işlemde diske yazılabilir toplam veri miktarını artırır. Ayrıca, son yapılan lara erişimi hızlandırıyor. Memstore boyutu aşağıdaki iki parametre ile tanımlanır:

* `hbase.regionserver.global.memstore.UpperLimit`: Memstore'un birleştirebileceği en yüksek bölge sunucusu yüzdesini tanımlar.

* `hbase.regionserver.global.memstore.LowerLimit`: Memstore'un birleştirebileceği en az bölge sunucusu yüzdesini tanımlar.

Rasgele okumalar için optimize etmek için, Memstore üst ve alt sınırları azaltabilirsiniz.

#### <a name="number-of-rows-fetched-when-scanning-from-disk"></a>Diskten tarama yaparken getirilen satır sayısı

Ayar, `hbase.client.scanner.caching` `next` yöntem tarayıcıda çağrıldığında diskten okunan satır sayısını tanımlar.  Varsayılan değer 100’dür. Sayı ne kadar yüksekse, istemciden bölge sunucusuna yapılan uzaktan aramalar o kadar az dır ve bu da daha hızlı taramalara neden olabilir. Ancak, bu ayar istemci üzerindeki bellek baskısını da artırır.

![Getirilen satırların Apache HBase sayısı](./media/hdinsight-changing-configs-via-ambari/hbase-num-rows-fetched.png)

> [!IMPORTANT]  
> Bir tarayıcıda bir sonraki yöntemin çağrılması arasındaki sürenin tarayıcı zaman daha büyük olduğu değeri ayarlamayın. Tarayıcı zaman zaman anına `hbase.regionserver.lease.period` kadar özellik tarafından tanımlanır.

### <a name="optimize-write-heavy-workloads"></a>Yazma ağıriş yüklerini optimize edin

Yazma ağır iş yüklerinin performansını artırmak için aşağıdaki yapılandırmalar önemlidir.

#### <a name="maximum-region-file-size"></a>Maksimum bölge dosya boyutu

HBase verileri *HFile*adı verilen dahili bir dosya biçiminde depolar. Özellik, `hbase.hregion.max.filesize` bir bölge için tek bir HFile boyutunu tanımlar.  Bir bölgedeki tüm HFiles'ın toplamı bu ayardadaha büyükse, bölge iki bölgeye ayrılır.

!['Apache HBase HRegion max filesize'](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-max-filesize.png)

Bölge dosya boyutu ne kadar büyükse, bölme sayısı da o kadar azdır. En yüksek yazma performansıyla sonuçlanan bir değer belirlemek için dosya boyutunu artırabilirsiniz.

#### <a name="avoid-update-blocking"></a>Güncelleştirme engellemeyi önleyin

* Özellik, `hbase.hregion.memstore.flush.size` Memstore'un diske atıldığı boyutu tanımlar. Varsayılan boyut 128 MB'dır.

* HBase bölge blok çarpanı `hbase.hregion.memstore.block.multiplier`ile tanımlanır. Varsayılan değer 4'tür. İzin verilen maksimum 8'dir.

* Memstore bayt`hbase.hregion.memstore.flush.size` * `hbase.hregion.memstore.block.multiplier`ise HBase güncelleştirmeleri engeller.

    Floş boyutu ve blok çarpanının varsayılan değerleri yle, Memstore 128 * 4 = 512 MB boyutunda olduğunda güncelleştirmeler engellenir. Güncelleştirme engelleme sayısını azaltmak için, `hbase.hregion.memstore.block.multiplier`değerini artırın.

![Apache HBase Bölge Blok Çarpanı](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-block-multiplier.png)

### <a name="define-memstore-size"></a>Memstore boyutunu tanımla

Memstore boyutu ve `hbase.regionserver.global.memstore.UpperLimit` `hbase.regionserver.global.memstore.LowerLimit` parametreleri ile tanımlanır. Bu değerleri birbirine eşit ayarlamak, yazma sırasında duraklamaları azaltır (ayrıca daha sık kızarmaya neden olur) ve yazma performansının artmasına neden olur.

### <a name="set-memstore-local-allocation-buffer"></a>Memstore yerel ayırma arabelleği ayarlama

Memstore yerel ayırma arabelleği kullanımı `hbase.hregion.memstore.mslab.enabled`özelliği tarafından belirlenir. Etkinleştirildiğinde (doğru), bu ayar ağır yazma işlemi sırasında yığın parçalanmasını önler. Varsayılan değer true şeklindedir.

![hbase.hregion.memstore.mslab.enabled](./media/hdinsight-changing-configs-via-ambari/hbase-hregion-memstore-mslab-enabled.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Ambari web UI ile HDInsight kümelerini yönetin](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
