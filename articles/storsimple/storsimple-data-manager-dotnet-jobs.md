---
title: Microsoft Azure StorSimple Veri Yöneticisi işleri için .NET SDK 'Yı kullanma
description: .NET SDK 'yı kullanarak StorSimple Veri Yöneticisi işleri başlatma hakkında bilgi edinin
author: alkohli
ms.service: storsimple
ms.topic: how-to
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: 316eddc98de78974bb2583f91ced635b148686c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514904"
---
# <a name="use-the-net-sdk-to-initiate-data-transformation"></a>.NET SDK kullanarak veri dönüştürmeyi başlatma

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple cihaz verilerini dönüştürmek için StorSimple Veri Yöneticisi hizmeti içinde veri dönüştürme özelliğini nasıl kullanabileceğiniz açıklanır. Dönüştürülen veriler daha sonra buluttaki diğer Azure hizmetleri tarafından kullanılır.

Bir veri dönüştürme işini iki şekilde başlatabilirsiniz:

- .NET SDK’yı kullanma
- Azure Otomasyonu runbook 'u kullanma
 
  Bu makalede, bir veri dönüştürme işi başlatmak ve sonra tamamlamak üzere izlemek için örnek bir .NET konsol uygulaması oluşturma işlemi açıklanır. Otomasyon aracılığıyla veri dönüştürmeyi başlatma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu runbook 'U kullanarak veri dönüştürme işlerini tetiklemeniz](storsimple-data-manager-job-using-automation.md)bölümüne gidin.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, şunları kullandığınızdan emin olun:
