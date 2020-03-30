---
title: Microsoft Azure StorBasit Veri Yöneticisi işleri için .NET SDK'yı kullanma
description: StorSimple Data Manager işlerini başlatmak için .NET SDK'yı nasıl kullanacağınızı öğrenin
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: alkohli
ms.openlocfilehash: b7cf1d3b9d4a9d751348c4792f904062b00ac104
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76270735"
---
# <a name="use-the-net-sdk-to-initiate-data-transformation"></a>Veri dönüşümlerini başlatmak için .NET SDK'yı kullanın

## <a name="overview"></a>Genel Bakış

Bu makalede, StorSimple aygıt verilerini dönüştürmek için StorSimple Veri Yöneticisi hizmetindeki veri dönüştürme özelliğini nasıl kullanabileceğiniz açıklanmaktadır. Dönüştürülen veriler daha sonra buluttaki diğer Azure hizmetleri tarafından tüketilir.

Veri dönüştürme işini iki şekilde başlatabilirsiniz:

- .NET SDK’yı kullanma
- Azure Otomasyon runbook'u kullanma
 
  Bu makalede, bir veri dönüştürme işi başlatmak ve daha sonra tamamlanması için izlemek için bir örnek .NET konsol uygulaması oluşturmak için nasıl ayrıntıları. Otomasyon aracılığıyla veri dönüşümlerini nasıl başlatılasınız hakkında daha fazla bilgi edinmek [için, veri dönüştürme işlerini tetiklemek için Azure Otomasyonu runbook'u kullanın'a](storsimple-data-manager-job-using-automation.md)gidin.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:
