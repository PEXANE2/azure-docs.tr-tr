---
title: Bulut iş yüklerini korumaya yardımcı olan güvenlik özellikleri
description: Yedeklemeleri daha güvenli hale getirmek için Azure Backup güvenlik özelliklerini kullanmayı öğrenin.
ms.topic: conceptual
ms.date: 09/13/2019
ms.openlocfilehash: bd7c86e18114513a264a0f9252589533fb7ff2d3
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80668732"
---
# <a name="security-features-to-help-protect-cloud-workloads-that-use-azure-backup"></a>Azure Backup kullanan bulut iş yüklerini korumanıza yardımcı olacak güvenlik özellikleri

Kötü amaçlı yazılım, fidye ve yetkisiz erişim gibi güvenlik sorunları hakkında sorunlar artıyor. Bu güvenlik sorunları hem para hem de veri bakımından maliyetli olabilir. Bu tür saldırılara karşı koruma için Azure Backup artık, silme işleminden sonra bile yedekleme verilerini korumaya yardımcı olacak güvenlik özellikleri sağlamaktadır.

Bu tür bir özellik, geçici bir DELETE. Geçici bir aktör bir VM 'nin yedeklemesini silse (veya yedekleme verileri yanlışlıkla silinse), yedekleme verileri 14 ek gün boyunca tutulur ve bu yedekleme öğesinin veri kaybı olmadan kurtarılmasını sağlar. "Geçici silme" durumundaki yedekleme verilerinin ek 14 gün bekletmesi müşteriye hiçbir ücret vermez. Azure, verilerinizin güvenliğini sağlamak için [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) kullanarak bekleyen tüm yedeklenen verileri de şifreler.

Azure sanal makineleri için geçici silme koruması genel kullanıma sunulmuştur.

>[!NOTE]
>Azure VM 'de SQL Server için geçici silme ve Azure VM iş yükleri SAP HANA için geçici silme, artık önizleme aşamasında kullanıma sunuldu.<br>
>Önizlemeye kaydolmak için, şurada bize yazın:AskAzureBackupTeam@microsoft.com

## <a name="soft-delete"></a>Geçici silme

### <a name="soft-delete-for-vms"></a>VM 'Ler için geçici silme

VM 'Ler için geçici silme, sanal makinelerinizin yedeklerini istenmeden silinmeye karşı korur. Yedeklemeler silindikten sonra bile, 14 ek gün boyunca geçici silme durumunda saklanır.

> [!NOTE]
> Geçici silme yalnızca silinen yedekleme verilerini korur. Bir VM, yedekleme olmadan silinirse, geçici silme özelliği verileri korumaz. Tam esnekliği sağlamak için tüm kaynakların Azure Backup korumalı olması gerekir.
>

### <a name="supported-regions"></a>Desteklenen bölgeler

Geçici silme Şu anda Orta Batı ABD, Doğu Asya, Kanada Orta, Kanada Doğu, Fransa Orta, Fransa Güney, Kore Orta, Kore Güney, UK Güney, UK Batı, Avustralya Doğu, Avustralya Güney Doğu, Kuzey Avrupa, Batı ABD, Batı ABD2, Orta ABD, Güney Doğu Asya, Orta Kuzey ABD, Orta Güney ABD, Japonya Doğu, Japonya Batı, Hindistan Güney, Hindistan Orta, Hindistan Batı, Doğu ABD 2 , İsviçre Kuzey, İsviçre Batı, Norveç Batı, Norveç Doğu ve tüm ulusal bölgeler.

### <a name="soft-delete-for-vms-using-azure-portal"></a>Azure portal kullanarak VM 'Ler için geçici silme

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

Bu akış grafiği, geçici silme etkinleştirildiğinde bir yedekleme öğesinin farklı adımlarını ve durumlarını gösterir:

![Geçici olarak silinen yedekleme öğesinin yaşam döngüsü](./media/backup-azure-security-feature-cloud/lifecycle.png)

