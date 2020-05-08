---
title: Azure HDInsight 'ta Apache HBase 'i Apache ambarı ile iyileştirme
description: Apache HBase 'i yapılandırmak ve iyileştirmek için Apache ambarı Web Kullanıcı arabirimini kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: a7da6bc23d797e0e89b2338f446fc850b0fd0577
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82797168"
---
# <a name="optimize-apache-hbase-with-apache-ambari-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache HBase 'i Apache ambarı ile iyileştirme

Apache ambarı, HDInsight kümelerini yönetmek ve izlemek için bir Web arabirimidir. Ambarı Web Kullanıcı arabirimine giriş için bkz. [Apache ambarı Web Kullanıcı arabirimini kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md).

Apache HBase yapılandırması **HBase configs** sekmesinden değiştirilir. Aşağıdaki bölümlerde, HBase performansını etkileyen bazı önemli yapılandırma ayarları açıklanır.

## <a name="set-hbase_heapsize"></a>HBASE_HEAPSIZE ayarla

HBase yığın boyutu, *bölgeye* ve *ana* sunuculara göre megabayt cinsinden kullanılacak maksimum yığın miktarını belirtir. Varsayılan değer 1.000 MB 'tır. Bu değerin küme iş yükü için ayarlanmış olması gerekir.

1. Değiştirmek için, HBase **configs** sekmesindeki `HBASE_HEAPSIZE` **Gelişmiş HBase-env** bölmesine gidin ve ayarı bulun.

1. Varsayılan değeri 5.000 MB olarak değiştirin.

    ![' Apache ambarı HBase belleği heapsize '](./media/optimize-hbase-ambari/ambari-hbase-heapsize.png)

## <a name="optimize-read-heavy-workloads"></a>Okuma ağır iş yüklerini iyileştirme

Okuma ağır iş yüklerinin performansını artırmak için aşağıdaki konfigürasyonlar önemlidir.

### <a name="block-cache-size"></a>Blok önbelleği boyutu

Blok önbelleği, okuma önbelleğidir. Boyutu `hfile.block.cache.size` parametresi tarafından denetlenir. Varsayılan değer, toplam bölge sunucu belleğinin yüzde 40 ' i olan 0,4 ' dir. Blok önbelleği boyutunun ne kadar büyük olması, rastgele okumaların daha hızlı olması olur.

1. Bu parametreyi değiştirmek için, HBase **configs** sekmesindeki **Ayarlar** sekmesine gidin ve ardından **okuma arabelleklerine ayrılan regionserver 'ın%**' i bulun.

    ![Apache HBase bellek bloğu önbellek boyutu](./media/optimize-hbase-ambari/hbase-block-cache-size.png)

1. Değeri değiştirmek için **Düzenle** simgesini seçin.

### <a name="memstore-size"></a>Memstore boyutu

Tüm düzenlemeler, bellek arabelleğinde saklanır, bu, *Memstore*olarak adlandırılır. Bu arabellek, tek bir işlemde diske yazılabilen toplam veri miktarını artırır. Ayrıca son düzenlemelere erişimi de hızlandırır. Memstore boyutu aşağıdaki iki parametre tarafından tanımlanır:

* `hbase.regionserver.global.memstore.UpperLimit`: Alt veri deposu 'nun kullanabileceği en yüksek bölge sunucusu yüzdesini tanımlar.

* `hbase.regionserver.global.memstore.LowerLimit`: Alt veri deposu 'nun kullanabileceği bölge sunucusu için minimum yüzdeyi tanımlar.

Rastgele okumaları iyileştirmek için, Memstore üst ve alt sınırlarını azaltabilirsiniz.

### <a name="number-of-rows-fetched-when-scanning-from-disk"></a>Diskten tarama sırasında getirilen satır sayısı

Bu `hbase.client.scanner.caching` ayar, `next` Yöntem bir tarayıcıda çağrıldığında diskten okunan satır sayısını tanımlar.  Varsayılan değer 100’dür. Sayı arttıkça, istemciden bölge sunucusuna yapılan uzak çağrılar daha az tarama elde edilir. Ancak, bu ayar istemcideki bellek basıncını de artırır.

