---
title: Azure dosyalarını PowerShell ile yedekleme
description: Bu makalede, Azure Backup hizmetini ve PowerShell 'i kullanarak Azure dosyalarını nasıl yedekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: f85451e0da6458de34aea936836b46781f4c4a21
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79273546"
---
# <a name="back-up-azure-files-with-powershell"></a>Azure dosyalarını PowerShell ile yedekleme

Bu makalede, bir [Azure Backup](backup-overview.md) kurtarma hizmetleri Kasası kullanarak bir Azure dosyaları dosya paylaşımının yedeklenmesi için Azure PowerShell nasıl kullanılacağı açıklanır.

Bu makalede nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
>
> * PowerShell 'i ayarlayın ve Azure kurtarma hizmetleri sağlayıcısını kaydedin.
> * Kurtarma Hizmetleri kasası oluşturun.
> * Azure dosya paylaşımının yedeklemesini yapılandırın.
> * Bir yedekleme işi çalıştırın.

## <a name="before-you-start"></a>Başlamadan önce

* Kurtarma Hizmetleri kasaları hakkında [daha fazla bilgi edinin](backup-azure-recovery-services-vault-overview.md) .
* [Azure dosya paylaşımlarını yedeklemeye](backup-afs.md)yönelik Önizleme özellikleri hakkında bilgi edinin.
* Kurtarma Hizmetleri için PowerShell nesne hiyerarşisini gözden geçirin.

## <a name="recovery-services-object-hierarchy"></a>Kurtarma Hizmetleri nesne hiyerarşisi

Nesne hiyerarşisi aşağıdaki diyagramda özetlenir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Azure kitaplığı 'ndaki **az. RecoveryServices** [cmdlet başvuru](/powershell/module/az.recoveryservices) başvurusunu gözden geçirin.

## <a name="set-up-and-install"></a>Ayarlama ve kurma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell 'i aşağıdaki şekilde ayarlayın:

