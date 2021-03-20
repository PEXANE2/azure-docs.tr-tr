---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: b657ee32e76dd90671f7e91337ced01b925889a1
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "67188830"
---
Herhangi bir cihaz sorunu yaşarsanız, sistem günlüklerinden bir destek paketi oluşturabilirsiniz. Microsoft Desteği sorunları gidermek için bu paketi kullanır. Destek paketi oluşturmak için aşağıdaki adımları izleyin:

1. [Cihazınızın PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. `Get-HcsNodeSupportPackage`Bir destek paketi oluşturmak için komutunu kullanın. Cmdlet 'in kullanımı aşağıdaki gibidir:

    ```powershell
    Get-HcsNodeSupportPackage [-Path] <string> [-Zip] [-ZipFileName <string>] [-Include {None | RegistryKeys | EtwLogs
            | PeriodicEtwLogs | LogFiles | DumpLog | Platform | FullDumps | MiniDumps | ClusterManagementLog | ClusterLog |
            UpdateLogs | CbsLogs | StorageCmdlets | ClusterCmdlets | ConfigurationCmdlets | KernelDump | RollbackLogs |
            Symbols | NetworkCmdlets | NetworkCmds | Fltmc | ClusterStorageLogs | UTElement | UTFlag | SmbWmiProvider |
            TimeCmds | LocalUILogs | ClusterHealthLogs | BcdeditCommand | BitLockerCommand | DirStats | ComputeRolesLogs |
            ComputeCmdlets | DeviceGuard | Manifests | MeasuredBootLogs | Stats | PeriodicStatLogs | MigrationLogs |
            RollbackSupportPackage | ArchivedLogs | Default}] [-MinimumTimestamp <datetime>] [-MaximumTimestamp <datetime>]
            [-IncludeArchived] [-IncludePeriodicStats] [-Credential <pscredential>]  [<CommonParameters>]
    ```

    Cmdlet 'i cihazınızdan günlükleri toplar ve bu günlükleri belirtilen bir ağa veya yerel paylaşıma kopyalar.

    Kullanılan parametreler aşağıdaki gibidir:

    - `-Path` -Destek paketini kopyalamak için ağı veya yerel yolu belirtin. istenir
    - `-Credential` -Korumalı yola erişmek için kimlik bilgilerini belirtin.
    - `-Zip` -Bir ZIP dosyası oluşturmak için belirtin.
    - `-Include` -Destek paketine dahil edilecek bileşenleri dahil etmek için belirtin. Belirtilmezse, `Default` varsayılır.
    - `-IncludeArchived` -Destek paketine arşivlenmiş günlükleri dahil etmek için belirtin.
    - `-IncludePeriodicStats` -Destek paketine düzenli stat günlüklerini dahil etmek için belirtin.

    
