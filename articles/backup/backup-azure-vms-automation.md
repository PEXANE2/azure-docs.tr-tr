---
title: PowerShell ile Azure VM'leri yedekle ve kurtarın
description: PowerShell ile Azure Yedekleme'yi kullanarak Azure VM'leri nasıl yedekleyip kurtarılamayı açıklar
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 1d1074eea3d530b17904e2f49fba7c0d24e84e59
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743284"
---
# <a name="back-up-and-restore-azure-vms-with-powershell"></a>PowerShell ile Azure VM'leri yedekle ve geri yükleme

Bu makalede, PowerShell cmdlets kullanarak Bir [Azure Yedekleme](backup-overview.md) Kurtarma Hizmetleri kasasında bir Azure VM'yi nasıl yedekleyip geri yükleyeceğimiz açıklanmaktadır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Kurtarma Hizmetleri kasası oluşturun ve kasa bağlamını ayarlayın.
> * Yedekleme ilkesi tanımlama
> * Birden çok sanal makineyi korumak için yedekleme ilkesini uygulama
> * Korunan sanal makineler için isteğe bağlı yedekleme işini tetikleme Sanal makineyi yedeklemeden (veya korumadan) önce, SANAL M'lerinizi korumak için ortamınızı hazırlamak için [ön koşulları](backup-azure-arm-vms-prepare.md) tamamlamanız gerekir.

## <a name="before-you-start"></a>Başlamadan önce

