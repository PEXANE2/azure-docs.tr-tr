---
title: Sanal makineler için geçici silme
description: Sanal makinelerin geçici silme işlemlerini yedeklemelerin daha güvenli hale getirme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 04/30/2020
ms.custom: references_regions
ms.openlocfilehash: e447db2c3f862d2f577a9e7d8767946375abf4e0
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86503549"
---
# <a name="soft-delete-for-virtual-machines"></a>Sanal makineler için geçici silme

VM 'Ler için geçici silme, sanal makinelerinizin yedeklerini istenmeden silinmeye karşı korur. Yedeklemeler silindikten sonra bile, 14 ek gün boyunca geçici silme durumunda saklanır.

> [!NOTE]
> Geçici silme yalnızca silinen yedekleme verilerini korur. Bir VM, yedekleme olmadan silinirse, geçici silme özelliği verileri korumaz. Tam esnekliği sağlamak için tüm kaynakların Azure Backup korumalı olması gerekir.
>

## <a name="supported-regions"></a>Desteklenen bölgeler

Geçici silme Şu anda Orta Batı ABD, Doğu Asya, Kanada Orta, Kanada Doğu, Fransa Orta, Fransa Güney, Kore Orta, Kore Güney, UK Güney, UK Batı, Avustralya Doğu, Avustralya Güney Doğu, Kuzey Avrupa, Batı ABD, Batı ABD2, Orta ABD, Güney Doğu Asya, Orta Kuzey ABD, Orta Güney ABD, Japonya Doğu, Japonya Batı, Hindistan Güney, Hindistan Orta, Hindistan Batı, Doğu ABD 2 , İsviçre Kuzey, İsviçre Batı, Norveç Batı, Norveç Doğu ve tüm ulusal bölgeler.

## <a name="soft-delete-for-vms-using-azure-portal"></a>Azure portal kullanarak VM 'Ler için geçici silme

1. Bir sanal makinenin yedekleme verilerini silmek için yedeklemenin durdurulması gerekir. Azure portal, kurtarma hizmetleri kasanıza gidin, Yedekleme öğesine sağ tıklayın ve **Yedeklemeyi Durdur**' u seçin.

   ![Azure portal yedekleme öğelerinin ekran görüntüsü](./media/backup-azure-security-feature-cloud/backup-stopped.png)

2. Aşağıdaki pencerede, yedekleme verilerini silme veya tutma seçeneği sunulur. **Yedekleme verilerini sil** ' i ve sonra **Yedeklemeyi Durdur**' u seçerseniz, VM yedeklemesi kalıcı olarak silinmez. Bunun yerine, yedekleme verileri geçici olarak silinen durumunda 14 gün boyunca tutulacaktır. **Yedekleme verilerini sil** seçilirse, yapılandırılan e-posta kimliğine bir silme e-postası uyarısı gönderilir. Bu, kullanıcıya 14 gün boyunca yedekleme verileri için genişletilmiş saklama süresi kaldığını bildirir. Ayrıca, 12. güne bir e-posta uyarısı gönderilir ve bu, silinen verileri yeniden gösteren iki gün daha olduğunu bildirir. Silme işlemi 15. güne kadar ertelenir ve kalıcı silme gerçekleşir ve verilerin kalıcı olarak silinmesini bildiren son bir e-posta uyarısı gönderilir.

   ![Azure portal ekran görüntüsü, yedekleme ekranını durdur](./media/backup-azure-security-feature-cloud/delete-backup-data.png)

