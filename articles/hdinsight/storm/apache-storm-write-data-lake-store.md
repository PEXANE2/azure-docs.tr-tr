---
title: 'Öğretici: HDInsight Apache Storm to Storage - Azure/Veri Gölü'
description: Öğretici - Azure HDInsight için HDFS uyumlu depolama alanına yazmak için Apache Storm'u nasıl kullanacağınızı öğrenin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 06/24/2019
ms.openlocfilehash: 579163180f6c7ba19927ca66d20bd92d1b2de52e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73241201"
---
# <a name="tutorial-write-to-apache-hadoop-hdfs-from-apache-storm-on-azure-hdinsight"></a>Öğretici: Azure HDInsight'ta Apache Storm'dan Apache Hadoop HDFS'ye yazın

Bu öğretici, Apache Storm tarafından HDInsight'ta kullanılan HDFS uyumlu depolama alanına veri yazmak için Apache Storm'un nasıl kullanılacağını göstermektedir. HDInsight, hem Azure Depolama'yı hem de Azure Veri Gölü Depolama'yı HDFS uyumlu depolama alanı olarak kullanabilir. Storm, HDFS'ye veri yazan bir [HdfsBolt](https://storm.apache.org/releases/current/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) bileşeni sağlar. Bu belge, HdfsBolt'tan her iki depolama türüne de yazma hakkında bilgi sağlar.

Bu belgede kullanılan örnek topoloji, HDInsight'ta Storm ile birlikte bulunan bileşenlere dayanır. Diğer Apache Storm kümeleriyle kullanıldığında Azure Veri Gölü Depolamaile çalışmak için değişiklik yapılması gerekebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Kümeyi komut dosyası eylemiyle yapılandırma
> * Topolojiyi oluşturun ve paketle
> * Topolojiyi dağıtın ve çalıştırın
> * Çıktı verilerini görüntüleme
> * Topolojiyi durdurma

## <a name="prerequisites"></a>Ön koşullar