![Getirilen Apache HBase satır sayısı](./media/optimize-hbase-ambari/hbase-num-rows-fetched.png)

> [!IMPORTANT]  
> Bir tarayıcıdaki sonraki yöntemin çağrılması ile ilgili saatin tarayıcı zaman aşımından daha büyük olması için değeri ayarlamayın. Tarayıcı zaman aşımı süresi, `hbase.regionserver.lease.period` özelliği tarafından tanımlanır.

## <a name="optimize-write-heavy-workloads"></a>Yazma ağır iş yüklerini iyileştirme

Aşağıdaki konfigürasyonlar, yazma ağır iş yüklerinin performansını artırmak için önemlidir.

### <a name="maximum-region-file-size"></a>En büyük bölge dosyası boyutu

HBase verileri *hfile*adlı dahili bir dosya biçiminde depolar. Özelliği `hbase.hregion.max.filesize` , bir bölge için tek bir hfile boyutunu tanımlar.  Bir bölgedeki tüm HFiles değerlerinin bu ayardan büyük olması halinde bölge iki bölgeye ayrılır.

![' Apache HBase HRegion en fazla dosya boyutu '](./media/optimize-hbase-ambari/hbase-hregion-max-filesize.png)

Bölge dosya boyutu ne kadar büyükse, bölme sayısı küçülür. En fazla yazma performansına neden olan bir değeri öğrenmek için dosya boyutunu artırabilirsiniz.

### <a name="avoid-update-blocking"></a>Güncelleştirme engellemeyi önleyin

* Özelliği `hbase.hregion.memstore.flush.size` , memstore 'nin diske boşaltılmakta olduğu boyutu tanımlar. Varsayılan boyut 128 MB 'tır.

* HBase bölgesi blok çarpanı tarafından `hbase.hregion.memstore.block.multiplier`tanımlanır. Varsayılan değer 4'tür. İzin verilen en fazla 8.

* Memstore (`hbase.hregion.memstore.flush.size` * `hbase.hregion.memstore.block.multiplier`) bayt ise, HBase tarafından güncelleştirmeler engellenir.

    Varsayılan Temizleme boyutu ve blok çarpanı değerleri ile, Memstore boyutu 128 * 4 = 512 MB olduğunda güncelleştirmeler engellenir. Güncelleştirme engelleme sayısını azaltmak için değerini artırın `hbase.hregion.memstore.block.multiplier`.

![Apache HBase bölgesi blok çarpanı](./media/optimize-hbase-ambari/hbase-hregion-memstore-block-multiplier.png)

## <a name="define-memstore-size"></a>Memstore boyutunu tanımla

Memstore boyutu `hbase.regionserver.global.memstore.UpperLimit` ve `hbase.regionserver.global.memstore.LowerLimit` parametreleri tarafından tanımlanır. Bu değerlerin birbirlerine eşit ayarlanması yazma sırasında duraklar azaltır (Ayrıca daha sık temizlemeye devam ediyor) ve daha fazla yazma performansı elde edin.

## <a name="set-memstore-local-allocation-buffer"></a>Memstore yerel ayırma arabelleğini ayarla

Memstore yerel ayırma arabelleği kullanımı, özelliği `hbase.hregion.memstore.mslab.enabled`tarafından belirlenir. Etkinleştirildiğinde (true), bu ayar ağır yazma işlemi sırasında yığın parçalanmasını önler. Varsayılan değer true şeklindedir.

![HBase. hregion. memstore. mslab. Enabled](./media/optimize-hbase-ambari/hbase-hregion-memstore-mslab-enabled.png)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerini Apache ambarı Web Kullanıcı arabirimi ile yönetme](hdinsight-hadoop-manage-ambari.md)
* [Apache ambarı REST API](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Kümeleri iyileştirme](./hdinsight-changing-configs-via-ambari.md)
* [Apache Hive iyileştirin](./optimize-hive-ambari.md)
* [Apache Pig 'i iyileştirme](./optimize-pig-ambari.md)
