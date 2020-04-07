---
title: HDInsight'ta Hadoop hizmetleri tarafından kullanılan bağlantı noktaları - Azure
description: Bu makale, Azure HDInsight'ta çalışan Apache Hadoop hizmetleri tarafından kullanılan bağlantı noktalarının bir listesini sağlar
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/06/2020
ms.openlocfilehash: fe2cb04f36026740dc54f4668d3c3188592bd8ae
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754217"
---
# <a name="ports-used-by-apache-hadoop-services-on-hdinsight"></a>HDInsight'ta Apache Hadoop hizmetleri tarafından kullanılan bağlantı noktaları

Bu belge, HDInsight kümelerinde çalışan Apache Hadoop hizmetleri tarafından kullanılan bağlantı noktalarının bir listesini sağlar. Ayrıca SSH kullanarak kümeye bağlanmak için kullanılan bağlantı noktaları hakkında bilgi sağlar.

## <a name="public-ports-vs-non-public-ports"></a>Ortak bağlantı noktaları ve halka açık olmayan bağlantı noktaları

Linux tabanlı HDInsight kümeleri yalnızca internette herkese açık üç bağlantı noktası nı ortaya çıkarır: 22, 23 ve 443. Bu bağlantı noktaları, Güvenli HTTPS protokolü üzerinde açığa çıkan SSH ve hizmetleri kullanarak küme erişimini güvence altına alır.

HDInsight, Azure Sanal Ağı'nda çalışan birkaç Azure Sanal Makinesi (küme düğümleri) tarafından uygulanır. Sanal ağ içinden, internet üzerinden maruz olmayan bağlantı noktalarına erişebilirsiniz. SSH üzerinden baş düğümüne bağlanırsanız, küme düğümlerinde çalışan hizmetlere doğrudan erişebilirsiniz.

> [!IMPORTANT]  
> HDInsight için yapılandırma seçeneği olarak bir Azure Sanal Ağı belirtmezseniz, otomatik olarak oluşturulur. Ancak, diğer makinelere (diğer Azure Sanal Makineleri veya istemci geliştirme makineniz gibi) bu sanal ağa katılamazsınız.

