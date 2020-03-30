---
title: Python bileşenleriyle Apache Storm - Azure HDInsight
description: Azure HDInsight'ta Python bileşenlerini kullanan bir Apache Storm topolojisini nasıl oluşturacağınızı öğrenin
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 12/16/2019
ms.openlocfilehash: 20e4827b1a86bff338646ef71f0dd732255c09c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77460033"
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>HDInsight'ta Python'u kullanarak Apache Storm topolojileri geliştirin

Python bileşenlerini kullanan bir [Apache Storm](https://storm.apache.org/) topolojisini nasıl oluşturacağınızı öğrenin. Apache Storm birden çok dili destekler, hatta birden fazla dilden bileşenleri tek bir topolojide birleştirmenize olanak sağlar. [Flux](https://storm.apache.org/releases/current/flux.html) çerçevesi (Storm 0.10.0 ile birlikte getirilir) Python bileşenlerini kullanan çözümleri kolayca oluşturmanıza olanak tanır.

> [!IMPORTANT]  
> Bu belgedeki bilgiler HDInsight 3.6'da Storm kullanılarak test edilmiştir.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Storm kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun](../hdinsight-hadoop-create-linux-clusters-portal.md) ve Küme türü için **Fırtına'yı**seçin. **Storm**

* Yerel bir Fırtına geliştirme ortamı (İsteğe bağlı). Yerel bir Fırtına ortamı sadece yerel topoloji çalıştırmak istiyorsanız gereklidir. Daha fazla bilgi için [bkz.](https://storm.apache.org/releases/current/Setting-up-development-environment.html)

* [Python 2.7 veya üzeri.](https://www.python.org/downloads/)

* [Java Geliştirici Kiti (JDK) sürüm 8](https://aka.ms/azure-jdks).

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün Apache göre [yüklü.](https://maven.apache.org/install.html)  Maven Java projeleri için bir proje inşa sistemidir.

## <a name="storm-multi-language-support"></a>Storm çok dilli destek

Apache Storm, herhangi bir programlama dili kullanılarak yazılmış bileşenlerle çalışmak üzere tasarlanmıştır. Bileşenler Storm için Thrift tanımı ile nasıl çalışacağını anlamak gerekir. Python için, Storm ile kolayca arayüz aranıza olanak tanıyan Apache Storm projesinin bir parçası olarak bir modül sağlanır. Bu modülü [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Storm, Java Virtual Machine (JVM) üzerinde çalışan bir Java işlemidir. Diğer dillerde yazılan bileşenler alt işlemler olarak yürütülür. Fırtına stdin /stdout üzerinden gönderilen JSON iletileri kullanarak bu alt süreçler ile iletişim kurar. Bileşenler arasındaki iletişim hakkında daha fazla ayrıntı [Multi-lang Protokol](https://storm.apache.org/releases/current/Multilang-protocol.html) belgelerinde bulunabilir.

## <a name="python-with-the-flux-framework"></a>Flux çerçevesi ile Python

Flux çerçevesi, Storm topolojilerini bileşenlerden ayrı olarak tanımlamanızı sağlar. Flux çerçevesi Fırtına topolojisini tanımlamak için YAML kullanır. Aşağıdaki metin, YAML belgesinde python bileşenine nasıl başvurulmaya bir örnektir:

```yaml
# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "org.apache.storm.flux.wrappers.spouts.FluxShellSpout"
    constructorArgs:
      # Command line
      - ["python", "sentencespout.py"]
      # Output field(s)
      - ["sentence"]
    # parallelism hint
    parallelism: 1
```

Sınıf, `FluxShellSpout` emziyi `sentencespout.py` uygulayan komut dosyasını başlatmak için kullanılır.

Flux, Python komut dosyalarının `/resources` topolojiiçeren kavanoz dosyasının içindeki dizinde olmasını bekler. Bu örnek, Python komut dosyalarını dizinde `/multilang/resources` depolar. Aşağıdaki `pom.xml` XML kullanarak bu dosyayı içerir:

```xml
<!-- include the Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

Daha önce de belirtildiği gibi, Fırtına için Thrift tanımını uygulayan bir `storm.py` dosya var. Akı çerçevesi, `storm.py` proje oluşturulurken otomatik olarak içerir, böylece projeyi dahil etme konusunda endişelenmenize gerek kalmaz.

## <a name="build-the-project"></a>Projeyi derleme

1. Projeyi ' [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount)den indirin.

1. Komut istemini açın ve proje `hdinsight-python-storm-wordcount-master`köküne gidin: . Aşağıdaki komutu girin:

    ```cmd
    mvn clean compile package
    ```

    Bu komut, `target/WordCount-1.0-SNAPSHOT.jar` derlenmiş topolojiiçeren bir dosya oluşturur.

## <a name="run-the-storm-topology-on-hdinsight"></a>HDInsight'ta Fırtına topolojisini çalıştırın

1. DOSYAYI HDInsight kümesinde `WordCount-1.0-SNAPSHOT.jar` Storm'a kopyalamak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    scp target/WordCount-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

1. Dosya yüklendikten sonra SSH kullanarak kümeye bağlanın:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. SSH oturumundan kümedeki topolojiyi başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

    Bir kez başladıktan sonra, bir Fırtına topolojisi durdurulana kadar çalışır.

1. Kümedeki topolojiyi görüntülemek için Fırtına UI'sini kullanın. Fırtına UI yer `https://CLUSTERNAME.azurehdinsight.net/stormui`almaktadır. Küme `CLUSTERNAME` adınız ile değiştirin.

1. Fırtına topolojisini durdurun. Kümedeki topolojiyi durdurmak için aşağıdaki komutu kullanın:

    ```bash
    storm kill wordcount
    ```

    Alternatif olarak, Fırtına UI'ı kullanabilirsiniz. Topoloji için **Topoloji eylemleri** altında, **Kill**seçin.

## <a name="run-the-topology-locally"></a>Topolojiyi yerel olarak çalıştırın

Topolojiyi yerel olarak çalıştırmak için aşağıdaki komutu kullanın:

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]  
> Bu komut yerel bir Fırtına geliştirme ortamı gerektirir. Daha fazla bilgi için [bkz.](https://storm.apache.org/releases/current/Setting-up-development-environment.html)

Topoloji başladıktan sonra, aşağıdaki metne benzer şekilde yerel konsola bilgi yayar:

```output
24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting the cow jumped over the moon
24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}
```

Topolojiyi durdurmak için __Ctrl + C__'yi kullanın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight ile Python'u kullanmanın diğer yolları için aşağıdaki belgelere bakın: [Apache Pig ve Apache Hive'da Python Kullanıcı Tanımlı Fonksiyonlar (UDF) nasıl kullanılır?](../hadoop/python-udf-hdinsight.md)
