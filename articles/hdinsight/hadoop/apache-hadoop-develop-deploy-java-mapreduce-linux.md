---
title: Apache Hadoop için Java MapReduce oluşturun - Azure HDInsight
description: Java tabanlı bir MapReduce uygulaması oluşturmak için Apache Maven'i nasıl kullanacağınızı öğrenin ve ardından Azure HDInsight'ta Hadoop ile çalıştırın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 01/16/2020
ms.openlocfilehash: a37a8bb45c11d5b74f3059a153806e3d083cf452
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76311963"
---
# <a name="develop-java-mapreduce-programs-for-apache-hadoop-on-hdinsight"></a>HDInsight'ta Apache Hadoop için Java MapReduce programları geliştirin

Java tabanlı bir MapReduce uygulaması oluşturmak için Apache Maven'i nasıl kullanacağınızı öğrenin ve ardından Azure HDInsight'ta Apache Hadoop ile çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

* [Java Geliştirici Kiti (JDK) sürüm 8](https://aka.ms/azure-jdks).

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün Apache göre [yüklü.](https://maven.apache.org/install.html)  Maven Java projeleri için bir proje inşa sistemidir.

## <a name="configure-development-environment"></a>Geliştirme ortamını yapılandırma

Bu makale için kullanılan ortam, Windows 10 çalıştıran bir bilgisayardı. Komutlar bir komut istemiyle yürütüldü ve çeşitli dosyalar Not Defteri ile düzenlendi. Ortamınız için buna göre değiştirin.

Komut isteminden, çalışma ortamı oluşturmak için aşağıdaki komutları girin:

```cmd
IF NOT EXIST C:\HDI MKDIR C:\HDI
cd C:\HDI
```

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturma

1. **Wordcountjava**adlı bir Maven projesi oluşturmak için aşağıdaki komutu girin:

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    Bu `artifactID` komut, parametre tarafından belirtilen adı içeren bir dizin oluşturur (bu örnekte**wordcountjava.)** Bu dizin aşağıdaki öğeleri içerir:

    * `pom.xml`- Projeyi oluşturmak için kullanılan bilgi ve yapılandırma ayrıntılarını içeren [Proje Nesnesi Modeli (POM).](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)
    * src\java\org\apache\hadoop\örnekleri: Uygulama kodunuzu içerir.
    * src\test\java\org\apache\hadoop\örnekleri: Uygulamanız için testler içerir.

1. Oluşturulan örnek kodu kaldırın. Oluşturulan test ve uygulama `AppTest.java`dosyalarını silin ve `App.java` aşağıdaki komutları girerek:

    ```cmd
    cd wordcountjava
    DEL src\main\java\org\apache\hadoop\examples\App.java
    DEL src\test\java\org\apache\hadoop\examples\AppTest.java
    ```

## <a name="update-the-project-object-model"></a>Proje Nesnesi Modelini Güncelleştirme

pom.xml dosyasının tam referansı https://maven.apache.org/pom.htmliçin bkz. Aşağıdaki `pom.xml` komutu girerek açın:

```cmd
notepad pom.xml
```

### <a name="add-dependencies"></a>Bağımlılıkekleme

In `pom.xml`, `<dependencies>` bölüme aşağıdaki metni ekleyin:

```xml
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-mapreduce-examples</artifactId>
    <version>2.7.3</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-mapreduce-client-common</artifactId>
    <version>2.7.3</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-common</artifactId>
    <version>2.7.3</version>
    <scope>provided</scope>
</dependency>
```

Bu, belirli bir &lt;sürümle\>(sürüm &lt;\>içinde listelenen) gerekli kitaplıkları (artifactId içinde listelenir) tanımlar. Derleme zamanında, bu bağımlılıklar varsayılan Maven deposundan indirilir. Daha fazla görüntülemek için [Maven deposu aramasını](https://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) kullanabilirsiniz.

Maven'e `<scope>provided</scope>` bu bağımlılıkların çalışma zamanında HDInsight kümesi tarafından sağlandığı için uygulamayla birlikte paketlememesi gerektiğini söyler.

> [!IMPORTANT]
> Kullanılan sürüm, kümenizdeki Hadoop sürümüyle eşleşmelidir. Sürümler hakkında daha fazla bilgi için [HDInsight bileşen sürüm belgesine](../hdinsight-component-versioning.md) bakın.

### <a name="build-configuration"></a>Yapı yapılandırması

Maven eklentileri, projenin yapı aşamalarını özelleştirmenize olanak sağlar. Bu bölüm eklentileri, kaynakları ve diğer yapı yapılandırma seçenekleri eklemek için kullanılır.

Aşağıdaki kodu dosyaya `pom.xml` ekleyin ve ardından dosyayı kaydedip kapatın. Bu metin, dosyadaki etiketlerin `<project>...</project>` içinde olmalıdır, `</dependencies>` `</project>`örneğin, arasında ve .

```xml
<build>
    <plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>2.3</version>
        <configuration>
        <transformers>
            <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
            </transformer>
        </transformers>
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
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.6.1</version>
        <configuration>
        <source>1.8</source>
        <target>1.8</target>
        </configuration>
    </plugin>
    </plugins>
</build>
```

Bu bölümde Apache Maven Derleyici Eklentisi ve Apache Maven Gölge Eklentisi yapılandırılır. Derleyici eklentisi topolojiderlemek için kullanılır. Gölge eklentisi, Maven tarafından üretilen JAR paketinde lisans yinelemesini önlemek için kullanılır. Bu eklenti, HDInsight kümesinde çalışma zamanında bir "yinelenen lisans dosyaları" hatasını önlemek için kullanılır. Uygulama ile maven-gölge eklentisi `ApacheLicenseResourceTransformer` kullanarak hata önler.

Maven-shade-plugin de uygulama tarafından gerekli tüm bağımlılıkları içeren bir uber kavanoz üretir.

`pom.xml` dosyasını kaydedin.

## <a name="create-the-mapreduce-application"></a>MapReduce uygulamasını oluşturun

1. Yeni bir dosya oluşturmak ve açmak `WordCount.java`için aşağıdaki komutu girin. Yeni bir dosya oluşturmak için istek te **Evet'i** seçin.

    ```cmd
    notepad src\main\java\org\apache\hadoop\examples\WordCount.java
    ```

2. Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın. O zaman dosyayı kapat.

    ```java
    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

        public static class TokenizerMapper
            extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
            StringTokenizer itr = new StringTokenizer(value.toString());
            while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
            }
        }
    }

    public static class IntSumReducer
            extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                            Context context
                            ) throws IOException, InterruptedException {
            int sum = 0;
            for (IntWritable val : values) {
            sum += val.get();
            }
            result.set(sum);
            context.write(key, result);
        }
    }

    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
            System.err.println("Usage: wordcount <in> <out>");
            System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
        }
    }
    ```

    Paket adının ve `org.apache.hadoop.examples` sınıf adının `WordCount`. MapReduce işini gönderirken bu adları kullanırsınız.

## <a name="build-and-package-the-application"></a>Uygulamayı oluşturma ve paketleme

`wordcountjava` Dizinden, uygulamayı içeren bir JAR dosyası oluşturmak için aşağıdaki komutu kullanın:

```cmd
mvn clean package
```

Bu komut, önceki yapı yapılarını temizler, yüklenmemiş bağımlılıkları karşıdan yükler ve ardından uygulamayı oluşturur ve paketler.

Komut bittikten sonra, `wordcountjava/target` dizin . `wordcountjava-1.0-SNAPSHOT.jar`

> [!NOTE]
> Dosya, `wordcountjava-1.0-SNAPSHOT.jar` yalnızca WordCount işini değil, aynı zamanda işin çalışma zamanında gerektirdiği bağımlılıkları da içeren bir uberjar'dır.

## <a name="upload-the-jar-and-run-jobs-ssh"></a>JAR'ı yükleyin ve işleri çalıştırın (SSH)

Aşağıdaki adımlar, `scp` JAR'ı HDInsight kümesindeki Apache HBase'inizin birincil baş düğümüne kopyalamak için kullanılır. Komut `ssh` daha sonra kümeye bağlanmak ve örneği doğrudan baş düğümünde çalıştırmak için kullanılır.

1. Kavanozu kümeye yükleyin. HDInsight küme adınız ile değiştirin `CLUSTERNAME` ve ardından aşağıdaki komutu girin:

    ```cmd
    scp target/wordcountjava-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

1. Kümeye bağlanın. HDInsight küme adınız ile değiştirin `CLUSTERNAME` ve ardından aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. SSH oturumundan, MapReduce uygulamasını çalıştırmak için aşağıdaki komutu kullanın:

   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```

    Bu komut WordCount MapReduce uygulamasını başlatır. Giriş dosyası `/example/data/gutenberg/davinci.txt`ve çıktı dizini . `/example/data/wordcountout` Hem giriş dosyası hem de çıktı küme için varsayılan depolama deposunda depolanır.

1. İş tamamlandıktan sonra, sonuçları görüntülemek için aşağıdaki komutu kullanın:

   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    Aşağıdaki metne benzer değerlere sahip bir sözcük ve sayım listesi almalısınız:

    ```output
    zeal    1
    zelus   1
    zenith  2
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Java MapReduce işini nasıl geliştireceklerini öğrendiniz. HDInsight ile çalışmanın diğer yolları için aşağıdaki belgelere bakın.

* [HDInsight ile Apache Hive'ı kullanma](hdinsight-use-hive.md)
* [HDInsight ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)
* [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/)
