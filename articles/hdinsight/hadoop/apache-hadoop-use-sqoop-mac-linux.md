---
title: Apache Hadoop-Azure HDInsight ile Apache Sqoop
description: HDInsight ve Azure SQL veritabanı arasında Apache Hadoop içeri aktarmak ve dışarı aktarmak için Apache Sqoop 'yi nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/28/2019
ms.openlocfilehash: 21bc903349876a76576fb742840e9899f9d94bcd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74769396"
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-apache-hadoop-on-hdinsight-and-sql-database"></a>HDInsight üzerinde Apache Hadoop ile SQL Veritabanı arasında veri içeri ve dışarı aktarma işlemleri için Apache Sqoop'u kullanma

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Azure HDInsight ve Azure SQL veritabanı veya Microsoft SQL Server veritabanında bir Apache Hadoop kümesi arasında içeri ve dışarı aktarmak için Apache Sqoop 'yi nasıl kullanacağınızı öğrenin. Bu belgedeki adımlar, doğrudan Hadoop kümesinin `sqoop` baş düğümüne öğesinden komutunu kullanır. Baş düğüme bağlanmak ve bu belgedeki komutları çalıştırmak için SSH 'yi kullanırsınız. Bu makalede, [HDInsight 'Ta Hadoop Ile Apache Sqoop kullanma](./hdinsight-use-sqoop.md)işlemi devam ediyor.

## <a name="prerequisites"></a>Ön koşullar

* [Test ortamını ayarlama](./hdinsight-use-sqoop.md#create-cluster-and-sql-database) , [HDInsight 'Ta Hadoop Ile Apache Sqoop kullanın](./hdinsight-use-sqoop.md).

* Bir SSH istemcisi. Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Sqoop ile benzerlik. Daha fazla bilgi için bkz. [Sqoop Kullanıcı Kılavuzu](https://sqoop.apache.org/docs/1.4.7/SqoopUserGuide.html).

## <a name="set-up"></a>Ayarla

1. Kümenize bağlanmak için [SSH komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Kullanım kolaylığı için değişkenleri ayarlayın. `MYSQLSERVER`, Ve `MYDATABASE` değerlerini ilgili değerlerle değiştirin `PASSWORD`ve ardından aşağıdaki komutları girin:

    ```bash
    export password='PASSWORD'
    export sqlserver="MYSQLSERVER"
    export database="MYDATABASE"


    export serverConnect="jdbc:sqlserver://$sqlserver.database.windows.net:1433;user=sqluser;password=$password"
    export serverDbConnect="jdbc:sqlserver://$sqlserver.database.windows.net:1433;user=sqluser;password=$password;database=$database"
    ```

## <a name="sqoop-export"></a>Sqoop dışarı aktarma

Hive 'dan SQL Server.

1. Sqoop 'nin SQL veritabanınızı görebildiğini doğrulamak için, Open SSH bağlantınızda aşağıdaki komutu girin. Bu komut, veritabanlarının bir listesini döndürür.

    ```bash
    sqoop list-databases --connect $serverConnect
    ```

1. Belirtilen veritabanı için Tablo listesini görmek için aşağıdaki komutu girin:

    ```bash
    sqoop list-tables --connect $serverDbConnect
    ```

1. Hive `hivesampletable` tablosundan SQL veritabanındaki `mobiledata` tabloya veri aktarmak için, Open SSH bağlantınızda aşağıdaki komutu girin:

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

SQL Server 'den Azure depolama 'ya.

1. SQL veritabanındaki `mobiledata` tablodaki verileri HDInsight üzerindeki `wasbs:///tutorials/usesqoop/importeddata` DIZINE aktarmak için Open SSH bağlantınızın aşağıdaki komutunu girin. Verilerdeki alanlar bir sekme karakteriyle ayrılır ve satırlar yeni satır karakteri ile sonlandırılır.

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

    1. Beeline ile `!exit`çıkış.

## <a name="limitations"></a>Sınırlamalar

* Toplu dışa aktarma-Linux tabanlı HDInsight Ile, Microsoft SQL Server veya Azure SQL veritabanı 'na veri aktarmak için kullanılan Sqoop Bağlayıcısı toplu eklemeleri desteklemez.

* Toplu işleme-Linux tabanlı HDInsight Ile, eklemeleri gerçekleştirirken `-batch` anahtarı kullanırken, ekleme işlemlerini toplu hale getirmek yerine Sqoop birden çok ekleme yapar.

## <a name="important-considerations"></a>Önemli noktalar

* Hem HDInsight hem de SQL Server aynı Azure sanal ağında olmalıdır.

    Bir örnek için bkz. HDInsight 'ı Şirket [içi ağ belgenize bağlama](./../connect-on-premises-network.md) .

    Azure sanal ağıyla HDInsight kullanma hakkında daha fazla bilgi için bkz. HDInsight 'ı [Azure sanal ağ Ile genişletme](../hdinsight-plan-virtual-network-deployment.md) . Azure sanal ağ hakkında daha fazla bilgi için bkz. [sanal ağa genel bakış](../../virtual-network/virtual-networks-overview.md) belgesi.

* SQL Server, SQL kimlik doğrulamasına izin verecek şekilde yapılandırılmalıdır. Daha fazla bilgi için bkz. [kimlik doğrulama modu seçme](https://msdn.microsoft.com/ms144284.aspx) belgesi.

* SQL Server uzak bağlantıları kabul edecek şekilde yapılandırmanız gerekebilir. Daha fazla bilgi için [SQL Server veritabanı altyapısı belgesine bağlanma sorunlarını giderme](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Sqoop 'yi nasıl kullanacağınızı öğrendiniz. Daha fazla bilgi için bkz:

* [HDInsight Ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md): bir Oozie Iş akışında Sqoop eylemini kullanın.
* [HDInsight kullanarak Uçuş gecikmesi verilerini çözümleme](../interactive-query/interactive-query-tutorial-analyze-flight-data.md): Uçuş gecikmesi verilerini çözümlemek Için etkileşimli sorgu kullanın ve ardından verileri BIR Azure SQL veritabanına aktarmak Için Sqoop kullanın.
* [HDInsight 'a veri yükleme](../hdinsight-upload-data.md): HDInsight/Azure Blob depolamaya veri yüklemek için diğer yöntemleri bulun.
