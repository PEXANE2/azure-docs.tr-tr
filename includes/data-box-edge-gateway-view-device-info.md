---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: d5af557a62f4bd35c242d334c28a38c3d632f7cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188831"
---
1. [PowerShell arabirimine bağlanın.](#connect-to-the-powershell-interface)
2. `Get-HcsApplianceInfo` Cihazınız için bilgi almak için kullanın.

    Aşağıdaki örnek, bu cmdlet kullanımını gösterir:

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Önemli aygıt bilgilerinden bazılarını özetleyen bir tablo aşağıda veda edebilirsiniz:
    
    | Parametre                             | Açıklama                                                                                                                                                  |   |
    |--------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    | Friendlyname                   | Aygıtın, aygıt dağıtımı sırasında yerel web Kullanıcı Arabirimi aracılığıyla yapılandırıldığı gibi, aygıtın dostu adı. Varsayılan dostu ad, aygıt seri numarasıdır.  |   |
    | SerialNumber                   | Aygıt seri numarası fabrikada atanan benzersiz bir numaradır.                                                                             |   |
    | Model                          | Veri Kutusu Kenar veya Veri Kutusu Ağ Geçidi aygıtınızın modeli. Model, Veri Kutusu Ağ Geçidi için sanal ve Data Box Edge için fizikseldir.                   |   |
    | FriendlySoftwareVersion        | Aygıt yazılımı sürümüne karşılık gelen dostu dize. Önizleme çalışan bir sistem için, dostu yazılım sürümü Data Box Edge 1902 olacaktır. |   |
    | HcsVersion                     | Cihazınızda çalışan HCS yazılım sürümü. Örneğin, Data Box Edge 1902'ye karşılık gelen HCS yazılım sürümü 1.4.771.324'dür.            |   |
    | LocalCapacityInmb              | Megabit'teki cihazın toplam yerel kapasitesi.                                                                                                        |   |
    | IsRegistered                   | Bu değer, cihazınızın hizmetle etkinleştirilip etkinleştirilmediğini gösterir.                                                                                         |   |


