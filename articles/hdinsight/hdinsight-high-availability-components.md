---
title: Azure HDInsight'ta yüksek kullanılabilirlik bileşenleri
description: HDInsight kümeleri tarafından kullanılan çeşitli yüksek kullanılabilirlik bileşenlerine genel bakış.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/11/2019
ms.openlocfilehash: 38fb45fd339b5e2c7cab6f66a1ed6c0df73fb29e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74069620"
---
# <a name="high-availability-services-supported-by-azure-hdinsight"></a>Azure HDInsight tarafından desteklenen yüksek kullanılabilirlik hizmetleri

 HdInsight, analitik bileşenleriniz için en uygun düzeyde kullanılabilirlik sağlamak amacıyla, kritik hizmetlerin yüksek kullanılabilirliğini (HA) sağlamak için benzersiz bir mimariyle geliştirilmiştir. Bu mimarinin bazı bileşenleri Microsoft tarafından otomatik hata sağlamak için geliştirilmiştir. Diğer bileşenler, belirli hizmetleri desteklemek için dağıtılan standart Apache bileşenleridir. Bu makalede, HDInsight'taki HA hizmet modelinin mimarisi, HDInsight'ın HA hizmetleri için başarısızlığı nasıl desteklediği ve diğer hizmet kesintilerinden kurtarılabilmek için en iyi uygulamalar açıklanmaktadır.

## <a name="high-availability-infrastructure"></a>Yüksek kullanılabilirlik altyapısı

HDInsight, otomatik arıza özellikleriyle dört ana hizmetin yüksek kullanılabilirlik olmasını sağlamak için özelleştirilmiş altyapı sağlar:

- Apache Ambari sunucusu
- Apache YARN için Uygulama Zaman Çizelgesi Sunucusu
- Hadoop MapReduce için İş Geçmişi Sunucusu
- Apaçi Livy

Bu altyapı, bazıları Microsoft tarafından tasarlanan bir dizi hizmet ve yazılım bileşeninden oluşur. Aşağıdaki bileşenler HDInsight platformuna özgüdür:

- Slave failover denetleyicisi
- Ana failover denetleyicisi
- Slave yüksek kullanılabilirlik hizmeti
- Master yüksek kullanılabilirlik hizmeti

![yüksek kullanılabilirlik altyapısı](./media/hdinsight-high-availability-components/high-availability-architecture.png)

Açık kaynak Apache güvenilirlik bileşenleri tarafından desteklenen diğer yüksek kullanılabilirlik hizmetleri de vardır. Bu bileşenler HDInsight kümelerinde de bulunur:

- Hadoop Dosya Sistemi (HDFS) NameNode
- İplik Kaynak Yöneticisi
- HBase Ustası

Aşağıdaki bölümlerde bu hizmetlerin birlikte nasıl çalıştığı hakkında daha fazla ayrıntı verecektir.

## <a name="hdinsight-high-availability-services"></a>HDInsight yüksek kullanılabilirlik hizmetleri

Microsoft, HDInsight kümelerinde aşağıdaki tabloda dört Apache hizmeti için destek sağlar. Apache bileşenleri tarafından desteklenen yüksek kullanılabilirlik hizmetleri onları ayırt etmek için, *hdInsight HA hizmetleri*denir.

| Hizmet | Küme düğümleri | Küme türleri | Amaç |
|---|---|---|---|
| Apache Ambari sunucusu| Aktif kafa düğümü | Tümü | Kümeyi izler ve yönetir.|
| Apache YARN için Uygulama Zaman Çizelgesi Sunucusu | Aktif kafa düğümü | Kafka hariç hepsi | Kümeüzerinde çalışan İplik işleri hakkında hata ayıklama bilgilerini tutar.|
| Hadoop MapReduce için İş Geçmişi Sunucusu | Aktif kafa düğümü | Kafka hariç hepsi | MapReduce işleri için hata ayıklama verilerini korur.|
| Apaçi Livy | Aktif kafa düğümü | Spark | REST arabirimi üzerinden bir Spark kümesiyle kolay etkileşim sağlar |

>[!Note]
> HDInsight Kurumsal Güvenlik Paketi (ESP) kümeleri şu anda yalnızca Ambari sunucusuna yüksek kullanılabilirlik sağlar.

### <a name="architecture"></a>Mimari

Her HDInsight kümesinin sırasıyla etkin ve bekleme modunda iki headnode'si vardır. HDInsight HA hizmetleri yalnızca kafa düğümlerinde çalışır. Bu hizmetler her zaman etkin headnode çalışıyor olmalı ve durdu ve bekleme headnode bakım modu koymak.

HDInsight, HA hizmetlerinin doğru durumlarını korumak ve hızlı bir başarısızlık sağlamak için, dağıtılmış uygulamalar için bir koordinasyon hizmeti olan Apache ZooKeeper'ı etkin headnode seçimi yapmak için kullanır. HDInsight ayrıca, HDInsight HA hizmetleri için başarısız prosedürü koordine eden birkaç arka plan Java işlemini de uygular. Bu hizmetler şunlardır: ana failover denetleyicisi, köle failover denetleyicisi, *master-ha-service*, ve *slave-ha-service*.

