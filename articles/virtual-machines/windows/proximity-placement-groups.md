---
title: 'PowerShell: Yakınlık yerleşim işlemi gruplarını kullan'
description: Azure PowerShell'i kullanarak yakınlık yerleşim grupları oluşturma ve kullanma hakkında bilgi edinin.
services: virtual-machines
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: cynthn
ms.openlocfilehash: f69e245d72a63b942896cdd9f4a2225cb4c1706d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78208534"
---
# <a name="deploy-vms-to-proximity-placement-groups-using-powershell"></a>PowerShell'i kullanarak yakın yerleşim gruplarına VM dağıtma


VM'leri mümkün olduğunca yakın hale getirmek ve mümkün olan en düşük gecikmeyi elde etmek için, bunları bir [yakınlık yerleşim grubu](co-location.md#proximity-placement-groups)içinde dağıtmanız gerekir.

Yakınlık yerleşim grubu, Azure hesaplama kaynaklarının fiziksel olarak birbirine yakın olduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimgrupları, düşük gecikme nin bir gereklilik olduğu iş yükleri için yararlıdır.


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


## <a name="create-a-vm"></a>VM oluşturma

VM'yi oluşturmak `-ProximityPlacementGroup $ppg.Id` için [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) kullandığınızda yakınlık yerleşim grubu kimliğine başvurmak için yakınlık yerleşim grubunda bir VM oluşturun.

```azurepowershell-interactive
$vmName = "myVM"

New-AzVm `
  -ResourceGroupName $resourceGroup `
  -Name $vmName `
  -Location $location `
  -OpenPorts 3389 `
  -ProximityPlacementGroup $ppg.Id
```

[Get-AzProximityPlacementGroup'u](/powershell/module/az.compute/get-azproximityplacementgroup)kullanarak yerleşim grubunda VM'yi görebilirsiniz.

```azurepowershell-interactive
Get-AzProximityPlacementGroup -ResourceId $ppg.Id |
    Format-Table -Property VirtualMachines -Wrap
```

### <a name="move-an-existing-vm-into-a-proximity-placement-group"></a>Varolan bir VM'yi yakınlık yerleşim grubuna taşıma

Ayrıca, bir yakınlık yerleşim grubuna varolan bir VM ekleyebilirsiniz. Önce VM'yi durdurup, sonra VM'yi güncelleştirmeli ve yeniden başlatmalısınız.

```azurepowershell-interactive
$ppg = Get-AzProximityPlacementGroup -ResourceGroupName myPPGResourceGroup -Name myPPG
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Stop-AzVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName
Update-AzVM -VM $vm -ResourceGroupName $vm.ResourceGroupName -ProximityPlacementGroupId $ppg.Id
Start-AzVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName
```

### <a name="move-an-existing-vm-out-of-a-proximity-placement-group"></a>Varolan bir VM'yi yakınlık yerleşim grubundan çıkarma

Bir VM'yi yakınlık yerleşim grubundan kaldırmak için, önce VM'yi durdurmanız, sonra VM'yi güncelleştirmeniz ve yeniden başlatmanız gerekir.

```azurepowershell-interactive
$ppg = Get-AzProximityPlacementGroup -ResourceGroupName myPPGResourceGroup -Name myPPG
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Stop-AzVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName
$vm.ProximityPlacementGroup = ""
Update-AzVM -VM $vm -ResourceGroupName $vm.ResourceGroupName 
Start-AzVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName
```


## <a name="availability-sets"></a>Kullanılabilirlik Kümeleri
Ayrıca, yakınlık yerleşim grubunuzun içinde bir kullanılabilirlik kümesi de oluşturabilirsiniz. Kullanılabilirlik `-ProximityPlacementGroup` kümesi oluşturmak için [New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset) cmdlet ile aynı parametreyi kullanın ve kullanılabilirlik kümesinde oluşturulan tüm VM'ler de aynı yakınlık yerleşim grubunda oluşturulur.

Bir yakınlık yerleşim grubuna varolan kullanılabilirlik kümesini eklemek veya kaldırmak için, öncelikle kullanılabilirlik kümesindeki tüm VM'leri durdurmanız gerekir. 

### <a name="move-an-existing-availability-set-into-a-proximity-placement-group"></a>Varolan kullanılabilirlik kümesini yakınlık yerleşim grubuna taşıma

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$avSetName = "myAvailabilitySet"
$avSet = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup -Name $avSetName
$vmIds = $avSet.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 

$ppg = Get-AzProximityPlacementGroup -ResourceGroupName myPPG -Name myPPG
Update-AzAvailabilitySet -AvailabilitySet $avSet -ProximityPlacementGroupId $ppg.Id
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName 
    } 
```

### <a name="move-an-existing-availability-set-out-of-a-proximity-placement-group"></a>Yakınlık yerleşim grubundan ayarlanan varolan kullanılabilirliği taşıma

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$avSetName = "myAvailabilitySet"
$avSet = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup -Name $avSetName
$vmIds = $avSet.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 

$avSet.ProximityPlacementGroup = ""
Update-AzAvailabilitySet -AvailabilitySet $avSet 
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName 
    } 
```

## <a name="scale-sets"></a>Ölçek kümeleri

Ayrıca, yakınlık yerleşim grubunda bir ölçek kümesi de oluşturabilirsiniz. Bir ölçek `-ProximityPlacementGroup` kümesi oluşturmak için [Yeni AzVmss](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) ile aynı parametreyi kullanın ve tüm örnekler aynı yakınlık yerleşim grubunda oluşturulacak.


Yakınlık yerleşim grubuna varolan ölçek kümesini eklemek veya kaldırmak için önce ölçek kümesini durdurmanız gerekir. 

### <a name="move-an-existing-scale-set-into-a-proximity-placement-group"></a>Varolan bir ölçek kümesini yakınlık yerleşim grubuna taşıma

```azurepowershell-interactive
$ppg = Get-AzProximityPlacementGroup -ResourceGroupName myPPG -Name myPPG
$vmss = Get-AzVmss -ResourceGroupName myVMSSResourceGroup -VMScaleSetName myScaleSet
Stop-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName
Update-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName -ProximityPlacementGroupId $ppg.Id
Start-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName
```

### <a name="move-an-existing-scale-set-out-of-a-proximity-placement-group"></a>Yakınlık yerleşim grubundan ayarlanan varolan bir ölçeği taşıma

```azurepowershell-interactive
$vmss = Get-AzVmss -ResourceGroupName myVMSSResourceGroup -VMScaleSetName myScaleSet
Stop-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName
$vmss.ProximityPlacementGroup = ""
Update-AzVmss -VirtualMachineScaleSet $vmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName  
Start-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Yakınlık yerleşim grupları oluşturmak için [Azure CLI'yi](../linux/proximity-placement-groups.md) de kullanabilirsiniz.
