---
title: Azure dosya paylaşma yedeklemelerini PowerShell ile yönetme
description: Azure Backup hizmeti tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için PowerShell 'in nasıl kullanılacağını öğrenin.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 889c9bb3ef087c700bbfc3a68959f2c5924bffda
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86538602"
---
# <a name="manage-azure-file-share-backups-with-powershell"></a>Azure dosya paylaşma yedeklemelerini PowerShell ile yönetme

Bu makalede, Azure Backup hizmeti tarafından yedeklenen Azure dosya paylaşımlarını yönetmek ve izlemek için Azure PowerShell nasıl kullanılacağı açıklanır.

> [!WARNING]
> AFS yedeklemeleri için PS sürümünün ' az. RecoveryServices 2.6.0 ' için en düşük sürüme yükseltildiğinden emin olun. Daha fazla ayrıntı için bu değişikliğin gereksinimini seviyelendirme [bölümüne](backup-azure-afs-automation.md#important-notice-backup-item-identification) bakın.

## <a name="modify-the-protection-policy"></a>Koruma ilkesini değiştirme

Azure dosya paylaşımının yedeklenmesi için kullanılan ilkeyi değiştirmek için [Enable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection)kullanın. İlgili yedekleme öğesini ve yeni yedekleme ilkesini belirtin.

Aşağıdaki örnek, **Testazurefs** koruma ilkesini, **davilyafs** 'den **monthlyafs**olarak değiştirir.

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="track-backup-and-restore-jobs"></a>Yedekleme ve geri yükleme işlerini izleme

İsteğe bağlı yedekleme ve geri yükleme işlemleri, [isteğe bağlı yedekleme çalıştırdığınızda](backup-azure-afs-automation.md#trigger-an-on-demand-backup)gösterildiği gıbı, kimlik ile birlikte bir iş döndürür. İş ilerleme durumunu ve ayrıntılarını izlemek için [Get-AzRecoveryServicesBackupJobDetails](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) cmdlet 'ini kullanın.

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

* Gelecekteki tüm yedekleme işlerini Durdur ve tüm kurtarma noktalarını *Sil*
* Gelecekteki tüm yedekleme işlerini durdur, ancak kurtarma noktalarını *bırak*

Azure Backup tarafından oluşturulan temeldeki anlık görüntüler bekletileceği için, kurtarma noktalarını depolamada bırakma ile ilişkili bir maliyet olabilir. Ancak, kurtarma noktalarından ayrılmasının avantajı, daha sonra isterseniz dosya paylaşımının geri yüklenmesini sağlayabilir. Kurtarma noktalarından ayrılma maliyeti hakkında daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/storage/files/). Tüm kurtarma noktalarını silmeyi seçerseniz dosya paylaşımından geri yükleme yapamazsınız.

## <a name="stop-protection-and-retain-recovery-points"></a>Korumayı durdurun ve kurtarma noktalarını koruyun

Verileri korurken korumayı durdurmak için [Disable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection) cmdlet 'ini kullanın.

Aşağıdaki örnek, *afsfileshare* dosya paylaşımının korumasını durduruyor ancak tüm kurtarma noktalarını korur:

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

Çıkışdaki Iş KIMLIĞI özniteliği, "korumayı Durdur" işleminin yedekleme hizmeti tarafından oluşturulan işin Iş KIMLIĞINE karşılık gelir. İşin durumunu izlemek için [Get-AzRecoveryServicesBackupJob](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) cmdlet 'ini kullanın.

## <a name="stop-protection-without-retaining-recovery-points"></a>Kurtarma noktalarını korumadan korumayı durdur

Kurtarma noktalarını korumadan korumayı durdurmak için [Disable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection) cmdlet 'ini kullanın ve **-RemoveRecoveryPoints** parametresini ekleyin.

Aşağıdaki örnek, kurtarma noktalarını korumadan *afsfileshare* dosya paylaşımının korumasını durduruyor:

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

Azure portal Azure dosya paylaşma yedeklemelerini yönetme [hakkında bilgi edinin](manage-afs-backup.md) .
