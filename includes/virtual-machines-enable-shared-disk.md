---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/14/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 29a90b94db5e6e5791361bad004efcf649e1950b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86500625"
---
## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [virtual-machines-disks-shared-limitations](virtual-machines-disks-shared-limitations.md)]

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Paylaşılan diskler çeşitli işletim sistemlerini destekler. Desteklenen işletim sistemleri için kavramsal makalenin [Windows](../articles/virtual-machines/windows/disks-shared.md#windows) ve [Linux](../articles/virtual-machines/linux/disks-shared.md#linux) bölümlerine bakın.

## <a name="disk-sizes"></a>Disk boyutları

[!INCLUDE [virtual-machines-disks-shared-sizes](virtual-machines-disks-shared-sizes.md)]

## <a name="deploy-shared-disks"></a>Paylaşılan diskleri dağıtma

### <a name="deploy-a-premium-ssd-as-a-shared-disk"></a>Premium SSD 'yi paylaşılan disk olarak dağıtma

Paylaşılan disk özelliği etkinken yönetilen bir disk dağıtmak için yeni özelliği kullanın `maxShares` ve 1 ' den büyük bir değer tanımlayın. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> Değeri `maxShares` yalnızca bir disk tüm VM 'lerden çıkarıldığınızda ayarlanabilir veya değiştirilebilir. İçin izin verilen değerler için [disk boyutlarına](#disk-sizes) bakın `maxShares` .

#### <a name="cli"></a>CLI
```azurecli

az disk create -g myResourceGroup -n mySharedDisk --size-gb 1024 -l westcentralus --sku PremiumSSD_LRS --max-shares 2

```

#### <a name="powershell"></a>PowerShell
```azurepowershell-interactive

$datadiskconfig = New-AzDiskConfig -Location 'WestCentralUS' -DiskSizeGB 1024 -AccountType PremiumSSD_LRS -CreateOption Empty

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'mySharedDisk' -Disk $datadiskconfig

```

#### <a name="azure-resource-manager"></a>Azure Resource Manager
Aşağıdaki şablonu kullanmadan önce,,, `[parameters('dataDiskName')]` `[resourceGroup().location]` `[parameters('dataDiskSizeGB')]` ve `[parameters('maxShares')]` değerlerini kendi değerlerinizle değiştirin.

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

Paylaşılan disk özelliği etkinken yönetilen bir disk dağıtmak için, `maxShares` parametreyi 1 ' den büyük bir değere değiştirin. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> Değeri `maxShares` yalnızca bir disk tüm VM 'lerden çıkarıldığınızda ayarlanabilir veya değiştirilebilir. İçin izin verilen değerler için [disk boyutlarına](#disk-sizes) bakın `maxShares` .

#### <a name="cli"></a>CLI
```azurecli
#Creating an Ultra shared Disk 
az disk create -g rg1 -n clidisk --size-gb 1024 -l westus --sku UltraSSD_LRS --max-shares 5 --disk-iops-read-write 2000 --disk-mbps-read-write 200 --disk-iops-read-only 100 --disk-mbps-read-only 1

#Updating an Ultra shared Disk 
az disk update -g rg1 -n clidisk --disk-iops-read-write 3000 --disk-mbps-read-write 300 --set diskIopsReadOnly=100 --set diskMbpsReadOnly=1

#Show shared disk properties:
az disk show -g rg1 -n clidisk
```

#### <a name="powershell"></a>PowerShell
```azurepowershell-interactive

$datadiskconfig = New-AzDiskConfig -Location 'WestCentralUS' -DiskSizeGB 1024 -AccountType UltraSSD_LRS -CreateOption Empty -DiskIOPSReadWrite 2000 -DiskMBpsReadWrite 200 -DiskIOPSReadOnly 100 -DiskMBpsReadOnly 1

New-AzDisk -ResourceGroupName 'myResourceGroup' -DiskName 'mySharedDisk' -Disk $datadiskconfig

```

#### <a name="azure-resource-manager"></a>Azure Resource Manager

Paylaşılan disk özelliği etkinken yönetilen bir disk dağıtmak için, özelliğini kullanın `maxShares` ve 1 ' den büyük bir değer tanımlayın. Bu, diski birden çok VM arasında paylaşılabilir hale getirir.

> [!IMPORTANT]
> Değeri `maxShares` yalnızca bir disk tüm VM 'lerden çıkarıldığınızda ayarlanabilir veya değiştirilebilir. İçin izin verilen değerler için [disk boyutlarına](#disk-sizes) bakın `maxShares` .

Aşağıdaki şablonu kullanmadan önce,,,,,, `[parameters('dataDiskName')]` `[resourceGroup().location]` `[parameters('dataDiskSizeGB')]` `[parameters('maxShares')]` `[parameters('diskIOPSReadWrite')]` `[parameters('diskMBpsReadWrite')]` `[parameters('diskIOPSReadOnly')]` ve `[parameters('diskMBpsReadOnly')]` değerlerini kendi değerlerinizle değiştirin.

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

İle paylaşılan bir disk dağıttıktan sonra `maxShares>1` , diski sanal makinelerinizdeki bir veya daha fazlasına bağlayabilirsiniz.

```azurepowershell-interactive

$resourceGroup = "myResourceGroup"
$location = "WestCentralUS"

$vm = New-AzVm -ResourceGroupName $resourceGroup -Name "myVM" -Location $location -VirtualNetworkName "myVnet" -SubnetName "mySubnet" -SecurityGroupName "myNetworkSecurityGroup" -PublicIpAddressName "myPublicIpAddress"

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
