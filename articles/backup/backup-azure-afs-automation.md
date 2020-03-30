---
title: PowerShell ile Azure Dosyalarını Yedekleme
description: Bu makalede, Azure Yedekleme hizmetini ve PowerShell'i kullanarak Azure Dosyalarını nasıl yedekleyebilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: f85451e0da6458de34aea936836b46781f4c4a21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273546"
---
# <a name="back-up-azure-files-with-powershell"></a>PowerShell ile Azure Dosyalarını Yedekleme

Bu makalede, Azure [Yedekleme](backup-overview.md) Kurtarma Hizmetleri kasasını kullanarak Azure Dosyaları dosya paylaşımını yedeklemek için Azure PowerShell'in nasıl kullanılacağı açıklanmaktadır.

Bu makalede, nasıl açıklanmaktadır:

> [!div class="checklist"]
>
> * PowerShell'i ayarlayın ve Azure Kurtarma Hizmetleri Sağlayıcısı'na kaydolun.
> * Kurtarma Hizmetleri kasası oluşturun.
> * Azure dosya paylaşımı için yedeklemeyi yapılandırın.
> * Yedek bir iş çalıştırın.

## <a name="before-you-start"></a>Başlamadan önce

* Kurtarma Hizmetleri kasaları hakkında [daha fazla bilgi edinin.](backup-azure-recovery-services-vault-overview.md)
* [Azure dosya paylaşımlarını yedeklemek](backup-afs.md)için önizleme özellikleri hakkında bilgi edinin.
* Kurtarma Hizmetleri için PowerShell nesne hiyerarşisini gözden geçirin.

## <a name="recovery-services-object-hierarchy"></a>Kurtarma Hizmetleri nesne hiyerarşisi

Nesne hiyerarşisi aşağıdaki diyagramda özetlenmiştir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Azure kitaplığındaki **Az.RecoveryServices** [cmdlet başvuru](/powershell/module/az.recoveryservices) başvurularını gözden geçirin.

## <a name="set-up-and-install"></a>Kurulum ve yükleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'i aşağıdaki gibi ayarlayın:

1. [Az PowerShell en son sürümünü indirin.](/powershell/azure/install-az-ps) Gerekli minimum sürüm 1.0.0'dır.

> [!WARNING]
> PS önizleme için gerekli minimum sürümü 'Az 1.0.0' idi. GA için yaklaşan değişiklikler nedeniyle, gerekli minimum PS sürümü 'Az.RecoveryServices 2.6.0' olacaktır. Mevcut tüm PS sürümlerini bu sürüme yükseltmek çok önemlidir. Aksi takdirde, varolan komut dosyaları GA sonra kırılır. Aşağıdaki PS komutlarıyla minimum sürümü yükleyin

```powershell
Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
```

