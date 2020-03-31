---
title: Ayrılmış diskler nedeniyle sanal makine dağıtımında sorun giderme | Microsoft Dokümanlar
description: Ayrılmış diskler nedeniyle sanal makine dağıtımının giderilmesi
services: virtual-machines-windows
documentationCenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/31/2019
ms.author: vaaga
ms.openlocfilehash: e049a2b914cbf9c4f0ca0f3a1dd0281d58f881b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75486826"
---
# <a name="troubleshoot-virtual-machine-deployment-due-to-detached-disks"></a>Ayrılmış diskler nedeniyle sanal makine dağıtımının giderilmesi

## <a name="symptom"></a>Belirti

Önceki veri diski ayrılması başarısız olan sanal bir makineyi güncelleştirmeye çalışırken, bu hata koduyla karşılaşabilirsiniz.

```
Code=\"AttachDiskWhileBeingDetached\" 
Message=\"Cannot attach data disk '{disk ID}' to virtual machine '{vmName}' because the disk is currently being detached or the last detach  operation failed. Please wait until the disk is completely detached, and then try again or delete/detach the disk explicitly again\” 
```

## <a name="cause"></a>Nedeni

Bu hata, son ayırma işlemi başarısız olan bir veri diskini yeniden eklemeyi denediğinizde oluşur. Bu durumdan çıkmanın en iyi yolu başarısız diski ayırmaktır.

## <a name="solution-1-powershell"></a>Çözüm 1: Powershell

### <a name="step-1-get-the-virtual-machine-and-disk-details"></a>Adım 1: Sanal makine ve disk ayrıntılarını alın

```azurepowershell-interactive
PS D:> $vm = Get-AzureRmVM -ResourceGroupName "Example Resource Group" -Name "ERGVM999999" 

PS D:> $vm.StorageProfile.DataDisks 
lun          : 0
name         : f94901ef-75ee-4477-9ad6-1c74da50e7ef
createOption : Attach
caching      : ReadOnly
managedDisk  : @{storageAccountType=Premium_LRS; id=/subscriptions/<subscription ID>/resourceGroups/<Resource Group name>}
diskSizeGB   : 8
toBeDetached : False 
```

### <a name="step-2-set-the-flag-for-failing-disks-to-true"></a>Adım 2: Başarısız diskler için bayrağı "true" olarak ayarlayın.

Başarısız diskin dizi dizini alın ve başarısız disk için **toBeDetached** bayrağını **(AttachDiskWhileBeingDetached** hatasının oluştuğu) "true" olarak ayarlayın. Bu ayar, diskin sanal makineden ayrılması anlamına gelir. Başarısız disk adı **errorMessage**bulunabilir.

> ! Not: Get and Put aramaları için belirtilen API sürümünün 2019-03-01 veya daha büyük olması gerekir.

```azurepowershell-interactive
PS D:> $vm.StorageProfile.DataDisks[0].ToBeDetached = $true 
```

Alternatif olarak, 01 Mart 2019'dan önce API sürümlerini kullanan kullanıcılar için yararlı olacak aşağıdaki komutu kullanarak bu diski de ayırabilirsiniz.

```azurepowershell-interactive
PS D:> Remove-AzureRmVMDataDisk -VM $vm -Name "<disk ID>" 
```

### <a name="step-3-update-the-virtual-machine"></a>Adım 3: Sanal makineyi güncelleştirin

```azurepowershell-interactive
PS D:> Update-AzureRmVM -ResourceGroupName "Example Resource Group" -VM $vm 
```

## <a name="solution-2-rest"></a>Çözüm 2: REST

### <a name="step-1-get-the-virtual-machine-payload"></a>Adım 1: Sanal makine yükünü alın.

```azurepowershell-interactive
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?$expand=instanceView&api-version=2019-03-01
```

### <a name="step-2-set-the-flag-for-failing-disks-to-true"></a>Adım 2: Başarısız diskler için bayrağı "true" olarak ayarlayın.

Adım 1'de döndürülen yükte başarısız disk için **toBeDetached** bayrağını ayarlayın. Lütfen Dikkat: Get and Put aramaları için belirtilen `2019-03-01` API sürümü veya daha büyük olmalıdır.