### <a name="apache-zookeeper"></a>Apaçi ZooKeeper

Apache ZooKeeper dağıtılmış uygulamalar için yüksek performanslı bir koordinasyon hizmetidir. Üretimde, ZooKeeper genellikle zookeeper sunucularının çoğaltılmış bir grup bir çoğunluk oluşturmak çoğaltılmış modunda çalışır. Her HDInsight kümesi, üç ZooKeeper sunucusunun bir çoğunluk oluşturmasına izin veren üç ZooKeeper düğümüne sahiptir. HDInsight'ın birbirine paralel çalışan iki ZooKeeper çoğunluğu vardır. Bir çoğunluk, HDInsight HA hizmetlerinin çalışması gereken bir kümedeki etkin headnode karar verir. Apache tarafından sağlanan HA hizmetlerini daha sonraki bölümlerde ayrıntılı olarak koordine etmek için başka bir çoğunluk kullanılır.

### <a name="slave-failover-controller"></a>Slave failover denetleyicisi

Slave failover denetleyicisi bir HDInsight kümesindeki her düğümüzerinde çalışır. Bu denetleyici, ambari ajanını ve her düğümde *köle-ha-hizmetini* başlatmaktan sorumludur. Aktif başlık hakkında ilk ZooKeeper çoğunluklarını düzenli olarak sorgular. Etkin ve bekleme headnodes değiştiğinde, köle failover denetleyiciaşağıdakileri gerçekleştirir:

1. Ana bilgisayar yapılandırma dosyasını güncelleştirir.
1. Ambari ajanını yeniden başlatın.

*Slave-ha-service,* bekleme başlığındaki HDInsight HA hizmetlerini (Ambari sunucusu hariç) durdurmakla yükümlüdür.

### <a name="master-failover-controller"></a>Ana failover denetleyicisi

Ana bir arıza denetleyicisi her iki headnode çalışır. Her iki ana failover denetleyicileri aktif headnode olarak çalışan headnode aday çıkarmak için ilk ZooKeeper çoğunluk ile iletişim.

Örneğin, headnode 0'daki ana başarısız denetleyici seçimi kazanırsa, aşağıdaki değişiklikler gerçekleşir:

1. Headnode 0 etkin hale gelir.
1. Ana failover denetleyicisi Ambari sunucusunu ve headnode 0'daki *master-ha-servisi* ni başlatır.
1. Diğer ana failover denetleyicisi Ambari sunucusunu ve *ana-ha-servisi* headnode 1'de durdurur.

Master-ha-service yalnızca etkin headnode çalışır, hdinsight HA hizmetlerini (Ambari sunucusu hariç) bekleme başlığında durdurur ve bunları etkin başlıkla başlatır.

### <a name="the-failover-process"></a>Başarısız süreci

![başarısız süreci](./media/hdinsight-high-availability-components/failover-steps.png)

Zookeeper çoğunluk için hearbeat bildirimler göndermek için ana failover denetleyicisi ile birlikte her headnode üzerinde çalışan bir sağlık monitörü çalışır. Headnode bu senaryoda bir HA hizmeti olarak kabul edilir. Sağlık monitörü, her yüksek kullanılabilirlik hizmetinin sağlıklı olup olmadığını ve liderlik seçimlerine katılmaya hazır olup olmadığını kontrol eder. Eğer evet sayılsa, bu kafa düğümü seçimlerde yarışacak. Yoksa, tekrar hazır olana kadar seçimden çıkacak.

Bekleme başlığı hiç liderlik elde ve aktif hale gelirse (önceki etkin düğüm ile bir başarısızlık durumunda gibi), onun ana failover denetleyicisi tüm HDInsight HA hizmetleri başlatacaktır. Ana failover denetleyicisi de diğer headnode bu hizmetleri durdurur.

Bir hizmetin kötü veya sağlıksız olması gibi HDInsight HA hizmet hataları için, ana arıza denetleyicisi headnode durumuna göre hizmetleri otomatik olarak yeniden başlatmalı veya durdurmalıdır. Kullanıcılar hdinsight HA hizmetlerini her iki kafa düğümünde de el ile başlatmamalıdır. Bunun yerine, hizmetin kurtarılmasına yardımcı olmak için otomatik veya el ile başarısız olun.

### <a name="inadvertent-manual-intervention"></a>Yanlışlıkla manuel müdahale

HDInsight HA hizmetleri yalnızca etkin başlık üzerinde çalışmalıdır ve gerektiğinde otomatik olarak yeniden başlatılır. Bireysel HA hizmetlerinin kendi sağlık monitörü olmadığından, failover bireysel hizmet düzeyinde tetiklenemez. Failover, hizmet düzeyinde değil, düğüm düzeyinde sağlanır.

