---
title: Abonelik Azure Resource Manager şablonunda kaynak grubu ve kaynakları oluşturma
description: Azure Resource Manager şablonunda bir kaynak grubu oluşturmayı açıklar. Ayrıca Azure abonelik kapsamındaki kaynakların nasıl dağıtılacağını gösterir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: tomfitz
ms.openlocfilehash: 37f2b04a62d94cce42b095540380460c38bc5b79
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772948"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma

Genellikle Azure kaynaklarını Azure aboneliğinizdeki bir kaynak grubuna dağıtırsınız. Bununla birlikte, Azure Kaynak grupları da oluşturabilir ve abonelik düzeyinde Azure kaynakları oluşturabilirsiniz. Şablonları abonelik düzeyinde dağıtmak için Azure CLı ve Azure PowerShell kullanırsınız. Azure portal, abonelik düzeyinde dağıtımı desteklemez.

Azure Resource Manager şablonunda bir kaynak grubu oluşturmak için kaynak grubu için bir ad ve konum içeren bir [Microsoft. resources/resourceGroups](/azure/templates/microsoft.resources/allversions) kaynağı tanımlayın. Kaynak grubu oluşturabilir ve aynı şablondaki kaynak grubuna kaynak dağıtabilirsiniz. Abonelik düzeyinde dağıtabileceğiniz kaynaklar şunlardır: [İlkeler](../governance/policy/overview.md)ve [rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

## <a name="deployment-considerations"></a>Dağıtma konuları

Abonelik düzeyi dağıtım, kaynak grubu dağıtımından aşağıdaki açılardan farklıdır:

### <a name="schema-and-commands"></a>Şema ve komutlar

Abonelik düzeyi dağıtımlar için kullandığınız şema ve komutlar, kaynak grubu dağıtımlarından farklıdır. 

Şema için öğesini kullanın `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`.

Azure CLı dağıtım komutu için [az Deployment Create](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create)komutunu kullanın. Örneğin, aşağıdaki CLı komutu bir kaynak grubu oluşturmak için bir şablon dağıtır:

```azurecli-interactive
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

PowerShell dağıtım komutu için [New-AzDeployment](/powershell/module/az.resources/new-azdeployment)kullanın. Örneğin, aşağıdaki PowerShell komutu bir kaynak grubu oluşturmak için bir şablon dağıtır:

```azurepowershell-interactive
New-AzDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

### <a name="deployment-name-and-location"></a>Dağıtım adı ve konumu

Aboneliğinize dağıtım yaparken, dağıtım için bir konum sağlamanız gerekir. Dağıtım için bir ad da sağlayabilirsiniz. Dağıtım için bir ad belirtmezseniz, şablonun adı dağıtım adı olarak kullanılır. Örneğin, **azuredeploy. JSON** adlı bir şablon dağıtmak, **azuredeploy**varsayılan dağıtım adını oluşturur.

Abonelik düzeyi dağıtımların konumu sabittir. Aynı ada ancak farklı konuma sahip mevcut bir dağıtım olduğunda tek bir konumda dağıtım oluşturamazsınız. Hata kodunu `InvalidDeploymentLocation`alırsanız, bu ad için önceki dağıtımla farklı bir ad veya aynı konumu kullanın.

### <a name="use-template-functions"></a>Şablon işlevlerini kullanma

Abonelik düzeyindeki dağıtımlar için, Şablon işlevleri kullanılırken bazı önemli noktalar vardır:

* [ResourceGroup ()](resource-group-template-functions-resource.md#resourcegroup) **işlevi desteklenmiyor.**
* [RESOURCEID ()](resource-group-template-functions-resource.md#resourceid) işlevi desteklenir. Abonelik düzeyi dağıtımlarında kullanılan kaynakların kaynak KIMLIĞINI almak için kullanın. Örneğin, bir ilke tanımının kaynak KIMLIĞINI şu şekilde alın`resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`
* [Reference ()](resource-group-template-functions-resource.md#reference) ve [List ()](resource-group-template-functions-resource.md#list) işlevleri desteklenir.

## <a name="create-resource-groups"></a>Kaynak grupları oluşturma

Aşağıdaki şablon boş bir kaynak grubu oluşturur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Şablon Şeması [burada](/azure/templates/microsoft.resources/allversions)bulunabilir. Benzer şablonlar [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments)' da bulunabilir.

## <a name="create-multiple-resource-groups"></a>Birden çok kaynak grubu oluşturma

Birden fazla kaynak grubu oluşturmak için kaynak gruplarıyla [Kopyala öğesini](resource-group-create-multiple.md) kullanın. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgNamePrefix": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "instanceCount": {
            "type": "int"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
            "copy": {
                "name": "rgCopy",
                "count": "[parameters('instanceCount')]"
            },
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Kaynak yinelemesi hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonlarındaki bir kaynak veya özelliğin birden fazla örneğini dağıtma](./resource-group-create-multiple.md)ve [öğretici: Kaynak Yöneticisi şablonlarıyla](./resource-manager-tutorial-create-multiple-instances.md)birden çok kaynak örneği oluşturun.

## <a name="create-resource-group-and-deploy-resources"></a>Kaynak grubu oluşturma ve kaynakları dağıtma

Kaynak grubu oluşturmak ve kaynaklarına kaynak dağıtmak için, iç içe geçmiş bir şablon kullanın. İç içe şablon, kaynak grubuna dağıtılacak kaynakları tanımlar. Kaynak grubunun kaynakları dağıtımdan önce mevcut olduğundan emin olmak için, iç içe geçmiş şablonu kaynak grubuna bağımlı olarak ayarlayın.

Aşağıdaki örnek, bir kaynak grubu oluşturur ve kaynak grubuna bir depolama hesabı dağıtır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        }
    },
    "variables": {
        "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "storageDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
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
                            "apiVersion": "2017-10-01",
                            "name": "[variables('storageName')]",
                            "location": "[parameters('rgLocation')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Standard_LRS"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

## <a name="create-policies"></a>İlkeleri oluşturma

### <a name="assign-policy"></a>İlke ata

Aşağıdaki örnek, aboneliğe var olan bir ilke tanımını atar. İlke parametreleri alırsa, bunları bir nesne olarak sağlayın. İlke parametre almadıysanız varsayılan boş nesneyi kullanın.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

Bu şablonu Azure CLı ile dağıtmak için şunu kullanın:

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Bu şablonu PowerShell ile dağıtmak için şunu kullanın:

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="define-and-assign-policy"></a>İlke tanımlama ve atama

Aynı şablonda bir ilke [tanımlayabilir](../governance/policy/concepts/definition-structure.md) ve atayabilirsiniz.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2018-05-01",
            "dependsOn": [
                "locationpolicy"
            ],
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
            }
        }
    ]
}
```

Aboneliğinizde ilke tanımı oluşturmak ve bunu aboneliğe uygulamak için aşağıdaki CLı komutunu kullanın:

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Bu şablonu PowerShell ile dağıtmak için şunu kullanın:

```azurepowershell
New-AzDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="next-steps"></a>Sonraki adımlar

* Rol atama hakkında bilgi edinmek için bkz. [RBAC ve Azure Resource Manager şablonlarını kullanarak Azure kaynaklarına erişimi yönetme](../role-based-access-control/role-assignments-template.md).
* Azure Güvenlik Merkezi için çalışma alanı ayarlarını dağıtmaya ilişkin bir örnek için bkz. [Deployascwithworkspace Settings. JSON](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Azure Resource Manager şablonları oluşturma hakkında bilgi edinmek için bkz. [yazma şablonları](resource-group-authoring-templates.md). 
* Bir şablondaki kullanılabilir işlevlerin listesi için bkz. [Şablon işlevleri](resource-group-template-functions.md).