**Örnek İstek Gövdesi**

```azurepowershell-interactive
{
  "properties": {
    "hardwareProfile": {
      "vmSize": "Standard_D2_v2"
    },
    "storageProfile": {
      "imageReference": {
        "sku": "2016-Datacenter",
        "publisher": "MicrosoftWindowsServer",
        "version": "latest",
        "offer": "WindowsServer"
      },
      "osDisk": {
        "caching": "ReadWrite",
        "managedDisk": {
          "storageAccountType": "Standard_LRS"
        },
        "name": "myVMosdisk",
        "createOption": "FromImage"
      },
      "dataDisks": [
        {
          "diskSizeGB": 1023,
          "createOption": "Empty",
        "name": "f94901ef-75ee-4477-9ad6-1c74da50e7ef",
          "lun": 0,
          "toBeDetached": true
        },
        {
          "diskSizeGB": 1023,
          "createOption": "Empty",
          "lun": 1,
          "toBeDetached": false
        }
      ]
    },
    "osProfile": {
      "adminUsername": "{your-username}",
      "computerName": "myVM",
      "adminPassword": "{your-password}"
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/{existing-nic-name}",
          "properties": {
            "primary": true
          }
        }
      ]
    }
  }
}
```

Alternatif olarak, 01 Mart 2019 tarihinden önce API sürümlerini kullanan kullanıcılar için yararlı olan başarısız veri diskini yukarıdaki yükten de kaldırabilirsiniz.

### <a name="step-3-update-the-virtual-machine"></a>Adım 3: Sanal makineyi güncelleştirin

Adım 2'de ayarlanan istek gövdesi yükünü kullanın ve sanal makineyi aşağıdaki gibi güncelleştirin:

```azurepowershell-interactive
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2019-03-01
```

**Örnek Yanıt:**

```azurepowershell-interactive
{
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "type": "Microsoft.Compute/virtualMachines",
  "properties": {
    "osProfile": {
      "adminUsername": "{your-username}",
      "secrets": [],
      "computerName": "myVM",
      "windowsConfiguration": {
        "provisionVMAgent": true,
        "enableAutomaticUpdates": true
      }
    },
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/nsgExistingNic",
          "properties": {
            "primary": true
          }
        }
      ]
    },
    "storageProfile": {
      "imageReference": {
        "sku": "2016-Datacenter",
        "publisher": "MicrosoftWindowsServer",
        "version": "latest",
        "offer": "WindowsServer"
      },
      "osDisk": {
        "osType": "Windows",
        "caching": "ReadWrite",
        "createOption": "FromImage",
        "name": "myVMosdisk",
        "managedDisk": {
          "storageAccountType": "Standard_LRS"
        }
      },
      "dataDisks": [
        {
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Standard_LRS"
          },
          "createOption": "Empty",
          "lun": 0,
          "diskSizeGB": 1023,
        "name": "f94901ef-75ee-4477-9ad6-1c74da50e7ef",
          "toBeDetached": true
        },
        {
          "caching": "None",
          "managedDisk": {
            "storageAccountType": "Standard_LRS"
          },
          "createOption": "Empty",
          "lun": 1,
          "diskSizeGB": 1023,
          "toBeDetached": false
        }
      ]
    },
    "vmId": "3906fef9-a1e5-4b83-a8a8-540858b41df0",
    "hardwareProfile": {
      "vmSize": "Standard_D2_v2"
    },
    "provisioningState": "Updating"
  },
  "name": "myVM",
  "location": "westus"
}
```

## <a name="next-steps"></a>Sonraki Adımlar

VM'nize bağlanmada sorun yaşıyorsanız, [bir Azure VM'ye bağlı SORUN Giderme RDP bağlantılarına](troubleshoot-rdp-connection.md)bakın.

VM'nizde çalışan uygulamalara erişimle ilgili sorunlar için [Windows VM'deki Sorun Giderme uygulaması bağlantısı sorunlarına](troubleshoot-app-connection.md)bakın.