*   Çalışan bir bilgisayar:

    - Visual Studio 2012, 2013, 2015 veya 2017.

    - Azure Powershell. [Azure Powershell'i indirin.](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)
*   Kaynak grubu içindeki StorSimple Data Manager'da doğru yapılandırılmış iş tanımı.
*   Gerekli tüm dlls. [GitHub deposundan](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls)bu dlls indirin.
*   [`Get-ConfigurationParams.ps1`](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Data_Manager_Job_Run/Get-ConfigurationParams.ps1)GitHub deposundan komut dosyası.

## <a name="step-by-step-procedure"></a>Adım adım yordam

Veri dönüştürme işi başlatmak için .NET'i kullanmak için aşağıdaki adımları gerçekleştirin.

1. Yapılandırma parametrelerini almak için aşağıdaki adımları yapın:
    1. GitHub `Get-ConfigurationParams.ps1` depo komut dosyasından `C:\DataTransformation` yerinde indirin.
    1. Komut `Get-ConfigurationParams.ps1` dosyasını GitHub deposundan çalıştırın. Aşağıdaki komutu yazın:

        ```
        C:\DataTransformation\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```
        ActiveDirectoryKey ve AppName için istediğiniz değeri aktarabilirsiniz.

2. Bu komut dosyası aşağıdaki değerleri çıkar:
    * İstemci Kimliği
    * Kiracı Kimliği
    * Etkin Dizin anahtarı (yukarıda girilenilenle aynı)
    * Abonelik Kimliği

        ![Yapılandırma parametreleri komut dosyası çıktısı](media/storsimple-data-manager-dotnet-jobs/get-config-parameters.png)

3. Visual Studio 2012, 2013 veya 2015'i kullanarak C# .NET konsol uygulaması oluşturun.

    1. Tanıtım **Görsel Studio 2012/2013/2015**.
    1. **Dosya > Yeni > Proje**'yi seçin.

        ![Proje oluşturma 1](media/storsimple-data-manager-dotnet-jobs/create-new-project-7.png)        
    2. **Visual C# > Konsol Uygulaması > Yüklü > Şablonlarını**seçin.
    3. **Ad**için **DataTransformationApp** girin.
    4. **Konum**için **C:\DataTransformation'ı** seçin.
    6. Projeyi oluşturmak için **Tamam**'a tıklayın.

        ![Proje oluşturma 2](media/storsimple-data-manager-dotnet-jobs/create-new-project-1.png)

4. Şimdi, oluşturduğunuz projede [dlls klasöründe](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/tree/master/Data_Manager_Job_Run/dlls) bulunan tüm dll'leri **Başvuru** olarak ekleyin. dll dosyalarını eklemek için aşağıdakileri gerçekleştirin:

   1. Visual Studio'da, **> Çözüm Gezgini'ni Görüntüle'ye**gidin.
   2. Veri Dönüştürme Uygulaması projesinin solundaki oku tıklatın. **Referanslar'ı** tıklatın ve ardından **Referans Ekle'ye**sağ tıklayın.
    
       ![dlls 1 ekle](media/storsimple-data-manager-dotnet-jobs/create-new-project-4.png)

   3. Paketler klasörünün konumuna göz atın, tüm dll'leri seçin ve **Ekle'yi**tıklatın ve sonra **Tamam'ı**tıklatın.

       ![dlls 2 ekle](media/storsimple-data-manager-dotnet-jobs/create-new-project-6.png)

5. Aşağıdaki **using** bildirimlerini projedeki kaynak dosyasına (Program.cs) ekleyin.

    ```
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using Microsoft.Azure.Management.HybridData.Models;
    using Microsoft.Internal.Dms.DmsWebJob;
    using Microsoft.Internal.Dms.DmsWebJob.Contracts;
    ```
    
6. Aşağıdaki kod veri dönüştürme iş örneğini başlatıyor. Bunu Ana **yönteme**ekleyin. Daha önce elde edilen yapılandırma parametrelerinin değerlerini değiştirin. **Kaynak Grubu Adı** ve Kaynak **Adı**değerlerini takın. **ResourceGroupName,** iş tanımının yapılandırıldığı StorSimple Veri Yöneticisi ile ilişkilidir. **ResourceName,** StorSimple Data Manager hizmetinizin adıdır.

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

    (VEYA)

    Çalışma süresi sırasında iş tanımı parametrelerini değiştirmek istiyorsanız, aşağıdaki kodu ekleyin:

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

8. Başlatmadan sonra, iş tanımında bir veri dönüştürme işini tetiklemek için aşağıdaki kodu ekleyin. Uygun İş **Tanımı Adını**takın.

    ```
    // Trigger a job, retrieve the jobId and the retry interval for polling.
    int retryAfter;
    string jobId = dataTransformationJob.RunJobAsync(jobDefinitionName, 
    dataTransformationInput, out retryAfter);
    Console.WriteLine("jobid: ", jobId);
    Console.ReadLine();

    ```
    Kod yapıştırıldıktan sonra çözümü oluşturun. Burada veri dönüştürme iş örneği önceletmek için kod snippet bir ekran görüntüsü.

   ![Veri dönüştürme işini başlatmaya çalışmak için kod snippet](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet-1.png)

9. Bu iş, StorSimple birimindeki kök dizini ve dosya filtrelerine uyan verileri dönüştürür ve belirtilen kapsayıcı/dosya paylaşımına koyar. Bir dosya dönüştürüldüğünde, iş tanımıyla aynı ada sahip bir depolama kuyruğuna (kapsayıcı/dosya paylaşımıyla aynı depolama hesabında) bir ileti eklenir. Bu ileti, dosyanın daha fazla işlenmesini başlatmak için tetikleyici olarak kullanılabilir.

10. İş tetiklendikten sonra, tamamlanmak üzere işi izlemek için aşağıdaki kodu kullanabilirsiniz. İş çalışması için bu kodu eklemek zorunlu değildir.

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
    Burada ,NET kullanarak işi tetiklemek için kullanılan tüm kod örneğinin bir ekran görüntüsü ver.

    ![Bir .NET işini tetiklemek için tam kod parçacıkları](media/storsimple-data-manager-dotnet-jobs/start-dotnet-job-code-snippet.png)

## <a name="next-steps"></a>Sonraki adımlar

[Verilerinizi dönüştürmek için StorSimple Data Manager Kullanıcı UI'ı kullanın.](storsimple-data-manager-ui.md)
