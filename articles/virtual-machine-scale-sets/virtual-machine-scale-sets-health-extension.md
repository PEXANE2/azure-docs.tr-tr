---
title: Azure sanal makine ölçek kümeleri ile uygulama durumu uzantısı 'nı kullanma
description: Sanal makine ölçek kümelerine dağıtılan uygulamalarınızın sistem durumunu izlemek için uygulama durumu uzantısının nasıl kullanılacağını öğrenin.
author: mayanknayar
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: manayar
ms.openlocfilehash: 37d93b04e6755512eac6c2a168bd2a04f8ac298f
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76275887"
---
# <a name="using-application-health-extension-with-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleri ile uygulama durumu uzantısı 'nı kullanma
Uygulamanızın sistem durumunu izlemek, dağıtımınızı yönetmek ve yükseltmek için önemli bir sinyaldir. Azure sanal makine ölçek kümeleri, dağıtımınızı yükseltmek için ayrı örneklerin sistem durumu izlemesini kullanan [Otomatik işletim sistemi görüntüsü yükseltmeleri](virtual-machine-scale-sets-automatic-upgrade.md)dahil olmak üzere, çalışan [yükseltmeler](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) için destek sağlar.

Bu makalede, sanal makine ölçek kümelerine dağıtılan uygulamalarınızın sistem durumunu izlemek için uygulama sistem durumu uzantısını nasıl kullanabileceğiniz açıklanır.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, hakkında bilgi sahibi olduğunuz varsayılmaktadır:
-   Azure sanal makine [uzantıları](../virtual-machines/extensions/overview.md)
-   Sanal makine ölçek kümelerini [değiştirme](virtual-machine-scale-sets-upgrade-scale-set.md)

## <a name="when-to-use-the-application-health-extension"></a>Uygulama durumu uzantısının ne zaman kullanılacağı
Uygulama sistem durumu uzantısı bir sanal makine ölçek kümesi örneği içine dağıtılır ve VM sistem durumu üzerinde, ölçek kümesi örneği içinden raporlar. Uzantıyı bir uygulama uç noktasında araştırma yapmak ve bu örnekteki uygulamanın durumunu güncelleştirmek için yapılandırabilirsiniz. Bu örnek durumu, bir örneğin yükseltme işlemlerine uygun olup olmadığını öğrenmek için Azure tarafından denetlenir.

Uzantı, bir VM içinden sistem durumunu raporladığında, uzantı, uygulama sistem durumu Araştırmaları (özel Azure Load Balancer [yoklamaları](../load-balancer/load-balancer-custom-probe-overview.md)kullanan) gibi dış araştırmaların kullanılamaz durumda olabilir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON, uygulama sistem durumu uzantısının şemasını gösterir. Uzantı, en azından ilişkili bağlantı noktası veya istek yoluyla bir "TCP" ya da "http" isteği gerektirir.

```json
{
  "type": "extensions",
  "name": "HealthExtension",
  "apiVersion": "2018-10-01",
  "location": "<location>",  
  "properties": {
    "publisher": "Microsoft.ManagedServices",
    "type": "< ApplicationHealthLinux or ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}  
```

### <a name="property-values"></a>Özellik değerleri

| Ad | Değer / örnek | Veri Türü
| ---- | ---- | ---- 
| apiVersion | `2018-10-01` | date |
| publisher | `Microsoft.ManagedServices` | string |
| type | `ApplicationHealthLinux` (Linux), `ApplicationHealthWindows` (Windows) | string |
| typeHandlerVersion | `1.0` | int |

### <a name="settings"></a>Ayarlar

| Ad | Değer / örnek | Veri Türü
| ---- | ---- | ----
| protocol | `http` veya `tcp` | string |
| port | Protokol `http`olduğunda isteğe bağlı, protokol `tcp` olduğunda zorunludur | int |
| Istek yolu | Protokol `http`olduğunda zorunlu, protokol `tcp` izin verilmez | string |

## <a name="deploy-the-application-health-extension"></a>Uygulama durumu uzantısını dağıtma
Aşağıdaki örneklerde açıklandığı şekilde, uygulama durumu uzantısını ölçek kümelerinizi dağıtmanın birden çok yolu vardır.

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, uygulama durumu uzantısını (myHealthExtension adlı) Windows tabanlı ölçek kümesinin ölçek kümesi modelinde extensionProfile öğesine ekler.

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/myHealthExtension?api-version=2018-10-01`
```

```json
{
  "name": "myHealthExtension",
  "properties": {
    "publisher": " Microsoft.ManagedServices",
    "type": "< ApplicationHealthWindows>",
    "autoUpgradeMinorVersion": true,
    "typeHandlerVersion": "1.0",
    "settings": {
      "protocol": "<protocol>",
      "port": "<port>",
      "requestPath": "</requestPath>"
    }
  }
}
```
Zaten dağıtılmış bir uzantıyı düzenlemek için `PATCH` kullanın.

### <a name="azure-powershell"></a>Azure PowerShell

Uygulama durumu uzantısını ölçek kümesi model tanımına eklemek için [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet 'ini kullanın.

Aşağıdaki örnek, uygulama sistem durumu uzantısını Windows tabanlı ölçek kümesinin ölçek kümesi modelinde `extensionProfile` ekler. Örnek, yeni az PowerShell modülünü kullanır.

```azurepowershell-interactive
# Define the scale set variables
$vmScaleSetName = "myVMScaleSet"
$vmScaleSetResourceGroup = "myVMScaleSetResourceGroup"

# Define the Application Health extension properties
$publicConfig = @{"protocol" = "http"; "port" = 80; "requestPath" = "/healthEndpoint"};
$extensionName = "myHealthExtension"
$extensionType = "ApplicationHealthWindows"
$publisher = "Microsoft.ManagedServices"

# Get the scale set object
$vmScaleSet = Get-AzVmss `
  -ResourceGroupName $vmScaleSetResourceGroup `
  -VMScaleSetName $vmScaleSetName

# Add the Application Health extension to the scale set model
Add-AzVmssExtension -VirtualMachineScaleSet $vmScaleSet `
  -Name $extensionName `
  -Publisher $publisher `
  -Setting $publicConfig `
  -Type $extensionType `
  -TypeHandlerVersion "1.0" `
  -AutoUpgradeMinorVersion $True

# Update the scale set
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup `
  -Name $vmScaleSetName `
  -VirtualMachineScaleSet $vmScaleSet
```


### <a name="azure-cli-20"></a>Azure CLI 2.0

Ölçek kümesi model tanımına uygulama durumu uzantısı eklemek için [az VMSS Extension set](/cli/azure/vmss/extension#az-vmss-extension-set) komutunu kullanın.

Aşağıdaki örnek, uygulama durumu uzantısını Linux tabanlı ölçek kümesinin ölçek kümesi modeline ekler.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthLinux \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --settings ./extension.json
```
Uzantı. JSON dosya içeriği.

```json
{
  "protocol": "<protocol>",
  "port": "<port>",
  "requestPath": "</requestPath>"
}
```


## <a name="troubleshoot"></a>Sorun giderme
Uzantı yürütme çıktısı, aşağıdaki dizinlerde bulunan dosyalara kaydedilir:

```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.ManagedServices.ApplicationHealthWindows\<version>\
```

```Linux
/var/lib/waagent/apphealth
```

Günlükler Ayrıca uygulamanın sistem durumunu düzenli olarak yakalar.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı sanal makine ölçek kümelerinde [dağıtmayı](virtual-machine-scale-sets-deploy-app.md) öğrenin.
