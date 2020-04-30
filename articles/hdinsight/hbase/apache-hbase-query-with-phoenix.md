---
title: 'Hızlı başlangıç: Apache HBase & Apache Phoenix-Azure HDInsight'
description: Bu hızlı başlangıçta, HDInsight 'ta Apache Phoenix kullanmayı öğreneceksiniz. Ayrıca, HDInsight 'ta bir HBase kümesine bağlanmak için bilgisayarınıza SQLLine yüklemeyi ve ayarlamayı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: 1c400e41c4c10023d2595bde8c0d62e26184cf05
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79370330"
---
# <a name="quickstart-query-apache-hbase-in-azure-hdinsight-with-apache-phoenix"></a>Hızlı başlangıç: Apache Phoenix ile Azure HDInsight 'ta Apache HBase 'i sorgulama

Bu hızlı başlangıçta, Azure HDInsight 'ta HBase sorgularını çalıştırmak için Apache Phoenix nasıl kullanacağınızı öğreneceksiniz. Apache Phoenix, Apache HBase için bir SQL sorgu altyapısıdır. Buna JDBC sürücüsü olarak erişilir ve bu SQL kullanarak HBase tablolarını sorgulamayı ve yönetmeyi sağlar. [Sqlline](http://sqlline.sourceforge.net/) , SQL yürütmek için bir komut satırı yardımcı programıdır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Apache HBase kümesi. HDInsight kümesi oluşturmak için bkz. [küme oluşturma](../hadoop/apache-hadoop-linux-tutorial-get-started.md) .  **HBase** küme türünü seçtiğinizden emin olun.

* Bir SSH istemcisi. Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="identify-a-zookeeper-node"></a>ZooKeeper düğümünü tanımla

Bir HBase kümesine bağlandığınızda Apache ZooKeeper düğümlerinden birine bağlanmanız gerekir. Her HDInsight kümesinde üç ZooKeeper düğümü vardır. Kıvrımlı, bir ZooKeeper düğümünü hızlı bir şekilde tanımlamak için kullanılabilir. Ve `PASSWORD` `CLUSTERNAME` değerlerini ilgili değerlerle değiştirerek aşağıdaki kıvrımlı komutunu düzenleyin ve komut istemine komutu girin:

```cmd
curl -u admin:PASSWORD -sS -G https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER
```

Çıktının bir kısmı şuna benzer olacaktır:

```output
    {
      "href" : "http://hn1-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net:8080/api/v1/clusters/myCluster/hosts/zk0-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net/host_components/ZOOKEEPER_SERVER",
      "HostRoles" : {
        "cluster_name" : "myCluster",
        "component_name" : "ZOOKEEPER_SERVER",
        "host_name" : "zk0-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net"
      }
```

Daha sonra kullanmak üzere için `host_name` değerini bir yere göz atın.

## <a name="create-a-table-and-manipulate-data"></a>Tablo oluşturma ve verileri işleme

HBase kümelerine bağlanmak için SSH kullanabilir ve sonra HBase tabloları oluşturmak, veri eklemek ve verileri sorgulamak için Apache Phoenix kullanabilirsiniz.

1. HBase kümenize bağlanmak için komutunu kullanın `ssh` . Aşağıdaki komutu, kümenizin adıyla değiştirerek `CLUSTERNAME` düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Dizini Phoenix istemcisine değiştirin. Aşağıdaki komutu girin:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

3. [Sqlline](http://sqlline.sourceforge.net/)'ı başlatın. Daha önce tanımlanan ZooKeeper düğümüyle değiştirerek `ZOOKEEPER` aşağıdaki komutu düzenleyin, sonra şu komutu girin:

    ```bash
    ./sqlline.py ZOOKEEPER:2181:/hbase-unsecure
    ```

4. Bir HBase tablosu oluşturun. Aşağıdaki komutu girin:

    ```sql
    CREATE TABLE Company (company_id INTEGER PRIMARY KEY, name VARCHAR(225));
    ```

5. HBase 'deki tüm `!tables` tabloları listelemek Için sqlline komutunu kullanın. Aşağıdaki komutu girin:

    ```sqlline
    !tables
    ```

6. Tablodaki değerleri ekleyin. Aşağıdaki komutu girin:

    ```sql
    UPSERT INTO Company VALUES(1, 'Microsoft');
    UPSERT INTO Company VALUES(2, 'Apache');
    ```

7. Tabloyu sorgulayın. Aşağıdaki komutu girin:

    ```sql
    SELECT * FROM Company;
    ```

8. Bir kaydı silin. Aşağıdaki komutu girin:

    ```sql
    DELETE FROM Company WHERE COMPANY_ID=1;
    ```

9. Tabloyu bırakın. Aşağıdaki komutu girin:

    ```hbase
    DROP TABLE Company;
    ```

10. SQLLine programından çıkmak `!quit` Için sqlline komutunu kullanın. Aşağıdaki komutu girin:

    ```sqlline
    !quit
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz. [tarayıcınızı, PowerShell 'i veya Azure CLI 'yı kullanarak HDInsight kümesini silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure HDInsight 'ta HBase sorgularını çalıştırmak için Apache Phoenix nasıl kullanacağınızı öğrendiniz. Apache Phoenix hakkında daha fazla bilgi edinmek için sonraki makalede daha derin bir inceleme sunulacaktır.

> [!div class="nextstepaction"]
> [HDInsight üzerinde Apache Phoenix](../hdinsight-phoenix-in-hdinsight.md)
