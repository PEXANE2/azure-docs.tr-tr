---
title: Azure HDInsight'ı kullanarak Storm sorunlarını giderme
description: Azure HDInsight ile Apache Storm kullanma hakkında sık sorulan soruların yanıtlarını alın.
keywords: Azure HDInsight, Storm, SSS, sorun giderme kılavuzu, yaygın sorunlar
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 11/08/2019
ms.custom: seodec18
ms.openlocfilehash: b51b2c21fd9256c93f6947386a48336af2b75d88
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75896016"
---
# <a name="troubleshoot-apache-storm-by-using-azure-hdinsight"></a>Azure HDInsight'ı kullanarak Apache Storm sorunlarını giderme

Sık karşılaşılan sorunlar ve çözümleri ile çalışmak için öğrenin [Apache Storm](https://storm.apache.org/) yüklerde [Apache Ambari](https://ambari.apache.org/).

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>Bir küme üzerindeki Storm kullanıcı arabirimini nasıl erişim sağlanır?

Storm kullanıcı arabirimini bir tarayıcıdan erişirken için iki seçeneğiniz vardır:

### <a name="apache-ambari-ui"></a>Apache ambarı Kullanıcı arabirimi

1. Ambari panosuna gidin.
2. Hizmetler listesinde seçin **Storm**.
3. İçinde **hızlı bağlantılar** menüsünde **Storm kullanıcı arabirimini**.

### <a name="direct-link"></a>Doğrudan bağlantı

Storm kullanıcı arabirimini şu URL'den erişebilirsiniz:

`https://<cluster DNS name>/stormui`

Örnek: `https://stormcluster.azurehdinsight.net/stormui`

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>Nasıl ı Storm olay hub'ı spout denetim noktası bilgilerini bir topolojiden diğerine aktarma?

Azure Event Hubs'dan okumayı topolojiler geliştirirken, .jar dosyasını spout HDInsight Storm olay hub'ı kullanarak, yeni kümede aynı ada sahip bir topoloji dağıtmanız gerekir. Ancak, hassastır denetim noktası verileri korumanız gerekir [Apache ZooKeeper](https://zookeeper.apache.org/) eski kümede.

### <a name="where-checkpoint-data-is-stored"></a>Denetim noktası verilerin depolandığı

Denetim noktası verileri ofsetleri için iki kök yolları, ZooKeeper, olay hub'ı spout tarafından depolanır:

- İşlem dışı Spout kontrol noktaları `/eventhubspout`depolanır.

- İşlem Spout denetim noktası verileri `/transactional`depolanır.

### <a name="how-to-restore"></a>Geri yükleme

ZooKeeper dışında verilerini dışarı aktarın ve ardından yeni bir adla ZooKeeper dön verileri almak için kullandığınız kitaplıklar ve komut dosyalarını almak için bkz. [HDInsight Storm örnekleri](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

LIB klasör, içeri/dışarı aktarma işlemi uygulamasını içeren .jar dosyalarını içerir. Bash klasör verilerini eski kümede ZooKeeper sunucusundan dışarı aktarmak ve yeni kümede ZooKeeper sunucuya geri alma yapmayı gösteren bir örnek betiği içerir.

Çalıştırma [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) betiğinden sonra verileri içeri ve dışarı aktarmak, ZooKeeper düğümleri. Betik doğru Hortonworks Data Platform (HDP) sürümüne güncelleştirin. (Bu betikler HDInsight, genel yapmaya çalışıyoruz. Genel komut dosyası herhangi bir düğüm kümesi değişiklik olmadan kullanıcı tarafından çalışır.)

Dışarı aktar komutu, ayarladığınız bir konuma (Azure Blob Storage veya Azure Data Lake Storage) meta verileri bir Apache Hadoop Dağıtılmış Dosya Sistemi (bir) yoluna yazar.

### <a name="examples"></a>Örnekler

#### <a name="export-offset-metadata"></a>Uzaklık meta verileri dışarı aktarma

1. ZooKeeper kümeye verilmesi gerekiyor, kontrol noktasını uzaklığı kümede gitmek için SSH kullanın.
2. ZooKeeper yol verilerini `/stormmetadta/zkdata` 00:00 yoluna dışarı aktarmak için aşağıdaki komutu çalıştırın (HDP sürüm dizesini güncelleştirdikten sonra):

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Uzaklık meta verileri içeri aktarma

1. ZooKeeper kümeye, kontrol noktası içeri aktarılacak gereksinimlerini uzaklığı kümede gitmek için SSH kullanın.
2. ZooKeeper `/stormmetadata/zkdata` Path 'den hedef kümedeki sunucusuna ZooKeeper sapmasını verileri aktarmak için aşağıdaki komutu çalıştırın (HDP sürüm dizesini güncelleştirdikten sonra):

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```

#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Topolojileri verileri işleme başına veya kullanıcının seçtiği zaman damgası başlayabilmesi uzaklık meta verilerini silme

1. ZooKeeper kümeye silinmesi gerekiyor, kontrol noktasını uzaklığı kümede gitmek için SSH kullanın.
2. (HDP sürüm dizesi güncelleştirdikten sonra) geçerli küme içindeki tüm ZooKeeper uzaklık verilerini silmek için aşağıdaki komutu çalıştırın:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Storm ikili dosyaları bir kümeye nasıl bulabilirim?

Geçerli HDP yığınının fırtınası ikilileri `/usr/hdp/current/storm-client`. Konumun baş düğümleri için hem de çalışan düğümleri için aynıdır.

/Usr/HDP içindeki belirli HDD sürümleri için birden çok ikili olabilir (örneğin, `/usr/hdp/2.5.0.1233/storm`). `/usr/hdp/current/storm-client` klasörü, kümede çalışan en son sürüme bağlı değildir.

Daha fazla bilgi için bkz. SSH ve [Apache Storm](https://storm.apache.org/) [kullanarak HDInsight kümesine bağlanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) .

## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>Bir Storm kümesine dağıtım topolojisi nasıl belirlerim?

İlk olarak, HDInsight Storm ile yüklü tüm bileşenleri tanımlayın. Bir Storm kümesi dört düğüm kategorisi oluşur:

* Ağ geçidi düğümleri
* Baş düğümler
* ZooKeeper düğümleri
* Çalışan düğümleri

### <a name="gateway-nodes"></a>Ağ geçidi düğümleri

Bir ağ geçidi ve etkin bir Ambari yönetim hizmeti genel erişim sağlayan ters proxy hizmeti, ağ geçidi düğüm. Ayrıca, Ambari öncü seçimi işler.

### <a name="head-nodes"></a>Baş düğümler

Storm baş düğümü, aşağıdaki hizmetleri çalıştırın:
* Nimbus
* Ambari sunucusunun
* Ambari ölçümleri sunucusu
* Ambari ölçümleri Toplayıcı
 
### <a name="zookeeper-nodes"></a>ZooKeeper düğümleri

HDInsight, üç düğümlü ZooKeeper çekirdek ile birlikte gelir. Çekirdek boyutu sabittir ve yapılandırılamaz.

Storm kümesi Hizmetleri'nde ZooKeeper çekirdeği otomatik olarak kullanmak üzere yapılandırılır.

### <a name="worker-nodes"></a>Çalışan düğümleri

Storm çalışan düğümleri aşağıdaki hizmetleri çalıştırın:
* Gözetmen
* Çalışan Java topolojileri çalıştırmak için sanal (JVMs)
* Ambari aracı

## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Geliştirme için Storm olay hub'ı spout ikili dosyaları nasıl bulabilirim?

Storm olay hub'ı spout .jar dosyalarını topolojinizi ile kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

### <a name="java-based-topology"></a>Java tabanlı topoloji

[(Java) HDInsight üzerinde Apache Storm ile Azure Event hubs'dan olayları işleme](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)

### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C#-topoloji (HDInsight 3.4 + Linux Storm kümeleri üzerinde Mono) tabanlı

[HDInsight üzerinde Apache Storm ile Azure Event hubs'tan olay işleme (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)

### <a name="latest-apache-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Apache Storm olay hub'ın en son spout ikili dosyaları HDInsight 3.5 + Linux Storm kümeleri

HDInsight 3.5 + Linux fırtınası kümeleriyle birlikte çalışarak en son fırtınası Olay Hub 'ı nasıl kullanacağınızı öğrenmek için bkz. [MVN-repo Benioku dosyası](https://github.com/hdinsight/mvn-repo/blob/master/README.md).

### <a name="source-code-examples"></a>Kaynak kod örnekleri

Bkz: [örnekler](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) okuma ve yazma (Java dilinde yazılmış) bir Apache Storm topolojisi kullanarak bir Azure HDInsight kümesinde Azure olay Hub'ından nasıl.

## <a name="how-do-i-locate-storm-log4j-2-configuration-files-on-clusters"></a>Storm Log4J 2 yapılandırma dosyalarını kümeleri nasıl bulabilirim?

Tanımlamak için [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) Storm hizmetler için yapılandırma dosyaları.

### <a name="on-head-nodes"></a>Baş düğümler üzerinde

Nimbus Log4J yapılandırması `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`okundu.

### <a name="on-worker-nodes"></a>Çalışan düğümlerine

Gözetmen Log4J yapılandırması `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`' den okundu.

Çalışan Log4J yapılandırma dosyası `/usr/hdp/\<HDP version>/storm/log4j2/worker.xml`okundu.

Örnekler: `/usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml`
`/usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml`

---

## <a name="not-a-leader-exception"></a>Öncü özel durum değil

Bir topoloji gönderirken Kullanıcı şuna benzer bir hata iletisi alabilir: `Topology submission exception, cause not a leader, the current leader is NimbusInfo`.

Bu sorunu çözmek için, kullanıcıların düğümlerin yeniden başlatılması/yeniden başlatılması için bir bilet dosyası erişmesi gerekebilir. Daha fazla bilgi için bkz. [https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html](https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html).

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

- [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
