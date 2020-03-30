---
title: "Quickstart: Azure HDInsight'ta Apache HBase sorgula - HBase Shell"
description: Bu hızlı başlangıçta, Apache HBase sorgularını çalıştırmak için Apache HBase Shell'i nasıl kullanacağınızı öğrenirsiniz.
keywords: hdinsight,hadoop,HBase
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: 572262cbece26171f9a67bf073906fa2dfd4d8e1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79371078"
---
# <a name="quickstart-query-apache-hbase-in-azure-hdinsight-with-hbase-shell"></a>Quickstart: HBase Shell ile Azure HDInsight'ta Apache HBase sorgula

Bu hızlı başlatmada, Bir HBase tablosu oluşturmak, veri eklemek ve sonra tabloyu sorgulamak için Apache HBase Shell'i nasıl kullanacağınızı öğrenirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Bir Apache HBase kümesi. Bkz. HDInsight kümesi oluşturmak için [Küme Oluştur'a](../hadoop/apache-hadoop-linux-tutorial-get-started.md) bakın.  **HBase** küme türünü seçtiğinizden emin olun.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="create-a-table-and-manipulate-data"></a>Tablo oluşturma ve verileri işleme

Çoğu kişi için veriler tablo biçiminde görünür:

![HDInsight Apache HBase tabular veri](./media/query-hbase-with-hbase-shell/hdinsight-hbase-contacts-tabular.png)

HBase 'de [(Cloud BigTable'ın](https://cloud.google.com/bigtable/)bir uygulaması) aynı veriler aşağıdaki gibi görünür:

![HDInsight Apache HBase BigTable verileri](./media/query-hbase-with-hbase-shell/hdinsight-hbase-contacts-bigtable.png)

HBase kümelerine bağlanmak için SSH'yi kullanabilir ve ardından HBase tabloları oluşturmak, veri eklemek ve sorgu verileri oluşturmak için Apache HBase Shell'i kullanabilirsiniz.

1. HBase kümenize bağlanmak için komutu kullanın. `ssh` Kümenizin adını değiştirerek `CLUSTERNAME` aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. HBase etkileşimli kabuğunu başlatmak için komutu kullanın. `hbase shell` SSH bağlantınızda aşağıdaki komutu girin:

    ```bash
    hbase shell
    ```

3. İki `create` sütunlu ailelerden bir HBase tablosu oluşturmak için komutu kullanın. Aşağıdaki komutu girin:

    ```hbase
    create 'Contacts', 'Personal', 'Office'
    ```

4. HBase'deki tüm tabloları listelemek için komutu kullanın. `list` Aşağıdaki komutu girin:

    ```hbase
    list
    ```

5. Belirli `put` bir tabloda belirli bir satırda belirli bir sütunda değerleri eklemek için komutu kullanın. Aşağıdaki komutu girin:

    ```hbase
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    ```

6. Tablo `scan` verilerini tarayıp döndürmek `Contacts` için komutu kullanın. Aşağıdaki komutu girin:

    ```hbase
    scan 'Contacts'
    ```

7. Bir `get` satırın içeriğini getirmek için komutu kullanın. Aşağıdaki komutu girin:

    ```hbase
    get 'Contacts', '1000'
    ```

    Yalnızca bir satır olduğundan, benzer sonuçları komutu `scan` kullanmak olarak görürsünüz.

8. Tablodaki hücre değerini silmek için komutu kullanın. `delete` Aşağıdaki komutu girin:

    ```hbase
    delete 'Contacts', '1000', 'Office:Address'
    ```

9. Tabloyu devre dışı kalmak için komutu kullanın. `disable` Aşağıdaki komutu girin:

    ```hbase
    disable 'Contacts'
    ```

10. HBase'den bir tablo düşürmek için komutu kullanın. `drop` Aşağıdaki komutu girin:

    ```hbase
    drop 'Contacts'
    ```

11. HBase etkileşimli kabuğunu durdurmak için komutu kullanın. `exit` Aşağıdaki komutu girin:

    ```hbase
    exit
    ```

HBase tablo şeması hakkında daha fazla bilgi için, [Bkz. Apache HBase Şema Tasarıma Giriş](http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf). HBase komutları hakkında daha fazla bilgi için bkz. [Apache HBase başvuru kılavuzu](https://hbase.apache.org/book.html#quickstart).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [Tarayıcınızı, PowerShell'i veya Azure CLI'yi kullanarak bir HDInsight kümesini sil.](../hdinsight-delete-cluster.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Bir HBase tablosu oluşturmak, veri eklemek ve sonra tabloyu sorgulamak için Apache HBase Shell'i nasıl kullanacağınızı öğrendiniz. HBase'de depolanan veriler hakkında daha fazla bilgi edinmek için, bir sonraki makalede Apache Spark ile sorguları nasıl yürüteceğiniz gösterilecek.

> [!div class="nextstepaction"]
> [Apache HBase verilerini okuyup yazmak için Apache Spark kullanma](../hdinsight-using-spark-query-hbase.md)