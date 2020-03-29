---
title: Verileri Veri Gölü Depolama Gen1 ve Azure SQL arasında kopyalama - Sqoop | Microsoft Dokümanlar
description: Azure SQL Veritabanı ile Azure Veri Gölü Depolama Gen1 arasındaki verileri kopyalamak için Sqoop'u kullanın
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: twooley
ms.openlocfilehash: cf3893706afcb4c4cc5b90dd3d2431ecedc71d0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73839072"
---
# <a name="copy-data-between-data-lake-storage-gen1-and-azure-sql-database-using-sqoop"></a>Veri Gölü Depolama Gen1 ve Azure SQL Veritabanı arasında Sqoop kullanarak veri kopyalama

Azure SQL Veritabanı ve Azure Veri Gölü Depolama Gen1 arasında veri almak ve aktarmak için Apache Sqoop'u nasıl kullanacağınızı öğrenin.

## <a name="what-is-sqoop"></a>Sqoop nedir?

Büyük veri uygulamaları, günlükler ve dosyalar gibi yapılandırılmamış ve yarı yapılandırılmış verileri işlemek için doğal bir seçimdir. Ancak, ilişkisel veritabanlarında depolanan yapılandırılmış verileri işleme gereksiniminde de olabilir.

[Apache Sqoop,](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) İlişkisel veritabanları ve Veri Gölü Depolama Gen1 gibi büyük bir veri deposu arasında veri aktarmak için tasarlanmış bir araçtır. Verileri Azure SQL Veritabanı gibi ilişkisel veritabanı yönetim sisteminden (RDBMS) Veri Gölü Depolama Gen1'e almak için kullanabilirsiniz. Daha sonra büyük veri iş yüklerini kullanarak verileri dönüştürebilir ve çözümleyebilir ve verileri bir RDBMS'ye geri aktarabilirsiniz. Bu makalede, bir Azure SQL veritabanını ilişkisel veritabanınız olarak içe aktarma/dışa aktarma için kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri almalısınız:

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Veri Gölü Depolama Gen1 hesabı.** Hesabın nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)
* Veri Gölü Depolama Gen1 hesabına erişim içeren **Azure HDInsight kümesi.** Bkz. [Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturun.](data-lake-store-hdinsight-hadoop-use-portal.md) Bu makalede, Data Lake Storage Gen1 erişimi ne sahip bir HDInsight Linux kümeniz olduğunu varsayar.
* **Azure SQL Veritabanı**. Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](../sql-database/sql-database-get-started.md)

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Azure SQL veritabanında örnek tablolar oluşturma

1. Başlamak için Azure SQL veritabanında iki örnek tablo oluşturun. Veritabanına bağlanmak ve ardından aşağıdaki sorguları çalıştırmak için [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) veya Visual Studio'yu kullanın.

    **Tablo Oluştur1**

       CREATE TABLE [dbo].[Table1](
       [ID] [int] NOT NULL,
       [FName] [nvarchar](50) NOT NULL,
       [LName] [nvarchar](50) NOT NULL,
        CONSTRAINT [PK_Table_1] PRIMARY KEY CLUSTERED
           (
                  [ID] ASC
           )
       ) ON [PRIMARY]
       GO

    **Tablo Oluşturma2**

       CREATE TABLE [dbo].[Table2](
       [ID] [int] NOT NULL,
       [FName] [nvarchar](50) NOT NULL,
       [LName] [nvarchar](50) NOT NULL,
        CONSTRAINT [PK_Table_2] PRIMARY KEY CLUSTERED
           (
                  [ID] ASC
           )
       ) ON [PRIMARY]
       GO

1. **Tablo1'e**bazı örnek veriler eklemek için aşağıdaki komutu çalıştırın. **Tablo2'yi** boş bırakın. Daha sonra, **Tablo1'den** Veri Gölü Depolama Gen1'e veri aktaracaksınız. Daha sonra, Veri Gölü Depolama Gen1'den **Tablo2'ye**veri aktarAcaksınız.

       INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1 erişimi olan bir HDInsight kümesinden Sqoop kullanın

Bir HDInsight kümesi zaten Sqoop paketleri mevcuttur. HDInsight kümesini Veri Gölü Depolama Gen1'i ek depolama alanı olarak kullanacak şekilde yapılandırıldıysanız, Azure SQL Veritabanı gibi ilişkisel bir veritabanı ile Veri Gölü Depolama Gen1 hesabı arasında veri almak/aktarmak için Sqoop'u (yapılandırma değişikliği olmadan) kullanabilirsiniz.

1. Bu makalede, kümeye bağlanmak için SSH kullanmanız gerektiği için bir Linux kümesi oluşturduğunuzu varsayıyoruz. Bkz. [Linux tabanlı HDInsight kümesine bağlanın.](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

1. Veri Gölü Depolama Gen1 hesabına kümeden erişip erişemeyeceğiniz doğrulayın. SSH komutundan aşağıdaki komutu çalıştırın:

       hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net/

   Bu komut, Veri Gölü Depolama Gen1 hesabındaki dosyaların/klasörlerin bir listesini sağlar.

### <a name="import-data-from-azure-sql-database-into-data-lake-storage-gen1"></a>Azure SQL Veritabanı'ndan Veri Gölü Depolama Gen1'e veri alma

1. Sqoop paketlerinin bulunduğu dizine gidin. Genellikle, bu konum `/usr/hdp/<version>/sqoop/bin`.

1. **Tablo1'deki** verileri Veri Gölü Depolama Gen1 hesabına aktarın. Aşağıdaki sözdizimini kullanın:

       sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

   **sql-database-server-name** placeholder, Azure SQL veritabanının çalıştığı sunucunun adını temsil eder. **sql-database-name** placeholder gerçek veritabanı adını temsil eder.

   Örneğin,

       sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=twooley@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. Verilerin Veri Gölü Depolama Gen1 hesabına aktarıldığını doğrulayın. Şu komutu çalıştırın:

       hdfs dfs -ls adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/

   Aşağıdaki çıkışı görmelisiniz.

       -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
       -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
       -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
       -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
       -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

   Her **part-m-*** dosyası kaynak tablodaki bir satıra karşılık gelir, **Tablo1.** Doğrulamak için part-m-* dosyalarının içeriğini görüntüleyebilirsiniz.

### <a name="export-data-from-data-lake-storage-gen1-into-azure-sql-database"></a>Veri Gölü Depolama Gen1'den verileri Azure SQL Veritabanına aktarma

1. Verileri Veri Gölü Depolama Gen1 hesabındaki verileri Azure SQL Veritabanı'ndaki boş tablo **Tablo2'ye**dışa aktarın. Aşağıdaki sözdizimini kullanın.

       sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-storage-gen1-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

   Örneğin,

       sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=twooley@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlsg1store.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. Verilerin SQL Veritabanı tablosuna yüklendiğini doğrulayın. Azure SQL Veritabanı'na bağlanmak ve ardından aşağıdaki sorguyu çalıştırmak için [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) veya Visual Studio'yu kullanın.

       SELECT * FROM TABLE2

   Bu komut aşağıdaki çıktıya sahip olmalıdır.

        ID  FName    LName
       -------------------
       1    Neal     Kell
       2    Lila     Fulton
       3    Erna     Myers
       4    Annette  Simpson

## <a name="performance-considerations-while-using-sqoop"></a>Sqoop kullanırken performans hususları

Veri Gölü Depolama Gen1 verileri kopyalamak için Sqoop iş atonlama performans hakkında bilgi için, [Sqoop performans blog yazısı](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Verileri Azure Depolama Blobs'undan Veri Gölü Depolama Gen1'e kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
