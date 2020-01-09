---
title: Azure HDInsight 'ta etkileşimli Spark kabuğu kullanma
description: Etkileşimli bir Spark kabuğu, Spark komutlarının tek seferde çalıştırılması ve sonuçları görmek için bir okuma-yürütme-yazdırma işlemi sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/12/2019
ms.openlocfilehash: f088b8210b8170d22e84d131f0a72f5f8caa3b92
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435222"
---
# <a name="run-apache-spark-from-the-spark-shell"></a>Spark kabuğu 'ndan Apache Spark çalıştırma

Etkileşimli bir [Apache Spark](https://spark.apache.org/) kabuğu Spark komutlarının tek seferde çalıştırılması ve sonuçları görmesi IÇIN bir REPL (okuma-yürütme-yazdırma döngüsü) ortamı sağlar. Bu işlem, geliştirme ve hata ayıklama için kullanışlıdır. Spark, desteklenen dillerin her biri için bir kabuk sağlar: Scala, Python ve R.

## <a name="run-an-apache-spark-shell"></a>Apache Spark kabuğu çalıştırma

1. Kümenize bağlanmak için [SSH komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Spark, Scala (Spark-Shell) ve Python (pyspark) için kabuklar sağlar. SSH oturumunda aşağıdaki komutlardan birini girin:

    ```bash
    spark-shell
    pyspark
    ```

    Artık Spark komutlarını uygun dilde girebilirsiniz.

1. Birkaç temel örnek komut:

    ```scala
    // Load data
    var data = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/HdiSamples/HdiSamples/SensorSampleData/building/building.csv")

    // Show data
    data.show()

    // Select certain columns
    data.select($"BuildingID", $"Country").show(10)

    // exit shell
    :q
    ```

## <a name="sparksession-and-sparkcontext-instances"></a>Mini oturum ve mini bağlam örnekleri

Spark kabuğunu çalıştırdığınızda varsayılan olarak, sizin için otomatik olarak Mini oturum ve parlak bağlam örnekleri oluşturulur.

Mini erişimli oturum örneğine erişmek için `spark`girin. Mini bağlam örneğine erişmek için `sc`girin.

## <a name="important-shell-parameters"></a>Önemli kabuk parametreleri

Spark Shell komutu (`spark-shell`veya `pyspark`) birçok komut satırı parametresini destekler. Parametrelerin tam listesini görmek için Spark kabuğunu anahtar `--help`başlatın. Bu parametrelerden bazıları yalnızca Spark kabuğu 'nun sarmaladığı `spark-submit`için uygulanabilir.

| anahtarı | açıklama | Örnek |
| --- | --- | --- |
| --Master MASTER_URL | Ana URL 'YI belirtir. HDInsight 'ta bu değer her zaman `yarn`. | `--master yarn`|
| --jars JAR_LIST | Sürücü ve yürütücü Sınıfyoluna dahil etmek için yerel jar dosyaları dışındaki 'ın virgülle ayrılmış listesi. HDInsight 'ta, bu liste Azure Storage veya Data Lake Storage varsayılan dosya sistemine yönelik yollardan oluşur. | `--jars /path/to/examples.jar` |
| --paketler MAVEN_COORDS | Sürücü ve yürütücü Sınıfyoluna dahil etmek için jar dosyaları dışındaki 'ın Maven koordinatlarının virgülle ayrılmış listesi. Yerel Maven depolarında, Maven Central, ardından `--repositories`ile belirtilen diğer uzak depolarda arama yapar. Koordinatlar için biçim *GroupID*:*ArtifactId*:*Version*. | `--packages "com.microsoft.azure:azure-eventhubs:0.14.0"`|
| --Kopyala-Dosyalar LISTESI | Yalnızca Python için, PYTHONPATH üzerine yerleştirilecek. zip,. yumurg veya. Kopyala dosyalarının virgülle ayrılmış bir listesi. | `--pyfiles "samples.py"` |

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Azure HDInsight 'ta Apache Spark giriş](apache-spark-overview.md) .
- Spark kümeleri ve parlak SQL ile çalışmak için bkz. [Azure HDInsight 'ta Apache Spark kümesi oluşturma](apache-spark-jupyter-spark-sql.md) .
- Spark ile akış verilerini işleyen uygulamalar yazmak için bkz. [Apache Spark yapısal akışı nedir?](apache-spark-streaming-overview.md) .
