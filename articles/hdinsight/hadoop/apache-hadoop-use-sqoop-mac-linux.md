---
title: Apache Hadoop-Azure HDInsight ile Apache Sqoop
description: HDInsight ve Azure SQL veritabanı Apache Hadoop arasında içeri ve dışarı aktarmak için Apache Sqoop 'yi nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/28/2019
ms.openlocfilehash: caa4d0d45152b49ee76c3f9795f9a05b888776d6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85319537"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-azure-sql-database"></a>HDInsight ve Azure SQL veritabanı Apache Hadoop arasında veri içeri ve dışarı aktarmak için Apache Sqoop kullanın

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Azure HDInsight ve Azure SQL veritabanı veya Microsoft SQL Server Apache Hadoop kümesi arasında içeri ve dışarı aktarmak için Apache Sqoop 'yi nasıl kullanacağınızı öğrenin. Bu belgedeki adımlar, `sqoop` doğrudan Hadoop kümesinin baş düğümüne öğesinden komutunu kullanır. Baş düğüme bağlanmak ve bu belgedeki komutları çalıştırmak için SSH 'yi kullanırsınız. Bu makalede, [HDInsight 'Ta Hadoop Ile Apache Sqoop kullanma](./hdinsight-use-sqoop.md)işlemi devam ediyor.

## <a name="prerequisites"></a>Ön koşullar

* [Test ortamını ayarlama](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) , [HDInsight 'Ta Hadoop Ile Apache Sqoop kullanın](./hdinsight-use-sqoop.md).

* Bir SSH istemcisi. Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Sqoop ile benzerlik. Daha fazla bilgi için bkz. [Sqoop Kullanıcı Kılavuzu](https://sqoop.apache.org/docs/1.4.7/SqoopUserGuide.html).

## <a name="set-up"></a>Kurulum

1. Kümenize bağlanmak için [SSH komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Kullanım kolaylığı için değişkenleri ayarlayın. , `PASSWORD` `MYSQLSERVER` Ve `MYDATABASE` değerlerini ilgili değerlerle değiştirin ve ardından aşağıdaki komutları girin:

    ```bash
    export password='PASSWORD'
    export sqlserver="MYSQLSERVER"
    export database="MYDATABASE"


    export serverConnect="jdbc:sqlserver://$sqlserver.database.windows.net:1433;user=sqluser;password=$password"
    export serverDbConnect="jdbc:sqlserver://$sqlserver.database.windows.net:1433;user=sqluser;password=$password;database=$database"
    ```

## <a name="sqoop-export"></a>Sqoop dışarı aktarma

Hive 'den SQL 'e.

1. Sqoop 'nin veritabanınızı görebildiğini doğrulamak için, Open SSH bağlantınızda aşağıdaki komutu girin. Bu komut, veritabanlarının bir listesini döndürür.

    ```bash
    sqoop list-databases --connect $serverConnect
    ```

1. Belirtilen veritabanı için Tablo listesini görmek için aşağıdaki komutu girin:

    ```bash
    sqoop list-tables --connect $serverDbConnect
    ```

1. Hive `hivesampletable` tablosundan veritabanınızdaki tabloya veri aktarmak için `mobiledata` , Open SSH bağlantınızda aşağıdaki komutu girin:

    ```bash
    sqoop export --connect $serverDbConnect \
    -table mobiledata \
    --hcatalog-table hivesampletable
    ```

1. Verilerin verildiğini doğrulamak için SSH bağlantınızdan aşağıdaki sorguları kullanarak içe aktarılmış verileri görüntüleyin:

    ```bash
    sqoop eval --connect $serverDbConnect \
    --query "SELECT COUNT(*) from dbo.mobiledata WITH (NOLOCK)"


    sqoop eval --connect $serverDbConnect \
    --query "SELECT TOP(10) * from dbo.mobiledata WITH (NOLOCK)"
    ```

## <a name="sqoop-import"></a>Sqoop içeri aktarma

SQL 'den Azure depolama 'ya.

1. `mobiledata`SQL 'deki tablodaki verileri HDInsight üzerindeki dizine aktarmak için Open SSH bağlantınızın aşağıdaki komutunu girin `wasbs:///tutorials/usesqoop/importeddata` . Verilerdeki alanlar bir sekme karakteriyle ayrılır ve satırlar yeni satır karakteri ile sonlandırılır.

    ```bash
    sqoop import --connect $serverDbConnect \
    --table mobiledata \
    --target-dir 'wasb:///tutorials/usesqoop/importeddata' \
    --fields-terminated-by '\t' \
    --lines-terminated-by '\n' -m 1
    ```

1. Alternatif olarak, bir Hive tablosu da belirtebilirsiniz:

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

1. İçeri aktarma işlemi tamamlandıktan sonra, yeni dizindeki verileri listelemek için Open SSH bağlantısına aşağıdaki komutu girin:

    ```bash
    hadoop fs -tail /tutorials/usesqoop/importeddata/part-m-00000
    ```

1. Tablonun Hive içinde oluşturulduğunu doğrulamak için [Beeline](./apache-hadoop-use-hive-beeline.md) ' yı kullanın.

    1. Bağlan

        ```bash
        beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
        ```

    1. Her sorguyu her seferinde bir kez yürütün ve çıktıyı gözden geçirin:

        ```hql
        show tables;
        describe mobiledata_imported2;
        SELECT COUNT(*) FROM mobiledata_imported2;
        SELECT * FROM mobiledata_imported2 LIMIT 10;
        ```

    1. Beeline ile çıkış `!exit` .

## <a name="limitations"></a>Sınırlamalar

* Toplu dışa aktarma-Linux tabanlı HDInsight Ile SQL 'e veri aktarmak için kullanılan Sqoop Bağlayıcısı toplu eklemeleri desteklemez.

* Toplu işleme-Linux tabanlı HDInsight Ile, `-batch` eklemeleri gerçekleştirirken anahtarı kullanırken, ekleme işlemlerini toplu hale getirmek yerine Sqoop birden çok ekleme yapar.

## <a name="important-considerations"></a>Önemli noktalar

* Hem HDInsight hem de SQL Server aynı Azure sanal ağında olmalıdır.

    Bir örnek için bkz. HDInsight 'ı Şirket [içi ağ belgenize bağlama](./../connect-on-premises-network.md) .

    Azure sanal ağıyla HDInsight kullanma hakkında daha fazla bilgi için bkz. HDInsight 'ı [Azure sanal ağ Ile genişletme](../hdinsight-plan-virtual-network-deployment.md) . Azure sanal ağ hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../../virtual-network/virtual-networks-overview.md) belgesi.

* SQL Server, SQL kimlik doğrulamasına izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için bkz. [kimlik doğrulama modu seçme](https://msdn.microsoft.com/ms144284.aspx) belgesi.

* SQL Server uzak bağlantıları kabul edecek şekilde yapılandırmanız gerekebilir. Daha fazla bilgi için [SQL Server veritabanı altyapısı belgesine bağlanma sorunlarını giderme](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Sqoop 'yi nasıl kullanacağınızı öğrendiniz. Daha fazla bilgi için bkz:

* [HDInsight Ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md): bir Oozie Iş akışında Sqoop eylemini kullanın.
* [HDInsight kullanarak Uçuş gecikmesi verilerini çözümleme](../interactive-query/interactive-query-tutorial-analyze-flight-data.md): Uçuş gecikmesi verilerini analiz etmek Için etkileşimli sorgu kullanın ve ardından Azure 'daki bir veritabanına veri aktarmak Için Sqoop kullanın.
* [HDInsight 'a veri yükleme](../hdinsight-upload-data.md): HDInsight/Azure Blob depolamaya veri yüklemek için diğer yöntemleri bulun.