3. Bu 14 gün boyunca, kurtarma hizmetleri kasasında, geçici olarak silinen VM bunun yanında kırmızı bir "geçici silme" simgesiyle görüntülenir.

   ![Azure portal ekran görüntüsü, geçici silme durumunda VM](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

   > [!NOTE]
   > Kasada geçici olarak silinen yedekleme öğeleri varsa, kasa bu anda silinemez. Lütfen yedekleme öğeleri kalıcı olarak silindikten sonra kasa silme işlemini deneyin ve kasada geçici olarak silinmiş durumda bir öğe yok.

4. Geçici olarak silinen VM 'yi geri yüklemek için önce silinmesi gerekir. Silmeyi geri almak için, geçici olarak silinen VM 'yi seçin ve sonra **silmeyi geri al**seçeneğini belirleyin.

   ![Azure portal ekran görüntüsü, geri alma VM](./media/backup-azure-security-feature-cloud/choose-undelete.png)

   Silmeyi geri alma seçilirse bir pencere görünür ve geri yükleme işlemi gerçekleştirmek için sanal makinenin tüm geri yükleme noktalarının silinmesi ve kullanılabilir olduğunu gösterir. Yedeklemeler duraklatıldıktan sonra yedekleme verileri sürekli tutulur ve yedekleme ilkesi etkin olmadan yedeklenmez.

   ![Azure portal ekran görüntüsü, silmeyi geri almayı Onayla](./media/backup-azure-security-feature-cloud/undelete-vm.png)

   Bu noktada VM 'yi, seçilen geri yükleme noktasından **VM 'Yi geri yükle** ' yi seçerek de geri yükleyebilirsiniz.  

   ![Azure portal, VM 'yi geri yükle seçeneğinin ekran görüntüsü](./media/backup-azure-security-feature-cloud/restore-vm.png)

   > [!NOTE]
   > Çöp toplayıcı, yalnızca Kullanıcı **yedeklemeyi sürdürür** işlemini gerçekleştirdikten sonra, zaman aşımına uğradı kurtarma noktalarını çalıştırır ve temizler.

5. Silmeyi geri alma işlemi tamamlandıktan sonra, durum "verileri koruyacak şekilde Yedeklemeyi Durdur" ' a döner ve sonra **yedeklemeyi devam et**' i seçebilirsiniz. **Yedeklemeyi yeniden başlatma** işlemi, yedekleme ve bekletme zamanlamalarını tanımlayan Kullanıcı tarafından seçilen bir yedekleme ilkesiyle ilişkili olan yedekleme öğesini etkin duruma getirir.

   ![Azure portal ekran görüntüsü, yedeklemeyi sürdürür seçeneği](./media/backup-azure-security-feature-cloud/resume-backup.png)

## <a name="soft-delete-for-vms-using-azure-powershell"></a>Azure PowerShell kullanarak VM 'Ler için geçici silme

> [!IMPORTANT]
> Azure PS kullanarak geçici silme kullanmak için gereken az. RecoveryServices sürümü min 2.2.0. ```Install-Module -Name Az.RecoveryServices -Force```En son sürümü almak için kullanın.

Azure portal için yukarıda özetlenen şekilde, adımların sırası Azure PowerShell kullanılırken de aynıdır.

### <a name="delete-the-backup-item-using-azure-powershell"></a>Azure PowerShell kullanarak yedekleme öğesini silme

[Devre dışı bırakma-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection) PS cmdlet 'ini kullanarak yedekleme öğesini silin.

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

Yedekleme öğesinin ' DeleteState ' öğesi ' NotDeleted ' iken ' ToBeDeleted ' olarak değiştirilecek. Yedekleme verileri 14 gün boyunca tutulacaktır. Silme işlemini geri almak isterseniz geri alma-silme gerçekleştirilmelidir.

### <a name="undoing-the-deletion-operation-using-azure-powershell"></a>Azure PowerShell kullanarak silme işlemini geri alma

İlk olarak, geçici silme durumundaki (yani, silinecek) ilgili yedekleme öğesini getirin.

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

Ardından, [Undo-Azrecoveryservicesbackupıtemsilinmeye](/powershell/module/az.recoveryservices/undo-azrecoveryservicesbackupitemdeletion) PS cmdlet 'ini kullanarak geri alma silme işlemini gerçekleştirin.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Yedekleme öğesinin ' DeleteState ' öğesi ' NotDeleted ' olarak döndürülecek. Ancak koruma hala durdurulmuş. Korumayı yeniden etkinleştirmek için [yedeklemeyi sürdürür](./backup-azure-vms-automation.md#change-policy-for-backup-items) .

## <a name="soft-delete-for-vms-using-rest-api"></a>REST API kullanarak VM 'Ler için geçici silme

- [Burada](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data)belirtildiği gibi REST API kullanarak yedekleri silin.
- Kullanıcı bu silme işlemlerini geri almayı istiyorsa, [burada](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data)bahsedilen adımlara bakın.

## <a name="how-to-disable-soft-delete"></a>Geçici silme devre dışı bırakma

Bu özelliğin devre dışı bırakılması önerilmez. Geçici silme işleminin devre dışı bırakılmasını göz önünde bulundurmanız gereken tek durumlar, korumalı öğelerinizi yeni bir kasaya taşımayı planlıyorsanız ve silme ve yeniden koruma (örneğin, bir test ortamında) için gereken 14 gün bekleyemez. Geçici silmeyi devre dışı bırakma hakkında yönergeler için bkz. [geçici silme etkinleştirme ve devre dışı bırakma](backup-azure-security-feature-cloud.md#enabling-and-disabling-soft-delete).

## <a name="next-steps"></a>Sonraki adımlar

- Geçici silme hakkında [sık sorulan soruları](backup-azure-security-feature-cloud.md#frequently-asked-questions) okuyun
- [Azure Backup tüm güvenlik özellikleri](security-overview.md) hakkında bilgi edinin
