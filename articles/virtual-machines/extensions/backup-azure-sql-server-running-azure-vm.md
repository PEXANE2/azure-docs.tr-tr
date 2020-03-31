---
title: Azure VM'de çalışan SQL Server için Azure Yedekleme
description: Bu makalede, Azure sanal makinede çalışan SQL Server'da Azure Yedekleme'yi nasıl kaydedebilirsiniz öğrenin.
services: backup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: dacurwin
ms.openlocfilehash: b17e4031edaedc6b0a63d305d20a77e5b58f91ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247393"
---
# <a name="azure-backup-for-sql-server-running-in-azure-vm"></a>Azure VM'de çalışan SQL Server için Azure Yedekleme

Azure Yedekleme, diğer tekliflerin yanı sıra, Azure VM'lerde çalışan SQL Server gibi iş yüklerini yedeklemek için destek sağlar. SQL uygulaması bir Azure VM içinde çalışıyor olduğundan, yedekleme hizmetinin uygulamaya erişmek ve gerekli ayrıntıları almak için izin alması gerekir.
Bunu yapmak için Azure Yedekleme, kullanıcı tarafından tetiklenen kayıt işlemi sırasında SQL Server'ın çalışmakta olduğu VM'ye **AzureBackupWindowsİş iş yükü** uzantısını yükler.

## <a name="prerequisites"></a>Ön koşullar

