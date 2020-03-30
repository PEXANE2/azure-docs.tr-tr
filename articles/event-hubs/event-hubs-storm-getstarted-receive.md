---
title: "Hızlı başlangıç: Apache Storm - Azure Etkinlik Hub'larını kullanarak etkinlik alma"
description: "Hızlı başlangıç: Bu makalede, Apache Storm kullanarak Azure Etkinlik Hub'larından etkinlik alma hakkında bilgi verilmektedir."
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: java
ms.devlang: multiple
ms.topic: quickstart
ms.custom: seodec18
ms.date: 11/05/2019
ms.author: shvija
ms.openlocfilehash: 90293da07d3a7ef1c32e5f82d35198d4ffa536b1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73717617"
---
# <a name="quickstart-receive-events-from-event-hubs-using-apache-storm"></a>Quickstart: Apache Storm kullanarak Olay Hub'larından etkinlik alma

[Apache Storm,](https://storm.incubator.apache.org) sınırsız veri akışlarının güvenilir şekilde işlenmesini kolaylaştıran dağıtılmış gerçek zamanlı bir hesaplama sistemidir. Bu bölümde, Etkinlik Hub'larından etkinlik almak için Azure Etkinlik Hub'ları Storm epout'unnasıl kullanılacağı gösterilmektedir. Apache Storm'u kullanarak, olayları farklı düğümlerde barındırılan birden çok işlem arasında bölebilirsiniz. Event Hubs'ın Storm ile tümleştirilmesi, Storm'un Zookeeper yüklemesini kullanarak ilerlemesini şeffaf bir şekilde kontrol ederek, kalıcı denetim noktalarını ve Olay Hub'larından gelen paralel alıcıları yöneterek olay tüketimini basitleştirir.

Olay Hub'ları desenleri hakkında daha fazla bilgi için [Olay Hub'larına genel bakış'a][Event Hubs overview]bakın.

## <a name="prerequisites"></a>Ön koşullar
Hızlı başlatma ile başlamadan **önce, bir Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** Etkinlik Hub'larının ad alanını oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanın. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin. 

## <a name="create-project-and-add-code"></a>Proje oluşturma ve kod ekleme

Bu öğretici, zaten mevcut olan Event Hubs spout ile birlikte gelen bir [HDInsight Storm][HDInsight Storm] yüklemesi kullanır.

1. Yeni bir HDInsight kümesi oluşturmak ve Uzak Masaüstü üzerinden ona bağlanmak için [HDInsight Storm - Get yordamını](../hdinsight/storm/apache-storm-overview.md) izleyin.
2. Dosyayı `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` yerel geliştirme ortamınıza kopyalayın. Bu olaylar-fırtına-emzme içerir.
3. Paketi yerel Maven deposuna yüklemek için aşağıdaki komutu kullanın. Bu, daha sonraki bir adımda Fırtına projesinde başvuru olarak eklemenizi sağlar.

    ```shell
    mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar
    ```
4. Eclipse, yeni bir Maven proje oluşturmak **(Dosya**tıklayın , sonra **Yeni**, sonra **Proje**).
   
    ![Dosya -> Yeni -> Projesi][12]
5. **Varsayılan Çalışma Alanı konumunu kullan'ı**seçin, ardından **İleri'yi** tıklatın
6. **maven-archetype-quickstart** archetype'ı seçin ve **ardından İleri'yi** tıklatın
7. **GroupId** ve **ArtifactId**ekle , ardından **Finish'i** tıklatın
8. **pom.xml'de**düğüme `<dependency>` aşağıdaki bağımlılıkları ekleyin.

    ```xml  
    <dependency>
        <groupId>org.apache.storm</groupId>
        <artifactId>storm-core</artifactId>
        <version>0.9.2-incubating</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>com.microsoft.eventhubs</groupId>
        <artifactId>eventhubs-storm-spout</artifactId>
        <version>0.9</version>
    </dependency>
    <dependency>
        <groupId>com.netflix.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>1.3.3</version>
        <exclusions>
            <exclusion>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
        </exclusions>
        <scope>provided</scope>
    </dependency>
    ```

9. **src** klasöründe **Config.properties** adlı bir dosya oluşturun ve aşağıdaki içeriği `receive rule key` kopyalayarak aşağıdaki değerleri ve `event hub name` değerleri kopyalayın:

    ```java
    eventhubspout.username = ReceiveRule
    eventhubspout.password = {receive rule key}
    eventhubspout.namespace = ioteventhub-ns
    eventhubspout.entitypath = {event hub name}
    eventhubspout.partitions.count = 16
       
    # if not provided, will use storm's zookeeper settings
    # zookeeper.connectionstring=localhost:2181
       
    eventhubspout.checkpoint.interval = 10
    eventhub.receiver.credits = 10
    ```
    **Eventhub.receiver.credits** değeri, fırtına kanalına bırakmadan önce kaç olayın toplu olarak toplu olarak paketlendiğini belirler. Basitlik uğruna, bu örnek bu değeri 10 olarak ayarlar. Üretimde, genellikle daha yüksek değerlere ayarlanmalıdır; örneğin, 1024.
10. Aşağıdaki kodla **LoggerBolt** adında yeni bir sınıf oluşturun:
    
    ```java
    import java.util.Map;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import backtype.storm.task.OutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichBolt;
    import backtype.storm.tuple.Tuple;
    
    public class LoggerBolt extends BaseRichBolt {
        private OutputCollector collector;
        private static final Logger logger = LoggerFactory
                  .getLogger(LoggerBolt.class);
    
        @Override
        public void execute(Tuple tuple) {
            String value = tuple.getString(0);
            logger.info("Tuple value: " + value);
   
            collector.ack(tuple);
        }
   
        @Override
        public void prepare(Map map, TopologyContext context, OutputCollector collector) {
            this.collector = collector;
            this.count = 0;
        }
        
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            // no output fields
        }
    
    }
    ```
    
    Bu Fırtına cıvatası alınan olayların içeriğini kaydeder. Bu, tuples'ı depolama hizmetinde depolamak için kolayca genişletilebilir. [Olay Hub'ı ile HDInsight Storm,] verileri Azure Depolama ve Power BI'de depolamak için de aynı yaklaşımı kullanır.
