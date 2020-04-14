---
title: Azure sanal makine ölçek kümeleriyle Uygulama Durumu uzantısını kullanma
description: Sanal makine ölçeği kümelerinde dağıtılan uygulamalarınızın durumunu izlemek için Uygulama Durumu uzantısını nasıl kullanacağınızı öğrenin.
author: mimckitt
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: mimckitt
ms.openlocfilehash: cb5f1d48bb1a95db004d9da553e19a35071c73b0
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273741"
---
# <a name="using-application-health-extension-with-virtual-machine-scale-sets"></a>Sanal makine ölçek kümeleriyle Uygulama Sistem Durumu uzantısını kullanma
Uygulama sistem durumunuizlemek, dağıtımınızı yönetmek ve yükseltmek için önemli bir sinyaldir. Azure sanal makine ölçek kümeleri, dağıtımınızı yükseltmek için tek tek örneklerin sistem durumu izlemesine dayanan [otomatik işletim sistemi görüntü yükseltmeleri](virtual-machine-scale-sets-automatic-upgrade.md)de dahil olmak üzere [yuvarlanan yükseltmeler](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) için destek sağlar.

Bu makalede, sanal makine ölçeği kümelerinde dağıtılan uygulamalarınızın durumunu izlemek için Uygulama Durumu uzantısını nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, aşina olduğunuzu varsayar:
-   Azure sanal makine [uzantıları](../virtual-machines/extensions/overview.md)
-   Sanal makine ölçek kümelerini [değiştirme](virtual-machine-scale-sets-upgrade-scale-set.md)

## <a name="when-to-use-the-application-health-extension"></a>Uygulama Durumu uzantısı ne zaman kullanılır?
Uygulama Durumu uzantısı sanal bir makine ölçeği kümesi örneği içinde dağıtılır ve ölçek kümesi örneği içinden VM durumu hakkında raporlar. Uzantıyı bir uygulama bitiş noktası üzerinde sonda lamak için yapılandırabilir ve uygulamanın durumunu bu örnekte güncelleştirebilirsiniz. Bu örnek durumu, bir örneğin yükseltme işlemleri için uygun olup olmadığını belirlemek için Azure tarafından denetlenir.

Uzantı bir VM içinden sağlık raporladığı için, uzantı, Uygulama Durumu Sondaları (özel Azure Yük Dengeleyici [sondaları](../load-balancer/load-balancer-custom-probe-overview.md)kullanan) gibi harici sondaların kullanılamayacağı durumlarda kullanılabilir.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Uygulama Durumu uzantısı için şema gösterir. Uzantı, en azından ilişkili bir bağlantı noktası veya istek yolu ile bir "tcp" veya "http" isteği gerektirir.

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

| Adı | Değer / Örnek | Veri Türü
| ---- | ---- | ---- 
| apiVersion | `2018-10-01` | date |
| yayımcı | `Microsoft.ManagedServices` | string |
| type | `ApplicationHealthLinux`(Linux), `ApplicationHealthWindows` (Windows) | string |
| typeHandlerVersion | `1.0` | int |

### <a name="settings"></a>Ayarlar

| Adı | Değer / Örnek | Veri Türü
| ---- | ---- | ----
| protokol | `http` veya `tcp` | string |
| port | Protokol isteğe `http`bağlı olduğunda, protokol zorunlu olduğunda`tcp` | int |
| requestPath | Protokol olduğunda `http`zorunludur, protokol izin verilmez`tcp` | string |

## <a name="deploy-the-application-health-extension"></a>Uygulama Durumu uzantısını dağıtma
Aşağıdaki örneklerde ayrıntılı olarak belirtildiği gibi, Uygulama Durumu uzantısını ölçek kümelerinize dağıtmanın birden çok yolu vardır.

### <a name="rest-api"></a>REST API

Aşağıdaki örnek, Windows tabanlı ölçek kümesinin ölçek kümesi modelindeki uzantı Profiline Uygulama Durumu uzantısını (myHealthExtension adıyla) ekler.

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
Zaten `PATCH` dağıtılan bir uzantıyı yeniden kullanmak için kullanın.

### <a name="azure-powershell"></a>Azure PowerShell

Uygulama Durumu uzantısını ölçek kümesi modeli tanımına eklemek için [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet'i kullanın.

Aşağıdaki örnek, Windows tabanlı ölçek `extensionProfile` kümesinin ölçek kümesi modeline Uygulama Durumu uzantısını ekler. Örnekte yeni Az PowerShell modülü kullanılır.

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

Ölçek kümesi modeli tanımına Uygulama Durumu uzantısını eklemek için [az vmss uzantı kümesini](/cli/azure/vmss/extension#az-vmss-extension-set) kullanın.

Aşağıdaki örnek, Uygulama Durumu uzantısını Linux tabanlı ölçek kümesinin ölçek kümesi modeline ekler.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthLinux \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --settings ./extension.json
```
Extension.json dosya içeriği.

```json
{
  "protocol": "<protocol>",
  "port": "<port>",
  "requestPath": "</requestPath>"
}
```


## <a name="troubleshoot"></a>Sorun giderme
Uzantı yürütme çıktısı aşağıdaki dizinlerde bulunan dosyalara kaydedilir:

```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.ManagedServices.ApplicationHealthWindows\<version>\
```

```Linux
/var/lib/waagent/apphealth
```

Günlükler ayrıca uygulama nın sistem durumu durumunu düzenli olarak yakalar.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı sanal makine ölçeği kümelerinde nasıl [dağıtılayacaklarınıöğrenin.](virtual-machine-scale-sets-deploy-app.md)
