---
title: PowerShell kullanarak bir Azure dosya paylaşımının yedeklenmesi
description: Bu makalede, Azure Backup hizmetini ve PowerShell 'i kullanarak bir Azure dosyaları dosya paylaşımının nasıl yedekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 08/20/2019
ms.openlocfilehash: 948931764769bc967b88e7942b7e8384b0f93dff
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077010"
---
# <a name="back-up-an-azure-file-share-by-using-powershell"></a>PowerShell kullanarak bir Azure dosya paylaşımının yedeklenmesi

Bu makalede, bir Azure dosya dosya paylaşımının bir [Azure Backup](backup-overview.md) kurtarma hizmetleri Kasası aracılığıyla yedeklenmesi için Azure PowerShell nasıl kullanılacağı açıklanır.

Bu makalede nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
>
> * PowerShell 'i ayarlayın ve kurtarma hizmetleri sağlayıcısını kaydedin.
> * Kurtarma Hizmetleri kasası oluşturun.
> * Azure dosya paylaşımının yedeklemesini yapılandırın.
> * Bir yedekleme işi çalıştırın.

## <a name="before-you-start"></a>Başlamadan önce

* Kurtarma Hizmetleri kasaları hakkında [daha fazla bilgi edinin](backup-azure-recovery-services-vault-overview.md) .
* Azure kitaplığı 'ndaki az. RecoveryServices [cmdlet başvuru](/powershell/module/az.recoveryservices) başvurusunu gözden geçirin.
* Kurtarma Hizmetleri için aşağıdaki PowerShell nesne hiyerarşisini gözden geçirin:

  ![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

## <a name="set-up-powershell"></a>PowerShell 'i ayarlama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell 'i aşağıdaki şekilde ayarlayın:

1. [En son Azure PowerShell sürümünü indirin](/powershell/azure/install-az-ps).

    > [!NOTE]
    > Azure dosya paylaşımlarının yedeklenmesi için gereken en düşük PowerShell sürümü az. RecoveryServices 2.6.0. En son sürümü veya en düşük sürümü kullanarak, mevcut betiklerle sorunları önlemenize yardımcı olur. Aşağıdaki PowerShell komutunu kullanarak en düşük sürümü yüklemelisiniz:
    >
    > ```powershell
    > Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
    > ```

2. Bu komutu kullanarak Azure Backup PowerShell cmdlet 'lerini bulun:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Azure Backup, Azure Site Recovery ve kurtarma hizmetleri Kasası için diğer adları ve cmdlet 'leri inceleyin. İşte neleri görebileceğiniz hakkında bir örnek. Cmdlet 'lerin tamamen bir listesi değildir.

    ![Kurtarma Hizmetleri cmdlet 'lerinin listesi](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. **Connect-AzAccount**komutunu kullanarak Azure hesabınızda oturum açın.
5. Görüntülenen Web sayfasında hesap kimlik bilgilerinizi girmeniz istenir.

    Alternatif olarak, **-Credential**kullanarak hesap kimlik bilgilerinizi **Connect-azaccount** cmdlet 'ine bir parametre olarak dahil edebilirsiniz.

    Bir kiracı adına çalışan bir CSP iş ortağıysanız, müşteriyi kiracı olarak belirtin. Kiracı KIMLIKLERINI veya kiracı birincil etki alanı adını kullanın. **Connect-AzAccount-Tenant "fabrikam.com"** bir örnektir.

6. Hesapla birlikte kullanmak istediğiniz aboneliği ilişkilendirin, çünkü bir hesap birden çok aboneliğe sahip olabilir:

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

7. Azure Backup ilk kez kullanıyorsanız, Azure kurtarma hizmetleri sağlayıcısını aboneliğinize kaydetmek için **register-AzResourceProvider** cmdlet 'ini kullanın:

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

8. Sağlayıcıların başarıyla kaydedildiğini doğrulayın:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

9. Komut çıkışında, **Registrationstate** ' ın **kayıtlı**olarak değiştiğini doğrulayın. Değilse, **register-AzResourceProvider** cmdlet 'ini tekrar çalıştırın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri Kasası bir Kaynak Yöneticisi kaynağıdır, bu nedenle onu bir kaynak grubuna yerleştirmeniz gerekir. Var olan bir kaynak grubunu kullanabilir veya **New-AzResourceGroup** cmdlet 'ini kullanarak bir kaynak grubu oluşturabilirsiniz. Bir kaynak grubu oluşturduğunuzda, bunun adını ve konumunu belirtin.

Kurtarma Hizmetleri Kasası oluşturmak için aşağıdaki adımları izleyin:

1. Mevcut bir kaynak grubunuz yoksa, [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) cmdlet 'ini kullanarak yeni bir tane oluşturun. Bu örnekte, Batı ABD bölgesinde bir kaynak grubu oluşturacağız:

   ```powershell
   New-AzResourceGroup -Name "test-rg" -Location "West US"
   ```

1. Kasayı oluşturmak için [New-Azrecoveryserviceskasa](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault) cmdlet 'ini kullanın. Kaynak grubu için kullandığınız kasa için aynı konumu belirtin.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

### <a name="view-the-vaults-in-a-subscription"></a>Bir abonelikteki kasaları görüntüleme

Abonelikteki tüm kasaları görüntülemek için [Get-Azrecoveryserviceskasasını](/powershell/module/az.recoveryservices/get-azrecoveryservicesvault)kullanın:

```powershell
Get-AzRecoveryServicesVault
```

Çıktı aşağıdakine benzer. Çıktının ilişkili kaynak grubunu ve konumunu sağladığını unutmayın.

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

Birçok Azure Backup cmdlet 'i, giriş olarak kurtarma hizmetleri Kasası nesnesini gerektirir, bu nedenle kasa nesnesini bir değişkende depolamak yararlıdır.

Kasa bağlamı, kasada korunan veri türüdür. [Set-AzRecoveryServicesVaultContext](/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext)kullanarak ayarlayın. Bağlam ayarlandıktan sonra, sonraki tüm cmdlet 'ler için geçerli olur.

Aşağıdaki örnek, **testkasası**için kasa bağlamını ayarlar:

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Kasa KIMLIĞINI getir

Azure PowerShell yönergelerine uygun olarak kasa bağlamı ayarını kullanımdan kaldırmayı planlıyoruz. Bunun yerine, kasa KIMLIĞINI depolayıp alıp ilgili komutlara geçirebilmeniz gerekir. Kasa bağlamını ayarlamadıysanız veya belirli bir kasa için çalıştırılacak komutu belirtmek istiyorsanız, kasa KIMLIĞINI `-vaultID` aşağıdaki gibi tüm ilgili komutlara geçirin:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultID $vaultID
```

## <a name="configure-a-backup-policy"></a>Yedekleme ilkesi yapılandırma

Yedekleme ilkesi yedeklemeler için zamanlamayı ve yedekleme kurtarma noktalarının ne kadar süreyle tutulması gerektiğini belirtir.

Yedekleme ilkesi en az bir bekletme ilkesiyle ilişkilendirilir. Bir bekletme ilkesi, bir kurtarma noktasının silinmeden önce ne kadar süreyle saklanacağını tanımlar. Yedeklemeleri günlük, haftalık, aylık veya yıllık bekletme ile yapılandırabilirsiniz.

Yedekleme ilkeleri için bazı cmdlet 'ler aşağıda verilmiştir:

* [Get-AzRecoveryServicesBackupRetentionPolicyObject](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject)kullanarak varsayılan yedekleme ilkesi bekletmesini görüntüleyin.
* [Get-AzRecoveryServicesBackupSchedulePolicyObject](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject)kullanarak varsayılan yedekleme ilkesi zamanlamasını görüntüleyin.
* [New-AzRecoveryServicesBackupProtectionPolicy](/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)kullanarak yeni bir yedekleme ilkesi oluşturun. Zamanlama ve bekletme ilkesi nesnelerini giriş olarak girersiniz.

Varsayılan olarak, bir başlangıç saati zamanlama İlkesi nesnesinde tanımlanmıştır. Başlangıç saatini istenen başlangıç saatine dönüştürmek için aşağıdaki örneği kullanın. İstenen başlangıç saati Evrensel Eşgüdümlü saat (UTC) biçiminde olmalıdır. Örnek, istenen başlangıç zamanının günlük yedeklemeler için 01:00 olarak UTC olduğunu varsayar.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$UtcTime = Get-Date -Date "2019-03-20 01:30:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Başlangıç saatini yalnızca 30 dakikalık katlara sağlamanız gerekir. Yukarıdaki örnekte, yalnızca "01:00:00" veya "02:30:00" olabilir. Başlangıç saati "01:15:00" olamaz.

Aşağıdaki örnek, zaman çizelgesi ilkesini ve bekletme ilkesini değişkenler halinde depolar. Daha sonra bu değişkenleri yeni bir ilke (**Newafspolicy**) için parametre olarak kullanır. **Newafspolicy** günlük bir yedekleme gerçekleştirir ve 30 gün boyunca saklar.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Çıkış aşağıdakine benzer:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2019 1:30:00 AM
```

## <a name="enable-backup"></a>Yedeklemeyi etkinleştir

Yedekleme ilkesini tanımladıktan sonra, ilkeyi kullanarak Azure dosya paylaşımının korumasını etkinleştirebilirsiniz.

### <a name="retrieve-a-backup-policy"></a>Yedekleme ilkesi alma

[Get-AzRecoveryServicesBackupProtectionPolicy](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy)kullanarak ilgili ilke nesnesini getirin. Bir iş yükü türüyle ilişkili ilkeleri görüntülemek veya belirli bir ilkeyi almak için bu cmdlet 'i kullanın.

#### <a name="retrieve-a-policy-for-a-workload-type"></a>İş yükü türü için ilke alma

Aşağıdaki örnek, **AzureFiles**iş yükü türü için ilkeleri alır:

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

Çıkış aşağıdakine benzer:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> PowerShell 'deki **backuptime** alanının saat dilimi UTC 'dir. Yedekleme saati Azure portal gösterildiğinde, saat yerel saat diliminize ayarlanır.

#### <a name="retrieve-a-specific-policy"></a>Belirli bir ilkeyi alma

Aşağıdaki ilke, **dadilyafs**adlı yedekleme ilkesini alır:

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

### <a name="enable-protection-and-apply-the-policy"></a>Korumayı etkinleştirme ve ilkeyi uygulama

[Enable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection)kullanarak korumayı etkinleştirin. İlke kasayla ilişkilendirildikten sonra, yedeklemeler ilke zamanlamasına uygun olarak tetiklenir.

Aşağıdaki örnek, **Teststorageacct**depolama hesabındaki Azure dosya paylaşımı **Testazurefileshare** için korumayı, **Bu ilkeyle birlikte**etkinleştirilir:

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

Komut, yapılandırma koruması işi bitene kadar bekler ve aşağıdaki örneğe benzer bir çıktı verir:

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

Bir depolama hesabı için dosya paylaşımlarının listesini alma hakkında daha fazla bilgi için [Bu makaleye](/powershell/module/az.storage/get-azstorageshare)bakın.

## <a name="important-notice-backup-item-identification"></a>Önemli duyuru: yedekleme öğesi kimliği

Bu bölümde, genel kullanıma hazırlık aşamasında Azure dosya paylaşımlarının yedeklemelerinde önemli bir değişiklik özetlenmektedir.

Azure dosya paylaşımları için bir yedeklemeyi etkinleştirirken, kullanıcı müşteriye varlık adı olarak bir dosya paylaşım adı verir ve bir yedekleme öğesi oluşturulur. Yedekleme öğesinin adı, Azure Backup hizmetinin oluşturduğu benzersiz bir tanımlayıcıdır. Genellikle tanımlayıcı, Kullanıcı dostu bir addır. Ancak, bir dosya paylaşımının silinebileceği ve aynı adla başka bir dosya paylaşımının oluşturulabildiği geçici silme senaryosunu işlemek için, bir Azure dosya paylaşımının benzersiz kimliği artık bir KIMLIK olur.

Her öğenin benzersiz KIMLIĞINI bildirmek için, tüm ilgili öğeleri almak üzere **Backupmanagementtype** ve **workloadtype** için Ilgili filtrelerle **Get-azrecoveryservicesbackupıtem** komutunu çalıştırın. Ardından döndürülen PowerShell nesnesi/yanıtı içindeki ad alanını gözlemleyin.

Öğeleri listemenizi ve sonra yanıttaki ad alanından benzersiz adlarını almanızı öneririz. Bu değeri, öğeleri *ad* parametresiyle filtrelemek için kullanın. Aksi takdirde, öğesini KIMLIĞINE sahip öğeyi almak için *FriendlyName* parametresini kullanın.

> [!IMPORTANT]
> PowerShell 'in Azure dosya paylaşımlarının yedeklemeleri için en düşük sürüme (az. RecoveryServices 2.6.0) yükseltildiğinden emin olun. Bu sürümle birlikte, *FriendlyName* filtresi **Get-Azrecoveryservicesbackupıtem** komutu için kullanılabilir.
>
> Azure dosya paylaşımının adını *FriendlyName* parametresine geçirin. Dosya paylaşımının adını *Name* parametresine geçirirseniz, bu sürüm adı *FriendlyName* parametresine geçirmek için bir uyarı oluşturur.
>
> En düşük sürümü yüklememe, mevcut betiklerin oluşmasına neden olabilecek. Aşağıdaki komutu kullanarak PowerShell 'in en düşük sürümünü yüklemelisiniz:
>
>```powershell
>Install-module -Name Az.RecoveryServices -RequiredVersion 2.6.0
>```

## <a name="trigger-an-on-demand-backup"></a>İsteğe bağlı yedekleme tetikleyin

Korumalı bir Azure dosya paylaşımının isteğe bağlı yedeklemesini çalıştırmak için [Backup-Azrecoveryservicesbackupıtem](/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem) komutunu kullanın:

1. [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer)komutunu kullanarak, yedekleme verilerinizi tutan kasaydaki kapsayıcıdan depolama hesabını alın.
2. Bir yedekleme işi başlatmak için [Get-Azrecoveryservicesbackupıtem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem)kullanarak Azure dosya paylaşımıyla ilgili bilgi edinin.
3. [Backup-Azrecoveryservicesbackupıtem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem)kullanarak isteğe bağlı yedekleme çalıştırın.

İsteğe bağlı yedeklemeyi aşağıdaki gibi çalıştırın:

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -FriendlyName "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

Bu komut, aşağıdaki örnekte gösterildiği gibi, KIMLIĞI izlenecek bir iş döndürür:

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Yedeklemeler çekilirken Azure dosya paylaşma anlık görüntüleri kullanılır. Genellikle iş, komutun bu çıktıyı döndürdüğü zamana göre tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Portal Azure dosyalarını yedekleme](backup-afs.md)hakkında bilgi edinin.
* Yedeklemeleri zamanlamak için bir Azure Otomasyonu runbook 'u kullanarak [GitHub 'daki örnek komut dosyasına](https://github.com/Azure-Samples/Use-PowerShell-for-long-term-retention-of-Azure-Files-Backup) bakın.
