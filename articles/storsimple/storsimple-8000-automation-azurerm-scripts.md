---
title: StorSimple aygıtlarını yönetmek için AzureRM PowerShell komut dosyalarını kullanma
description: StorSimple işlerini otomatikleştirmek için Azure Kaynak Yöneticisi komut dosyalarını nasıl kullanacağınızı öğrenin
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 10/03/2017
ms.author: alkohli
ms.openlocfilehash: 03a5ef49b2d58d351d882b30b5d11e4a5ba90264
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471967"
---
# <a name="use-azure-resource-manager-sdk-based-scripts-to-manage-storsimple-devices"></a>StorSimple aygıtlarını yönetmek için Azure Kaynak Yöneticisi SDK tabanlı komut dosyalarını kullanma

Bu makalede, Azure Kaynak Yöneticisi SDK tabanlı komut dosyalarının StorSimple 8000 serisi aygıtınızı yönetmek için nasıl kullanılabileceğini açıklanmaktadır. Bu komut dosyalarını çalıştırmak için ortamınızı yapılandırma adımlarında size yol gösteren örnek bir komut dosyası da dahildir.

Bu makale, yalnızca Azure portalında çalışan StorSimple 8000 serisi aygıtlar için geçerlidir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

Çeşitli StorSimple işlerini otomatikleştirmek için aşağıdaki örnek komut dosyaları kullanılabilir.

#### <a name="table-of-azure-resource-manager-sdk-based-sample-scripts"></a>Azure Kaynak Yöneticisi SDK tabanlı örnek komut dosyaları tablosu

| Azure Kaynak Yöneticisi Komut Dosyası                    | Açıklama                                                                                                                                                                                                       |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Authorize-ServiceEncryptionRollover.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Authorize-ServiceEncryptionRollover.ps1)          | Bu komut dosyası, Servis veri şifreleme anahtarını değiştirmek için StorSimple cihazınızı yetkilendirmenize olanak tanır.                                                                                                           |
| [Create-StorSimpleCloudAppliance.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Create-StorSimpleCloudAppliance.ps1)              | Bu komut dosyası bir 8010 veya 8020 StorSimple Cloud Appliance oluşturur. Bulut cihazı daha sonra StorSimple Data Manager hizmetinize yapılandırılabilir ve kaydedilebilir.                                                       |
| [CreateOrUpdate-Volume.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/CreateOrUpdate-Volume.ps1)                        | Bu komut dosyası StorSimple birimlerini oluşturur veya değiştirir.                                                                                                                                                             |
| [Al-DeviceBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceBackup.ps1)                             | Bu komut dosyası, StorSimple Device Manager hizmetinize kayıtlı bir aygıtın tüm yedeklerini listeler.                                                                                                          |
| [Get-DeviceBackupPolicy.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceBackupPolicy.ps1)                       | Bu komut dosyası StorSimple cihazınız için tüm yedekleme ilkeleri.                                                                                                                                                 |
| [Get-DeviceJobs.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1)                               | Bu komut dosyası, StorSimple Device Manager hizmetinizde çalışan tüm StorSimple işlerini alır.                                                                                                                     |
| [Get-DeviceUpdateAvailability.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceUpdateAvailability.ps1)                 | Bu komut dosyası, Update sunucusunu tarar ve StorSimple cihazınıza yüklemek için güncelleştirmelerin kullanılabilir olup olmadığını bilmenizi sağlar.                                                                                          |
| [Install-DeviceUpdate.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Install-DeviceUpdate.ps1)                         | Bu komut dosyası, kullanılabilir güncelleştirmeleri StorSimple cihazınıza yükler.                                                                                                                                           |
| [Yönet-CloudSnapshots.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Manage-CloudSnapshots.ps1)                        | Bu komut dosyası, el ile bulut anlık görüntüsünü başlatır ve belirtilen bekletme günlerinden daha eski bulut anlık görüntülerini siler.                                                                                                   |
| [Monitör-Backups.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Monitor-Backups.ps1)                              | Bu Azure Otomasyon Runbook PowerShell komut dosyası, tüm yedekleme işlerinin durumunu bildirir.                                                                                                              |
| [Kaldır-AygıtYedekleme.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Remove-DeviceBackup.ps1)                          | Bu komut dosyası tek bir yedekleme nesnesi siler.                                                                                                                                                           |
| [Başlat-AygıtBackupJob.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Start-DeviceBackupJob.ps1)                        | Bu komut dosyası, StorSimple cihazınızda manuel yedekleme yi başlatır.                                                                                                                                       |
| [Güncelleme-CloudApplianceServiceEncryptionKey.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Update-CloudApplianceServiceEncryptionKey.ps1)    | Bu komut dosyası, StorSimple Device Manager hizmetinize kayıtlı tüm 8010/8020 StorSimple Cloud Appliances için hizmet veri şifreleme anahtarını güncelleştirir.                                     |
| [Verify-BackupScheduleAndBackup.ps1](https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Verify-BackupScheduleAndBackup.ps1)               | Bu komut dosyası, yedekleme ilkeleriyle ilişkili tüm zamanlamaları çözümledikten sonra eksik yedeklemeleri vurgular. Ayrıca, yedek kataloğu kullanılabilir yedekleme listesiyle birlikte doğrular.             |




