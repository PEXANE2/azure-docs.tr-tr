---
title: 'Öğretici: PowerShell ile birden fazla Azure VM yedeklemesi'
description: Bu öğretici, Azure PowerShell kullanarak birden çok Azure VM'yi Kurtarma Hizmetleri kasasına yedekleme ayrıntılarını açıklar.
ms.topic: tutorial
ms.date: 03/05/2019
ms.custom: mvc
ms.openlocfilehash: 154238eae78ce44b9fc91058e58d9a11e254c0f9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74171777"
---
# <a name="back-up-azure-vms-with-powershell"></a>Azure VM’lerini PowerShell ile yedekleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğretici, PowerShell kullanarak birden çok Azure VM'sini yedeklemek için Azure [Yedekleme](backup-overview.md) Kurtarma Hizmetleri kasasının nasıl dağıtılanabildiğini açıklar.  

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Kurtarma Hizmetleri kasası oluşturun ve kasa bağlamını ayarlayın.
> * Yedekleme ilkesi tanımlama
> * Birden çok sanal makineyi korumak için yedekleme ilkesini uygulama
> * Korunan sanal makineler için isteğe bağlı yedekleme işini tetikleme Sanal makineyi yedeklemeden (veya korumadan) önce, SANAL M'lerinizi korumak için ortamınızı hazırlamak için [ön koşulları](backup-azure-arm-vms-prepare.md) tamamlamanız gerekir.

> [!IMPORTANT]
> Bu öğretici, önceden bir kaynak grubu ve bir Azure sanal makinesi oluşturmuş olduğunuzu varsayar.

## <a name="sign-in-and-register"></a>Oturum açın ve kaydolun

1. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

    ```powershell
    Connect-AzAccount
    ```

2. Azure Yedekleme'yi ilk kez kullandığınızda, aboneliğinizde Azure Kurtarma Hizmeti sağlayıcısını [Register-AzResourceProvider'a](/powershell/module/az.Resources/Register-azResourceProvider)kaydettirmelisiniz. Daha önce kaydolduysanız, bu adımı atlayın.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[Kurtarma Hizmetleri kasası,](backup-azure-recovery-services-vault-overview.md) Azure VM'leri gibi korumalı kaynaklar için yedekleme verilerini depolayan mantıksal bir kapsayıcıdır. Yedekleme işi çalıştığında, Kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

* Bu öğreticide, yedeklemek istediğiniz VM ile aynı kaynak grubunda ve konumda kasa oluşturursunuz.
* Azure Yedekleme, yedeklenmiş veriler için depolamayı otomatik olarak işler. Varsayılan olarak kasa [Geo-Redundant Depolama (GRS)](../storage/common/storage-redundancy-grs.md)kullanır. Coğrafi artıklık, yedeklenen verilerin birincil bölgeden yüzlerce mil uzaktaki ikincil bir Azure bölgesine çoğaltılmasını sağlar.

Aşağıdaki gibi tonoz oluşturun:

1. Kasaoluşturmak için [New-AzRecoveryServicesVault'u](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault)kullanın. Yedeklemek istediğiniz VM'nin kaynak grubu adını ve konumunu belirtin.

    ```powershell
    New-AzRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
    ```

2. Çoğu Azure Backup cmdlet’i, girdi olarak Kurtarma Hizmetleri kasasını gerektirir. Bu nedenle, Yedekleme Kurtarma Hizmetleri kasasının bir değişkende depolanması uygundur.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault –Name myRSVault
    ```

3. [Set-AzRecoveryServicesVaultContext](/powershell/module/az.RecoveryServices/Set-azRecoveryServicesVaultContext)ile kasa bağlamını ayarlayın.

   * Kasa bağlamı, kasada korunan veri türüdür.
   * Bağlam ayarlandıktan sonra, sonraki tüm cmdlets için geçerlidir

     ```powershell
     Get-AzRecoveryServicesVault -Name "myRSVault" | Set-AzRecoveryServicesVaultContext
     ```

## <a name="back-up-azure-vms"></a>Azure VM'lerini yedekleme

Yedeklemeler, yedekleme ilkesinde belirtilen zamanlamaya göre çalışır. Bir Kurtarma Hizmetleri kasası oluşturduğunuzda bu, varsayılan koruma ve saklama ilkeleri ile birlikte gelir.

* Varsayılan koruma ilkesi, belirli bir saatte günde bir kez yedekleme işini tetikler.
* Varsayılan saklama ilkesi, 30 gün boyunca günlük kurtarma noktasını korur.

Bu eğitimde Azure VM'yi etkinleştirmek ve yedeklemek için aşağıdakileri yaparız:

1. [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-Azrecoveryservicesbackupcontainer)ile yedekleme verilerinizi tutan kasada bir kapsayıcı belirtin.
2. Yedekleme için her VM bir öğedir. Yedek bir işe başlamak için [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/Get-AzRecoveryServicesBackupItem)ile VM hakkında bilgi edinebilirsiniz.
3. [Backup-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/backup-Azrecoveryservicesbackupitem)ile isteğe bağlı yedekleme çalıştırın.
    * İlk yedekleme işi tam bir kurtarma noktası oluşturur.
    * İlk yedeklemeden sonra, her yedekleme işi artımlı kurtarma noktaları oluşturur.
    * Yalnızca son yedekleme sonrasında yapılan değişiklikleri aktardığından artımlı kurtarma noktaları depolama alanı ve süre açısından verimlilik sağlar.

Yedeklemeyi aşağıdaki gibi etkinleştirin ve çalıştırın:

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzRecoveryServicesBackupItem -Item $item
```

## <a name="troubleshooting"></a>Sorun giderme

Sanal makinenizi yedeklerken sorunlarla karşılaştıysanız, bu [sorun giderme makalelerini](backup-azure-vms-troubleshoot.md)gözden geçirin.

### <a name="deleting-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası silme

Bir kasayı silmeniz gerekiyorsa, önce kasadaki kurtarma noktalarını silin ve ardından kasanın kaydını aşağıdaki gibi silin:

```powershell
$Cont = Get-AzRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzRecoveryServicesVault -Vault $vault1
```

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell ile Azure VM'leri yedekleme ve geri alma yla ilgili daha ayrıntılı bir gözden [geçirme.](backup-azure-vms-automation.md)
* [Azure VM'leri yönetme ve izleme](backup-azure-manage-vms.md)
* [Azure VM'lerini geri yükleme](backup-azure-arm-restore-vms.md)