Daha fazla bilgi için aşağıdaki [sık sorulan sorular](backup-azure-security-feature-cloud.md#frequently-asked-questions) bölümüne bakın.

### <a name="soft-delete-for-vms-using-azure-powershell"></a>Azure PowerShell kullanarak VM 'Ler için geçici silme

> [!IMPORTANT]
> Azure PS kullanarak geçici silme kullanmak için gereken az. RecoveryServices sürümü min 2.2.0. En ```Install-Module -Name Az.RecoveryServices -Force``` son sürümü almak için kullanın.

Azure portal için yukarıda özetlenen şekilde, adımların sırası Azure PowerShell kullanılırken de aynıdır.

#### <a name="delete-the-backup-item-using-azure-powershell"></a>Azure PowerShell kullanarak yedekleme öğesini silme

[Devre dışı bırakma-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) PS cmdlet 'ini kullanarak yedekleme öğesini silin.

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

Yedekleme öğesinin ' DeleteState ' öğesi ' NotDeleted ' iken ' ToBeDeleted ' olarak değiştirilecek. Yedekleme verileri 14 gün boyunca tutulacaktır. Silme işlemini geri almak isterseniz geri alma-silme gerçekleştirilmelidir.

#### <a name="undoing-the-deletion-operation-using-azure-powershell"></a>Azure PowerShell kullanarak silme işlemini geri alma

İlk olarak, geçici silme durumundaki (yani, silinecek) ilgili yedekleme öğesini getirin.

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

Ardından, [Undo-Azrecoveryservicesbackupıtemsilinmeye](https://docs.microsoft.com/powershell/module/az.recoveryservices/undo-azrecoveryservicesbackupitemdeletion?view=azps-3.1.0) PS cmdlet 'ini kullanarak geri alma silme işlemini gerçekleştirin.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Yedekleme öğesinin ' DeleteState ' öğesi ' NotDeleted ' olarak döndürülecek. Ancak koruma hala durdurulmuş. Korumayı yeniden etkinleştirmek için [yedeklemeyi sürdürür](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#change-policy-for-backup-items) .

### <a name="soft-delete-for-vms-using-rest-api"></a>REST API kullanarak VM 'Ler için geçici silme

- [Burada](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data)belirtildiği gibi REST API kullanarak yedekleri silin.
- Kullanıcı bu silme işlemlerini geri almayı istiyorsa, [burada](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data)bahsedilen adımlara bakın.

## <a name="disabling-soft-delete"></a>Geçici silme devre dışı bırakılıyor

Geçici silme, yeni oluşturulan kasaların yanlışlıkla veya kötü amaçlı silmeleri arasında yedekleme verilerini korumak için varsayılan olarak etkindir.  Bu özelliğin devre dışı bırakılması önerilmez. Geçici silme işleminin devre dışı bırakılmasını göz önünde bulundurmanız gereken tek durumlar, korumalı öğelerinizi yeni bir kasaya taşımayı planlıyorsanız ve silme ve yeniden koruma (örneğin, bir test ortamında) için gereken 14 gün bekleyemez. Yalnızca kasa sahibi bu özelliği devre dışı bırakabilir. Bu özelliği devre dışı bırakırsanız, korunan öğelerin gelecekteki tüm silmeleri, geri yükleme özelliği olmadan hemen kaldırılmasına neden olur. Bu özelliği devre dışı bırakmadan önce geçici olarak silinen durumunda bulunan yedekleme verileri, 14 günlük süre boyunca geçici olarak silinmiş durumda kalır. Bunları hemen kalıcı olarak silmek isterseniz, kalıcı olarak silinmesi için silmeyi geri almanız ve silmeniz gerekir.

### <a name="disabling-soft-delete-using-azure-portal"></a>Azure portal kullanarak geçici silme devre dışı bırakılıyor

Geçici silme devre dışı bırakmak için şu adımları izleyin:

1. Azure Portal, kasanıza gidin ve **Ayarlar** -> **Özellikler**' e gidin.
2. Özellikler bölmesinde **güvenlik ayarları** -> **güncelleştirme**' yi seçin.  
3. Güvenlik ayarları bölmesinde, **geçici silme**altında **devre dışı bırak**' ı seçin.

![Geçici silmeyi devre dışı bırak](./media/backup-azure-security-feature-cloud/disable-soft-delete.png)

### <a name="disabling-soft-delete-using-azure-powershell"></a>Azure PowerShell kullanarak geçici silme devre dışı bırakılıyor

> [!IMPORTANT]
> Azure PS kullanarak geçici silme kullanmak için gereken az. RecoveryServices sürümü min 2.2.0. En ```Install-Module -Name Az.RecoveryServices -Force``` son sürümü almak için kullanın.

Devre dışı bırakmak için [set-AzRecoveryServicesVaultBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty?view=azps-3.1.0) PS cmdlet 'ini kullanın.

```powershell
Set-AzRecoveryServicesVaultProperty -VaultId $myVaultID -SoftDeleteFeatureState Disable


StorageModelType       :
StorageType            :
StorageTypeState       :
EnhancedSecurityState  : Enabled
SoftDeleteFeatureState : Disabled
```

### <a name="disabling-soft-delete-using-rest-api"></a>REST API kullanarak geçici silme devre dışı bırakılıyor

REST API kullanarak geçici silme işlevini devre dışı bırakmak için [burada](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api)bahsedilen adımlara bakın.

## <a name="permanently-deleting-soft-deleted-backup-items"></a>Geçici olarak silinen yedekleme öğelerini kalıcı olarak silme

Bu özelliğin devre dışı bırakılmasından önce geçici olarak silinen durumda verileri yedekleyin, geçici olarak silinmiş durumda kalır. Bunları hemen kalıcı olarak silmek istiyorsanız silmeyi geri alın ve kalıcı olarak silmek için yeniden silin.

### <a name="using-azure-portal"></a>Azure portalını kullanma

Şu adımları uygulayın:

1. [Geçici silme özelliğini devre dışı bırakmak](#disabling-soft-delete)için adımları izleyin.
2. Azure portal kasanıza gidin, **yedekleme öğeleri**' ne gidin ve geçici olarak silinen VM 'yi seçin.

   ![Geçici olarak silinen VM 'yi seçin](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

3. **Silmeyi geri al**seçeneğini belirleyin.

   ![Silmeyi geri al seçeneğini belirleyin](./media/backup-azure-security-feature-cloud/choose-undelete.png)

4. Bir pencere görüntülenir. **Silmeyi geri al**seçeneğini belirleyin.

   ![Silmeyi geri al seçeneğini belirleyin](./media/backup-azure-security-feature-cloud/undelete-vm.png)

5. Yedekleme verilerini kalıcı olarak silmek için **yedekleme verilerini sil** ' i seçin.

   ![Yedekleme verilerini Sil ' i seçin](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-buttom.png)

6. Kurtarma noktalarını silmek istediğinizi onaylamak için yedekleme öğesinin adını yazın.

   ![Yedekleme öğesinin adını yazın](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-data1.png)

7. Öğe için yedekleme verilerini silmek için **Sil**' i seçin. Bir bildirim iletisi, yedekleme verilerinin silindiğini bilmenizi sağlar.

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

Geçici silme devre dışı bırakıldıktan sonra öğeler silinmişse, bunlar geçici olarak silinmiş durumda olur. Bunları hemen silmek için silme işleminin ters alınması ve sonra yeniden gerçekleştirilmesi gerekir.

Geçici olarak silinmiş durumdaki öğeleri belirler.

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

Ardından, geçici silme etkinken gerçekleştirilen silme işlemini tersine çevirin.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Geçici silme devre dışı olduğundan, silme işlemi yedekleme verilerinin anında kaldırılmasına neden olur.

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

### <a name="using-rest-api"></a>REST API kullanma

Geçici silme devre dışı bırakıldıktan sonra öğeler silinmişse, bunlar geçici olarak silinmiş durumda olur. Bunları hemen silmek için silme işleminin ters alınması ve sonra yeniden gerçekleştirilmesi gerekir.

1. İlk olarak, [burada](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data)bahsedilen adımlarla silme işlemlerini geri alın.
2. Ardından, [burada](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api)bahsedilen adımları kullanarak REST API kullanarak geçici silme işlevini devre dışı bırakın.
3. Ardından, [burada](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data)belirtildiği gibi REST API kullanarak yedeklemeleri silin.

## <a name="encryption"></a>Şifreleme

Tüm yedeklenen verileriniz, Azure depolama şifrelemesi kullanılarak bulutta depolandığında, güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olacak şekilde otomatik olarak şifrelenir. Bekleyen bu veriler 256 bit AES şifrelemesi kullanılarak şifrelenir, en güçlü blok şifrelemeleri kullanılabilir ve FIPS 140-2 uyumludur.

Bekleyen şifrelemeye ek olarak, yoldaki tüm yedekleme verileriniz HTTPS üzerinden aktarılır. Her zaman Azure omurga ağında kalır.

Daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption). Şifreleme hakkında karşılaşabileceğiniz soruları yanıtlamak için [Azure Backup SSS](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#encryption) bölümüne bakın.

### <a name="encryption-of-backup-data-using-platform-managed-keys"></a>Platform tarafından yönetilen anahtarlar kullanılarak yedekleme verilerinin şifrelenmesi

Varsayılan olarak, tüm verileriniz platform tarafından yönetilen anahtarlar kullanılarak şifrelenir. Bu şifrelemeyi etkinleştirmek için herhangi bir açık işlem yapmanız gerekmez ve kurtarma hizmetleri kasanıza yedeklenen tüm iş yükleri için geçerlidir.

### <a name="encryption-of-backup-data-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinin şifrelenmesi

Azure sanal makinelerinizi yedeklerken artık size ait olan ve yönetilen anahtarları kullanarak verilerinizi şifreleyebilirsiniz. Azure Backup, yedeklemelerinizi şifrelemek için Azure Key Vault depolanan RSA anahtarlarınızı kullanmanıza olanak sağlar. Yedeklemeleri şifrelemek için kullanılan şifreleme anahtarı, kaynak için kullanılan birinden farklı olabilir. Veriler, sırasıyla anahtarlarınız kullanılarak korunan bir AES 256 tabanlı veri şifreleme anahtarı (DEK) kullanılarak korunur. Bu sayede veriler ve anahtarlar üzerinde tam denetim elde edersiniz. Şifrelemeye izin vermek için, kurtarma hizmetleri kasasının Azure Key Vault şifreleme anahtarına erişim verilmesi gerekir. Her gerektiğinde anahtarı devre dışı bırakabilir veya erişimi iptal edebilirsiniz. Ancak, kasadaki herhangi bir öğeyi korumayı denemeden önce anahtarlarınızı kullanarak şifrelemeyi etkinleştirmeniz gerekir.

>[!NOTE]
>Bu özellik şu anda sınırlı kullanılabilirliğe sahip. Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinizi şifrelemek istiyorsanız AskAzureBackupTeam@microsoft.com lütfen [Bu anketi](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapURE9TTDRIUEUyNFhNT1lZS1BNVDdZVllHWi4u) ve e-postayı doldurun. Bu özelliği kullanma yeteneğinin Azure Backup hizmetten onaya tabi olduğunu unutmayın.

### <a name="backup-of-managed-disk-vms-encrypted-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmiş yönetilen disk VM 'lerinin yedeklemesi

Azure Backup Ayrıca, sunucu tarafı şifreleme için anahtarınızı kullanan Azure VM 'lerinizi yedeklemenizi sağlar. Diskleri şifrelemek için kullanılan anahtar Azure Key Vault depolanır ve sizin tarafınızdan yönetilir. Müşteri tarafından yönetilen anahtarları kullanan sunucu tarafı şifreleme, Azure disk şifrelemesi 'nden farklıdır, çünkü ADE, Konuk içi şifrelemeyi gerçekleştirmek için BitLocker (Windows için) ve DM-crypt (Linux için) için, SSE depolama hizmetindeki verileri şifreler ve VM 'niz için herhangi bir işletim sistemi veya görüntü kullanmanıza olanak sağlar. Daha fazla ayrıntı için [, müşteri tarafından yönetilen anahtarlarla yönetilen disklerin şifrelenmesi](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys) bölümüne bakın.

### <a name="backup-of-vms-encrypted-using-ade"></a>ADE kullanılarak şifrelenen VM 'lerin yedeklenmesi

Azure Backup ile, işletim sistemi veya veri disklerinin Azure disk şifrelemesi kullanılarak şifrelendiği Azure sanal makinelerinizi da yedekleyebilirsiniz. ADE, Konuk içi şifrelemeyi gerçekleştirmek için Windows VM 'Leri için BitLocker ve Linux sanal makineleri için DM-Crypt kullanır. Ayrıntılar için bkz. [Azure Backup ile şifrelenmiş sanal makineleri yedekleme ve geri yükleme](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

## <a name="private-endpoints"></a>Özel uç noktalar

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

## <a name="other-security-features"></a>Diğer güvenlik özellikleri

### <a name="protection-of-azure-backup-recovery-points"></a>Azure Backup kurtarma noktalarının korunması

Kurtarma Hizmetleri kasaları tarafından kullanılan depolama hesapları yalıtılmış ve herhangi bir kötü amaçlı kullanıcı tarafından erişilemez. Erişime yalnızca geri yükleme gibi Azure Backup yönetim işlemleri aracılığıyla izin verilir. Bu yönetim işlemleri rol tabanlı Access Control (RBAC) aracılığıyla denetlenir.

Daha fazla bilgi için bkz. [Azure Backup kurtarma noktalarını yönetmek Için rol tabanlı Access Control kullanma](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="for-soft-delete"></a>Geçici silme için

#### <a name="do-i-need-to-enable-the-soft-delete-feature-on-every-vault"></a>Her kasada geçici silme özelliğini etkinleştirmem gerekir mi?

Hayır, tüm kurtarma hizmetleri kasaları için varsayılan olarak oluşturulur ve etkinleştirilir.

#### <a name="can-i-configure-the-number-of-days-for-which-my-data-will-be-retained-in-soft-deleted-state-after-delete-operation-is-complete"></a>Silme işlemi tamamlandıktan sonra verileri geçici olarak silinen durumda tutulacak gün sayısını yapılandırabilir miyim?

Hayır, silme işleminden sonra 14 gün daha fazla bekletme için düzeltildi.

#### <a name="do-i-need-to-pay-the-cost-for-this-additional-14-day-retention"></a>Bu ek 14 günlük bekletme için maliyeti ödemem gerekiyor mu?

Hayır, bu 14 günlük ek bekletme, geçici silme işlevselliğinin bir parçası olarak ücretsiz maliyetlidir.

#### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-delete-state"></a>Verilerim geçici silme durumunda olduğunda geri yükleme işlemi gerçekleştirebilir miyim?

Hayır, geri yüklemek için geçici olarak silinen kaynağı geri almanız gerekir. Silmeyi geri alma işlemi, kaynağı, zaman içinde herhangi bir noktaya geri yüklediğiniz **verileri koruyun durumuyla yeniden durdur** 'a geri gönderir. Çöp toplayıcı bu durumda duraklatılmaya devam eder.

#### <a name="will-my-snapshots-follow-the-same-lifecycle-as-my-recovery-points-in-the-vault"></a>Anlık görüntülerim kasadaki kurtarma noktalarım ile aynı yaşam döngüsünü izlesin mi?

Evet.

#### <a name="how-can-i-trigger-the-scheduled-backups-again-for-a-soft-deleted-resource"></a>Zamanlanmış yedeklemeleri, geçici olarak silinen bir kaynak için yeniden nasıl tetikleyebilirim?

Geri alma işleminden sonra devam ederseniz kaynak yeniden korunur. Özgeçmişi işlemi, zamanlanan yedeklemeleri seçilen bekletme süresiyle tetiklemek için bir yedekleme ilkesini ilişkilendirir. Ayrıca, çöp toplayıcı, işlem tamamlandıktan hemen sonra çalışır. Bitiş tarihini aşan bir kurtarma noktasından geri yükleme gerçekleştirmek istiyorsanız, bu işlemi, sürdürülmesi işlemini tetiklemeden önce yapmanız önerilir.

#### <a name="can-i-delete-my-vault-if-there-are-soft-deleted-items-in-the-vault"></a>Kasada geçici olarak silinen öğeler varsa kasamı silebilir miyim?

Kasadaki geçici olarak silinen durumda yedekleme öğeleri varsa kurtarma hizmetleri Kasası silinemez. Geçici olarak silinen öğeler, silme işleminden sonra 14 gün sonra kalıcı olarak silinir. 14 gün bekleyemez, [geçici silme işlemini devre dışı bırakın](#disabling-soft-delete), geçici olarak silinen öğeleri geri alın ve kalıcı olarak silmek için yeniden silin. Korunan öğe olmadığından ve geçici olarak silinen öğeler olmadığından, kasa silinebilir.  

#### <a name="can-i-delete-the-data-earlier-than-the-14-days-soft-delete-period-after-deletion"></a>Silme işleminden sonra 14 gün geçici silme süresinden önceki verileri silebilir miyim?

Hayır. Geçici olarak silinen öğeleri silmeye zorleyemiyorum, bunlar 14 gün sonra otomatik olarak silinir. Bu güvenlik özelliği, yedeklenen verilerin yanlışlıkla veya kötü amaçlı silmelerden korunmasını sağlamak için etkinleştirilir.  VM üzerinde başka bir işlem gerçekleştirmeden önce 14 gün beklemeniz gerekir.  Geçici olarak silinen öğelerin ücretlendirilildiği.  Yeni bir kasaya 14 gün içinde geçici silme için işaretlenen VM 'Lerin yeniden korunmasını istiyorsanız Microsoft desteği 'ne başvurun.

#### <a name="can-soft-delete-operations-be-performed-in-powershell-or-cli"></a>PowerShell veya CLı 'de geçici silme işlemleri yapılabilir mi?

Geçici silme işlemleri, [PowerShell](#soft-delete-for-vms-using-azure-powershell)kullanılarak gerçekleştirilebilir. Şu anda CLı desteklenmez.

#### <a name="is-soft-delete-supported-for-other-cloud-workloads-like-sql-server-in-azure-vms-and-sap-hana-in-azure-vms"></a>Azure VM 'lerinde SQL Server ve Azure VM 'lerinde SAP HANA gibi diğer bulut iş yükleri için de geçici silme destekleniyor mu?

Hayır. Şu anda geçici silme yalnızca Azure sanal makinelerinde desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Backup Için güvenlik denetimleri](backup-security-controls.md)hakkında bilgi edinin.
