---
title: Azure'da bir Windows VM'yi yeniden boyutlandırma
description: Azure sanal makinesi için kullanılan VM boyutunu değiştirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 01/13/2020
ms.author: cynthn
ms.openlocfilehash: 6718804d4635edb2628b53017ab9d377928afad8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75941734"
---
# <a name="resize-a-windows-vm"></a>Bir Windows VM'yi yeniden boyutlandırma

Bu makalede, bir VM'yi farklı bir [VM boyutuna](sizes.md)nasıl taşıyabileceğinizi gösterir.

Sanal bir makine (VM) oluşturduktan sonra, VM boyutunu değiştirerek VM'yi yukarı veya aşağı ölçeklendirebilirsiniz. Bazı durumlarda, önce VM'nin yerini tespit etmelidir. Yeni boyut şu anda VM barındıran donanım kümesinde kullanılamıyorsa bu durum olabilir.

VM'niz Premium Depolama kullanıyorsa, Premium Depolama desteği almak için boyutun bir **sürümünü** seçtiğinizden emin olun. Örneğin, Standard_E4_v3 yerine Standard_E4 **_v3'ı**seçin.

## <a name="use-the-portal"></a>Portalı kullanma

1. Azure [portalını](https://portal.azure.com)açın.
1. Sanal makine için sayfayı açın.
1. Sol menüde **Boyut'u**seçin.
1. Kullanılabilir boyutlar listesinden yeni bir boyut seçin ve ardından **Yeniden Boyutlandırma'yı**seçin.


Sanal makine şu anda çalışıyorsa, boyutunu değiştirmek yeniden başlatılmasına neden olur. Sanal makineyi durdurmak ek boyutlar ortaya çıkarabilir.

## <a name="use-powershell-to-resize-a-vm-not-in-an-availability-set"></a>Kullanılabilirlik kümesinde olmayan bir VM'yi yeniden boyutlandırmak için PowerShell'i kullanın

Bazı değişkenler ayarlayın. Değerleri kendi bilgilerinizle değiştirin.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

VM'nin barındırıldığı donanım kümesinde bulunan VM boyutlarını listele. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

İstediğiniz boyut listelenmişse, VM'yi yeniden boyutlandırmak için aşağıdaki komutları çalıştırın. İstenilen boyut listelenmemişse, adım 3'e geçin.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

İstediğiniz boyut listelenmezse, VM'yi bulmak, yeniden boyutlandırmak ve VM'yi yeniden başlatmak için aşağıdaki komutları çalıştırın. ** \<Yeni VMsize>** istediğiniz boyutla değiştirin.
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> VM'nin ayrılması, VM'ye atanan dinamik IP adreslerini serbest bırakır. İşletim sistemi ve veri diskleri etkilenmez. 
> 
> 

## <a name="use-powershell-to-resize-a-vm-in-an-availability-set"></a>Kullanılabilirlik kümesinde VM'yi yeniden boyutlandırmak için PowerShell'i kullanın

Kullanılabilirlik kümesindeki bir VM'nin yeni boyutu şu anda VM'yi barındıran donanım kümesinde kullanılamıyorsa, kullanılabilirlik kümesindeki tüm VM'lerin VM'yi yeniden boyutlandırmak için ayrılması gerekir. Ayrıca, bir VM yeniden boyutlandırıldıktan sonra kullanılabilirlik kümesindeki diğer VM'lerin boyutunu güncelleştirmeniz gerekebilir. Bir kullanılabilirlik kümesinde VM'yi yeniden boyutlandırmak için aşağıdaki adımları gerçekleştirin.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

VM'nin barındırıldığı donanım kümesinde bulunan VM boyutlarını listele. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

İstenilen boyut listelenirse, VM'yi yeniden boyutlandırmak için aşağıdaki komutları çalıştırın. Listede yoksa, bir sonraki bölüme gidin.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
İstediğiniz boyut listelenmezse, kullanılabilirlik kümesindeki tüm VM'leri yeniden belirlemek, VM'leri yeniden boyutlandırmak ve yeniden başlatmak için aşağıdaki adımları takip edin.

Kullanılabilirlik kümesindeki tüm VM'leri durdurun.
   
```powershell
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

Kullanılabilirlik kümesindeki VM'leri yeniden boyutlandırın ve yeniden başlatın.
   
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

Ek ölçeklenebilirlik için birden çok VM örneği çalıştırın ve ölçeklendirin. Daha fazla bilgi için bkz. [Sanal Makine Ölçeği Kümesi'ndeki Windows makinelerini otomatik olarak ölçeklendirin.](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)

