---
title: Yönetilen diskdepolamayı standart ve premium SSD arasında dönüştürme
description: Azure PowerShell'i kullanarak Azure yönetilen diskleri Standart'tan Premium'a veya Premium'dan Standard'a dönüştürme.
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: e339f0c7ca0807eec3e160eeb3464044c2ef29ba
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720954"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Yönetilen bir diskin depolama türünü güncelleştirme

Azure yönetilen disklerin dört disk türü vardır: Azure ultra SSD'leri (önizleme), premium SSD, standart SSD ve standart HDD. Performans gereksinimlerinize bağlı olarak üç GA disk türü (premium SSD, standart SSD ve standart HDD) arasında geçiş yapabilirsiniz. Henüz ultra SSD'den veya ultra SSD'ye geçiş yapamadığınız için, yeni bir tane dağıtmanız gerekir.

Bu işlevsellik yönetilmeyen diskler için desteklenmez. Ancak, disk türleri arasında geçiş [yapabilmek için yönetilmeyen bir diski kolayca yönetilen bir diske dönüştürebilirsiniz.](convert-unmanaged-to-managed-disks.md)

 

## <a name="prerequisites"></a>Ön koşullar

* Dönüştürme sanal makinenin (VM) yeniden başlatılmasını gerektirdiğinden, disk depolama alanınızın geçişini önceden varolan bir bakım penceresi sırasında zamanlamanız gerekir.
* Diskiniz yönetilmiyorsa, depolama seçenekleri arasında geçiş yapabilmeniz için önce [yönetilen bir diske dönüştürün.](convert-unmanaged-to-managed-disks.md)

## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Bir VM'nin yönetilen tüm disklerini Premium ve Standart arasında değiştirme

Bu örnek, bir VM'nin tüm disklerinin Standart'tan Premium depolama alanına veya Premium depolama alanına nasıl dönüştürüleceğini gösterir. Premium yönetilen diskleri kullanmak için VM'nizin Premium depolama alanını destekleyen bir [VM boyutu](sizes.md) kullanması gerekir. Bu örnek, premium depolamayı destekleyen bir boyuta da geçer:

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from Standard to Premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports Premium storage
# Skip this step if converting storage from Premium to Standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to Premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $disk.Sku = [Microsoft.Azure.Management.Compute.Models.DiskSku]::new($storageType)
        $disk | Update-AzDisk
    }
}

Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Tek tek yönetilen diskleri Standart ve Premium arasında geçiş

Geliştirme/test iş yükünüz için, maliyetlerinizi azaltmak için Standart ve Premium disklerin bir karışımını isteyebilirsiniz. Yalnızca daha iyi performans gerektiren diskleri yükseltmeyi seçebilirsiniz. Bu örnek, tek bir VM diskinin Standart depolamadan Premium depolama alanına veya Premium depolama alanına nasıl dönüştürüleceğini gösterir. Premium yönetilen diskleri kullanmak için VM'nizin Premium depolama alanını destekleyen bir [VM boyutu](sizes.md) kullanması gerekir. Bu örnek, Premium depolamayı destekleyen bir büyüklüğe nasıl geçilen gösteriş de gösterir:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports Premium storage
# Skip this step if converting storage from Premium to Standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$disk.Sku = [Microsoft.Azure.Management.Compute.Models.DiskSku]::new($storageType)
$disk | Update-AzDisk

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-managed-disks-from-standard-to-premium-in-the-azure-portal"></a>Azure portalında yönetilen diskleri Standart'tan Premium'a dönüştürme

Şu adımları uygulayın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Portaldaki **Sanal makineler** listesinden VM'yi seçin.
3. VM durdurulmazsa, VM **Genel Bakış** bölmesinin üst kısmında **Dur'u** seçin ve VM'nin durmasını bekleyin.
3. VM bölmesinde menüden **Diskler'i** seçin.
4. Dönüştürmek istediğiniz diski seçin.
5. Menüden **Yapılandırma'yı** seçin.
6. Hesap **türünü** **Standart HDD'den** **Premium SSD'ye**değiştirin.
7. **Kaydet'i**tıklatın ve disk bölmesini kapatın.

Disk türü dönüştürme anında. Dönüşümden sonra VM'nizi başlatabilirsiniz.

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Yönetilen diskleri Standart HDD ile Standart SSD arasında değiştirme 

Bu örnek, tek bir VM diskinin Standart HDD'den Standart SSD'ye veya Standart SSD'den Standart HDD'ye nasıl dönüştürüleceğimi gösterir:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Update the storage type
$disk.Sku = [Microsoft.Azure.Management.Compute.Models.DiskSku]::new($storageType)
$disk | Update-AzDisk

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Sonraki adımlar

[Anlık görüntü](snapshot-copy-managed-disk.md)kullanarak VM'nin salt okunur kopyasını yapın.
