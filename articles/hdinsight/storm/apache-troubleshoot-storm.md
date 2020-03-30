---
title: Azure HDInsight'ı kullanarak Sorun Giderme Fırtınası
description: Azure HDInsight ile Apache Storm'u kullanma yla ilgili sık sorulan soruların yanıtlarını alın.
keywords: Azure HDInsight, Fırtına, SSS, sorun giderme kılavuzu, sık karşılaşılan sorunlar
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 11/08/2019
ms.custom: seodec18
ms.openlocfilehash: b51b2c21fd9256c93f6947386a48336af2b75d88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79271934"
---
# <a name="troubleshoot-apache-storm-by-using-azure-hdinsight"></a>Azure HDInsight'ı kullanarak Apache Storm sorun giderme

[Apache Ambari'de](https://ambari.apache.org/) [Apache Storm](https://storm.apache.org/) yükleri ile çalışmak için en önemli konular ve onların kararları hakkında bilgi edinin.

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>Bir kümedeki Fırtına UI'ına nasıl erişebilirim?

Bir tarayıcıdan Storm UI'ye erişmek için iki seçeneğiniz var:

### <a name="apache-ambari-ui"></a>Apache Ambari UI

1. Ambari panosuna git.
2. Hizmetler listesinde **Fırtına'yı**seçin.
3. Hızlı **Bağlantılar** menüsünde **Fırtına UI'yi**seçin.

### <a name="direct-link"></a>Doğrudan bağlantı

Fırtına UI'ye aşağıdaki URL'den erişebilirsiniz:

`https://<cluster DNS name>/stormui`

Örnek: `https://stormcluster.azurehdinsight.net/stormui`

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>Storm olay merkezi spout denetim noktası bilgilerini bir topolojiden diğerine nasıl aktarabilirim?

HDInsight Storm olay hub'ı spout .jar dosyasını kullanarak Azure Etkinlik Hub'larından okunan topolojiler geliştirdiğinizde, yeni bir kümede aynı ada sahip bir topoloji dağıtmanız gerekir. Ancak, eski kümeüzerinde [Apache ZooKeeper'a](https://zookeeper.apache.org/) adanmış denetim noktası verilerini saklamanız gerekir.

### <a name="where-checkpoint-data-is-stored"></a>Denetim noktası verilerinin depolandığı yer

Uzaklıklar için denetim noktası verileri ZooKeeper'daki olay merkezi tarafından iki kök yoliçinde depolanır:

- İşlem dışı spout denetim noktaları `/eventhubspout`' nda depolanır.

- İşlemsel spout denetim noktası `/transactional`verileri .

### <a name="how-to-restore"></a>Nasıl geri yüklenir

ZooKeeper'tan veri aktarmak ve verileri yeni bir adla ZooKeeper'a geri aktarmak için kullandığınız komut dosyalarını ve kitaplıkları almak için [HDInsight Storm örneklerine](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0)bakın.

Lib klasöründe dışa aktarma/alma işlemi için uygulama içeren .jar dosyaları vardır. Bash klasöründe, eski kümedeki ZooKeeper sunucusundan veri nasıl dışa aktarılabildiğini ve ardından yeni kümedeki ZooKeeper sunucusuna nasıl aktarılabildiğini gösteren bir örnek komut dosyası vardır.

Veri aktarmak ve sonra veri almak için ZooKeeper düğümlerinden [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) komut dosyası çalıştırın. Komut dosyasını doğru Hortonworks Veri Platformu (HDP) sürümünde güncelleştirin. (Bu komut dosyalarını HDInsight'ta genel hale getirmek için çalışıyoruz. Genel komut dosyaları, kullanıcı tarafından değişiklik yapılmadan kümedeki herhangi bir düğümden çalıştırılabilir.)

Dışa aktarma komutu meta verileri ayarladığınız bir konumda (Azure Blob Depolama veya Azure Veri Gölü Depolaması'nda) Bir Apache Hadoop Dağıtılmış Dosya Sistemi (HDFS) yoluna yazar.

### <a name="examples"></a>Örnekler

#### <a name="export-offset-metadata"></a>Dışa aktarma meta verilerini dışa aktarma

1. Denetim noktası ofsetinin dışa aktarılması gereken kümedeki ZooKeeper kümesine gitmek için SSH'yi kullanın.
2. ZooKeeper ofset verilerini HDFS yoluna aktarmak için aşağıdaki komutu (HDP sürüm dizesini güncelleştirdikten sonra) çalıştırın: `/stormmetadta/zkdata`

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>İçme ofset meta verileri

1. Denetim noktası ofsetinin alınması gereken kümedeki ZooKeeper kümesine gitmek için SSH'yi kullanın.
2. ZooKeeper ofset verilerini HDFS yolundan `/stormmetadata/zkdata` hedef kümedeki ZooKeeper sunucusuna almak için aşağıdaki komutu (HDP sürüm dizesini güncelledikten sonra) çalıştırın:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```

#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Topolojilerin verileri en başından veya kullanıcının seçtiği bir zaman damgasından işlemeye başlayabilmesi için uzaklık meta verilerini silme

1. Denetim noktası ofsetinin silinmesi gereken kümedeki ZooKeeper kümesine gitmek için SSH'yi kullanın.
2. Geçerli kümedeki tüm ZooKeeper ofset verilerini silmek için aşağıdaki komutu (HDP sürüm dizesini güncelleştirdikten sonra) çalıştırın:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Bir kümedeki Fırtına ikililerini nasıl bulurum?

Mevcut HDP destesi için `/usr/hdp/current/storm-client`fırtına ikilileri bulunmaktadır. Konum hem baş düğümleri hem de işçi düğümleri için aynıdır.

/usr/hdp'deki belirli HDP sürümleri için birden fazla `/usr/hdp/2.5.0.1233/storm`ikili olabilir (örneğin, ). Klasör, `/usr/hdp/current/storm-client` kümede çalışan en son sürüme bağlı.

Daha fazla bilgi için, SSH ve [Apache](https://storm.apache.org/) [Storm'u kullanarak BIR HDInsight kümesine bağlan'a](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) bakın.

## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Bir Fırtına kümesinin dağıtım topolojisini nasıl belirleyebilirim?

İlk olarak, HDInsight Storm ile yüklenen tüm bileşenleri tanımlayın. Fırtına kümesi dört düğüm kategorisinden oluşur:

* Ağ geçidi düğümleri
* Kafa düğümleri
* ZooKeeper düğümleri
* İşçi düğümleri

### <a name="gateway-nodes"></a>Ağ geçidi düğümleri

Ağ geçidi düğümü, etkin bir Ambari yönetim hizmetine genel erişim sağlayan bir ağ geçidi ve ters proxy hizmetidir. Parti, Ambari lider seçimlerini de ele alıyor.

### <a name="head-nodes"></a>Kafa düğümleri

Fırtına kafa düğümleri aşağıdaki hizmetleri çalıştırın:
* Nimbus
* Ambari sunucusu
* Ambari Metrics sunucusu
* Ambari Ölçüleri Toplayıcı
 
### <a name="zookeeper-nodes"></a>ZooKeeper düğümleri

HDInsight üç düğümlü ZooKeeper çoğunluğu ile birlikte gelir. Çoğunluk boyutu sabittir ve yeniden yapılandırılamaz.

Kümedeki fırtına hizmetleri, ZooKeeper çoğunluklarını otomatik olarak kullanacak şekilde yapılandırılır.

### <a name="worker-nodes"></a>İşçi düğümleri

Fırtına işçi düğümleri aşağıdaki hizmetleri çalıştırın:
* Gözetmen
* İşçi Java sanal makineleri (JVMs), topolojileri çalıştırmak için
* Ambari ajanı

## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Geliştirme için Storm olay merkezi nin ikililerini nasıl bulurum?

Topolojinizle Birlikte Storm olay hub'ı spout .jar dosyalarını kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

### <a name="java-based-topology"></a>Java tabanlı topoloji

[HDInsight'ta (Java) Apache Storm ile Azure Etkinlik Hub'larından etkinlikleri işleme](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)

### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C# tabanlı topoloji (Mono HDInsight 3.4+ Linux Storm kümeleri üzerine)

[HDInsight'ta Apache Storm ile Azure Etkinlik Hub'larından etkinlikleri işleme (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)

### <a name="latest-apache-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>HDInsight 3.5+ Linux Storm kümeleri için en son Apache Storm etkinlik merkezi spout ikilileri

HDInsight 3.5+ Linux Storm kümeleriyle çalışan en son Storm etkinlik merkezini nasıl kullanacağınızı öğrenmek için [mvn-repo readme dosyasına](https://github.com/hdinsight/mvn-repo/blob/master/README.md)bakın.

### <a name="source-code-examples"></a>Kaynak kodu örnekleri

Azure HDInsight kümesinde Apache Storm topolojisi (Java dilinde yazılmış) kullanarak Azure Event Hub'dan nasıl okunup yazacağınız la ilgili [örneklere](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) bakın.

## <a name="how-do-i-locate-storm-log4j-2-configuration-files-on-clusters"></a>Kümeler üzerindeki Storm Log4J 2 yapılandırma dosyalarını nasıl bulabiliyorum?

Fırtına hizmetleri için [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) yapılandırma dosyalarını tanımlamak için.

### <a name="on-head-nodes"></a>Baş düğümleri üzerinde

Nimbus Log4J yapılandırması `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`buradan okunur.

### <a name="on-worker-nodes"></a>İşçi düğümleri üzerinde

Denetçi Log4J yapılandırması `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`.

İşçi Log4J yapılandırma dosyası `/usr/hdp/\<HDP version>/storm/log4j2/worker.xml`okunur.

Örnekler:`/usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml`
`/usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml`

---

## <a name="not-a-leader-exception"></a>Lider istisnası değil

Bir topoloji gönderirken, kullanıcı benzer bir hata `Topology submission exception, cause not a leader, the current leader is NimbusInfo`iletisi alabilir: .

Çözmek için, kullanıcıdüğümleri yeniden başlatmak/yeniden başlatmak için bir bilet dosyalamak gerekebilir. Daha fazla bilgi [https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html](https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html)için bkz.

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

- [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
