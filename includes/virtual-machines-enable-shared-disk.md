---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 0df74b82c847c9738d97d2001573666714c17672
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81008377"
---
## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="disk-sizes"></a>Disk boyutları

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="deploy-shared-disks"></a>Paylaşılan diskleri dağıtma

### <a name="deploy-a-premium-ssd-as-a-shared-disk"></a>Premium SSD 'yi paylaşılan disk olarak dağıtma

Paylaşılan disk özelliği etkinken yönetilen bir disk dağıtmak için yeni özelliği `maxShares` kullanın ve 1 ' den büyük bir değer tanımlayın. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> Değeri `maxShares` yalnızca bir disk tüm VM 'lerden çıkarıldığınızda ayarlanabilir veya değiştirilebilir. İçin `maxShares`izin verilen değerler için [disk boyutlarına](#disk-sizes) bakın.

Aşağıdaki şablonu kullanmadan önce,,, `[parameters('dataDiskName')]`ve `[resourceGroup().location]` `[parameters('maxShares')]` değerlerini `[parameters('dataDiskSizeGB')]`kendi değerlerinizle değiştirin.

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

### <a name="deploy-an-ultra-disk-as-a-shared-disk"></a>Bir ultra diski paylaşılan disk olarak dağıtma

#### <a name="cli"></a>CLI

Paylaşılan disk özelliği etkinken yönetilen bir disk dağıtmak için, `maxShares` parametreyi 1 ' den büyük bir değere değiştirin. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> Değeri `maxShares` yalnızca bir disk tüm VM 'lerden çıkarıldığınızda ayarlanabilir veya değiştirilebilir. İçin `maxShares`izin verilen değerler için [disk boyutlarına](#disk-sizes) bakın.

```azurecli
#Creating an Ultra shared Disk 
az disk create -g rg1 -n clidisk --size-gb 1024 -l westus --sku UltraSSD_LRS --max-shares 5 --disk-iops-read-write 2000 --disk-mbps-read-write 200 --disk-iops-read-only 100 --disk-mbps-read-only 1

#Updating an Ultra shared Disk 
az disk update -g rg1 -n clidisk --disk-iops-read-write 3000 --disk-mbps-read-write 300 --set diskIopsReadOnly=100 --set diskMbpsReadOnly=1

#Show shared disk properties:
az disk show -g rg1 -n clidisk
```

#### <a name="azure-resource-manager"></a>Azure Resource Manager

Paylaşılan disk özelliği etkinken yönetilen bir disk dağıtmak için, özelliğini `maxShares` kullanın ve 1 ' den büyük bir değer tanımlayın. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> Değeri `maxShares` yalnızca bir disk tüm VM 'lerden çıkarıldığınızda ayarlanabilir veya değiştirilebilir. İçin `maxShares`izin verilen değerler için [disk boyutlarına](#disk-sizes) bakın.

Aşağıdaki şablonu kullanmadan önce,,, `[parameters('dataDiskName')]`, `[resourceGroup().location]`, `[parameters('dataDiskSizeGB')]`, `[parameters('maxShares')]`ve `[parameters('diskIOPSReadWrite')]` `[parameters('diskMBpsReadOnly')]` değerlerini `[parameters('diskMBpsReadWrite')]`kendi `[parameters('diskIOPSReadOnly')]`değerlerinizle değiştirin.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "diskName": {
      "type": "string",
      "defaultValue": "uShared30"
    },
    "location": {
        "type": "string",
        "defaultValue": "westus",
        "metadata": {
                "description": "Location for all resources."
        }
    },
    "dataDiskSizeGB": {
      "type": "int",
      "defaultValue": 1024
    },
    "maxShares": {
      "type": "int",
      "defaultValue": 2
    },
    "diskIOPSReadWrite": {
      "type": "int",
      "defaultValue": 2048
    },
    "diskMBpsReadWrite": {
      "type": "int",
      "defaultValue": 20
    },    
    "diskIOPSReadOnly": {
      "type": "int",
      "defaultValue": 100
    },
    "diskMBpsReadOnly": {
      "type": "int",
      "defaultValue": 1
    }    
  }, 
  "resources": [
    {
        "type": "Microsoft.Compute/disks",
        "name": "[parameters('diskName')]",
        "location": "[parameters('location')]",
        "apiVersion": "2019-07-01",
        "sku": {
            "name": "UltraSSD_LRS"
        },
        "properties": {
            "creationData": {
                "createOption": "Empty"
            },
            "diskSizeGB": "[parameters('dataDiskSizeGB')]",
            "maxShares": "[parameters('maxShares')]",
            "diskIOPSReadWrite": "[parameters('diskIOPSReadWrite')]",
            "diskMBpsReadWrite": "[parameters('diskMBpsReadWrite')]",
            "diskIOPSReadOnly": "[parameters('diskIOPSReadOnly')]",
            "diskMBpsReadOnly": "[parameters('diskMBpsReadOnly')]"
        }
    }
  ]
}
```

### <a name="using-azure-shared-disks-with-your-vms"></a>Sanal makinelerinizdeki Azure Paylaşılan disklerini kullanma

İle `maxShares>1`paylaşılan bir disk dağıttıktan sonra, diski sanal makinelerinizdeki bir veya daha fazlasına bağlayabilirsiniz.

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

Paylaşılan diskler gerçekleştirmeye ilgileniyorsanız, [önizlememiz için kaydolun](https://aka.ms/AzureSharedDiskPreviewSignUp).