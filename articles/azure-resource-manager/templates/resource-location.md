---
title: Şablon kaynak konumu
description: Azure Resource Manager şablonunda (ARM şablonu) kaynak konumunun nasıl ayarlanacağını açıklar.
ms.topic: conceptual
ms.date: 09/04/2019
ms.custom: ''
ms.openlocfilehash: 84a818109e6681b8d0e18de4d2d7969310582818
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96922396"
---
# <a name="set-resource-location-in-arm-template"></a>ARM şablonunda kaynak konumunu ayarlama

Azure Resource Manager şablonu (ARM şablonu) dağıttığınızda, her kaynak için bir konum sağlamanız gerekir. Konumun kaynak grubu konumuyla aynı konumda olması gerekmez.

## <a name="get-available-locations"></a>Kullanılabilir konumları al

Farklı konumlarda farklı kaynak türleri desteklenir. Kaynak türü için desteklenen konumları almak üzere Azure PowerShell veya Azure CLı kullanın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes `
  | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az provider show \
  --namespace Microsoft.Batch \
  --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" \
  --out table
```

---

## <a name="use-location-parameter"></a>Konum parametresini kullan

Şablonunuzun dağıtımı sırasında esneklik sağlamak için, kaynakların konumunu belirtmek için bir parametre kullanın. Parametresinin varsayılan değerini olarak ayarlayın `resourceGroup().location` .

Aşağıdaki örnekte, parametresi olarak belirtilen bir konuma dağıtılan bir depolama hesabı gösterilmektedir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('storage', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-07-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Şablon işlevlerinin tam listesi için bkz. [ARM şablon işlevleri](template-functions.md).
* Şablon dosyaları hakkında daha fazla bilgi için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
