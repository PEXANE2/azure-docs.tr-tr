---
title: Kaynakları dağıtma, kaynak grubu & abonelik leri çapraz olarak dağıtma
description: Dağıtım sırasında birden fazla Azure aboneliği ve kaynak grubunu nasıl hedefleyeceklerini gösterir.
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: 70868f5a3598c26ffff81f0ad3536a6c5c0a7e53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460356"
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Azure kaynaklarını birden fazla abonelik veya kaynak grubuna dağıtma

Genellikle, şablonunuzdaki tüm kaynakları tek bir [kaynak grubuna](../management/overview.md)dağıtırsınız. Ancak, bir dizi kaynağı birlikte dağıtmak ancak bunları farklı kaynak gruplarına veya aboneliklere yerleştirmek istediğiniz senaryolar vardır. Örneğin, Azure Site Kurtarma yedekleme sanal makinesini ayrı bir kaynak grubuna ve konuma dağıtmak isteyebilirsiniz. Kaynak Yöneticisi, birden fazla abonelik ve kaynak grubunu hedeflemek için iç içe doğru şablonlar kullanmanıza olanak tanır.

> [!NOTE]
> Tek bir dağıtımda yalnızca beş kaynak grubuna dağıtabilirsiniz. Genellikle, bu sınırlama, üst şablon için belirtilen bir kaynak grubuna ve iç içe veya bağlantılı dağıtımlarda en fazla dört kaynak grubuna dağıtabileceğiniz anlamına gelir. Ancak, üst şablonunuz yalnızca iç içe veya bağlantılı şablonlar içeriyorsa ve kendisi herhangi bir kaynak dağıtmıyorsa, iç içe veya bağlantılı dağıtımlara en fazla beş kaynak grubu ekleyebilirsiniz.

## <a name="specify-subscription-and-resource-group"></a>Abonelik ve kaynak grubunu belirtin

Farklı bir kaynak grubunu veya aboneliği hedeflemek için [iç içe veya bağlantılı](linked-templates.md)bir şablon kullanın. Kaynak `Microsoft.Resources/deployments` türü, `subscriptionId` iç `resourceGroup`içe geçme için abonelik ve kaynak grubunu belirtmenizi sağlayan parametreler sağlar. Abonelik kimliği veya kaynak grubunu belirtmezseniz, üst şablondaki abonelik ve kaynak grubu kullanılır. Dağıtımı çalıştırmadan önce tüm kaynak gruplarının bulunması gerekir.

Şablonu dağıtmak için kullandığınız hesabın belirtilen abonelik kimliğine dağıtmak için izinleri olmalıdır. Belirtilen abonelik farklı bir Azure Etkin Dizin kiracısında varsa, [başka bir dizinden konuk kullanıcılar eklemeniz](../../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)gerekir.

Farklı bir kaynak grubu ve abonelik belirtmek için şunları kullanın:

```json
"resources": [
  {
    "apiVersion": "2017-05-10",
    "name": "nestedTemplate",
    "type": "Microsoft.Resources/deployments",
    "resourceGroup": "[parameters('secondResourceGroup')]",
    "subscriptionId": "[parameters('secondSubscriptionID')]",
    ...
  }
]
```

Kaynak gruplarınız aynı abonelikteyse, **subscriptionId** değerini kaldırabilirsiniz.

Aşağıdaki örnekte iki depolama hesabı dağıtılır. İlk depolama hesabı dağıtım sırasında belirtilen kaynak grubuna dağıtılır. İkinci depolama `secondResourceGroup` hesabı, `secondSubscriptionID` parametrelerde belirtilen kaynak grubuna dağıtılır:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storagePrefix": {
      "type": "string",
      "maxLength": 11
    },
    "secondResourceGroup": {
      "type": "string"
    },
    "secondSubscriptionID": {
      "type": "string",
      "defaultValue": ""
    },
    "secondStorageLocation": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]"
    }
  },
  "variables": {
    "firstStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
    "secondStorageName": "[concat(parameters('storagePrefix'), uniqueString(parameters('secondSubscriptionID'), parameters('secondResourceGroup')))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2017-06-01",
      "name": "[variables('firstStorageName')]",
      "location": "[resourceGroup().location]",
      "sku":{
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "nestedTemplate",
      "resourceGroup": "[parameters('secondResourceGroup')]",
      "subscriptionId": "[parameters('secondSubscriptionID')]",
      "properties": {
      "mode": "Incremental",
      "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2017-06-01",
            "name": "[variables('secondStorageName')]",
            "location": "[parameters('secondStorageLocation')]",
            "sku":{
              "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
          }
          ]
      },
      "parameters": {}
      }
    }
  ]
}
```

Var olmayan `resourceGroup` bir kaynak grubunun adını ayarlarsanız, dağıtım başarısız olur.

Önceki şablonu sınamak ve sonuçları görmek için PowerShell veya Azure CLI'yi kullanın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

**Aynı abonelikteki**iki kaynak grubuna iki depolama hesabı dağıtmak için şunları kullanın:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzResourceGroup -Name $firstRG -Location southcentralus
New-AzResourceGroup -Name $secondRG -Location eastus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

**İki aboneye**iki depolama hesabı dağıtmak için şunları kullanın:

```azurepowershell-interactive
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzSubscription -Subscription $secondSub
New-AzResourceGroup -Name $secondRG -Location eastus

