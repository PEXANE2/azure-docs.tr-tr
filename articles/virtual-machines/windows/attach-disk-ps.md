---
title: PowerShell'i kullanarak Azure'da bir Windows VM'ye veri diski ekleme
description: Kaynak Yöneticisi dağıtım modeliyle PowerShell'i kullanarak windows vm'ye yeni veya varolan bir veri diski nasıl ekilir?
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: ce995a84d2290845e83416caf9c8b0004242eed4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267761"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>PowerShell ile Windows VM'ye veri diski ekleme

Bu makalede, PowerShell kullanarak bir Windows sanal makineye hem yeni hem de varolan diskler nasıl ekleniyor gösterir. 

İlk olarak, şu ipuçlarını gözden geçirin:

* Sanal makinenin boyutu, kaç veri diski ekebileceğinizi denetler. Daha fazla bilgi [için sanal makineler için Boyutlar'a](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.
* Premium SSD'leri kullanmak için, DS serisi veya GS serisi sanal makine gibi [üstün depolama özellikli VM türüne](sizes-memory.md)ihtiyacınız vardır.

Bu makalede, en son sürüme sürekli olarak güncelleştirilen [Azure Bulut Shell'de PowerShell](https://docs.microsoft.com/azure/cloud-shell/overview)kullanır. Bulut Kabuğu'nu açmak için, herhangi bir kod bloğunun üstünden **deneyin'i** seçin.

## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Sanal makineye boş bir veri diski ekleme

Bu örnek, boş bir veri diskinin varolan bir sanal makineye nasıl ekleyeceğini gösterir.

### <a name="using-managed-disks"></a>Yönetilen diskleri kullanma

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Kullanılabilirlik Bölgesinde yönetilen diskleri kullanma

Kullanılabilirlik Bölgesinde bir disk oluşturmak için parametreli [Yeni-AzDiskConfig'i](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig) `-Zone` kullanın. Aşağıdaki örnek, bölge *1'de*bir disk oluşturur.

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2'
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="initialize-the-disk"></a>Diski başlatma

Boş bir disk ekledikten sonra, diski başlatmanız gerekir. Diski başlatmak için bir VM'de oturum açabilir ve disk yönetimini kullanabilirsiniz. [Oluşturduğunuzda WinRM'i](https://docs.microsoft.com/windows/desktop/WinRM/portal) ve VM'deki bir sertifikayı etkinleştirdiyseniz, diski açmak için uzaktan PowerShell'i kullanabilirsiniz. Özel bir komut dosyası uzantısı da kullanabilirsiniz:

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

Komut dosyası dosyası, diskleri başlatmaya yönelik kod içerebilir, örneğin:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```

## <a name="attach-an-existing-data-disk-to-a-vm"></a>Varolan bir veri diskini VM'ye ekleme

Varolan yönetilen bir diski veri diski olarak VM'ye ekleyebilirsiniz.

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen diskleri şablonları kullanarak da dağıtabilirsiniz. Daha fazla bilgi için azure [kaynak yöneticisi şablonlarında Yönetilen Diskleri Kullanma](using-managed-disks-template-deployments.md) veya birden çok veri diski dağıtmak için hızlı başlangıç [şablonuna](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-multiple-data-disk) bakın.
