---
title: Kaynakları dağıtım & çapraz abonelik kaynak grubu
description: Dağıtım sırasında birden fazla Azure aboneliğini ve kaynak grubunu nasıl hedefleyecek olduğunu gösterir.
ms.topic: conceptual
ms.date: 12/09/2019
ms.openlocfilehash: 0754895215384f76b1cb44224f3ba06c80181827
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74978773"
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Azure kaynaklarını birden fazla aboneliğe veya kaynak grubuna dağıtın

Genellikle, şablonunuzda bulunan tüm kaynakları tek bir [kaynak grubuna](resource-group-overview.md)dağıtırsınız. Ancak, bir kaynak kümesini birlikte dağıtmak ve bunları farklı kaynak gruplarına veya aboneliklerine yerleştirmek istediğiniz senaryolar vardır. Örneğin, Azure Site Recovery için yedekleme sanal makinesini ayrı bir kaynak grubuna ve konuma dağıtmak isteyebilirsiniz. Kaynak Yöneticisi, birden fazla aboneliği ve kaynak grubunu hedeflemek için iç içe geçmiş şablonlar kullanmanıza olanak sağlar.

> [!NOTE]
> Tek bir dağıtımda yalnızca beş kaynak grubuna dağıtım yapabilirsiniz. Genellikle, bu sınırlama, ana şablon için belirtilen bir kaynak grubuna ve iç içe veya bağlı dağıtımlardaki en fazla dört kaynak grubuna dağıtabileceğiniz anlamına gelir. Ancak, ana şablonunuz yalnızca iç içe veya bağlı şablonlar içeriyorsa ve herhangi bir kaynak dağıtmadığından, iç içe veya bağlı dağıtımlara en fazla beş kaynak grubu ekleyebilirsiniz.

## <a name="specify-subscription-and-resource-group"></a>Abonelik ve kaynak grubu belirtin

Farklı bir kaynak grubu veya aboneliği hedeflemek için, [iç içe veya bağlı bir şablon](resource-group-linked-templates.md)kullanın. `Microsoft.Resources/deployments` kaynak türü, iç içe dağıtım için aboneliği ve kaynak grubunu belirtmenizi sağlayan `subscriptionId` ve `resourceGroup`için parametreler sağlar. Abonelik KIMLIĞINI veya kaynak grubunu belirtmezseniz, üst şablondaki abonelik ve kaynak grubu kullanılır. Dağıtımı çalıştırmadan önce tüm kaynak gruplarının mevcut olması gerekir.

Şablonu dağıtmak için kullandığınız hesabın, belirtilen abonelik KIMLIĞINE dağıtım izinleri olmalıdır. Belirtilen abonelik farklı bir Azure Active Directory kiracısında varsa, [başka bir dizinden Konuk kullanıcılar eklemeniz](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)gerekir.

Farklı bir kaynak grubu ve abonelik belirtmek için şunu kullanın:

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

Kaynak gruplarınız aynı abonelikte yer alıyorsa, **SubscriptionID** değerini kaldırabilirsiniz.

Aşağıdaki örnek iki depolama hesabı dağıtır. İlk depolama hesabı, dağıtım sırasında belirtilen kaynak grubuna dağıtılır. İkinci depolama hesabı, `secondResourceGroup` ve `secondSubscriptionID` parametrelerinde belirtilen kaynak grubuna dağıtılır:

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
            "name": "[variables('firstStorageName')]",
            "apiVersion": "2017-06-01",
            "location": "[resourceGroup().location]",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        },
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
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
                            "name": "[variables('secondStorageName')]",
                            "apiVersion": "2017-06-01",
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

`resourceGroup`, mevcut olmayan bir kaynak grubunun adına ayarlarsanız, dağıtım başarısız olur.

Önceki şablonu test etmek ve sonuçları görmek için PowerShell veya Azure CLı kullanın.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

İki depolama hesabını **aynı abonelikte**iki kaynak grubuna dağıtmak için şunu kullanın:

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

İki **aboneliğe**iki depolama hesabı dağıtmak için şunu kullanın:

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

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

İki depolama hesabını **aynı abonelikte**iki kaynak grubuna dağıtmak için şunu kullanın:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

İki **aboneliğe**iki depolama hesabı dağıtmak için şunu kullanın:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

---

## <a name="use-functions"></a>İşlev kullanma

[ResourceGroup ()](resource-group-template-functions-resource.md#resourcegroup) ve [Subscription ()](resource-group-template-functions-resource.md#subscription) işlevleri, şablonu nasıl belirtkullanılacağına göre farklı şekilde çözümlenir. Bir dış şablona bağlandığınızda işlevler her zaman ilgili şablonun kapsamına çözümlenir. Bir şablonu bir üst şablon içine yuvalandığınızda, işlevlerin ana şablon veya iç içe şablon için kaynak grubuna ve aboneliğe çözümlenip çözümlenmediğini belirtmek için `expressionEvaluationOptions` özelliğini kullanın. İç içe şablon için kapsama çözülecek özelliği `inner` olarak ayarlayın. Üst şablonun kapsamına çözülecek özelliği `outer` olarak ayarlayın.

Aşağıdaki tabloda, işlevlerin üst veya katıştırılmış kaynak grubuna ve aboneliğe çözümlenip çözümlenmediğini gösterir.

| Şablon türü | Kapsam | Çözünürlük |
| ------------- | ----- | ---------- |
| iç içe geçmiş        | dış (varsayılan) | Üst kaynak grubu |
| iç içe geçmiş        | iç | Alt kaynak grubu |
| bağlandı        | Yok   | Alt kaynak grubu |

Aşağıdaki [örnek şablonda]((https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json)) şunları gösterir:

* Varsayılan (dış) kapsamlı iç içe şablon
* iç kapsama sahip iç içe şablon
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
            "name": "defaultScopeTemplate",
            "apiVersion": "2017-05-10",
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
            "name": "innerScopeTemplate",
            "apiVersion": "2017-05-10",
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
            "apiVersion": "2017-05-10",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
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

Önceki şablonu test etmek ve sonuçları görmek için PowerShell veya Azure CLı kullanın.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name parentGroup -Location southcentralus
New-AzResourceGroup -Name inlineGroup -Location southcentralus
New-AzResourceGroup -Name linkedGroup -Location southcentralus

New-AzResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Yukarıdaki örnekteki çıktı:

```powershell
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         String                     Parent resource group is parentGroup
 defaultScopeRG   String                     Default scope resource group is parentGroup
 innerScopeRG     String                     Inner scope resource group is inlineGroup
 linkedRG         String                     Linked resource group is linkedgroup
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

Yukarıdaki örnekteki çıktı:

```azurecli
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

* Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](resource-group-authoring-templates.md).
* Yaygın dağıtım hatalarını çözümlemeye yönelik ipuçları için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
* SAS belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](resource-manager-powershell-sas-token.md).
