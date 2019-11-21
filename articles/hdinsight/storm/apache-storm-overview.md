---
title: Apache Storm nedir? - Azure HDInsight
description: Apache Storm, veri akışlarını gerçek zamanlı olarak işlemenize olanak tanır. Azure HDInsight, Azure bulutu üzerinde Storm kümelerini kolayca oluşturmanıza olanak tanır. Visual Studio ile C# kullanarak Storm çözümleri oluşturabilir ve sonra HDInsight Storm kümelerinize dağıtabilirsiniz.
author: hrasheed-msft
ms.reviewer: jasonh
keywords: apache storm kullanım örnekleri,storm kümesi,apache storm nedir
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: overview
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: 9c7e49fe522859f97f00f760822d5eef60db5f69
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74228851"
---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Azure HDInsight’ta Apache Storm nedir?

[Apache Storm](https://storm.apache.org/); dağıtılmış, hataya dayanıklı, açık kaynaklı bir hesaplama sistemidir. You can use Storm to process streams of data in real time with [Apache Hadoop](https://hadoop.apache.org/). Storm çözümleri ayrıca ilk seferde başarılı bir şekilde işlenmemiş verileri yeniden yürütme özelliğiyle birlikte verilerin garantili işlenmesini sağlayabilir.

## <a name="why-use-apache-storm-on-hdinsight"></a>Why use Apache Storm on HDInsight?

HDInsight üzerinde Storm aşağıdaki özellikleri sunar:

* __Storm çalışma süresiyle ilgili %99 Hizmet Düzeyi Sözleşmesi__ : Daha fazla bilgi için [HDInsight için SLA bilgileri](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/) belgesine göz atın.

* Oluşturma sırasında veya sonrasında Storm kümesinde betik çalıştırarak kolay özelleştirmeyi destekler. Daha fazla bilgi için bkz. [HDInsight kümelerini betik eylemi kullanarak özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md).

* **Birden fazla dilde çözümler oluşturma**: Storm bileşenlerini Java, C# ve Python gibi dilediğiniz bir dilde yazabilirsiniz.

    * C# topolojisi geliştirme, yönetme ve izleme için HDInsight ile Visual Studio’yu tümleştirir. Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

    * Trident Java arabirimini destekler. İletilerin tam olarak bir kez işlenmesini, işlemsel veri deposu kalıcılığını ve sık kullanılan Stream Analytics işlemlerini destekleyen Storm topolojileri oluşturabilirsiniz.

* **Dinamik ölçeklendirme**: Çalışan Storm topolojilerini etkilemeden çalışan düğümleri ekleyebilir veya kaldırabilirsiniz.

    * Ölçeklendirme işlemleriyle eklenen birkaç yeni düğümden yararlanmak için, çalışan topolojileri devre dışı bırakıp yeniden etkinleştirebilirsiniz.

* **Create streaming pipelines using multiple Azure services**: Storm on HDInsight integrates with other Azure services such as Event Hubs, SQL Database, Azure Storage, and Azure Data Lake Storage.

    For an example solution that integrates with Azure services, see [Process events from Event Hubs with Apache Storm on HDInsight](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).

Gerçek zamanlı analiz çözümleri için Apache Storm kullanan şirketlerin listesi için bkz. [Apache Storm Kullanan Şirketler](https://storm.apache.org/documentation/Powered-By.html).

To get started using Storm, see [Create and monitor an Apache Storm topology in Azure HDInsight](apache-storm-quickstart.md).

## <a name="how-does-apache-storm-work"></a>How does Apache Storm work

Storm runs topologies instead of the [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html)  jobs that you might be familiar with. Storm topolojileri döngüsel olmayan yönlü grafikte (DAG) düzenlenmiş birden fazla bileşenden oluşur. Veriler grafikteki bileşenler arasında akar. Her bileşen bir veya daha fazla veri akışı kullanır ve isteğe bağlı olarak bir veya daha fazla akış yayar. Aşağıdaki diyagram, temel bir sözcük sayısı topolojisindeki bileşenler arasında verilerin nasıl aktığını gösterir:

![Bileşenlerin bir Storm topolojisinde nasıl düzenlendiğini gösteren örnek](./media/apache-storm-overview/example-apache-storm-topology-diagram.png)

* Spout bileşenleri, verileri bir topolojiye getirir. Bu bileşenler topolojiye bir veya daha fazla akış yayar.

* Bolt bileşenleri, spout veya diğer boltlardan yayılan akışları kullanır. Boltlar topolojiye isteğe bağlı olarak akışlar yayabilir. Boltlar ayrıca HDFS, Kafka veya HBase gibi dış hizmetlere veya depolama alanlarına veri yazmaktan sorumludur.

## <a name="reliability"></a>Güvenilirlik

Apache Storm, veri analizi yüzlerce düğüme dağıldığında bile tüm gelen iletilerin her zaman tamamen işleneceğini garanti eder.

The Nimbus node provides functionality similar to the Apache Hadoop JobTracker, and it assigns tasks to other nodes in a cluster through [Apache ZooKeeper](https://zookeeper.apache.org/). Zookeeper düğümleri küme için eşgüdüm sağlar ve çalışan düğümleri üzerinde Nimbus ile Supervisor işlemi arasındaki iletişimi kolaylaştırır. Bir işleme düğümü devre dışı kalırsa Nimbus düğümü bilgilendirilir ve görevi ve ilişkili verileri başka bir düğüme atar.

Apache Storm kümeleri için varsayılan yapılandırma yalnızca bir Nimbus düğümü içerir. HDInsight üzerindeki Storm iki Nimbus düğümü sağlar. Birincil düğüm başarısız olursa birincil düğüm kurtarılırken Storm kümesi ikincil düğüme geçiş yapar. Aşağıdaki diyagramda HDInsight üzerinde Storm için görev akışı yapılandırması gösterilmektedir:

![Nimbus, zookeeper ve supervisor diyagramı](./media/apache-storm-overview/storm-diagram-nimbus.png)

## <a name="ease-of-creation"></a>Oluşturma kolaylığı

HDInsight üzerinde dakikalar için yeni bir Storm kümesi oluşturabilirsiniz. For more information on creating a Storm cluster, see [Create Apache Hadoop clusters using the Azure portal](../hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="ease-of-use"></a>Kullanım kolaylığı

* __Secure Shell (SSH) bağlantısı__: Storm kümenizin baş düğümlerine İnternet üzerinden SSH kullanarak erişebilirsiniz. SSH kullanarak komutları doğrudan kümeniz üzerinde çalıştırabilirsiniz.

  Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

* __Web bağlantısı__: Tüm HDInsight kümeleri Ambari web kullanıcı arabirimini sunar. Ambari web kullanıcı arabirimini kullanarak kümenizdeki hizmetleri kolayca izleyebilir, yapılandırabilir ve yönetebilirsiniz. Storm kümeleri ayrıca Storm Kullanıcı Arabirimini sağlar. Storm kullanıcı arabirimini kullanarak, çalışan Storm topolojilerini tarayıcınızdan izleyip yönetebilirsiniz.

  For more information, see the [Manage HDInsight using the Apache Ambari Web UI](../hdinsight-hadoop-manage-ambari.md) and [Monitor and manage using the Apache Storm UI](apache-storm-deploy-monitor-topology-linux.md#monitor-and-manage-a-topology-using-the-storm-ui) documents.

* __Azure PowerShell and Azure Classic CLI__: PowerShell and classic CLI both provide command-line utilities that you can use from your client system to work with HDInsight and other Azure services.

* __Visual Studio integration__: Azure Data Lake Tools for Visual Studio include project templates for creating C# Storm topologies by using the SCP.NET framework. Data Lake Araçları ayrıca HDInsight üzerinde Storm ile çözümleri dağıtma, izleme ve yönetmeye yönelik araçlar sağlar.

  Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Diğer Azure hizmetleriyle tümleştirme

* __Azure Data Lake Storage__: For an example of using Data Lake Storage with a Storm cluster, see [Use Azure Data Lake Storage with Apache Storm on HDInsight](apache-storm-write-data-lake-store.md).

* __Event Hubs__: Event Hubs’ı bir Storm kümesi ile kullanma örneği için aşağıdaki örneklere bakın:

    * [Process events from Azure Event Hubs with Apache Storm on HDInsight (Java)](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)

    * [Process events from Azure Event Hubs with Apache Storm on HDInsight (C#)](apache-storm-develop-csharp-event-hub-topology.md)

* __SQL Veritabanı__, __Cosmos DB__, __Event Hubs__ ve __HBase__: Visual Studio için Data Lake Araçlarına şablon örnekleri eklenmiştir. For more information, see [Develop a C# topology for Apache Storm on HDInsight](apache-storm-develop-csharp-visual-studio-topology.md).

## <a name="support"></a>Destek

HDInsight üzerinde Storm kurumsal düzeyde kesintisiz tam destek ile birlikte gelir. HDInsight üzerinde Storm ayrıca yüzde 99,9 SLA’ya sahiptir. Diğer bir deyişle Microsoft, Storm kümesinin, sürenin en az yüzde 99,9’unda dış bağlantıya sahip olacağını garanti eder.

Daha fazla bilgi için bkz. [Azure desteği](https://azure.microsoft.com/support/options/).

## <a name="apache-storm-use-cases"></a>Apache Storm kullanım örnekleri

HDInsight üzerinde Storm kullanabileceğiniz bazı yaygın senaryolar aşağıda verilmiştir:

* Nesnelerin İnterneti (IoT)
* Sahtekarlık algılama
* Sosyal analiz
* Ayıklama, dönüşüm ve yükleme (ETL)
* Ağ izleme
* Arama
* Mobil katılım

For information about real-world scenarios, see the [How companies are using Apache Storm](https://storm.apache.org/Powered-By.html) document.

## <a name="development"></a>Geliştirme

.NET geliştiricileri, Visual Studio için Data Lake Araçları kullanarak C# dilinde topolojiler tasarlayıp uygulayabilir. Ayrıca Java ve C# bileşenlerini kullanan karma topolojiler de oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

Ayrıca seçtiğiniz IDE’yi kullanarak Java çözümleri geliştirebilirsiniz. For more information, see [Develop Java topologies for Apache Storm on HDInsight](apache-storm-develop-java-topology.md).

Storm bileşenleri geliştirmek için Python da kullanılabilir. For more information, see [Develop Apache Storm topologies using Python on HDInsight](apache-storm-develop-python-topology.md).

## <a name="common-development-patterns"></a>Yaygın geliştirme desenleri

### <a name="guaranteed-message-processing"></a>Garantili ileti işleme

Apache Storm farklı düzeylerde garantili ileti işleme sağlayabilir. For example, a basic Storm application can guarantee at-least-once processing, and [Trident](https://storm.apache.org/releases/current/Trident-API-Overview.html) can guarantee exactly once processing.

Daha fazla bilgi için apache.org sayfasındaki [Veri işleme garantileri](https://storm.apache.org/about/guarantees-data-processing.html) bölümüne bakın.

### <a name="ibasicbolt"></a>IBasicBolt

The pattern of reading an input tuple, emitting zero or more tuples, and then acknowledging the input tuple immediately at the end of the execute method is common. Storm bu deseni otomatik hale getirmek için [IBasicBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/IBasicBolt.html) arabirimini kullanır.

### <a name="joins"></a>Birleştirme

Uygulamalar arasına veri akışları nasıl katılır? Örneğin, yeni birden fazla akıştaki her bir tanımlama grubunu yeni bir akışta birleştirebilir veya yalnızca toplu tanımlama gruplarını belirli bir pencere için birleştirebilirsiniz. Her iki durumda da katılım, [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanılarak gerçekleştirilebilir. Alan gruplandırma, tanımlama gruplarının boltlara nasıl yönlendirileceğini tanımlamanın bir yoludur.

Aşağıdaki Java örneğinde fieldsGrouping, "1", "2" ve "3" bileşenlerinden kaynaklanan tanımlama gruplarını MyJoiner boltuna yönlendirmek için kullanılır:

```java
builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));
```

### <a name="batches"></a>Toplu işler

Apache Storm "değer çizgisi tanımlama grubu" olarak bilinen bir iç zamanlama mekanizmasını sağlar. Değer çizgisi tanımlama grubunun topolojiniz içinde ne sıklıkta yayıldığını ayarlayabilirsiniz.

Bir C# bileşeninden değer çizgisi tanımlama grubu kullanımı örneği için bkz. [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

### <a name="caches"></a>Caches

Bellek içi önbelleğe alma, sık kullanılan varlıkları bellekte tuttuğu için çoğunlukla işlemeyi hızlandırmaya yönelik bir mekanizma olarak kullanılır. Bir topoloji birden fazla düğüme ve her bir düğüm içindeki birden fazla işleme dağıtıldığı için, [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanmayı düşünmeniz gerekir. Önbellek araması için kullanılan alanları içeren tanımlama gruplarının her zaman aynı işleme yönlendirildiğinden emin olmak için `fieldsGrouping` kullanın. Bu gruplandırma işlevi, süreçler arasında önbellek girişlerinin yinelenmesini önler.

### <a name="stream-top-n"></a>"İlk N" akış

Topolojiniz bir "ilk N" değeri hesaplamaya bağlıysa, ilk N değeri paralel olarak hesaplayın. Ardından bu hesaplamaların çıktısını genel bir değerde birleştirmeniz gerekir. Bu işlem, alanı paralel işleme için yönlendirmek üzere [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanılarak yapılabilir. Ardından, ilk n değerini genel olarak belirleyen bir bolta yönlendirebilirsiniz.

İlk N değeri hesaplama örneği için [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java) örneğine bakın.

## <a name="logging"></a>Günlüğe kaydetme

Storm uses [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) to log information. Varsayılan olarak, büyük miktarlarda veriler günlüğe kaydedilir ve bilgilerin sıralanması zor olabilir. Günlüğe kaydetme davranışını denetlemek üzere Storm topolojinizin bir parçası olarak günlük yapılandırma dosyası ekleyebilirsiniz.

Günlüğün nasıl yapılandırılacağını gösteren örnek bir topoloji için HDInsight üzerinde Storm için [Java tabanlı WordCount](apache-storm-develop-java-topology.md) örneğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Learn more about real-time analytics solutions with Apache Storm on HDInsight:

* [Create and monitor an Apache Storm topology in Azure HDInsight](apache-storm-quickstart.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md)
