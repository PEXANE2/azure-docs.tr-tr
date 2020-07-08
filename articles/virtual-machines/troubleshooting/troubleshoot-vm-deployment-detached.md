---
title: Ayrılmış diskler nedeniyle sanal makine dağıtımı sorunlarını giderme | Microsoft Docs
description: Ayrılmış diskler nedeniyle sanal makine dağıtımı sorunlarını giderme
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75486826"
---
# <a name="troubleshoot-virtual-machine-deployment-due-to-detached-disks"></a>Ayrılmış diskler nedeniyle sanal makine dağıtımı sorunlarını giderme

## <a name="symptom"></a>Belirti

Önceki veri diski bağlantısı başarısız olan bir sanal makineyi güncelleştirmeye çalışırken, bu hata kodu boyunca gelebilir.

```
Code=\"AttachDiskWhileBeingDetached\" 
Message=\"Cannot attach data disk '{disk ID}' to virtual machine '{vmName}' because the disk is currently being detached or the last detach  operation failed. Please wait until the disk is completely detached, and then try again or delete/detach the disk explicitly again\” 
```

## <a name="cause"></a>Nedeni

Son ayırma işlemi başarısız olan bir veri diskini yeniden iliştirçalıştığınızda bu hata oluşur. Bu durumdan ayrılmak için en iyi yol, hatalı diski ayırmanız olur.

## <a name="solution-1-powershell"></a>Çözüm 1: PowerShell

### <a name="step-1-get-the-virtual-machine-and-disk-details"></a>1. Adım: sanal makineyi ve disk ayrıntılarını alın

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

### <a name="step-2-set-the-flag-for-failing-disks-to-true"></a>2. Adım: başarısız disklerin bayrağını "true" olarak ayarlayın.

Başarısız olan diskin dizi dizinini alın ve hatalı disk için **Tobeayrılan** bayrağını ( **Attachdiskwhilebeingayrılan** hata oluştuğunda) "true" olarak ayarlayın. Bu ayar, diski sanal makineden ayırmayı gerektirir. Hatalı disk adı **ErrorMessage**içinde bulunabilir.

> ! Note: get ve put çağrıları için belirtilen API sürümünün 2019-03-01 veya daha büyük olması gerekir.

```azurepowershell-interactive
PS D:> $vm.StorageProfile.DataDisks[0].ToBeDetached = $true 
```

Alternatif olarak, aşağıdaki komutu kullanarak bu diski de ayırabilirsiniz, bu da, 01 Mart 2019 ' den önceki API sürümlerini kullanan kullanıcılar için yararlı olacaktır.

```azurepowershell-interactive
PS D:> Remove-AzureRmVMDataDisk -VM $vm -Name "<disk ID>" 
```

### <a name="step-3-update-the-virtual-machine"></a>3. Adım: sanal makineyi güncelleştirme

```azurepowershell-interactive
PS D:> Update-AzureRmVM -ResourceGroupName "Example Resource Group" -VM $vm 
```

## <a name="solution-2-rest"></a>Çözüm 2: REST

### <a name="step-1-get-the-virtual-machine-payload"></a>1. Adım: sanal makine yükünü alın.

```azurepowershell-interactive
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?$expand=instanceView&api-version=2019-03-01
```

### <a name="step-2-set-the-flag-for-failing-disks-to-true"></a>2. Adım: başarısız disklerin bayrağını "true" olarak ayarlayın.

Adım 1 ' de döndürülen yükte hatalı disk için **Tobeayrılan** bayrağını doğru olarak ayarlayın. Lütfen unutmayın: get ve put çağrıları için belirtilen API sürümünün olması `2019-03-01` veya daha büyük olması gerekir.

**Örnek Istek gövdesi**

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

Alternatif olarak, API sürümlerini kullanan kullanıcılar için, 01 Mart 2019 ' den önce yardımcı olan yukarıdaki yük üzerinden başarısız olan veri diskini da kaldırabilirsiniz.

### <a name="step-3-update-the-virtual-machine"></a>3. Adım: sanal makineyi güncelleştirme

2. adımdaki istek gövdesi yük kümesini kullanın ve sanal makineyi şu şekilde güncelleştirin:

```azurepowershell-interactive
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}?api-version=2019-03-01
```

**Örnek yanıt:**

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

Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure VM 'ye YÖNELIK RDP bağlantılarında sorun giderme](troubleshoot-rdp-connection.md).

VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [WINDOWS VM 'de uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).