## <a name="configure-and-run-a-sample-script"></a>Örnek komut dosyalarını yapılandırma ve çalıştırma

Bu bölümde örnek bir komut dosyası alır ve komut dosyasını çalıştırmak için gereken çeşitli adımları ayrıntıları.

### <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:

*   Azure PowerShell yüklendi. Azure PowerShell modüllerini yüklemek için:
    * Windows ortamında, [Azure PowerShell'i Yükle ve yapılandırma](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps)adımlarını izleyin. Kullanıyorsanız StorSimple'ınız için Windows Server ana bilgisayarınızda Azure PowerShell yükleyebilirsiniz.
    * Linux veya MacOS ortamında, Azure [PowerShell'i Yükleme ve MacOS veya Linux'ta yapılandırma](https://docs.microsoft.com/powershell/azure/azurerm/install-azurermps-maclinux)adımlarını izleyin.

Azure PowerShell'i kullanma hakkında daha fazla bilgi için [Azure PowerShell'i kullanmaya başlayın.](https://docs.microsoft.com/powershell/azure/get-started-azureps)

### <a name="run-azure-powershell-script"></a>Azure PowerShell komut dosyasını çalıştırma

Bu örnekte kullanılan komut dosyası, StorSimple aygıtındaki tüm işleri listeler. Bu, başarılı olan, başarısız olan veya devam eden işleri içerir. Komut dosyasını indirmek ve çalıştırmak için aşağıdaki adımları gerçekleştirin.

1. Azure PowerShell’i çalıştırın. Yeni bir klasör oluşturun ve dizini yeni klasöre değiştirin.

    ```
        mkdir C:\scripts\StorSimpleSDKTools
        cd C:\scripts\StorSimpleSDKTools
    ```    
2. [NuGet CLI'yi](https://www.nuget.org/downloads) önceki adımda oluşturulan klasörün altına indirin. _Nuget.exe_çeşitli sürümleri vardır. SDK'nıza karşılık gelen sürümü seçin. Her indirme bağlantısı doğrudan bir _.exe_ dosyasına işaret edin. Dosyayı tarayıcıdan çalıştırmak yerine sağ tıklayıp bilgisayarınıza kaydettiğinizden emin olun.

    Komut dosyasını daha önce oluşturduğunuz klasörü indirmek ve depolamak için aşağıdaki komutu da çalıştırabilirsiniz.
    
    ```
        wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```
3. Bağımlı SDK'yı indirin.

    ```
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
        C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```    
4. Komut dosyasını örnek GitHub projesinden indirin.

    ```
        wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1 -Out Get-DeviceJobs.ps1

    ```

5. Betiği çalıştırın. Kimlik doğrulaması istendiğinde Azure kimlik bilgilerinizi sağlayın. Bu komut dosyası, StorSimple aygıtınızdaki tüm işlerin filtrelenmiş bir listesini çıktırmalıdır.
           
    ```           
        .\Get-StorSimpleJob.ps1 -SubscriptionId [subid] -TenantId [tenant id] -DeviceName [name of device] -ResourceGroupName [name of resource group] -ManagerName[name of device manager] -FilterByStatus [Filter for job status] -FilterByJobType [Filter for job type] -FilterByStartTime [Filter for start date time] -FilterByEndTime [Filter for end date time]

    ```

### <a name="sample-output"></a>Örnek çıktı

Örnek komut dosyası çalıştırıldığında aşağıdaki çıktı sunulur. Çıktı, 25 Eylül 2017'de başlayan ve 2 Ekim 2017'de tamamlanan kayıtlı bir cihazda çalıştırılan tüm işleri içerir.

```
-----------------------------------------
Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

PS C:\Scripts\StorSimpleSDKTools> wget https://raw.githubusercontent.com/anoobbacker/storsimpledevicemgmttools/master/Get-DeviceJobs.ps1 -Out Get-DeviceJobs.ps1
PS C:\Scripts\StorSimpleSDKTools> .\Get-DeviceJobs.ps1 -SubscriptionId 1234ab5c-678d-910e-9fc4-0accc9c0166e -TenantId 12a345bc-67d8-91ef-01ab-2c7cd123ef45 -DeviceName 8600-ABC1234567D89EF -ResourceGroupName Contoso -ManagerName ContosoDeviceMgr -FilterByStartTime "09/25/2017 08:10:02" -FilterByEndTime "10/02/2017 08:10:02"


Status            : Succeeded
StartTime         : 10/2/2017 10:30:02 PM
EndTime           : 10/2/2017 10:31:36 PM
PercentComplete   : 100
Error             :
JobType           : ScheduledBackup
DataStats         : Microsoft.Azure.Management.StorSimple8000Series.Models.DataStatistics
EntityLabel       : ss-asr-policy1
EntityType        : Microsoft.StorSimple/managers/devices/backupPolicies
JobStages         :
DeviceId          : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-SHG0997877L71FC
IsCancellable     : True
BackupType        : CloudSnapshot
SourceDeviceId    :
BackupPointInTime : 1/1/0001 12:00:00 AM
Id                : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-SHG0997877L71FC/jobs/75905c48-b153-4af1-8b21-4b9a2ff9
                    825b
Name              : 75905c48-b153-4af1-8b21-4b9a2ff9825b
Type              : Microsoft.StorSimple/managers/devices/jobs
Kind              : Series8000
-------------------------------------------
CUT              CUT  
-------------------------------------------
Status            : Succeeded
StartTime         : 9/26/2017 5:00:02 PM
EndTime           : 9/26/2017 5:01:20 PM
PercentComplete   : 100
Error             :
JobType           : ScheduledBackup
DataStats         : Microsoft.Azure.Management.StorSimple8000Series.Models.DataStatistics
EntityLabel       : 8010 policy
EntityType        : Microsoft.StorSimple/managers/devices/backupPolicies
JobStages         :
DeviceId          : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-ABC1234567D89EF
IsCancellable     : True
BackupType        : CloudSnapshot
SourceDeviceId    :
BackupPointInTime : 1/1/0001 12:00:00 AM
Id                : /subscriptions/1234ab5c-678d-910e-9fc4-0accc9c0166e/resourceGroups/Contoso/providers/Microsoft.Stor
                    Simple/managers/ContosoDeviceMgr/devices/8600-ABC1234567D89EF/jobs/3cfd8108-db60-4e9a-a8da-6d8fe457
                    8d2b
Name              : 3cfd8108-db60-4e9a-a8da-6d8fe4578d2b
Type              : Microsoft.StorSimple/managers/devices/jobs
Kind              : Series8000



PS C:\Scripts\StorSimpleSDKTools>
--------------------------------------------

```


## <a name="next-steps"></a>Sonraki adımlar

[StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)
