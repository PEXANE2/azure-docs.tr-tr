---
title: Azure 'da Windows VM 'yi yeniden boyutlandırma
description: Bir Azure sanal makinesi için kullanılan VM boyutunu değiştirin.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: sizes
ms.workload: infrastructure
ms.topic: article
ms.date: 01/13/2020
ms.author: cynthn
ms.openlocfilehash: f456af143ac6ec21bcb9b0c3ec75635c51f748ef
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82099895"
---
# <a name="resize-a-windows-vm"></a>Bir Windows VM'yi yeniden boyutlandırma

Bu makalede, bir sanal makineyi farklı bir [VM boyutuna](sizes.md)nasıl taşıyacağınız gösterilmektedir.

Bir sanal makine (VM) oluşturduktan sonra VM boyutunu değiştirerek VM 'yi yukarı veya aşağı ölçeklendirebilirsiniz. Bazı durumlarda, önce VM 'yi serbest bırakın. Yeni boyut, şu anda VM 'yi barındıran donanım kümesinde yoksa bu durum oluşabilir.

VM 'niz Premium Depolama kullanıyorsa, Premium Depolama desteğini almak için boyutun bir **s** sürümünü seçtiğinizden emin olun. Örneğin, Standard_E4_v3 yerine Standard_E4**s**_v3 seçin.

## <a name="use-the-portal"></a>Portalı kullanma

1. [Azure Portal](https://portal.azure.com)açın.
1. Sanal makine için sayfayı açın.
1. Sol taraftaki menüde **Boyut**' u seçin.
1. Kullanılabilir boyutlar listesinden yeni bir boyut seçip **yeniden boyutlandır**' ı seçin.


Sanal makine şu anda çalışıyorsa boyutunu değiştirmek, yeniden başlatılmasına neden olur. Sanal makineyi durdurmak ek boyutları açığa çıkabilir.

## <a name="use-powershell-to-resize-a-vm-not-in-an-availability-set"></a>Kullanılabilirlik kümesinde bulunmayan bir VM 'yi yeniden boyutlandırmak için PowerShell 'i kullanma

Bazı değişkenleri ayarlayın. Değerleri kendi bilgileriniz ile değiştirin.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

VM 'nin barındırıldığı donanım kümesinde kullanılabilir olan VM boyutlarını listeleyin. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

İstediğiniz boyut listeleniyorsa, sanal makineyi yeniden boyutlandırmak için aşağıdaki komutları çalıştırın. İstenen boyut listelenmemişse adım 3 ' e gidin.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

İstediğiniz boyut listelenmemişse, VM 'yi serbest bırakmak, yeniden boyutlandırmak ve VM 'yi yeniden başlatmak için aşağıdaki komutları çalıştırın. ** \<Newvmsize>** öğesini istediğiniz boyutla değiştirin.
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> VM 'nin ayırmayı kaldırma, sanal makineye atanan tüm dinamik IP adreslerini yayınlar. İşletim sistemi ve veri diskleri etkilenmez. 
> 
> 

## <a name="use-powershell-to-resize-a-vm-in-an-availability-set"></a>Kullanılabilirlik kümesindeki bir VM 'yi yeniden boyutlandırmak için PowerShell 'i kullanma

Bir kullanılabilirlik kümesindeki bir VM için yeni boyut, şu anda VM 'yi barındıran donanım kümesinde yoksa, VM 'nin yeniden boyutlandırılması için kullanılabilirlik kümesindeki tüm VM 'Lerin serbest bırakılmasıyla sonuçlanır. Ayrıca, bir VM yeniden boyutlandırılırken kullanılabilirlik kümesindeki diğer VM 'lerin boyutunu güncelleştirmeniz gerekebilir. Bir kullanılabilirlik kümesindeki bir VM 'yi yeniden boyutlandırmak için aşağıdaki adımları gerçekleştirin.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

VM 'nin barındırıldığı donanım kümesinde kullanılabilir olan VM boyutlarını listeleyin. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

İstenen boyut listeleniyorsa, sanal makineyi yeniden boyutlandırmak için aşağıdaki komutları çalıştırın. Listede yoksa bir sonraki bölüme gidin.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
İstediğiniz boyut listelenmemişse, kullanılabilirlik kümesindeki tüm VM 'Leri serbest bırakmak, VM 'Leri yeniden boyutlandırmak ve yeniden başlatmak için aşağıdaki adımlarla devam edin.

Kullanılabilirlik kümesindeki tüm VM 'Leri durdurun.
   
```powershell
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

Kullanılabilirlik kümesindeki VM 'Leri yeniden boyutlandırın ve yeniden başlatın.
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>Sonraki adımlar

Ek ölçeklenebilirlik için birden fazla sanal makine örneği çalıştırın ve ölçeği ölçeklendirin. Daha fazla bilgi için bkz. [sanal makine ölçek kümesindeki Windows makinelerini otomatik olarak ölçeklendirme](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

