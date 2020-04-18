---
title: Kaynakları aboneye dağıtma
description: Azure Kaynak Yöneticisi şablonunda kaynak grubunun nasıl oluşturulurulur açıklanır. Ayrıca, Azure abonelik kapsamında kaynakların nasıl dağıtılancaya yapılacağını da gösterir.
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: 6bec29a07653ff5ad7d1e2f8317246049e127c8c
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604999"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma

Azure aboneliğinizdeki kaynakların yönetimini kolaylaştırmak için, abonelik genelinde ilkeler veya [rol tabanlı erişim denetimleri](../../role-based-access-control/overview.md) tanımlayabilir ve atayabilirsiniz. [policies](../../governance/policy/overview.md) Abonelik düzeyi şablonları ile, bildirimsel olarak ilkeleri uygular ve abonelikte roller atarsınız. Ayrıca kaynak grupları oluşturabilir ve kaynakları dağıtabilirsiniz.

Şablonları abonelik düzeyinde dağıtmak için Azure CLI, PowerShell veya REST API'yi kullanın. Azure portalı abonelik düzeyinde dağıtımı desteklemez.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Aşağıdaki kaynak türlerini abonelik düzeyinde dağıtabilirsiniz:

* [Bütçe](/azure/templates/microsoft.consumption/budgets)
* [dağıtımlar](/azure/templates/microsoft.resources/deployments) - kaynak gruplarına dağıtılan iç içe yönelik şablonlar için.
* [eventAbonelikler](/azure/templates/microsoft.eventgrid/eventsubscriptions)
* [peerAsns](/azure/templates/microsoft.peering/2019-09-01-preview/peerasns)
* [ilkeAtamalar](/azure/templates/microsoft.authorization/policyassignments)
* [policyTanımlar](/azure/templates/microsoft.authorization/policydefinitions)
* [politikaSetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [düzeltmeler](/azure/templates/microsoft.policyinsights/2019-07-01/remediations)
* [resourceGroups](/azure/templates/microsoft.resources/resourcegroups)
* [roleAtamaları](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)
* [kapsamAtamaları](/azure/templates/microsoft.managednetwork/scopeassignments)
* [destekPlanTypes](/azure/templates/microsoft.addons/supportproviders/supportplantypes)
* [Etiketler](/azure/templates/microsoft.resources/tags)

### <a name="schema"></a>Şema

Abonelik düzeyi dağıtımları için kullandığınız şema, kaynak grubu dağıtımları için şemadan farklıdır.

Şablonlar için şunları kullanın:

```json
https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#
```

Parametre dosyasının şeması tüm dağıtım kapsamları için aynıdır. Parametre dosyaları için şunları kullanın:

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="deployment-commands"></a>Dağıtım komutları

Abonelik düzeyi dağıtımları için komutlar kaynak grubu dağıtımları için komutlardan farklıdır.

Azure CLI için [az dağıtım alt oluşturma'yı](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-create)kullanın. Aşağıdaki örnek, kaynak grubu oluşturmak için bir şablon dağıtır:

```azurecli-interactive
az deployment sub create \
  --name demoSubDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

PowerShell dağıtım komutu için [Yeni-AzDeployment](/powershell/module/az.resources/new-azdeployment) veya **New-AzSubscriptionDeployment'ı**kullanın. Aşağıdaki örnek, kaynak grubu oluşturmak için bir şablon dağıtır:

```azurepowershell-interactive
New-AzSubscriptionDeployment `
  -Name demoSubDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

REST API için Dağıtımları kullanın [- Abonelik Kapsamında Oluştur.](/rest/api/resources/deployments/createorupdateatsubscriptionscope)

## <a name="deployment-location-and-name"></a>Dağıtım konumu ve adı

Abonelik düzeyi dağıtımları için dağıtım için bir konum sağlamanız gerekir. Dağıtımın konumu, dağıttığınız kaynakların konumundan ayrıdır. Dağıtım konumu dağıtım verilerinin nerede depolanır olduğunu belirtir.

Dağıtım için bir ad sağlayabilir veya varsayılan dağıtım adını kullanabilirsiniz. Varsayılan ad şablon dosyasının adıdır. Örneğin, **azuredeploy.json** adlı bir şablonu dağıtmak **azuredeploy**varsayılan dağıtım adı oluşturur.

Her dağıtım adı için konum değişmez. Farklı bir konumda aynı ada sahip varolan bir dağıtım olduğunda, tek bir konumda dağıtım oluşturamazsınız. Hata kodunu `InvalidDeploymentLocation`alırsanız, bu ad için farklı bir ad veya önceki dağıtımla aynı konumu kullanın.

## <a name="use-template-functions"></a>Şablon işlevlerini kullanma

Abonelik düzeyi dağıtımları için şablon işlevlerini kullanırken bazı önemli noktalar vardır:

* [ResourceGroup()](template-functions-resource.md#resourcegroup) işlevi **desteklenmez.**
* [Başvuru()](template-functions-resource.md#reference) ve [list()](template-functions-resource.md#list) işlevleri desteklenir.
* Abonelik düzeyinde dağıtılan kaynaklar için kaynak kimliğini almak için [subscriptionResourceId()](template-functions-resource.md#subscriptionresourceid) işlevini kullanın.

  Örneğin, bir ilke tanımı için kaynak kimliğini almak için şunları kullanın:
  
  ```json
  subscriptionResourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))
  ```
  
  Döndürülen kaynak kimliği aşağıdaki biçime sahiptir:

  ```json
  /subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-resource-groups"></a>Kaynak grupları oluşturma

