---
title: PowerShell ile Azure Dosyalarını Geri Yükleme
description: Bu makalede, Azure Yedekleme hizmetini ve PowerShell'i kullanarak Azure Dosyalarını nasıl geri yükleyin öğrenin.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 99aeaa6173bb5336e6e1719a9fc0df0c668374e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086831"
---
# <a name="restore-azure-files-with-powershell"></a>PowerShell ile Azure Dosyalarını Geri Yükleme

Bu makalede, Azure Powershell kullanarak [Azure Yedekleme](backup-overview.md) hizmeti tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımının veya belirli dosyaların nasıl geri yüklenir.

Tüm dosya paylaşımını veya paylaşımla ilgili belirli dosyaları geri yükleyebilirsiniz. Özgün konuma veya başka bir konuma geri yükleyebilirsiniz.

> [!WARNING]
> PS sürümünün AFS yedeklemeleri için 'Az.RecoveryServices 2.6.0' için minimum sürüme yükseltildiklerinden emin olun. Daha fazla ayrıntı için, bu değişikliğin gereksinimini özetleyen [bölüme](backup-azure-afs-automation.md#important-notice---backup-item-identification-for-afs-backups) bakın.

## <a name="fetch-recovery-points"></a>Kurtarma noktalarını getir

Yedeklenen öğenin tüm kurtarma noktalarını listelemek için [Get-AzRecoveryServicesBackupRecoveryPoint'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint?view=azps-1.4.0) kullanın.

Aşağıdaki komut dosyasında:

* Değişken **$rp,** seçilen yedekleme öğesi için son yedi gün içinde bir dizi kurtarma noktasıdır.
* Dizi, dizin **0'daki**en son kurtarma noktasıyla ters sırayla sıralanır.
* Kurtarma noktasını seçmek için standart PowerShell dizi dizi dizi dizisini kullanın.
* Örnekte, **$rp[0]** en son kurtarma noktasını seçer.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

Çıktı aşağıdakine benzer.

```powershell
FileShareSnapshotUri : https://testStorageAcct.file.core.windows.net/testAzureFS?sharesnapshot=2018-11-20T00:31:04.00000
                       00Z
RecoveryPointType    : FileSystemConsistent
RecoveryPointTime    : 11/20/2018 12:31:05 AM
RecoveryPointId      : 86593702401459
ItemName             : testAzureFS
Id                   : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Micros                      oft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;teststorageRG;testStorageAcct/protectedItems/AzureFileShare;testAzureFS/recoveryPoints/86593702401462
WorkloadType         : AzureFiles
ContainerName        : storage;teststorageRG;testStorageAcct
ContainerType        : AzureStorage
BackupManagementType : AzureStorage
```

İlgili kurtarma noktası seçildikten sonra, dosya paylaşımını veya dosyayı özgün konuma veya alternatif bir konuma geri yüklersiniz.

## <a name="restore-an-azure-file-share-to-an-alternate-location"></a>Azure dosya paylaşımını alternatif bir konuma geri yükleme

Seçili kurtarma noktasına geri yüklemek için [Geri Yükleme-AzRecoveryServicesBackupItem'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) kullanın. Alternatif konumu tanımlamak için bu parametreleri belirtin:

* **TargetStorageAccountName**: Yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **TargetFileShareName**: Yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşımları.
* **TargetFolder**: Dosya payının altındaki klasör, verilerin geri yüklendiği klasördür. Yedeklenen içerik bir kök klasörüne geri yüklenecekse, hedef klasör değerlerini boş bir dize olarak verin.
* **Çözüm:** Geri yüklenen verilerle çakışması varsa yönerge. **Overwrite** veya **Skip'i**kabul eder.

Aşağıdaki parametreleri ile cmdlet çalıştırın:

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
```

Komut, aşağıdaki örnekte gösterildiği gibi izlenecek kimliği olan bir işi döndürür.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
```

## <a name="restore-an-azure-file-to-an-alternate-location"></a>Azure dosyasını alternatif bir konuma geri yükleme

Seçili kurtarma noktasına geri yüklemek için [Geri Yükleme-AzRecoveryServicesBackupItem'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) kullanın. Alternatif konumu tanımlamak ve geri yüklemek istediğiniz dosyayı benzersiz olarak tanımlamak için bu parametreleri belirtin.

* **TargetStorageAccountName**: Yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **TargetFileShareName**: Yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşımları.
* **TargetFolder**: Dosya payının altındaki klasör, verilerin geri yüklendiği klasördür. Yedeklenen içerik bir kök klasörüne geri yüklenecekse, hedef klasör değerlerini boş bir dize olarak verin.
* **SourceFilePath**: Dosyanın mutlak yolu, dosya payı içinde geri yüklenecek, bir dize olarak. Bu yol **Get-AzStorageFile** PowerShell cmdlet kullanılan aynı yoldur.
* **SourceFileType**: Dizin veya dosya seçilip seçilmediği. **Dizin** veya **Dosya**kabul eder.
* **Çözüm:** Geri yüklenen verilerle çakışması varsa yönerge. **Overwrite** veya **Skip'i**kabul eder.

Ek parametreler (SourceFilePath ve SourceFileType) yalnızca geri yüklemek istediğiniz tek tek dosyayla ilişkilidir.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Bu komut, önceki bölümde gösterildiği gibi izlenecek kimliği olan bir işi döndürür.

## <a name="restore-azure-file-shares-and-files-to-the-original-location"></a>Azure dosya paylaşımlarını ve dosyalarını özgün konuma geri yükleme

Özgün bir konuma geri yüklediğinizde, hedef ve hedefle ilgili parametreleri belirtmeniz gerekmez. Yalnızca **ÇözümÇakış** sağlanmalıdır.

#### <a name="overwrite-an-azure-file-share"></a>Azure dosya paylaşımının üzerine yazma

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Azure dosyasının üzerine yazma

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="next-steps"></a>Sonraki adımlar

Azure portalında Azure Dosyalarını geri alma [hakkında bilgi edinin.](restore-afs.md)
