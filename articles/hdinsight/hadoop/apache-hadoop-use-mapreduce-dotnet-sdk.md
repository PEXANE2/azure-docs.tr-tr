---
title: MapInsight .NET SDK kullanarak iş gönderme - Azure
description: HDInsight .NET SDK'yı kullanarak MapReduce işlerini Azure HDInsight Apache Hadoop'a nasıl göndereceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/15/2020
ms.openlocfilehash: e50510f2420d69be37af584a2648a794e1561ee3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76157064"
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>HDInsight .NET SDK'sını kullanarak MapReduce işlerini çalıştırma

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

HDInsight .NET SDK'yı kullanarak MapReduce işlerini nasıl göndereceğinizi öğrenin. HDInsight kümeleri, bazı MapReduce örnekleriyle birlikte bir kavanoz dosyasıyla birlikte gelir. Kavanoz `/example/jars/hadoop-mapreduce-examples.jar`dosyası.  Örneklerden biri **sözcük sayısıdır.** Wordcount işi göndermek için c# konsolu uygulaması geliştirirsiniz.  İş dosyayı `/example/data/gutenberg/davinci.txt` okur ve sonuçları `/example/data/davinciwordcount`.  Uygulamayı yeniden çalıştırmak istiyorsanız, çıktı klasörünü temizlemeniz gerekir.

> [!NOTE]  
> Bu makaledeki adımlar bir Windows istemcisinden gerçekleştirilmelidir. Hive ile çalışmak için bir Linux, OS X veya Unix istemcisi kullanma hakkında daha fazla bilgi için makalenin üst kısmında gösterilen sekme seçiciyi kullanın.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Hadoop kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun.](../hdinsight-hadoop-create-linux-clusters-portal.md)

* [Görsel Stüdyo](https://visualstudio.microsoft.com/vs/community/).

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>MapInsight .NET SDK kullanarak iş gönderme

HDInsight .NET SDK, .NET'ten HDInsight kümeleriyle çalışmayı kolaylaştıran .NET istemci kitaplıkları sağlar.

1. Visual Studio'yu başlatın ve bir C# konsol uygulaması oluşturun.

1. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'na** gidin ve aşağıdaki komutu girin:

    ```   
    Install-Package Microsoft.Azure.Management.HDInsight.Job
    ```

1. Aşağıdaki kodu **Program.cs'** ye kopyalayın. Daha sonra kodu aşağıdaki `existingClusterName`ler için `existingClusterPassword`, `defaultStorageAccountName` `defaultStorageAccountKey`, `defaultStorageContainerName`, ve .

    ```csharp
    using System.Collections.Generic;
    using System.IO;
    using System.Text;
    using System.Threading;
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    
    namespace SubmitHDInsightJobDotNet
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;
    
            private const string existingClusterName = "<Your HDInsight Cluster Name>";
            private const string existingClusterPassword = "<Cluster User Password>";
            private const string defaultStorageAccountName = "<Default Storage Account Name>"; 
            private const string defaultStorageAccountKey = "<Default Storage Account Key>";
            private const string defaultStorageContainerName = "<Default Blob Container Name>";
    
            private const string existingClusterUsername = "admin";
            private const string existingClusterUri = existingClusterName + ".azurehdinsight.net";
            private const string sourceFile = "/example/data/gutenberg/davinci.txt";
            private const string outputFolder = "/example/data/davinciwordcount";
    
            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");
    
                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
    
                SubmitMRJob();
    
                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }
    
            private static void SubmitMRJob()
            {
                List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };
    
                var paras = new MapReduceJobSubmissionParameters
                {
                    JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                    JarClass = "wordcount",
                    Arguments = args
                };
    
                System.Console.WriteLine("Submitting the MR job to the cluster...");
                var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
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
                System.Console.WriteLine("Job output is: ");
                var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                    defaultStorageContainerName);
    
                if (jobDetail.ExitValue == 0)
                {
                    // Create the storage account object
                    CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" +
                        defaultStorageAccountName +
                        ";AccountKey=" + defaultStorageAccountKey);
    
                    // Create the blob client.
                    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    
                    // Retrieve reference to a previously created container.
                    CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);
    
                    CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");
    
                    using (var stream = blockBlob.OpenRead())
                    {
                        using (StreamReader reader = new StreamReader(stream))
                        {
                            while (!reader.EndOfStream)
                            {
                                System.Console.WriteLine(reader.ReadLine());
                            }
                        }
                    }
                }
                else
                {
                    // fetch stderr output in case of failure
                    var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess);
    
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
    
                }
            }
        }
    }

    ```

1. Uygulamayı çalıştırmak için **F5**'e basın.

İşi yeniden çalıştırmak için, iş çıktısı klasörü adını değiştirmeniz gerekir, örnekte . `/example/data/davinciwordcount`

İş başarıyla tamamlandığında, uygulama çıktı dosyasının `part-r-00000`içeriğini yazdırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir HDInsight kümesi oluşturmak için çeşitli yollar öğrendim. Daha fazla bilgi için aşağıdaki makalelere bakın:

* Bir Kovan işi göndermek için, [HDInsight .NET SDK kullanarak Apache Hive sorgularını çalıştır'a](apache-hadoop-use-hive-dotnet-sdk.md)bakın.
* HDInsight kümeleri oluşturmak için, [HDInsight'ta Linux tabanlı Apache Hadoop kümeleri oluşturun'a](../hdinsight-hadoop-provision-linux-clusters.md)bakın.
* HDInsight kümelerini yönetmek için [HDInsight'taki Apache Hadoop kümelerini yönet'e](../hdinsight-administer-use-portal-linux.md)bakın.
* HDInsight .NET SDK'yı öğrenmek için [HDInsight .NET SDK referansına](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight)bakın.
* Azure'da etkileşimli olmayan kimlik doğrulama için [etkileşimli olmayan kimlik doğrulaması .NET HDInsight uygulamaları oluştur'a](../hdinsight-create-non-interactive-authentication-dotnet-applications.md)bakın.