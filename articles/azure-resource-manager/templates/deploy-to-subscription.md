---
title: Kaynakları aboneliğe dağıtma
description: Azure Resource Manager şablonunda bir kaynak grubu oluşturmayı açıklar. Ayrıca Azure abonelik kapsamındaki kaynakların nasıl dağıtılacağını gösterir.
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: 80fe451f696480ec24b3d8eced64941de9492fef
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610828"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma

Azure aboneliğinizdeki kaynakların yönetimini basitleştirmek için, abonelik genelinde [ilkeleri](../../governance/policy/overview.md) veya [rol tabanlı erişim denetimlerini](../../role-based-access-control/overview.md) tanımlayabilir ve atayabilirsiniz. Abonelik düzeyi şablonları ile, ilkeleri bir şekilde uygulayın ve aboneliğe roller atayın. Ayrıca, kaynak grupları oluşturabilir ve kaynakları dağıtabilirsiniz.

Şablonları abonelik düzeyinde dağıtmak için Azure CLı, PowerShell veya REST API kullanın. Azure portal, abonelik düzeyinde dağıtımı desteklemez.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Aşağıdaki kaynak türlerini abonelik düzeyinde dağıtabilirsiniz:

* [Blueprint](/azure/templates/microsoft.blueprint/blueprints)
* [bütçelerinin](/azure/templates/microsoft.consumption/budgets)
* [dağıtımlar](/azure/templates/microsoft.resources/deployments) -kaynak gruplarına dağıtan iç içe şablonlar için.
* [Eventabonelikleri](/azure/templates/microsoft.eventgrid/eventsubscriptions)
* [peerAsns](/azure/templates/microsoft.peering/2019-09-01-preview/peerasns)
* [Poliyasatamaları](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [düzeltmeler](/azure/templates/microsoft.policyinsights/2019-07-01/remediations)
* [resourceGroups](/azure/templates/microsoft.resources/resourcegroups)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)
* [Scopeasyleri](/azure/templates/microsoft.managednetwork/scopeassignments)
* [supportPlanTypes](/azure/templates/microsoft.addons/supportproviders/supportplantypes)
* [lerimi](/azure/templates/microsoft.resources/tags)

### <a name="schema"></a>Şema

Abonelik düzeyi dağıtımlar için kullandığınız şema, kaynak grubu dağıtımları için şemadan farklıdır.

Şablonlar için şunu kullanın:

```json
https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#
```

Bir parametre dosyasının şeması, tüm dağıtım kapsamları için aynıdır. Parametre dosyaları için şunu kullanın:

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="deployment-commands"></a>Dağıtım komutları

Abonelik düzeyi dağıtımlara yönelik komutlar, kaynak grubu dağıtımları için komutlardan farklıdır.

Azure CLı için [az Deployment Sub Create](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-create)kullanın. Aşağıdaki örnek, bir kaynak grubu oluşturmak için bir şablon dağıtır:

```azurecli-interactive
az deployment sub create \
  --name demoSubDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

PowerShell dağıtım komutu için [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) veya **New-azsubscriptiondeployment**kullanın. Aşağıdaki örnek, bir kaynak grubu oluşturmak için bir şablon dağıtır:

```azurepowershell-interactive
New-AzSubscriptionDeployment `
  -Name demoSubDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

REST API için [dağıtımlar-abonelik kapsamında oluştur](/rest/api/resources/deployments/createorupdateatsubscriptionscope)' u kullanın.

## <a name="deployment-location-and-name"></a>Dağıtım konumu ve adı

Abonelik düzeyindeki dağıtımlar için, dağıtım için bir konum sağlamanız gerekir. Dağıtımın konumu, dağıttığınız kaynakların konumundan ayrıdır. Dağıtım konumu, dağıtım verilerinin depolanacağı konumu belirtir.

Dağıtım için bir ad verebilir veya varsayılan dağıtım adını kullanabilirsiniz. Varsayılan ad şablon dosyasının adıdır. Örneğin, **azuredeploy. JSON** adlı bir şablon dağıtmak, **azuredeploy**varsayılan dağıtım adını oluşturur.

Her dağıtım adı için konum sabittir. Farklı bir konumda aynı ada sahip mevcut bir dağıtım olduğunda tek bir konumda dağıtım oluşturamazsınız. Hata kodunu `InvalidDeploymentLocation`alırsanız, bu ad için önceki dağıtımla farklı bir ad veya aynı konumu kullanın.

## <a name="use-template-functions"></a>Şablon işlevlerini kullanma

Abonelik düzeyindeki dağıtımlar için, Şablon işlevleri kullanılırken bazı önemli noktalar vardır:

* [ResourceGroup ()](template-functions-resource.md#resourcegroup) **işlevi desteklenmiyor.**
* [Reference ()](template-functions-resource.md#reference) ve [List ()](template-functions-resource.md#list) işlevleri desteklenir.
* Abonelik düzeyinde dağıtılan kaynakların kaynak KIMLIĞINI almak için [Subscriptionresourceıd ()](template-functions-resource.md#subscriptionresourceid) işlevini kullanın.

  Örneğin, bir ilke tanımının kaynak KIMLIĞINI almak için şunu kullanın:
  
  ```json
  subscriptionResourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))
  ```
  
  Döndürülen kaynak KIMLIĞI şu biçimdedir:

  ```json
  /subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-resource-groups"></a>Kaynak grupları oluşturma

