---
title: Apache HBase için Azure HDInsight Hızlandırılmış Yazma İşlemleri
description: Apache HBase Write Ahead Log'un performansını artırmak için premium yönetilen diskler kullanan Azure HDInsight Hızlandırılmış Yazma özelliğine genel bir bakış sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: 7165bab96d037f6782bc9aa6767cadd9b35f058c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76764608"
---
# <a name="azure-hdinsight-accelerated-writes-for-apache-hbase"></a>Apache HBase için Azure HDInsight Hızlandırılmış Yazma İşlemleri

Bu makalede, Azure HDInsight'taki Apache HBase için **Hızlandırılmış Yazma** özelliği ve yazma performansını artırmak için nasıl etkili bir şekilde kullanılabildiği hakkında arka plan sağlanmaktadır. **Hızlandırılmış Yazmalar,** Apache HBase Write Ahead Log'un (WAL) performansını artırmak için [Azure premium SSD yönetilen diskleri](../../virtual-machines/linux/disks-types.md#premium-ssd) kullanır. Apache HBase hakkında daha fazla bilgi edinmek için [HDInsight'ta Apache HBase nedir'e](apache-hbase-overview.md)bakın.

## <a name="overview-of-hbase-architecture"></a>HBase mimarisine genel bakış

HBase'de, **bir satır** bir veya daha fazla **sütundan** oluşur ve bir satır **tuşu**ile tanımlanır. Birden çok satır bir **tablo**yutabilir. Sütunlar, o sütundaki değerin zaman damgalı sürümleri olan **hücreler**içerir. Sütunlar sütun **ailelerine**gruplandırılır ve sütun ailesindeki tüm sütunlar **HFiles**adı verilen depolama dosyalarında birlikte depolanır.

HBase'deki **bölgeler** veri işleme yükünü dengelemek için kullanılır. HBase önce tablo satırlarını tek bir bölgede depolar. Tablodaki veri miktarı arttıkça satırlar birden çok bölgeye yayılır. **Bölge Sunucuları,** birden çok bölge için istekleri işleyebilir.

## <a name="write-ahead-log-for-apache-hbase"></a>Apache HBase için İleri Günlük Yaz

HBase önce veri güncelleştirmelerini Write Ahead Log (WAL) adı verilen bir işleme günlüğü türüne yazar. Güncelleme WAL'da depolandıktan sonra, bellekteki **MemStore'a**yazılır. Bellekteki veriler maksimum kapasitesine ulaştığında, diske **HFile**olarak yazılır.

Bir **RegionServer** MemStore temizlenmeden önce çöker veya kullanılamaz hale gelirse, güncelleştirmeleri yeniden oynatmak için Önceden Yaz Günlüğü kullanılabilir. WAL olmadan, bir **RegionServer** bir **HFile**güncellemeleri yıkama önce çökerse, tüm bu güncelleştirmeleri kaybolur.

## <a name="accelerated-writes-feature-in-azure-hdinsight-for-apache-hbase"></a>Apache HBase için Azure HDInsight'ta Hızlandırılmış Yazma özelliği

Hızlandırılmış Yazma özelliği, bulut depolama alanında bulunan Write Ahead Logs'u kullanarak kaynaklanan daha yüksek yazma gecikmeleri sorununu çözer.  HDInsight Apache HBase kümeleri için Hızlandırılmış Yazma özelliği, her RegionServer'a (işçi düğümü) premium SSD yönetilen diskler bağlar. Write Ahead Günlükleri daha sonra bulut depolama yerine bu premium yönetilen disklere monte edilmiş Hadoop Dosya Sistemi'ne (HDFS) yazılır.  Premium yönetilen diskler Katı Hal Diskleri (SSD' ler) kullanır ve hata toleransı ile mükemmel G/Ç performansı sunar.  Yönetilmeyen disklerin aksine, bir depolama birimi çökerse, aynı kullanılabilirlik kümesindeki diğer depolama birimlerini etkilemez.  Sonuç olarak, yönetilen diskler uygulamalarınız için düşük yazma gecikmesi ve daha iyi esneklik sağlar. Azure tarafından yönetilen diskler hakkında daha fazla bilgi edinmek [için Azure'a Giriş yönetilen diskler'e](../../virtual-machines/windows/managed-disks-overview.md)bakın.

## <a name="how-to-enable-accelerated-writes-for-hbase-in-hdinsight"></a>HDInsight HBase için Hızlandırılmış Yazma nasıl etkinleştirilir

Hızlandırılmış Yazma özelliğine sahip yeni bir HBase kümesi oluşturmak için, **Adım 3, Depolama'ya**ulaşana kadar [HDInsight'ta kümeleri ayarlama](../hdinsight-hadoop-provision-linux-clusters.md) adımlarını izleyin. **Metastore Ayarları**altında, **HBase hızlandırılmış yazmayı etkinleştirmek**için yanındaki onay kutusunu seçin. Ardından, küme oluşturma için kalan adımları ile devam edin.

![HDInsight Apache HBase için hızlandırılmış yazma seçeneğini etkinleştirme](./media/apache-hbase-accelerated-writes/azure-portal-cluster-storage-hbase.png)

## <a name="other-considerations"></a>Diğer konular

Veri dayanıklılığını korumak için, en az üç alt düğümiçeren bir küme oluşturun. Oluşturulduktan sonra, kümeyi üç alt düğümden daha az boyuta ölçeklendiremezsiniz.

Kümeyi silmeden önce HBase tablolarınızı temize veya devre dışı bırakın, böylece İleriye Yaz Günlüğü verilerini kaybetmeyin.

```
flush 'mytable'
```

```
disable 'mytable'
```

Kümenizi ölçeklerken benzer adımları izleyin: tablolarınızı temize ve gelen verileri durdurmak için tablolarınızı devre dışı bırakın. Kümenizi üç düğümden daha az boyuta küçültemezsiniz.

Bu adımları izleyerek başarılı bir ölçeklendirme sağlayacak ve az çoğaltılmış veya geçici dosyalar nedeniyle bir ad düğümünün güvenli moda gitme olasılığını önleyecektir.

Ad düğümünüz bir küçültüldükten sonra güvenli modda yer alıyorsa, çoğaltılan blokları yeniden çoğaltmak ve hdf'leri güvenli moddan çıkarmak için HDFS komutlarını kullanın. Bu yeniden çoğaltma, HBase'i başarıyla yeniden başlatmanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [İleri Ye sil özelliğindeki](https://hbase.apache.org/book.html#wal) Resmi Apache HBase belgeleri
* Hızlandırılmış Yazmaları kullanmak için HDInsight Apache HBase kümenizi yükseltmek [için](apache-hbase-migrate-new-version.md)bkz.
