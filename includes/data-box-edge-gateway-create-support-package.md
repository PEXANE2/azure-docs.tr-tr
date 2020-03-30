---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: b657ee32e76dd90671f7e91337ced01b925889a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188830"
---
Herhangi bir aygıt sorunuyla karşılaşırsanız, sistem günlüklerinden bir destek paketi oluşturabilirsiniz. Microsoft Destek sorunları gidermek için bu paketi kullanır. Destek paketi oluşturmak için aşağıdaki adımları izleyin:

1. [Cihazınızın PowerShell arayüzüne bağlanın.](#connect-to-the-powershell-interface)
2. Destek `Get-HcsNodeSupportPackage` paketi oluşturmak için komutu kullanın. Cmdlet kullanımı aşağıdaki gibidir:

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

    Cmdlet, cihazınızdaki günlükleri toplar ve bu günlükleri belirli bir ağa veya yerel paylaşıma kopyalar.

    Kullanılan parametreler aşağıdaki gibidir:

    - `-Path`- Destek paketini kopyalamak için ağı veya yerel yolu belirtin. (gerekli)
    - `-Credential`- Korunan yola erişmek için kimlik bilgilerini belirtin.
    - `-Zip`- Zip dosyası oluşturmak için belirtin.
    - `-Include`- Destek paketine eklenecek bileşenleri içerecek şekilde belirtin. Belirtilmemişse, `Default` kabul edilir.
    - `-IncludeArchived`- Destek paketine arşivlenmiş günlükleri eklemek için belirtin.
    - `-IncludePeriodicStats`- Destek paketine periyodik stat günlüklerini eklemek için belirtin.

    