### <a name="some-known-issues"></a>Bilinen bazı sorunlar

- Bekleme başlığında bir HA hizmetini el ile başlatırken, bir sonraki başarısız olana kadar durmaz. HA hizmetleri her iki headnodes üzerinde çalışan zaman, bazı potansiyel sorunlar şunlardır: Ambari UI erişilemez, Ambari hataları atar, İplik, Kıvılcım, ve Oozie işleri sıkışmış alabilirsiniz.

- Etkin headnode bir HA hizmeti durduğunda, bir sonraki failover gerçekleşene veya ana failover denetleyicisi/master-ha-service yeniden başlatAna gelene kadar yeniden başlatılamayacak. Bir veya daha fazla HA hizmeti aktif başlıkta durduğunda, özellikle Ambari sunucusu durduğunda Ambari UI'ye erişilemez, diğer olası sorunlar arasında İplik, Kıvılcım ve Oozie iş hataları yer alır.

## <a name="apache-high-availability-services"></a>Apache yüksek kullanılabilirlik hizmetleri

Apache, HDInsight kümelerinde de bulunan HDFS NameNode, YARN ResourceManager ve HBase Master için yüksek kullanılabilirlik sağlar. HDInsight HA hizmetlerinin aksine, ESP kümelerinde desteklenirler. Apache HA hizmetleri aktif/bekleme durumlarını seçmek ve otomatik arıza yapmak için ikinci ZooKeeper çoğunluğuyla (yukarıdaki bölümde açıklanan) iletişim kurar. Aşağıdaki bölümlerde bu hizmetlerin nasıl çalıştığı ayrıntılı olarak açıklanmıştır.

### <a name="hadoop-distributed-file-system-hdfs-namenode"></a>Hadoop Dağıtılmış Dosya Sistemi (HDFS) NameNode

Apache Hadoop 2.0 veya üzeri tabanlı HDInsight kümeleri NameNode'de yüksek kullanılabilirlik sağlar. Headnodes üzerinde çalışan iki NameNodes vardır, otomatik failover için yapılandırılmıştır. NameNodes aktif / bekleme durumu seçmek için Zookeeper ile iletişim kurmak için *ZKFailoverController* kullanın. *ZKFailoverController* her iki headnode üzerinde çalışır ve yukarıdaki ana failover denetleyicisi ile aynı şekilde çalışır.

İkinci Zookeeper çoğunluk ilk çoğunluk bağımsızdır, bu nedenle etkin NameNode etkin headnode üzerinde çalışmayabilir. Etkin NameNode ölü veya sağlıksız olduğunda, bekleme namenode seçimi kazanır ve etkin hale gelir.

### <a name="yarn-resourcemanager"></a>İplik Kaynak Yöneticisi

Apache Hadoop 2.4 veya üzeri tabanlı HDInsight kümeleri, IPN ResourceManager'ın yüksek kullanılabilirliğini destekler. Sırasıyla headnode 0 ve headnode 1 üzerinde çalışan iki ResourceManagers, rm1 ve rm2 vardır. NameNode gibi, YARN ResourceManager da otomatik failover için yapılandırılmıştır. Geçerli etkin Kaynak Yöneticisi küçüldüğünde veya yanıt vermediğinde başka bir Kaynak Yöneticisi otomatik olarak etkin olarak seçilir.

YARN ResourceManager bir hata dedektörü ve lider elektörü olarak gömülü *ActiveStandbyElector* kullanır. HDFS NameNode aksine, YARN ResourceManager ayrı bir ZKFC daemon gerekmez. Etkin ResourceManager durumlarını Apache Zookeeper'a yazar.

İPLik Kaynak Yöneticisi'nin yüksek kullanılabilirliği NameNode ve diğer HDInsight HA hizmetlerinden bağımsızdır. Etkin ResourceManager etkin headnode veya etkin NameNode çalışan headnode üzerinde çalışmayabilir. YARN ResourceManager yüksek kullanılabilirliği hakkında daha fazla bilgi için Kaynak [Yöneticisi Yüksek Kullanılabilirlik'e](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/ResourceManagerHA.html)bakın.

### <a name="hbase-master"></a>HBase Ustası

HDInsight HBase kümeleri HBase Master yüksek kullanılabilirliğini destekler. Headnodes üzerinde çalışan diğer HA hizmetlerinin aksine, HBase Masters üç Zookeeper düğümleri üzerinde çalıştırın, bunlardan biri aktif ana ve diğer iki bekleme vardır. NameNode gibi, HBase Master lider seçimi için Apache Zookeeper ile koordine ve mevcut aktif master sorunları olduğunda otomatik failover yok. Herhangi bir zamanda yalnızca bir etkin HBase Master vardır.

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight'ta Apache Hadoop kümelerinin kullanılabilirliği ve güvenilirliği](hdinsight-high-availability-linux.md)
- [Azure HDInsight sanal ağ mimarisi](hdinsight-virtual-network-architecture.md)
