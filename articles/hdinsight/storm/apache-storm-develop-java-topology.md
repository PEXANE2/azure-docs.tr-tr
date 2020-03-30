---
title: Apache Storm örnek Java topolojisi - Azure HDInsight
description: Örnek bir kelime sayısı topolojisi oluşturarak Java'da Apache Storm topolojilerini nasıl oluşturacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/14/2019
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 75100b47ddf8f36ed9a22ff3073c439f8ad9040b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083290"
---
# <a name="create-an-apache-storm-topology-in-java"></a>Java'da Apache Storm topolojisi oluşturun

[Apache Storm](https://storm.apache.org/)için Java tabanlı bir topoloji yi nasıl oluşturacağınızı öğrenin. Burada, sözcük sayısı uygulaması uygulayan bir Fırtına topolojisi oluşturursunuz. Projeyi oluşturmak ve paketlemek için [Apache Maven'i](https://maven.apache.org/) kullanıyorsunuz. Daha sonra, [Apache Storm Flux](https://storm.apache.org/releases/2.0.0/flux.html) çerçevesini kullanarak topolojiyi nasıl tanımlayabileceğinizi öğrenirsiniz.

Bu belgedeki adımları tamamladıktan sonra, topolojiyi HDInsight'ta Apache Storm'a dağıtabilirsiniz.

> [!NOTE]  
> Bu belgede oluşturulan Fırtına topoloji örneklerinin tamamlanmış [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount)bir sürümü ne kadar dır:

## <a name="prerequisites"></a>Ön koşullar

* [Java Geliştirici Kiti (JDK) sürüm 8](https://aka.ms/azure-jdks)

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün Apache göre [yüklü.](https://maven.apache.org/install.html)  Maven Java projeleri için bir proje inşa sistemidir.

## <a name="test-environment"></a>Test ortamı

Bu makale için kullanılan ortam, Windows 10 çalıştıran bir bilgisayardı.  Komutlar bir komut istemiyle yürütüldü ve çeşitli dosyalar Not Defteri ile düzenlendi.

Komut isteminden, çalışma ortamı oluşturmak için aşağıdaki komutları girin:

```cmd
mkdir C:\HDI
cd C:\HDI
```

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturma

**WordCount**adında bir Maven projesi oluşturmak için aşağıdaki komutu girin:

```cmd
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

cd WordCount
mkdir resources
```

Bu komut, temel bir `WordCount` Maven projesi içeren geçerli konumda adlı bir dizin oluşturur. İkinci komut, şimdiki çalışma dizinini '' olarak `WordCount`değiştirir. Üçüncü komut, `resources`daha sonra kullanılacak yeni bir dizin oluşturur.  Dizin `WordCount` aşağıdaki öğeleri içerir:

* `pom.xml`: Maven projesinin ayarlarını içerir.
* `src\main\java\com\microsoft\example`: Uygulama kodunuzu içerir.
* `src\test\java\com\microsoft\example`: Uygulamanız için testler içerir.  

### <a name="remove-the-generated-example-code"></a>Oluşturulan örnek kodu kaldırma

Oluşturulan test ve uygulama `AppTest.java`dosyalarını silin ve `App.java` aşağıdaki komutları girerek:

```cmd
DEL src\main\java\com\microsoft\example\App.java
DEL src\test\java\com\microsoft\example\AppTest.java
```

## <a name="add-maven-repositories"></a>Maven depoları ekle

HDInsight Hortonworks Veri Platformu 'na (HDP) dayanmaktadır, bu nedenle Apache Storm projelerinizin bağımlılıklarını indirmek için Hortonworks deposunu kullanmanızı öneririz.  

Aşağıdaki `pom.xml` komutu girerek açın:

```cmd
notepad pom.xml
```

Ardından satırdan sonra aşağıdaki `<url>https://maven.apache.org</url>` XML'yi ekleyin:

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>https://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>https://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Özellik ekleme

Maven özellikleri olarak adlandırılan proje düzeyi değerlerini tanımlamanızı sağlar. In `pom.xml`, satırdan `</repositories>` sonra aşağıdaki metni ekleyin:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from the Hortonworks repository that is compatible with HDInsight 3.6.
    -->
    <storm.version>1.1.0.2.6.1.9-1</storm.version>
</properties>
```

Artık bu değeri diğer bölümlerde `pom.xml`kullanabilirsiniz. Örneğin, Storm bileşenlerinin sürümünü belirtirken, bir `${storm.version}` değeri kodlamak yerine kullanabilirsiniz.

## <a name="add-dependencies"></a>Bağımlılıkekleme

Storm bileşenleri için bağımlılık ekleyin. In `pom.xml`, `<dependencies>` bölüme aşağıdaki metni ekleyin:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of the jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

Derleme zamanında, Maven bu bilgileri Maven `storm-core` deposunda aramak için kullanır. İlk olarak yerel bilgisayarınızdaki depoya bakar. Dosyalar yoksa, Maven bunları halka açık Maven deposundan indirir ve yerel depoda saklar.

> [!NOTE]  
> Bu `<scope>provided</scope>` bölümdeki satıra dikkat edin. Bu ayar, Sistem tarafından sağlandığı için Maven'e oluşturulan tüm JAR dosyalarından **fırtına çekirdeğini** hariç tutmasını söyler.

## <a name="build-configuration"></a>Yapı yapılandırması

Maven eklentileri, projenin yapı aşamalarını özelleştirmenize olanak sağlar. Örneğin, projenin nasıl derleniş olduğu veya jar dosyasına nasıl paketlenilecek. In `pom.xml`, satırının hemen `</project>` üstüne aşağıdaki metni ekleyin.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Bu bölüm eklentileri, kaynakları ve diğer yapı yapılandırma seçenekleri eklemek için kullanılır. Dosyanın `pom.xml` tam başvurusu için [https://maven.apache.org/pom.html](https://maven.apache.org/pom.html)bkz.

### <a name="add-plug-ins"></a>Eklentiekleme

* **Exec Maven Eklentisi**

    Java'da uygulanan Apache Storm topolojileri için [Exec Maven Plugin,](https://www.mojohaus.org/exec-maven-plugin/) geliştirme ortamınızda topolojiyi yerel olarak kolayca çalıştırmanızı sağladığı için kullanışlıdır. Exec Maven eklentisini eklemek için `<plugins>` `pom.xml` dosyanın bölümüne aşağıdakileri ekleyin:

    ```xml
    <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.6.0</version>
        <executions>
            <execution>
                <goals>
                    <goal>exec</goal>
                </goals>
            </execution>
        </executions>
        <configuration>
            <executable>java</executable>
            <includeProjectDependencies>true</includeProjectDependencies>
            <includePluginDependencies>false</includePluginDependencies>
            <classpathScope>compile</classpathScope>
            <mainClass>${storm.topology}</mainClass>
            <cleanupDaemonThreads>false</cleanupDaemonThreads>
        </configuration>
    </plugin>
    ```

* **Apache Maven Derleyici Eklentisi**

    Başka bir yararlı eklenti [Apache Maven Derleyici Eklentisi](https://maven.apache.org/plugins/maven-compiler-plugin/), derleme seçenekleri değiştirmek için kullanılır. Maven'in uygulamanız için kaynak ve hedef için kullandığı Java sürümünü değiştirin.

  * HDInsight __3.4 veya daha önce__için kaynak ve hedef Java sürümünü __1,7__olarak ayarlayın.

  * HDInsight __3.5__için, kaynak ve hedef Java sürümünü __1.8__olarak ayarlayın.

  Apache Maven Derleyici eklentisini eklemek için `<plugins>` `pom.xml` dosyanın bölümüne aşağıdaki metni ekleyin. Bu örnekte 1.8 belirtilir, bu nedenle hedef HDInsight sürümü 3.5'tir.

  ```xml
  <plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.8.1</version>
    <configuration>
            <source>1.8</source>
            <target>1.8</target>
    </configuration>
  </plugin>
  ```

### <a name="configure-resources"></a>Kaynakları yapılandırma

Kaynaklar bölümü, bileşenlerin ihtiyaç duyduğu yapılandırma dosyaları gibi kod dışı kaynakları topolojiye eklemenize olanak tanır. Bu örnekte, `<resources>` `pom.xml` dosyanın bölümüne aşağıdaki metni ekleyin. Sonra dosyayı kaydedin ve kapatın.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
            <include>log4j2.xml</include>
    </includes>
</resource>
```

Bu örnek, kaynak içeren bir konum olarak`${basedir}`projenin kökünde ( ) kaynak dizinini ekler ve adlı `log4j2.xml`dosyayı içerir. Bu dosya, topoloji tarafından hangi bilgilerin günlüğe kaydedildiğini yapılandırmak için kullanılır.

## <a name="create-the-topology"></a>Topolojiyi oluşturun

Java tabanlı Apache Storm topolojisi, bağımlılık olarak yazmanız (veya referans) gereken üç bileşenden oluşur.

* **Spouts**: Dış kaynaklardan gelen verileri okur ve topolojiye veri akışı yalar.

* **Cıvatalar**: Emzilen veya diğer cıvatalardan yayılan akışlarda işleme yapar ve bir veya daha fazla akış yalar.

* **Topoloji**: Emzitlerin ve cıvataların nasıl düzenlendiğini tanımlar ve topolojinin giriş noktasını sağlar.

### <a name="create-the-spout"></a>Emziyi oluşturma

Dış veri kaynakları nın ayarlanması için gereksinimleri azaltmak için, aşağıdaki spout yalnızca rasgele cümleler yayır. [Storm-Starter örnekleriile](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter)sağlanan bir emzit değiştirilmiş bir sürümüdür.  Bu topoloji de yalnızca bir emzme kullansa da, diğerleri farklı kaynaklardan gelen verileri topolojiye aktaran birkaç tane olabilir.

Yeni bir dosya oluşturmak ve açmak `RandomSentenceSpout.java`için aşağıdaki komutu girin:

```cmd
notepad src\main\java\com\microsoft\example\RandomSentenceSpout.java
```

Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın.  O zaman dosyayı kapat.

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used to emit output
  SpoutOutputCollector _collector;
  //Used to generate a random number
  Random _rand;

  //Open is called when an instance of the class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set the instance collector to the one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data to the stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //The sentences that are randomly emitted
    String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
        "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit the sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare the output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]  
> Harici bir veri kaynağından okuyan bir emzme örneği için aşağıdaki örneklerden birine bakın:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): Twitter'dan okuyan bir örnek.
> * [Fırtına-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): Kafka'dan okunan bir zıdd.

### <a name="create-the-bolts"></a>Cıvataları oluşturma

Cıvatalar veri işleme yi yönetir. Cıvatalar, örneğin hesaplama, kalıcılık veya dış bileşenlerle konuşma gibi her şeyi yapabilir. Bu topoloji iki cıvata kullanır:

* **SplitSentence**: **RandomSentenceSpout** tarafından yayılan cümleleri tek tek sözcüklere böler.

* **Sözcük Sayısı**: Her sözcüğün kaç kez oluştuğunu sayar.

#### <a name="splitsentence"></a>Bölünmüş Tümce

Yeni bir dosya oluşturmak ve açmak `SplitSentence.java`için aşağıdaki komutu girin:

```cmd
notepad src\main\java\com\microsoft\example\SplitSentence.java
```

Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın.  O zaman dosyayı kapat.

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get the sentence content from the tuple
    String sentence = tuple.getString(0);
    //An iterator to get each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give the iterator the sentence
    boundary.setText(sentence);
    //Find the beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it to the output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get the word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a>Sözcük Sayısı

Yeni bir dosya oluşturmak ve açmak `WordCount.java`için aşağıdaki komutu girin:

```cmd
notepad src\main\java\com\microsoft\example\WordCount.java
```

Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın.  O zaman dosyayı kapat.

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often to emit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default to 60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used to trigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get the word contents from the tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment the count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-the-topology"></a>Topolojiyi tanımlayın

Topoloji, emzme leri ve cıvataları bir araya getirerek, bileşenler arasında verilerin nasıl aktığını tanımlayan bir grafiğe bağlar. Ayrıca, Storm'un küme içindeki bileşenlerin örneklerini oluştururken kullandığı paralellik ipuçları da sağlar.

Aşağıdaki resim, bu topoloji için bileşenlerin grafiğinin temel bir diyagramıdır.

![emzme ve cıvata düzenini gösteren diyagram](./media/apache-storm-develop-java-topology/word-count-topology1.png)

Topolojiyi uygulamak için, yeni bir dosya `WordCountTopology.java`oluşturmak ve açmak için aşağıdaki komutu girin:

```cmd
notepad src\main\java\com\microsoft\example\WordCountTopology.java
```

Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın.  O zaman dosyayı kapat.

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for the topology
  public static void main(String[] args) throws Exception {
  //Used to build the topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add the spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add the SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes to the spout, and equally distributes
    //tuples (sentences) across instances of the SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add the counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes to the split bolt, and
    //ensures that the same word is sent to the same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set to false to disable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint to set the number of workers
      conf.setNumWorkers(3);
      //submit the topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap the maximum number of executors that can be spawned
      //for a component to 3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used to run locally
      LocalCluster cluster = new LocalCluster();
      //submit the topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down the cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a>Günlük işlemlerini yapılandırma

Fırtına bilgileri günlüğe kaydetmek için [Apache Log4j 2](https://logging.apache.org/log4j/2.x/) kullanır. Günlüğe kaydetmeyi yapılandırmazsanız, topoloji tanılama bilgilerini yayar. Günlüğe kaydedilenleri denetlemek için aşağıdaki `log4j2.xml` komutu girerek dizinde `resources` adı geçen bir dosya oluşturun:

```cmd
notepad resources\log4j2.xml
```

Ardından aşağıdaki XML metnini kopyalayıp yeni dosyaya yapıştırın.  O zaman dosyayı kapat.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
</Configuration>
```

Bu XML, bu örnek topolojideki bileşenleri içeren `com.microsoft.example` sınıf için yeni bir logger yapılandırır. Düzey, bu topolojideki bileşenler tarafından yayılan günlük bilgilerini yakalayan bu kaydediciiçin izlenebilir şekilde ayarlanmıştır.

Bölüm, `<Root level="error">` günlük günlüğünün kök düzeyini (her şey içinde `com.microsoft.example`olmayan) yalnızca günlük hata bilgilerine göre yapılandırır.

Log4j 2 için günlük yapılandırma hakkında [https://logging.apache.org/log4j/2.x/manual/configuration.html](https://logging.apache.org/log4j/2.x/manual/configuration.html)daha fazla bilgi için bkz.

> [!NOTE]  
> Fırtına sürümü 0.10.0 ve daha yüksek kullanım Log4j 2.x. Storm'un eski sürümlerinde log yapılandırması için farklı bir biçim kullanan Log4j 1.x kullanılmıştır. Eski yapılandırma hakkında bilgi [https://wiki.apache.org/logging-log4j/Log4jXmlFormat](https://wiki.apache.org/logging-log4j/Log4jXmlFormat)için bkz.

## <a name="test-the-topology-locally"></a>Topolojiyi yerel olarak test edin

Dosyaları kaydettikten sonra, topolojiyi yerel olarak test etmek için aşağıdaki komutu kullanın.

```cmd
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

Çalışırken, topoloji başlangıç bilgilerini görüntüler. Aşağıdaki metin sözcük sayısı çıktısı bir örnektir:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Bu örnek günlük, 've' sözcüğünün 113 kez yayıldığını gösterir. Topoloji devam ettiği sürece sayım artmaya devam eder çünkü emzsürekli olarak aynı cümleler ilerler.

Kelimelerin ve sayıların salınımı arasında 5 saniyelik bir zaman aralığı vardır. **WordCount** bileşeni yalnızca bir kene tuple geldiğinde bilgi yonamak için yapılandırılır. Bu kene tuples sadece her beş saniyede teslim edilmesini ister.

## <a name="convert-the-topology-to-flux"></a>Topolojiyi Flux'a dönüştürün

[Flux,](https://storm.apache.org/releases/2.0.0/flux.html) yapılandırmayı uygulamadan ayırmanızı sağlayan Storm 0.10.0 ve üzeri ile kullanılabilen yeni bir çerçevedir. Bileşenleriniz hala Java'da tanımlanmıştır, ancak topoloji bir YAML dosyası kullanılarak tanımlanır. Varsayılan topoloji tanımını projenizle paketleyebilir veya topolojigönderirken bağımsız bir dosya kullanabilirsiniz. Topolojiyi Fırtına'ya gönderirken, YAML topoloji sayılma değerlerinin doldurulması için ortam değişkenlerini veya yapılandırma dosyalarını kullanabilirsiniz.

YAML dosyası, topoloji ve aralarındaki veri akışı için kullanılacak bileşenleri tanımlar. Kavanoz dosyasının bir parçası olarak bir YAML dosyası ekleyebilirsiniz veya harici bir YAML dosyası kullanabilirsiniz.

Flux hakkında daha fazla bilgi için [Flux çerçevesinehttps://storm.apache.org/releases/current/flux.html)](https://storm.apache.org/releases/current/flux.html)bakın ( .

> [!WARNING]  
> Bir hata nedeniyle [(Stormhttps://issues.apache.org/jira/browse/STORM-2055) ](https://issues.apache.org/jira/browse/STORM-2055) 1.0.1 ile, Flux topologları yerel çalıştırmak için bir [Fırtına geliştirme ortamı](https://storm.apache.org/releases/current/Setting-up-development-environment.html) yüklemeniz gerekebilir.

1. Daha önce, `WordCountTopology.java` topoloji tanımlanan, ancak Flux ile gerekli değildir. Aşağıdaki komutla dosyayı silin:

    ```cmd
    DEL src\main\java\com\microsoft\example\WordCountTopology.java
    ```

1. Yeni bir dosya oluşturmak ve açmak `topology.yaml`için aşağıdaki komutu girin:

    ```cmd
    notepad resources\topology.yaml
    ```

    Ardından aşağıdaki metni kopyalayıp yeni dosyaya yapıştırın.  O zaman dosyayı kapat.

    ```yaml
    name: "wordcount"       # friendly name for the topology

    config:                 # Topology configuration
           topology.workers: 1     # Hint for the number of workers to create
  
    spouts:                 # Spout definitions
    - id: "sentence-spout"
           className: "com.microsoft.example.RandomSentenceSpout"
           parallelism: 1      # parallelism hint

    bolts:                  # Bolt definitions
    - id: "splitter-bolt"
           className: "com.microsoft.example.SplitSentence"
           parallelism: 1

    - id: "counter-bolt"
           className: "com.microsoft.example.WordCount"
           constructorArgs:
             - 10
           parallelism: 1

    streams:                # Stream definitions
    - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
           from: "sentence-spout"       # The stream emitter
           to: "splitter-bolt"          # The stream consumer
           grouping:                    # Grouping type
             type: SHUFFLE

    - name: "Splitter -> Counter"
           from: "splitter-bolt"
           to: "counter-bolt"
           grouping:
             type: FIELDS
             args: ["word"]           # field(s) to group on
    ```

1. Aşağıda açıklanan düzeltmeleri `pom.xml` yapmak için açmak için aşağıdaki komutu girin:

    ```cmd
    notepad pom.xml
    ```

   1. Bölüme aşağıdaki yeni bağımlılık `<dependencies>` ekleyin:

        ```xml
        <!-- Add a dependency on the Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```

   1. Bölüme aşağıdaki eklentiyi `<plugins>` ekleyin. Bu eklenti, proje için bir paket (kavanoz dosyası) oluşturulmasını işler ve paketi oluştururken Akı'ya özgü bazı dönüşümler uygular.

        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.2.1</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer to it as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
                </transformers>
                <!-- Keep us from getting a bad signature error -->
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```

   1. Exec Maven Eklentisi bölümü `<configuration>`  >  `<mainClass>` için, `${storm.topology}` `org.apache.storm.flux.Flux`gidin ve değiştirin. Bu ayar, Flux'un topolojiyi geliştirme aşamasında yerel olarak çalıştırmayı işlemesini sağlar.

   1. Bölüme `<resources>` aşağıdakileri `<includes>`ekleyin. Bu XML, topolojiyi projenin bir parçası olarak tanımlayan YAML dosyasını içerir.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-the-flux-topology-locally"></a>Akı topolojisini yerel olarak test edin

1. Akı topolojisini Maven kullanarak derlemek ve yürütmek için aşağıdaki komutu girin:

    ```cmd
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    > [!WARNING]  
    > Topolojiniz Storm 1.0.1 bitlerini kullanıyorsa, bu komut başarısız olur. Bu hata, [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).'den kaynaklanır. Bunun yerine, [Geliştirme ortamınıza Storm'u yükleyin](https://storm.apache.org/releases/current/Setting-up-development-environment.html) ve aşağıdaki adımları kullanın:
    >
    > [Geliştirme ortamınıza Storm yüklediyseniz,](https://storm.apache.org/releases/current/Setting-up-development-environment.html)bunun yerine aşağıdaki komutları kullanabilirsiniz:
    >
    > ```cmd
    > mvn compile package
    > storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    > ```

    Parametre, `--local` geliştirme ortamınızdaki yerel modda topolojiyi çalıştırır. `-R /topology.yaml` Parametre, topolojiyi `topology.yaml` tanımlamak için kavanoz dosyasındaki dosya kaynağını kullanır.

    Çalışırken, topoloji başlangıç bilgilerini görüntüler. Aşağıdaki metin çıktının bir örneğidir:

    ```
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    ```

    Günlüğe kaydedilmiş bilgi toplu arasında 10 saniyelik bir gecikme var.

2. Projeden yeni bir topoloji yaml oluşturun.

    1. Açmak `topology.xml`için aşağıdaki komutu girin:

    ```cmd
    notepad resources\topology.yaml
    ```

    1. Aşağıdaki bölümü bulun ve değerini `10` `5`değiştirin. Bu değişiklik, sözcük sayılarını yayan gruplar arasındaki aralığı 10 saniyeden 5 saniyeye değiştirir.  

    ```yaml
    - id: "counter-bolt"
           className: "com.microsoft.example.WordCount"
           constructorArgs:
             - 5
           parallelism: 1  
    ```

    1. Dosyayı `newtopology.yaml`' olarak kaydet

3. Topolojiyi çalıştırmak için aşağıdaki komutu girin:

    ```cmd
    mvn exec:java -Dexec.args="--local resources/newtopology.yaml"
    ```

    Veya geliştirme ortamınızda Storm varsa:

    ```cmd
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local resources/newtopology.yaml
    ```

    Bu komut `newtopology.yaml` topoloji tanımı olarak kullanır. Parametreyi eklemediğimiz `compile` için, Maven önceki adımlarda oluşturulmuş projenin sürümünü kullanır.

    Topoloji başladıktan sonra, yayılan toplu işarasındaki sürenin `newtopology.yaml`değeri yansıtacak şekilde değiştiğini fark etmelisiniz. Böylece, topolojiyi yeniden derlemek zorunda kalmadan yapılandırmanızı bir YAML dosyası üzerinden değiştirebileceğinizi görebilirsiniz.

Flux çerçevesinin bu ve diğer özellikleri hakkında daha fazla bilgi için [Flux (.https://storm.apache.org/releases/current/flux.html)](https://storm.apache.org/releases/current/flux.html)

## <a name="trident"></a>Trident

[Trident,](https://storm.apache.org/releases/current/Trident-API-Overview.html) Storm tarafından sağlanan üst düzey bir soyutlamadır. Bu durum lu işleme destekler. Trident'in en önemli avantajı, topolojiye giren her mesajın yalnızca bir kez işlenmesini garanti edebildiğidir. Trident kullanmadan, topolojiniz iletilerin en az bir kez işlenmeyi garanti edebilir. Cıvata oluşturmak yerine kullanılabilecek yerleşik bileşenler gibi başka farklılıklar da vardır. Aslında, cıvatalar filtreler, projeksiyonlar ve işlevler gibi daha az genel bileşenlerle değiştirilir.

Trident uygulamaları Maven projeleri kullanılarak oluşturulabilir. Bu makalede daha önce sunulan aynı temel adımları kullanırsınız—yalnızca kod farklıdır. Trident de (şu anda) Flux çerçeve ile kullanılamaz.

Trident hakkında daha fazla bilgi için [Trident API Genel Bakış'a](https://storm.apache.org/releases/current/Trident-API-Overview.html)bakın.

## <a name="next-steps"></a>Sonraki Adımlar

Java'yı kullanarak Apache Storm topolojisi oluşturmayı öğrendiniz. Şimdi nasıl yapılacağını öğrenin:

* [HDInsight'ta Apache Storm topolojilerini dağıtın ve yönetin](apache-storm-deploy-monitor-topology-linux.md)

* [Visual Studio'yu kullanarak HDInsight'ta Apache Storm için C# topolojileri geliştirin](apache-storm-develop-csharp-visual-studio-topology.md)

Daha fazla örnek Apache Storm topolojileri [HDInsight apache Storm için Örnek topologies](apache-storm-example-topology.md)ziyaret ederek bulabilirsiniz.
