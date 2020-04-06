---
title: Bulut iş yüklerinin korunmasına yardımcı olacak güvenlik özellikleri
description: Yedeklemeleri daha güvenli hale getirmek için Azure Yedekleme'de güvenlik özelliklerini nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 09/13/2019
ms.openlocfilehash: bd7c86e18114513a264a0f9252589533fb7ff2d3
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668732"
---
# <a name="security-features-to-help-protect-cloud-workloads-that-use-azure-backup"></a>Azure Yedekleme kullanan bulut iş yüklerinin korunmasına yardımcı olacak güvenlik özellikleri

Kötü amaçlı yazılımlar, fidye yazılımları ve izinsiz giriş gibi güvenlik sorunlarıyla ilgili endişeler artmaktadır. Bu güvenlik sorunları hem para hem de veri açısından pahalıya mal olabilir. Azure Yedekleme, bu tür saldırılara karşı korunmak için artık silme işleminden sonra bile yedekleme verilerinin korunmasına yardımcı olacak güvenlik özellikleri sağlar.

Böyle bir özellik yumuşak silme olduğunu. Kötü niyetli bir aktör bir VM'nin yedeklemesini silse bile (veya yedekleme verileri yanlışlıkla silinse bile) yumuşak silme ile yedekleme verileri 14 gün daha saklanır ve bu da yedekleme öğesinin veri kaybı olmadan kurtarılmasına olanak sağlar. Yedekleme verilerinin "yumuşak silme" durumunda ki ek 14 gün tutulması, müşteriye herhangi bir maliyet ememez. Azure ayrıca, verilerinizi daha da güvenli hale getirmek için Depolama Hizmeti Şifrelemesi'ni kullanarak yedeklenen tüm verileri de şifreler. [Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)

Azure sanal makineleri için yumuşak silme koruması genellikle kullanılabilir.

>[!NOTE]
>Azure VM'deki SQL sunucusu için yumuşak silme ve Azure VM iş yüklerinde SAP HANA için yumuşak silme artık önizlemede kullanılabilir.<br>
>Önizleme için kaydolmak için, bize yazınAskAzureBackupTeam@microsoft.com

## <a name="soft-delete"></a>Yumuşak silme

### <a name="soft-delete-for-vms"></a>VM'ler için yumuşak silme

VM'ler için yumuşak silme, VM'lerinizin yedeklerini istenmeyen silmeye karşı korur. Yedeklemeler silindikten sonra bile, 14 gün boyunca yumuşak silme durumunda saklanır.

> [!NOTE]
> Yumuşak silme yalnızca silinmiş yedekleme verilerini korur. Bir VM yedekleme olmadan silinirse, yumuşak silme özelliği verileri korumaz. Tam esneklik sağlamak için tüm kaynaklar Azure Yedekleme ile korunmalıdır.
>

### <a name="supported-regions"></a>Desteklenen bölgeler

Yumuşak silme şu anda Batı Orta ABD, Doğu Asya, Kanada Orta, Kanada Doğu, Fransa Orta, Fransa Güney, Kore Orta, Kore Güney, İngiltere Güney, İngiltere Batı, Avustralya Doğu, Avustralya Güney Doğu, Kuzey Avrupa, Batı ABD, Batı US2, Orta ABD, Güney Doğu Asya, Kuzey Orta ABD, Güney Orta ABD, Japonya Doğu, Japonya Batı, Hindistan Güney, Hindistan Orta, Hindistan Batı, Doğu ABD 2 desteklenir , İsviçre Kuzey, İsviçre Batı, Norveç Batı, Norveç Doğu ve tüm Ulusal bölgeler.

### <a name="soft-delete-for-vms-using-azure-portal"></a>Azure portalını kullanan VM'ler için yumuşak silme

1. Bir VM'nin yedekleme verilerini silmek için yedeklemenin durdurulması gerekir. Azure portalında kurtarma hizmetleri kasanıza gidin, yedekleme öğesine sağ tıklayın ve **yedeklemeyi durdur'u**seçin.

   ![Azure portalı Yedekleme Öğeleri ekran görüntüsü](./media/backup-azure-security-feature-cloud/backup-stopped.png)