Desteklenen senaryolar listesi için Azure Yedekleme tarafından desteklenen [desteklenebilirlik matrisine](../../backup/sql-support-matrix.md#scenario-support) bakın.

## <a name="network-connectivity"></a>Ağ bağlantısı

Azure Yedekleme, bir proxy sunucusu veya listelenen IP aralıkları dağıtarak NSG Etiketlerini destekler; yöntemlerin her biri hakkında ayrıntılı bilgi için bu [makaleye](https://docs.microsoft.com/azure/backup/backup-sql-server-database-azure-vms#establish-network-connectivity)bakın.

## <a name="extension-schema"></a>Uzantı şeması

Uzantı şeması ve özellik değerleri, hizmetin CRP API'ye geçtiği yapılandırma değerleri (çalışma zamanı ayarları) dır. Bu config değerleri kayıt ve yükseltme sırasında kullanılır. **AzureBackupWindowsWorkload** uzantısı da bu şema kullanır. Şema önceden ayarlanmıştır; objectStr alanına yeni bir parametre eklenebilir

  ```json
      "runtimeSettings": [{
      "handlerSettings": {
      "protectedSettingsCertThumbprint": "",
      "protectedSettings": {
      "objectStr": "",
      "logsBlobUri": "",
      "statusBlobUri": ""
      }
      },
      "publicSettings": {
      "locale": "en-us",
      "taskId": "1c0ae461-9d3b-418c-a505-bb31dfe2095d",
      "objectStr": "",
      "commandStartTimeUTCTicks": "636295005824665976",
      "vmType": "vmType"
      }
      }]
      }
  ```

Aşağıdaki JSON, WorkloadBackup uzantısı için şema gösterir.  

  ```json
  {
    "type": "extensions",
    "name": "WorkloadBackup",
    "location":"<myLocation>",
    "properties": {
      "publisher": "Microsoft.RecoveryServices",
      "type": "AzureBackupWindowsWorkload",
      "typeHandlerVersion": "1.1",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "locale":"<location>",
        "taskId":"<TaskId used by Azure Backup service to communicate with extension>",

        "objectStr": "<The configuration passed by Azure Backup service to extension>",

        "commandStartTimeUTCTicks": "<Scheduled start time of registration or upgrade task>",
        "vmType": "<Type of VM where registration got triggered Eg. Compute or ClassicCompute>"
      },
      "protectedSettings": {
        "objectStr": "<The sensitive configuration passed by Azure Backup service to extension>",
        "logsBlobUri": "<blob uri where logs of command execution by extension are written to>",
        "statusBlobUri": "<blob uri where status of the command executed by extension is written>"
      }
    }
  }
  ```

### <a name="property-values"></a>Özellik değerleri

Adı | Değer/örnek | Veri türü
 --- | --- | ---
yerel ayar | tr-tr  |  string
taskId | "1c0ae461-9d3b-418c-a505-bb31dfe2095d"  | string
nesneStr <br/> (genel Ayarlar)  | "eyJjb250YWluZXJQcm9wZXJ0aWVzIjp7IkNvbnRhaW5lcklEIJoiMzVjMjQXYTItOGRjNy00ZGE5LWI4NTMtMjjJHHNDM2ZKIwiSWRNZ210Q29ud GGFpbmVySWQiOjM0NTY3ODg5LCJSZXNvdXJjZUlkIjoiMDU5NWIwOGEtYzI4Zi00ZmFlLWE5ODItOTtwOWMyMGVJNjVhIiwiU3Vic2NyaXB0aW9uSWQiOiJKNG EzOTliNy1iYjAyLTQ2MWMtODmYS1jNTM5ODI3ZTgzNTQiLCJVbmlxdWVDb250YWluZXJOYW1lIjoiODM4MDZJODUtNTTQ4OS00NMNhLWEyZTCtNMZNMZNzNhYjg3OTcyIn0sInN0YW1wTGlzdCI6W3siU2VydmljZU5hbWUiOjUsIlNlcnZpY2VTdGFtcFVybCI6Imh0dHA6XC9cL015V0xGYWJTdMuY29t1dfQ ==" | string
komutStartTimeUTCTicks | "636967192566036845"  | string
vmType  | "microsoft.compute/virtualmachines"  | string
nesneStr <br/> (protectedAyarlar) | "eyJjb250YWluZXJQcm9wZXJ0aWVzIjp7IkNvbnRhaW5lcklEIJoiMzVjMjQXYTItOGRjNy00ZGE5LWI4NTMtMjjJHHNDM2ZKIwiSWRNZ210Q29ud GGFpbmVySWQiOjM0NTY3ODg5LCJSZXNvdXJjZUlkIjoiMDU5NWIwOGEtYzI4Zi00ZmFlLWE5ODItOTtwOWMyMGVJNjVhIiwiU3Vic2NyaXB0aW9uSWQiOiJKNG EzOTliNy1iYjAyLTQ2MWMtODmYS1jNTM5ODI3ZTgzNTQiLCJVbmlxdWVDb250YWluZXJOYW1lIjoiODM4MDZJODUtNTTQ4OS00NMNhLWEyZTCtNMZNMZNzNhYjg3OTcyIn0sInN0YW1wTGlzdCI6W3siU2VydmljZU5hbWUiOjUsIlNlcnZpY2VTdGFtcFVybCI6Imh0dHA6XC9cL015V0xGYWJTdMuY29t1dfQ ==" | string
kütüklerBlobUri | <https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Logs.txt?sv=2014-02-14&sr=b&sig=DbwYhwfeAC5YJzISgxoKk%2FEWQq2AO1vS1E0rDW%2FlsBw%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw> | string
durumBlobUri | <https://seapod01coord1exsapk732.blob.core.windows.net/bcdrextensionlogs-d45d8a1c-281e-4bc8-9d30-3b25176f68ea/sopattna-vmubuntu1404ltsc.v2.Status.txt?sv=2014-02-14&sr=b&sig=96RZBpTKCjmV7QFeXm5IduB%2FILktwGbLwbWg6Ih96Ao%3D&st=2017-11-09T14%3A33%3A29Z&se=2017-11-09T17%3A38%3A29Z&sp=rw> | string

## <a name="template-deployment"></a>Şablon dağıtımı

Sanal makineye AzureBackupWindowsWorkload uzantısı eklemenin, sanal makinede SQL Server yedeklemesini etkinleştirmenizi tavsiye ettik. Bu, SQL Server VM'de yedeklemeyi otomatikleştirmek için tasarlanan [Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-recovery-services-vm-workload-backup) aracılığıyla elde edilebilir.

## <a name="powershell-deployment"></a>PowerShell dağıtımı

Kurtarma hizmetleri kasasıyla SQL uygulamasını içeren Azure VM'sini 'kaydetmeniz' gerekir. Kayıt sırasında AzureBackupWindowsİş uzantısı VM'ye yüklenir. VM'yi kaydetmek için [Register-AzRecoveryServicesBackupContainerPS](https://docs.microsoft.com/powershell/module/az.recoveryservices/Register-AzRecoveryServicesBackupContainer?view=azps-1.5.0) cmdlet kullanın.

```powershell
$myVM = Get-AzVM -ResourceGroupName <VMRG Name> -Name <VMName>
Register-AzRecoveryServicesBackupContainer -ResourceId $myVM.ID -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID -Force
```

Komut bu kaynağın **yedek kapsayıcısını** döndürecek ve durum **kaydedilecek.**

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL Server VM yedekleme sorun giderme yönergeleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/backup/backup-sql-server-azure-troubleshoot)
- Azure sanal makinelerde (VM) çalışan ve Azure Yedekleme hizmetini kullanan SQL Server veritabanlarını yedekleme hakkında [sık sorulan sorular.](https://docs.microsoft.com/azure/backup/faq-backup-sql-server)
