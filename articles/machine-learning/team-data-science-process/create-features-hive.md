---
title: Azure HDInsight Hadoop kümesindeki veriler için özellikler oluşturma - Ekip Veri Bilimi Süreci
description: Azure HDInsight Hadoop kümesinde depolanan verilerde özellikler oluşturan Hive sorgularına örnekler.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721787"
---
# <a name="create-features-for-data-in-a-hadoop-cluster-using-hive-queries"></a>Hive sorgularını kullanarak Hadoop kümesindeki veriler için özellikler oluşturma
Bu belge, Hive sorgularını kullanarak Bir Azure HDInsight Hadoop kümesinde depolanan veriler için özelliklerin nasıl oluşturuluğa ilişkin özellikleri gösterir. Bu Kovan sorguları, sağlanan komut dosyaları olan gömülü Hive Kullanıcı Tanımlı İşlevler (UDFs) kullanır.

Özellikleri oluşturmak için gereken işlemler bellek yoğun olabilir. Hive sorgularının performansı bu gibi durumlarda daha kritik hale gelir ve belirli parametreleri alarak geliştirilebilir. Bu parametrelerin tuning son bölümde ele alınmıştır.

Sunulan sorguörnekleri [NYC Taksi Gezisi Veri](https://chriswhong.com/open-data/foil_nyc_taxi/) senaryoları için özel de [GitHub deposunda](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts)sağlanır. Bu sorgular zaten veri şeması belirtilen ve çalıştırmak için gönderilmeye hazırdır. Son bölümde, kullanıcıların Hive sorgularının performansının geliştirilebilmeleri için ayarlayabildiği parametreler de tartışılır.

Bu [görev, Ekip Veri Bilimi Süreci'nin (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımıdır.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sahip olduğunuzu varsayar:

* Bir Azure depolama hesabı oluşturdu. Yönergelere ihtiyacınız [varsa,](../../storage/common/storage-account-create.md) bkz.
* HDInsight hizmeti ile özelleştirilmiş bir Hadoop kümesi sağlanmış.  Talimatlara ihtiyacınız varsa, [Gelişmiş Analitik için Azure HDInsight Hadoop Kümelerini Özelleştir'e](customize-hadoop-cluster.md)bakın.
* Veriler Azure HDInsight Hadoop kümelerinde Hive tablolarına yüklendi. Yoksa, önce Kovan tablolarına veri yüklemek için [Veri Oluştur'u izleyin ve Verileri Kovan tablolarına yükleyin.](move-hive-tables.md)
* Kümeye uzaktan erişim etkin. Talimatlara ihtiyacınız varsa, [Hadoop Kümesi'nin Baş Düğümüne Eriş'e](customize-hadoop-cluster.md)bakın.

## <a name="feature-generation"></a><a name="hive-featureengineering"></a>Özellik oluşturma
Bu bölümde, hive sorguları kullanılarak özelliklerin nasıl üretilebileceğine birkaç örnek açıklanmıştır. Ek özellikler oluşturduktan sonra, bunları varolan tabloya sütun olarak ekleyebilir veya ek özellikler ve birincil anahtarla birlikte özgün tabloyla birleşebilecek yeni bir tablo oluşturabilirsiniz. Sunulan örnekler şunlardır:

1. [Frekans Tabanlı Özellik Oluşturma](#hive-frequencyfeature)
2. [İkili Sınıflandırmada Kategorik Değişkenlerin Riskleri](#hive-riskfeature)
3. [Datetime Field'dan özellikleri ayıklama](#hive-datefeatures)
4. [Metin Alanından özellikler ayıklama](#hive-textfeatures)
5. [GPS koordinatları arasındaki mesafeyi hesaplama](#hive-gpsdistance)

### <a name="frequency-based-feature-generation"></a><a name="hive-frequencyfeature"></a>Frekans tabanlı özellik oluşturma
Genellikle bir kategorik değişkenin düzeylerinin frekanslarını veya birden çok kategorik değişkenden belirli düzey kombinasyonlarının frekanslarını hesaplamak yararlıdır. Kullanıcılar bu frekansları hesaplamak için aşağıdaki komut dosyasını kullanabilir:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


### <a name="risks-of-categorical-variables-in-binary-classification"></a><a name="hive-riskfeature"></a>İkili sınıflandırmada kategorik değişkenlerin riskleri
İkili sınıflandırmada, kullanılan modeller yalnızca sayısal özellikler aldığında sayısal olmayan kategorik değişkenler sayısal özelliklere dönüştürülmelidir. Bu dönüştürme, sayısal olmayan her düzeyin sayısal riskle değiştirilmesiyle yapılır. Bu bölümde, kategorik bir değişkenin risk değerlerini (günlük oranları) hesaplayan bazı genel Hive sorguları gösterilmektedir.

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

Bu örnekte, `smooth_param1` değişkenler ve `smooth_param2` verilerden hesaplanan risk değerlerini düzeltmek için ayarlanır. Riskler -Inf ve Inf arasında bir aralık vardır. 0'> bir risk, hedefin 1'e eşit olma olasılığının 0,5'ten büyük olduğunu gösterir.

Risk tablosu hesaplandıktan sonra, kullanıcılar risk tablosuna katılarak bir tabloya risk değerleri atayabilir. Hive birleştirme sorgusu önceki bölümde sağlanmıştır.

### <a name="extract-features-from-datetime-fields"></a><a name="hive-datefeatures"></a>Datetime alanlarından özellikleri ayıklama
Kovan, tarih zaman alanlarını işlemek için bir dizi UDF ile birlikte gelir. Kovan'da varsayılan tarih biçimi 'yyyy-MM-dd 00:00:00' ('1970-01-01 12:21:32' örneğin) şeklindedir. Bu bölümde, bir ayın gününü ayıklayan örnekler, bir datetime alanından ay ve varsayılan biçim dışındaki bir biçimde bir datetime dizesini varsayılan biçimde bir datetime dizesine dönüştüren diğer örnekler gösterilmektedir.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Bu Kovan sorgusu, * \<>datetime alanının* varsayılan datetime biçiminde olduğunu varsayar.

Bir datetime alanı varsayılan biçimde değilse, önce tarih zaman alanını Unix zaman damgasına dönüştürmeniz ve ardından Unix zaman damgasını varsayılan biçimde bir datetime dizesi dönüştürmeniz gerekir. Datetime varsayılan biçiminde olduğunda, kullanıcılar özellikleri ayıklamak için katıştırılmış datetime UDF'leri uygulayabilir.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

Bu sorguda, * \<tarih>* *03/26/2015 12:04:39*gibi desen varsa, * \<tarih alanı>' deseni* olmalıdır. `'MM/dd/yyyy HH:mm:ss'` Bunu test etmek için, kullanıcılar

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

Bu sorgudaki *kovan örnekleme tablosu,* kümeler sağlandığında varsayılan olarak tüm Azure HDInsight Hadoop kümelerinde önceden yüklenmiş olarak gelir.

### <a name="extract-features-from-text-fields"></a><a name="hive-textfeatures"></a>Metin alanlarından özellikleri ayıklama
Hive tablosunda boşluklarla sınırlandırılmış bir sözcük dizesi içeren bir metin alanı olduğunda, aşağıdaki sorgu dize uzunluğunu ve dizedeki sözcük sayısını ayıklar.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

### <a name="calculate-distances-between-sets-of-gps-coordinates"></a><a name="hive-gpsdistance"></a>GPS koordinatkümeleri arasındaki mesafeleri hesaplama
Bu bölümde verilen sorgu doğrudan NYC Taksi Gezisi Verileri uygulanabilir. Bu sorgunun amacı, özellikleri oluşturmak için Hive gömülü bir matematiksel işlevin nasıl uygulanacağı gösteriş tir.

Bu sorguda kullanılan alanlar pikap ve bırakma konumlarının GPS koordinatları, *pikap\_boylam*adlı, *pikap\_enlem*, *\_bırakma boylam*, ve *\_bırakma enlem*. Teslim alma ve bırakma koordinatları arasındaki doğrudan mesafeyi hesaplayan sorgular şunlardır:

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

İki GPS koordinatı arasındaki mesafeyi hesaplayan matematiksel denklemler, Peter Lapisu tarafından yazılan <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Hareketli Tip Komut Dosyaları</a> sitesinde bulunabilir. Bu Javascript'te `toRad()` işlev, dereceleri radyanlara dönüştüren pi/180 *lat_or_lon.* Burada, *lat_or_lon* enlem veya boylam olduğunu. Hive `atan2`işlevini sağlamadığından, ancak işlevi `atan`sağladığından, `atan2` işlev `atan` <a href="https://en.wikipedia.org/wiki/Atan2" target="_blank">Vikipedi'de</a>sağlanan tanım kullanılarak yukarıdaki Hive sorgusunda işlev tarafından uygulanır.

![Çalışma alanı oluşturma](./media/create-features-hive/atan2new.png)

Hive gömülü UDFs tam bir listesini <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">Apache Hive wiki</a> **yerleşik Fonksiyonlar** bölümünde bulunabilir).  

## <a name="advanced-topics-tune-hive-parameters-to-improve-query-speed"></a><a name="tuning"></a>Gelişmiş konular: Sorgu hızını artırmak için Kovan parametrelerini ayarlayın
Hive kümesinin varsayılan parametre ayarları, Hive sorguları ve sorguların işlemesi verileri için uygun olmayabilir. Bu bölümde, kullanıcıların Hive sorgularının performansını artırmak için ayarlayabildiği bazı parametreler anlatılmaktadır. Kullanıcıların veri işleme sorguları önce parametre atonlama sorguları eklemeniz gerekir.

1. **Java yığın alanı**: Büyük veri kümelerine katılmayı veya uzun kayıtları işlemeyi içeren sorgularda **yığın alanının bitmesi** sık karşılaşılan hatalardan biridir. *Mapreduce.map.java.opts* ve *mapreduce.task.io.sort.mb* parametrelerini istenilen değerlere ayarlayarak bu hata önlenebilir. Örnek aşağıda verilmiştir:
   
        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;

    Bu parametre, 4 GB belleği Java yığın alanına ayırır ve ayrıca bunun için daha fazla bellek ayırarak sıralamayı daha verimli hale getirir. Yığın alanıyla ilgili herhangi bir iş hatası hatası varsa, bu ayırmalarla oynamak iyi bir fikirdir.

1. **DFS blok boyutu**: Bu parametre, dosya sisteminin depolayan en küçük veri birimini ayarlar. Örnek olarak, DFS blok boyutu 128 MB ise, boyutu 128 MB'dan küçük ve 128 MB'a kadar olan tüm veriler tek bir blokta depolanır. 128 MB'dan büyük verilere fazladan blok tahsis edilir. 
2. Ad düğümü dosyayla ilgili ilgili bloğu bulmak için çok daha fazla isteği işlemek zorunda olduğundan, küçük bir blok boyutu seçmek Hadoop'ta büyük genel giderlere neden olur. Gigabayt (veya daha büyük) verilerle uğraşırken önerilen ayar aşağıdakileri

        set dfs.block.size=128m;

2. **Hive'de birleştirme işleminin optimizasyonu**: Harita/azaltma çerçevesindeki birleştirme işlemleri genellikle azaltma aşamasında gerçekleşirken, bazen harita aşamasında birleştirmelerin ("mapjoins" olarak da adlandırılır) planlanmasıyla büyük kazanımlar elde edilebilir. Bu seçeneği ayarlayın:
   
       set hive.auto.convert.join=true;

3. **Hive için mappers sayısını belirtme**: Hadoop kullanıcı indirgeyici lerin sayısını ayarlamak için izin verirken, mappers sayısı genellikle kullanıcı tarafından ayarlanmaz. Bu sayı üzerinde bir dereceye kadar kontrol sağlayan bir hile, her harita görevinin boyutu olarak *mapred.min.split.size* ve *mapred.max.split.size* hadoop değişkenlerini seçmektir:
   
        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))
   
    Tipik olarak, varsayılan değeri:
    
   - *mapred.min.split.size* 0, bu
   - *mapred.max.split.size* **Long.MAX** ve bu olduğunu 
   - *dfs.block.size* 64 MB olduğunu.

     Gördüğümüz gibi, veri boyutu göz önüne alındığında, bu parametreleri "ayarlayarak" ayarlamak kullanılan haritapernizin sayısını ayarlamamızı sağlar.

4. Burada Hive performansını optimize etmek için birkaç daha **gelişmiş seçenekler** vardır. Bu seçenekler, görevleri eşlemek ve azaltmak için ayrılan belleği ayarlamanızı sağlar ve performansı ayarlamada yararlı olabilir. *Mapreduce.reduce.memory.mb'nin* Hadoop kümesindeki her işçi düğümünün fiziksel bellek boyutundan büyük olamayacağını unutmayın.
   
        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;

