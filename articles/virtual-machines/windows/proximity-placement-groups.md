---
title: Windows VM 'Leri için yakınlık yerleştirme gruplarını kullanma | Microsoft Docs
description: Azure 'da Windows sanal makineleri için yakınlık yerleşimi grupları oluşturma ve kullanma hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: 9e9b8dddf7f48fe672a37b0343d215e0735ed760
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302510"
---
# <a name="preview-deploy-vms-to-proximity-placement-groups-using-powershell"></a>Önizleme: PowerShell kullanarak VM 'Leri yakınlık yerleşimi gruplarına dağıtma


Olası en düşük gecikme süresini elde etmek üzere VM 'Leri olabildiğince yakın bir şekilde almak için, bunları bir [yakınlık yerleşimi grubu](co-location.md#preview-proximity-placement-groups)içinde dağıtmanız gerekir.

Yakınlık yerleşimi grubu, Azure işlem kaynaklarının fiziksel olarak birbirlerine yakın bir yerde bulunduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimi grupları, düşük gecikme süresinin gereksinim olduğu iş yükleri için faydalıdır.

> [!IMPORTANT]
> Yakınlık yerleşimi grupları Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Önizleme sırasında yakınlık yerleşimi grupları bu bölgelerde kullanılamaz: **Japonya Doğu**, **Avustralya Doğu** ve **Hindistan Orta**.


## <a name="create-a-proximity-placement-group"></a>Yakınlık yerleştirme grubu oluşturma
[New-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/new-azproximityplacementgroup) cmdlet 'ini kullanarak bir yakınlık yerleşimi grubu oluşturun. 

```azurepowershell-interactive
$resourceGroup = "myPPGResourceGroup"
$location = "East US"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>Yakınlık yerleşimi gruplarını Listele

[Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup) cmdlet 'ini kullanarak yakınlık yerleşimi gruplarının tümünü listeleyebilirsiniz.

```azurepowershell-interactive
Get-AzProximityPlacementGroup
```


## <a name="create-a-vm"></a>VM oluşturma

Sanal makineyi oluşturmak için [New-azvm](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) kullandığınızda `-ProximityPlacementGroup $ppg.Id` yakınlık yerleşimi grubu kimliğine başvurmak için kullanarak yakınlık yerleşimi grubunda bir VM oluşturun.

```azurepowershell-interactive
$vmName = "myVM"

New-AzVm `
  -ResourceGroupName $resourceGroup `
  -Name $vmName `
  -Location $location `
  -OpenPorts 3389 `
  -ProximityPlacementGroup $ppg.Id
```

Sanal makineyi, [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup)kullanarak yerleştirme grubunda görebilirsiniz.

```azurepowershell-interactive
Get-AzProximityPlacementGroup -ResourceId $ppg.Id |
    Format-Table -Property VirtualMachines -Wrap
```

## <a name="availability-sets"></a>Kullanılabilirlik Kümeleri
Ayrıca, yakınlık yerleşimi grubunuzda bir kullanılabilirlik kümesi oluşturabilirsiniz. Bir kullanılabilirlik kümesi `-ProximityPlacementGroup` oluşturmak için [New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset) cmdlet 'i ile aynı parametreyi kullanın ve kullanılabilirlik kümesinde oluşturulan tüm VM 'lerin aynı yakınlık yerleştirme grubunda de oluşturulması gerekir.

## <a name="scale-sets"></a>Ölçek kümeleri

Ayrıca, yakınlık yerleştirme grubunuzda bir ölçek kümesi de oluşturabilirsiniz. Bir ölçek kümesi `-ProximityPlacementGroup` oluşturmak için [New-azvmss](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) ile aynı parametreyi kullanın ve tüm örnekler aynı yakınlık yerleşimi grubunda oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, yakınlık yerleşimi grupları oluşturmak için [Azure CLI](../linux/proximity-placement-groups.md) 'yi de kullanabilirsiniz.
