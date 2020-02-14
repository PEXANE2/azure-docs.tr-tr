---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4487beffd0e6a0643f4d3d88f5bb342f23eb848f
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202261"
---
## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="disk-sizes"></a>Disk boyutları

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="deploy-an-azure-shared-disk"></a>Azure Paylaşılan diski dağıtma

Paylaşılan disk özelliği etkinken yönetilen bir disk dağıtmak için yeni özelliği `maxShares` kullanın ve bir değer `>1`tanımlayın. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> `maxShares` değeri yalnızca bir disk tüm VM 'lerden çıkarıldığınızda ayarlanabilir veya değiştirilebilir. `maxShares`için izin verilen değerler için [disk boyutlarına](#disk-sizes) bakın.

Aşağıdaki şablonu kullanmadan önce `[parameters('dataDiskName')]`, `[resourceGroup().location]`, `[parameters('dataDiskSizeGB')]`ve `[parameters('maxShares')]` değerlerini kendi değerlerinizle değiştirin.

```json
{ 
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "dataDiskName": {
      "type": "string",
      "defaultValue": "mySharedDisk"
    },
    "dataDiskSizeGB": {
      "type": "int",
      "defaultValue": 1024
    },
    "maxShares": {
      "type": "int",
      "defaultValue": 2
    }
  },
  "resources": [
    {
      "type": "Microsoft.Compute/disks",
      "name": "[parameters('dataDiskName')]",
      "location": "[resourceGroup().location]",
      "apiVersion": "2019-07-01",
      "sku": {
        "name": "Premium_LRS"
      },
      "properties": {
        "creationData": {
          "createOption": "Empty"
        },
        "diskSizeGB": "[parameters('dataDiskSizeGB')]",
        "maxShares": "[parameters('maxShares')]"
      }
    }
  ] 
}
```

### <a name="using-azure-shared-disks-with-your-vms"></a>Sanal makinelerinizdeki Azure Paylaşılan disklerini kullanma

`maxShares>1`ile paylaşılan bir disk dağıttıktan sonra, diski bir veya daha fazla sanal makinelerinize bağlayabilirsiniz.

> [!IMPORTANT]
> Bir diski paylaşan tüm VM 'Lerin aynı [yakınlık yerleşimi grubuna](../articles/virtual-machines/windows/proximity-placement-groups.md)dağıtılması gerekir.

```azurepowershell-interactive

$resourceGroup = "myResourceGroup"
$location = "WestCentralUS"
$ppgName = "myPPG"
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard

$vm = New-AzVm -ResourceGroupName $resourceGroup -Name "myVM" -Location $location -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress" -ProximityPlacementGroup $ppg.Id

$dataDisk = Get-AzDisk -ResourceGroupName $resourceGroup -DiskName "mySharedDisk"

$vm = Add-AzVMDataDisk -VM $vm -Name "mySharedDisk" -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 0

update-AzVm -VM $vm -ResourceGroupName $resourceGroup
```

## <a name="supported-scsi-pr-commands"></a>Desteklenen SCSI PR komutları

Paylaşılan diski kümenize sanal makinelerinize bağladıktan sonra, çekirdek oluşturabilir ve SCSI PR kullanarak diske okuma/yazma yapabilirsiniz. Azure paylaşılan diskler kullanılırken aşağıdaki PR komutları kullanılabilir:

Diskle etkileşimde bulunmak için kalıcı rezervasyon eylem listesiyle başlayın:

```
PR_REGISTER_KEY 

PR_REGISTER_AND_IGNORE 

PR_GET_CONFIGURATION 

PR_RESERVE 

PR_PREEMPT_RESERVATION 

PR_CLEAR_RESERVATION 

PR_RELEASE_RESERVATION 
```

PR_RESERVE, PR_PREEMPT_RESERVATION veya PR_RELEASE_RESERVATION kullanırken, şu kalıcı rezervasyon türünden birini sağlayın:

```
PR_NONE 

PR_WRITE_EXCLUSIVE 

PR_EXCLUSIVE_ACCESS 

PR_WRITE_EXCLUSIVE_REGISTRANTS_ONLY 

PR_EXCLUSIVE_ACCESS_REGISTRANTS_ONLY 

PR_WRITE_EXCLUSIVE_ALL_REGISTRANTS 

PR_EXCLUSIVE_ACCESS_ALL_REGISTRANTS 
```

PR_RESERVE, PR_REGISTER_AND_IGNORE, PR_REGISTER_KEY, PR_PREEMPT_RESERVATION, PR_CLEAR_RESERVATION veya PR_RELEASE AYıRMAYı kullanırken de kalıcı rezervasyon anahtarı sağlamanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan diskler gerçekleştirmeye ilgileniyorsanız, [önizlememiz için kaydolun](https://aka.ms/shareddisksignup).