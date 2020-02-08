---
title: Azure dosyalarını PowerShell ile geri yükleme
description: Bu makalede, Azure Backup hizmetini ve PowerShell 'i kullanarak Azure dosyalarını geri yüklemeyi öğrenin.
ms.topic: conceptual
ms.date: 1/27/2020
ms.openlocfilehash: 99aeaa6173bb5336e6e1719a9fc0df0c668374e2
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77086831"
---
# <a name="restore-azure-files-with-powershell"></a>Azure dosyalarını PowerShell ile geri yükleme

Bu makalede, Azure PowerShell kullanılarak [Azure Backup](backup-overview.md) hizmeti tarafından oluşturulan bir geri yükleme noktasından tüm dosya paylaşımının veya belirli dosyaların nasıl geri yükleneceği açıklanmaktadır.

Dosya paylaşımının tamamını veya paylaşımdaki belirli dosyaları geri yükleyebilirsiniz. Özgün konuma veya alternatif bir konuma geri yükleyebilirsiniz.

> [!WARNING]
> AFS yedeklemeleri için PS sürümünün ' az. RecoveryServices 2.6.0 ' için en düşük sürüme yükseltildiğinden emin olun. Daha fazla ayrıntı için bu değişikliğin gereksinimini seviyelendirme [bölümüne](backup-azure-afs-automation.md#important-notice---backup-item-identification-for-afs-backups) bakın.

## <a name="fetch-recovery-points"></a>Kurtarma noktalarını getir

Yedeklenen öğenin tüm kurtarma noktalarını listelemek için [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint?view=azps-1.4.0) komutunu kullanın.

Aşağıdaki betikte:

* **$RP** değişkeni, son yedi gündeki seçili yedekleme öğesi için bir kurtarma noktaları dizisidir.
* Dizi, Dizin **0**' daki en son kurtarma noktasıyla ters sırada sıralanır.
* Kurtarma noktasını seçmek için standart PowerShell dizisi dizinlemeyi kullanın.
* Örnekte, **$RP [0]** en son kurtarma noktasını seçer.

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

İlgili kurtarma noktası seçildikten sonra, dosya paylaşma veya dosyayı özgün konuma veya alternatif bir konuma geri yükleyin.

## <a name="restore-an-azure-file-share-to-an-alternate-location"></a>Azure dosya paylaşımından alternatif bir konuma geri yükleme

Seçili kurtarma noktasına geri yüklemek için [restore-Azrecoveryservicesbackupıtem öğesini](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) kullanın. Alternatif konumu tanımlamak için bu parametreleri belirtin:

* **Targetstorageaccountname**: yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **Targetfilesharename**: yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşımları.
* **TargetFolder**: verilerin geri yüklendiği dosya paylaşımının altındaki klasör. Yedeklenen içerik bir kök klasöre geri yüklenirse, hedef klasör değerlerini boş bir dize olarak verin.
* **ResolveConflict**: geri yüklenen verilerle bir çakışma varsa yönerge. **Overwrite** veya **Skip**kabul eder.

Cmdlet 'ini parametrelerle aşağıdaki gibi çalıştırın:

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
```

Komut, aşağıdaki örnekte gösterildiği gibi, bir KIMLIĞE sahip bir iş döndürür.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
```

## <a name="restore-an-azure-file-to-an-alternate-location"></a>Azure dosyasını alternatif bir konuma geri yükleme

Seçili kurtarma noktasına geri yüklemek için [restore-Azrecoveryservicesbackupıtem öğesini](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem?view=azps-1.4.0) kullanın. Alternatif konumu tanımlamak ve geri yüklemek istediğiniz dosyayı benzersiz olarak tanımlamak için bu parametreleri belirtin.

* **Targetstorageaccountname**: yedeklenen içeriğin geri yüklendiği depolama hesabı. Hedef depolama hesabı kasayla aynı konumda olmalıdır.
* **Targetfilesharename**: yedeklenen içeriğin geri yüklendiği hedef depolama hesabı içindeki dosya paylaşımları.
* **TargetFolder**: verilerin geri yüklendiği dosya paylaşımının altındaki klasör. Yedeklenen içerik bir kök klasöre geri yüklenirse, hedef klasör değerlerini boş bir dize olarak verin.
* **SourceFilePath**: dosya paylaşımında, bir dize olarak geri yüklenecek dosyanın mutlak yolu. Bu yol, **Get-AzStorageFile** PowerShell cmdlet 'inde kullanılan yoldur.
* **Sourcefiletype**: bir dizin veya dosya seçili olup olmadığı. **Dizini** veya **dosyayı**kabul eder.
* **ResolveConflict**: geri yüklenen verilerle bir çakışma varsa yönerge. **Overwrite** veya **Skip**kabul eder.

Ek parametreler (SourceFilePath ve SourceFileType) yalnızca geri yüklemek istediğiniz tek dosya ile ilgilidir.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Bu komut, önceki bölümde gösterildiği gibi, bir KIMLIĞE sahip bir iş döndürür.

## <a name="restore-azure-file-shares-and-files-to-the-original-location"></a>Azure dosya paylaşımlarını ve dosyalarını özgün konumuna geri yükleme

Özgün bir konuma geri yüklediğinizde, hedef ve hedefle ilgili parametreleri belirtmeniz gerekmez. Yalnızca **ResolveConflict** sağlanmalıdır.

#### <a name="overwrite-an-azure-file-share"></a>Azure dosya paylaşımının üzerine yaz

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Azure dosyasının üzerine yaz

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="next-steps"></a>Sonraki adımlar

Azure portal Azure dosyalarını geri yükleme [hakkında bilgi edinin](restore-afs.md) .
