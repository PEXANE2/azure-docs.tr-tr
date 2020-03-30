---
title: Azure HDInsight'ta Kovan sorgularını optimize edin
description: Bu makalede, HDInsight'ta Hadoop için Apache Hive sorgularınızı nasıl optimize edeceğiniz açıklanmaktadır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2019
ms.openlocfilehash: 144d51d08a61526ec0f183a63e1fdf5658136293
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272337"
---
# <a name="optimize-apache-hive-queries-in-azure-hdinsight"></a>Azure HDInsight’ta Apache Hive sorgularını iyileştirme

Azure HDInsight'ta, Apache Hive sorgularını çalıştırabilen birkaç küme türü ve teknoloji vardır. HDInsight kümenizi oluştururken, iş yükünüz için performansı optimize etmeye yardımcı olmak için uygun küme türünü seçin.

Örneğin, geçici, etkileşimli sorgular için en iyi duruma getirmek için **Etkileşimli Sorgu** küme türünü seçin. Toplu işlem olarak kullanılan Hive sorguları için en iyi duruma getirmek için Apache **Hadoop** küme türünü seçin. **Spark** ve **HBase** küme türleri de Kovan sorguları çalıştırabilirsiniz. Çeşitli HDInsight küme türlerinde Hive sorgularını çalıştırma hakkında daha fazla bilgi için Azure [HDInsight'ta Apache Hive ve HiveQL nedir?](hadoop/hdinsight-use-hive.md)

Hadoop küme türü HDInsight kümeleri varsayılan olarak performans için optimize edilemez. Bu makalede, sorgularınıza uygulayabileceğiniz en yaygın Hive performans optimizasyonu yöntemlerinden bazıları açıklanmaktadır.

## <a name="scale-out-worker-nodes"></a>İşçi düğümlerini ölçeklendir

BIR HDInsight kümesindeki çalışan düğüm lerinin sayısını artırmak, çalışmanın daha fazla haritalayıcı ve azaltıcının paralel olarak çalıştırılmasına olanak tanır. HDInsight'ta ölçeği artırmanın iki yolu vardır:

* Bir küme oluşturduğunuzda, Azure portalı, Azure PowerShell veya komut satırı arabirimini kullanarak çalışan düğümlerinin sayısını belirtebilirsiniz.  Daha fazla bilgi için bkz. [HDInsight kümesi oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Aşağıdaki ekran görüntüsü, Azure portalındaki alt düğüm yapılandırmasını gösterir:
  
    ![Azure portal küme boyutu düğümleri](./media/hdinsight-hadoop-optimize-hive-query/azure-portal-cluster-configuration.png "scaleout_1")

* Oluşturmadan sonra, bir kümeyi yeniden oluşturmadan daha da ölçeklendirmek için alt düğüm sayısını da edinebilirsiniz:

    ![Azure portal ölçeği küme boyutu](./media/hdinsight-hadoop-optimize-hive-query/azure-portal-settings-nodes.png "scaleout_2")

HDInsight'ı ölçeklendirme hakkında daha fazla bilgi için [Bkz. Ölçek HDInsight kümeleri](hdinsight-scaling-best-practices.md)

## <a name="use-apache-tez-instead-of-map-reduce"></a>Harita Küçültme yerine Apache Tez kullanın

[Apache Tez](https://tez.apache.org/) MapReduce motoru için alternatif bir yürütme motorudur. Linux tabanlı HDInsight kümeleri Tez varsayılan olarak etkin.

![HDInsight Apache Tez genel bakış diyagramı](./media/hdinsight-hadoop-optimize-hive-query/hdinsight-tez-engine.png)

Tez daha hızlıçünkü:

* **MapReduce motorunda tek bir iş olarak Yönlendirilmiş Asiklik Grafik (DAG) çalıştırın.** DAG, her mapper setinin bir dizi redüktör tarafından takip edilmesini gerektirir. Bu, her Hive sorgusu için birden çok MapReduce işinin kapatılmasına neden olur. Tez böyle bir kısıtlama yok ve böylece iş başlangıç yükü en aza indirmek bir iş olarak karmaşık DAG işleyebilir.
* **Gereksiz yazmalardan kaçınır.** MapReduce motorunda aynı Hive sorgusunu işlemek için birden çok iş kullanılır. Her MapReduce işinin çıktısı ara veriler için HDFS'ye yazılır. Tez her Hive sorgusu için iş sayısını en aza indirdiğinden, gereksiz yazmalardan kaçınabilir.
* **Başlatma gecikmelerini en aza indirir.** Tez, başlaması gereken haritacı sayısını azaltarak ve aynı zamanda optimizasyonu geliştirerek başlangıç gecikmesini en aza indirebilir.
* **Kapsayıcıları yeniden kullanır.** Tez mümkün olduğunda konteynerlerin başlatılmasından kaynaklanan gecikme nin azaltılmasını sağlamak için kapları yeniden kullanabilir.
* **Sürekli optimizasyon teknikleri.** Geleneksel optimizasyon derleme aşamasında yapıldı. Ancak, çalışma süresi boyunca daha iyi optimizasyon sağlayan girişler hakkında daha fazla bilgi kullanılabilir. Tez, planı çalışma süresi aşamasına kadar optimize etmesini sağlayan sürekli optimizasyon teknikleri kullanır.

Bu kavramlar hakkında daha fazla bilgi için [bkz.](https://tez.apache.org/)

Aşağıdaki set komutu ile sorgu önreleyerek herhangi bir Hive sorgusu Tez etkin yapabilirsiniz:

```hive
set hive.execution.engine=tez;
```

## <a name="hive-partitioning"></a>Kovan bölümleme

G/Ç işlemleri, Hive sorgularını çalıştırmak için en önemli performans darboğazlarıdır. Okunması gereken veri miktarı azaltılabilirse performans artırılabilir. Varsayılan olarak, Hive sorguları tüm Hive tabloları tarayın. Ancak yalnızca az miktarda veriyi taraması gereken sorgular için (örneğin, filtreleme içeren sorgular), bu davranış gereksiz ek yükü oluşturur. Kovan bölümleme Hive sorguları Hive tablolarda veri yalnızca gerekli miktarda erişmek için izin verir.

Kovan bölümleme, ham verileri yeni dizinler halinde yeniden düzenleyerek uygulanır. Her bölümün kendi dosya dizini vardır. Bölümleme kullanıcı tarafından tanımlanır. Aşağıdaki diyagram, bir Kovan tablosunun *yıl*sütununa bölünmesini göstermektedir. Her yıl için yeni bir dizin oluşturulur.

![HDInsight Apache Hive bölümleme](./media/hdinsight-hadoop-optimize-hive-query/hdinsight-partitioning.png)

Bazı bölümleme hususlar:

* **Bölüm altında yapmayın** - Yalnızca birkaç değere sahip sütunlarda bölümleme birkaç bölüme neden olabilir. Örneğin, cinsiyet bölümleme yalnızca oluşturulacak iki bölüm oluşturur (erkek ve kadın), böylece gecikme süresini en fazla yarı yarıya azaltır.
* **Bölme üzerinde yapmayın** - Diğer aşırı, benzersiz bir değer (örneğin, userid) ile bir sütun üzerinde bir bölüm oluşturma birden çok bölüm neden olur. Çok sayıda dizin işlemek zorunda olduğu için, bölme üzerinde çok fazla strese neden olur.
* **Veri eğriltme kaçının** - Tüm bölümlerin eşit boyut olması için bölümleme anahtarınızı akıllıca seçin. Örneğin, *Durum* sütununda bölümleme veri dağılımı çarpık olabilir. Kaliforniya eyaleti Vermont neredeyse 30x bir nüfusa sahip olduğundan, bölüm boyutu potansiyel çarpık ve performans muazzam değişebilir.

Bölüm tablosu oluşturmak için *Bölümlenmiş* yan tümceyi kullanın:

```sql
CREATE TABLE lineitem_part
      (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
      L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
      L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
      L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
      L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT STRING)
PARTITIONED BY(L_SHIPDATE STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
STORED AS TEXTFILE;
```

Bölümlenmiş tablo oluşturulduktan sonra, statik bölümleme veya dinamik bölümleme oluşturabilirsiniz.

* **Statik bölümleme,** uygun dizinlerde zaten veri kırık olduğu anlamına gelir. Statik bölümlerle, dizin konumuna göre Kovan bölümlerini el ile eklersiniz. Aşağıdaki kod parçacığı bir örnektir.
  
   ```sql
   INSERT OVERWRITE TABLE lineitem_part
   PARTITION (L_SHIPDATE = '5/23/1996 12:00:00 AM')
   SELECT * FROM lineitem
   WHERE lineitem.L_SHIPDATE = '5/23/1996 12:00:00 AM'

   ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = '5/23/1996 12:00:00 AM')
   LOCATION 'wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
   ```

* **Dinamik bölümleme,** Hive'ın sizin için otomatik olarak bölüm oluşturmasını istediğiniz anlamına gelir. Bölme tablosunu evreleme tablosundan zaten oluşturduğunuzdan, tek yapmanız gereken bölümlenmiş tabloya veri eklemektir:
  
   ```hive
   SET hive.exec.dynamic.partition = true;
   SET hive.exec.dynamic.partition.mode = nonstrict;
   INSERT INTO TABLE lineitem_part
   PARTITION (L_SHIPDATE)
   SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY,
       L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
       L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
       L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as L_RETURNFLAG,
       L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as L_SHIPDATE_PS,
       L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as L_RECEIPTDATE,
       L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as L_SHIPMODE,
       L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;
   ```

Daha fazla bilgi için [Bölümlenmiş Tablolar'a](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables)bakın.

## <a name="use-the-orcfile-format"></a>ORCFile biçimini kullanma

Kovan farklı dosya biçimlerini destekler. Örnek:

* **Metin**: varsayılan dosya biçimi ve çoğu senaryo ile çalışır.
* **Avro**: birlikte çalışabilirlik senaryoları için iyi çalışır.
* **ORC/Parke**: performans için en uygun.

ORC (Optimize Edilmiş Satır Sütun) biçimi, Kovan verilerini depolamanın son derece verimli bir yoludur. Diğer biçimlerle karşılaştırıldığında, ORC'nin aşağıdaki avantajları vardır:

* DateTime ve karmaşık ve yarı yapılandırılmış türleri de dahil olmak üzere karmaşık türleri için destek.
* %70'e kadar sıkıştırma.
* satırları atlama izin veren her 10.000 satır, dizinler.
* çalışma süresi yürütme önemli bir düşüş.

ORC biçimini etkinleştirmek için, önce *ORC olarak depolanan*yan tümcesini içeren bir tablo oluşturursunuz:

```sql
CREATE TABLE lineitem_orc_part
      (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
      L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
      L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
      L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
      L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
PARTITIONED BY(L_SHIPDATE STRING)
STORED AS ORC;
```

Ardından, hazırlama tablosundan ORC tablosuna veri eklersiniz. Örnek:

```sql
INSERT INTO TABLE lineitem_orc
SELECT L_ORDERKEY as L_ORDERKEY,
         L_PARTKEY as L_PARTKEY ,
         L_SUPPKEY as L_SUPPKEY,
         L_LINENUMBER as L_LINENUMBER,
         L_QUANTITY as L_QUANTITY,
         L_EXTENDEDPRICE as L_EXTENDEDPRICE,
         L_DISCOUNT as L_DISCOUNT,
         L_TAX as L_TAX,
         L_RETURNFLAG as L_RETURNFLAG,
         L_LINESTATUS as L_LINESTATUS,
         L_SHIPDATE as L_SHIPDATE,
         L_COMMITDATE as L_COMMITDATE,
         L_RECEIPTDATE as L_RECEIPTDATE,
         L_SHIPINSTRUCT as L_SHIPINSTRUCT,
         L_SHIPMODE as L_SHIPMODE,
         L_COMMENT as L_COMMENT
FROM lineitem;
```

[Apache Hive Language kılavuzunda](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC)ORC formatı hakkında daha fazla bilgi edinebilirsiniz.

## <a name="vectorization"></a>Vektörelleştirme

Vektörelleştirme, Hive'ın bir defada bir satırı işlemek yerine 1024 satırlık bir toplu işlemi birlikte işlemesini sağlar. Daha az iç kodun çalışması gerektiğinden, basit işlemlerin daha hızlı yapıldığı anlamına gelir.

Vektörelleştirme önekinizi aşağıdaki ayarda kovan sorgunuzu etkinleştirmek için:

```hive
set hive.vectorized.execution.enabled = true;
```

Daha fazla bilgi [için, Vektörel sorgu yürütme](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution)bakın.

## <a name="other-optimization-methods"></a>Diğer optimizasyon yöntemleri

Örneğin, göz önünde bulundurabileceğiniz daha fazla optimizasyon yöntemi vardır:

* **Kovalama kovalama:** sorgu performansını en iyi duruma getirmek için büyük veri kümelerini kümeleme veya segmente etme olanağı sağlayan bir teknik.
* **Optimizasyona katılın:** Birleştirmelerin verimliliğini artırmak ve kullanıcı ipuçlarına olan gereksinimi azaltmak için Hive'ın sorgu yürütme planlamasının optimizasyonu. Daha fazla bilgi için [bkz.](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization)
* **Azaltıcıları Artırın.**

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, birkaç ortak Hive sorgu optimizasyonu yöntemleri öğrendim. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight'ta Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight'ta İnteraktif Sorgu'u kullanarak uçuş gecikme verilerini analiz edin](/azure/hdinsight/interactive-query/interactive-query-tutorial-analyze-flight-data)
* [HDInsight'ta Apache Hive kullanarak Twitter verilerini analiz edin](hdinsight-analyze-twitter-data-linux.md)
