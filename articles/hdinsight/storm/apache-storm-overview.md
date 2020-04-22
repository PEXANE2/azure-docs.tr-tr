---
title: Apache Storm nedir? - Azure HDInsight
description: Apache Storm, veri akışlarını gerçek zamanlı olarak işlemenize olanak tanır. Azure HDInsight, Azure bulutu üzerinde Storm kümelerini kolayca oluşturmanıza olanak tanır. Visual Studio ile C# kullanarak Storm çözümleri oluşturabilir ve sonra HDInsight Storm kümelerinize dağıtabilirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 04/20/2020
ms.openlocfilehash: 44c0ca3a2fc16b805744678cc3358b4f5690766a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687647"
---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Azure HDInsight’ta Apache Storm nedir?

[Apache Storm](https://storm.apache.org/); dağıtılmış, hataya dayanıklı, açık kaynaklı bir hesaplama sistemidir. [Apache Hadoop](../hadoop/apache-hadoop-introduction.md)ile gerçek zamanlı veri akışlarını işlemek için Storm'u kullanabilirsiniz. Fırtına çözümleri, ilk seferinde başarıyla işlenmemiş verileri yeniden oynatma olanağıyla verilerin garantili olarak işlenmesini de sağlayabilir.

## <a name="why-use-apache-storm-on-hdinsight"></a>HdInsight'ta neden Apache Storm kullanıyorsun?

HDInsight üzerinde Storm aşağıdaki özellikleri sunar:

* __Fırtına çalışma süresinde %99 Hizmet Düzeyi Sözleşmesi (SLA)__: Storm on HDInsight tam sürekli destek le birlikte gelir. HDInsight üzerinde Storm ayrıca yüzde 99,9 SLA’ya sahiptir. Diğer bir deyişle Microsoft, Storm kümesinin, sürenin en az yüzde 99,9’unda dış bağlantıya sahip olacağını garanti eder. Daha fazla bilgi için bkz. [Azure desteği](https://azure.microsoft.com/support/options/). Ayrıca, [HDInsight belgesi için SLA bilgilerine](https://azure.microsoft.com/support/legal/sla/hdinsight/v1_0/) bakın.

* Oluşturma sırasında veya sonrasında Storm kümesinde betik çalıştırarak kolay özelleştirmeyi destekler. Daha fazla bilgi için bkz. [HDInsight kümelerini betik eylemi kullanarak özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md).

* **Birden fazla dilde çözümler oluşturma**: Storm bileşenlerini Java, C# ve Python gibi dilediğiniz bir dilde yazabilirsiniz.

    * C# topolojisi geliştirme, yönetme ve izleme için HDInsight ile Visual Studio’yu tümleştirir. Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

    * Trident Java arabirimini destekler. İletilerin tam olarak bir kez işlenmesini, işlemsel veri deposu kalıcılığını ve sık kullanılan Stream Analytics işlemlerini destekleyen Storm topolojileri oluşturabilirsiniz.

* **Dinamik ölçeklendirme**: Çalışan Storm topolojilerini etkilemeden çalışan düğümleri ekleyebilir veya kaldırabilirsiniz. Ölçekleme işlemleri yle eklenen yeni düğümlerden yararlanmak için çalışan topolojileri devre dışı bırakın ve yeniden etkinleştirin.

* **Birden çok Azure hizmetini kullanarak akış lı ardışık hatlar oluşturun**: HDInsight'ta Fırtına diğer Azure hizmetleriyle tümleşir. Olay Hub'ları, SQL Veritabanı, Azure Depolama ve Azure Veri Gölü Depolama gibi. Azure hizmetleriyle tümlebilen örnek bir çözüm için, [HDInsight'ta Apache Storm ile Olay Hub'larından İşlem etkinliklerine](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)bakın.

Apache Storm'u gerçek zamanlı analiz çözümleri için kullanan şirketlerin listesi için, [Apache Storm kullanan şirketlere](https://storm.apache.org/Powered-By.html)bakın.

Storm'u kullanmaya başlamak için [Azure HDInsight'ta Apache Storm topolojisi oluştur'a](apache-storm-quickstart.md)bakın ve izleyin.

## <a name="how-does-apache-storm-work"></a>Apaçi Fırtınası nasıl çalışır?

Fırtına, aşina olabileceğiniz [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html) işleri yerine topolojiler çalıştırın. Storm topolojileri döngüsel olmayan yönlü grafikte (DAG) düzenlenmiş birden fazla bileşenden oluşur. Veriler grafikteki bileşenler arasında akar. Her bileşen bir veya daha fazla veri akışı kullanır ve isteğe bağlı olarak bir veya daha fazla akış yayar. Aşağıdaki diyagram, temel bir sözcük sayısı topolojisindeki bileşenler arasında verilerin nasıl aktığını gösterir:

![Bileşenlerin bir Storm topolojisinde nasıl düzenlendiğini gösteren örnek](./media/apache-storm-overview/example-apache-storm-topology-diagram.png)

* Spout bileşenleri, verileri bir topolojiye getirir. Bu bileşenler topolojiye bir veya daha fazla akış yayar.

* Bolt bileşenleri, spout veya diğer boltlardan yayılan akışları kullanır. Boltlar topolojiye isteğe bağlı olarak akışlar yayabilir. Boltlar ayrıca HDFS, Kafka veya HBase gibi dış hizmetlere veya depolama alanlarına veri yazmaktan sorumludur.

## <a name="reliability"></a>Güvenilirlik

Apache Storm, veri analizi yüzlerce düğüme dağıldığında bile tüm gelen iletilerin her zaman tamamen işleneceğini garanti eder.

Nimbus düğümü, Apache Hadoop JobTracker'a benzer işlevsellik sağlar. Nimbus, Apache ZooKeeper aracılığıyla kümedeki diğer düğümlere görevler atar. Zookeeper düğümleri bir küme için koordinasyonu sağlar ve Nimbus ile Alt düğümler üzerindeki Süpervizör işlemi arasındaki iletişime yardımcı olur. Bir işleme düğümü devre dışı kalırsa Nimbus düğümü bilgilendirilir ve görevi ve ilişkili verileri başka bir düğüme atar.

Apache Storm kümeleri için varsayılan yapılandırma yalnızca bir Nimbus düğümü içerir. HDInsight üzerindeki Storm iki Nimbus düğümü sağlar. Birincil düğüm başarısız olursa birincil düğüm kurtarılırken Storm kümesi ikincil düğüme geçiş yapar. Aşağıdaki diyagramda HDInsight üzerinde Storm için görev akışı yapılandırması gösterilmektedir:

![Nimbus, zookeeper ve supervisor diyagramı](./media/apache-storm-overview/storm-diagram-nimbus.png)

## <a name="ease-of-use"></a>Kullanım kolaylığı

|Kullanım |Açıklama |
|---|---|
|Güvenli Kabuk (SSH) bağlantısı|SSH kullanarak Fırtına kümenizin kafa düğümlerine Internet üzerinden erişebilirsiniz. SSH kullanarak komutları doğrudan kümeniz üzerinde çalıştırabilirsiniz. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).|
|Web bağlantısı|Tüm HDInsight kümeleri Ambari web UI'yi sağlar. Ambari web kullanıcı arabirimini kullanarak kümenizdeki hizmetleri kolayca izleyebilir, yapılandırabilir ve yönetebilirsiniz. Storm kümeleri ayrıca Storm Kullanıcı Arabirimini sağlar. Storm kullanıcı arabirimini kullanarak, çalışan Storm topolojilerini tarayıcınızdan izleyip yönetebilirsiniz. Daha fazla bilgi için [Apache Ambari Web UI'yi kullanarak HDInsight'ı yönet'e](../hdinsight-hadoop-manage-ambari.md) bakın ve [Apache Storm UI belgelerini kullanarak yönetin.](apache-storm-deploy-monitor-topology-linux.md#monitor-and-manage-a-topology-using-the-storm-ui)|
|Azure PowerShell ve Azure CLI|PowerShell ve Azure CLI, HDInsight ve diğer Azure hizmetleriyle çalışmak için istemci sisteminizden kullanabileceğiniz komut satırı yardımcı programları sağlar.|
|Visual Studio ile tümleştirme|Visual Studio için Azure Veri Gölü Araçları, SCP.NET çerçevesini kullanarak C# Storm topolojileri oluşturmak için proje şablonları içerir. Data Lake Araçları ayrıca HDInsight üzerinde Storm ile çözümleri dağıtma, izleme ve yönetmeye yönelik araçlar sağlar. Daha fazla bilgi için bkz. [Visual Studio için HDInsight Araçlarıyla C# Storm topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).|

## <a name="integration-with-other-azure-services"></a>Diğer Azure hizmetleriyle tümleştirme

* __Azure Veri Gölü Depolama__: [Bkz. HDInsight'ta Apache Fırtınası ile Azure Veri Gölü Depolama'yı kullanın.](apache-storm-write-data-lake-store.md)

* __Event Hubs__: Event Hubs’ı bir Storm kümesi ile kullanma örneği için aşağıdaki örneklere bakın:

    * [HDInsight'ta (Java) Apache Storm ile Azure Etkinlik Hub'larından etkinlikleri işleme](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)

    * [HDInsight'ta Apache Storm ile Azure Etkinlik Hub'larından etkinlikleri işleme (C#)](apache-storm-develop-csharp-event-hub-topology.md)

* __SQL Veritabanı__, __Cosmos DB__, __Event Hubs__ ve __HBase__: Visual Studio için Data Lake Araçlarına şablon örnekleri eklenmiştir. Daha fazla bilgi için [hdinsight'ta Apache Storm için C# topolojisi geliştirin.](apache-storm-develop-csharp-visual-studio-topology.md)

## <a name="apache-storm-use-cases"></a>Apache Storm kullanım örnekleri

HDInsight üzerinde Storm kullanabileceğiniz bazı yaygın senaryolar aşağıda verilmiştir:

* Nesnelerin İnterneti (IoT)
* Sahtekarlık algılama
* Sosyal analiz
* Ayıklama, dönüşüm ve yükleme (ETL)
* Ağ izleme
* Arama
* Mobil katılım

Gerçek dünya senaryoları hakkında bilgi için [şirketlerin Apache Storm](https://storm.apache.org/Powered-By.html) belgesini nasıl kullandığını öğrenin.

## <a name="development"></a>Geliştirme

.NET geliştiricileri, Visual Studio için Data Lake Araçları kullanarak C# dilinde topolojiler tasarlayıp uygulayabilir. Ayrıca Java ve C# bileşenlerini kullanan karma topolojiler de oluşturabilirsiniz. Daha fazla bilgi için bkz. [Visual Studio kullanarak HDInsight üzerinde Apache Storm için C# topolojileri geliştirme](apache-storm-develop-csharp-visual-studio-topology.md).

Ayrıca seçtiğiniz IDE’yi kullanarak Java çözümleri geliştirebilirsiniz. Daha fazla bilgi için, [HDInsight'ta Apache Storm için Java topolojileri geliştirin'](apache-storm-develop-java-topology.md)e bakın.

Storm bileşenleri geliştirmek için Python da kullanılabilir. Daha fazla bilgi için, [HDInsight'ta Python'u kullanarak Apache Storm topolojileri geliştirin'](apache-storm-develop-python-topology.md)e bakın.

## <a name="common-development-patterns"></a>Yaygın geliştirme desenleri

### <a name="guaranteed-message-processing"></a>Garantili ileti işleme

Apache Storm farklı düzeylerde garantili ileti işleme sağlayabilir. Örneğin, temel bir Storm uygulaması en az bir kez işlemegarantisi ve Trident tam olarak bir kez işleme garanti edebilir. apache.org'da [veri işleme garantilerine](https://storm.apache.org/about/guarantees-data-processing.html) bakın.

### <a name="ibasicbolt"></a>IBasicBolt

Bir giriş tuple okuma deseni, sıfır veya daha fazla tuples yayan ve daha sonra yürütme yönteminin sonunda hemen giriş tuple onaylayan yaygındır. Storm bu deseni otomatik hale getirmek için [IBasicBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/IBasicBolt.html) arabirimini kullanır.

### <a name="joins"></a>Birleştirme

Uygulamalar arasına veri akışları nasıl katılır? Örneğin, birden çok akıştan her tuple'ı yeni bir akışa katılabilir veya belirli bir pencere için yalnızca tuples toplu larını birleştirebilirsiniz. Her iki durumda da katılım, [fieldsGrouping](https://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) kullanılarak gerçekleştirilebilir. Alan gruplandırma, tanımlama gruplarının boltlara nasıl yönlendirileceğini tanımlamanın bir yoludur.

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

Üst N değerini hesaplama örneği için [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java) örneğine bakın.

## <a name="logging"></a>Günlüğe Kaydetme

Fırtına bilgileri günlüğe kaydetmek için Apache Log4j 2 kullanır. Varsayılan olarak, büyük miktarlarda veriler günlüğe kaydedilir ve bilgilerin sıralanması zor olabilir. Günlüğe kaydetme davranışını denetlemek üzere Storm topolojinizin bir parçası olarak günlük yapılandırma dosyası ekleyebilirsiniz.

Günlüğün nasıl yapılandırılacağını gösteren örnek bir topoloji için HDInsight üzerinde Storm için [Java tabanlı WordCount](apache-storm-develop-java-topology.md) örneğine bakın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight'ta Apache Storm ile gerçek zamanlı analiz çözümleri hakkında daha fazla bilgi edinin:

* [Azure HDInsight'ta Bir Apache Storm topolojisi oluşturun ve izleyin](apache-storm-quickstart.md)
* [HDInsight üzerinde Apache Storm için örnek topolojiler](apache-storm-example-topology.md)