Azure Resource Manager şablonunda bir kaynak grubu oluşturmak için kaynak grubu için bir ad ve konum içeren bir [Microsoft. resources/resourceGroups](/azure/templates/microsoft.resources/allversions) kaynağı tanımlayın. Kaynak grubu oluşturabilir ve aynı şablondaki kaynak grubuna kaynak dağıtabilirsiniz.

Aşağıdaki şablon boş bir kaynak grubu oluşturur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
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
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    }
  ],
  "outputs": {}
}
```

Birden fazla kaynak grubu oluşturmak için kaynak gruplarıyla [Kopyala öğesini](copy-resources.md) kullanın.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
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

Kaynak yinelemesi hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonlarda bir kaynağın birden fazla örneğini dağıtma](./copy-resources.md)ve [öğretici: Kaynak Yöneticisi şablonlarla birden fazla kaynak örneği oluşturma](./template-tutorial-create-multiple-instances.md).

## <a name="resource-group-and-resources"></a>Kaynak grubu ve kaynaklar

Kaynak grubu oluşturmak ve kaynaklarına kaynak dağıtmak için, iç içe geçmiş bir şablon kullanın. İç içe şablon, kaynak grubuna dağıtılacak kaynakları tanımlar. Kaynak grubunun kaynakları dağıtımdan önce mevcut olduğundan emin olmak için, iç içe geçmiş şablonu kaynak grubuna bağımlı olarak ayarlayın.

Aşağıdaki örnek, bir kaynak grubu oluşturur ve kaynak grubuna bir depolama hesabı dağıtır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
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
              "sku": {
                "name": "Standard_LRS"
              },
              "kind": "StorageV2"
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

## <a name="azure-policy"></a>Azure İlkesi

### <a name="assign-policy-definition"></a>İlke tanımı ata

Aşağıdaki örnek, aboneliğe var olan bir ilke tanımını atar. İlke tanımı parametreleri alırsa, bunları bir nesne olarak sağlayın. İlke tanımı parametre almadıysanız varsayılan boş nesneyi kullanın.

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
      "apiVersion": "2018-03-01",
      "name": "[parameters('policyName')]",
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
# Built-in policy definition that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Bu şablonu PowerShell ile dağıtmak için şunu kullanın:

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzSubscriptionDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="create-and-assign-policy-definitions"></a>İlke tanımları oluşturma ve atama

Aynı şablonda bir ilke tanımı [tanımlayabilir](../../governance/policy/concepts/definition-structure.md) ve atayabilirsiniz.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyDefinitions",
      "apiVersion": "2018-05-01",
      "name": "locationpolicy",
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
      "apiVersion": "2018-05-01",
      "name": "location-lock",
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

Aboneliğinizde ilke tanımı oluşturmak ve aboneliği aboneliğe atamak için aşağıdaki CLı komutunu kullanın:

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

Bu şablonu PowerShell ile dağıtmak için şunu kullanın:

```azurepowershell
New-AzSubscriptionDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

## <a name="azure-blueprints"></a>Azure Blueprints

### <a name="create-blueprint-definition"></a>Şema tanımı oluştur

Şablondan bir şema tanımı [oluşturabilirsiniz](../../governance/blueprints/tutorials/create-from-sample.md) .

:::code language="json" source="~/quickstart-templates/subscription-level-deployments/blueprints-new-blueprint/azuredeploy.json":::

Aboneliğinizde şema tanımını oluşturmak için aşağıdaki CLI komutunu kullanın:

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-level-deployments/blueprints-new-blueprint/azuredeploy.json"
```

Bu şablonu PowerShell ile dağıtmak için şunu kullanın:

```azurepowershell
New-AzSubscriptionDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-level-deployments/blueprints-new-blueprint/azuredeploy.json"
```

## <a name="template-samples"></a>Şablon örnekleri

* [Bir kaynak grubu oluşturun, kilitleyin ve buna izin verin](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment).
* [Kaynak grubu, ilke ve ilke ataması oluşturun](https://github.com/Azure/azure-docs-json-samples/blob/master/subscription-level-deployment/azuredeploy.json).

## <a name="next-steps"></a>Sonraki adımlar

* Rol atama hakkında bilgi edinmek için bkz. [RBAC ve Azure Resource Manager şablonlarını kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-template.md).
* Azure Güvenlik Merkezi için çalışma alanı ayarlarını dağıtmaya ilişkin bir örnek için bkz. [Deployascwithworkspace Settings. JSON](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Örnek Şablonlar [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments)' da bulunabilir.
* Ayrıca, şablonları [Yönetim grubu düzeyinde](deploy-to-management-group.md) ve [kiracı düzeyinde](deploy-to-tenant.md)dağıtabilirsiniz.
