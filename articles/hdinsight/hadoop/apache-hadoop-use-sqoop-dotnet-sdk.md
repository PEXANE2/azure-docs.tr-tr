---
title: .NET ve HDInsight kullanarak Apache Sqoop işlerini çalıştırın - Azure
description: Apache Hadoop kümesi ile Azure SQL Veritabanı arasında Apache Sqoop alma ve dışa aktarma yı çalıştırmak için HDInsight .NET SDK'yı nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 01/14/2020
ms.openlocfilehash: f0f767273a40bc91b1d49477c896b0b157623106
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76157075"
---
# <a name="run-apache-sqoop-jobs-by-using-net-sdk-for-apache-hadoop-in-hdinsight"></a>HDInsight'ta Apache Hadoop için .NET SDK'yı kullanarak Apache Sqoop işlerini çalıştırın

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

BIR HDInsight kümesi ile Azure SQL Veritabanı veya SQL Server veritabanı arasında aktarım ve dışa aktarma için HDInsight'ta Apache Sqoop işlerini çalıştırmak için Azure HDInsight .NET SDK'yı nasıl kullanacağınızı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* [HDInsight'ta Hadoop ile Apache Sqoop'u Kullanın'dan](./hdinsight-use-sqoop.md) [test ortamını ayarlama.](./hdinsight-use-sqoop.md#create-cluster-and-sql-database)

* [Görsel Stüdyo](https://visualstudio.microsoft.com/vs/community/).

* Sqoop'a aşinalık. Daha fazla bilgi için [Sqoop Kullanım Kılavuzu'na](https://sqoop.apache.org/docs/1.4.7/SqoopUserGuide.html)bakın.

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>.NET SDK ile HDInsight kümelerinde Sqoop'u kullanın

HDInsight .NET SDK ,NET istemci kitaplıkları sağlar, böylece .NET'ten HDInsight kümeleriyle çalışmak daha kolay olur. Bu bölümde, önkoşullardan oluşturduğunuz Azure `hivesampletable` SQL Veritabanı tablosuna dışa aktarmak için bir C# konsolu uygulaması oluşturursunuz.

## <a name="set-up"></a>Ayarla

1. Visual Studio'yu başlatın ve bir C# konsol uygulaması oluşturun.

1. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'na** gidin ve aşağıdaki komutu çalıştırın:

    ```
    Install-Package Microsoft.Azure.Management.HDInsight.Job
    ```

## <a name="sqoop-export"></a>Sqoop ihracat

Hive'dan SQL Server'a.  Bu örnek, Hive `hivesampletable` tablosundaki `mobiledata` verileri SQL Veritabanı'ndaki tabloya aktarabilir.

1. Program.cs dosyasında aşağıdaki kodu kullanın. Ve `ExistingClusterPassword`. değerlerini ayarlamak için `ExistingClusterName`kodu edin

    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;
    
    namespace SubmitHDInsightJobDotNet
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;
    
            private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
            private const string ExistingClusterPassword = "<Cluster User Password>";
            private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
            private const string ExistingClusterUsername = "admin";
    
            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");
    
                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
    
                SubmitSqoopJob();
    
                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }
    
            private static void SubmitSqoopJob()
            {
                var sqlDatabaseServerName = ExistingClusterName + "dbserver";
                var sqlDatabaseLogin = "sqluser";
                var sqlDatabaseLoginPassword = ExistingClusterPassword;
                var sqlDatabaseDatabaseName = ExistingClusterName + "db";
    
                // Connection string for using Azure SQL Database; Comment if using SQL Server
                var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
    
                // Connection string for using SQL Server; Uncomment if using SQL Server
                // var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
    
                //sqoop start
                var tableName = "mobiledata";
    
                var parameters = new SqoopJobSubmissionParameters
                {
                     Command = "export --connect " + connectionString + " --table " + tableName + " --hcatalog-table hivesampletable"
                };
                //sqoop end
    
                System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

1. Programı çalıştırmak için **F5** tuşunu seçin.

## <a name="sqoop-import"></a>Sqoop ithalat

SQL Server'dan Azure Depolama'ya. Bu örnek, yukarıdaki dışa aktarma nın gerçekleştirilmesine bağlıdır.  Bu örnek, SQL `mobiledata` Veritabanı'ndaki tablodaki verileri kümenin varsayılan Depolama Hesabı'ndaki `wasb:///tutorials/usesqoop/importeddata` dizine aktarıyor.

1. `//sqoop start //sqoop end` Bloktaki yukarıdaki kodu aşağıdaki kodla değiştirin:

    ```csharp
    var tableName = "mobiledata";
    var exportDir = "/tutorials/usesqoop/importeddata";
    
    var parameters = new SqoopJobSubmissionParameters
    {
        Command = "import --connect " + connectionString + " --table " + tableName + " --target-dir " +  exportDir + " --fields-terminated-by \\t --lines-terminated-by \\n -m 1"
    };
    ```

1. Programı çalıştırmak için **F5** tuşunu seçin.

## <a name="limitations"></a>Sınırlamalar

Linux tabanlı HDInsight aşağıdaki sınırlamaları sunar:

* Toplu dışa aktarma: Verileri Microsoft SQL Server veya Azure SQL Veritabanı'na aktarmak için kullanılan Sqoop bağlayıcısı şu anda toplu ekleri desteklemiyor.

* Toplu İşlem: Sqoop `-batch` anahtarı kullanarak, kesici uç işlemlerini toplu olarak kullanmak yerine birden çok kesici uç gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Sqoop'u kullanmayı öğrendin. Daha fazla bilgi için bkz:

* [HDInsight ile Apache Oozie kullanın:](../hdinsight-use-oozie-linux-mac.md)Bir Oozie iş akışında Sqoop eylem kullanın.
* [HDInsight'a veri yükleme](../hdinsight-upload-data.md): HDInsight veya Azure Blob depolamasına veri yüklemek için başka yöntemler bulun.
