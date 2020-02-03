---
title: Azure HDInsight Hadoop kümesinde veri için özellikler oluşturma-Team Data Science Işlemi
description: Bir Azure HDInsight Hadoop kümesinde depolanan verilerin özelliklerini oluşturma Hive sorgularının örnekleri.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: c926aac3ea4360793ff52b616a55dc6198357c8a
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721787"
---
# <a name="create-features-for-data-in-a-hadoop-cluster-using-hive-queries"></a>Bir Hadoop kümesinde Hive sorgularını kullanarak verilerin özelliklerini oluşturma
Bu belge, Hive sorgularını kullanarak bir Azure HDInsight Hadoop kümesinde depolanan verilerin özelliklerini oluşturma işlemi gösterilmektedir. Bu Hive sorguları katıştırılmış Hive User-Defined betikleri, sağlanan işlevler (UDF'ler) kullanın.

Özellikler oluşturmak için gereken işlemleri, bellek kullanımı yoğun olabilir. Hive sorgu performansı bu gibi durumlarda daha önemli hale gelir ve belirli parametreleri ayarlayarak geliştirilebilir. Bu parametreleri ayarlama son bölümde ele alınmıştır.

Şu şekilde gösterilen sorgu örnekleri, [GitHub deposunda](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)de yer alan [NYC TAXI seyahat verileri](https://chriswhong.com/open-data/foil_nyc_taxi/) senaryolarına özgüdür. Bu sorgular zaten belirtilen veri şemasına sahip ve çalıştırmak için gönderilmeye hazır. Son bölümde, kullanıcılar ayarlayabilirsiniz ve böylelikle Hive sorgu performansı artırılabilir parametreleri de ele alınmıştır.

Bu görev, [ekip veri bilimi işlemindeki (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımdır.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, olduğunu varsayar:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz. [Azure depolama hesabı oluşturma](../../storage/common/storage-account-create.md)
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi hazırlandı.  Yönergelere ihtiyacınız varsa bkz. [Gelişmiş analiz için Azure HDInsight Hadoop kümelerini özelleştirme](customize-hadoop-cluster.md).
* Azure HDInsight Hadoop kümeleri Hive tablolarında için verileri karşıya yüklendi. Bu yoksa, önce verileri Hive tablolarına yüklemek için [Create ve Hive tablolarına veri yükle](move-hive-tables.md) ' yi izleyin.
* Kümeye uzaktan erişim etkin. Yönergelere ihtiyacınız varsa bkz. [Hadoop kümesinin baş düğümüne erişme](customize-hadoop-cluster.md).

## <a name="hive-featureengineering"></a>Özellik oluşturma
Bu bölümde, hangi özellikleri kullanarak Hive sorguları oluşturma yol çeşitli örneklerini açıklanmaktadır. Ek özellikler oluşturduktan sonra bunları mevcut tabloya sütun olarak ekleyin veya birincil anahtar, özgün tablonun katılabilir ve ek özellikler ile yeni bir tablo oluşturabilirsiniz. Sunulan örnekleri aşağıda verilmiştir:

1. [Sıklık tabanlı özellik oluşturma](#hive-frequencyfeature)
2. [Ikili sınıflandırmada kategorik değişkenlerin riskleri](#hive-riskfeature)
3. [Tarih saat alanından özellikleri Ayıkla](#hive-datefeatures)
4. [Metin alanından özellikleri Ayıkla](#hive-textfeatures)
5. [GPS koordinatları arasındaki mesafeyi hesaplama](#hive-gpsdistance)

### <a name="hive-frequencyfeature"></a>Sıklık tabanlı özellik oluşturma
Genellikle, Kategorik bir değişken düzeyleri sıklığını ya da birden fazla kategorik değişken düzeylerinden belirli birleşimlerini sıklığını hesaplamak kullanışlıdır. Kullanıcılar bu frekansları hesaplamak için aşağıdaki betiği kullanabilirsiniz:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <a name="hive-riskfeature"></a>İkili sınıflandırmada kategorik değişkenlerin riskleri
Yalnızca kullanılan modelleri sayısal özelliklerini alırken ikili Sınıflandırma, sayısal olmayan kategorik değişkenleri sayısal özelliklerini dönüştürülmesi gerekir. Bu dönüştürme, her sayısal olmayan düzeyi ile sayısal bir risk değiştirilerek gerçekleştirilir. Bu bölüm, Kategorik bir değişken (günlük ekledikçe) risk değerleri hesaplama genel bazı Hive sorguları gösterir.

        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

Bu örnekte, `smooth_param1` ve `smooth_param2` değişkenleri verilerden hesaplanan risk değerlerini düzgünleştirmek üzere ayarlanır. Riskler -INF INF arasındaki aralığı vardır. Bir risk > 0 hedefi 1'e eşit olduğunu olasılık 0,5 büyük olduğunu gösterir.

Risk sonra tablo hesaplanır, kullanıcıların risk değerlerinin bir tabloya risk tabloyla katılarak atayabilirsiniz. Hive katılan sorgu, önceki bölümde sağlanmadı.

### <a name="hive-datefeatures"></a>Tarih saat alanlarından özellikleri Ayıkla
Hive, datetime alanları işleme için bir UDF'ler kümesi ile birlikte gelir. Hive, varsayılan datetime biçimi ' yyyy-aa-gg 00:00:00 ' ('1970-01-01 12:21:32 ' gibi). Bu bölümde, bir ay, bir datetime alanı ayın gününü çıkarma örnekleri ve başka bir tarih saat dizesi için varsayılan biçimi varsayılan biçimlendirme dışında bir biçimde bir tarih/saat dizeye Dönüştür diğer örnekler gösterilmektedir.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Bu Hive sorgusu, *\<DateTime alanının >* varsayılan tarih saat biçiminde olduğunu varsayar.

Bir datetime alanı varsayılan biçiminde değilse, datetime alanı Unix zaman damgası dönüştürmeniz ve varsayılan biçiminde olan bir tarih saat dizesi Unix zaman damgası dönüştürmek gerekir. Varsayılan tarih ve saat biçim olduğunda, kullanıcılar katıştırılmış tarih ve saat özellikleri ayıklanacak UDF'ler uygulayabilir.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

Bu sorguda, *\<DateTime alanı >* *03/26/2015 12:04:39*gibi bir düzene sahipse, *DateTime alanı > '\<deseninin* `'MM/dd/yyyy HH:mm:ss'`olması gerekir. Kullanıcılar, test etmek için çalıştırabilirsiniz

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Bu sorgudaki *hivesampletable* , kümeler sağlandığında varsayılan olarak tüm Azure HDInsight Hadoop kümelerine önceden yüklenmiş olarak gelir.

### <a name="hive-textfeatures"></a>Metin alanlarından özellikleri Ayıkla
Hive tablosu boşluklarla ayrılmış sözcük içeren bir metin alanı varsa, aşağıdaki sorguyu dize ve dize sözcük sayısı uzunluğunu ayıklar.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="hive-gpsdistance"></a>GPS koordinatları kümeleri arasında uzaklıkları hesaplama
Bu bölümde belirtilen sorgu için NYC taksi seyahat verilerini doğrudan uygulanabilir. Bu sorgu amacı özellikler oluşturmak için Hive içinde katıştırılmış bir matematiksel işlev uygulamak nasıl göstermektir.

Bu sorguda kullanılan alanlar, toplama *\_Boylam*, *toplama\_enlem*, *açılan\_Boylam*ve Dropoff *\_ENLEM*adlı toplama ve bırakma konumlarından oluşan GPS koordinatları. Toplama ve dropoff koordinatları arasında doğrudan uzaklık hesaplayın sorgular şunlardır:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

İki GPS koordinatları arasındaki mesafeyi hesaplayan matematik denklemleri, Peter Lapisu tarafından yazılan <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">taşınabilir tür betikleri</a> sitesinde bulunabilir. Bu JavaScript 'te işlev `toRad()`, Dereceyi radyana dönüştüren yalnızca Pi/180 *lat_or_lon*. Burada *lat_or_lon* Enlem veya boylam. Hive işlevi `atan2`sağlamıyor, ancak işlevi `atan`sağladığından, `atan2` işlevi, <a href="https://en.wikipedia.org/wiki/Atan2" target="_blank">Vipev'de</a>sağlanan tanım kullanılarak yukarıdaki Hive sorgusunda `atan` işlevi tarafından uygulanır.

![Çalışma alanı oluşturma](./media/create-features-hive/atan2new.png)

Hive Embedded UDF 'Leri listesini <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a>'Nin **yerleşik işlevler** bölümünde bulabilirsiniz.  

## <a name="tuning"></a>Gelişmiş Konular: sorgu hızını artırmak için Hive parametrelerini ayarlayın
Hive kümesinin varsayılan parametre ayarları Hive sorguları ve sorgular işlenirken veri için uygun olmayabilir. Bu bölümde, kullanıcılar Hive sorgularının performansını geliştirmek için dinleyebilirsiniz bazı parametreler açıklanmaktadır. Veri işleme sorgular önce sorguları ayarlama parametre eklemek kullanıcıların gerekir.

1. **Java yığın alanı**: büyük veri kümelerine katılmayı veya uzun kayıtları işlemeyi içeren sorgular için, **yığın alanı tükeniyor** ortak hatalardan biridir. *MapReduce. Map. Java. opts* ve *MapReduce. Task. IO. Sort. MB* parametreleri istenen değerlere ayarlanarak bu hata kaçınılabilir. Örnek aşağıda verilmiştir:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Bu parametre, Java yığın alanına 4 GB bellek ayırır ve ayrıca sıralama, daha fazla bellek ayırarak daha verimli hale gelir. Yığın alanı ilgili bir başarısızlık hatalarını herhangi bir iş varsa bu ayırmaları ile yürütmek için iyi bir fikirdir.

1. **DFS blok boyutu**: Bu parametre, dosya sisteminin depoladığı en küçük veri birimini ayarlar. DFS blok boyutu düşük ve en fazla 128 MB, ardından boyuttaki veriyi olursa örnek olarak, 128 MB tek bir blok içinde depolanır. 128 MB'den büyük veri, ek blokları atanır. 
2. Ad düğümü dosyasıyla ilgili blok bulmak için çok daha fazla isteklerini işlemek olduğundan küçük blok boyutu seçme büyük ek yüklerini Hadoop neden olur. Bir önerilen ilgilenme gigabayt ile (veya daha büyük olduğunda) ayarı veriler:

        set dfs.block.size=128m;

2. **Hive 'de birleştirme Işlemini En Iyi duruma getirme**: harita/küçültme çerçevesindeki birleştirme işlemleri tipik olarak azaltma aşamasında gerçekleşirken, bazen de büyük bir kazanç, eşleme aşamasında ("mapjoın" olarak da bilinir) zamanlama ile zaman ayırarak elde edilebilir. Bu seçeneği ayarlayın:
   
       set hive.auto.convert.join=true;

3. **Hive 'e Mapvı sayısını belirtme**: Hadoop, kullanıcının azaltıcının sayısını ayarlamasına izin veriyorsa, mapvana sayısı genellikle kullanıcı tarafından ayarlanmamalıdır. Bu sayı üzerinde bir ölçüde denetim kullanılmasına izin veren bir eli, her harita görevinin boyutu tarafından belirlendiği şekilde *mapred. min. Split. size* ve *mapred. Max. Split. size* Hadoop değişkenlerini seçmektedir:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Genellikle, varsayılan değeri:
    
   - *mapred. min. Split. size* değeri 0,
   - *mapred. Max. Split. size* **Long. Max** ve 
   - *DFS. Block. size* 64 MB 'tır.

     Biz, veri boyutu verilen görebileceğiniz gibi "ayarı" tarafından bu parametreleri ayarlama sağlar bize kullanılan azaltıcının sayısını ayarlamak.

4. Hive performansını iyileştirmeye yönelik daha **Gelişmiş diğer seçenekler** aşağıda verilmiştir. Bu seçenekler, görevleri eşlemek ve azaltmak için ayrılan belleği ayarlamanıza olanak tanır ve performans için çok daha fazla performans sağlar. *MapReduce. azaltma. Memory. MB* ' nın, Hadoop kümesindeki her bir çalışan düğümünün fiziksel bellek boyutundan büyük olamaz.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

