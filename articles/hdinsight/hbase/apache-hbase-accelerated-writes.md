---
title: Apache HBase için Azure HDInsight Hızlandırılmış Yazma İşlemleri
description: Apache HBase yazma öncesinde günlüğü performansını geliştirmek için Premium yönetilen diskler kullanan Azure HDInsight hızlandırılmış yazma özelliğine genel bakış sunar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/24/2020
ms.openlocfilehash: 7165bab96d037f6782bc9aa6767cadd9b35f058c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76764608"
---
# <a name="azure-hdinsight-accelerated-writes-for-apache-hbase"></a>Apache HBase için Azure HDInsight Hızlandırılmış Yazma İşlemleri

Bu makalede, Azure HDInsight 'ta Apache HBase için **hızlandırılmamış yazma** özelliği ve yazma performansını geliştirmek için etkin bir şekilde nasıl kullanılabilecek arka plan sağlanmaktadır. **Hızlandırılmış yazma işlemleri** , Apache HBase yazma günlüğü (Wal) performansını geliştirmek için [Azure Premium SSD tarafından yönetilen diskleri](../../virtual-machines/linux/disks-types.md#premium-ssd) kullanır. Apache HBase hakkında daha fazla bilgi edinmek için bkz. [HDInsight 'Ta Apache HBase nedir](apache-hbase-overview.md).

## <a name="overview-of-hbase-architecture"></a>HBase mimarisine genel bakış

HBase 'de, bir **satır** bir veya daha fazla **sütundan** oluşur ve bir **satır anahtarı**tarafından tanımlanır. Birden çok satır bir **tablo**yapar. Sütunlar, bu sütundaki değerin zaman damgamış sürümleri olan **hücreler**içerir. Sütunlar **sütun aileleri**halinde gruplandırılır ve bir sütun ailesindeki tüm sütunlar, **hfiles**adlı depolama dosyalarında birlikte depolanır.

HBase **bölgeleri** , veri işleme yükünü dengelemek için kullanılır. HBase öncelikle bir tablonun satırlarını tek bir bölgede depolar. Tablodaki veri miktarı arttıkça, satırlar birden çok bölgeye yayılır. **Bölge sunucuları** , birden çok bölgeye yönelik istekleri işleyebilir.

## <a name="write-ahead-log-for-apache-hbase"></a>Apache HBase için önceden yazma günlüğü

HBase önce veri güncelleştirmelerini yazma öncesinde günlük (WAL) olarak adlandırılan bir işleme günlüğü türüne yazar. Güncelleştirme WAL 'de depolandıktan sonra, bellek içi **Memstore**'e yazılır. Bellekteki veriler en yüksek kapasiteye ulaştığında, bir **hfile**olarak diske yazılır.

Bir **Regionserver** , memstore temizlenmeden önce kilitlenirse veya kullanılamaz hale gelirse, güncelleştirmeleri yeniden oynatmak Için önceden yazma günlüğü kullanılabilir. WAL olmadan, bir **Regionserver** bir **hfile**güncelleştirmelerini reçeteye göre kilitlenirse, tüm bu güncelleştirmeler kaybedilir.

## <a name="accelerated-writes-feature-in-azure-hdinsight-for-apache-hbase"></a>Apache HBase için Azure HDInsight 'ta hızlandırılmış yazma özelliği

Hızlandırılmış yazma özelliği, bulut depolamadaki ön yazma günlüklerini kullanmanın neden olduğu daha yüksek yazma gecikme süreleriyle ilgili sorunu çözer.  HDInsight Apache HBase kümeleri için hızlandırılmış yazma özelliği, Premium SSD ile yönetilen diskleri her RegionServer 'a (çalışan düğümü) iliştirir. Sonradan yazma günlükleri, bulut depolaması yerine bu Premium yönetilen disklere takılan Hadoop dosya sistemine (SUI) yazılır.  Premium yönetilen diskler, katı hal diskleri (SSD 'Ler) kullanır ve hata toleransı ile mükemmel g/ç performansı sunar.  Yönetilmeyen disklerden farklı olarak, bir depolama birimi kapalıysa, aynı Kullanılabilirlik kümesindeki diğer depolama birimlerini etkilemez.  Sonuç olarak, yönetilen diskler, uygulamalarınız için düşük yazma gecikme süresi ve daha iyi dayanıklılık sağlar. Azure tarafından yönetilen diskler hakkında daha fazla bilgi edinmek için bkz. [Azure yönetilen disklere giriş](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="how-to-enable-accelerated-writes-for-hbase-in-hdinsight"></a>HDInsight 'ta HBase için hızlandırılmış yazma Işlemleri nasıl etkinleştirilir

Hızlandırılmış yazma özelliği ile yeni bir HBase kümesi oluşturmak için, **3. adıma**ulaşana kadar [HDInsight 'ta kümeleri ayarlama](../hdinsight-hadoop-provision-linux-clusters.md) bölümündeki adımları izleyin. **Meta veri deposu ayarları**altında **HBase hızlandırılmış yazmaları etkinleştir**' in yanındaki onay kutusunu işaretleyin. Ardından, küme oluşturma için kalan adımlara devam edin.

![HDInsight Apache HBase için hızlandırılmış yazma seçeneğini etkinleştirme](./media/apache-hbase-accelerated-writes/azure-portal-cluster-storage-hbase.png)

## <a name="other-considerations"></a>Diğer konular

Veri dayanıklılığını korumak için en az üç çalışan düğümü olan bir küme oluşturun. Oluşturulduktan sonra, kümeyi üçten az çalışan düğümden ölçeklendirdirebilirsiniz.

Küme silinmeden önce HBase tablolarınızı boşaltıp veya devre dışı bırakarak, yazma öncesi günlük verilerini kaybetmezsiniz.

```
flush 'mytable'
```

```
disable 'mytable'
```

Kümenizin ölçeğini ölçeklendirirken benzer adımları izleyin: tabloları temizleyin ve gelen verileri durdurmak için tablolarınızı devre dışı bırakın. Kümenizi üçten az düğüme ölçeklendiremez.

Bu adımları takip etmek, başarılı bir ölçeklendirme işlemi sağlar ve bir süs Yot 'ın, alt veya geçici dosyalar nedeniyle güvenli mod olma olasılığını ortadan önler.

Süs Yot ölçeği bir ölçeklendirdikten sonra safemode ' e gidilırsa, doğru çoğaltılan blokları yeniden çoğaltmak ve güvenli moddan yararlanmak için, bu komutu kullanın. Bu yeniden çoğaltma, HBase 'i başarıyla yeniden başlatmanıza olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Öne yazma günlüğü özelliği](https://hbase.apache.org/book.html#wal) hakkında resmi Apache HBase belgeleri
* HDInsight Apache HBase kümenizi hızlandırılmış yazmaları kullanacak şekilde yükseltmek için bkz. [Apache HBase kümesini yeni bir sürüme geçirme](apache-hbase-migrate-new-version.md).