2. Bu komutla Azure Backup PowerShell cmdlets'i bulun:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Azure Yedekleme, Azure Site Kurtarma ve Kurtarma Hizmetleri kasası için takma adları ve cmdlets'i gözden geçirin. İşte göreebileceklerinizin bir örneği. Cmdlet'lerin tam listesi değil.

    ![Kurtarma Hizmetleri cmdlets listesi](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. **Connect-AzAccount**ile Azure hesabınızda oturum açın.
5. Görünen web sayfasında, hesap kimlik bilgilerinizi girdiniz istenir.

    * Alternatif olarak, **-Kimlik**bilgileri ile **Connect-AzAccount** cmdlet'e hesap kimlik bilgilerinizi parametre olarak ekleyebilirsiniz.
    * Kiracı adına çalışan bir CSP iş ortağıysanız, kiracı kimliğini veya birincil etki alanı adını kullanarak müşteriyi kiracı olarak belirtin. Bir örnek **Connect-AzAccount -Kiracı** fabrikam.com.

6. Bir hesabın birkaç aboneliği olabileceğinden, kullanmak istediğiniz aboneliği hesapla ilişkilendirin.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Azure Yedekleme'yi ilk kez kullanıyorsanız, Azure Kurtarma Hizmetleri sağlayıcısını aboneliğinize kaydettirmek için **Register-AzResourceProvider** cmdlet'ini kullanın.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Sağlayıcıların başarılı bir şekilde kaydolduğunu doğrulayın:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. Komut çıktısında, **RegistrationState'in** **Kayıtlı'da**değişiklik yaptığından doğrulayın. Yoksa **Register-AzResourceProvider** cmdlet'i yeniden çalıştırın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası bir Kaynak Yöneticisi kaynağıdır, bu nedenle kaynak grubuna yerleştirmeniz gerekir. Varolan bir kaynak grubunu kullanabilir veya Yeni Kaynak **Grubu** cmdlet'ini içeren bir kaynak grubu oluşturabilirsiniz. Bir kaynak grubu oluşturduğunuzda, kaynak grubunun adını ve konumunu belirtin.

Kurtarma Hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin.

1. Bir kaynak grubuna kasa yerleştirilir. Varolan bir kaynak grubunuz yoksa, Yeni Kaynak [Grubu](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0)ile yeni bir kaynak grubu oluşturun. Bu örnekte, Batı ABD bölgesinde yeni bir kaynak grubu oluşturuyoruz.

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```

2. Tonoz oluşturmak için [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) cmdlet'i kullanın. Kaynak grubu için kullanılan kasa için aynı konumu belirtin.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. Kasa depolamasında kullanılacak artıklık türünü belirtin.

   * [Yerel olarak yedekli depolama](../storage/common/storage-redundancy-lrs.md) veya [coğrafi yedekli depolama](../storage/common/storage-redundancy-grs.md)kullanabilirsiniz.
   * Aşağıdaki örnek, **GeoRedundant**olarak ayarlanmış **testvault** için[Set-AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) cmd için **-BackupStorageRedundancy** seçeneğini ayarlar.

     ```powershell
     $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
     Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
     ```

### <a name="view-the-vaults-in-a-subscription"></a>Abonelikteki kasaları görüntüleme

Abonelikteki tüm kasaları görüntülemek için [Get-AzRecoveryServicesVault'u](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0)kullanın.

```powershell
Get-AzRecoveryServicesVault
```

Çıktı aşağıdakine benzer. İlişkili kaynak grubunun ve konumunun sağlandığını unutmayın.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

### <a name="set-the-vault-context"></a>Kasa bağlamını ayarlama

Kasa nesnesini bir değişkende saklayın ve kasa bağlamını ayarlayın.

* Birçok Azure Yedekleme cmdlet'i giriş olarak Kurtarma Hizmetleri kasa nesnesi gerektirir, bu nedenle kasa nesnesini bir değişkende depolamak uygundur.
* Kasa bağlamı, kasada korunan veri türüdür. [Set-AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0)ile ayarlayın. Bağlam ayarlandıktan sonra, sonraki tüm cmdlets için geçerlidir.

Aşağıdaki örnek, **test vault**için kasa bağlamını ayarlar.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Kasa kimliğini getirin

Azure PowerShell yönergelerine uygun olarak kasa bağlam ayarını küçümsemeyi planlıyoruz. Bunun yerine, kasa kimliğini saklayabilir veya getirebilir ve ilgili komutlara geçirebilirsiniz. Bu nedenle, kasa bağlamını ayarlamadıysanız veya belirli bir kasa için çalışacak komutu belirtmek istiyorsanız, kasa kimliğini ilgili tüm komutlara aşağıdaki gibi "-vaultID" olarak geçirin:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultID $vaultID
```

## <a name="configure-a-backup-policy"></a>Yedekleme ilkesini yapılandırma

Yedekleme ilkesi, yedekleme zamanlamasını ve yedekleme kurtarma noktalarının ne kadar süreyle tutulması gerektiğini belirtir:

* Yedekleme ilkesi en az bir bekletme ilkesiyle ilişkilidir. Bekletme ilkesi, kurtarma noktasının silinmeden önce ne kadar süreyle tutulduğunu tanımlar.
* [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0)kullanarak varsayılan yedekleme ilkesi tutma görüntüleyin.
* [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0)kullanarak varsayılan yedekleme ilkesi zamanlamasını görüntüleyin.
* Yeni bir yedekleme ilkesi oluşturmak için [Yeni AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) cmdlet'i kullanırsınız. Zamanlama ve bekletme ilkesi nesnelerini girdiniz.

Varsayılan olarak, Zamanlama İlkesi Nesnesi'nde bir başlangıç zamanı tanımlanır. Başlangıç saatini istenilen başlangıç saatine değiştirmek için aşağıdaki örneği kullanın. İstenilen başlangıç süresi UTC'de de olmalıdır. Aşağıdaki örnekte, günlük yedeklemeler için istenen başlangıç saatinin 01:00 UTC olduğu varsayar.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$UtcTime = Get-Date -Date "2019-03-20 01:30:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Başlangıç saatini yalnızca 30 dakikalık katlar olarak sağlamanız gerekir. Yukarıdaki örnekte, yalnızca "01:00:00" veya "02:30:00" olabilir. Başlangıç saati "01:15:00" olamaz

Aşağıdaki örnek, zamanlama ilkesini ve bekletme ilkesini değişkenlerde saklar. Daha sonra bu değişkenleri yeni bir ilke **(NewAFSPolicy)** için parametre olarak kullanır. **NewAFSPolicy** günlük yedekleme alır ve 30 gün boyunca saklar.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Çıktı aşağıdakine benzer.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2019 1:30:00 AM
```

## <a name="enable-backup"></a>Yedeklemeyi etkinleştirme

Yedekleme ilkesini tanımladıktan sonra, ilkeyi kullanarak Azure dosya paylaşımının korunmasını etkinleştirebilirsiniz.

### <a name="retrieve-a-backup-policy"></a>Yedekleme ilkesi alma

[Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0)ile ilgili ilke nesnesini getirin. Belirli bir ilke almak veya iş yükü türüyle ilişkili ilkeleri görüntülemek için bu cmdlet'i kullanın.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>İş yükü türü için bir ilke alma

Aşağıdaki örnek, **AzureFiles**iş yükü türü için ilkeleri alır.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

Çıktı aşağıdakine benzer.

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> PowerShell'deki **BackupTime** alanının saat dilimi Evrensel Eşgüdümlü Saat (UTC) 'dir. Yedekleme zamanı Azure portalında gösterildiğinde, saat yerel saat diliminize ayarlanır.

### <a name="retrieve-a-specific-policy"></a>Belirli bir ilke alma

Aşağıdaki ilke **dailyafs**adlı yedekleme ilkesini alır.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-backup-and-apply-policy"></a>Yedeklemeyi etkinleştirme ve ilke uygulayın

[Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0)ile korumayı etkinleştirin. İlke kasayla ilişkilendirildikten sonra, yedeklemeler ilke zamanlamasına göre tetiklenir.

Aşağıdaki örnek, azure dosya paylaşımı **testiAzureFileShare** için depolama hesabı **testiStorageAcct**için koruma sağlar , politika **dailyafs**ile .

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Komut, yapılandırma koruma işi tamamlanana kadar bekler ve gösterildiği gibi benzer bir çıktı verir.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

## <a name="important-notice---backup-item-identification-for-afs-backups"></a>Önemli uyarı - AFS yedeklemeleri için yedek madde tanımlama

Bu bölümde GA için hazırlık AFS yedekleme önemli bir değişiklik özetliyor.

KULLANıCı, AFS yedeklemesini etkinleştirirken, varlık adı olarak müşteri dostu Dosya paylaşım adını sağlar ve bir yedek öğe oluşturulur. Yedekleme öğesinin 'adı' Azure Yedekleme hizmeti tarafından oluşturulan benzersiz bir tanımlayıcıdır. Genellikle tanımlayıcı kullanıcı dostu adı içerir. Ancak, bir dosya paylaşımının silinebileceği ve aynı adla başka bir dosya paylaşımı oluşturulabildiği önemli yumuşak silme senaryosunu işlemek için, Azure dosya paylaşımının benzersiz kimliği artık müşteri dostu ad yerine bir kimlik olacaktır. Her öğenin benzersiz kimliğini/adını bilmek için, ```Get-AzRecoveryServicesBackupItem``` ilgili tüm öğeleri almak için backupManagementType ve WorkloadType için ilgili filtreleri çalıştırmanın ve ardından döndürülen PS nesnesi/yanıtındaki ad alanını gözlemlemenin izniniz. Her zaman öğeleri listelemek ve daha sonra yanıt olarak 'ad' alanından benzersiz adlarını almak önerilir. 'Ad' parametresi ile öğeleri filtrelemek için bu değeri kullanın. Aksi takdirde, müşteri dostu adı/tanımlayıcısı ile öğeyi almak için FriendlyName parametresini kullanın.

> [!WARNING]
> PS sürümünün AFS yedeklemeleri için 'Az.RecoveryServices 2.6.0' için minimum sürüme yükseltildiklerinden emin olun. Bu sürümde komut için ```Get-AzRecoveryServicesBackupItem``` 'friendlyName' filtresi kullanılabilir. Azure Dosya Paylaşımı adını friendlyName parametresine geçirin. Azure Dosya Payı adını 'Ad' parametresine geçirirseniz, bu sürüm bu dost adı dost ad parametresine geçirmek için bir uyarı atar. Bu minimum sürümün yüklenmemesi varolan komut dosyalarının başarısızlığa yol açabileceğini. Aşağıdaki komutla PS'nin minimum sürümünü yükleyin.

```powershell
Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
```

## <a name="trigger-an-on-demand-backup"></a>İsteğe bağlı yedeklemeyi tetikleme

Korumalı bir Azure dosya paylaşımı için isteğe bağlı yedekleme çalıştırmak için [Backup-AzRecoveryServicesBackupItem'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem?view=azps-1.4.0) kullanın.

1. [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer)ile yedekleme verilerinizi tutan kasadaki kasadaki kasadaki depolama hesabını alın.
2. Yedekleme işi başlatmak [için, Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem)ile Azure dosya paylaşımı hakkında bilgi alırsınız.
3. [Backup-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem)ile isteğe bağlı yedekleme çalıştırın.

İsteğe bağlı yedeklemeyi aşağıdaki gibi çalıştırın:

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -FriendlyName "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Komut, aşağıdaki örnekte gösterildiği gibi izlenecek kimliği olan bir işi döndürür.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Yedeklemeler alınırken Azure dosya paylaşımı anlık görüntüleri kullanılır, bu nedenle genellikle komut bu çıktıyı döndürene kadar iş tamamlanır.

### <a name="using-on-demand-backups-to-extend-retention"></a>Bekletme süresini uzatmak için isteğe bağlı yedeklemeleri kullanma

İsteğe bağlı yedeklemeler anlık görüntülerinizi 10 yıl boyunca saklamak için kullanılabilir. Zamanlayıcılar, isteğe bağlı PowerShell komut dosyalarını seçilen bekletme yle çalıştırmak ve böylece her hafta, ay veya yılda düzenli aralıklarla anlık görüntü almak için kullanılabilir. Düzenli anlık görüntüler çekerken, Azure yedeklemesini kullanarak [isteğe bağlı yedeklemesınırlamalarına](https://docs.microsoft.com/azure/backup/backup-azure-files-faq#how-many-on-demand-backups-can-i-take-per-file-share) bakın.

Örnek komut dosyaları arıyorsanız, yedeklemeleri periyodik olarak zamanlamanızı ve<https://github.com/Azure-Samples/Use-PowerShell-for-long-term-retention-of-Azure-Files-Backup>10 yıla kadar saklamanızı sağlayan Azure Otomasyon runbook'u kullanarak GitHub'daki örnek komut dosyasına başvurabilirsiniz.

> [!WARNING]
> PS sürümünün otomasyon runbook'larınızdaki AFS yedekleri için 'Az.RecoveryServices 2.6.0' için minimum sürüme yükseltildiklerinden emin olun. Eski 'AzureRM' modüllerini 'Az' modülüyle değiştirmeniz gerekir. Bu sürümde komut için ```Get-AzRecoveryServicesBackupItem``` 'friendlyName' filtresi kullanılabilir. Azure dosya paylaşım adını friendlyName parametresine geçirin. Azure dosya paylaşım adını 'Ad' parametresine geçirirseniz, bu sürüm bu dost adı dost ad parametresine geçirmek için bir uyarı atar.

## <a name="next-steps"></a>Sonraki adımlar

Azure portalında Azure Dosyalarını yedekleme [hakkında bilgi edinin.](backup-afs.md)
