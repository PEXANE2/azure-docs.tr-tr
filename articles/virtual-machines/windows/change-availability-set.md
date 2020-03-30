---
title: VM kullanılabilirlik kümesini değiştirme
description: Azure PowerShell'i kullanarak sanal makinenizin kullanılabilirlik kümesini nasıl değiştireceğinizi öğrenin.
ms.service: virtual-machines
author: cynthn
ms.topic: article
ms.date: 01/31/2020
ms.author: cynthn
ms.openlocfilehash: 092dafff6622d3402322eb96d0fe4215e52e16b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76964932"
---
# <a name="change-the-availability-set-for-a-vm"></a>Bir sanal makinenin kullanılabilirlik kümesini değiştirme
Aşağıdaki adımlar, Azure PowerShell kullanarak bir VM kullanılabilirlik kümesini nasıl değiştireceğinizaçıklanmaktadır. VM yalnızca oluşturulduğunda kullanılabilirlik kümesine eklenebilir. Kullanılabilirlik kümesini değiştirmek için sanal makineyi silmeniz ve sonra yeniden oluşturmanız gerekir. 

Bu makale hem Linux hem de Windows VM'leri için geçerlidir.

Bu makale en son 2/12/2019 tarihinde [Azure Bulut Kabuğu](https://shell.azure.com/powershell) ve Az [PowerShell modülü](https://docs.microsoft.com/powershell/azure/install-az-ps) sürümü 1.2.0 kullanılarak test edilmiştir.


## <a name="change-the-availability-set"></a>Kullanılabilirlik kümesini değiştirme 

Aşağıdaki komut dosyası, gerekli bilgileri toplama, özgün VM'yi silme ve ardından yeni bir kullanılabilirlik kümesinde yeniden oluşturma örneği sağlar.

```powershell
# Set variables
    $resourceGroup = "myResourceGroup"
    $vmName = "myVM"
    $newAvailSetName = "myAvailabilitySet"

# Get the details of the VM to be moved to the Availability Set
    $originalVM = Get-AzVM `
       -ResourceGroupName $resourceGroup `
       -Name $vmName

# Create new availability set if it does not exist
    $availSet = Get-AzAvailabilitySet `
       -ResourceGroupName $resourceGroup `
       -Name $newAvailSetName `
       -ErrorAction Ignore
    if (-Not $availSet) {
    $availSet = New-AzAvailabilitySet `
       -Location $originalVM.Location `
       -Name $newAvailSetName `
       -ResourceGroupName $resourceGroup `
       -PlatformFaultDomainCount 2 `
       -PlatformUpdateDomainCount 2 `
       -Sku Aligned
    }
    
# Remove the original VM
    Remove-AzVM -ResourceGroupName $resourceGroup -Name $vmName    

# Create the basic configuration for the replacement VM. 
    $newVM = New-AzVMConfig `
       -VMName $originalVM.Name `
       -VMSize $originalVM.HardwareProfile.VmSize `
       -AvailabilitySetId $availSet.Id
 
# For a Linux VM, change the last parameter from -Windows to -Linux 
    Set-AzVMOSDisk `
       -VM $newVM -CreateOption Attach `
       -ManagedDiskId $originalVM.StorageProfile.OsDisk.ManagedDisk.Id `
       -Name $originalVM.StorageProfile.OsDisk.Name `
       -Windows

# Add Data Disks
    foreach ($disk in $originalVM.StorageProfile.DataDisks) { 
    Add-AzVMDataDisk -VM $newVM `
       -Name $disk.Name `
       -ManagedDiskId $disk.ManagedDisk.Id `
       -Caching $disk.Caching `
       -Lun $disk.Lun `
       -DiskSizeInGB $disk.DiskSizeGB `
       -CreateOption Attach
    }
    
# Add NIC(s) and keep the same NIC as primary
    foreach ($nic in $originalVM.NetworkProfile.NetworkInterfaces) {    
    if ($nic.Primary -eq "True")
        {
            Add-AzVMNetworkInterface `
            -VM $newVM `
            -Id $nic.Id -Primary
            }
        else
            {
              Add-AzVMNetworkInterface `
              -VM $newVM `
              -Id $nic.Id 
                }
    }

# Recreate the VM
    New-AzVM `
       -ResourceGroupName $resourceGroup `
       -Location $originalVM.Location `
       -VM $newVM `
       -DisableBginfoExtension
```

## <a name="next-steps"></a>Sonraki adımlar

Ek bir [veri diski](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)ekleyerek VM'nize ek depolama alanı ekleyin.

