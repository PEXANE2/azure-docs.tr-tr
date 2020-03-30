---
title: Azure VM yedeklemesinde SQL DB, PowerShell üzerinden geri &
description: Azure Yedekleme ve PowerShell'i kullanarak Azure VM'lerde SQL Veritabanlarını yedekleyin ve geri yükleyin.
ms.topic: conceptual
ms.date: 03/15/2019
ms.assetid: 57854626-91f9-4677-b6a2-5d12b6a866e1
ms.openlocfilehash: 9608b02869b1d41d901ec77a42cfaa6d882040e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131822"
---
# <a name="back-up-and-restore-sql-databases-in-azure-vms-with-powershell"></a>PowerShell ile Azure VM'lerde SQL veritabanlarını yedekleme ve geri yükleme

Bu makalede, [Azure Yedekleme](backup-overview.md) Kurtarma Hizmetleri kasasını kullanarak Bir Azure VM'de bir SQL DB'yi yedeklemek ve kurtarmak için Azure PowerShell'in nasıl kullanılacağı açıklanmaktadır.

Bu makalede, nasıl açıklanmaktadır:

> [!div class="checklist"]
>
> * PowerShell'i ayarlayın ve Azure Kurtarma Hizmetleri Sağlayıcısı'na kaydolun.
> * Kurtarma Hizmetleri kasası oluşturun.
> * Bir Azure VM içinde SQL DB için yedekleme yapılandırma.
> * Yedek bir iş çalıştırın.
> * Yedeklenmiş bir SQL DB'yi geri yükleyin.
> * Yedeklemeyi izleyin ve işleri geri yükleyin.

## <a name="before-you-start"></a>Başlamadan önce