* [Java Geliştirici Kiti (JDK) sürüm 8](https://aka.ms/azure-jdks)

* [Apache Maven](https://maven.apache.org/download.cgi) düzgün Apache göre [yüklü.](https://maven.apache.org/install.html)  Maven Java projeleri için bir proje inşa sistemidir.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

* Kümeleriniz birincil depolama için [URI düzeni.](../hdinsight-hadoop-linux-information.md#URI-and-scheme) Bu, `wasb://` Azure Depolama, `abfs://` Azure Veri Gölü Depolama `adl://` Gen2 veya Azure Veri Gölü Depolama Gen1 için olacaktır. Azure Depolama için güvenli aktarım etkinleştirilirse, URI `wasbs://`.  Ayrıca bakınız, [güvenli aktarım.](../../storage/common/storage-require-secure-transfer.md)

### <a name="example-configuration"></a>Örnek yapılandırma

Aşağıdaki YAML, örnekte `resources/writetohdfs.yaml` yer alan dosyadan bir alıntıdır. Bu dosya, Apache Storm için [Flux](https://storm.apache.org/releases/current/flux.html) çerçevesini kullanarak Fırtına topolojisini tanımlar.

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  # Rotate files when they hit 5 MB
  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.FileSizeRotationPolicy"
    constructorArgs:
      - 5
      - "MB"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

Bu YAML aşağıdaki öğeleri tanımlar:

* `syncPolicy`: Dosyaların dosya sistemine senkronize edilmesi/silinmesini tanımlar. Bu örnekte, her 1000 tuples.
* `fileNameFormat`: Dosya yazarken kullanılacak yol ve dosya adı deseni tanımlar. Bu örnekte, yol çalışma zamanında bir filtre kullanılarak sağlanır `.txt`ve dosya uzantısı.
* `recordFormat`: Yazılan dosyaların iç biçimini tanımlar. Bu örnekte, alanlar `|` karakterle sınırlandırılır.
* `rotationPolicy`: Dosyaları ne zaman döndüreceklerini tanımlar. Bu örnekte, hiçbir döndürme gerçekleştirilir.
* `hdfs-bolt`: Önceki bileşenleri sınıf için `HdfsBolt` yapılandırma parametreleri olarak kullanır.

Flux çerçevesi hakkında daha fazla [https://storm.apache.org/releases/current/flux.html](https://storm.apache.org/releases/current/flux.html)bilgi için bkz.

## <a name="configure-the-cluster"></a>Kümeyi yapılandırma

Varsayılan olarak, HDInsight'taki Storm, `HdfsBolt` Storm'un sınıf yolunda Azure Depolama veya Veri Gölü Depolama ile iletişim kurmak için kullanan bileşenleri içermez. Kümenizdeki Fırtına `extlib` dizinine bu bileşenleri eklemek için aşağıdaki komut dosyası eylemini kullanın:

| Özellik | Değer |
|---|---|
|Komut dosyası türü |- Özel|
|Bash script URI |`https://hdiconfigactions.blob.core.windows.net/linuxstormextlibv01/stormextlib.sh`|
|Düğüm türü(ler) |Nimbus, Süpervizör|
|Parametreler |None|

Bu komut dosyasını kümenizle kullanma hakkında daha fazla bilgi için, komut dosyası eylemleri belgesini [kullanarak HDInsight kümelerini Özelleştir'e](./../hdinsight-hadoop-customize-cluster-linux.md) bakın.

## <a name="build-and-package-the-topology"></a>Topolojiyi oluşturun ve paketle

1. Örnek projeyi [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) geliştirme ortamınıza indirin.

2. Komut istemi, terminal veya kabuk oturumundan dizinleri indirilen projenin köküne değiştirin. Topolojioluşturmak ve paketlemek için aşağıdaki komutu kullanın:

    ```cmd
    mvn compile package
    ```

    Yapı ve paketleme tamamlandıktan sonra, adlı `target` `StormToHdfs-1.0-SNAPSHOT.jar`yeni bir dizin vardır. Bu dosya derlenmiş topolojiiçerir.

## <a name="deploy-and-run-the-topology"></a>Topolojiyi dağıtın ve çalıştırın

1. Topolojiyi HDInsight kümesine kopyalamak için aşağıdaki komutu kullanın. Kümenin adı ile değiştirin. `CLUSTERNAME`

    ```cmd
    scp target\StormToHdfs-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs-1.0-SNAPSHOT.jar
    ```

1. Yükleme tamamlandıktan sonra, SSH kullanarak HDInsight kümesine bağlanmak için aşağıdakileri kullanın. Kümenin adı ile değiştirin. `CLUSTERNAME`

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Bağlandıktan sonra, aşağıdaki komutu kullanarak `dev.properties`aşağıdaki leri kullanarak aşağıdaki leri kullanarak aşağıdaki leri kullanarak aşağıdaki leri kullanarak:

    ```bash
    nano dev.properties
    ```

1. `dev.properties` Aşağıdaki metni dosyanın içeriği olarak kullanın. [URI şemanıza](../hdinsight-hadoop-linux-information.md#URI-and-scheme)göre gerektiği gibi gözden geçirin.

    ```
    hdfs.write.dir: /stormdata/
    hdfs.url: wasbs:///
    ```

    Dosyayı kaydetmek için __Ctrl + X__, sonra __Y__, ve son olarak __girin__. Bu dosyadaki değerler depolama URL'sini ve verilerin yazıldığı dizin adını ayarlar.

1. Topolojiyi başlatmak için aşağıdaki komutu kullanın:

    ```bash
    storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties
    ```

    Bu komut, topolojiyi kümenin Nimbus düğümüne göndererek Akı çerçevesini kullanarak başlatır. Topoloji, kavanozda `writetohdfs.yaml` bulunan dosya ile tanımlanır. Dosya `dev.properties` filtre olarak geçirilir ve dosyada bulunan değerler topoloji tarafından okunur.

## <a name="view-output-data"></a>Çıktı verilerini görüntüleme

Verileri görüntülemek için aşağıdaki komutu kullanın:

  ```bash
  hdfs dfs -ls /stormdata/
  ```

Bu topoloji tarafından oluşturulan dosyaların listesi görüntülenir. Aşağıdaki liste, önceki komutlar tarafından döndürülen verilere bir örnektir:

```output
Found 23 items
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-0-1561407909895.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-1-1561407915577.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-10-1561407943327.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-11-1561407946312.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-12-1561407949320.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-13-1561407952662.txt
-rw-r--r--   1 storm supergroup    5242880 2019-06-24 20:25 /stormdata/hdfs-bolt-3-14-1561407955502.txt
```

## <a name="stop-the-topology"></a>Topolojiyi durdurma

Fırtına topolojileri durdurulana veya küme silinene kadar çalışır. Topolojiyi durdurmak için aşağıdaki komutu kullanın:

```bash
storm kill hdfswriter
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici ile oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve sonra __Kaynak Grupları__'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Apache Storm tarafından HDInsight'ta kullanılan HDFS uyumlu depolama alanına veri yazmak için Apache Storm'u nasıl kullanacağınızı öğrendiniz.

> [!div class="nextstepaction"]
> HDInsight için diğer [Apache Storm örneklerini](apache-storm-example-topology.md) keşfedin