---
title: Apache Hive Azure HDInsight ile Java kullanıcı tanımlı işlev (UDF)
description: Apache Hive ile çalışan Java tabanlı kullanıcı tanımlı bir işlev (UDF) nasıl oluşturacağınızı öğrenin. Bu örnek UDF, metin dizeleri tablosunu küçük harfe dönüştürür.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/20/2019
ms.openlocfilehash: 73a2a612a4eeb4a59f12abf0660fffb092f0547f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74327204"
---
# <a name="use-a-java-udf-with-apache-hive-in-hdinsight"></a>HDInsight'ta Apache Hive ile Java UDF kullanın

Apache Hive ile çalışan Java tabanlı kullanıcı tanımlı bir işlev (UDF) nasıl oluşturacağınızı öğrenin. Bu örnekteki Java UDF, metin dizeleri tablosunu tüm küçük karakterlere dönüştürür.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta hadoop kümesi. [Linux'ta HDInsight ile başlayın](./apache-hadoop-linux-tutorial-get-started.md)bakın.
* [Java Geliştirici Kiti (JDK) sürüm 8](https://aka.ms/azure-jdks)
* [Apache Maven](https://maven.apache.org/download.cgi) düzgün Apache göre [yüklü.](https://maven.apache.org/install.html)  Maven Java projeleri için bir proje inşa sistemidir.
* Kümeleriniz birincil depolama için [URI düzeni.](../hdinsight-hadoop-linux-information.md#URI-and-scheme) Bu, Azure Depolama için wasb://, Azure Veri Gölü Depolama Gen2 için abfs:// veya Azure Veri Gölü Depolama Gen1 için adl://. Azure Depolama için güvenli aktarım etkinleştirilirse, URI `wasbs://`.  Ayrıca bakınız, [güvenli aktarım.](../../storage/common/storage-require-secure-transfer.md)

* Metin düzenleyicisi veya Java IDE

    > [!IMPORTANT]  
    > Windows istemcisinde Python dosyalarını oluşturursanız, LF'yi satır sonu olarak kullanan bir düzenleyici kullanmanız gerekir. Düzenleyicinizin LF veya CRLF kullanıp kullanmadığından emin değilseniz, CR karakterini kaldırma adımlarını gidermek için [Sorun Giderme](#troubleshooting) bölümüne bakın.

## <a name="test-environment"></a>Test ortamı

Bu makale için kullanılan ortam, Windows 10 çalıştıran bir bilgisayardı.  Komutlar bir komut istemiyle yürütüldü ve çeşitli dosyalar Not Defteri ile düzenlendi. Ortamınız için buna göre değiştirin.

Komut isteminden, çalışma ortamı oluşturmak için aşağıdaki komutları girin:

```cmd
IF NOT EXIST C:\HDI MKDIR C:\HDI
cd C:\HDI
```

## <a name="create-an-example-java-udf"></a>Örnek bir Java UDF oluşturma

1. Aşağıdaki komutu girerek yeni bir Maven projesi oluşturun:

    ```cmd
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    Bu `exampleudf`komut, Maven projesini içeren bir dizin oluşturur.

2. Proje oluşturulduktan sonra, aşağıdaki `exampleudf/src/test` komutu girerek projenin bir parçası olarak oluşturulan dizini silin:

    ```cmd
    cd ExampleUDF
    rmdir /S /Q "src/test"
    ```

3. Aşağıdaki `pom.xml` komutu girerek açın:

    ```cmd
    notepad pom.xml
    ```

    Ardından varolan `<dependencies>` girişi aşağıdaki XML ile değiştirin:

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.3</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

    Bu girişler, HDInsight 3.6 ile birlikte hadoop ve hive sürümünü belirtir. [HDInsight bileşen sürüm](../hdinsight-component-versioning.md) belgesinden HDInsight ile sağlanan Hadoop ve Hive sürümleri hakkında bilgi bulabilirsiniz.

    Dosyanın `<build>` sonundaki `</project>` satırdan önce bir bölüm ekleyin. Bu bölüm aşağıdaki XML içermelidir:

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.6  -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.1</version>
                <configuration>
                    <!-- Keep us from getting a can't overwrite file error -->
                    <transformers>
                        <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                        </transformer>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
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
        </plugins>
    </build>
    ```

    Bu girişler, projenin nasıl inşa edilebildiğini tanımlar. Özellikle, projenin kullandığı Java sürümü ve kümeye dağıtım için bir uberjar nasıl inşa edilebilmiştir.

    Değişiklikler yapıldıktan sonra dosyayı kaydedin.

4. Yeni bir dosya oluşturmak ve açmak `ExampleUDF.java`için aşağıdaki komutu girin:

    ```cmd
    notepad src/main/java/com/microsoft/examples/ExampleUDF.java
    ```

    Ardından aşağıdaki java kodunu kopyalayıp yeni dosyaya yapıştırın. O zaman dosyayı kapat.

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of the UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of the input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If the value is null, return a null
            if(input == null)
                return null;
            // Lowercase the input string and return it
            return input.toLowerCase();
        }
    }
    ```

    Bu kod, dize değerini kabul eden ve dizeküçük bir sürümünü döndüren bir UDF uygular.

## <a name="build-and-install-the-udf"></a>UDF'yi oluşturma ve yükleme

Aşağıdaki komutlarda, farklıysa gerçek kullanıcı adı ile değiştirin. `sshuser` Gerçek `mycluster` küme adı ile değiştirin.

1. Aşağıdaki komutu girerek UDF'yi derleyip paketle:

    ```cmd
    mvn compile package
    ```

    Bu komut UDF'yi oluşturur `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` ve dosyaya paketler.

2. Aşağıdaki `scp` komutu girerek dosyayı HDInsight kümesine kopyalamak için komutu kullanın:

    ```cmd
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net:
    ```

3. Aşağıdaki komutu girerek SSH kullanarak kümeye bağlanın:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

4. Açık SSH oturumundan, kavanoz dosyasını HDInsight depolama alanına kopyalayın.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>Kovan'daki UDF'yi kullanma

1. Aşağıdaki komutu girerek Beeline istemcisini SSH oturumundan başlatın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    Bu komut, kümenizin giriş hesabı için **yönetici** varsayılanını kullandığınızı varsayar.

2. `jdbc:hive2://localhost:10001/>` İstem istemine vardığınızda, UDF'yi Hive'a eklemek ve bir işlev olarak ortaya çıkarmak için aşağıdakileri girin.

    ```hiveql
    ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

3. Tablodan alınan değerleri küçük harf dizelerine dönüştürmek için UDF'yi kullanın.

    ```hiveql
    SELECT tolower(state) AS ExampleUDF, state FROM hivesampletable LIMIT 10;
    ```

    Bu sorgu tablodan durumu seçer, dizeyi küçük harfe dönüştürür ve sonra değiştirilmemiş adla birlikte görüntüler. Çıktı aşağıdaki metne benzer görünür:

        +---------------+---------------+--+
        |  exampleudf   |     state     |
        +---------------+---------------+--+
        | california    | California    |
        | pennsylvania  | Pennsylvania  |
        | pennsylvania  | Pennsylvania  |
        | pennsylvania  | Pennsylvania  |
        | colorado      | Colorado      |
        | colorado      | Colorado      |
        | colorado      | Colorado      |
        | utah          | Utah          |
        | utah          | Utah          |
        | colorado      | Colorado      |
        +---------------+---------------+--+

## <a name="troubleshooting"></a>Sorun giderme

Kovan işini çalıştırırken, aşağıdaki metne benzer bir hatayla karşılaşabilirsiniz:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Bu sorun Python dosyasındaki satır sonları neden olabilir. Birçok Windows düzenleyicisi satır bitişi olarak CRLF kullanarak varsayılan, ancak Linux uygulamaları genellikle LF bekliyoruz.

Dosyayı HDInsight'a yüklemeden önce CR karakterlerini kaldırmak için aşağıdaki PowerShell ifadelerini kullanabilirsiniz:

```PowerShell
# Set $original_file to the python file path
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışmanın diğer yolları için [HDInsight ile Apache Hive'ı kullanın'](hdinsight-use-hive.md)a bakın.

Hive Kullanıcı Tanımlı Fonksiyonlar hakkında daha fazla bilgi için, apache.org'daki Hive wiki'sinin [Apache Hive Operatörleri ve Kullanıcı Tanımlı Fonksiyonlar](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) bölümüne bakın.
