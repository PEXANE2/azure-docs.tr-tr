---
title: Azure'da yönetilen bir resimden VM oluşturma
description: Kaynak Yöneticisi dağıtım modelinde Azure PowerShell veya Azure portalını kullanarak genelleştirilmiş yönetilen bir görüntüden bir Windows sanal makinesi oluşturun.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: de59edc2e2c702993efd6187a590264d9aac16a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74841940"
---
# <a name="create-a-vm-from-a-managed-image"></a>Yönetilen bir görüntüden VM oluşturma

Azure portalını veya PowerShell'i kullanarak Azure yönetilen bir VM görüntüden birden çok sanal makine (VM) oluşturabilirsiniz. Yönetilen bir VM görüntüsü, işletim sistemi ve veri diskleri de dahil olmak üzere bir VM oluşturmak için gereken bilgileri içerir. Hem işletim sistemi diskleri hem de herhangi bir veri diski de dahil olmak üzere görüntüyü oluşturan sanal sabit diskler (VHD'ler) yönetilen diskler olarak depolanır. 

Yeni bir VM oluşturmadan önce, kaynak görüntü olarak kullanmak ve görüntüye erişimi olması gereken herhangi bir kullanıcıya görüntü üzerinde okuma erişimi vermek için [yönetilen bir VM görüntüsü oluşturmanız](capture-image-resource.md) gerekir. 


## <a name="use-the-portal"></a>Portalı kullanma

1. Yönetilen bir görüntü bulmak için [Azure portalına](https://portal.azure.com) gidin. **Resimleri**arayın ve seçin.
3. Listeden kullanmak istediğiniz resmi seçin. Resim **Genel Bakış** sayfası açılır.
4. Menüden **VM Oluştur'u** seçin.
5. Sanal makine bilgilerini girin. Buraya girilen kullanıcı adı ve şifre sanal makineye giriş yapmak için kullanılacaktır. Tamamlandığında **Tamam'ı**seçin. Varolan bir kaynak grubunda yeni VM oluşturabilir veya VM'yi depolamak için yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçebilirsiniz.
6. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü Görüntüle'yi** seçin veya **Desteklenen disk türü** filtresini değiştirin. 
7. **Ayarlar**altında, gerektiği gibi değişiklikler yapın ve **Tamam'ı**seçin. 
8. Özet sayfasında, resim adınızı **Özel resim**olarak listelemelisiniz. Sanal makine dağıtımını başlatmak için **Tamam'ı** seçin.


## <a name="use-powershell"></a>PowerShell kullanma

PowerShell'i, [New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) cmdlet için basitleştirilmiş parametre kümesini kullanarak görüntüden VM oluşturmak için kullanabilirsiniz. Görüntünün VM'yi oluşturacağınız kaynak grubunda olması gerekir.

 

[New-AzVm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) için basitleştirilmiş parametre kümesi, yalnızca görüntüden bir VM oluşturmak için bir ad, kaynak grubu ve görüntü adı sağlamanızı gerektirir. New-AzVm, **-Ad** parametresinin değerini otomatik olarak oluşturduğu tüm kaynakların adı olarak kullanır. Bu örnekte, kaynakların her biri için daha ayrıntılı adlar sağlarız, ancak cmdlet'in bunları otomatik olarak oluşturmasına izin veriyoruz. Ayrıca, sanal ağ gibi kaynakları önceden oluşturabilir ve kaynak adını cmdlet'e geçirebilirsiniz. New-AzVm, varolan kaynakları kendi adlarıyla bulabilirse kullanır.

Aşağıdaki örnek, *myImage*adlı resimden *myVMFromImage*adlı bir VM oluşturur. *myResourceGroup* 


```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Sonraki adımlar
[Azure PowerShell modülü yle Windows VM'leri oluşturun ve yönetin](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

