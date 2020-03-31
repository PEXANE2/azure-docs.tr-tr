---
title: Sanal makine ölçek kümeleri için yakınlık yerleşim grupları önizlemesi
description: Azure'da Windows sanal makine ölçek kümeleri için yakınlık yerleşim grupları oluşturma ve kullanma hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: 4fa2949e2a7e1b99ac26caa35f967e9dc9cf359a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76273614"
---
# <a name="preview-creating-and-using-proximity-placement-groups-using-powershell"></a>Önizleme: PowerShell kullanarak yakınlık yerleşim grupları oluşturma ve kullanma

VM'leri mümkün olduğunca yakın alabilmek ve mümkün olan en düşük gecikmeyi elde etmek için ölçek kümenizi bir [yakınlık yerleşim grubu](co-location.md#preview-proximity-placement-groups)içinde dağıtmanız gerekir.

Yakınlık yerleşim grubu, Azure hesaplama kaynaklarının fiziksel olarak birbirine yakın olduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimgrupları, düşük gecikme nin bir gereklilik olduğu iş yükleri için yararlıdır.

> [!IMPORTANT]
> Yakınlık Yerleşim Grupları şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
>
> Yakınlık yerleştirme grupları önizleme sırasında bu bölgelerde mevcut değildir: **Japonya Doğu,** **Avustralya Doğu** ve **Hindistan Orta**.


## <a name="create-a-proximity-placement-group"></a>Yakınlık yerleştirme grubu oluşturma
[New-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/new-azproximityplacementgroup) cmdlet'i kullanarak bir yakınlık yerleşim grubu oluşturun. 

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

## <a name="list-proximity-placement-groups"></a>Yakınlık yerleşim gruplarını listele

[Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup) cmdlet'i kullanarak tüm yakınlık yerleşim gruplarını listeleyebilirsiniz.

```azurepowershell-interactive
Get-AzProximityPlacementGroup
```


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma

Ölçek kümesini oluşturmak `-ProximityPlacementGroup $ppg.Id` için [Yeni-AzVMSS](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) kullandığınızda yakınlık yerleşim grubu kimliğine başvurmak için yakınlık yerleşim grubunda bir ölçek oluşturun.

```azurepowershell-interactive
$scalesetName = "myVM"

New-AzVmss `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -VMScaleSetName $scalesetName `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ProximityPlacementGroup $ppg.Id
```

[Get-AzProximityPlacementGroup'u](/powershell/module/az.compute/get-azproximityplacementgroup)kullanarak yerleşim grubunda örneği görebilirsiniz.

```azurepowershell-interactive
  Get-AzProximityPlacementGroup `
   -ResourceId $ppg.Id | Format-Table `
   -Wrap `
   -Property VirtualMachineScaleSets
```

## <a name="next-steps"></a>Sonraki adımlar

Yakınlık yerleşim grupları oluşturmak için [Azure CLI'yi](../virtual-machines/linux/proximity-placement-groups.md) de kullanabilirsiniz.
