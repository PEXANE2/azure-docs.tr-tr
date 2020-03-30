---
title: Quickstart - PowerShell ile vm yedekleme
description: Bu Quickstart'ta Azure Sanal Makinelerinizi Azure PowerShell modülüyle nasıl yedekleyin öğrenin.
ms.devlang: azurecli
ms.topic: quickstart
ms.date: 04/16/2019
ms.custom: mvc
ms.openlocfilehash: 8021ca553a1434c891bee911e85d351c61938594
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74171953"
---
# <a name="back-up-a-virtual-machine-in-azure-with-powershell"></a>PowerShell ile Azure'daki bir sanal makineyi yedekleme

[Azure PowerShell AZ](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) modülü, komut satırından veya komut dosyasından Azure kaynaklarını oluşturmak ve yönetmek için kullanılır.

[Azure Yedekleme,](backup-overview.md) şirket içi makineleri ve uygulamaları ve Azure VM'leri yedekler. Bu makalede, AZ modülü ile bir Azure VM yedekleme kullanabilirsiniz. Alternatif olarak, [Azure CLI'yi](quick-backup-vm-cli.md)kullanarak veya [Azure portalında](quick-backup-vm-portal.md)bir VM yedekleyebilirsiniz.

Bu hızlı başlangıç belgesi var olan bir Azure VM'de yedeklemeyi etkinleştirir. Bir sanal makine oluşturmanız gerekiyorsa [Azure PowerShell ile sanal makine oluşturabilirsiniz](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).

Bu hızlı başlatma, Azure PowerShell AZ modül sürümü 1.0.0 veya daha sonra gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-and-register"></a>Oturum açın ve kaydolun

1. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

    ```powershell
    Connect-AzAccount
    ```

2. Azure Yedekleme'yi ilk kez kullandığınızda, aboneliğinizdeki Azure Kurtarma Hizmeti sağlayıcısını [Register-AzResourceProvider'a](/powershell/module/az.Resources/Register-azResourceProvider)kaydettirmeniz gerekir, aşağıdaki gibi:

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

[Kurtarma Hizmetleri kasası,](backup-azure-recovery-services-vault-overview.md) Azure VM'leri gibi korumalı kaynaklar için yedekleme verilerini depolayan mantıksal bir kapsayıcıdır. Yedekleme işi çalıştığında, Kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

Kasayı oluşturduğunuzda:

- Kaynak grubu ve konumu için, yedeklemek istediğiniz VM'nin kaynak grubunu ve konumunu belirtin.
- VM oluşturmak için bu [örnek komut dosyası](../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) kullandıysanız, kaynak grubu **myResourceGroup,** VM ***myVM**ve kaynakları **WestEurope** bölgesinde bulunmaktadır.
- Azure Yedekleme, yedeklenmiş veriler için depolamayı otomatik olarak işler. Varsayılan olarak kasa [Geo-Redundant Depolama (GRS)](../storage/common/storage-redundancy-grs.md)kullanır. Coğrafi artıklık, yedeklenen verilerin birincil bölgeden yüzlerce mil uzaktaki ikincil bir Azure bölgesine çoğaltılmasını sağlar.

Şimdi bir kasa oluşturun:

