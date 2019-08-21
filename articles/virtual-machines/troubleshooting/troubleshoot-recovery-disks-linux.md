---
title: Azure CLı ile Linux sorun giderme VM kullanma | Microsoft Docs
description: Azure CLı kullanarak işletim sistemi diskini bir kurtarma VM 'sine bağlayarak Linux VM sorunlarını giderme hakkında bilgi edinin
services: virtual-machines-linux
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: genli
ms.openlocfilehash: 49ee83e451e9d555a7fe5fca57bc58d6616334da
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69641062"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli"></a>Azure CLı ile işletim sistemi diskini bir kurtarma sanal makinesine ekleyerek bir Linux sanal makinesi sorunlarını giderme
Linux sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, sanal sabit diskin kendisi üzerinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Ortak bir örnek, VM 'nin başarıyla önyükleme yapabilmesini engelleyen ' de `/etc/fstab` geçersiz bir giriş olabilir. Bu makalede, herhangi bir hatayı onarmak üzere sanal sabit diskinizi başka bir Linux VM 'sine bağlamak için Azure CLı 'nın nasıl kullanılacağı açıklanır ve ardından özgün VM 'nizi yeniden oluşturabilirsiniz. 

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen VM 'yi durdurun.
1. VM 'nin işletim sistemi diskinden bir anlık görüntü alın.
1. İşletim sistemi diski anlık görüntüsünden bir disk oluşturun.
1. Sorun giderme amacıyla yeni işletim sistemi diskini başka bir Linux VM 'sine ekleyin ve bağlayın.
1. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Yeni işletim sistemi diskindeki sorunları gidermek için dosyaları düzenleyin veya herhangi bir araç çalıştırın.
1. Yeni işletim sistemi diskini sorun giderme VM 'sinden çıkarın ve ayırın.
1. Etkilenen VM için işletim sistemi diskini değiştirin.

Bu sorun giderme adımlarını gerçekleştirmek için, [az Login](/cli/azure/reference-index)kullanılarak en son [Azure CLI](/cli/azure/install-az-cli2) 'Nın yüklü ve bir Azure hesabında oturum açmış olmanız gerekir.

> [!Important]
> Bu makaledeki betikler yalnızca [yönetilen disk](../linux/managed-disks-overview.md)kullanan VM 'ler için geçerlidir. 

Aşağıdaki örneklerde parametre adlarını kendi değerlerinizle değiştirin. Örnek parametre adları ve `myResourceGroup` `myVM`içerir.

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
SANAL makinenizin neden doğru şekilde önyüklenemediğini anlamak için seri çıktıyı inceleyin. Ortak bir örnek, içinde `/etc/fstab`geçersiz bir giriştir veya silinmekte veya taşınmakta olan temeldeki sanal sabit disk.

[Az VM Boot-Diagnostics Get-boot-log](/cli/azure/vm/boot-diagnostics)ile önyükleme günlüklerini alın. Aşağıdaki örnek, adlı `myVM` `myResourceGroup`kaynak grubunda adlı VM 'den alınan seri çıktıyı alır:

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

VM 'nin neden önyükleme başarısız olduğunu anlamak için seri çıktıyı gözden geçirin. Seri çıkış herhangi bir bildirim sağlamadıysa, sanal sabit diski bir sorun giderme sanal makinesine bağlıyken `/var/log` günlük dosyalarını gözden geçirmeniz gerekebilir.

## <a name="stop-the-vm"></a>VM’yi durdurma

Aşağıdaki örnek, adlı `myVM` `myResourceGroup`kaynak grubundan adlı VM 'yi durduruyor:

```powershell
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Sonraki adıma geçmeden önce VM 'nin silme işlemini tamamlamasını bekleyin.

## <a name="create-a-snapshot-from-the-os-disk-of-the-vm"></a>VM 'nin işletim sistemi diskinden anlık görüntü oluşturma

Anlık görüntü, bir VHD 'nin tam ve salt okunurdur kopyasıdır. Bir sanal makineye bağlanamaz. Bir sonraki adımda, bu anlık görüntüden bir disk oluşturacağız. Aşağıdaki örnek, ' myvm ' adlı `mySnapshot` VM 'nin işletim sistemi diskinden adında bir anlık görüntü oluşturur. 

```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  

#Get the VM
$vm = get-azvm `
-ResourceGroupName $resourceGroupName `
-Name $vmName

#Create the snapshot configuration for the OS disk
$snapshot =  New-AzSnapshotConfig `
-SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
-Location $location `
-CreateOption copy

#Take the snapshot
New-AzSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```
## <a name="create-a-disk-from-the-snapshot"></a>Anlık görüntüden disk oluşturma

Bu betik adlı `mysnapshot`anlık görüntüden adında bir yönetilen `newOSDisk` disk oluşturur.  

```powershell
#Set the context to the subscription Id where Managed Disk will be created
#You can skip this step if the subscription is already selected

$subscriptionId = 'yourSubscriptionId'

Select-AzSubscription -SubscriptionId $SubscriptionId

#Provide the name of your resource group
$resourceGroupName ='myResourceGroup'

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshotName = 'mySnapshot' 

#Provide the name of the Managed Disk
$diskName = 'newOSDisk'

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize = '128'

#Provide the storage type for Managed Disk. PremiumLRS or StandardLRS.
$storageType = 'StandardLRS'

#Provide the Azure region (e.g. westus) where Managed Disks will be located.
#This location should be same as the snapshot location
#Get all the Azure location using command below:
#Get-AzLocation
$location = 'eastus'

$snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
 
$diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
 
