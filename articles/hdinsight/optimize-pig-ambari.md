---
title: Apache Pig 'i Azure HDInsight 'ta Apache ambarı ile iyileştirme
description: Apache Pig 'yi yapılandırmak ve iyileştirmek için Apache ambarı Web Kullanıcı arabirimini kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 469019eb1e90654d1953156337593d5de99b46c0
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82796687"
---
# <a name="optimize-apache-pig-with-apache-ambari-in-azure-hdinsight"></a>Apache Pig 'i Azure HDInsight 'ta Apache ambarı ile iyileştirme

Apache ambarı, HDInsight kümelerini yönetmek ve izlemek için bir Web arabirimidir. Ambarı Web Kullanıcı arabirimine giriş için bkz. [Apache ambarı Web Kullanıcı arabirimini kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md).

Apache Pig özellikleri, Pig sorgularını ayarlamak için, ambarı Web kullanıcı arabiriminden değiştirilebilir. Pig özelliklerinin doğrudan ambarı içinden değiştirilmesi, `/etc/pig/2.4.2.0-258.0/pig.properties` dosyadaki Pig özelliklerini doğrudan değiştirir.

1. Pig özelliklerini değiştirmek için Pig **configs** sekmesine gidin ve ardından **Gelişmiş Pig-Özellikler** bölmesini genişletin.

1. Değiştirmek istediğiniz özelliğin değerini bulun, açıklama kaldırın ve değiştirin.

1. Yeni değeri kaydetmek için pencerenin sağ üst tarafındaki **Kaydet** ' i seçin. Bazı özellikler, bir hizmetin yeniden başlatılmasını gerektirebilir.

    ![Gelişmiş Apache Pig özellikleri](./media/optimize-pig-ambari/advanced-pig-properties.png)

> [!NOTE]  
> Herhangi bir oturum düzeyi ayarı, `pig.properties` dosyadaki özellik değerlerini geçersiz kılar.

## <a name="tune-execution-engine"></a>Yürütme altyapısını ayarla

Pig betiklerini yürütmek için iki yürütme altyapısı mevcuttur: MapReduce ve tez. Tez, iyileştirilmiş bir altyapıdır ve MapReduce 'den çok daha hızlıdır.

1. Yürütme altyapısını değiştirmek için, **Gelişmiş Pig-Özellikler** bölmesinde özelliği `exectype`bulun.

1. Varsayılan değer **MapReduce**' dir. **Tez**olarak değiştirin.

## <a name="enable-local-mode"></a>Yerel modu etkinleştir

Hive ile benzer şekilde, yerel mod, işleri görece daha az miktarda veri ile hızlandırmak için kullanılır.

1. Yerel modu etkinleştirmek için **true**olarak ayarlayın `pig.auto.local.enabled` . Varsayılan değer false'tur.

1. Giriş veri boyutu `pig.auto.local.input.maxbytes` özellik değerinden küçük olan işlerin küçük işler olduğu kabul edilir. Varsayılan değer 1 GB 'tır.

## <a name="copy-user-jar-cache"></a>Kullanıcı jar önbelleğini Kopyala

Pig, UDF 'ler için gereken JAR dosyalarını görev düğümleri için kullanılabilir hale getirmek üzere dağıtılmış bir önbelleğe kopyalar. Bu jliler sıklıkla değişmez. Etkinleştirilirse, bu `pig.user.cache.enabled` ayar jar dosyaları dışındaki 'ın aynı kullanıcı tarafından çalıştırılan işler için yeniden kullanmak üzere bir önbelleğe yerleştirilmesine izin verir. Bu ayar, İş performansının küçük bir artışına neden olur.

1. Özelliğini etkinleştirmek için true `pig.user.cache.enabled` olarak ayarlayın. Varsayılan değer false.

1. Önbelleğe alınan jars 'ın temel yolunu ayarlamak için taban yoluna ayarlayın `pig.user.cache.location` . Varsayılan değer: `/tmp`.

## <a name="optimize-performance-with-memory-settings"></a>Bellek ayarlarıyla performansı iyileştirin

Aşağıdaki bellek ayarları Pig betiği performansını iyileştirmenize yardımcı olabilir.

* `pig.cachedbag.memusage`: Bir paket için verilen bellek miktarı. Bir paket, tanımlama grupları koleksiyonudur. Kayıt düzeni sıralı bir alan kümesidir ve bir alan veri parçasıdır. Bir paket içindeki veriler, belirtilen belleğin ötesinde, diske taşılır. Varsayılan değer, kullanılabilir belleğin yüzde 20 ' sini temsil eden 0,2 ' dir. Bu bellek, bir uygulamadaki tüm baorda paylaşılır.

* `pig.spill.size.threshold`: Bu taşma boyutundan (bayt cinsinden) daha büyük olan paketler diske taşılır. Varsayılan değer 5 MB 'tır.

## <a name="compress-temporary-files"></a>Geçici dosyaları sıkıştır

Pig, iş yürütülürken geçici dosyalar oluşturur. Dosyaları okurken veya diske yazarken geçici dosyaları sıkıştırmak performans artışına neden olur. Geçici dosyaları sıkıştırmak için aşağıdaki ayarlar kullanılabilir.

* `pig.tmpfilecompression`: Doğru olduğunda geçici dosya sıkıştırmasını etkinleştirilir. Varsayılan değer false'tur.

* `pig.tmpfilecompression.codec`: Geçici dosyaları sıkıştırmak için kullanılacak sıkıştırma codec bileşeni. Önerilen sıkıştırma codec bileşenleri, daha düşük CPU kullanımı için LZO ve Snappy ' dir.

## <a name="enable-split-combining"></a>Bölünmüş birleştirmeyi etkinleştir

Etkinleştirildiğinde, küçük dosyalar daha az eşleme görevi için birleştirilir. Bu ayar birçok küçük dosya içeren işlerin verimliliğini artırır. Özelliğini etkinleştirmek için true `pig.noSplitCombination` olarak ayarlayın. Varsayılan değer false'tur.

## <a name="tune-mappers"></a>Mapto ayarla

Eşleme sayısı, özelliği `pig.maxCombinedSplitSize`değiştirilerek denetlenir. Bu özellik, tek bir eşleme göreviyle işlenecek verilerin boyutunu belirtir. Varsayılan değer FileSystem 'ın varsayılan blok boyutudur. Bu değeri artırmak, daha az Eşleyici görevi elde ediyor.

## <a name="tune-reducers"></a>Azaltıcının ayarla

Azaltıcının sayısı, parametresine `pig.exec.reducers.bytes.per.reducer`göre hesaplanır. Parametresi, varsayılan 1 GB olarak Reducer başına işlenen bayt sayısını belirtir. En fazla azaltıcının sayısını sınırlandırmak için, `pig.exec.reducers.max` özelliği varsayılan 999 olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerini Apache ambarı Web Kullanıcı arabirimi ile yönetme](hdinsight-hadoop-manage-ambari.md)
* [Apache ambarı REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Kümeleri iyileştirme](./hdinsight-changing-configs-via-ambari.md)
* [Apache HBase 'i iyileştirme](./optimize-hbase-ambari.md)
* [Apache Hive iyileştirin](./optimize-hive-ambari.md)
