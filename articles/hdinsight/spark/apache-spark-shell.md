---
title: Azure HDInsight'ta Etkileşimli Kıvılcım Kabuğu Kullanma
description: Etkileşimli Bir Spark Shell, Spark komutlarını teker teker çalıştırmak ve sonuçları görmek için bir okuma-yürütme-yazdırma işlemi sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/10/2020
ms.openlocfilehash: f8737f645df2aefbf9ce544199f0cc45ce6a3d60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162812"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Kıvılcım Kabuğundan Apache Kıvılcımı Çalıştır

Etkileşimli [Apache Spark](https://spark.apache.org/) Shell, Spark komutlarını teker teker çalıştırmak ve sonuçları görmek için bir REPL (okuma-çalıştır-yazdırma döngüsü) ortamı sağlar. Bu işlem geliştirme ve hata ayıklama için yararlıdır. Spark desteklenen dillerin her biri için bir kabuk sağlar: Scala, Python ve R.

## <a name="run-an-apache-spark-shell"></a>Bir Apache Spark Shell çalıştırın

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Kıvılcım, Scala (kıvılcım kabuğu) ve Python (pyspark) için kabuksağlar. SSH oturumunuzda aşağıdaki komutlardan *birini* girin:

    ```bash
    spark-shell

    # Optional configurations
    # spark-shell --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4
    ```

    ```bash
    pyspark

    # Optional configurations
    # pyspark --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4
    ```

    İsteğe bağlı yapılandırma yı kullanmak istiyorsanız, [öncelikle Apache Spark için OutOfMemoryError özel durumlarını](./apache-spark-troubleshoot-outofmemory.md)gözden geçirdiğinizden emin olun.

1. Birkaç temel örnek komutları. İlgili dili seçin:

    ```spark-shell
    val textFile = spark.read.textFile("/example/data/fruits.txt")
    textFile.first()
    textFile.filter(line => line.contains("apple")).show()
    ```

    ```pyspark
    textFile = spark.read.text("/example/data/fruits.txt")
    textFile.first()
    textFile.filter(textFile.value.contains("apple")).show()
    ```

1. CSV dosyasorgulama. Aşağıdaki dilin için `spark-shell` `pyspark`çalıştığını ve .

    ```scala
    spark.read.csv("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv").show()
    ```

1. Bir CSV dosyasorgulama ve değişken sonuçları depolar:

    ```spark-shell
    var data = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv")
    ```

    ```pyspark
    data = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv")
    ```

1. Görüntü sonuçları:

    ```spark-shell
    data.show()
    data.select($"BuildingID", $"Country").show(10)
    ```

    ```pyspark
    data.show()
    data.select("BuildingID", "Country").show(10)
    ```

1. Çık

    ```spark-shell
    :q
    ```

    ```pyspark
    exit()
    ```

## <a name="sparksession-and-sparkcontext-instances"></a>SparkSession ve SparkContext örnekleri

Varsayılan olarak Spark Shell'i çalıştırdığınızda, SparkSession ve SparkContext örnekleri sizin için otomatik olarak anında alınır.

SparkSession örneğine erişmek `spark`için . SparkContext örneğine erişmek `sc`için .

## <a name="important-shell-parameters"></a>Önemli kabuk parametreleri

Kıvılcım Kabuğu komutu`spark-shell`( `pyspark`veya ) birçok komut satırı parametrelerini destekler. Parametrelerin tam listesini görmek için, Anahtarla `--help`Kıvılcım Kabuğunu başlatın. Bu parametrelerden bazıları yalnızca `spark-submit`Spark Shell'in sararolduğu için geçerli olabilir.

| switch | açıklama | örnek |
| --- | --- | --- |
| --usta MASTER_URL | Ana URL'yi belirtir. HDInsight'ta bu değer `yarn`her zaman . | `--master yarn`|
| --kavanoz JAR_LIST | Sürücü ve uygulayıcı sınıf yollarına dahil edilebilmek için virgülle ayrılmış yerel kavanozların listesi. HDInsight'ta bu liste, Azure Depolama veya Veri Gölü Depolama'daki varsayılan dosya sistemine giden yollardan oluşur. | `--jars /path/to/examples.jar` |
| --paketler MAVEN_COORDS | Sürücü ve uygulayıcı sınıf yollarına dahil etmek için kavanozların maven koordinatlarının virgülle ayrılmış listesi. Yerel maven repo arar, sonra maven merkezi, sonra herhangi bir `--repositories`ek uzak depoları ile belirtilir . Koordinatların biçimi *groupId*:*artifactId*:*sürüm*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --py-files LİsteSİ | Yalnızca Python için, PYTHONPATH'e yerleştirilecek .zip, .egg veya .py dosyalarının virgülle ayrılmış bir listesi. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için [Azure HDInsight'ta Apache Spark'a Giriş'e](apache-spark-overview.md) bakın.
- Bkz. Spark kümeleri ve SparkSQL ile çalışmak için [Azure HDInsight'ta bir Apache Spark kümesi oluşturun.](apache-spark-jupyter-spark-sql.md)
- Bkz. [Apache Spark Yapılandırılmış Akış nedir?](apache-spark-streaming-overview.md)