*   Çalıştıran bir bilgisayar:

    - Visual Studio 2012, 2013, 2015 veya 2017.

    - Azure PowerShell. [Azure PowerShell 'ı indirin](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*   Kaynak grubu içinde StorSimple Veri Yöneticisi doğru şekilde yapılandırılmış bir iş tanımı.
*   Tüm gerekli dll 'ler. Bu dll 'leri [GitHub deposundan](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls)indirin.
*   [`Get-ConfigurationParams.ps1`](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1)GitHub deposundan betiği.

## <a name="step-by-step-procedure"></a>Adım adım yordam

.NET kullanarak bir veri dönüştürme işi başlatmak için aşağıdaki adımları gerçekleştirin.

1. Yapılandırma parametrelerini almak için aşağıdaki adımları uygulayın:
    1. `Get-ConfigurationParams.ps1`Konumundaki GitHub depo betiği ' nden indirin `C:\DataTransformation` .
    1. `Get-ConfigurationParams.ps1`Betiği GitHub deposundan çalıştırın. Aşağıdaki komutu yazın:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        ActiveDirectoryKey ve AppName için herhangi bir değer geçirebilirsiniz.

2. Bu betik aşağıdaki değerleri verir:
    * İstemci Kimliği
    * Kiracı Kimliği
    * Active Directory anahtar (Yukarıda girilen bir ile aynı)
    * Abonelik Kimliği

        ![Yapılandırma parametreleri betik çıkışı](media/storsimple-data-manager-dotnet-jobs/get-config-parameters.png)

3. Visual Studio 2012, 2013 veya 2015 kullanarak bir C# .NET konsol uygulaması oluşturun.

    1. **Visual Studio 2012/2013/2015**' i başlatın.
    1. **Dosya > Yeni > Proje**'yi seçin.

        ![Proje oluşturun 1](media/storsimple-data-manager-dotnet-jobs/create-new-project-7.png)        
    2. **Visual C# > konsol uygulaması > yüklü > şablonları**' nı seçin.
    3. **Ad**Için **Datadönüştürme tionapp** yazın.
    4. **Konum**için **c:\datatransformation** öğesini seçin.
    6. Projeyi oluşturmak için **Tamam**'a tıklayın.

        ![Proje oluşturma 2](media/storsimple-data-manager-dotnet-jobs/create-new-project-1.png)

4. Şimdi, [DLL klasöründe](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) bulunan tüm dll 'leri oluşturduğunuz projede **başvuru** olarak ekleyin. Dll dosyalarını eklemek için aşağıdakileri yapın:

   1. Visual Studio 'da **> Çözüm Gezgini görüntüle**' ye gidin.
   2. Veri dönüştürme uygulaması projesinin solundaki oka tıklayın. **Başvurular** ' a tıklayın ve ardından **başvuru eklemek**için sağ tıklayın.
    
       ![Dll 'leri Ekle 1](media/storsimple-data-manager-dotnet-jobs/create-new-project-4.png)

   3. Paketler klasörünün konumuna gidin, tüm dll 'leri seçin ve **Ekle**' ye tıklayın ve ardından **Tamam**' a tıklayın.

       ![Dll 'leri ekleme 2](media/storsimple-data-manager-dotnet-jobs/create-new-project-6.png)

5. Aşağıdaki **using** bildirimlerini projedeki kaynak dosyasına (Program.cs) ekleyin.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```
    
6. Aşağıdaki kod, veri dönüştürme işi örneğini başlatır. Bunu **Main yöntemine**ekleyin. Yapılandırma parametrelerinin değerlerini daha önce elde edilen şekilde değiştirin. **Kaynak grubu adı** ve **resourceName**değerlerini takın. **Resourcegroupname** , iş tanımının yapılandırıldığı StorSimple veri Yöneticisi ile ilişkilidir. **ResourceName** , StorSimple veri Yöneticisi hizmetinizin adıdır.

    ```
    // Setup the configuration parameters.
    var configParams = new ConfigurationParams
    {
        ClientId = "client-id",
        TenantId = "tenant-id",
        ActiveDirectoryKey = "active-directory-key",
        SubscriptionId = "subscription-id",
        ResourceGroupName = "resource-group-name",
        ResourceName = "resource-name"
    };

    // Initialize the Data Transformation Job instance.
    DataTransformationJob dataTransformationJob = new DataTransformationJob(configParams);
    ```
   
7. İş tanımının çalıştırılması gereken parametreleri belirtin

    ```
    string jobDefinitionName = "job-definition-name";

    DataTransformationInput dataTransformationInput = dataTransformationJob.GetJobDefinitionParameters(jobDefinitionName);
    ```

    VEYA

    Çalışma zamanında iş tanımı parametrelerini değiştirmek istiyorsanız aşağıdaki kodu ekleyin:

    ```
    string jobDefinitionName = "job-definition-name";
    // Must start with a '\'
    var rootDirectories = new List<string> {@"\root"};

    // Name of the volume on the StorSimple device.
    var volumeNames = new List<string> {"volume-name"};

    var dataTransformationInput = new DataTransformationInput
    {
        // If you require the latest existing backup to be picked else use TakeNow to trigger a new backup.
        BackupChoice = BackupChoice.UseExistingLatest.ToString(),
        // Name of the StorSimple device.
        DeviceName = "device-name",
        // Name of the container in Azure storage where the files will be placed after execution.
        ContainerName = "container-name",
        // File name filter (search pattern) to be applied on files under the root directory. * - Match all files.
        FileNameFilter = "*",
        // List of root directories.
        RootDirectories = rootDirectories,
        // Name of the volume on StorSimple device on which the relevant data is present. 
        VolumeNames = volumeNames
    };
    ```

8. Başlatma işleminden sonra, iş tanımında bir veri dönüştürme işini tetiklemek için aşağıdaki kodu ekleyin. Uygun **Iş tanımı adını**takın.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);
    Console.WriteLine("jobid: ", jobId);
    Console.ReadLine();

    ```
    Kod yapıştırıldıktan sonra çözümü oluşturun. Veri dönüştürme işi örneğini başlatmak için kod parçacığının bir ekran görüntüsü aşağıda verilmiştir.

   ![Veri dönüştürme işini başlatmak için kod parçacığı](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet-1.png)

9. Bu iş, StorSimple birimi içindeki kök dizin ve dosya filtreleriyle eşleşen verileri dönüştürür ve belirtilen kapsayıcıya/dosya paylaşımıyla geçirir. Bir dosya dönüştürüldüğünde, iş tanımıyla aynı ada sahip bir depolama kuyruğuna (kapsayıcı/dosya paylaşımıyla aynı depolama hesabında) bir ileti eklenir. Bu ileti, dosyanın daha fazla işlenmesini başlatmak için bir tetikleyici olarak kullanılabilir.

10. İş tetiklendiğinde, işin tamamlanmasını izlemek için aşağıdaki kodu kullanabilirsiniz. Bu kodun iş çalıştırması için eklenmesi zorunlu değildir.

    ```
    Job jobDetails = null;

    // Poll the job.
    do
    {
        jobDetails = dataTransformationJob.GetJob(jobDefinitionName, jobId);

        // Wait before polling for the status again.
        Thread.Sleep(TimeSpan.FromSeconds(retryAfter));

    } while (jobDetails.Status == JobStatus.InProgress);

    // Completion status of the job.
    Console.WriteLine("JobStatus: {0}", jobDetails.Status);
    
    // To hold the console before exiting.
    Console.Read();

    ```
    İşte .NET kullanarak işi tetiklemek için kullanılan tüm kod örneğinin ekran görüntüsü.

    ![.NET işini tetiklemek için kodun tam parçacığı](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet.png)

## <a name="next-steps"></a>Sonraki adımlar

[Verilerinizi dönüştürmek için StorSimple veri Yöneticisi Kullanıcı arabirimini kullanın](storsimple-data-manager-ui.md).