Azure Kaynak Yöneticisi şablonunda bir kaynak grubu oluşturmak için, kaynak grubu için ad ve konuma sahip bir [Microsoft.Resources/resourceGroups](/azure/templates/microsoft.resources/allversions) kaynağı tanımlayın. Bir kaynak grubu oluşturabilir ve kaynakları aynı şablonda bu kaynak grubuna dağıtabilirsiniz.

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

Birden fazla kaynak grubu oluşturmak için kaynak gruplarıyla [kopyalama öğesini](copy-resources.md) kullanın.

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

Kaynak yinelemesi hakkında bilgi için bkz: [Azure Kaynak Yöneticisi Şablonlarında bir kaynağın birden fazla örneğini dağıt](./copy-resources.md)ve [Öğretici: Kaynak Yöneticisi şablonları ile birden çok kaynak örneği oluşturun.](./template-tutorial-create-multiple-instances.md)

## <a name="resource-group-and-resources"></a>Kaynak grubu ve kaynaklar

Kaynak grubunu oluşturmak ve kaynakları ona dağıtmak için iç içe bir şablon kullanın. İç içe şablon, kaynak grubuna dağıtılabilmek için kaynakları tanımlar. Kaynakları dağıtmadan önce kaynak grubunun bulunduğundan emin olmak için iç içe geçen şablonu kaynak grubuna bağımlı olarak ayarlayın.

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

## <a name="create-policies"></a>İlkeleri oluşturma

### <a name="assign-policy"></a>İlke atama

Aşağıdaki örnek, aboneye varolan bir ilke tanımı atar. İlke parametreleri alıyorsa, bunları nesne olarak sağlayın. İlke parametreleri almazsa, varsayılan boş nesneyi kullanın.

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

Bu şablonu Azure CLI ile dağıtmak için şunları kullanın:

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Bu şablonu PowerShell ile dağıtmak için şunları kullanın:

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

### <a name="define-and-assign-policy"></a>İlke tanımlama ve atama

Aynı şablonda bir ilke [tanımlayabilir](../../governance/policy/concepts/definition-structure.md) ve atayabilirsiniz.

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

Aboneliğinizde ilke tanımıoluşturmak ve aboneye uygulamak için aşağıdaki CLI komutunu kullanın:

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

Bu şablonu PowerShell ile dağıtmak için şunları kullanın:

```azurepowershell
New-AzSubscriptionDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

## <a name="template-samples"></a>Şablon örnekleri

* [Bir kaynak grubu oluşturun, kilitleyin ve izin verin.](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment)
* [Kaynak grubu, ilke ve ilke ataması oluşturun.](https://github.com/Azure/azure-docs-json-samples/blob/master/subscription-level-deployment/azuredeploy.json)

## <a name="next-steps"></a>Sonraki adımlar

* Roller atama hakkında bilgi edinmek [için RBAC ve Azure Kaynak Yöneticisi şablonlarını kullanarak Azure kaynaklarına erişimi yönet'e](../../role-based-access-control/role-assignments-template.md)bakın.
* Azure Güvenlik Merkezi için çalışma alanı ayarlarını dağıtma örneği [için](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json)bkz.
* Örnek şablonlar [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments)bulunabilir.
* Ayrıca, yönetim grubu [düzeyinde](deploy-to-management-group.md) ve [kiracı düzeyinde](deploy-to-tenant.md)şablonlar dağıtabilirsiniz.
