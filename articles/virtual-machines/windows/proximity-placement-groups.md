---
title: 'PowerShell: yakınlık yerleştirme gruplarını kullanın'
description: Azure PowerShell kullanarak yakınlık yerleşimi grupları oluşturma ve kullanma hakkında bilgi edinin.
services: virtual-machines
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: cynthn
ms.openlocfilehash: 7d529335d18f9016ea7a02ac9b03461e3784b08e
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "78204455"
---
# <a name="deploy-vms-to-proximity-placement-groups-using-powershell"></a>PowerShell kullanarak VM 'Leri yakınlık yerleşimi gruplarına dağıtma


Olası en düşük gecikme süresini elde etmek üzere VM 'Leri olabildiğince yakın bir şekilde almak için, bunları bir [yakınlık yerleşimi grubu](co-location.md#proximity-placement-groups)içinde dağıtmanız gerekir.

Yakınlık yerleşimi grubu, Azure işlem kaynaklarının fiziksel olarak birbirlerine yakın bir yerde bulunduğundan emin olmak için kullanılan mantıksal bir gruplandırmadır. Yakınlık yerleşimi grupları, düşük gecikme süresinin gereksinim olduğu iş yükleri için faydalıdır.


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

Sanal makineyi oluşturmak için [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) kullandığınızda yakınlık YERLEŞIMI grubu kimliğine başvurmak için `-ProximityPlacementGroup $ppg.Id` kullanarak yakınlık yerleşimi grubunda bir VM oluşturun.

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

### <a name="move-an-existing-vm-into-a-proximity-placement-group"></a>Var olan bir VM 'yi bir yakınlık yerleşimi grubuna taşıma

Ayrıca, bir yakınlık yerleşimi grubuna var olan bir VM ekleyebilirsiniz. Önce VM 'yi durdurup yeniden başlatmanız gerekir, sonra VM 'yi güncelleştirip yeniden başlatın.

```azurepowershell-interactive
$ppg = Get-AzProximityPlacementGroup -ResourceGroupName myPPGResourceGroup -Name myPPG
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Stop-AzVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName
Update-AzVM -VM $vm -ResourceGroupName $vm.ResourceGroupName -ProximityPlacementGroupId $ppg.Id
Start-AzVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName
```

### <a name="move-an-existing-vm-out-of-a-proximity-placement-group"></a>Var olan bir VM 'yi bir yakınlık yerleşimi grubundan taşıyın

Bir sanal makineyi bir yakınlık yerleşimi grubundan kaldırmak için önce VM 'yi durdurup, ardından VM 'yi güncelleştirip yeniden başlatmanız gerekir.

```azurepowershell-interactive
$ppg = Get-AzProximityPlacementGroup -ResourceGroupName myPPGResourceGroup -Name myPPG
$vm = Get-AzVM -ResourceGroupName myResourceGroup -Name myVM
Stop-AzVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName
$vm.ProximityPlacementGroupId = ""
Update-AzVM -VM $vm -ResourceGroupName $vm.ResourceGroupName 
Start-AzVM -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName
```


## <a name="availability-sets"></a>Kullanılabilirlik Kümeleri
Ayrıca, yakınlık yerleşimi grubunuzda bir kullanılabilirlik kümesi oluşturabilirsiniz. Bir kullanılabilirlik kümesi oluşturmak için [New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset) cmdlet 'i ile aynı `-ProximityPlacementGroup` parametresini kullanın ve kullanılabilirlik kümesinde oluşturulan tüm VM 'lerin aynı yakınlık yerleştirme grubunda de oluşturulması gerekir.

Bir yakınlık yerleşimi grubuna var olan bir kullanılabilirlik kümesi eklemek veya kaldırmak için, önce kullanılabilirlik kümesindeki tüm VM 'Leri durdurmanız gerekir. 

### <a name="move-an-existing-availability-set-into-a-proximity-placement-group"></a>Var olan bir kullanılabilirlik kümesini yakınlık yerleşimi grubuna Taşı

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

### <a name="move-an-existing-availability-set-out-of-a-proximity-placement-group"></a>Var olan bir kullanılabilirlik kümesini yakınlık yerleşimi grubundan taşıyın

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

Ayrıca, yakınlık yerleştirme grubunuzda bir ölçek kümesi de oluşturabilirsiniz. Bir ölçek kümesi oluşturmak için [New-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) ile aynı `-ProximityPlacementGroup` parametresini kullanın ve tüm örnekler aynı yakınlık yerleşimi grubunda oluşturulur.


Bir yakınlık yerleşimi grubuna var olan bir ölçek kümesi eklemek veya kaldırmak için, önce ölçek kümesini durdurmanız gerekir. 

### <a name="move-an-existing-scale-set-into-a-proximity-placement-group"></a>Mevcut bir ölçek kümesini yakınlık yerleşimi grubuna taşıma

```azurepowershell-interactive
$ppg = Get-AzProximityPlacementGroup -ResourceGroupName myPPG -Name myPPG
$vmss = Get-AzVmss -ResourceGroupName myVMSSResourceGroup -VMScaleSetName myScaleSet
Stop-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName
Update-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName -ProximityPlacementGroupId $ppg.Id
Start-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName
```

### <a name="move-an-existing-scale-set-out-of-a-proximity-placement-group"></a>Varolan ölçek kümesini bir yakınlık yerleşimi grubundan taşıyın

```azurepowershell-interactive
$vmss = Get-AzVmss -ResourceGroupName myVMSSResourceGroup -VMScaleSetName myScaleSet
Stop-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName
$vmss.ProximityPlacementGroup = ""
Update-AzVmss -VirtualMachineScaleSet $vmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName  
Start-AzVmss -VMScaleSetName $vmss.Name -ResourceGroupName $vmss.ResourceGroupName
```

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, yakınlık yerleşimi grupları oluşturmak için [Azure CLI](../linux/proximity-placement-groups.md) 'yi de kullanabilirsiniz.
