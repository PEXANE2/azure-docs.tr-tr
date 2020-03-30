---
title: PowerShell ' ile işletim sistemi diskini Azure VM ile değiştirin
description: PowerShell kullanarak bir Azure sanal makinesi tarafından kullanılan işletim sistemi diskini değiştirin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: ec66892804f3c2d1f831168a2955f2498462cbf3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033021"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-powershell"></a>PowerShell kullanarak Azure VM tarafından kullanılan işletim sistemi diskini değiştirme

Varolan bir VM'iniz varsa, ancak diski bir yedekleme diski veya başka bir işletim sistemi diski ile değiştirmek istiyorsanız, işletim sistemi disklerini değiştirmek için Azure PowerShell'i kullanabilirsiniz. VM'yi silmek ve yeniden oluşturmak zorunda değilsiniz. Zaten kullanılmamış olması durumunda yönetilen bir diski başka bir kaynak grubunda bile kullanabilirsiniz.

 

VM durdurulması\tahsis edilmesi gerekir, sonra yönetilen diskin kaynak kimliği farklı bir yönetilen diskin kaynak kimliği ile değiştirilebilir.

VM boyutunun ve depolama türünün eklemek istediğiniz diskle uyumlu olduğundan emin olun. Örneğin, kullanmak istediğiniz disk Premium Depolama'daysa, VM'nin Premium Depolama özelliğine sahip olması gerekir (DS serisi boyutu gibi). Her iki disk de aynı boyutta olmalıdır.

[Get-AzDisk'i](https://docs.microsoft.com/powershell/module/az.compute/get-azdisk) kullanarak kaynak grubundaki disklerin listesini alma

```azurepowershell-interactive
Get-AzDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```
 
Kullanmak istediğiniz diskin adını aldığınızda, bunu VM'nin işletim sistemi diski olarak ayarlayın. Bu örnek stop\deallocates *myVM* adlı VM ve yeni işletim sistemi diski olarak *newDisk* adlı disk atar. 
 
```azurepowershell-interactive 
# Get the VM 
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzVM -Name $vm.Name -ResourceGroupName myResourceGroup

```

**Sonraki adımlar**

Bir diskin kopyasını oluşturmak için, [bir diskin Anlık Görüntüsü'ne](snapshot-copy-managed-disk.md)bakın.
