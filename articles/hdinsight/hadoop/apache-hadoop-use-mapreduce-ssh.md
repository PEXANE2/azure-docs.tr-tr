---
title: Apache Hadoop ile MapReduce ve SSH bağlantısı - Azure HDInsight
description: HDInsight'ta Apache Hadoop kullanarak MapReduce işlerini çalıştırmak için SSH'yi nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/10/2020
ms.openlocfilehash: 543bc29adc85bd767de9479607d067fadf7b0078
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934710"
---
# <a name="use-mapreduce-with-apache-hadoop-on-hdinsight-with-ssh"></a>SSH ile HDInsight'ta Apache Hadoop ile MapReduce'ı kullanın

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Secure Shell (SSH) bağlantısından HDInsight'a MapReduce işlerini nasıl göndereceğinizi öğrenin.

> [!NOTE]
> Linux tabanlı Apache Hadoop sunucularını kullanmaya zaten aşinaysanız, ancak HDInsight'ta yeniyseniz, [Linux tabanlı HDInsight ipuçlarına](../hdinsight-hadoop-linux-information.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

HDInsight'ta bir Apache Hadoop kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun.](../hdinsight-hadoop-create-linux-clusters-portal.md)

## <a name="use-hadoop-commands"></a>Hadoop komutlarını kullanma

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. HDInsight kümesine bağlandıktan sonra, MapReduce işini başlatmak için aşağıdaki komutu kullanın:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    Bu komut, `wordcount` `hadoop-mapreduce-examples.jar` dosyada bulunan sınıfı başlatır. Belgeyi `/example/data/gutenberg/davinci.txt` giriş olarak kullanır ve çıktı `/example/data/WordCountOutput`.

    > [!NOTE]
    > Bu MapReduce işi ve örnek veriler hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop'ta MapReduce'i kullanın.](hdinsight-use-mapreduce.md)

    İş, işlerken ayrıntıları yayar ve iş tamamlandığında aşağıdaki metne benzer bilgileri döndürür:

    ```output
    File Input Format Counters
    Bytes Read=1395666
    File Output Format Counters
    Bytes Written=337623
    ```

1. İş tamamlandığında, çıktı dosyalarını listelemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Bu komut iki `_SUCCESS` dosya `part-r-00000`görüntüler ve . Dosya, `part-r-00000` bu işin çıktısını içerir.

    > [!NOTE]  
    > Bazı MapReduce işleri sonuçları birden çok **part-r-#######** dosyasına bölebilir. Bu durumda, dosyaların sırasını belirtmek için ##### soneki kullanın.

1. Çıktıyı görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Bu komut, **wasbs://example/data/gutenberg/davinci.txt** dosyasında bulunan sözcüklerin listesini ve her sözcüğün kaç kez oluştuğunu görüntüler. Aşağıdaki metin, dosyada bulunan verilere bir örnektir:

    ```output
    wreathed        3
    wreathing       1
    wreaths         1
    wrecked         3
    wrenching       1
    wretched        6
    wriggling       1
    ```

## <a name="next-steps"></a>Sonraki adımlar

Gördüğünüz gibi, Hadoop komutları MapReduce işlerini bir HDInsight kümesinde çalıştırmak ve ardından iş çıktısını görüntülemek için kolay bir yol sağlar. HDInsight'ta Hadoop ile çalışabileceğiniz diğer yollar hakkında bilgi için:

* [HDInsight Hadoop'ta MapReduce'i kullanın](hdinsight-use-mapreduce.md)
* [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hdinsight-use-hive.md)
