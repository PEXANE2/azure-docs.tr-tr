---
title: Azure sanal makine ölçek kümeleriyle uzantı sıralamasını kullanma
description: Sanal makine ölçeği kümelerine birden fazla uzantı dağıtırken uzantı sağlamayı nasıl sıralayın öğrenin.
author: mayanknayar
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: manayar
ms.openlocfilehash: cde3fb8b56d8509a45bde00dde55e3c69d015b8e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278064"
---
# <a name="sequence-extension-provisioning-in-virtual-machine-scale-sets"></a>Sanal makine ölçek kümelerinde sıra uzantısı sağlama
Azure sanal makine uzantıları dağıtım sonrası yapılandırma ve yönetim, izleme, güvenlik ve daha fazlası gibi özellikler sağlar. Üretim dağıtımları genellikle istenen sonuçları elde etmek için VM örnekleri için yapılandırılan birden çok uzantının bir birleşimini kullanır.

Sanal bir makinede birden çok uzantı kullanırken, aynı işletim sistemi kaynaklarına sahip uzantıların bu kaynakları aynı anda elde etmeye çalışmadığından emin olmak önemlidir. Bazı uzantılar, ortam ayarları ve sırlar gibi gerekli yapılandırmaları sağlamak için diğer uzantılara da bağlıdır. Doğru sıralama ve sıralama yerinde olmadan, bağımlı uzantı dağıtımları başarısız olabilir.

Bu makalede, sanal makine ölçek kümelerinde VM örnekleri için yapılandırılacak uzantıları nasıl sıralayabileceğiniz ayrıntılı olarak açıklanmıştır.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, aşina olduğunuzu varsayar:
-   Azure sanal makine [uzantıları](../virtual-machines/extensions/overview.md)
-   Sanal makine ölçek kümelerini [değiştirme](virtual-machine-scale-sets-upgrade-scale-set.md)

## <a name="when-to-use-extension-sequencing"></a>Uzantı sıralaması ne zaman kullanılır?
Uzantıları ölçek kümeleri için zorunlu olmayan şekilde sıralamave belirtilmediği sürece, uzantılar herhangi bir sırada bir ölçek kümesi örneğinde sağlanabilir.

Örneğin, ölçek kümesi modelinizde modelde belirtilen iki uzantıvarsa -ExtensionA ve ExtensionB - varsa, aşağıdaki sağlama dizilerinden biri oluşabilir:
-   ExtensionA -> ExtensionB
-   ExtensionB -> ExtensionA

Uygulamanız Uzantı A'nın Her zaman Uzantısı B'den önce sağlanmasını gerektiriyorsa, bu makalede açıklandığı gibi uzantı sıralamasını kullanmalısınız. Uzantı sıralaması ile artık yalnızca bir sıra oluşur:
-   ExtensionA - > Uzantıb

Tanımlı bir sağlama dizisinde belirtilmeyen uzantılar, tanımlanmış bir diziden önce, sonra veya bu sırada dahil olmak üzere herhangi bir zamanda sağlanabilir. Uzantı sıralaması yalnızca belirli bir uzantınbaşka bir belirli uzantıdan sonra sağlanacak larını belirtir. Modelde tanımlanan başka bir uzantının sağlanmasını etkilemez.

Örneğin, ölçek kümesi modelinizde modelde belirtilen üç uzantı varsa -Uzantı A, Uzantı B ve Uzantı C – belirtilirse ve Uzantı C Uzantısı A'dan sonra sağlanacak şekilde ayarlanmışsa, aşağıdaki sağlama dizilerinden biri oluşabilir:
-   ExtensionA -> ExtensionC -> ExtensionB
-   ExtensionB -> ExtensionA -> ExtensionC
-   ExtensionA -> ExtensionB -> ExtensionC