Sanal ağa ek makinelere katılmak için önce sanal ağı oluşturmanız ve ardından HDInsight kümenizi oluştururken belirtmeniz gerekir. Daha fazla bilgi için [HDInsight için sanal ağ planı'na](hdinsight-plan-virtual-network-deployment.md)bakın.

## <a name="public-ports"></a>Ortak bağlantı noktaları

BIR HDInsight kümesindeki tüm düğümler bir Azure Sanal Ağı'nda bulunur. Düğümlere internetten doğrudan erişilemez. Ortak ağ geçidi, tüm HDInsight küme türlerinde yaygın olan aşağıdaki bağlantı noktalarına internet erişimi sağlar.

| Hizmet | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- |
| Sshd |22 |SSH |Müşterileri birincil başlıktaki sshd'ye bağlar. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Sshd |22 |SSH |İstemcileri kenar düğümündeki sshd'ye bağlar. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Sshd |23 |SSH |İstemcileri ikincil başlıktaki sshd'ye bağlar. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md). |
| Ambari |443 |HTTPS |Ambari web UI. [Apache Ambari Web UI'yi kullanarak HDInsight'ı yönetin](hdinsight-hadoop-manage-ambari.md) |
| Ambari |443 |HTTPS |Ambari REST API. [Apache Ambari REST API'yi kullanarak HDInsight'ı yönetin](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat |443 |HTTPS |HCatalog REST API. Bkz. [Curl ile MapReduce kullanın](hadoop/apache-hadoop-use-mapreduce-curl.md) |
| HiveServer2 |443 |ODBC |ODBC kullanarak Hive'a bağlanır. [Bkz. Microsoft ODBC sürücüsü ile Excel'i HDInsight'a bağlayın.](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md) |
| HiveServer2 |443 |JDBC |JDBC kullanarak ApacheHive bağlanır. [Hive JDBC sürücüsünü kullanarak HDInsight'ta Apache Hive'a Bağlan'a](hadoop/apache-hadoop-connect-hive-jdbc-driver.md) bakın |

Belirli küme türleri için aşağıdakiler kullanılabilir:

| Hizmet | Bağlantı noktası | Protokol | Küme türü | Açıklama |
| --- | --- | --- | --- | --- |
| `Stargate` |443 |HTTPS |HBase |HBase REST API. Bkz. [Apache HBase'i kullanmaya başlayın](hbase/apache-hbase-tutorial-get-started-linux.md) |
| Livy |443 |HTTPS |Spark |Kıvılcım REST API. Bkz. [Apache Livy'yi kullanarak Apache Spark işlerini uzaktan gönder](spark/apache-spark-livy-rest-interface.md) |
| Kıvılcım Tutumlu sunucu |443 |HTTPS |Spark |Spark Thrift sunucusu Hive sorguları göndermek için kullanılır. [HDInsight'ta Apache Hive ile Beeline Kullanın](hadoop/apache-hadoop-use-hive-beeline.md) |
| Storm |443 |HTTPS |Storm |Fırtına web ui. [HDInsight'ta Apache Storm topolojilerini dağıtın ve yönetin](storm/apache-storm-deploy-monitor-topology-linux.md) |
| Kafka Rest vekil |443 |HTTPS |Kafka |Kafka REST API. [REST proxy kullanarak Azure HDInsight'taki Apache Kafka kümeleriyle etkileşime](kafka/rest-proxy.md) bakın |

### <a name="authentication"></a>Kimlik Doğrulaması

Internet'te herkese açık tüm hizmetlerin kimlik doğrulaması yapılmalıdır:

| Bağlantı noktası | Kimlik Bilgileri |
| --- | --- |
| 22 veya 23 |Küme oluşturma sırasında belirtilen SSH kullanıcı kimlik bilgileri |
| 443 |Küme oluşturma sırasında ayarlanan giriş adı (varsayılan: yönetici) ve parola |

## <a name="non-public-ports"></a>Genel olmayan bağlantı noktaları

> [!NOTE]  
> Bazı hizmetler yalnızca belirli küme türlerinde kullanılabilir. Örneğin, HBase yalnızca HBase küme türlerinde kullanılabilir.

> [!IMPORTANT]  
> Bazı hizmetler aynı anda yalnızca bir headnode çalışır. Birincil başlıktaki servise bağlanmaya çalışır ve bir hata alırsanız, ikincil başlığı kullanmayı yeniden deneyin.

### <a name="ambari"></a>Ambari

| Hizmet | Düğümler | Bağlantı noktası | URL yolu | Protokol |
| --- | --- | --- | --- | --- |
| Ambari web UI | Kafa düğümleri | 8080 | / | HTTP |
| Ambari REST API | Kafa düğümleri | 8080 | /api/v1 | HTTP |

Örnekler:

* Ambari REST API:`curl -u admin "http://10.0.0.11:8080/api/v1/clusters"`

### <a name="hdfs-ports"></a>HDFS bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| NameNode web Kullanıcı Arabirimi |Kafa düğümleri |30070 |HTTPS |Durumu görüntülemek için Web UI |
| NameNode meta veri hizmeti |baş düğümleri |8020 |ıpc |Dosya sistemi meta verileri |
| DataNode |Tüm işçi düğümleri |30075 |HTTPS |Durumu, günlükleri ve benzeri durumları görüntülemek için Web UI. |
| DataNode |Tüm işçi düğümleri |30010 |&nbsp; |Veri aktarımı |
| DataNode |Tüm işçi düğümleri |30020 |ıpc |Meta veri işlemleri |
| İkincil NameNode |Kafa düğümleri |50090 |HTTP |NameNode meta verileri için denetim noktası |

### <a name="yarn-ports"></a>İplik bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Kaynak Yöneticisi web UI |Kafa düğümleri |8088 |HTTP |Kaynak Yöneticisi için Web Arabirimi |
| Kaynak Yöneticisi web UI |Kafa düğümleri |8090 |HTTPS |Kaynak Yöneticisi için Web Arabirimi |
| Kaynak Yöneticisi yönetici arabirimi |baş düğümleri |8141 |ıpc |Uygulama gönderimleri için (Hive, Hive sunucusu, Domuz vb.) |
| Kaynak Yöneticisi zamanlayıcısı |baş düğümleri |8030 |HTTP |Yönetim arabirimi |
| Kaynak Yöneticisi uygulama arabirimi |baş düğümleri |8050 |HTTP |Applications manager arabiriminin adresi |
| DüğümYöneticisi |Tüm işçi düğümleri |30050 |&nbsp; |Konteyner yöneticisinin adresi |
| NodeManager web UI |Tüm işçi düğümleri |30060 |HTTP |Kaynak Yöneticisi arabirimi |
| Zaman çizelgesi adresi |Kafa düğümleri |10200 |RPC |Zaman çizelgesi hizmeti RPC hizmeti. |
| Zaman çizelgesi web UI |Kafa düğümleri |8188 |HTTP |Zaman çizelgesi hizmeti web UI |

### <a name="hive-ports"></a>Kovan bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| HiveServer2 |Kafa düğümleri |10001 |Tasarruf |Hive'a bağlanma hizmeti (Thrift/JDBC) |
| Hive Meta Veri Deposu |Kafa düğümleri |9083 |Tasarruf |Hive meta verilerine bağlanma hizmeti (Thrift/JDBC) |

### <a name="webhcat-ports"></a>WebHCat bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| WebHCat sunucusu |Kafa düğümleri |30111 |HTTP |HCatalog ve diğer Hadoop hizmetlerinin üstündeki Web API'si |

### <a name="mapreduce-ports"></a>MapKüçült bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| İş Geçmişi |Kafa düğümleri |19888 |HTTP |MapReduce JobHistory web UI |
| İş Geçmişi |Kafa düğümleri |10020 |&nbsp; |MapReduce JobHistory sunucusu |
| ShuffleHandler |&nbsp; |13562 |&nbsp; |Ara Harita çıktılarını isteyen Azaltıcılara aktarın |

### <a name="oozie"></a>Oozie

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Oozie sunucusu |Kafa düğümleri |11000 |HTTP |Oozie hizmeti için URL |
| Oozie sunucusu |Kafa düğümleri |11001 |HTTP |Oozie admin için Bağlantı Noktası |

### <a name="ambari-metrics"></a>Ambari Ölçümleri

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| TimeLine (Uygulama geçmişi) |Kafa düğümleri |6188 |HTTP |TimeLine hizmet web UI |
| TimeLine (Uygulama geçmişi) |Kafa düğümleri |30200 |RPC |TimeLine hizmet web UI |

### <a name="hbase-ports"></a>HBase bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| HMaster |Kafa düğümleri |16000 |&nbsp; |&nbsp; |
| HMaster bilgi Web Web UI |Kafa düğümleri |16010 |HTTP |HBase Master web UI için bağlantı noktası |
| Bölge sunucusu |Tüm işçi düğümleri |16020 |&nbsp; |&nbsp; |
| &nbsp; |&nbsp; |2181 |&nbsp; |Müşterilerin ZooKeeper'a bağlanmak için kullandıkları bağlantı noktası |

### <a name="kafka-ports"></a>Kafka limanları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | Açıklama |
| --- | --- | --- | --- | --- |
| Broker |İşçi düğümleri |9092 |[Kafka Tel Protokolü](https://kafka.apache.org/protocol.html) |İstemci iletişimi için kullanılır |
| &nbsp; |Zookeeper düğümleri |2181 |&nbsp; |Müşterilerin Zookeeper'a bağlanmak için kullandıkları bağlantı noktası |
| REST proxy | Kafka yönetim düğümleri |9400 |HTTPS |[Kafka REST şartnamesi](https://docs.microsoft.com/rest/api/hdinsight-kafka-rest-proxy/) |

### <a name="spark-ports"></a>Kıvılcım bağlantı noktaları

| Hizmet | Düğümler | Bağlantı noktası | Protokol | URL yolu | Açıklama |
| --- | --- | --- | --- | --- | --- |
| Kıvılcım Thrift sunucuları |Kafa düğümleri |10002 |Tasarruf | &nbsp; | Spark SQL'e bağlanma hizmeti (Thrift/JDBC) |
| Livy sunucu | Kafa düğümleri | 8998 | HTTP | &nbsp; | Ekstreleri, işleri ve uygulamaları çalıştırmak için hizmet |
| Jupyter dizüstü bilgisayar | Kafa düğümleri | 8001 | HTTP | &nbsp; | Jupyter dizüstü bilgisayar sitesi |

Örnekler:

* Livy: `curl -u admin -G "http://10.0.0.11:8998/"`. Bu örnekte, `10.0.0.11` Livy hizmetini barındıran başlık IP adresidir.