New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
```
Artık özgün işletim sistemi diskinin bir kopyasına sahipsiniz. Sorun giderme amacıyla bu yeni diski başka bir Windows sanal makinesine bağlayabilirsiniz.

## <a name="attach-the-new-virtual-hard-disk-to-another-vm"></a>Yeni sanal sabit diski başka bir VM 'ye iliştirme
Sonraki birkaç adımda, sorun giderme amacıyla başka bir VM kullanırsınız. Diskin içeriğini taramak ve düzenlemek için bu sorun giderme sanal makinesine diski iliştirin. Bu işlem, örneğin yapılandırma hatalarını düzeltmenize veya ek uygulama ya da sistem günlük dosyalarını incelemenizi sağlar. Sorun giderme amacıyla kullanmak için başka bir VM seçin veya oluşturun.

Var olan sanal sabit diski [az VM yönetilmeyen disk Attach](/cli/azure/vm/unmanaged-disk)ile ekleyin. Var olan sanal sabit diski iliştirdiğinizde, önceki `az vm show` komutta elde edilen diskin URI 'sini belirtin. Aşağıdaki örnek, adlı `myVMRecovery` `myResourceGroup`kaynak grubunda adlı sorun giderme sanal makinesine var olan bir sanal sabit diski iliştirir:

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```
## <a name="mount-the-attached-data-disk"></a>Bağlı veri diskini bağlama

> [!NOTE]
> Aşağıdaki örneklerde bir Ubuntu VM 'de gereken adımlar ayrıntılı olarak verilmiştir. Red Hat Enterprise Linux veya SUSE gibi farklı bir Linux kaldırı kullanıyorsanız, günlük dosyası konumları ve `mount` komutları biraz farklı olabilir. Komutlarınıza ilişkin uygun değişiklikler için özel olarak verilen belgelere başvurun.

1. Uygun kimlik bilgilerini kullanarak sorun giderme sanal makinesine SSH. Bu disk, sorun giderme sanal makinesine bağlı ilk veri diskidir, disk muhtemelen öğesine `/dev/sdc`bağlıdır. Eklenen `dmseg` diskleri görüntülemek için kullanın:

    ```bash
    dmesg | grep SCSI
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    Yukarıdaki örnekte, işletim sistemi diski `/dev/sda` ve her sanal makine `/dev/sdb`için belirtilen geçici disk ' dir. Birden çok veri diskine sahipseniz,, vb. olmaları gerekir `/dev/sdd`. `/dev/sde`

2. Var olan sanal sabit diskinizi bağlamak için bir dizin oluşturun. Aşağıdaki örnek adlı `troubleshootingdisk`bir dizin oluşturur:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Var olan sanal sabit diskinizde birden çok bölüm varsa, gerekli bölümü bağlayın. Aşağıdaki örnek, konumundaki `/dev/sdc1`ilk birincil bölümü bağlar:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > En iyi yöntem, sanal sabit diskin evrensel benzersiz tanımlayıcısını (UUID) kullanarak Azure 'daki VM 'Lere veri disklerini bağlamasıdır. Bu kısa sorun giderme senaryosunda, UUID 'yi kullanarak sanal sabit diski bağlama gerekli değildir. Ancak, normal kullanım altında, sanal `/etc/fstab` sabit disklerin UUID yerine cihaz adı kullanılarak bağlanması için düzenlemenin başarısız olması VM 'nin önyüklenememesine neden olabilir.


## <a name="fix-issues-on-the-new-os-disk"></a>Yeni işletim sistemi diskinde sorunları çözme
Var olan sanal sabit disk takılı olduğunda, artık gereken tüm bakım ve sorun giderme adımlarını gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-the-new-os-disk"></a>Yeni işletim sistemi diskini çıkarma ve ayırma
Hatalar çözümlendikten sonra, var olan sanal sabit diski sorun giderme sanal makinenizin bağlantısını çıkarırın. Sanal sabit diski, sorun giderme VM 'sine iliştirene kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. SSH oturumundan sorun giderme sanal makinesine var olan sanal sabit diski çıkarın. İlk olarak bağlama noktanız için üst dizinden geçiş yapın:

    ```bash
    cd /
    ```

    Şimdi var olan sanal sabit diski çıkarın. Aşağıdaki örnek, cihazı şu adreste `/dev/sdc1`kaldırır:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Artık sanal sabit diski VM 'den ayırın. SSH oturumundan sorun giderme sanal makinesine çıkın. Bağlı veri disklerini, [az VM yönetilmeyen disk listesi](/cli/azure/vm/unmanaged-disk)ile sorun giderme sanal makinesine listeleyin. Aşağıdaki örnek, adlı `myVMRecovery` `myResourceGroup`kaynak grubunda adlı sanal makineye bağlı veri disklerini listeler:

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    Var olan sanal sabit diskinizin adını aklınızda edin. Örneğin, URI 'si **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** olan bir diskin adı **myvhd**'dir. 

    Veri diskini VM 'nizden ayırın [az VM yönetilmeyen-disk Detach](/cli/azure/vm/unmanaged-disk). Aşağıdaki örnek, `myVHD` `myResourceGroup` kaynak grubunda adlı `myVMRecovery` VM 'den adlı diski ayırır:

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>Etkilenen VM için işletim sistemi diskini değiştirme

İşletim sistemi disklerini değiştirmek için Azure PowerShell kullanabilirsiniz. VM 'yi silip yeniden oluşturmanız gerekmez.

Bu örnek, adlı `myVM` VM 'yi durdurup yeni işletim sistemi diski olarak adlandırılan `newOSDisk` diski atar. 

```powershell
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name  -sto

# Update the VM with the new OS disk. Possible values of StorageAccountType include: 'Standard_LRS' and 'Premium_LRS'
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm -StorageAccountType <Type of the storage account >

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure VM Ile SSH bağlantılarında sorun giderme](troubleshoot-ssh-connection.md). VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [bir LINUX sanal makinesinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).