Tanımlanan uzantı sırası yürütülerken başka bir uzantı sağlanmadığından emin olmanız gerekiyorsa, ölçek kümesi modelinizdeki tüm uzantıları sıralamanızı öneririz. Yukarıdaki örnekte, Uzantı B, Yalnızca bir sıra nın oluşabileceği C Uzantısı'ndan sonra sağlanacak şekilde ayarlanabilir:
-   ExtensionA -> ExtensionC -> ExtensionB


## <a name="how-to-use-extension-sequencing"></a>Uzantı sıralaması nasıl kullanılır?
Dizi uzantısı sağlama için, ölçek kümesi modelindeki uzantı tanımını, bir dizi uzantı adını kabul eden "provisionAfterExtensions" özelliğini içerecek şekilde güncelleştirmeniz gerekir. Özellik dizi değeri belirtilen uzantıları tam ölçek kümesi modelinde tanımlanmış olmalıdır.

### <a name="template-deployment"></a>Şablon Dağıtımı
Aşağıdaki örnek, ölçek kümesinin üç uzantısı olan bir şablonu tanımlar ( ExtensionA, ExtensionB ve ExtensionC – uzantıların sırayla sağlanması gibi:
-   ExtensionA -> ExtensionB -> ExtensionC

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "ExtensionA",
        "properties": {
          "publisher": "ExtensionA.Publisher",
          "settings": {},
          "typeHandlerVersion": "1.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionA"
        }
      },
      {
        "name": "ExtensionB",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionA"
          ],
          "publisher": "ExtensionB.Publisher",
          "settings": {},
          "typeHandlerVersion": "2.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionB"
        }
      }, 
      {
        "name": "ExtensionC",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionB"
          ],
          "publisher": "ExtensionC.Publisher",
          "settings": {},
          "typeHandlerVersion": "3.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionC"                   
        }
      }
    ]
  }
}
```

Özellik "provisionAfterExtensions" uzantı adları bir dizi kabul yana, yukarıdaki örnek ExtensionC ExtensionA ve ExtensionB sonra sağlanan gibi değiştirilebilir, ancak ExtensionA ve ExtensionB arasında herhangi bir sipariş gereklidir. Aşağıdaki şablon bu senaryoyu elde etmek için kullanılabilir:

```json
"virtualMachineProfile": {
  "extensionProfile": {
    "extensions": [
      {
        "name": "ExtensionA",
        "properties": {
          "publisher": "ExtensionA.Publisher",
          "settings": {},
          "typeHandlerVersion": "1.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionA"
        }
      },
      {
        "name": "ExtensionB",
        "properties": {
          "publisher": "ExtensionB.Publisher",
          "settings": {},
          "typeHandlerVersion": "2.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionB"
        }
      }, 
      {
        "name": "ExtensionC",
        "properties": {
          "provisionAfterExtensions": [
            "ExtensionA","ExtensionB"
          ],
          "publisher": "ExtensionC.Publisher",
          "settings": {},
          "typeHandlerVersion": "3.0",
          "autoUpgradeMinorVersion": true,
          "type": "ExtensionC"                   
        }
      }
    ]
  }
}
```

### <a name="rest-api"></a>REST API
Aşağıdaki örnek, bir ölçek kümesi modeline Uzantı c adlı yeni bir uzantı ekler. ExtensionC' in, ölçek kümesi modelinde zaten tanımlanmış olan ExtensionA ve ExtensionB' ye olan bağımlılıkları vardır.

```
PUT on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/ExtensionC?api-version=2018-10-01`
```

```json
{ 
  "name": "ExtensionC",
  "properties": {
    "provisionAfterExtensions": [
      "ExtensionA","ExtensionB"
    ],
    "publisher": "ExtensionC.Publisher",
    "settings": {},
    "typeHandlerVersion": "3.0",
    "autoUpgradeMinorVersion": true,
    "type": "ExtensionC" 
  }                  
}
```

Uzantı, ölçek kümesi modelinde daha önce tanımlanmışsa ve şimdi bağımlılıklarını `PATCH` eklemek istiyorsanız, zaten dağıtılan uzantın özelliklerini düzenlemek için bir uygulama yürütebilirsiniz.