* Kurtarma Hizmetleri kasaları hakkında [daha fazla bilgi edinin.](backup-azure-recovery-services-vault-overview.md)
* [Azure VM'lerde SQL DB'leri yedeklemek](backup-azure-sql-database.md#before-you-start)için özellik özellikleri hakkında bilgi edinin.
* Kurtarma Hizmetleri için PowerShell nesne hiyerarşisini gözden geçirin.

### <a name="recovery-services-object-hierarchy"></a>Kurtarma Hizmetleri nesne hiyerarşisi

Nesne hiyerarşisi aşağıdaki diyagramda özetlenmiştir.

![Kurtarma Hizmetleri nesne hiyerarşisi](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Azure kitaplığındaki **Az.RecoveryServices** [cmdlet başvuru](/powershell/module/az.recoveryservices) başvurularını gözden geçirin.

### <a name="set-up-and-install"></a>Kurulum ve yükleme

PowerShell'i aşağıdaki gibi ayarlayın:

1. [Az PowerShell en son sürümünü indirin.](/powershell/azure/install-az-ps) Gerekli minimum sürüm 1.5.0'dır.

2. Bu komutla Azure Backup PowerShell cmdlets'i bulun:

    ```powershell
    Get-Command *azrecoveryservices*
    ```

3. Azure Yedekleme ve Kurtarma Hizmetleri kasası için takma adları ve cmdlets'i gözden geçirin. İşte göreebileceklerinizin bir örneği. Cmdlet'lerin tam listesi değil.

    ![Kurtarma Hizmetleri cmdlets listesi](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

4. **Connect-AzAccount**ile Azure hesabınızda oturum açın.
5. Görünen web sayfasında, hesap kimlik bilgilerinizi girdiniz istenir.

    * Alternatif olarak, **-Kimlik**bilgileri ile **Connect-AzAccount** cmdlet'e hesap kimlik bilgilerinizi parametre olarak ekleyebilirsiniz.
    * Kiracı için çalışan bir CSP iş ortağıysanız, kiracı kimliğini veya birincil etki alanı adını kullanarak müşteriyi kiracı olarak belirtin. Bir örnek **Connect-AzAccount -Kiracı** fabrikam.com.

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

Kurtarma Hizmetleri kasası oluşturmak için aşağıdaki adımları izleyin.

Kurtarma Hizmetleri kasası bir Kaynak Yöneticisi kaynağıdır, bu nedenle kaynak grubuna yerleştirmeniz gerekir. Varolan bir kaynak grubunu kullanabilir veya Yeni Kaynak **Grubu** cmdlet'ini içeren bir kaynak grubu oluşturabilirsiniz. Bir kaynak grubu oluşturduğunuzda, kaynak grubunun adını ve konumunu belirtin.

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
    * Aşağıdaki örnek, **GeoRedundant**olarak ayarlanmış **testvault** için[Set-AzRecoveryServicesBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupproperty) cmd için **-BackupStorageRedundancy** seçeneğini ayarlar.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

### <a name="view-the-vaults-in-a-subscription"></a>Abonelikteki kasaları görüntüleme

Abonelikteki tüm kasaları görüntülemek için [Get-AzRecoveryServicesVault'u](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0)kullanın.

```powershell
Get-AzRecoveryServicesVault
```

Çıktı aşağıdakine benzer. İlişkili kaynak grubu ve konumu sağlanır.

```output
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

Azure PowerShell yönergelerine uygun olarak kasa bağlam ayarını küçümsemeyi planlıyoruz. Bunun yerine, kasa kimliğini saklayabilir veya getirebilirsiniz ve aşağıdaki gibi ilgili komutlara geçirebilirsiniz:

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-a-backup-policy"></a>Yedekleme ilkesini yapılandırma

Yedekleme ilkesi, yedekleme zamanlamasını ve yedekleme kurtarma noktalarının ne kadar süreyle tutulması gerektiğini belirtir:

* Yedekleme ilkesi en az bir bekletme ilkesiyle ilişkilidir. Bekletme ilkesi, kurtarma noktasının silinmeden önce ne kadar süreyle tutulduğunu tanımlar.
* [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject?view=azps-1.4.0)kullanarak varsayılan yedekleme ilkesi tutma görüntüleyin.
* [Get-AzRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject?view=azps-1.4.0)kullanarak varsayılan yedekleme ilkesi zamanlamasını görüntüleyin.
* Yeni bir yedekleme ilkesi oluşturmak için [Yeni AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy?view=azps-1.4.0) cmdlet'i kullanırsınız. Zamanlama ve bekletme ilkesi nesnelerini girdiniz.

Varsayılan olarak, Zamanlama İlkesi Nesnesi'nde bir başlangıç zamanı tanımlanır. Başlangıç saatini istenilen başlangıç saatine değiştirmek için aşağıdaki örneği kullanın. İstenilen başlangıç süresi UTC'de de olmalıdır. Aşağıdaki örnekte, günlük yedeklemeler için istenen başlangıç saatinin 01:00 UTC olduğu varsayar.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "MSSQL"
$UtcTime = Get-Date -Date "2019-03-20 01:30:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> Başlangıç saatini yalnızca 30 dakikalık katlar olarak sağlamanız gerekir. Yukarıdaki örnekte, yalnızca "01:00:00" veya "02:30:00" olabilir. Başlangıç saati "01:15:00" olamaz

Aşağıdaki örnek, zamanlama ilkesini ve bekletme ilkesini değişkenlerde saklar. Daha sonra bu değişkenleri yeni bir ilke **(NewSQLPolicy)** için parametre olarak kullanır. **NewSQLPolicy** günlük "Tam" yedekleme alır, 180 gün boyunca saklar ve her 2 saatte bir günlük yedekleme alır

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "MSSQL"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "MSSQL"
$NewSQLPolicy = New-AzRecoveryServicesBackupProtectionPolicy -Name "NewSQLPolicy" -WorkloadType "MSSQL" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

Çıktı aşağıdakine benzer.

```output
Name                 WorkloadType       BackupManagementType BackupTime                Frequency                                IsDifferentialBackup IsLogBackupEnabled
                                                                                                                                Enabled
----                 ------------       -------------------- ----------                ---------                                -------------------- ------------------
NewSQLPolicy         MSSQL              AzureWorkload        3/15/2019 01:30:00 AM      Daily                                    False                True
```

## <a name="enable-backup"></a>Yedeklemeyi etkinleştirme

### <a name="registering-the-sql-vm"></a>SQL VM'nin kaydedilmesi

Azure VM yedeklemeleri ve Azure Dosyası paylaşımları için Yedekleme hizmeti bu Azure Kaynak Yöneticisi kaynaklarına bağlanabilir ve ilgili ayrıntıları alabilir. SQL bir Azure VM içinde bir uygulama olduğundan, Yedekleme hizmetinin uygulamaya erişmek ve gerekli ayrıntıları almak için izin alması gerekir. Bunu yapmak için, Kurtarma hizmetleri kasası ile SQL uygulamasını içeren Azure VM'yi *'kaydetmeniz'* gerekir. Bir SQL VM'yi kasayla kaydettirdikten sonra, SQL DB'leri yalnızca bu kasaya koruyabilirsiniz. VM'yi kaydetmek için [Register-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/Register-AzRecoveryServicesBackupContainer?view=azps-1.5.0) PS cmdlet kullanın.

```powershell
 $myVM = Get-AzVM -ResourceGroupName <VMRG Name> -Name <VMName>
Register-AzRecoveryServicesBackupContainer -ResourceId $myVM.ID -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID -Force
```

Komut bu kaynağın bir 'yedek kapsayıcı' döndürecek ve durum 'kayıtlı' olacak

> [!NOTE]
> Kuvvet parametresi verilmezse, kullanıcıdan 'Bu kapsayıcı için korumayı devre dışı kılmış olmak istiyor musunuz' metniyle onaylaması istenir. Lütfen bu metni yoksayın ve onaylamak için "Y" deyin. Bu bilinen bir sorundur ve biz metin ve kuvvet parametresi için gereksinimi kaldırmak için çalışıyoruz.

### <a name="fetching-sql-dbs"></a>SQL DB'leri Alma

Kayıt yapıldıktan sonra, Yedekleme hizmeti VM içindeki tüm kullanılabilir SQL bileşenlerini listeleyebilecektir. Henüz bu kasaya yedeklenecek tüm SQL bileşenlerini görüntülemek için [Get-AzRecoveryServicesBackupProtectableItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS cmdlet kullanın

```powershell
Get-AzRecoveryServicesBackupProtectableItem -WorkloadType MSSQL -VaultId $targetVault.ID
```

Çıktı, bu kasaya kaydedilmiş tüm SQL VM'lerde Item Type ve ServerName ile tüm korumasız SQL bileşenlerini gösterir. '-Kapsayıcı' parametresini geçerek belirli bir SQL VM'ye daha fazla filtre uygulayabilir veya benzersiz bir SQL öğesine ulaşmak için ItemType bayrağıyla birlikte 'Ad' ve 'ServerName' birleşimini kullanabilirsiniz.

```powershell
$SQLDB = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLDataBase -VaultId $targetVault.ID -Name "<Item Name>" -ServerName "<Server Name>"
```

### <a name="configuring-backup"></a>Yedeklemeyi yapılandırma

Artık gerekli SQL DB'ye ve yedeklenmesi gereken ilkeye sahip olduğumuza göre, bu SQL DB için yedekleme yapılandırmak için [Enable-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) cmdlet'i kullanabiliriz.

```output
Enable-AzRecoveryServicesBackupProtection -ProtectableItem $SQLDB -Policy $NewSQLPolicy
```

Komut, yapılandırma yedeklemesi tamamlanana kadar bekler ve aşağıdaki çıktıyı döndürür.

```output
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
master           ConfigureBackup      Completed            3/18/2019 6:00:21 PM      3/18/2019 6:01:35 PM      654e8aa2-4096-402b-b5a9-e5e71a496c4e
```

### <a name="fetching-new-sql-dbs"></a>Yeni SQL DB'leri alma

Makine kaydedildikten sonra, Yedekleme hizmeti kullanılabilir DB'lerin ayrıntılarını getirir. Kullanıcı daha sonra kayıtlı makineye SQL DBs/SQL örnekleri eklerse, tüm korumasız DB'leri (yeni eklenenler dahil) yeniden almak için yeni bir 'sorgulama' gerçekleştirmek için yedekleme hizmetini el ile tetiklemenin gerekir. Yeni bir sorgulama yapmak için SQL VM'deki [Initialize-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Initialize-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS cmdlet'ini kullanın. Komut, işlem tamamlanana kadar bekler. Daha sonra en son korumasız SQL bileşenlerinin listesini almak için [Get-AzRecoveryServicesBackupProtectableItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupProtectableItem?view=azps-1.5.0) PS cmdlet kullanın

```powershell
$SQLContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName <VM name> -VaultId $targetvault.ID
Initialize-AzRecoveryServicesBackupProtectableItem -Container $SQLContainer -WorkloadType MSSQL -VaultId $targetvault.ID
Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLDataBase -VaultId $targetVault.ID
```

İlgili korunabilir öğeler alındıktan sonra, [yukarıdaki bölümde](#configuring-backup)belirtildiği gibi yedekleri etkinleştirin.
Bir el ile yeni DBs algılamak istemiyorsanız, [onlar aşağıda](#enable-autoprotection)açıklandığı gibi otomatik koruma için tercih edebilirsiniz.

## <a name="enable-autoprotection"></a>Otomatik korumayı etkinleştirme

Kullanıcı, gelecekte eklenen tüm DB'ler belirli bir ilke yle otomatik olarak korunacak şekilde yedeklemeyi yapılandırabilir. Otomatik korumayı etkinleştirmek için [Enable-AzRecoveryServicesBackupAutoProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupAutoProtection?view=azps-1.5.0) PS cmdlet kullanın.

Yönerge gelecekteki tüm DB'leri yedeklemek olduğundan, işlem SQLInstance düzeyinde yapılır.

```powershell
$SQLInstance = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLInstance -VaultId $targetVault.ID -Name "<Protectable Item name>" -ServerName "<Server Name>"
Enable-AzRecoveryServicesBackupAutoProtection -InputItem $SQLInstance -BackupManagementType AzureWorkload -WorkloadType MSSQL -Policy $NewSQLPolicy -VaultId $targetvault.ID
```

Otomatik koruma amacı verildikten sonra, yeni eklenen DB'leri getirmek için makineye yapılan sorgulama her 8 saatte bir planlanmış bir arka plan görevi olarak gerçekleşir.

## <a name="restore-sql-dbs"></a>SQL DB'leri Geri Yükleme

Azure Yedekleme, Azure VM'lerinde çalışan SQL Server veritabanlarını aşağıdaki gibi geri yükleyebilir:

* İşlem günlüğü yedeklemelerini kullanarak belirli bir tarihe veya saate (ikinciye) geri yükleyin. Azure Yedekleme, uygun tam diferansiyel yedeklemeyi ve seçilen zamana bağlı olarak geri yüklemesi gereken günlük yedeklemeleri zincirini otomatik olarak belirler.
* Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya diferansiyel yedekleme geri yükleyin.

SQL DB'leri geri almadan önce [burada](restore-sql-database-azure-vm.md#prerequisites) belirtilen ön koşulları kontrol edin.

Önce [Get-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS cmdlet kullanarak ilgili yedeklenen SQL DB getirin.

```powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
```

### <a name="fetch-the-relevant-restore-time"></a>İlgili geri yükleme süresini getirin

Yukarıda belirtildiği gibi, kullanıcı yedeklenmiş SQL DB'yi tam/diferansiyel kopya **veya** zamanında bir günlük noktasına geri yükleyebilir.

#### <a name="fetch-distinct-recovery-points"></a>Farklı kurtarma noktaları getir

Yedeklenmiş bir SQL DB için farklı (Tam/diferansiyel) kurtarma noktaları getirmek için [Get-AzRecoveryServicesBackupRecoveryPoint'i](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupRecoveryPoint?view=azps-1.5.0) kullanın.

```powershell
$startDate = (Get-Date).AddDays(-7).ToUniversalTime()
$endDate = (Get-Date).ToUniversalTime()
Get-AzRecoveryServicesBackupRecoveryPoint -Item $bkpItem -VaultId $targetVault.ID -StartDate $startdate -EndDate $endDate
```

Çıktı aşağıdaki örneğe benzer

```output
RecoveryPointId    RecoveryPointType  RecoveryPointTime      ItemName                             BackupManagemen
                                                                                                  tType
---------------    -----------------  -----------------      --------                             ---------------
6660368097802      Full               3/18/2019 8:09:35 PM   MSSQLSERVER;model             AzureWorkload
```

İlgili kurtarma noktasını almak için 'RecoveryPointId' filtresini veya dizi filtresini kullanın.

```powershell
$FullRP = Get-AzRecoveryServicesBackupRecoveryPoint -Item $bkpItem -VaultId $targetVault.ID -RecoveryPointId "6660368097802"
```

#### <a name="fetch-point-in-time-recovery-point"></a>Zaman noktası kurtarma noktasını getir

Kullanıcı DB'yi belirli bir zaman diliminde geri yüklemek istiyorsa [Get-AzRecoveryServicesBackupRecoveryLogChain](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupRecoveryLogChain?view=azps-1.5.0) PS cmdlet'i kullanın. Cmdlet, bu SQL yedekleme öğesi için kesintisiz, sürekli günlük zincirinin başlangıç ve bitiş saatlerini gösteren tarihlerin listesini döndürür. İstenilen zaman dilimi bu aralıkta olmalıdır.

```powershell
Get-AzRecoveryServicesBackupRecoveryLogChain -Item $bkpItem -Item -VaultId $targetVault.ID
```

Çıktı aşağıdaki örneğe benzer olacaktır.

```output
ItemName                       StartTime                      EndTime
--------                       ---------                      -------
SQLDataBase;MSSQLSERVER;azu... 3/18/2019 8:09:35 PM           3/19/2019 12:08:32 PM
```

Yukarıdaki çıktı, kullanıcının görüntülenen başlangıç saati ile bitiş saati arasındaki herhangi bir zaman diliminde geri yükleyebileceği anlamına gelir. Zaman UTC bulunmaktadır. PS'de yukarıda gösterilen aralıkta herhangi bir zaman diliminde oluşturma.

> [!NOTE]
> Geri yükleme için bir giriş noktası-in-time seçildiğinde, kullanıcı başlangıç noktası yani, DB geri yüklenir tam yedekleme belirtmeniz gerekmez. Azure Yedekleme hizmeti, hangi tam yedeklemenin seçilebilen, hangi günlük yedeklemelerinin uygulanacağı vb. tüm kurtarma planını,

### <a name="determine-recovery-configuration"></a>Kurtarma yapılandırmasının belirlenmesi

SQL DB geri yüklemesi durumunda, aşağıdaki geri yükleme senaryoları desteklenir.

* Yedeklenmiş SQL DB'yi başka bir kurtarma noktasından gelen verilerle geçersiz kılma - OriginalWorkloadRestore
* SQL DB'yi aynı SQL örneğinde yeni bir DB olarak geri yükleme - AlternateWorkloadRestore
* Başka bir SQL VM'de başka bir SQL örneğinde SQL DB'yi yeni bir DB olarak geri yükleme - Alternatif İş YüküGeri Yükleme
* SQL DB'yi .bak dosyaları olarak geri yükleme -RestoreAsFiles

İlgili kurtarma noktasını (farklı veya giriş noktası zamanında) aldıktan sonra, [get-AzRecoveryServicesBackupWorkloadRecoveryConfig](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupWorkloadRecoveryConfig?view=azps-1.5.0) PS cmdlet kullanarak kurtarma config nesnesini istenilen kurtarma planına göre getirin.

#### <a name="original-workload-restore"></a>Özgün iş yükü geri yüklemesi

Yedeklenmiş DB'yi kurtarma noktasından gelen verilerle geçersiz kılmak için, aşağıdaki örnekte gösterildiği gibi doğru bayrağı ve ilgili kurtarma noktasını belirtmeniz yeterli dir.

##### <a name="original-restore-with-distinct-recovery-point"></a>Farklı Kurtarma noktası ile özgün geri yükleme

```powershell
$OverwriteWithFullConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -RecoveryPoint $FullRP -OriginalWorkloadRestore -VaultId $targetVault.ID
```

##### <a name="original-restore-with-log-point-in-time"></a>Giriş noktası zamanında orijinal geri yükleme

```powershell
$OverwriteWithLogConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -PointInTime $PointInTime -Item $bkpItem  -OriginalWorkloadRestore -VaultId $targetVault.ID
```

#### <a name="alternate-workload-restore"></a>Alternatif iş yükü geri yüklemesi

> [!IMPORTANT]
> Yedeklenmiş bir SQL DB, bu kasaya 'kayıtlı' bir Azure VM'de yalnızca başka bir SQLInstance'a yeni bir DB olarak geri yüklenebilir.

Yukarıda belirtildiği gibi, hedef SQLInstance başka bir Azure VM içinde yatıyorsa, [bu kasaya kayıtlı](#registering-the-sql-vm) olduğundan ve ilgili SQLInstance'ın korunabilir bir öğe olarak göründüğünden emin olun.

```powershell
$TargetInstance = Get-AzRecoveryServicesBackupProtectableItem -WorkloadType MSSQL -ItemType SQLInstance -Name "<SQLInstance Name>" -ServerName "<SQL VM name>" -VaultId $targetVault.ID
```

Sonra sadece ilgili kurtarma noktası geçmek, hedef SQL örnek sağ bayrak ile aşağıda gösterildiği gibi.

##### <a name="alternate-restore-with-distinct-recovery-point"></a>Farklı Kurtarma noktasıyla alternatif geri yükleme

```powershell
$AnotherInstanceWithFullConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -RecoveryPoint $FullRP -TargetItem $TargetInstance -AlternateWorkloadRestore -VaultId $targetVault.ID
```

##### <a name="alternate-restore-with-log-point-in-time"></a>Giriş noktası zamanında ile alternatif geri yükleme

```powershell
$AnotherInstanceWithLogConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -PointInTime $PointInTime -Item $bkpItem -AlternateWorkloadRestore -VaultId $targetVault.ID
```

##### <a name="restore-as-files"></a>Dosya Olarak Geri Yükleme

Yedekleme verilerini veritabanı yerine .bak dosyaları olarak geri yüklemek için **Dosyalar olarak Geri Yükle** seçeneğini belirleyin. Yedeklenmiş SQL DB, bu kasaya kayıtlı herhangi bir hedef VM'ye geri yüklenebilir.

```powershell
$TargetContainer= Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName "VM name" -VaultId $vaultID
```

##### <a name="restore-as-files-with-distinct-recovery-point"></a>Farklı Kurtarma noktası olan dosya olarak geri yükleme

```powershell
$FileRestoreWithFullConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -RecoveryPoint $FullRP -TargetContainer $TargetContainer -RestoreAsFiles -FilePath "<>" -VaultId $targetVault.ID
```

##### <a name="restore-as-files-with-log-point-in-time-from-latest-full"></a>En son tam dan giriş noktası zamanında dosyaları olarak geri yükleme

```powershell
$FileRestoreWithLogConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -PointInTime $PointInTime -TargetContainer $TargetContainer -RestoreAsFiles -FilePath "<>" -VaultId $targetVault.ID
```

##### <a name="restore-as-files-with-log-point-in-time-from-a-specified-full"></a>Belirli bir tam oturum açma noktasında olan dosyalar olarak geri yükleme

Geri yükleme için kullanılması gereken belirli bir tam vermek istiyorsanız, aşağıdaki komutu kullanın:

```powershell
$FileRestoreWithLogAndSpecificFullConfig = Get-AzRecoveryServicesBackupWorkloadRecoveryConfig -PointInTime $PointInTime -FromFull $FullRP -TargetContainer $TargetContainer -RestoreAsFiles -FilePath "<>" -VaultId $targetVault.ID
```

[Get-AzRecoveryServicesBackupWorkloadRecoveryConfig](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupWorkloadRecoveryConfig?view=azps-1.5.0) PS cmdlet elde edilen son kurtarma noktası yapılandırma nesnegeri yükleme için tüm ilgili bilgilere sahiptir ve aşağıda gösterildiği gibi.

```output
TargetServer         : <SQL server name>
TargetInstance       : <Target Instance name>
RestoredDBName       : <Target Instance name>/azurebackup1_restored_3_19_2019_1850
OverwriteWLIfpresent : No
NoRecoveryMode       : Disabled
targetPhysicalPath   : {azurebackup1, azurebackup1_log}
ContainerId          : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/vmappcontainer;compute;computeRG;SQLVMName
SourceResourceId     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/computeRG/VMAppContainer/SQLVMName
RestoreRequestType   : Alternate WL Restore
RecoveryPoint        : Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureWorkloadRecoveryPoint
PointInTime          : 1/1/0001 12:00:00 AM
```

Geri yüklenen DB adını, OverwriteWLIfpresent'ı, NoRecoveryMode'u ve targetPhysicalPath alanlarını düzenleyebilirsiniz. Hedef dosya yolları için aşağıda gösterildiği gibi daha fazla ayrıntı alın.

```powershell
$AnotherInstanceWithFullConfig.targetPhysicalPath
```

```output
MappingType SourceLogicalName SourcePath                  TargetPath
----------- ----------------- ----------                  ----------
Data        azurebackup1      F:\Data\azurebackup1.mdf    F:\Data\azurebackup1_1553001753.mdf
Log         azurebackup1_log  F:\Log\azurebackup1_log.ldf F:\Log\azurebackup1_log_1553001753.ldf
```

İlgili PS özelliklerini aşağıda gösterildiği gibi dize değerleri olarak ayarlayın.

```powershell
$AnotherInstanceWithFullConfig.OverwriteWLIfpresent = "Yes"
$AnotherInstanceWithFullConfig | fl
```

```output
TargetServer         : <SQL server name>
TargetInstance       : <Target Instance name>
RestoredDBName       : <Target Instance name>/azurebackup1_restored_3_19_2019_1850
OverwriteWLIfpresent : Yes
NoRecoveryMode       : Disabled
targetPhysicalPath   : {azurebackup1, azurebackup1_log}
ContainerId          : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/vmappcontainer;compute;computeRG;SQLVMName
SourceResourceId     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/computeRG/VMAppContainer/SQLVMName
RestoreRequestType   : Alternate WL Restore
RecoveryPoint        : Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureWorkloadRecoveryPoint
PointInTime          : 1/1/0001 12:00:00 AM
```

> [!IMPORTANT]
> Geri yükleme işlemi config nesnesini temel alacağından, son kurtarma config nesnesinin gerekli ve uygun değerlere sahip olduğundan emin olun.

### <a name="restore-with-relevant-configuration"></a>İlgili yapılandırmayla geri yükleme

İlgili kurtarma Config nesnesi elde edildikten ve doğrulandıktan sonra, geri yükleme işlemini başlatmak için [Geri Yükleme-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Restore-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS cmdlet'i kullanın.

```powershell
Restore-AzRecoveryServicesBackupItem -WLRecoveryConfig $AnotherInstanceWithLogConfig -VaultId $targetVault.ID
```

Geri yükleme işlemi izlenecek bir işi döndürür.

```output
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
MSSQLSERVER/m... Restore              InProgress           3/17/2019 10:02:45 AM                                3274xg2b-e4fg-5952-89b4-8cb566gc1748
```

## <a name="manage-sql-backups"></a>SQL yedeklemelerini yönetme

### <a name="on-demand-backup"></a>İsteğe bağlı yedekleme

Bir DB için yedekleme etkinleştirildikten sonra, kullanıcı [Yedekleme-AzRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/az.recoveryservices/Backup-AzRecoveryServicesBackupItem?view=azps-1.5.0) PS cmdlet kullanarak DB için isteğe bağlı yedeklemeyi de tetikleyebilir. Aşağıdaki örnek, sıkıştırma etkin leştirilmiş bir SQL DB'de tam yedeklemeyi tetikler ve tam yedekleme 60 gün boyunca saklanmalıdır.

```powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
$endDate = (Get-Date).AddDays(60).ToUniversalTime()
Backup-AzRecoveryServicesBackupItem -Item $bkpItem -BackupType Full -EnableCompression -VaultId $targetVault.ID -ExpiryDateTimeUTC $endDate
```

İsteğe bağlı yedekleme komutu izlenecek bir işi döndürür.

```output
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
MSSQLSERVER/m... Backup               InProgress           3/18/2019 8:41:27 PM                                2516bb1a-d3ef-4841-97a3-9ba455fb0637
```

Çıktı kaybolursa veya ilgili İş Kimliğini almak istiyorsanız, Azure Yedekleme [hizmetinden işlerin listesini alın](#track-azure-backup-jobs) ve ardından bu bilgileri ve ayrıntılarını izleyin.

### <a name="change-policy-for-backup-items"></a>Yedekleme öğeleri için ilkeyi değiştirme

Kullanıcı, varolan ilkeyi değiştirebilir veya yedeklenmiş öğenin ilkesini İlke1'den İlke2'ye değiştirebilir. Yedeklenmiş bir öğe için ilkegeçiş yapmak için, ilgili ilkeyi [Enable-AzRecoveryServices](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) getirin ve öğeyi yedekleyin ve yedek öğeyi parametre olarak yedek öğeyle birlikte kullanın.

```powershell
$TargetPol1 = Get-AzRecoveryServicesBackupProtectionPolicy -Name <PolicyName>
$anotherBkpItem = Get-AzRecoveryServicesBackupItem -WorkloadType MSSQL -BackupManagementType AzureWorkload -Name "<BackupItemName>"
Enable-AzRecoveryServicesBackupProtection -Item $anotherBkpItem -Policy $TargetPol1
```

Komut, yapılandırma yedeklemesi tamamlanana kadar bekler ve aşağıdaki çıktıyı döndürür.

```output
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
master           ConfigureBackup      Completed            3/18/2019 8:00:21 PM      3/18/2019 8:02:16 PM      654e8aa2-4096-402b-b5a9-e5e71a496c4e
```

### <a name="re-register-sql-vms"></a>SQL VM'leri yeniden kaydettirin

> [!WARNING]
> Yeniden kayıt denemeden önce hata belirtilerini ve nedenlerini anlamak için bu [belgeyi](backup-sql-server-azure-troubleshoot.md#re-registration-failures) okuduğunuzdan emin olun

SQL VM'nin yeniden kaydolmasını tetiklemek için, ilgili yedek kabı getirin ve kayıt cmdlet'ine geçirin.

```powershell
$SQLContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName <VM name> -VaultId $targetvault.ID
Register-AzRecoveryServicesBackupContainer -Container $SQLContainer -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetVault.ID
```

### <a name="stop-protection"></a>Korumayı durdurma

#### <a name="retain-data"></a>Verileri tutma

Kullanıcı korumayı durdurmak isterse, [Devre Dışı Bırak-AzRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0) PS cmdlet'i kullanabilir. Bu, zamanlanan yedeklemeleri durdurur, ancak şimdiye kadar yedeklenen veriler sonsuza kadar saklanır.

```powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureWorkload -WorkloadType MSSQL -Name "<backup item name>" -VaultId $targetVault.ID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID
```

#### <a name="delete-backup-data"></a>Yedekleme verilerini silme

Kasada depolanan yedekleme verilerini tamamen kaldırmak için '-RemoveRecoveryPoints' bayrağını/anahtarını ['devre dışı bırak' koruma komutuna](#retain-data)eklemeniz yeterlidir.

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID -RemoveRecoveryPoints
```

#### <a name="disable-auto-protection"></a>Otomatik korumayı devre dışı

Otomatik koruma bir SQLInstance üzerinde yapılandırıldıysa, kullanıcı [Devre Dışı Bırak-AzRecoveryServicesBackupAutoProtection](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupAutoProtection?view=azps-1.5.0) PS cmdlet kullanarak devre dışı bırakabilirsiniz.

```powershell
$SQLInstance = Get-AzRecoveryServicesBackupProtectableItem -workloadType MSSQL -ItemType SQLInstance -VaultId $targetVault.ID -Name "<Protectable Item name>" -ServerName "<Server Name>"
Disable-AzRecoveryServicesBackupAutoProtection -InputItem $SQLInstance -BackupManagementType AzureWorkload -WorkloadType MSSQL -VaultId $targetvault.ID
```

#### <a name="unregister-sql-vm"></a>SQL VM'nin kaydını silme

Bir SQL sunucusunun tüm DB'leri [artık korunmuyorsa ve yedek veri yoksa,](#delete-backup-data)kullanıcı bu kasadan SQL VM kaydını açabilir. Ancak o zaman kullanıcı DB'leri başka bir kasaya koruyabilir. SQL VM'nin kaydını çıkarmak için [Kayıt Dışı-AzRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/az.recoveryservices/Unregister-AzRecoveryServicesBackupContainer?view=azps-1.5.0) PS cmdlet kullanın.

```powershell
$SQLContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVMAppContainer -FriendlyName <VM name> -VaultId $targetvault.ID
 Unregister-AzRecoveryServicesBackupContainer -Container $SQLContainer -VaultId $targetvault.ID
```

### <a name="track-azure-backup-jobs"></a>Azure Yedekleme işlerini izleme

Azure Yedekleme'nin yalnızca kullanıcı tarafından tetiklenen SQL yedeklemesindeki işleri izlediğini unutmayın. Zamanlanmış yedeklemeler (günlük yedeklemeleri dahil) portal/powershell'de görünmez. Ancak, zamanlanan işler başarısız olursa, [bir yedekleme uyarısı](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) oluşturulur ve portalda gösterilir. Tüm zamanlanmış işleri ve diğer ilgili bilgileri izlemek için [Azure Monitor'u kullanın.](backup-azure-monitoring-use-azuremonitor.md)

Kullanıcılar, yedekleme gibi eşzamanlı işlerin [çıktısında](#on-demand-backup) döndürülen JobID ile isteğe bağlı/kullanıcı tetiklenen işlemleri izleyebilir. İşi ve ayrıntılarını takip etmek için [Get-AzRecoveryServicesBackupJobDetail](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupJobDetail) PS cmdlet'i kullanın.

```powershell
 Get-AzRecoveryServicesBackupJobDetails -JobId 2516bb1a-d3ef-4841-97a3-9ba455fb0637 -VaultId $targetVault.ID
```

İsteğe bağlı işlerin listesini ve durumlarını Azure Yedekleme hizmetinden almak için [Get-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupJob?view=azps-1.5.0) PS cmdlet'i kullanın. Aşağıdaki örnek, devam eden tüm SQL işlerini döndürür.

```powershell
Get-AzRecoveryServicesBackupJob -Status InProgress -BackupManagementType AzureWorkload
```

Devam eden bir işi iptal etmek için [Stop-AzRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/az.recoveryservices/Stop-AzRecoveryServicesBackupJob?view=azps-1.5.0) PS cmdlet'i kullanın.

## <a name="managing-sql-always-on-availability-groups"></a>SQL Her Zaman Kullanılabilirlik gruplarını Yönetme

SQL Always On Availability Groups için Kullanılabilirlik grubunun (AG) [tüm düğümlerini kaydettiğinden](#registering-the-sql-vm) emin olun. Tüm düğümler için kayıt yapıldıktan sonra, bir SQL kullanılabilirlik grubu nesnesi mantıksal olarak korunabilir öğeler altında oluşturulur. SQL AG altındaki veritabanları 'SQLDatabase' olarak listelenecektir. Düğümler bağımsız örnekler olarak gösterilecek ve bunların altındaki varsayılan SQL veritabanları da SQL veritabanları olarak listelenecektir.

Örneğin, bir SQL AG'nin iki düğümü olduğunu varsayalım: 'sql-server-0' ve 'sql-server-1' ve 1 SQL AG DB. Her iki düğüm de kaydedildikten sonra, kullanıcı [korunabilir öğeleri](#fetching-sql-dbs)listelerse, aşağıdaki bileşenleri listeler

* SQL AG nesnesi - SQLAvailabilityGroup olarak korunabilir madde türü
* A SQL AG DB - SQLDatabase olarak korunabilir madde türü
* sql-server-0 - SQLInstance olarak korunabilir madde türü
* sql-server-1 - SQLInstance olarak korunabilir madde türü
* SQL-server-0 altında varsayılan SQL DB'ler (master, model, msdb) - SQLDatabase olarak korunabilir madde türü
* SQL-server-1 altında varsayılan SQL DB'ler (master, model, msdb) - SQLDatabase olarak korunabilir madde türü

sql-server-0, sql-server-1 de [yedek kapsayıcılar listelendiğinde](https://docs.microsoft.com/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupContainer?view=azps-1.5.0)"AzureVMAppContainer" olarak listelenecektir.

Yedeklemeyi [etkinleştirmek](#configuring-backup) için ilgili SQL veritabanını getirmenizi ve [isteğe bağlı yedeklemeyi](#on-demand-backup) ve [PS cmdletlerini geri yüklemenin](#restore-sql-dbs) biraynı olduğunu.