1. Kasaoluşturmak için [New-AzRecoveryServicesVault'u](/powershell/module/az.recoveryservices/new-azrecoveryservicesvault) kullanın:

    ```powershell
    New-AzRecoveryServicesVault `
        -ResourceGroupName "myResourceGroup" `
        -Name "myRecoveryServicesVault" `
    -Location "WestEurope"
    ```

2. [Set-AzRecoveryServicesVaultContext](/powershell/module/az.RecoveryServices/Set-azRecoveryServicesVaultContext)ile kasa bağlamını aşağıdaki gibi ayarlayın:

    ```powershell
    Get-AzRecoveryServicesVault `
        -Name "myRecoveryServicesVault" | Set-AzRecoveryServicesVaultContext
    ```

3. [Set-AzRecoveryServicesBackupProperty](https://docs.microsoft.com/powershell/module/az.recoveryservices/Set-AzRecoveryServicesBackupProperty)ile kasanın depolama artıklık yapılandırmasını (LRS/GRS) aşağıdaki gibi değiştirin:

    ```powershell
    Get-AzRecoveryServicesVault `
        -Name "myRecoveryServicesVault" | Set-AzRecoveryServicesBackupProperty -BackupStorageRedundancy LocallyRedundant/GeoRedundant
    ```

    > [!NOTE]
    > Depolama Artıklığı, yalnızca kasada korunan yedek öğeler yoksa değiştirilebilir.

## <a name="enable-backup-for-an-azure-vm"></a>Bir Azure sanal makinesi için yedeklemeyi etkinleştirme

Bir Azure VM için yedeklemeyi etkinleştirin ve bir yedekleme ilkesi belirtin.

- İlke, yedeklemelerin ne zaman çalıştırıldığını ve yedeklemeler tarafından oluşturulan kurtarma noktalarının ne kadar süreyle korunması gerektiğini tanımlar.
- Varsayılan koruma ilkesi VM için günde bir kez yedekleme çalıştırıyor ve oluşturulan kurtarma noktalarını 30 gün boyunca saklar. VM'nizi hızla korumak için bu varsayılan ilkeyi kullanabilirsiniz.

Yedeklemeyi aşağıdaki gibi etkinleştirin:

1. İlk olarak, [Get-AzRecoveryServicesBackupProtectionPolicy](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy)ile varsayılan ilkeyi ayarlayın:

    ```powershell
    $policy = Get-AzRecoveryServicesBackupProtectionPolicy     -Name "DefaultPolicy"
    ```

2. [Enable-AzRecoveryServicesBackupProtection](/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection)ile VM yedeklemesini etkinleştirin. İlkeyi, kaynak grubunu ve VM adını belirtin.

    ```powershell
    Enable-AzRecoveryServicesBackupProtection `
        -ResourceGroupName "myResourceGroup" `
        -Name "myVM" `
        -Policy $policy
    ```

## <a name="start-a-backup-job"></a>Bir yedekleme işi başlatma

Yedeklemeler, yedekleme ilkesinde belirtilen zamanlamaya göre çalışır. İsteğe bağlı yedekleme de çalıştırabilirsiniz:

- İlk yedekleme işi tam bir kurtarma noktası oluşturur.
- İlk yedeklemeden sonra, her yedekleme işi artımlı kurtarma noktaları oluşturur.
- Yalnızca son yedekleme sonrasında yapılan değişiklikleri aktardığından artımlı kurtarma noktaları depolama alanı ve süre açısından verimlilik sağlar.

İsteğe bağlı yedekleme çalıştırmak için [Backup-AzRecoveryServicesBackupItem'i](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem)kullanırsınız.

- [Get-AzRecoveryServicesBackupContainer](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer)ile yedekleme verilerinizi tutan kasada bir kapsayıcı belirtirsiniz.
- Yedeklenecek her sanal makine ayrı bir öğe olarak ele alınır. Yedek bir işe başlamak için [Get-AzRecoveryServicesBackupItem](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem)ile VM hakkında bilgi edinebilirsiniz.

İsteğe bağlı yedekleme işini aşağıdaki gibi çalıştırın:

1. Kapsayıcıyı belirtin, VM bilgilerini edinin ve yedeklemeyi çalıştırın.

    ```powershell
    $backupcontainer = Get-AzRecoveryServicesBackupContainer `
        -ContainerType "AzureVM" `
        -FriendlyName "myVM"

    $item = Get-AzRecoveryServicesBackupItem `
        -Container $backupcontainer `
        -WorkloadType "AzureVM"

    Backup-AzRecoveryServicesBackupItem -Item $item
    ```

2. İlk yedekleme işi tam bir kurtarma noktası oluşturduğundan, 20 dakika kadar beklemeniz gerekebilir. Bir sonraki yordamda açıklandığı gibi işi izleyin.

## <a name="monitor-the-backup-job"></a>Yedekleme işini izleme

1. İş durumunu izlemek için [Get-AzRecoveryservicesBackupJob](/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob) çalıştırın.

    ```powershell
    Get-AzRecoveryservicesBackupJob
    ```

    Çıktı, işi **InProgress**olarak gösteren aşağıdaki örneğe benzer:

    ```output
    WorkloadName   Operation         Status       StartTime              EndTime                JobID
    ------------   ---------         ------       ---------              -------                -----
    myvm           Backup            InProgress   9/18/2017 9:38:02 PM                          9f9e8f14
    myvm           ConfigureBackup   Completed    9/18/2017 9:33:18 PM   9/18/2017 9:33:51 PM   fe79c739
    ```

2. İş durumu **tamamlandığında,** VM korunur ve tam bir kurtarma noktası depolanır.

## <a name="clean-up-the-deployment"></a>Dağıtımı temizleme

VM'yi artık yedeklemenize gerek yoksa, temizleyebilirsiniz.

- VM'yi geri geri getirmek istiyorsanız, temizlemeyi atlayın.
- Varolan bir VM kullandıysanız, kaynak grubunu ve VM'yi yerinde bırakmak için son [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet'ini atlayabilirsiniz.

Korumayı devre dışı, geri yükleme noktalarını ve kasayı çıkarın. Ardından kaynak grubunu ve ilişkili VM kaynaklarını aşağıdaki gibi silin:

```powershell
Disable-AzRecoveryServicesBackupProtection -Item $item -RemoveRecoveryPoints
$vault = Get-AzRecoveryServicesVault -Name "myRecoveryServicesVault"
Remove-AzRecoveryServicesVault -Vault $vault
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Kurtarma Hizmetleri kasası oluşturdunuz, bir sanal makine için koruma özelliklerini etkinleştirdiniz ve ilk kurtarma noktasını oluşturdunuz.

- Azure portalında VM'leri [nasıl](tutorial-backup-vm-at-scale.md) yedekleyin öğrenin.
- VM'yi hızlı bir şekilde [nasıl](tutorial-restore-disk.md) geri yükleyin