Select-AzSubscription -Subscription $firstSub
New-AzResourceGroup -Name $firstRG -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

**Aynı abonelikteki**iki kaynak grubuna iki depolama hesabı dağıtmak için şunları kullanın:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az deployment group create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

**İki aboneye**iki depolama hesabı dağıtmak için şunları kullanın:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az deployment group create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

---

## <a name="use-functions"></a>İşlev kullanma

[Kaynak Grubu()](template-functions-resource.md#resourcegroup) ve [abonelik()](template-functions-resource.md#subscription) işlevleri şablonu nasıl belirttiğinize bağlı olarak farklı şekilde çözülür. Harici bir şablona bağlantı ettiğinizde, işlevler her zaman bu şablonun kapsamına gider. Bir şablonu bir üst şablona `expressionEvaluationOptions` yuvaya yuvayaptığınızda, işlevlerin kaynak grubuna ve ana şablona veya iç içe yönelik şablona göre çalışıp çalışmadığını belirtmek için özelliği kullanın. İç içe `inner` geçen şablonun kapsamına çözümünü ayarlamak için özelliği ayarlayın. Üst şablonun `outer` kapsamına çözümünü sağlamak için özelliği ayarlayın.

Aşağıdaki tablo, işlevlerin üst veya katıştırılmış kaynak grubu ve abonelik için çözülüp çözülmediğini gösterir.

| Şablon türü | Kapsam | Çözüm |
| ------------- | ----- | ---------- |
| Iç içe        | dış (varsayılan) | Üst kaynak grubu |
| Iç içe        | Iç | Alt kaynak grubu |
| Bağlantılı        | Yok   | Alt kaynak grubu |

Aşağıdaki [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) gösterir:

* varsayılan (dış) kapsamı olan iç içe şablon
* iç kapsamı ile iç içe şablon
* bağlantılı şablon

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "defaultScopeTemplate",
      "resourceGroup": "inlineGroup",
      "properties": {
      "mode": "Incremental",
      "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
          ],
          "outputs": {
          "resourceGroupOutput": {
            "type": "string",
            "value": "[resourceGroup().name]"
          }
          }
      },
      "parameters": {}
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "innerScopeTemplate",
      "resourceGroup": "inlineGroup",
      "properties": {
      "expressionEvaluationOptions": {
          "scope": "inner"
      },
      "mode": "Incremental",
      "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
          ],
          "outputs": {
          "resourceGroupOutput": {
            "type": "string",
            "value": "[resourceGroup().name]"
          }
          }
      },
      "parameters": {}
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "resourceGroup": "linkedGroup",
      "properties": {
      "mode": "Incremental",
      "templateLink": {
          "contentVersion": "1.0.0.0",
          "uri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/resourceGroupName.json"
      },
      "parameters": {}
      }
    }
  ],
  "outputs": {
    "parentRG": {
      "type": "string",
      "value": "[concat('Parent resource group is ', resourceGroup().name)]"
    },
    "defaultScopeRG": {
      "type": "string",
      "value": "[concat('Default scope resource group is ', reference('defaultScopeTemplate').outputs.resourceGroupOutput.value)]"
    },
    "innerScopeRG": {
      "type": "string",
      "value": "[concat('Inner scope resource group is ', reference('innerScopeTemplate').outputs.resourceGroupOutput.value)]"
    },
    "linkedRG": {
      "type": "string",
      "value": "[concat('Linked resource group is ', reference('linkedTemplate').outputs.resourceGroupOutput.value)]"
    }
  }
}
```

Önceki şablonu sınamak ve sonuçları görmek için PowerShell veya Azure CLI'yi kullanın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name parentGroup -Location southcentralus
New-AzResourceGroup -Name inlineGroup -Location southcentralus
New-AzResourceGroup -Name linkedGroup -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Önceki örnekten çıktı:

```output
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         String                     Parent resource group is parentGroup
 defaultScopeRG   String                     Default scope resource group is parentGroup
 innerScopeRG     String                     Inner scope resource group is inlineGroup
 linkedRG         String                     Linked resource group is linkedgroup
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az deployment group create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Önceki örnekten çıktı:

```output
"outputs": {
  "defaultScopeRG": {
    "type": "String",
    "value": "Default scope resource group is parentGroup"
  },
  "innerScopeRG": {
    "type": "String",
    "value": "Inner scope resource group is inlineGroup"
  },
  "linkedRG": {
    "type": "String",
    "value": "Linked resource group is linkedGroup"
  },
  "parentRG": {
    "type": "String",
    "value": "Parent resource group is parentGroup"
  }
},
```

---

## <a name="next-steps"></a>Sonraki adımlar

* Şablonunuzdaparametrelerin nasıl tanımlandığını anlamak için [bkz.](template-syntax.md)
* Sık kullanılan dağıtım hatalarını çözme yle ilgili ipuçları için, [Azure Kaynak Yöneticisi ile sık karşılaşılan Azure dağıtım hatalarını giderme](common-deployment-errors.md)sorununa bakın.
* SAS belirteci gerektiren bir şablonu dağıtma hakkında bilgi [için](secure-template-with-sas-token.md)bkz.