1. [Az PowerShell ' in en son sürümünü indirin](/powershell/azure/install-az-ps). Gerekli en düşük sürüm 1.0.0 ' dir.

> [!WARNING]
> Önizleme için gereken en düşük PS sürümü ' az 1.0.0 ' idi. GA için yaklaşan değişiklikler nedeniyle, gereken en düşük PS sürümü ' az. RecoveryServices 2.6.0 ' olacaktır. Tüm mevcut PS sürümlerini bu sürüme yükseltmek çok önemlidir. Aksi takdirde, mevcut betikler GA 'dan sonra kesilir. En düşük sürümü aşağıdaki PS komutlarıyla birlikte yükler

```powershell
Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
```

2. Bu komutla Azure Backup PowerShell cmdlet 'lerini bulun:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Azure Backup, Azure Site Recovery ve kurtarma hizmetleri Kasası için diğer adları ve cmdlet 'leri gözden geçirin. İşte neleri görebileceğiniz hakkında bir örnek. Cmdlet 'lerin tamamen bir listesi değildir.

    ![Kurtarma Hizmetleri cmdlet 'lerinin listesi](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. **Connect-AzAccount**ile Azure hesabınızda oturum açın.
5. Görüntülenen Web sayfasında hesap kimlik bilgilerinizi girmeniz istenir.

    * Alternatif olarak, hesap kimlik bilgilerinizi **-Credential**ile **Connect-azaccount** cmdlet 'ine bir parametre olarak dahil edebilirsiniz.
    * Bir kiracı adına çalışan bir CSP iş ortağıysanız, istemci Tenantıd veya kiracı birincil etki alanı adını kullanarak müşteriyi kiracı olarak belirtin. **Connect-AzAccount-Tenant** fabrikam.com bir örnektir.

6. Hesapla birlikte kullanmak istediğiniz aboneliği ilişkilendirin, çünkü bir hesap birden fazla aboneliğe sahip olabilir.

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Azure Backup ilk kez kullanıyorsanız, Azure kurtarma hizmetleri sağlayıcısını aboneliğinize kaydetmek için **register-AzResourceProvider** cmdlet 'ini kullanın.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Sağlayıcıların başarıyla kaydedildiğini doğrulayın:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. Komut çıkışında, **Registrationstate** ' ın **kayıtlı**olarak değiştiğini doğrulayın. Değilse, **register-AzResourceProvider** cmdlet 'ini tekrar çalıştırın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri Kasası bir Kaynak Yöneticisi kaynağıdır, bu nedenle onu bir kaynak grubuna yerleştirmeniz gerekir. Var olan bir kaynak grubunu kullanabilir veya **New-AzResourceGroup** cmdlet 'i ile bir kaynak grubu oluşturabilirsiniz. Bir kaynak grubu oluşturduğunuzda, kaynak grubu için adı ve konumu belirtin.

Kurtarma Hizmetleri Kasası oluşturmak için bu adımları izleyin.

1. Kasa, bir kaynak grubuna yerleştirilir. Mevcut bir kaynak grubunuz yoksa, [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-1.4.0)ile yeni bir tane oluşturun. Bu örnekte, Batı ABD bölgesinde yeni bir kaynak grubu oluşturacağız.

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```

2. Kasayı oluşturmak için [New-Azrecoveryserviceskasa](https://docs.microsoft.com/powershell/module/az.recoveryservices/New-AzRecoveryServicesVault?view=azps-1.4.0) cmdlet 'ini kullanın. Kaynak grubu için kullanılan kasa için aynı konumu belirtin.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. Kasa depolaması için kullanılacak artıklık türünü belirtin.

   * [Yerel olarak yedekli depolama](../storage/common/storage-redundancy-lrs.md) veya coğrafi olarak [yedekli depolama](../storage/common/storage-redundancy-grs.md)kullanabilirsiniz.
   * Aşağıdaki örnek, **testkasasının** [set-AzRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) cmd Için **-BackupStorageRedundancy** seçeneğini **geoyedekli**olarak ayarlar.

     ```powershell
     $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
     Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
     ```

### <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikteki kasaları görüntüleme

Abonelikteki tüm kasaları görüntülemek için [Get-Azrecoveryserviceskasasını](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0)kullanın.

```powershell
Get-AzRecoveryServicesVault
```

Çıktı aşağıdakine benzer. İlişkili kaynak grubu ve konumunun sağlandığını unutmayın.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

### <a name="set-the-vault-context"></a>Kasa bağlamını ayarla

Kasa nesnesini bir değişkende depolayın ve kasa bağlamını ayarlayın.

* Birçok Azure Backup cmdlet 'i, giriş olarak kurtarma hizmetleri Kasası nesnesini gerektirir, bu nedenle kasa nesnesini bir değişkende depolamak yararlıdır.
* Kasa bağlamı, kasada korunan veri türüdür. [Set-AzRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0)ile ayarlayın. Bağlam ayarlandıktan sonra, sonraki tüm cmdlet 'ler için geçerli olur.

Aşağıdaki örnek, **testkasası**için kasa bağlamını ayarlar.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Kasa KIMLIĞINI getir

Azure PowerShell yönergelerine uygun olarak kasa bağlamı ayarını kullanımdan kaldırmayı planlıyoruz. Bunun yerine, kasa KIMLIĞINI depolayıp alıp ilgili komutlara geçirebilmeniz gerekir. Bu nedenle, kasa bağlamını ayarlamadıysanız veya belirli bir kasa için çalıştırılacak komutu belirtmek istiyorsanız, kasa KIMLIĞINI "-Vatıd" olarak tüm ilgili komuta aşağıdaki gibi geçirin:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultID $vaultID
```

## <a name="configure-a-backup-policy"></a>Yedekleme ilkesi yapılandırma

Yedekleme ilkesi yedeklemeler için zamanlamayı ve yedekleme kurtarma noktalarının ne kadar süreyle tutulacağını belirtir:

* Yedekleme ilkesi en az bir bekletme ilkesiyle ilişkilendirilir. Bir bekletme ilkesi, bir kurtarma noktasının silinmeden önce ne kadar süreyle saklanacağını tanımlar.
* [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0)kullanarak varsayılan yedekleme ilkesi bekletmesini görüntüleyin.
* [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0)kullanarak varsayılan yedekleme ilkesi zamanlamasını görüntüleyin.
* Yeni bir yedekleme ilkesi oluşturmak için [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) cmdlet 'ini kullanırsınız. Zamanlama ve bekletme ilkesi nesnelerini girin.

Varsayılan olarak, bir başlangıç saati zamanlama Ilkesi nesnesinde tanımlanmıştır. Başlangıç saatini istenen başlangıç saatine dönüştürmek için aşağıdaki örneği kullanın. İstenen başlangıç saati UTC biçiminde de olmalıdır. Aşağıdaki örnek, günlük yedeklemeler için istenen başlangıç zamanının 01:00. UTC olduğunu varsayar.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$UtcTime = Get-Date -Date "2019-03-20 01:30:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Başlangıç saatini yalnızca 30 dakikalık katları olarak sağlamanız gerekir. Yukarıdaki örnekte, yalnızca "01:00:00" veya "02:30:00" olabilir. Başlangıç saati "01:15:00" olamaz

Aşağıdaki örnek, zaman çizelgesi ilkesini ve bekletme ilkesini değişkenler halinde depolar. Daha sonra bu değişkenleri yeni bir ilke (**Newafspolicy**) için parametre olarak kullanır. **Newafspolicy** günlük bir yedekleme gerçekleştirir ve 30 gün boyunca saklar.

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

## <a name="enable-backup"></a>Yedeklemeyi etkinleştir

Yedekleme ilkesini tanımladıktan sonra, ilkeyi kullanarak Azure dosya paylaşımının korumasını etkinleştirebilirsiniz.

### <a name="retrieve-a-backup-policy"></a>Yedekleme ilkesi alma

[Get-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0)ile ilgili ilke nesnesini getirin. Belirli bir ilkeyi almak veya bir iş yükü türüyle ilişkili ilkeleri görüntülemek için bu cmdlet 'i kullanın.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>İş yükü türü için ilke alma

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
> PowerShell 'deki **backuptime** alanının saat dilimi Evrensel Eşgüdümlü saat (UTC) ' dir. Yedekleme saati Azure portal gösterildiğinde, saat yerel saat diliminize ayarlanır.

### <a name="retrieve-a-specific-policy"></a>Belirli bir ilkeyi alma

Aşağıdaki ilke, **dadilyafs**adlı yedekleme ilkesini alır.

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-backup-and-apply-policy"></a>Yedeklemeyi etkinleştirme ve ilkeyi uygulama

[Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection?view=azps-1.4.0)ile korumayı etkinleştirin. İlke kasayla ilişkilendirildikten sonra, yedeklemeler ilke zamanlamasına uygun olarak tetiklenir.

Aşağıdaki örnek, **Teststorageacct**depolama hesabındaki Azure dosya paylaşımı **Testazurefileshare** ' **i ilkeyle izin**vermez.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Komut, korumayı Yapılandır işi bitene kadar bekler ve gösterildiği gibi benzer bir çıkış verir.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

## <a name="important-notice---backup-item-identification-for-afs-backups"></a>Önemli bildirim-AFS yedeklemeleri için yedekleme öğesi kimliği

Bu bölümde, GA için hazırlanan AFS yedeklemesiyle ilgili önemli bir değişiklik özetlenmektedir.

AFS için yedeklemeyi etkinleştirirken, Kullanıcı, varlık adı olarak müşterinin kolay dosya paylaşma adını sağlar ve bir yedekleme öğesi oluşturulur. Yedekleme öğesinin ' name ', Azure Backup hizmeti tarafından oluşturulan benzersiz bir tanımlayıcıdır. Genellikle tanımlayıcı Kullanıcı dostu adı içerir. Ancak, bir dosya paylaşımının silinebileceği ve aynı adla başka bir dosya paylaşımının oluşturulabilmesi için önemli olan geçici bir senaryoyu işlemek için, Azure dosya paylaşımının benzersiz kimliği artık müşterinin kolay adı yerine bir KIMLIK olur. Her öğenin benzersiz kimliğini/adını bilmek için, tüm ilgili öğeleri almak üzere backupManagementType ve WorkloadType için ilgili filtrelerle ```Get-AzRecoveryServicesBackupItem``` komutunu çalıştırın ve ardından döndürülen PS nesnesi/yanıtı içindeki ad alanını gözlemleyin. Öğeleri listelemek ve sonra ' name ' alanından yanıt olarak benzersiz adlarını almak her zaman önerilir. ' Name ' parametresiyle öğeleri filtrelemek için bu değeri kullanın. Aksi takdirde, öğeyi müşterinin kolay adı/tanımlayıcısı ile almak için FriendlyName parametresini kullanın.

> [!WARNING]
> AFS yedeklemeleri için PS sürümünün ' az. RecoveryServices 2.6.0 ' için en düşük sürüme yükseltildiğinden emin olun. Bu sürümde, ' friendlyName ' filtresi ```Get-AzRecoveryServicesBackupItem``` komutu için kullanılabilir. Azure dosya paylaşımının adını friendlyName parametresine geçirin. Azure dosya paylaşımının adını ' name ' parametresine geçirirseniz, bu sürüm kolay ad parametresine bu kolay adı geçirmek için bir uyarı oluşturur. Bu en düşük sürümü yüklememe, mevcut betiklerin oluşmasına neden olabilecek. Aşağıdaki komutla en düşük PS sürümünü yükler.

```powershell
Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
```

## <a name="trigger-an-on-demand-backup"></a>İsteğe bağlı yedekleme tetikleyin

Korumalı bir Azure dosya paylaşımının isteğe bağlı yedeklemesini çalıştırmak için [Backup-Azrecoveryservicesbackupıtem](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem?view=azps-1.4.0) komutunu kullanın.

1. [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer)ile yedekleme verilerinizi tutan, kasadaki kapsayıcıdan depolama hesabını alın.
2. Bir yedekleme işi başlatmak için [Get-Azrecoveryservicesbackupıtem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem)ile Azure dosya paylaşımıyla ilgili bilgiler elde edersiniz.
3. [Backup-Azrecoveryservicesbackupıtem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem)ile isteğe bağlı bir yedekleme çalıştırın.

İsteğe bağlı yedeklemeyi aşağıdaki gibi çalıştırın:

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -FriendlyName "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Komut, aşağıdaki örnekte gösterildiği gibi, bir KIMLIĞE sahip bir iş döndürür.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Yedeklemeler çekilirken Azure dosya paylaşımının anlık görüntüleri kullanılır, bu nedenle genellikle iş komutun bu çıktıyı döndürdüğü zamana göre tamamlanır.

### <a name="using-on-demand-backups-to-extend-retention"></a>Bekletmeyi uzatmak için isteğe bağlı yedeklemeleri kullanma

İstek üzerine yedeklemeler, anlık görüntülerinizi 10 yıl boyunca koruyacak şekilde kullanılabilir. Zamanlayıcılar, seçili bekletme ile isteğe bağlı PowerShell betikleri çalıştırmak için kullanılabilir ve bu nedenle, anlık görüntüleri her hafta, ay veya yılda düzenli aralıklarla alabilir. Düzenli anlık görüntüler alırken Azure Backup kullanarak [isteğe bağlı yedeklemelerin kısıtlamalarına](https://docs.microsoft.com/azure/backup/backup-azure-files-faq#how-many-on-demand-backups-can-i-take-per-file-share) bakın.

Örnek komut dosyaları arıyorsanız, Azure Otomasyonu runbook 'u kullanarak GitHub 'daki örnek betiğe (<https://github.com/Azure-Samples/Use-PowerShell-for-long-term-retention-of-Azure-Files-Backup>) başvurabilirsiniz. bu sayede, yedeklemeleri düzenli aralıklarla zamanlamanıza ve 10 yıla kadar bile koruyabilirsiniz.

> [!WARNING]
> Automation runbook 'larınızda AFS yedeklemeleri için PS sürümünün ' az. RecoveryServices 2.6.0 ' için en düşük sürüme yükseltildiğinden emin olun. Eski ' Azurere ' modülünü ' az ' modülle değiştirmeniz gerekecektir. Bu sürümde, ' friendlyName ' filtresi ```Get-AzRecoveryServicesBackupItem``` komutu için kullanılabilir. Azure dosya paylaşımının adını friendlyName parametresine geçirin. Azure dosya paylaşımının adını ' name ' parametresine geçirirseniz, bu sürüm kolay ad parametresine bu kolay adı geçirmek için bir uyarı oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Azure portal Azure dosyalarını yedekleme [hakkında bilgi edinin](backup-afs.md) .
