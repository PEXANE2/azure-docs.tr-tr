---
title: Bir Windows VM 'den veri diski ayırma-Azure
description: Kaynak Yöneticisi dağıtım modelini kullanarak Azure 'daki bir sanal makineden bir veri diskini ayırın.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 01/08/2020
ms.author: cynthn
ms.openlocfilehash: cae75c88b4803912565e010f744a7757a3b98f04
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98201561"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Bir Windows sanal makinesindeki veri diskini ayırma

Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu, diski sanal makineden kaldırır, ancak depolama alanından kaldırmaz.

> [!WARNING]
> Bir diski ayırdıysanız otomatik olarak silinmez. Premium depolamaya abone olduysa, disk için depolama ücretleri uygulanmaya devam edersiniz. Daha fazla bilgi için [Premium Depolama kullanılırken fiyatlandırma ve faturalandırma](../disks-types.md#billing)bölümüne bakın.

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.

 

## <a name="detach-a-data-disk-using-powershell"></a>PowerShell kullanarak veri diskini ayırma

PowerShell kullanarak bir veri diskini *etkin* bir şekilde kaldırabilirsiniz, ancak diskin VM 'den kullanımdan çıkarmadan önce hiçbir şeyin etkin bir şekilde kullanıldığından emin olun.

Bu örnekte, **Myresourcegroup** kaynak grubundaki **myvm** VM 'sinden **mydisk** adlı diski kaldırdık. Önce [Remove-AzVMDataDisk](/powershell/module/az.compute/remove-azvmdatadisk) cmdlet 'ini kullanarak diski kaldırırsınız. Ardından, veri diskini kaldırma işlemini gerçekleştirmek için [Update-AzVM](/powershell/module/az.compute/update-azvm) cmdlet 'ini kullanarak sanal makinenin durumunu güncelleştirin.

```azurepowershell-interactive
$VirtualMachine = Get-AzVM `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVM"
Remove-AzVMDataDisk `
   -VM $VirtualMachine `
   -Name "myDisk"
Update-AzVM `
   -ResourceGroupName "myResourceGroup" `
   -VM $VirtualMachine
```

Disk depolamada kalır, ancak artık bir sanal makineye bağlı değildir.

## <a name="detach-a-data-disk-using-the-portal"></a>Portalı kullanarak veri diski çıkarma

Bir veri diskini *etkin* bir şekilde kaldırabilirsiniz, ancak diski VM 'den kullanımdan kaldırmadan önce hiçbir şeyin etkin bir şekilde kullanıldığından emin olun.

1. Sol taraftaki menüden **sanal makineler**' i seçin.
1. Ayırmak istediğiniz veri diskine sahip sanal makineyi seçin.
1. **Ayarlar**'ın altında **Diskler**’i seçin.
1. **Diskler** bölmesinde, ayırmak istediğiniz veri diskinin en sağında bulunan **X** Sil düğmesine tıklayın.
1. Değişikliklerinizi kaydetmek için sayfanın üst kısmındaki **Kaydet** ' i seçin.

Disk depolamada kalır, ancak artık bir sanal makineye bağlı değildir.

## <a name="next-steps"></a>Sonraki adımlar

Veri diskini yeniden kullanmak istiyorsanız, [bunu başka BIR VM 'ye](attach-managed-disk-portal.md) ekleyebilirsiniz
