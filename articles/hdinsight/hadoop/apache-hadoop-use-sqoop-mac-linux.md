---
title: Apache Hadoop ile Apache Sqoop - Azure HDInsight
description: HDInsight'ta Apache Hadoop ile Azure SQL Veritabanı arasında alma ve dışa aktarma için Apache Sqoop'u nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/28/2019
ms.openlocfilehash: 21bc903349876a76576fb742840e9899f9d94bcd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74769396"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>HDInsight üzerinde Apache Hadoop ile SQL Veritabanı arasında veri içeri ve dışarı aktarma işlemleri için Apache Sqoop'u kullanma

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Azure HDInsight ve Azure SQL Veritabanı veya Microsoft SQL Server veritabanında bir Apache Hadoop kümesi arasında alma ve dışa aktarma için Apache Sqoop'u nasıl kullanacağınızı öğrenin. Bu belgedeki adımlar, `sqoop` hadoop kümesinin headnode doğrudan komutunu kullanır. Kafa düğümüne bağlanmak ve bu belgedeki komutları çalıştırmak için SSH'yi kullanırsınız. Bu makale, [HDInsight Hadoop ile Kullanım Apache Sqoop](./hdinsight-use-sqoop.md)bir devamıdır.

## <a name="prerequisites"></a>Ön koşullar

