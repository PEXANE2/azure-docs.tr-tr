---
title: HDInsight .NET SDK-Azure kullanarak MapReduce işlerini gönderme
description: HDInsight .NET SDK kullanarak MapReduce işlerini Azure HDInsight Apache Hadoop gönderme hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/15/2020
ms.openlocfilehash: e50510f2420d69be37af584a2648a794e1561ee3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76157064"
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>HDInsight .NET SDK'sını kullanarak MapReduce işlerini çalıştırma

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

HDInsight .NET SDK kullanarak MapReduce işlerini göndermeyi öğrenin. HDInsight kümeleri, bazı MapReduce örneklerine sahip bir jar dosyası ile gelir. Jar dosyası `/example/jars/hadoop-mapreduce-examples.jar`.  Örneklerden biri **WORDCOUNT**. Bir WORDCOUNT işi göndermek için bir C# konsol uygulaması geliştirirsiniz.  İş `/example/data/gutenberg/davinci.txt` dosyayı okur ve sonuçları ' a çıkarır `/example/data/davinciwordcount`.  Uygulamayı yeniden çalıştırmak istiyorsanız, çıkış klasörünü temizlemeniz gerekir.

> [!NOTE]  
> Bu makaledeki adımların bir Windows istemcisinden gerçekleştirilmesi gerekir. Bir Linux, OS X veya UNIX istemcisini Hive ile çalışmak üzere kullanma hakkında bilgi için, makalenin üst kısmında gösterilen sekme seçiciyi kullanın.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Hadoop kümesi. Bkz. [Azure Portal kullanarak Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md).

* [Visual Studio](https://visualstudio.microsoft.com/vs/community/).

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>HDInsight .NET SDK kullanarak MapReduce işlerini gönderme

HDInsight .NET SDK 'Sı .NET istemci kitaplıklarını sağlar ve bu sayede .NET 'teki HDInsight kümeleri ile çalışmayı kolaylaştırmaktadır.

1. Visual Studio 'Yu başlatın ve bir C# konsol uygulaması oluşturun.

1. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' na gidin ve şu komutu girin:

    ```   
    Install-Package Microsoft.Azure.Management.HDInsight.Job
    ```

1. Aşağıdaki kodu **program.cs**'e kopyalayın. Ardından `existingClusterName`,:, `existingClusterPassword` `defaultStorageAccountName` `defaultStorageAccountKey`,, ve `defaultStorageContainerName`değerlerini ayarlayarak kodu düzenleyin.

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

İşi yeniden çalıştırmak için, örnek olan iş çıkış klasörü adını değiştirmeniz gerekir `/example/data/davinciwordcount`.

İş başarıyla tamamlandığında, uygulama çıkış dosyasının `part-r-00000`içeriğini yazdırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir HDInsight kümesi oluşturmanın çeşitli yollarını öğrendiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* Hive işi göndermek için bkz. [HDInsight .NET SDK kullanarak Apache Hive sorguları çalıştırma](apache-hadoop-use-hive-dotnet-sdk.md).
* HDInsight kümeleri oluşturmak için bkz. [HDInsight 'Ta Linux tabanlı Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).
* HDInsight kümelerini yönetmek için bkz. [HDInsight 'ta Apache Hadoop kümelerini yönetme](../hdinsight-administer-use-portal-linux.md).
* HDInsight .NET SDK 'sını öğrenmek için bkz. [HDInsight .NET SDK başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight).
* Azure 'da etkileşimli olmayan kimlik doğrulaması için bkz. [etkileşimli olmayan kimlik doğrulaması .net HDInsight uygulamaları oluşturma](../hdinsight-create-non-interactive-authentication-dotnet-applications.md).