---
title: PowerShell ile Azure dosya paylaşımı yedeklemelerini yönetme
description: Azure Yedekleme hizmeti tarafından desteklenen Azure dosya paylaşımlarını yönetmek ve izlemek için PowerShell'i nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: a9dc421db740963fc5cd11e868eb383694376ce1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083171"
---
# <a name="manage-azure-file-share-backups-with-powershell"></a>PowerShell ile Azure dosya paylaşımı yedeklemelerini yönetme

Bu makalede, Azure Yedekleme hizmeti tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için Azure PowerShell'in nasıl kullanılacağı açıklanmaktadır.

> [!WARNING]
> PS sürümünün AFS yedeklemeleri için 'Az.RecoveryServices 2.6.0' için minimum sürüme yükseltildiklerinden emin olun. Daha fazla ayrıntı için, bu değişikliğin gereksinimini özetleyen [bölüme](backup-azure-afs-automation.md#important-notice---backup-item-identification-for-afs-backups) bakın.

## <a name="modify-the-protection-policy"></a>Koruma ilkesini değiştirme

Azure dosya paylaşımını yedeklemek için kullanılan ilkeyi değiştirmek için [Etkinleştir-AzRecoveryServicesBackupProtection'ı](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0)kullanın. İlgili yedekleme öğesini ve yeni yedekleme ilkesini belirtin.

Aşağıdaki örnek, **testAzureFS** koruma ilkesini **dailyafs'tan** **monthlyafs'a**değiştirir.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="track-backup-and-restore-jobs"></a>Yedekleme ve işleri geri yükleme

İsteğe bağlı yedekleme ve geri yükleme işlemleri, [isteğe bağlı yedekleme çalıştırdığınızda](backup-azure-afs-automation.md#trigger-an-on-demand-backup)gösterildiği gibi, bir işi bir kimlikle birlikte döndürün. İş ilerlemesini ve ayrıntıları izlemek için [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-1.4.0) cmdlet'i kullanın.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support.
```

## <a name="stop-protection-on-a-file-share"></a>Dosya paylaşımı üzerindeki korumayı durdurma

Azure dosya paylaşımlarını korumayı durdurmanın iki yolu vardır:

* Gelecekteki tüm yedekleme işlerini durdurun ve tüm kurtarma noktalarını *silin*
* Gelecekteki tüm yedekleme işlerini durdurun, ancak kurtarma noktalarını *bırakın*

Azure Yedekleme tarafından oluşturulan temel anlık görüntüler korunacağı için, kurtarma noktalarını depolama alanında bırakmanın bir maliyeti olabilir. Ancak, kurtarma noktaları bırakarak yararı, istenirse, daha sonra dosya paylaşımı geri yükleyebilirsiniz. Kurtarma noktalarından çıkmanın maliyeti hakkında bilgi için [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/storage/files/)bakın. Tüm kurtarma noktalarını silmeyi seçerseniz, dosya paylaşımını geri yükleyemezsiniz.

## <a name="stop-protection-and-retain-recovery-points"></a>Korumayı durdurun ve kurtarma noktalarını koruyun

Verileri tutarken korumayı durdurmak için [Devre Dışı Bırak-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection?view=azps-3.3.0) cmdlet'ini kullanın.

Aşağıdaki *örnek, afsfileshare* dosya paylaşımı için korumayı durdurur, ancak tüm kurtarma noktalarını korur:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "afstesting" -Name "afstest" | select -ExpandProperty ID
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureStorage -WorkloadType AzureFiles -Name "afsfileshare" -VaultId $vaultID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $vaultID
```

```output
WorkloadName     Operation         Status         StartTime                 EndTime                   JobID
------------     ---------         ------         ---------                 -------                   -----
afsfileshare     DisableBackup     Completed      1/26/2020 2:43:59 PM      1/26/2020 2:44:21 PM      98d9f8a1-54f2-4d85-8433-c32eafbd793f
```

Çıktıdaki İş Kimliği özniteliği, "korumayı durdurma" işleminiz için yedekleme hizmeti tarafından oluşturulan işin İş Kimliği'ne karşılık gelir. İşin durumunu izlemek için [Get-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob?view=azps-3.3.0) cmdlet'i kullanın.

## <a name="stop-protection-without-retaining-recovery-points"></a>Kurtarma noktalarını tutmadan korumayı durdurma

Kurtarma noktalarını tutmadan korumayı durdurmak [için, Devre Dışı Bırak-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection?view=azps-3.3.0) cmdlet'i kullanın ve **-RemoveRecoveryPoints** parametresini ekleyin.

Aşağıdaki örnek, kurtarma noktalarını tutmadan *afsfileshare* dosya paylaşımı için korumayı durdurur:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "afstesting" -Name "afstest" | select -ExpandProperty ID
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureStorage -WorkloadType AzureFiles -Name "afsfileshare" -VaultId $vaultID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $vaultID -RemoveRecoveryPoints
```

```output
WorkloadName     Operation            Status         StartTime                 EndTime                   JobID
------------     ---------            ------         ---------                 -------                   -----
afsfileshare     DeleteBackupData     Completed      1/26/2020 2:50:57 PM      1/26/2020 2:51:39 PM      b1a61c0b-548a-4687-9d15-9db1cc5bcc85
```

## <a name="next-steps"></a>Sonraki adımlar

Azure portalında Azure dosya paylaşım yedeklemelerini yönetme [hakkında bilgi edinin.](manage-afs-backup.md)