* [HDInsight'ta Hadoop ile Apache Sqoop'u Kullanın'dan](./hdinsight-use-sqoop.md) [test ortamını ayarlama.](./hdinsight-use-sqoop.md#create-cluster-and-sql-database)

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

* Sqoop'a aşinalık. Daha fazla bilgi için [Sqoop Kullanım Kılavuzu'na](https://sqoop.apache.org/docs/1.4.7/SqoopUserGuide.html)bakın.

## <a name="set-up"></a>Ayarla

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Kullanım kolaylığı için değişkenleri ayarlayın. `PASSWORD`Değiştirin `MYSQLSERVER`, `MYDATABASE` ve ilgili değerlerle ve ardından aşağıdaki komutları girin:

    ```bash
    export password='PASSWORD'
    export sqlserver="MYSQLSERVER"
    export database="MYDATABASE"


    export serverConnect="jdbc:sqlserver://$sqlserver.database.windows.net:1433;user=sqluser;password=$password"
    export serverDbConnect="jdbc:sqlserver://$sqlserver.database.windows.net:1433;user=sqluser;password=$password;database=$database"
    ```

## <a name="sqoop-export"></a>Sqoop ihracat

Hive'dan SQL Server'a.

1. Sqoop'un SQL Veritabanınızı görebileceğini doğrulamak için, açık SSH bağlantınıza aşağıdaki komutu girin. Bu komut, veritabanlarının listesini döndürür.

    ```bash
    sqoop list-databases --connect $serverConnect
    ```

1. Belirtilen veritabanı için tabloların listesini görmek için aşağıdaki komutu girin:

    ```bash
    sqoop list-tables --connect $serverDbConnect
    ```

1. Hive `hivesampletable` tablosundaki verileri SQL `mobiledata` Veritabanı'ndaki tabloya aktarmak için, açık SSH bağlantınıza aşağıdaki komutu girin:

    ```bash
    sqoop export --connect $serverDbConnect \
    -table mobiledata \
    --hcatalog-table hivesampletable
    ```

1. Verilerin dışa aktarıldığını doğrulamak için, dışa aktarılan verileri görüntülemek için SSH bağlantınızdan aşağıdaki sorguları kullanın:

    ```bash
    sqoop eval --connect $serverDbConnect \
    --query "SELECT COUNT(*) from dbo.mobiledata WITH (NOLOCK)"


    sqoop eval --connect $serverDbConnect \
    --query "SELECT TOP(10) * from dbo.mobiledata WITH (NOLOCK)"
    ```

## <a name="sqoop-import"></a>Sqoop ithalat

SQL Server'dan Azure depolamaalanına.

1. SQL Veritabanı'ndaki `mobiledata` tablodan HDInsight `wasbs:///tutorials/usesqoop/importeddata` dizinine veri almak için açık SSH bağlantınıza aşağıdaki komutu girin. Verilerdeki alanlar bir sekme karakteriyle ayrılır ve satırlar yeni bir satır karakteri tarafından sonlandırılır.

    ```bash
    sqoop import --connect $serverDbConnect \
    --table mobiledata \
    --target-dir 'wasb:///tutorials/usesqoop/importeddata' \
    --fields-terminated-by '\t' \
    --lines-terminated-by '\n' -m 1
    ```

1. Alternatif olarak, bir Kovan tablosu da belirtebilirsiniz:

    ```bash
    sqoop import --connect $serverDbConnect \
    --table mobiledata \
    --target-dir 'wasb:///tutorials/usesqoop/importeddata2' \
    --fields-terminated-by '\t' \
    --lines-terminated-by '\n' \
    --create-hive-table \
    --hive-table mobiledata_imported2 \
    --hive-import -m 1
    ```

1. Alma işlemi tamamlandıktan sonra, yeni dizindeki verileri listelemek için açık SSH bağlantınıza aşağıdaki komutu girin:

    ```bash
    hadoop fs -tail /tutorials/usesqoop/importeddata/part-m-00000
    ```

1. Tablonun Hive'da oluşturulduğunu doğrulamak için [beeline](./apache-hadoop-use-hive-beeline.md) kullanın.

    1. Bağlan

        ```bash
        beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
        ```

    1. Her sorguyu teker teker çalıştırın ve çıktıyı gözden geçirin:

        ```hql
        show tables;
        describe mobiledata_imported2;
        SELECT COUNT(*) FROM mobiledata_imported2;
        SELECT * FROM mobiledata_imported2 LIMIT 10;
        ```

    1. Çıkış beeline `!exit`ile .

## <a name="limitations"></a>Sınırlamalar

* Toplu dışa aktarma - Linux tabanlı HDInsight ile, Verileri Microsoft SQL Server veya Azure SQL Veritabanı'na aktarmak için kullanılan Sqoop bağlayıcısı toplu ekleri desteklemez.

* Toplu İşlem - Linux tabanlı HDInsight `-batch` ile, eklemeleri gerçekleştirirken anahtarı kullanırken, Sqoop ekleme işlemlerini toplu hale getirme yerine birden çok kesici uç yapar.

## <a name="important-considerations"></a>Önemli noktalar

* Hem HDInsight hem de SQL Server aynı Azure Sanal Ağ'da olmalıdır.

    Örneğin, [şirket içi ağ belgenize HDInsight'ı bağlayın.](./../connect-on-premises-network.md)

    Bir Azure Sanal Ağı ile HDInsight kullanma hakkında daha fazla bilgi için Azure Sanal Ağ ile [HDInsight'ı Genişlet'e](../hdinsight-plan-virtual-network-deployment.md) bakın. Azure Sanal Ağı hakkında daha fazla bilgi için [Sanal Ağa Genel Bakış](../../virtual-network/virtual-networks-overview.md) belgesine bakın.

* SQL Server, SQL kimlik doğrulamasına izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için [Kimlik Doğrulama Modu](https://msdn.microsoft.com/ms144284.aspx) seç belgesine bakın.

* Uzak bağlantıları kabul etmek için SQL Server'ı yapılandırmanız gerekebilir. Daha fazla bilgi için [SQL Server veritabanı altyapısı belgesine bağlanma](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) da sorun giderme sorununa bakın.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Sqoop'u kullanmayı öğrendin. Daha fazla bilgi için bkz:

* [HDInsight ile Apache Oozie kullanın:](../hdinsight-use-oozie-linux-mac.md)Bir Oozie iş akışında Sqoop eylem kullanın.
* [HDInsight kullanarak uçuş gecikme verilerini analiz edin](../interactive-query/interactive-query-tutorial-analyze-flight-data.md): Uçuş gecikme verilerini analiz etmek için Etkileşimli Sorgu'yı kullanın ve ardından verileri Azure SQL veritabanına aktarmak için Sqoop'u kullanın.
* [HDInsight'a veri yükleme](../hdinsight-upload-data.md): HDInsight/Azure Blob depolamasına veri yüklemek için başka yöntemler bulun.
