---
title: Bir Windows VM 'den veri diski ayırma-Azure
description: Kaynak Yöneticisi dağıtım modelini kullanarak Azure 'daki bir sanal makineden bir veri diskini ayırın.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 01/08/2020
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 301f3abd26f702f3f29c8833c835ba7d0e41bcaf
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834597"
---
# <a name="how-to-detach-a-data-disk-from-a-windows-virtual-machine"></a>Bir Windows sanal makinesinden veri diski ayırma

Sanal makineye bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. Bu, diski sanal makineden kaldırır, ancak depolama alanından kaldırmaz.

> [!WARNING]
> Bir diski ayırdıysanız otomatik olarak silinmez. Premium depolamaya abone olduysa, disk için depolama ücretleri uygulanmaya devam edersiniz. Daha fazla bilgi için [Premium Depolama kullanılırken fiyatlandırma ve faturalandırma](disks-types.md#billing)bölümüne bakın.

Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı sanal makineye veya başka birine yeniden ekleyebilirsiniz.

 

## <a name="detach-a-data-disk-using-powershell"></a>PowerShell kullanarak veri diskini ayırma

PowerShell kullanarak bir veri diskini *etkin* bir şekilde kaldırabilirsiniz, ancak diskin VM 'den kullanımdan çıkarmadan önce hiçbir şeyin etkin bir şekilde kullanıldığından emin olun.

Bu örnekte, **Myresourcegroup** kaynak grubundaki **myvm** VM 'sinden **mydisk** adlı diski kaldırdık. Önce [Remove-AzVMDataDisk](https://docs.microsoft.com/powershell/module/az.compute/remove-azvmdatadisk) cmdlet 'ini kullanarak diski kaldırırsınız. Ardından, veri diskini kaldırma işlemini gerçekleştirmek için [Update-AzVM](https://docs.microsoft.com/powershell/module/az.compute/update-azvm) cmdlet 'ini kullanarak sanal makinenin durumunu güncelleştirin.

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
1. **Diskler** bölmesinin üst kısmında **Düzenle**' yi seçin.
1. **Diskler** bölmesinde, ayırmak istediğiniz veri diskinin en sağında yer alan **Ayır**' ı seçin.
1. Değişikliklerinizi kaydetmek için sayfanın üst kısmındaki **Kaydet** ' i seçin.

Disk depolamada kalır, ancak artık bir sanal makineye bağlı değildir.

## <a name="next-steps"></a>Sonraki adımlar

Veri diskini yeniden kullanmak istiyorsanız, [bunu başka BIR VM 'ye](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ekleyebilirsiniz
