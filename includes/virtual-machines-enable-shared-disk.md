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
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81008377"
---
## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="disk-sizes"></a>Disk boyutları

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="deploy-shared-disks"></a>Paylaşılan diskleri dağıtma

### <a name="deploy-a-premium-ssd-as-a-shared-disk"></a>Premium SSD'yi paylaşılan disk olarak dağıtma

Paylaşılan disk özelliği etkinleştirilmiş yönetilen bir disk dağıtmak `maxShares` için yeni özelliği kullanın ve 1'den büyük bir değer tanımlayın. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> `maxShares` Değeri yalnızca bir disk tüm VM'lerden söküldüğünde ayarlanabilir veya değiştirilebilir. Için izin verilen değerler için `maxShares`Disk [boyutlarını](#disk-sizes) görün.

Aşağıdaki şablonu kullanmadan `[parameters('dataDiskName')]` `[resourceGroup().location]`önce, değiştirin , , `[parameters('dataDiskSizeGB')]`, ve `[parameters('maxShares')]` kendi değerleri ile.

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

### <a name="deploy-an-ultra-disk-as-a-shared-disk"></a>Ultra diski paylaşılan disk olarak dağıtma

#### <a name="cli"></a>CLI

Paylaşılan disk özelliği etkinleştirilmiş yönetilen bir disk `maxShares` dağıtmak için parametreyi 1'den büyük bir değere değiştirin. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> `maxShares` Değeri yalnızca bir disk tüm VM'lerden söküldüğünde ayarlanabilir veya değiştirilebilir. Için izin verilen değerler için `maxShares`Disk [boyutlarını](#disk-sizes) görün.

```azurecli
#Creating an Ultra shared Disk 
az disk create -g rg1 -n clidisk --size-gb 1024 -l westus --sku UltraSSD_LRS --max-shares 5 --disk-iops-read-write 2000 --disk-mbps-read-write 200 --disk-iops-read-only 100 --disk-mbps-read-only 1

#Updating an Ultra shared Disk 
az disk update -g rg1 -n clidisk --disk-iops-read-write 3000 --disk-mbps-read-write 300 --set diskIopsReadOnly=100 --set diskMbpsReadOnly=1

#Show shared disk properties:
az disk show -g rg1 -n clidisk
```

#### <a name="azure-resource-manager"></a>Azure Resource Manager

Paylaşılan disk özelliği etkinleştirilmiş yönetilen bir disk `maxShares` dağıtmak için özelliği kullanın ve 1'den büyük bir değer tanımlayın. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> `maxShares` Değeri yalnızca bir disk tüm VM'lerden söküldüğünde ayarlanabilir veya değiştirilebilir. Için izin verilen değerler için `maxShares`Disk [boyutlarını](#disk-sizes) görün.

Aşağıdaki şablonu kullanmadan `[parameters('dataDiskName')]` `[resourceGroup().location]`önce, `[parameters('diskIOPSReadWrite')]`kendi `[parameters('diskMBpsReadWrite')]` `[parameters('diskIOPSReadOnly')]`değerlerinizle `[parameters('diskMBpsReadOnly')]` değiştirin. `[parameters('dataDiskSizeGB')]` `[parameters('maxShares')]`

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

### <a name="using-azure-shared-disks-with-your-vms"></a>Azure paylaşılan diskleri VM'lerinizle kullanma

Paylaşılan bir diski dağıttıktan `maxShares>1`sonra, diski VM'lerinizden birine veya daha fazlasına monte edebilirsiniz.

> [!IMPORTANT]
> Bir diski paylaşan tüm VM'ler aynı [yakınlık yerleşim grubunda](../articles/virtual-machines/windows/proximity-placement-groups.md)dağıtılmalıdır.

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

Paylaşılan diski kümenizde VM'lerinize monte ettikten sonra, çoğunluk oluşturabilir ve SCSI PR'ı kullanarak diske okuma/yazma yapabilirsiniz. Azure paylaşılan diskleri kullanırken aşağıdaki PR komutları kullanılabilir:

Diskle etkileşimkurmak için kalıcı rezervasyon eylem listesiyle başlayın:

```
PR_REGISTER_KEY 

PR_REGISTER_AND_IGNORE 

PR_GET_CONFIGURATION 

PR_RESERVE 

PR_PREEMPT_RESERVATION 

PR_CLEAR_RESERVATION 

PR_RELEASE_RESERVATION 
```

PR_RESERVE, PR_PREEMPT_RESERVATION veya PR_RELEASE_RESERVATION kullanırken aşağıdaki kalıcı rezervasyon türünden birini sağlayın:

```
PR_NONE 

PR_WRITE_EXCLUSIVE 

PR_EXCLUSIVE_ACCESS 

PR_WRITE_EXCLUSIVE_REGISTRANTS_ONLY 

PR_EXCLUSIVE_ACCESS_REGISTRANTS_ONLY 

PR_WRITE_EXCLUSIVE_ALL_REGISTRANTS 

PR_EXCLUSIVE_ACCESS_ALL_REGISTRANTS 
```

Ayrıca, PR_RESERVE, PR_REGISTER_AND_IGNORE, PR_REGISTER_KEY, PR_PREEMPT_RESERVATION, PR_CLEAR_RESERVATION veya PR_RELEASE-RESERVATION kullanırken kalıcı rezervasyon anahtarı sağlamanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan diskleri denemek istiyorsanız [önizlememize kaydolun.](https://aka.ms/AzureSharedDiskPreviewSignUp)