11. **LogTopology** adlı bir sınıf oluşturun ve aşağıdaki kodu kodlayın:
    
    ```java
    import java.io.FileReader;
    import java.util.Properties;
    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.generated.StormTopology;
    import backtype.storm.topology.TopologyBuilder;
    import com.microsoft.eventhubs.samples.EventCount;
    import com.microsoft.eventhubs.spout.EventHubSpout;
    import com.microsoft.eventhubs.spout.EventHubSpoutConfig;
        
    public class LogTopology {
        protected EventHubSpoutConfig spoutConfig;
        protected int numWorkers;
        
        protected void readEHConfig(String[] args) throws Exception {
            Properties properties = new Properties();
            if (args.length > 1) {
                properties.load(new FileReader(args[1]));
            } else {
                properties.load(EventCount.class.getClassLoader()
                        .getResourceAsStream("Config.properties"));
            }
        
            String username = properties.getProperty("eventhubspout.username");
            String password = properties.getProperty("eventhubspout.password");
            String namespaceName = properties
                    .getProperty("eventhubspout.namespace");
            String entityPath = properties.getProperty("eventhubspout.entitypath");
            String zkEndpointAddress = properties
                    .getProperty("zookeeper.connectionstring"); // opt
            int partitionCount = Integer.parseInt(properties
                    .getProperty("eventhubspout.partitions.count"));
            int checkpointIntervalInSeconds = Integer.parseInt(properties
                    .getProperty("eventhubspout.checkpoint.interval"));
            int receiverCredits = Integer.parseInt(properties
                    .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                // (opt)
            System.out.println("Eventhub spout config: ");
            System.out.println("  partition count: " + partitionCount);
            System.out.println("  checkpoint interval: "
                    + checkpointIntervalInSeconds);
            System.out.println("  receiver credits: " + receiverCredits);
     
            spoutConfig = new EventHubSpoutConfig(username, password,
                    namespaceName, entityPath, partitionCount, zkEndpointAddress,
                    checkpointIntervalInSeconds, receiverCredits);
        
            // set the number of workers to be the same as partition number.
            // the idea is to have a spout and a logger bolt co-exist in one
            // worker to avoid shuffling messages across workers in storm cluster.
            numWorkers = spoutConfig.getPartitionCount();
        
            if (args.length > 0) {
                // set topology name so that sample Trident topology can use it as
                // stream name.
                spoutConfig.setTopologyName(args[0]);
            }
        }
        
        protected StormTopology buildTopology() {
            TopologyBuilder topologyBuilder = new TopologyBuilder();
       
            EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
            topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                    spoutConfig.getPartitionCount()).setNumTasks(
                    spoutConfig.getPartitionCount());
            topologyBuilder
                    .setBolt("LoggerBolt", new LoggerBolt(),
                            spoutConfig.getPartitionCount())
                    .localOrShuffleGrouping("EventHubsSpout")
                    .setNumTasks(spoutConfig.getPartitionCount());
            return topologyBuilder.createTopology();
        }
        
        protected void runScenario(String[] args) throws Exception {
            boolean runLocal = true;
            readEHConfig(args);
            StormTopology topology = buildTopology();
            Config config = new Config();
            config.setDebug(false);
        
            if (runLocal) {
                config.setMaxTaskParallelism(2);
                LocalCluster localCluster = new LocalCluster();
                localCluster.submitTopology("test", config, topology);
                Thread.sleep(5000000);
                localCluster.shutdown();
            } else {
                config.setNumWorkers(numWorkers);
                StormSubmitter.submitTopology(args[0], config, topology);
            }
        }
        
        public static void main(String[] args) throws Exception {
            LogTopology topology = new LogTopology();
            topology.runScenario(args);
        }
    }
    ```

    Bu sınıf, yapılandırma dosyasındaki özellikleri kullanarak yeni bir Olay Hub'ları oluşturur. Bu örnek, olay hub'ının izin verdiği en büyük paralelliği kullanmak için olay merkezindeki bölüm sayısı kadar spouts görevi oluşturduğunu belirtmek önemlidir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay Hub'larına genel bakış][Event Hubs overview]
* [Etkinlik merkezi oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[HDInsight Storm]: ../hdinsight/storm/apache-storm-overview.md
[Olay Hub örneği ile HDInsight Storm]: https://github.com/Azure-Samples/hdinsight-java-storm-eventhub

<!-- Images -->

[12]: ./media/event-hubs-get-started-receive-storm/create-storm1.png