```
PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/extensions/ExtensionC?api-version=2018-10-01`
```

```json
{ 
  "properties": {
    "provisionAfterExtensions": [
      "ExtensionA","ExtensionB"
    ]
  }                  
}
```
Varolan ölçek kümesi örneklerindeki değişiklikler bir sonraki [yükseltmede](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model)uygulanır.

### <a name="azure-powershell"></a>Azure PowerShell
Uygulama Durumu uzantısını ölçek kümesi modeli tanımına eklemek için [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet'i kullanın. Uzatma sıralaması Az PowerShell 1.2.0 veya üzeri kullanımını gerektirir.

Aşağıdaki örnek, Windows tabanlı ölçek `extensionProfile` kümesinin ölçek kümesi modeline [Uygulama Durumu uzantısını](virtual-machine-scale-sets-health-extension.md) ekler. Uygulama Durumu uzantısı, ölçek kümesinde zaten tanımlanan [Özel Komut Dosyası Uzantısı'nı](../virtual-machines/extensions/custom-script-windows.md)sağattıktan sonra sağlanacaktır.

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
  -ProvisionAfterExtension "CustomScriptExtension" `
  -AutoUpgradeMinorVersion $True

# Update the scale set
Update-AzVmss -ResourceGroupName $vmScaleSetResourceGroup `
  -Name $vmScaleSetName `
  -VirtualMachineScaleSet $vmScaleSet
```

### <a name="azure-cli-20"></a>Azure CLI 2.0
Ölçek kümesi modeli tanımına Uygulama Durumu uzantısını eklemek için [az vmss uzantı kümesini](/cli/azure/vmss/extension#az-vmss-extension-set) kullanın. Uzantı sıralaması, Azure CLI 2.0.55 veya üzeri kullanımını gerektirir.

Aşağıdaki örnek, Windows tabanlı ölçek kümesinin ölçek kümesi modeline [Uygulama Durumu uzantısını](virtual-machine-scale-sets-health-extension.md) ekler. Uygulama Durumu uzantısı, ölçek kümesinde zaten tanımlanan [Özel Komut Dosyası Uzantısı'nı](../virtual-machines/extensions/custom-script-windows.md)sağattıktan sonra sağlanacaktır.

```azurecli-interactive
az vmss extension set \
  --name ApplicationHealthWindows \
  --publisher Microsoft.ManagedServices \
  --version 1.0 \
  --resource-group <myVMScaleSetResourceGroup> \
  --vmss-name <myVMScaleSet> \
  --provision-after-extensions CustomScriptExtension \
  --settings ./extension.json
```


## <a name="troubleshoot"></a>Sorun giderme

### <a name="not-able-to-add-extension-with-dependencies"></a>Bağımlılıklarla uzantı ekleyemeyecek misiniz?
1. ProvisionAfterExtensions'da belirtilen uzantıların ölçek kümesi modelinde tanımlandığından emin olun.
2. Dairesel bağımlılıkların uygulanmadığından emin olun. Örneğin, aşağıdaki sıraya izin verilmez: ExtensionA -> ExtensionB -> ExtensionC -> ExtensionA
3. Bağımlılık yaptığınız uzantıların, uzantılı "özellikler" altında bir "ayarlar" özelliğine sahip olduğundan emin olun. Örneğin, ExtentionB'nin ExtensionA'dan sonra sağlanması gerekiyorsa, ExtensionA'nın ExtensionA "özellikleri" altındaki "ayarlar" alanına sahip olması gerekir. Uzantı gerekli ayarları zorunlu değilse boş bir "ayarlar" özelliği belirtebilirsiniz.

### <a name="not-able-to-remove-extensions"></a>Uzantıları kaldıramadınız mı?
Kaldırılan uzantıların diğer uzantılar için provisionAfterExtensions kapsamında listelenmediğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
Uygulamanızı sanal makine ölçeği kümelerinde nasıl [dağıtılayacaklarınıöğrenin.](virtual-machine-scale-sets-deploy-app.md)
