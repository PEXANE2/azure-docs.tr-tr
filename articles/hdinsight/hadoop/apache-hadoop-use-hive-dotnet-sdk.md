---
title: HDInsight .NET SDK kullanarak Apache Hive sorgularını çalıştırın - Azure
description: HDInsight .NET SDK'yı kullanarak Apache Hadoop işlerini Azure HDInsight Apache Hadoop'a nasıl göndereceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/24/2019
ms.openlocfilehash: a9d71c8aebb9cc4a0adbd461aead6e2612bd13bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552500"
---
# <a name="run-apache-hive-queries-using-hdinsight-net-sdk"></a>HDInsight .NET SDK kullanarak Apache Hive sorgularını çalıştırın

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

HDInsight .NET SDK'yı kullanarak Apache Hive sorgularını nasıl göndereceğinizi öğrenin. Hive tablolarını listelemek için hive sorgusu göndermek ve sonuçları görüntülemek için bir C# programı yazarsınız.

> [!NOTE]  
> Bu makaledeki adımlar bir Windows istemcisinden gerçekleştirilmelidir. Hive ile çalışmak için bir Linux, OS X veya Unix istemcisi kullanma hakkında daha fazla bilgi için makalenin üst kısmında gösterilen sekme seçiciyi kullanın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleye başlamadan önce aşağıdaki öğelere sahip olmalısınız:

* HDInsight'ta bir Apache Hadoop kümesi. Bkz. [HDInsight'ta Linux tabanlı Hadoop'u kullanmaya başlayın.](apache-hadoop-linux-tutorial-get-started.md)

    > [!IMPORTANT]  
    > HDInsight .NET SDK, 15 Eylül 2017 itibarıyla yalnızca Azure Depolama hesaplarından dönen Hive sorgu sonuçlarını destekler. Bu örneği, Azure Veri Gölü Depolamasını birincil depolama alanı olarak kullanan bir HDInsight kümesiyle kullanırsanız, .NET SDK'yı kullanarak arama sonuçlarını alamazsınız.

* [Visual Studio](https://visualstudio.microsoft.com/vs/community/) 2013 ve sonrası. En azından iş yükü **.NET masaüstü geliştirme** yüklü olmalıdır.

## <a name="run-a-hive-query"></a>Kovan Sorgusu Çalıştır

HDInsight .NET SDK ,NET istemci kitaplıkları sağlar, bu da .NET'ten HDInsight kümeleriyle çalışmayı kolaylaştırır.

1. Visual Studio'da C# konsolu uygulaması oluşturun.

1. Nuget Paket Yöneticisi Konsolundan aşağıdaki komutu çalıştırın:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

1. Değişkenlerin değerlerini başlatmak için aşağıdaki kodu `ExistingClusterName, ExistingClusterUsername, ExistingClusterPassword,DefaultStorageAccountName,DefaultStorageAccountKey,DefaultStorageContainerName`edin: . Ardından, Visual Studio'daki **Program.cs** tüm içeriği olarak gözden geçirilmiş kodu kullanın.

    ```csharp
    using System.Collections.Generic;
    using System.IO;
    using System.Text;
    using System.Threading;
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitHDInsightJobDotNet
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;

            private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
            private const string ExistingClusterUsername = "<Cluster Username>";
            private const string ExistingClusterPassword = "<Cluster User Password>";

            // Only Azure Storage accounts are supported by the SDK
            private const string DefaultStorageAccountName = "<Default Storage Account Name>";
            private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
            private const string DefaultStorageContainerName = "<Default Blob Container Name>";

            private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";

            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitHiveJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitHiveJob()
            {
                Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                List<string> args = new List<string> { { "argA" }, { "argB" } };
                var parameters = new HiveJobSubmissionParameters
                {
                    Query = "SHOW TABLES",
                    Defines = defines,
                    Arguments = args
                };

                System.Console.WriteLine("Submitting the Hive job to the cluster...");
                var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                System.Console.WriteLine("JobId is " + jobId);

                System.Console.WriteLine("Waiting for the job completion ...");

                // Wait for job completion
                var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                while (!jobDetail.Status.JobComplete)
                {
                    Thread.Sleep(1000);
                    jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                }

                // Get job output
                var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                    DefaultStorageContainerName);
                var output = (jobDetail.ExitValue == 0)
                    ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                    : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                System.Console.WriteLine("Job output is: ");

                using (var reader = new StreamReader(output, Encoding.UTF8))
                {
                    string value = reader.ReadToEnd();
                    System.Console.WriteLine(value);
                }
            }
        }
    }
    ```

1. Uygulamayı çalıştırmak için **F5**'e basın.

Uygulamanın çıktısı aşağıdakilere benzer olmalıdır:

![HDInsight Hadoop Kovan iş çıktısı](./media/apache-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, HDInsight .NET SDK kullanarak Apache Hive sorguları göndermeyi öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure HDInsight ile başlayın](apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight'ta Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight .NET SDK referansı](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)
* [HDInsight ile Apache Sqoop kullanın](apache-hadoop-use-sqoop-mac-linux.md)
* [Etkileşimli olmayan kimlik doğrulaması .NET HDInsight uygulamaları oluşturma](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)
