---
title: 'Hızlı başlangıç: Apache HBase & Apache Phoenix - Azure HDInsight'
description: Bu hızlı başlangıçta, HDInsight'ta Apache Phoenix'in nasıl kullanılacağını öğreneceksiniz. Ayrıca, HDInsight'taki bir HBase kümesine bağlanmak için bilgisayarınıza SQLLine'ı nasıl yükleyip kurabilirsiniz öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: 1c400e41c4c10023d2595bde8c0d62e26184cf05
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79370330"
---
# <a name="quickstart-query-apache-hbase-in-azure-hdinsight-with-apache-phoenix"></a>Quickstart: Apache Phoenix ile Azure HDInsight'ta Apache HBase sorgula

Bu hızlı başlangıçta, Azure HDInsight'ta HBase sorgularını çalıştırmak için Apache Phoenix'i nasıl kullanacağınızı öğrenirsiniz. Apache Phoenix, Apache HBase için bir SQL sorgu motorudur. Buna JDBC sürücüsü olarak erişilir ve bu SQL kullanarak HBase tablolarını sorgulamayı ve yönetmeyi sağlar. [SQLLine,](http://sqlline.sourceforge.net/) SQL yürütmek için bir komut satırı yardımcı programıdır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Bir Apache HBase kümesi. Bkz. HDInsight kümesi oluşturmak için [Küme Oluştur'a](../hadoop/apache-hadoop-linux-tutorial-get-started.md) bakın.  **HBase** küme türünü seçtiğinizden emin olun.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="identify-a-zookeeper-node"></a>ZooKeeper düğümlerini tanımla

Bir HBase kümesine bağlandığınızda, Apache ZooKeeper düğümlerinden birine bağlanmanız gerekir. Her HDInsight kümesinin üç ZooKeeper düğümü vardır. Curl hızlı bir ZooKeeper düğüm tanımlamak için kullanılabilir. Aşağıdaki kıvırma komutunu `PASSWORD` değiştirerek ve `CLUSTERNAME` ilgili değerlerle birlikte düzenleme ve ardından komut istemiyle komutu girin:

```cmd
curl -u admin:PASSWORD -sS -G https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER
```

Çıktının bir kısmı aşağıdakilere benzer:

```output
    {
      "href" : "http://hn1-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net:8080/api/v1/clusters/myCluster/hosts/zk0-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net/host_components/ZOOKEEPER_SERVER",
      "HostRoles" : {
        "cluster_name" : "myCluster",
        "component_name" : "ZOOKEEPER_SERVER",
        "host_name" : "zk0-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net"
      }
```

Daha sonra kullanmak `host_name` için değeri dikkate alın.

## <a name="create-a-table-and-manipulate-data"></a>Tablo oluşturma ve verileri işleme

HBase kümelerine bağlanmak için SSH'yi ve ardından HBase tabloları, veri eklemek ve sorgu verileri oluşturmak için Apache Phoenix'i kullanabilirsiniz.

1. HBase kümenize bağlanmak için komutu kullanın. `ssh` Kümenizin adını değiştirerek `CLUSTERNAME` aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Dizini Phoenix istemcisi olarak değiştirin. Aşağıdaki komutu girin:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

3. [SQLLine'ı](http://sqlline.sourceforge.net/)başlatın. Daha önce tanımlanan ZooKeeper `ZOOKEEPER` düğümlerini değiştirerek aşağıdaki komutu düzenlemeniz ve ardından komutu girin:

    ```bash
    ./sqlline.py ZOOKEEPER:2181:/hbase-unsecure
    ```

4. Bir HBase tablosu oluşturun. Aşağıdaki komutu girin:

    ```sql
    CREATE TABLE Company (company_id INTEGER PRIMARY KEY, name VARCHAR(225));
    ```

5. HBase'deki `!tables` tüm tabloları listelemek için SQLLine komutunu kullanın. Aşağıdaki komutu girin:

    ```sqlline
    !tables
    ```

6. Tabloya değerleri ekleyin. Aşağıdaki komutu girin:

    ```sql
    UPSERT INTO Company VALUES(1, 'Microsoft');
    UPSERT INTO Company VALUES(2, 'Apache');
    ```

7. Tabloyu sorgula. Aşağıdaki komutu girin:

    ```sql
    SELECT * FROM Company;
    ```

8. Bir kaydı silin. Aşağıdaki komutu girin:

    ```sql
    DELETE FROM Company WHERE COMPANY_ID=1;
    ```

9. Masayı bırak. Aşağıdaki komutu girin:

    ```hbase
    DROP TABLE Company;
    ```

10. SQLLine'dan `!quit` çıkmak için SQLLine komutunu kullanın. Aşağıdaki komutu girin:

    ```sqlline
    !quit
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlatmayı tamamladıktan sonra, kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [Tarayıcınızı, PowerShell'i veya Azure CLI'yi kullanarak bir HDInsight kümesini sil.](../hdinsight-delete-cluster.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure HDInsight'ta HBase sorgularını çalıştırmak için Apache Phoenix'i nasıl kullanacağınızı öğrendiniz. Apache Phoenix hakkında daha fazla bilgi edinmek için, bir sonraki makalede daha derin bir inceleme sağlayacaktır.

> [!div class="nextstepaction"]
> [HDInsight üzerinde Apache Phoenix](../hdinsight-phoenix-in-hdinsight.md)