2. Aşağıdaki pencerede, yedekleme verilerini silme veya saklama seçeneği ne zaman verilir. **Yedekleme verilerini sil** ve sonra **yedeklemeyi durdur'u**seçerseniz, VM yedeklemekalıcı olarak silinmez. Bunun yerine, yedekleme verileri yumuşak silinmiş durumda 14 gün boyunca saklanır. **Yedekleme verilerini sil** seçilirse, yapılandırılan e-posta kimliğine kullanıcıya yedekleme verileri için 14 gün kalan uzatılmış bekletme süresinin kaldığını bildiren bir silme e-posta uyarısı gönderilir. Ayrıca, silinen verileri yeniden canlandırmak için iki gün daha kaldığını bildiren bir e-posta uyarısı 12. Silme işlemi, kalıcı silme işleminin gerçekleşeceği ve verilerin kalıcı olarak silinmesi hakkında bilgi veren son bir e-posta uyarısı gönderildiği 15.

   ![Azure portalının ekran görüntüsü, Yedeklemeyi Durdur ekranı](./media/backup-azure-security-feature-cloud/delete-backup-data.png)

3. Bu 14 gün boyunca, Kurtarma Hizmetleri Vault, yumuşak silinmiş VM yanında kırmızı bir "yumuşak silme" simgesi ile görünür.

   ![Azure portalının ekran görüntüsü, yumuşak silme durumunda VM](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

   > [!NOTE]
   > Kasada yumuşak silinmiş yedekleme öğeleri varsa, kasa o anda silinemez. Lütfen yedekleme öğeleri kalıcı olarak silindikten sonra kasa silme işlemini deneyin ve kasada yumuşak silinmiş durumda öğe kalmaz.

4. Yumuşak silinmiş VM'yi geri yüklemek için önce silinmemiş olması gerekir. Undelete için yumuşak silinmiş VM'yi seçin ve ardından **Undelete**seçeneğini seçin.

   ![Azure portalının ekran görüntüsü, Undelete VM](./media/backup-azure-security-feature-cloud/choose-undelete.png)

   Delete seçilirse, VM'nin tüm geri yükleme noktalarının silinip geri yükleme işlemi için kullanılabilir olacağı konusunda bir pencere görüntülenir. VM, yedeklemeler duraklatılmış ve yedekleme verileri sonsuza kadar yedek ilkesi etkin olmadan sonsuza kadar korunurken "saklama verilerini korumayı durdurun" durumunda tutulur.

   ![Azure portalının ekran görüntüsü, Delete VM'yi onaylayın](./media/backup-azure-security-feature-cloud/undelete-vm.png)

   Bu noktada, seçilen geri yükleme noktasından **VM'yi geri yükle'yi** seçerek VM'yi geri yükleyebilirsiniz.  

   ![Azure portalının ekran görüntüsü, VM'yi geri yükleme seçeneği](./media/backup-azure-security-feature-cloud/restore-vm.png)

   > [!NOTE]
   > Çöp toplayıcı, süresi dolan kurtarma noktalarını yalnızca kullanıcı **Özgeçmiş yedekleme** işlemini yaptıktan sonra çalıştırıp temizler.

5. Undelete işlemi tamamlandıktan sonra durum "Saklama verileriyle yedeklemeyi durdur" durumuna döner ve ardından **Yedeklemeye Devam'ı**seçebilirsiniz. **Devam yedekleme** işlemi, yedekleme ve bekletme zamanlamalarını tanımlayan kullanıcı tarafından seçilen bir yedekleme ilkesiyle ilişkili olan etkin durumdaki yedekleme öğesini geri getirir.

   ![Azure portalının ekran görüntüsü, Yedeklemeye Devam seçeneği](./media/backup-azure-security-feature-cloud/resume-backup.png)

Bu akış grafiği, Yumuşak Silme etkinleştirildiğinde yedekleme maddesinin farklı adımlarını ve durumlarını gösterir:

![Yumuşak silinmiş yedekleme öğesinin yaşam döngüsü](./media/backup-azure-security-feature-cloud/lifecycle.png)

Daha fazla bilgi için aşağıdaki [Sık Sorulan Sorular](backup-azure-security-feature-cloud.md#frequently-asked-questions) bölümüne bakın.

### <a name="soft-delete-for-vms-using-azure-powershell"></a>Azure PowerShell kullanan VM'ler için yumuşak silme

> [!IMPORTANT]
> Azure PS kullanarak yumuşak silme kullanmak için gereken Az.RecoveryServices sürümü min 2.2.0'dır. En ```Install-Module -Name Az.RecoveryServices -Force``` son sürümü almak için kullanın.

Azure portalı için yukarıda belirtildiği gibi, Azure PowerShell'i kullanırken de adım sırası aynıdır.

#### <a name="delete-the-backup-item-using-azure-powershell"></a>Azure PowerShell'i kullanarak yedekleme öğesini silme

[Disable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) PS cmdlet kullanarak yedek öğeyi silin.

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

Yedekleme öğesinin 'DeleteState' 'NotDeleted' 'ToBeDeleted' olarak değişecektir. Yedekleme verileri 14 gün boyunca saklanır. Silme işlemini geri almak istiyorsanız, geri alma-silme işlemi yapılmalıdır.

#### <a name="undoing-the-deletion-operation-using-azure-powershell"></a>Azure PowerShell kullanarak silme işlemini geri alma

İlk olarak, yumuşak silme durumundaolan ilgili yedekleme öğesini getirin (diğer bir deyişle, silinmek üzere).

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

Daha sonra, [Geri Alma-AzRecoveryServicesBackupItemDeletion](https://docs.microsoft.com/powershell/module/az.recoveryservices/undo-azrecoveryservicesbackupitemdeletion?view=azps-3.1.0) PS cmdlet kullanarak geri alma işlemini gerçekleştirin.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Yedekleme öğesinin 'DeleteState' 'NotDeleted' olarak geri döner. Ama koruma hala durduruldu. Korumayı yeniden etkinleştirmek için [yedeklemeye devam edin.](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#change-policy-for-backup-items)

### <a name="soft-delete-for-vms-using-rest-api"></a>REST API'yi kullanarak VM'ler için yumuşak silme

- [Burada](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data)belirtildiği gibi REST API kullanarak yedekleri silin.
- Kullanıcı bu silme işlemlerini geri almak isterse, [burada](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data)belirtilen adımlara bakın.

## <a name="disabling-soft-delete"></a>Yumuşak silmeyi devre dışı bırakma

Yedekleme verilerini yanlışlıkla veya kötü amaçlı silmelere karşı korumak için yeni oluşturulan kasalarda varsayılan olarak yumuşak silme etkinleştirilir.  Bu özelliğidevre dışı bırakmak önerilmez. Yumuşak silmeyi devre dışı bırakmayı düşünmeniz gereken tek durum, korumalı öğelerinizi yeni bir kasaya taşımayı planlıyorsanız ve silme ve yeniden koruma için gereken 14 günü bekleyemezseniz (örneğin, bir test ortamında.) Bu özelliği yalnızca kasa sahibi devre dışı edebilir. Bu özelliği devre dışı ederseniz, korumalı öğelerin gelecekteki tüm silme işlemleri, geri yükleme olanağı olmadan derhal kaldırılmasına neden olur. Bu özelliği devre dışı bırakmadan önce yumuşak silinmiş durumda bulunan yedekleme verileri, 14 gün boyunca silinmiş durumda kalır. Bunları hemen kalıcı olarak silmek istiyorsanız, kalıcı olarak silinmek için bunları silmeniz ve tekrar silmeniz gerekir.

### <a name="disabling-soft-delete-using-azure-portal"></a>Azure portalLarını kullanarak yumuşak silmeyi devre dışı bırakma

Yumuşak silmeyi devre dışı katmak için aşağıdaki adımları izleyin:

1. Azure portalında kasanıza gidin ve ardından **Ayarlar** -> **Özellikleri'ne**gidin.
2. Özellikler bölmesinde Güvenlik **Ayarları** -> **Güncelleştirmesi'ni**seçin.  
3. Güvenlik ayarları bölmesinde, **Yumuşak Sil'in**altında, **Devre dışı aç'ı**seçin.

![Yumuşak silmeyi devre dışı](./media/backup-azure-security-feature-cloud/disable-soft-delete.png)

### <a name="disabling-soft-delete-using-azure-powershell"></a>Azure PowerShell'i kullanarak yumuşak silmeyi devre dışı bırakma

> [!IMPORTANT]
> Azure PS kullanarak yumuşak silme kullanmak için gereken Az.RecoveryServices sürümü min 2.2.0'dır. En ```Install-Module -Name Az.RecoveryServices -Force``` son sürümü almak için kullanın.

Devre dışı bırakmak için [Set-AzRecoveryServicesVaultBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty?view=azps-3.1.0) PS cmdlet'i kullanın.

```powershell
Set-AzRecoveryServicesVaultProperty -VaultId $myVaultID -SoftDeleteFeatureState Disable


StorageModelType       :
StorageType            :
StorageTypeState       :
EnhancedSecurityState  : Enabled
SoftDeleteFeatureState : Disabled
```

### <a name="disabling-soft-delete-using-rest-api"></a>REST API'yi kullanarak yumuşak silmeyi devre dışı bırakma

REST API'yi kullanarak yumuşak silme işlevini devre dışı katmak için [burada](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api)belirtilen adımlara bakın.

## <a name="permanently-deleting-soft-deleted-backup-items"></a>Yumuşak silinmiş yedekleme öğelerini kalıcı olarak silme

Bu özelliği devre dışı bırakmadan önce yumuşak silinmiş durumdaki yedekleme verileri, silinmiş yumuşak durumda kalır. Bunları hemen kalıcı olarak silmek isterseniz, kalıcı olarak silinmek için bunları silin ve tekrar silin.

### <a name="using-azure-portal"></a>Azure portalını kullanma

Şu adımları uygulayın:

1. [Yumuşak silmeyi devre dışı bırakabilmek](#disabling-soft-delete)için adımları izleyin.
2. Azure portalında kasanıza gidin, **Yedekleme Öğeleri'ne**gidin ve yumuşak silinmiş VM'yi seçin.

   ![Yumuşak silinmiş VM'yi seçin](./media/backup-azure-security-feature-cloud/vm-soft-delete.png)

3. **Undelete**seçeneğini seçin.

   ![Undelete'i seçin](./media/backup-azure-security-feature-cloud/choose-undelete.png)

4. Bir pencere görüntülenir. **Undelete'i**seçin.

   ![Undelete'i seçin](./media/backup-azure-security-feature-cloud/undelete-vm.png)

5. Yedekleme verilerini kalıcı olarak silmek için **yedekleme verilerini sil'i** seçin.

   ![Yedekleme verilerini sil'i seçin](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-buttom.png)

6. Kurtarma noktalarını silmek istediğinizi doğrulamak için yedekleme öğesinin adını yazın.

   ![Yedekleme öğesinin adını yazın](https://docs.microsoft.com/azure/backup/media/backup-azure-manage-vms/delete-backup-data1.png)

7. Öğenin yedekleme verilerini silmek için **Sil'i**seçin. Bildirim iletisi, yedekleme verilerinin silindiğini bilmenizi sağlar.

### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

Öğeler yumuşak silme devre dışı bırakılmadan önce silinmişse, bunlar yumuşak silinmiş durumda olur. Bunları hemen silmek için silme işleminin tersine çevrilmesi ve sonra tekrar gerçekleştirilmesi gerekir.

Yumuşak silinmiş durumda olan öğeleri tanımlayın.

```powershell

Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID | Where-Object {$_.DeleteState -eq "ToBeDeleted"}

Name                                     ContainerType        ContainerUniqueName                      WorkloadType         ProtectionStatus     HealthStatus         DeleteState
----                                     -------------        -------------------                      ------------         ----------------     ------------         -----------
VM;iaasvmcontainerv2;selfhostrg;AppVM1    AzureVM             iaasvmcontainerv2;selfhostrg;AppVM1       AzureVM              Healthy              Passed               ToBeDeleted

$myBkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -VaultId $myVaultID -Name AppVM1
```

Ardından, yumuşak silme etkinleştirildiğinde gerçekleştirilen silme işlemini tersine çevirin.

```powershell
Undo-AzRecoveryServicesBackupItemDeletion -Item $myBKpItem -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           Undelete             Completed            12/5/2019 12:47:28 PM     12/5/2019 12:47:40 PM     65311982-3755-46b5-8e53-c82ea4f0d2a2
```

Yumuşak silme devre dışı bırakıldığından, silme işlemi yedekleme verilerinin hemen kaldırılmasına neden olur.

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $myBkpItem -RemoveRecoveryPoints -VaultId $myVaultID -Force

WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
AppVM1           DeleteBackupData     Completed            12/5/2019 12:44:15 PM     12/5/2019 12:44:50 PM     0488c3c2-accc-4a91-a1e0-fba09a67d2fb
```

### <a name="using-rest-api"></a>REST API kullanma

Öğeler yumuşak silme devre dışı bırakılmadan önce silinmişse, bunlar yumuşak silinmiş durumda olur. Bunları hemen silmek için silme işleminin tersine çevrilmesi ve sonra tekrar gerçekleştirilmesi gerekir.

1. İlk olarak, [burada](backup-azure-arm-userestapi-backupazurevms.md#undo-the-stop-protection-and-delete-data)belirtilen adımlarla silme işlemlerini geri al.
2. Daha sonra [burada](use-restapi-update-vault-properties.md#update-soft-delete-state-using-rest-api)belirtilen adımları kullanarak REST API kullanarak yumuşak silme işlevini devre dışı.
3. Daha sonra [burada](backup-azure-arm-userestapi-backupazurevms.md#stop-protection-and-delete-data)belirtildiği gibi REST API kullanarak yedekleri silin.

## <a name="encryption"></a>Şifreleme

Tüm yedekverileriniz, güvenlik ve uyumluluk taahhütlerinizi karşılamanıza yardımcı olan Azure Depolama şifrelemesi kullanılarak bulutta depolandığında otomatik olarak şifrelenir. Bu veriler, mevcut en güçlü blok şifrelerinden biri olan 256 bit AES şifrelemesi kullanılarak şifrelenir ve FIPS 140-2 uyumludur.

Şifrelemeye ek olarak, aktarımsırasındaki tüm yedekleme verileriniz HTTPS üzerinden aktarılır. Her zaman Azure omurga ağında kalır.

Daha fazla bilgi [için, veriler için Azure Depolama şifrelemesi'ne](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)bakın. Şifreleme yle ilgili sorularınızı yanıtlamak için [Azure Yedekleme SSS'sine](https://docs.microsoft.com/azure/backup/backup-azure-backup-faq#encryption) bakın.

### <a name="encryption-of-backup-data-using-platform-managed-keys"></a>Platform tarafından yönetilen anahtarları kullanarak yedekleme verilerinin şifrelemesi

Varsayılan olarak, tüm verileriniz platform tarafından yönetilen tuşlar kullanılarak şifrelenir. Bu şifrelemeyi etkinleştirmek için kendi ucunızdan herhangi bir açık eylemyapmanız gerekmez ve bu, Kurtarma Hizmetleri kasanıza yedeklenen tüm iş yükleri için geçerlidir.

### <a name="encryption-of-backup-data-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları kullanarak yedekleme verilerinin şifrelemesi

Azure Sanal Makinelerinizi yedeklerken, artık sahip olduğunuz ve yönettiğiniz anahtarları kullanarak verilerinizi şifreleyebilirsiniz. Azure Yedekleme, yedeklemelerinizi şifrelemek için Azure Key Vault'ta depolanan RSA anahtarlarınızı kullanmanıza olanak tanır. Yedeklemeleri şifrelemek için kullanılan şifreleme anahtarı, kaynak için kullanılananahtardan farklı olabilir. Veriler, sırayla anahtarlarınızı kullanarak korunan AES 256 tabanlı veri şifreleme anahtarı (DEK) kullanılarak korunur. Bu, veriler ve anahtarlar üzerinde tam kontrol sağlar. Şifrelemeye izin vermek için, Kurtarma Hizmetleri kasasına Azure Anahtar Kasası'ndaki şifreleme anahtarına erişim izni verilmesi gerekir. Anahtarı devre dışı layabilir veya gerektiğinde erişimi iptal edebilirsiniz. Ancak, kasadaki öğeleri korumaya çalışmadan önce anahtarlarınızı kullanarak şifrelemeyi etkinleştirmeniz gerekir.

>[!NOTE]
>Bu özellik şu anda sınırlı kullanılabilirlik tedir. Müşteri yönetilen [this survey](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0H3_nezt2RNkpBCUTbWEapURE9TTDRIUEUyNFhNT1lZS1BNVDdZVllHWi4u) anahtarları kullanarak yedekleme AskAzureBackupTeam@microsoft.com verilerinizi şifrelemek istiyorsanız lütfen bu anketi doldurun ve bize e-posta gönderin. Bu özelliği kullanma olanağının Azure Yedekleme hizmetinin onayına tabi olduğunu unutmayın.

### <a name="backup-of-managed-disk-vms-encrypted-using-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar kullanılarak şifrelenmiş yönetilen disk VM'lerinin yedeklemesi

Azure Yedekleme, sunucu tarafındaki şifreleme için anahtarınızı kullanan Azure VM'lerinizi yedeklemenize de olanak tanır. Diskleri şifrelemek için kullanılan anahtar Azure Anahtar Kasası'nda depolanır ve sizin yeriniz tarafından yönetilir. Müşteri tarafından yönetilen anahtarları kullanan sunucu tarafı şifrelemesi Azure Disk Şifreleme'den farklıdır, çünkü ADE, BitLocker (Windows için) ve DM-Crypt'i (Linux için) konuk içi şifreleme gerçekleştirmek için kullanır, SSE depolama hizmetindeki verileri şifreler ve Sanal İletileriniz için herhangi bir işletim sistemi veya resim kullanmanızı sağlar. Daha fazla ayrıntı için [müşteri yönetilen anahtarlarla yönetilen disklerin Şifrelemesine](https://docs.microsoft.com/azure/virtual-machines/windows/disk-encryption#customer-managed-keys) bakın.

### <a name="backup-of-vms-encrypted-using-ade"></a>ADE kullanılarak şifrelenmiş VM'lerin yedeklemesi

Azure Yedekleme ile, işletim sistemi veya veri diskleri Azure Disk Şifrelemesi kullanılarak şifrelenmiş olan Azure Sanal makinelerinizi de yedekleyebilirsiniz. ADE, konuk şifreleme yapmak için Windows VM'ler için BitLocker ve Linux VM'ler için DM-Crypt kullanır. Ayrıntılar için Azure [Yedekleme ile gelişmiş sanal makineleri yedekleyin ve geri yükleyin.](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption)

## <a name="private-endpoints"></a>Özel Uç Noktalar

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

## <a name="other-security-features"></a>Diğer güvenlik özellikleri

### <a name="protection-of-azure-backup-recovery-points"></a>Azure Yedekleme kurtarma noktalarının korunması

Kurtarma hizmetleri kasaları tarafından kullanılan depolama hesapları yalıtılır ve kullanıcılar tarafından kötü amaçlı olarak erişilemez. Erişime yalnızca geri yükleme gibi Azure Yedekleme yönetimi işlemleri aracılığıyla izin verilir. Bu yönetim işlemleri Rol Tabanlı Erişim Denetimi (RBAC) aracılığıyla denetlenir.

Daha fazla bilgi için Azure [Yedekleme kurtarma noktalarını yönetmek için Rol Tabanlı Erişim Denetimini Kullan'a](https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault)bakın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="for-soft-delete"></a>Yumuşak silme için

#### <a name="do-i-need-to-enable-the-soft-delete-feature-on-every-vault"></a>Her kasadaki yumuşak silme özelliğini etkinleştirmem gerekiyor mu?

Hayır, tüm kurtarma hizmetleri kasaları için varsayılan olarak oluşturulmuş ve etkinleştirilmiştir.

#### <a name="can-i-configure-the-number-of-days-for-which-my-data-will-be-retained-in-soft-deleted-state-after-delete-operation-is-complete"></a>Silme işlemi tamamlandıktan sonra verilerimin yumuşak silinmiş durumda tutulacağı gün sayısını yapılandırabilir miyim?

Hayır, silme işleminden sonra 14 günlük ek bekletme ile sabitlenir.

#### <a name="do-i-need-to-pay-the-cost-for-this-additional-14-day-retention"></a>Bu ek 14 günlük saklama nın maliyetini ödemem gerekiyor mu?

Hayır, bu 14 günlük ek saklama, yumuşak silme işlevinin bir parçası olarak ücretsiz olarak gelir.

#### <a name="can-i-perform-a-restore-operation-when-my-data-is-in-soft-delete-state"></a>Verilerim yumuşak silme durumundayken geri yükleme işlemi gerçekleştirebilir miyim?

Hayır, geri yüklemek için silinen yumuşak kaynağı silmeniz gerekir. Undelete işlemi, zaman içinde herhangi bir noktaya geri yükleyebileceğiniz **veri durumunu korumakla** birlikte kaynağı Durdur korumasına geri getirir. Çöp toplayıcı bu durumda duraklatılmış kalır.

#### <a name="will-my-snapshots-follow-the-same-lifecycle-as-my-recovery-points-in-the-vault"></a>Enstantanelerim kasadaki iyileşme noktalarımla aynı yaşam döngüsünü izleyecek mi?

Evet.

#### <a name="how-can-i-trigger-the-scheduled-backups-again-for-a-soft-deleted-resource"></a>Yumuşak silinmiş bir kaynak için zamanlanmış yedeklemeleri yeniden nasıl tetikleyebilirim?

Undelete'in ardından devam işlemi kaynağı yeniden koruyacaktır. Devam işlemi, zamanlanmış yedeklemeleri seçili bekletme dönemiyle tetiklemek için bir yedekleme ilkesini ilişkilendirer. Ayrıca, çöp toplayıcısı devam işlemi tamamlanır tamamlanmaz çalışır. Son kullanma tarihi geçmiş bir kurtarma noktasından geri yükleme yapmak istiyorsanız, devam işlemini tetiklemeden önce bunu yapmanız önerilir.

#### <a name="can-i-delete-my-vault-if-there-are-soft-deleted-items-in-the-vault"></a>Kasada yumuşak silinmiş öğeler varsa kasamı silebilir miyim?

Kurtarma Hizmetleri kasası, kasada yumuşak silinmiş durumda yedek öğeler varsa silinemez. Yumuşak silinen öğeler silme işleminden 14 gün sonra kalıcı olarak silinir. 14 gün bekleyemezseniz, [yumuşak silinmiş](#disabling-soft-delete)öğeleri silin ve kalıcı olarak silinmek için tekrar silin' i devre dışı bırakabilirsiniz. Korunan öğe ve yumuşak silinmiş öğe olmadığından emin olduktan sonra kasa silinebilir.  

#### <a name="can-i-delete-the-data-earlier-than-the-14-days-soft-delete-period-after-deletion"></a>Verileri silme işleminden sonraki 14 günlük yumuşak silme döneminden önce silebilir miyim?

Hayır. Yumuşak silinmiş öğeleri silmeye zorlayamazsınız, 14 gün sonra otomatik olarak silinirler. Bu güvenlik özelliği, yedeklenen verileri yanlışlıkla veya kötü amaçlı silmelere karşı korumak için etkinleştirilir.  VM üzerinde başka bir eylem gerçekleştirmeden önce 14 gün beklemelisiniz.  Yumuşak silinmiş öğeler ücretlendirilmez.  14 gün içinde yumuşak silme için işaretlenmiş VM'leri yeni bir kasaya yeniden korumanız gerekiyorsa, Microsoft desteğine başvurun.

#### <a name="can-soft-delete-operations-be-performed-in-powershell-or-cli"></a>Soft silme işlemleri PowerShell veya CLI'de gerçekleştirilebilir mi?

Soft silme işlemleri [PowerShell](#soft-delete-for-vms-using-azure-powershell)kullanılarak gerçekleştirilebilir. Şu anda CLI desteklenmiyor.

#### <a name="is-soft-delete-supported-for-other-cloud-workloads-like-sql-server-in-azure-vms-and-sap-hana-in-azure-vms"></a>Azure VM'lerde SQL Server ve Azure VM'lerde SAP HANA gibi diğer bulut iş yükleri için yumuşak silme desteklenir mi?

Hayır. Şu anda yumuşak silme yalnızca Azure sanal makineleri için desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Yedekleme için Güvenlik denetimleri](backup-security-controls.md)hakkında bilgi edinin.