* Kurtarma Hizmetleri kasaları hakkında [daha fazla bilgi edinin.](backup-azure-recovery-services-vault-overview.md)
* Azure VM yedekleme mimarisini [gözden geçirin,](backup-architecture.md#architecture-built-in-azure-vm-backup) yedekleme işlemi [hakkında bilgi edinin](backup-azure-vms-introduction.md) ve desteği, sınırlamaları ve ön koşulları gözden [geçirin.](backup-support-matrix-iaas.md)
* Kurtarma Hizmetleri için PowerShell nesne hiyerarşisini gözden geçirin.

## <a name="recovery-services-object-hierarchy"></a>Kurtarma Hizmetleri nesne hiyerarşisi

Nesne hiyerarşisi aşağıdaki diyagramda özetlenmiştir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Azure kitaplığındaki **Az.RecoveryServices** [cmdlet başvuru](https://docs.microsoft.com/powershell/module/Az.RecoveryServices/?view=azps-1.4.0) başvurularını gözden geçirin.

## <a name="set-up-and-register"></a>Ayarlama ve kaydetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Başlamak için:

1. [PowerShell'in en son sürümünü indirin](https://docs.microsoft.com/powershell/azure/install-az-ps)

2. Aşağıdaki komutu yazarak kullanılabilir Azure Backup PowerShell cmdlets'i bulun:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

    Azure Yedekleme, Azure Site Kurtarma ve Kurtarma Hizmetleri kasası için takma adlar ve cmdlets görünür. Aşağıdaki resim, göreceğiniz şeyin bir örneğidir. Cmdletlerin tam listesi değildir.

    ![Kurtarma Hizmetleri listesi](./media/backup-azure-vms-automation/list-of-recoveryservices-ps.png)

3. **Connect-AzAccount'ı**kullanarak Azure hesabınızda oturum açın. Bu cmdlet, hesap kimlik bilgileriniz için sizi bir web sayfası olarak açar:

    * Alternatif olarak, **-Kimlik** belgesi parametresini kullanarak hesap kimlik bilgilerinizi **Connect-AzAccount** cmdlet'e parametre olarak ekleyebilirsiniz.
    * Kiracı adına çalışan CSP iş ortağıysanız, kiracı kimliğini veya birincil etki alanı adını kullanarak müşteriyi kiracı olarak belirtin. Örneğin: **Connect-AzAccount -Kiracı "fabrikam.com"**

4. Bir hesabın birden fazla aboneliği olabileceğinden, kullanmak istediğiniz aboneliği hesapla ilişkilendirin:

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

5. Azure Yedekleme'yi ilk kez kullanıyorsanız, Azure Kurtarma Hizmeti sağlayıcısını aboneliğinize kaydettirmek için **[Register-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)** cmdlet'ini kullanmanız gerekir.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Sağlayıcılar'ın aşağıdaki komutları kullanarak başarılı bir şekilde kaydolduğunu doğrulayabilirsiniz:

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

    Komut çıktısında, **RegistrationState'in** **Kayıtlı**olarak değişmesi gerekir. Değilse, **[register-azresourceprovider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)** cmdlet'i yeniden çalıştırmanızı.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Aşağıdaki adımlar, bir Kurtarma Hizmetleri kasası oluşturmanıza yol açar. Kurtarma Hizmetleri kasası Yedekleme kasasından farklıdır.

1. Kurtarma Hizmetleri kasası bir Kaynak Yöneticisi kaynağıdır, bu nedenle kaynak grubuna yerleştirmeniz gerekir. Varolan bir kaynak grubunu kullanabilir veya Yeni **[Kaynak Grubu](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)** cmdlet'ine sahip bir kaynak grubu oluşturabilirsiniz. Kaynak grubu oluştururken, kaynak grubunun adını ve konumunu belirtin.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```

2. Kurtarma Hizmetleri kasasını oluşturmak için [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesvault?view=azps-1.4.0) cmdlet'ini kullanın. Kaynak grubu için kullanılan kasa için aynı konumu belirttiğinden emin olun.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. Kullanılacak depolama artıklığının türünü belirtin; [Yerel Olarak Yedekli Depolama (LRS)](../storage/common/storage-redundancy-lrs.md) veya [Geo Redundant Depolama (GRS)](../storage/common/storage-redundancy-grs.md)kullanabilirsiniz. Aşağıdaki örnekte test vault için -BackupStorageRedundancy seçeneği GeoRedundant olarak ayarlanır gösterir.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperty  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>Abonelikteki kasaları görüntüleme

Abonelikteki tüm kasaları görüntülemek için [Get-AzRecoveryServicesVault'u](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0)kullanın:

```powershell
Get-AzRecoveryServicesVault
```

Çıktı aşağıdaki örneğe benzer, ilişkili Kaynak GroupName ve Konumu sağlanır dikkat edin.

```output
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

## <a name="back-up-azure-vms"></a>Azure VM'lerini yedekleme

Sanal makinelerinizi korumak için Kurtarma Hizmetleri kasası kullanın. Korumayı uygulamadan önce kasa bağlamını (kasada korunan veri türü) ayarlayın ve koruma ilkesini doğrulayın. Koruma ilkesi, yedekleme işleri çalıştırıldığında ve her yedek anlık görüntünün ne kadar süreyle tutulduğu zamanlamadır.

### <a name="set-vault-context"></a>Tonoz bağlamı ayarlama

VM'de korumayı etkinleştirmeden önce kasa bağlamını ayarlamak için [Set-AzRecoveryServicesVaultContext'ı](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0) kullanın. Kasa bağlamı ayarlandıktan sonra, sonraki tüm cmdlet’ler için geçerli olur. Aşağıdaki örnek, tonoz, *testvault*için tonoz bağlamını ayarlar.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "Contoso-docs-rg" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>Kasa kimliğini getirin

Azure PowerShell yönergelerine uygun olarak kasa bağlam ayarını küçümsemeyi planlıyoruz. Bunun yerine, kasa kimliğini saklayabilir veya getirebilir ve ilgili komutlara geçirebilirsiniz. Bu nedenle, kasa bağlamını ayarlamadıysanız veya belirli bir kasa için çalışacak komutu belirtmek istiyorsanız, kasa kimliğini ilgili tüm komutlara aşağıdaki gibi "-vaultID" olarak geçirin:

```powershell
$targetVault = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault"
$targetVault.ID
```

Veya

```powershell
$targetVaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

### <a name="modifying-storage-replication-settings"></a>Depolama çoğaltma ayarlarını değiştirme

Kasanın Depolama çoğaltma yapılandırmasını LRS/GRS olarak ayarlamak için [Set-AzRecoveryServicesBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/Set-AzRecoveryServicesBackupProperty) komutunu kullanın

```powershell
Set-AzRecoveryServicesBackupProperty -Vault $targetVault -BackupStorageRedundancy GeoRedundant/LocallyRedundant
```

> [!NOTE]
> Depolama Artıklığı, yalnızca kasada korunan yedek öğeler yoksa değiştirilebilir.

### <a name="create-a-protection-policy"></a>Koruma ilkesi oluşturma

Bir Kurtarma Hizmetleri kasası oluşturduğunuzda bu, varsayılan koruma ve saklama ilkeleri ile birlikte gelir. Varsayılan koruma ilkesi, her gün belirtilen saatte bir yedekleme işini tetikler. Varsayılan saklama ilkesi, 30 gün boyunca günlük kurtarma noktasını korur. VM'nizi hızlı bir şekilde korumak ve daha sonra farklı ayrıntılarla ilkeyi yeniden sağlamak için varsayılan ilkeyi kullanabilirsiniz.

Kasada bulunan koruma ilkelerini görüntülemek için **[Get-AzRecoveryServicesBackupProtectionPolicy'yi](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy)** kullanın. Bu cmdlet'i belirli bir ilke almak veya iş yükü türüyle ilişkili ilkeleri görüntülemek için kullanabilirsiniz. Aşağıdaki örnek, iş yükü türü, AzureVM için ilkeler alır.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM" -VaultId $targetVault.ID
```

Çıktı aşağıdaki örneğe benzer:

```output
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> PowerShell'deki BackupTime alanının saat dilimi UTC'dir. Ancak, yedekleme süresi Azure portalında gösterildiğinde, saat yerel saat diliminize ayarlanır.
>
>

Yedekleme koruma ilkesi en az bir bekletme ilkesiyle ilişkilidir. Bekletme ilkesi, kurtarma noktasının silinmeden önce ne kadar süreyle tutulduğunu tanımlar.

* Varsayılan bekletme ilkesini görüntülemek için [Get-AzRecoveryServicesBackupRetentionPolicyObject'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject) kullanın.
* Benzer şekilde, varsayılan zamanlama ilkesini elde etmek için [Get-AzRecoveryServicesBackupSchedulePolicyObject'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject) kullanabilirsiniz.
* [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy) cmdlet yedekleme ilkesi bilgilerini tutan bir PowerShell nesnesi oluşturur.
* Zamanlama ve bekletme ilkesi nesneleri, Yeni-AzRecoveryServicesBackupProtectionPolicy cmdlet'e giriş olarak kullanılır.

Varsayılan olarak, Zamanlama İlkesi Nesnesi'nde bir başlangıç zamanı tanımlanır. Başlangıç saatini istenilen başlangıç saatine değiştirmek için aşağıdaki örneği kullanın. İstenilen başlangıç süresi UTC'de de olmalıdır. Aşağıdaki örnekte, günlük yedeklemeler için istenen başlangıç saatinin 01:00 UTC olduğu varsayar.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM" 
$UtcTime = Get-Date -Date "2019-03-20 01:00:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Başlangıç saatini yalnızca 30 dakikalık katlar olarak sağlamanız gerekir. Yukarıdaki örnekte, yalnızca "01:00:00" veya "02:30:00" olabilir. Başlangıç saati "01:15:00" olamaz

Aşağıdaki örnek, zamanlama ilkesini ve bekletme ilkesini değişkenlerde saklar. Örnek, bir koruma ilkesi, *NewPolicy*oluştururken parametreleri tanımlamak için bu değişkenleri kullanır.

```powershell
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM" 
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultId $targetVault.ID
```

Çıktı aşağıdaki örneğe benzer:

```output
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```

### <a name="enable-protection"></a>Korumayı etkinleştir

Koruma ilkesini tanımladıktan sonra, yine de bir öğeiçin ilkeyi etkinleştirmeniz gerekir. Koruma sağlamak için [Enable-AzRecoveryServicesBackupProtection'ı](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection) kullanın. Korumayı etkinleştirmek için iki nesne vardır: öğe ve ilke. İlke kasayla ilişkilendirildikten sonra, yedekleme iş akışı ilke zamanlamasında tanımlanan anda tetiklenir.

> [!IMPORTANT]
> Aynı anda birden çok VM'nin yedeklemesini etkinleştirmek için PS'yi kullanırken, tek bir ilkeyle ilişkili 100'den fazla VM olmadığından emin olun. Bu [önerilen en iyi uygulamadır.](https://docs.microsoft.com/azure/backup/backup-azure-vm-backup-faq#is-there-a-limit-on-number-of-vms-that-can-beassociated-with-a-same-backup-policy) Şu anda, PS istemcisi 100'den fazla VM varsa açıkça engellemez, ancak çek gelecekte eklenmesi planlanmaktadır.

Aşağıdaki örnekler, NewPolicy ilkesini kullanarak v2VM öğesi için koruma sağlar. Örnekler, VM'nin şifrelenip şifrelenmediğine ve ne tür şifrelemeye bağlı olarak farklılık gösterir.

**Şifrelenmemiş Kaynak Yöneticisi VM'lerinde**korumayı etkinleştirmek için:

```powershell
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1" -VaultId $targetVault.ID
```

Şifreli VM'lerde (BEK ve KEK kullanılarak şifrelenmiş) korumayı etkinleştirmek için, Azure Yedekleme hizmetine anahtar kasasından anahtarları ve sırları okuma izni vermeniz gerekir.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1" -VaultId $targetVault.ID
```

**Şifreli VM'lerde (yalnızca BEK kullanılarak şifrelenmiş)** korumayı etkinleştirmek için, Azure Yedekleme hizmetine anahtar kasasından sırları okuma izni vermeniz gerekir.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1" -VaultId $targetVault.ID
```

> [!NOTE]
> Azure Devlet bulutu kullanıyorsanız, [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) cmdlet'teki ServicePrincipalName parametresi için ff281ffe-705c-4f53-9f37-a40e6f2c68f3 değerini kullanın.
>

## <a name="monitoring-a-backup-job"></a>Yedekleme işini izleme

Azure portalını kullanmadan yedekleme işleri gibi uzun süren işlemleri izleyebilirsiniz. Devam eden bir iş durumunu almak için [Get-AzRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) cmdlet'i kullanın. Bu cmdlet belirli bir kasa için yedek işleri alır ve bu tonoz tonoz bağlamında belirtilir. Aşağıdaki örnek, devam eden bir işin durumunu bir dizi olarak alır ve durumu $joblist değişkeninde depolar.

```powershell
$joblist = Get-AzRecoveryservicesBackupJob –Status "InProgress" -VaultId $targetVault.ID
$joblist[0]
```

Çıktı aşağıdaki örneğe benzer:

```output
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016                5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Bunun yerine tamamlanması için bu işleri yoklama - gereksiz ek kod - [Wait-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) cmdlet kullanın. Bu cmdlet, iş tamamlanana veya belirtilen zaman öncesi değerine ulaşılına kadar yürütmeyi duraklatır.

```powershell
Wait-AzRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200 -VaultId $targetVault.ID
```

## <a name="manage-azure-vm-backups"></a>Azure VM yedeklemelerini yönetme

### <a name="modify-a-protection-policy"></a>Koruma ilkesini değiştirme

Koruma ilkesini değiştirmek için, SchedulePolicy veya RetentionPolicy nesnelerini değiştirmek için [Set-AzRecoveryServicesBackupProtectionPolicy'yi](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy) kullanın.

#### <a name="modifying-scheduled-time"></a>Zamanlanan saati değiştirme

Bir koruma ilkesi oluşturduğunuzda, varsayılan olarak bir başlangıç zamanı atanır. Aşağıdaki örnekler, bir koruma ilkesinin başlangıç saatini nasıl değiştireceğimi gösterir.

````powershell
$SchPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
$UtcTime = Get-Date -Date "2019-03-20 01:00:00Z" (This is the time that the customer wants to start the backup)
$UtcTime = $UtcTime.ToUniversalTime()
$SchPol.ScheduleRunTimes[0] = $UtcTime
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Set-AzRecoveryServicesBackupProtectionPolicy -Policy $pol  -SchedulePolicy $SchPol -VaultId $targetVault.ID
````

#### <a name="modifying-retention"></a>Bekletme değiştirme

Aşağıdaki örnek, kurtarma noktası bekletme 365 gün değiştirir.

```powershell
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
$retPol.DailySchedule.DurationCountInDays = 365
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Set-AzRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol -VaultId $targetVault.ID
```

#### <a name="configuring-instant-restore-snapshot-retention"></a>Anında geri yükleme anlık görüntü tutma yapılandırma

> [!NOTE]
> Az PS sürüm 1.6.0'dan itibaren Powershell'i kullanarak politikadaki anlık görüntü tutma süresini güncelleyebilirsiniz

````powershell
$bkpPol = Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM" -VaultId $targetVault.ID
$bkpPol.SnapshotRetentionInDays=7
Set-AzRecoveryServicesBackupProtectionPolicy -policy $bkpPol -VaultId $targetVault.ID
````

Varsayılan değer 2 olacak, kullanıcı değeri 1 dk ve en fazla 5 dk ile ayarlayabilir. Haftalık yedekleme ilkeleri için dönem 5 olarak ayarlanır ve değiştirilemez.

#### <a name="creating-azure-backup-resource-group-during-snapshot-retention"></a>Anlık görüntü tutma sırasında Azure Yedekleme kaynak grubu oluşturma

> [!NOTE]
> Azure PS sürüm 3.7.0'dan itibaren anlık anlık görüntüleri depolamak için oluşturulan kaynak grubunu oluşturabilir ve edebilirsiniz.

Kaynak grubu oluşturma kuralları ve diğer ilgili ayrıntılar hakkında daha fazla bilgi almak için Sanal Makineler belgeleri [için Azure Yedekleme kaynak grubuna](https://docs.microsoft.com/azure/backup/backup-during-vm-creation#azure-backup-resource-group-for-virtual-machines) bakın.

```powershell
$bkpPol = Get-AzureRmRecoveryServicesBackupProtectionPolicy -name "DefaultPolicyForVMs"
$bkpPol.AzureBackupRGName="Contosto_"
$bkpPol.AzureBackupRGNameSuffix="ForVMs"
Set-AzureRmRecoveryServicesBackupProtectionPolicy -policy $bkpPol
```

### <a name="trigger-a-backup"></a>Yedeklemeyi tetikleme

Yedekleme işini tetiklemek için [Backup-AzRecoveryServicesBackupItem'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem) kullanın. İlk yedekleme ise, tam bir yedeklemedir. Sonraki yedeklemeler artımlı bir kopya alır. Aşağıdaki örnekte, 60 gün boyunca saklanacak bir VM yedeklemesi gerekir.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM" -VaultId $targetVault.ID
$item = Get-AzRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM" -VaultId $targetVault.ID
$endDate = (Get-Date).AddDays(60).ToUniversalTime()
$job = Backup-AzRecoveryServicesBackupItem -Item $item -VaultId $targetVault.ID -ExpiryDateTimeUTC $endDate
```

Çıktı aşağıdaki örneğe benzer:

```output
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup              InProgress          4/23/2016                  5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> PowerShell'deki StartTime ve EndTime alanlarının saat dilimi UTC'dir. Ancak, zaman Azure portalında gösterildiğinde, saat yerel saat diliminize ayarlanır.
>
>

### <a name="change-policy-for-backup-items"></a>Yedekleme öğeleri için ilkeyi değiştirme

Kullanıcı, varolan ilkeyi değiştirebilir veya yedeklenmiş öğenin ilkesini İlke1'den İlke2'ye değiştirebilir. Yedeklenmiş bir öğe için ilkegeçiş yapmak için, ilgili ilkeyi [Enable-AzRecoveryServices](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) getirin ve öğeyi yedekleyin ve yedek öğeyi parametre olarak yedek öğeyle birlikte kullanın.

````powershell
$TargetPol1 = Get-AzRecoveryServicesBackupProtectionPolicy -Name <PolicyName> -VaultId $targetVault.ID
$anotherBkpItem = Get-AzRecoveryServicesBackupItem -WorkloadType AzureVM -BackupManagementType AzureVM -Name "<BackupItemName>" -VaultId $targetVault.ID
Enable-AzRecoveryServicesBackupProtection -Item $anotherBkpItem -Policy $TargetPol1 -VaultId $targetVault.ID
````

Komut, yapılandırma yedeklemesi tamamlanana kadar bekler ve aşağıdaki çıktıyı döndürür.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
TestVM           ConfigureBackup      Completed            3/18/2019 8:00:21 PM      3/18/2019 8:02:16 PM      654e8aa2-4096-402b-b5a9-e5e71a496c4e
```

### <a name="stop-protection"></a>Korumayı durdurma

#### <a name="retain-data"></a>Verileri tutma

Kullanıcı korumayı durdurmak isterse, [Devre Dışı Bırak-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) PS cmdlet'i kullanabilir. Bu, zamanlanan yedeklemeleri durdurur, ancak şimdiye kadar yedeklenen veriler sonsuza kadar saklanır.

````powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -Name "<backup item name>" -VaultId $targetVault.ID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID
````

#### <a name="delete-backup-data"></a>Yedekleme verilerini silme

Kasada depolanan yedekleme verilerini tamamen kaldırmak için '-RemoveRecoveryPoints' bayrağını/anahtarını ['devre dışı bırak' koruma komutuna](#retain-data)eklemeniz yeterlidir.

````powershell
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID -RemoveRecoveryPoints
````

## <a name="restore-an-azure-vm"></a>Azure VM'yi geri yükleme

Azure portalını kullanarak bir VM'yi geri alma ile PowerShell kullanarak bir VM'i geri geri alma arasında önemli bir fark vardır. PowerShell ile geri yükleme işlemi, kurtarma noktasından gelen diskler ve yapılandırma bilgileri oluşturulduktan sonra tamamlanır. Geri yükleme işlemi sanal makineoluşturmaz. Diskten sanal bir makine oluşturmak için, bölüme bakın, [geri yüklenen disklerden VM oluşturun.](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) VM'nin tamamını geri yüklemek istemiyor, ancak Azure VM yedeklemesinden birkaç dosyayı geri yüklemek veya kurtarmak istiyorsanız, [dosya kurtarma bölümüne](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup)bakın.

> [!Tip]
> Geri yükleme işlemi sanal makine oluşturmaz.
>
>

Aşağıdaki grafik, RecoveryServicesVault'tan BackupRecoveryPoint'e kadar nesne hiyerarşisini gösterir.

![Yedekleme Kapsayıcısı'nı gösteren Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Yedekleme verilerini geri yüklemek için, yedeklenen öğeyi ve zaman içinde verileri tutan kurtarma noktasını tanımlayın. Kasadan hesabınıza veri geri yüklemek için [Geri Yükleme-AzRecoveryServicesBackupItem'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem) kullanın.

Azure VM'yi geri yüklemek için gereken temel adımlar şunlardır:

* VM’yi seçin.
* Bir kurtarma noktası seçin.
* Diskleri geri yükleyin.
* Depolanan disklerden VM oluşturun.

### <a name="select-the-vm"></a>VM'yi seçin

Doğru yedekleme öğesini tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcıdan başlayın ve nesne hiyerarşisi aşağı doğru şekilde çalışın. VM'yi temsil eden kapsayıcıyı seçmek için [Get-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer) cmdlet ve pipe'ı [get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem) cmdlet'e kullanın.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM" -VaultId $targetVault.ID
$backupitem = Get-AzRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM" -VaultId $targetVault.ID
```

### <a name="choose-a-recovery-point"></a>Kurtarma noktası seçin

Yedek öğenin tüm kurtarma noktalarını listelemek için [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint) cmdlet'i kullanın. Sonra geri yüklemek için kurtarma noktasını seçin. Hangi kurtarma noktasının kullanılacağından emin değilseniz, listedeki en son RecoveryPointType = AppConsistent noktasını seçmek iyi bir uygulamadır.

Aşağıdaki komut dosyasında, **$rp**değişken, seçilen yedekleme öğesi için son yedi gün içinde bir kurtarma noktası dizisidir. Dizi, dizin 0'daki en son kurtarma noktasıyla ters sırayla sıralanır. Kurtarma noktasını seçmek için standart PowerShell dizi dizi dizi dizisini kullanın. Örnekte, $rp[0] en son kurtarma noktasını seçer.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime() -VaultId $targetVault.ID
$rp[0]
```

Çıktı aşağıdaki örneğe benzer:

```output
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="restore-the-disks"></a>Diskleri geri yükleme

Yedek öğenin verilerini ve yapılandırmasını kurtarma noktasına geri yüklemek için [Geri Yükleme-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem) cmdlet'ini kullanın. Bir kurtarma noktası tanımladıktan sonra, **-RecoveryPoint** parametresi için değer olarak kullanın. Yukarıdaki örnekte, **$rp[0]** kullanılacak kurtarma noktasıdır. Aşağıdaki örnek kodda, **$rp[0]** diski geri getirmek için kullanılacak kurtarma noktasıdır.

Diskleri ve yapılandırma bilgilerini geri yüklemek için:

```powershell
$restorejob = Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG" -VaultId $targetVault.ID
$restorejob
```

#### <a name="restore-managed-disks"></a>Yönetilen diskleri geri yükleme

> [!NOTE]
> Desteklenen VM diskleri yönetmişse ve bunları yönetilen diskler olarak geri yüklemek istiyorsanız, Azure PowerShell RM modülü v 6.7.0'dan bu özelliği sunduk. Itibaren
>
>

Yönetilen disklerin geri yüklendiği RG'yi belirtmek için ek bir parametre **TargetResourceGroupName** sağlayın.

> [!NOTE]
> Önemli performans iyileştirmeleri ile sonuçlanması nedeniyle yönetilen diskleri geri getirmek için **TargetResourceGroupName** parametresini kullanmanız önerilir. Ayrıca, Azure Powershell Az modülü 1.0'dan itibaren yönetilen disklerle geri yükleme durumunda bu parametre zorunludur
>
>

```powershell
$restorejob = Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG" -TargetResourceGroupName "DestRGforManagedDisks" -VaultId $targetVault.ID
```

**VMConfig.JSON** dosyası depolama hesabına geri yüklenir ve yönetilen diskler belirtilen hedef RG'ye geri yüklenir.

Çıktı aşağıdaki örneğe benzer:

```output
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Geri Yükleme işinin tamamlanmasını beklemek için [Wait-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) cmdlet'i kullanın.

```powershell
Wait-AzRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Geri Yükleme işi tamamlandıktan sonra, geri yükleme işleminin ayrıntılarını almak için [Get-AzRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob) cmdlet'i kullanın. JobDetails özelliği, VM'yi yeniden oluşturmak için gereken bilgilere sahiptir.

```powershell
$restorejob = Get-AzRecoveryServicesBackupJob -Job $restorejob -VaultId $targetVault.ID
$details = Get-AzRecoveryServicesBackupJobDetails -Job $restorejob -VaultId $targetVault.ID
```

Diskleri geri yükledikten sonra, VM'yi oluşturmak için bir sonraki bölüme gidin.

## <a name="replace-disks-in-azure-vm"></a>Azure VM'de diskleri değiştirme

Diskleri ve yapılandırma bilgilerini değiştirmek için aşağıdaki adımları gerçekleştirin:

* Adım 1: [Diskleri geri yükleme](backup-azure-vms-automation.md#restore-the-disks)
* Adım 2: [PowerShell kullanarak veri diskini ayırma](https://docs.microsoft.com/azure/virtual-machines/windows/detach-disk#detach-a-data-disk-using-powershell)
* Adım 3: [PowerShell ile Windows VM'ye veri diski ekleme](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps)

## <a name="create-a-vm-from-restored-disks"></a>Geri yüklenen disklerden VM oluşturma

Diskleri geri yükledikten sonra, sanal makineyi diskten oluşturmak ve yapılandırmak için aşağıdaki adımları kullanın.

> [!NOTE]
>
> 1. AzureAz modülü 3.0.0 veya üzeri gereklidir. <br>
> 2. Geri yüklenen disklerden şifreli VM'ler oluşturmak için Azure rolünüz, **Microsoft.KeyVault/vaults/deploy/action**adresindeki eylemi gerçekleştirmek için izin almış olmalıdır. Rolünüz bu izne sahip değilse, bu eylemle özel bir rol oluşturun. Daha fazla bilgi için [Azure RBAC'da Özel Roller'e](../role-based-access-control/custom-roles.md)bakın. <br>
> 3. Diskleri geri aldıktan sonra, artık yeni bir VM oluşturmak için doğrudan kullanabileceğiniz bir dağıtım şablonu alabilirsiniz. Artık şifrelenmiş/şifresiz yönetilen/yönetilmeyen VM'ler oluşturmak için farklı PS cmdletleri oluşturmaz.<br>
> <br>

### <a name="create-a-vm-using-the-deployment-template"></a>Dağıtım şablonunu kullanarak Bir VM oluşturma

Ortaya çıkan iş ayrıntıları, sorgulanabilen ve dağıtılabilen URI şablonuna verir.

```powershell
   $properties = $details.properties
   $storageAccountName = $properties["Target Storage Account Name"]
   $containerName = $properties["Config Blob Container Name"]
   $templateBlobURI = $properties["Template Blob Uri"]
```

Müşterinin depolama hesabı ve verilen kapsayıcı altında olduğundan şablona doğrudan erişilemez. Bu şablona erişmek için tam URL'ye (geçici bir SAS belirteciyle birlikte) ihtiyacımız vardır.

1. Önce şablon adını BlobURI şablonundan ayıklayın. Biçim aşağıda belirtilmiştir. Powershell'deki bölme işlemini bu URL'den son şablon adını ayıklamak için kullanabilirsiniz.

```http
https://<storageAccountName.blob.core.windows.net>/<containerName>/<templateName>
```

2. Daha sonra tam URL [burada](https://docs.microsoft.com/azure/azure-resource-manager/templates/secure-template-with-sas-token?tabs=azure-powershell#provide-sas-token-during-deployment)açıklandığı gibi oluşturulabilir.

```powershell
Set-AzCurrentStorageAccount -Name $storageAccountName -ResourceGroupName <StorageAccount RG name>
$templateBlobFullURI = New-AzStorageBlobSASToken -Container $containerName -Blob <templateName> -Permission r -FullUri
```

3. [Burada](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)açıklandığı gibi yeni bir VM oluşturmak için şablonu dağıtın.

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment ResourceGroupName ExampleResourceGroup -TemplateUri $templateBlobFullURI -storageAccountType Standard_GRS
```

### <a name="create-a-vm-using-the-config-file"></a>Config dosyasını kullanarak VM oluşturma

Aşağıdaki bölümde "VMConfig" dosyasını kullanarak bir VM oluşturmak için gereken adımlar listelenir.

> [!NOTE]
> VM oluşturmak için yukarıda ayrıntılı dağıtım şablonu kullanılması önerilir. Bu bölüm (Puan 1-6) yakında amortismana alınacaktır.

1. İş ayrıntıları için geri yüklenen disk özelliklerini sorgula.

   ```powershell
   $properties = $details.properties
   $storageAccountName = $properties["Target Storage Account Name"]
   $containerName = $properties["Config Blob Container Name"]
   $configBlobName = $properties["Config Blob Name"]
   ```

2. Azure depolama bağlamını ayarlayın ve JSON yapılandırma dosyasını geri yükleyin.

   ```powershell
   Set-AzCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
   $destination_path = "C:\vmconfig.json"
   Get-AzStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
   $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
   ```

3. VM yapılandırmasını oluşturmak için JSON yapılandırma dosyasını kullanın.

   ```powershell
   $vm = New-AzVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
   ```

4. İşletim sistemi diskini ve veri disklerini takın. Bu adım, çeşitli yönetilen ve şifrelenmiş VM yapılandırmaları için örnekler sağlar. VM yapılandırmanıza uygun örneği kullanın.

* **Yönetilmeyen ve şifrelenmemiş VM'ler** - Yönetilmeyen, şifrelenmemiş VM'ler için aşağıdaki örneği kullanın.

```powershell
       Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
       $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
       foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
       {
        $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
       }
```

* **Azure AD ile yönetilmeyen ve şifrelenmiş VM'ler (yalnızca BEK)** - Azure AD'li (yalnızca BEK kullanılarak şifrelenmiş) yönetilen olmayan, şifrelenmiş VM'ler için disk leri takmadan önce sırrı anahtar kasasına geri yüklemeniz gerekir. Daha fazla bilgi için, [Azure Yedekleme kurtarma noktasından şifrelenmiş sanal makineyi geri yükle'ye](backup-azure-restore-key-secret.md)bakın. Aşağıdaki örnek, şifreli VM'ler için işletim sistemi ve veri disklerinin nasıl eklenilen gösteriş olduğunu gösterir. İşletim sistemi diskini ayarlarken, ilgili işletim sistemi türünden bahsettiğinden emin olun.

```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
      {
       $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
```

* **Azure AD (BEK ve KEK) ile yönetilmeyen ve şifrelenmiş VM'ler** - Azure AD ile yönetilmeyen, şifrelenmiş VM'ler için (BEK ve KEK kullanılarak şifrelenmiş), diskleri takmadan önce anahtarı ve sırrı anahtar kasasına geri yükleyin. Daha fazla bilgi için bkz: [Azure Yedekleme kurtarma noktasından şifrelenmiş sanal makineyi geri yükle.](backup-azure-restore-key-secret.md) Aşağıdaki örnek, şifreli VM'ler için işletim sistemi ve veri disklerinin nasıl eklenilen gösteriş olduğunu gösterir.

```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
```

* **Azure AD'si olmayan yönetilen ve şifrelenmemiş VM'ler (yalnızca BEK)** - Azure AD'si olmayan yönetilen, şifrelenmemiş VM'ler için (yalnızca BEK kullanılarak şifrelenir), kaynak **keyVault/secret kullanılamıyorsa,** azure [yedekleme kurtarma noktasından şifrelenmemiş sanal makineyi geri yükleme](backup-azure-restore-key-secret.md)yordamını kullanarak sırları anahtar kasasına geri yükleyin. Ardından, geri yüklenen işletim sistemi blob'unda şifreleme ayrıntılarını ayarlamak için aşağıdaki komut dosyalarını uygulayın (bu adım veri blob'u için gerekli değildir). $dekurl geri yüklenen keyVault'tan alınabilir.

Aşağıdaki komut dosyasının yalnızca kaynak keyVault/secret kullanılamadığında yürütülmesi gerekir.

```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $encSetting = "{""encryptionEnabled"":true,""encryptionSettings"":[{""diskEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""secretUrl"":""$dekUrl""}}]}"
      $osBlobName = $obj.'properties.StorageProfile'.osDisk.name + ".vhd"
      $osBlob = Get-AzStorageBlob -Container $containerName -Blob $osBlobName
      $osBlob.ICloudBlob.Metadata["DiskEncryptionSettings"] = $encSetting
      $osBlob.ICloudBlob.SetMetadata()
```

**Sırlar kullanılabilir** ve şifreleme ayrıntıları da Işletim Sistemi Blob'da ayarlandıktan sonra, diskleri aşağıda verilen komut dosyasını kullanarak takın.

Kaynak keyVault/secrets zaten varsa, yukarıdaki komut dosyasının yürütülmesi gerekmez.

```powershell
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
      foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
      {
      $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
```

* **Azure AD'siz (BEK ve KEK) olmayan yönetilmeyen ve şifrelenmiş VM'ler** - Azure AD'si olmayan (BEK & KEK kullanılarak şifrelenmiş) yönetilmeyen, şifrelenmiş VM'ler için, kaynak **anahtarıVault/key/secret kullanılamıyorsa,** Azure [Yedekleme kurtarma noktasından şifrelenmemiş sanal makineyi geri yükleme](backup-azure-restore-key-secret.md)yordamını kullanarak anahtarı ve sırları anahtar kasasına geri yükleyin. Ardından, geri yüklenen işletim sistemi blob'unda şifreleme ayrıntılarını ayarlamak için aşağıdaki komut dosyalarını uygulayın (bu adım veri blob'u için gerekli değildir). $dekurl ve $kekurl geri yüklenen anahtarVault'tan alınabilir.

Aşağıdaki komut dosyasının yalnızca kaynak keyVault/key/secret kullanılamadığında yürütülmesi gerekir.

```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $encSetting = "{""encryptionEnabled"":true,""encryptionSettings"":[{""diskEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""secretUrl"":""$dekUrl""},""keyEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""keyUrl"":""$kekUrl""}}]}"
      $osBlobName = $obj.'properties.StorageProfile'.osDisk.name + ".vhd"
      $osBlob = Get-AzStorageBlob -Container $containerName -Blob $osBlobName
      $osBlob.ICloudBlob.Metadata["DiskEncryptionSettings"] = $encSetting
      $osBlob.ICloudBlob.SetMetadata()
```

**Anahtar/sırlar kullanılabilir** hale sunulduktan ve şifreleme ayrıntıları Işletim Sistemi Blob'da ayarlandıktan sonra, diskleri aşağıda verilen komut dosyasını kullanarak takın.

Kaynak anahtarıVault/key/secrets varsa, yukarıdaki komut dosyasının yürütülmesi gerekmez.

```powershell
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
      foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
      {
      $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
```

* **Yönetilen ve şifrelenmemiş VM'ler** - Yönetilen şifrelenmemiş VM'ler için geri yüklenen yönetilen diskleri takın. Ayrıntılı bilgi için bkz. [PowerShell kullanarak bir Windows VM'ye veri diski ekle.](../virtual-machines/windows/attach-disk-ps.md)

* **Azure AD ile yönetilen ve şifrelenmiş VM'ler (yalnızca BEK)** - Azure AD ile yönetilen şifreli VM'ler için (yalnızca BEK kullanılarak şifrelenmiş), geri yüklenen yönetilen diskleri takın. Ayrıntılı bilgi için bkz. [PowerShell kullanarak bir Windows VM'ye veri diski ekle.](../virtual-machines/windows/attach-disk-ps.md)

* **Azure AD (BEK ve KEK) ile yönetilen ve şifrelenmiş VM'ler** - Azure AD ile yönetilen şifreli VM'ler için (BEK ve KEK kullanılarak şifrelenmiş), geri yüklenen yönetilen diskleri takın. Ayrıntılı bilgi için bkz. [PowerShell kullanarak bir Windows VM'ye veri diski ekle.](../virtual-machines/windows/attach-disk-ps.md)

* **Azure AD'sız yönetilen ve şifrelenmiş Sanal Tom'lar (yalnızca BEK)** -Azure AD'sız yönetilen, şifrelenmiş VM'ler için (yalnızca BEK kullanılarak şifrelenmiş), kaynak **keyVault/secret kullanılamıyorsa,** [azure yedekleme kurtarma noktasından şifrelenmemiş bir sanal makineyi geri yükleme](backup-azure-restore-key-secret.md)yordamını kullanarak sırları anahtar kasasına geri yükleyin. Ardından, geri yüklenen işletim sistemi diskinde şifreleme ayrıntılarını ayarlamak için aşağıdaki komut dosyalarını uygulayın (bu adım veri diski için gerekli değildir). $dekurl geri yüklenen keyVault'tan alınabilir.

Aşağıdaki komut dosyasının yalnızca kaynak keyVault/secret kullanılamadığında yürütülmesi gerekir.  

```powershell
$dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
$keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
$diskupdateconfig = New-AzDiskUpdateConfig -EncryptionSettingsEnabled $true
$encryptionSettingsElement = New-Object Microsoft.Azure.Management.Compute.Models.EncryptionSettingsElement
$encryptionSettingsElement.DiskEncryptionKey = New-Object Microsoft.Azure.Management.Compute.Models.KeyVaultAndSecretReference
$encryptionSettingsElement.DiskEncryptionKey.SourceVault = New-Object Microsoft.Azure.Management.Compute.Models.SourceVault
$encryptionSettingsElement.DiskEncryptionKey.SourceVault.Id = $keyVaultId
$encryptionSettingsElement.DiskEncryptionKey.SecretUrl = $dekUrl
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettings = New-Object System.Collections.Generic.List[Microsoft.Azure.Management.Compute.Models.EncryptionSettingsElement]
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettings.Add($encryptionSettingsElement)
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettingsVersion = "1.1"
Update-AzDisk -ResourceGroupName "testvault" -DiskName $obj.'properties.StorageProfile'.osDisk.name -DiskUpdate $diskupdateconfig
```

Sırlar kullanılabilir hale geldikten ve şifreleme ayrıntıları işletim sistemi diskinde ayarlandıktan sonra, geri yüklenen yönetilen diskleri eklemek [için powershell kullanarak bir Windows VM'ye veri diski ekleme'ye](../virtual-machines/windows/attach-disk-ps.md)bakın.

* **Azure AD olmadan yönetilen ve şifrelenmiş Sanal M'ler (BEK ve KEK)** - Azure AD'sız yönetilen, şifrelenmiş VM'ler için (BEK & KEK kullanılarak şifrelenmiş), kaynak **anahtarıVault/key/secret kullanılamıyorsa,** [azure yedekleme kurtarma noktasından şifrelenmemiş bir sanal makineyi geri yükleme](backup-azure-restore-key-secret.md)yordamını kullanarak anahtarı ve sırları anahtar kasasına geri yükleyin. Ardından, geri yüklenen işletim sistemi diskinde şifreleme ayrıntılarını ayarlamak için aşağıdaki komut dosyalarını uygulayın (bu adım veri diskleri için gerekli değildir). $dekurl ve $kekurl geri yüklenen anahtarVault'tan alınabilir.

Aşağıdaki komut dosyasının yalnızca kaynak keyVault/key/secret kullanılamadığında yürütülmesi gerekir.

```powershell
$dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
$kekUrl = "https://ContosoKeyVault.vault.azure.net/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
$keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
$diskupdateconfig = New-AzDiskUpdateConfig -EncryptionSettingsEnabled $true
$encryptionSettingsElement = New-Object Microsoft.Azure.Management.Compute.Models.EncryptionSettingsElement
$encryptionSettingsElement.DiskEncryptionKey = New-Object Microsoft.Azure.Management.Compute.Models.KeyVaultAndSecretReference
$encryptionSettingsElement.DiskEncryptionKey.SourceVault = New-Object Microsoft.Azure.Management.Compute.Models.SourceVault
$encryptionSettingsElement.DiskEncryptionKey.SourceVault.Id = $keyVaultId
$encryptionSettingsElement.DiskEncryptionKey.SecretUrl = $dekUrl
$encryptionSettingsElement.KeyEncryptionKey = New-Object Microsoft.Azure.Management.Compute.Models.KeyVaultAndKeyReference
$encryptionSettingsElement.KeyEncryptionKey.SourceVault = New-Object Microsoft.Azure.Management.Compute.Models.SourceVault
$encryptionSettingsElement.KeyEncryptionKey.SourceVault.Id = $keyVaultId
$encryptionSettingsElement.KeyEncryptionKey.KeyUrl = $kekUrl
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettings = New-Object System.Collections.Generic.List[Microsoft.Azure.Management.Compute.Models.EncryptionSettingsElement]
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettings.Add($encryptionSettingsElement)
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettingsVersion = "1.1"
Update-AzDisk -ResourceGroupName "testvault" -DiskName $obj.'properties.StorageProfile'.osDisk.name -DiskUpdate $diskupdateconfig
```

Anahtar/sırlar kullanılabilir hale geldikten ve şifreleme ayrıntıları işletim sistemi diskinde ayarlandıktan sonra, geri yüklenen yönetilen diskleri takmak için [PowerShell kullanarak bir Windows VM'ye veri diski ekleme'ye](../virtual-machines/windows/attach-disk-ps.md)bakın.

5. Ağ ayarlarını ayarlayın.

    ```powershell
    $nicName="p1234"
    $pip = New-AzPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    $virtualNetwork | Set-AzVirtualNetwork
    $vnet = Get-AzVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    $subnetindex=0
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    $vm=Add-AzVMNetworkInterface -VM $vm -Id $nic.Id
    ```

6. Sanal makineyi oluşturun.

    ```powershell  
    New-AzVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

7. ADE uzantısını itin.
   ADE uzantıları itilmezse, veri diskleri şifrelenmemiş olarak işaretlenir, bu nedenle aşağıdaki adımların yürütülmesi zorunludur:

   * **Azure AD'li VM** için - Veri diskleri için şifrelemeyi el ile etkinleştirmek için aşağıdaki komutu kullanın  

     **Sadece BEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm.Name -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

     **BEK ve KEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm.Name -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId  -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

   * **Azure AD'si olmayan VM** için - Veri diskleri için şifrelemeyi el ile etkinleştirmek için aşağıdaki komutu kullanın.

     Komut yürütme sırasında AADClientID sorarsa, Azure PowerShell'inizi güncelleştirmeniz gerekir.

     **Sadece BEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm.Name -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -SkipVmBackup -VolumeType "All"
      ```

      **BEK ve KEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm.Name -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -SkipVmBackup -VolumeType "All"
      ```

> [!NOTE]
> Şifreli VM geri yükleme disk işleminin bir sAğlık sınır

## <a name="restore-files-from-an-azure-vm-backup"></a>Azure VM yedeklemesinden dosyaları geri yükleme

Diskleri geri yüklemenin yanı sıra, azure VM yedeklemesinden tek tek dosyaları geri yükleyebilirsiniz. Dosyaları geri yükleme işlevi, kurtarma noktasındaki tüm dosyalara erişim sağlar. Dosyaları normal dosyalarda olduğu gibi Dosya Gezgini üzerinden yönetin.

Azure VM yedeklemesinden dosyayı geri yüklemek için gereken temel adımlar şunlardır:

* VM'yi seçin
* Kurtarma noktası seçin
* Kurtarma noktasının disklerini monte edin
* Gerekli dosyaları kopyalama
* Diski sökme

### <a name="select-the-vm"></a>VM'yi seçin

Doğru yedekleme öğesini tanımlayan PowerShell nesnesini almak için kasadaki kapsayıcıdan başlayın ve nesne hiyerarşisi aşağı doğru şekilde çalışın. VM'yi temsil eden kapsayıcıyı seçmek için [Get-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer) cmdlet ve pipe'ı [get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem) cmdlet'e kullanın.

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM" -VaultId $targetVault.ID
$backupitem = Get-AzRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM" -VaultId $targetVault.ID
```

### <a name="choose-a-recovery-point"></a>Kurtarma noktası seçin

Yedek öğenin tüm kurtarma noktalarını listelemek için [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint) cmdlet'i kullanın. Sonra geri yüklemek için kurtarma noktasını seçin. Hangi kurtarma noktasının kullanılacağından emin değilseniz, listedeki en son RecoveryPointType = AppConsistent noktasını seçmek iyi bir uygulamadır.

Aşağıdaki komut dosyasında, **$rp**değişken, seçilen yedekleme öğesi için son yedi gün içinde bir kurtarma noktası dizisidir. Dizi, dizin 0'daki en son kurtarma noktasıyla ters sırayla sıralanır. Kurtarma noktasını seçmek için standart PowerShell dizi dizi dizi dizisini kullanın. Örnekte, $rp[0] en son kurtarma noktasını seçer.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime() -VaultId $targetVault.ID
$rp[0]
```

Çıktı aşağıdaki örneğe benzer:

```output
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="mount-the-disks-of-recovery-point"></a>Kurtarma noktasının disklerini monte edin

Kurtarma noktasının tüm disklerini monte etmek için komut dosyası almak için [Get-AzRecoveryServicesBackupRPMountScript](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprpmountscript) cmdlet kullanın.

> [!NOTE]
> Diskler, komut dosyasının çalıştırıldığı makineye iSCSI ekli diskler olarak monte edilir. Montaj hemen gerçekleşir ve herhangi bir ücrete tabi değildir.
>
>

```powershell
Get-AzRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0] -VaultId $targetVault.ID
```

Çıktı aşağıdaki örneğe benzer:

```output
OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```

Dosyaları kurtarmak istediğiniz makinede komut dosyasını çalıştırın. Komut dosyasını yürütmek için sağlanan parolayı girmeniz gerekir. Diskler bağlandıktan sonra, yeni birimlere ve dosyalara göz atmak için Windows Dosya Gezgini'ni kullanın. Daha fazla bilgi için, Yedekleme makalesine bakın, [Dosyaları Azure sanal makine yedeklemesinden kurtarın.](backup-azure-restore-files-from-vm.md)

### <a name="unmount-the-disks"></a>Disklerin montajı

Gerekli dosyalar kopyalandıktan sonra diskleri sökmek için [Devre Dışı-AzRecoveryServicesBackupRPMountScript'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackuprpmountscript) kullanın. Kurtarma noktasının dosyalarına erişimkaldırılmak için diskleri kaldırdığından emin olun.

```powershell
Disable-AzRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0] -VaultId $targetVault.ID
```

## <a name="next-steps"></a>Sonraki adımlar

Azure kaynaklarınız ile etkileşimde bulunmak için PowerShell'i kullanmayı tercih ediyorsanız, Windows Server için PowerShell makalesine bakın, [Windows Server için Yedeklemeyi Dağıtın ve Yönetin.](backup-client-automation.md) DPM yedeklemelerini yönetiyorsanız, [dpm için makaleyi, Dağıtımı ve DPM için Yedeklemeyi Yönet'e](backup-dpm-automation.md)bakın.
