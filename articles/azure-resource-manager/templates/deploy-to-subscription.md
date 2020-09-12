---
title: Kaynakları aboneliğe dağıtma
description: Azure Resource Manager şablonunda bir kaynak grubu oluşturmayı açıklar. Ayrıca Azure abonelik kapsamındaki kaynakların nasıl dağıtılacağını gösterir.
ms.topic: conceptual
ms.date: 09/04/2020
ms.openlocfilehash: ef4f92d2e113e7cd393c50ba4eb8b47eb4ad9d08
ms.sourcegitcommit: 4feb198becb7a6ff9e6b42be9185e07539022f17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89468649"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma

Kaynakların yönetimini basitleştirmek için, Azure aboneliğinizin düzeyindeki kaynakları dağıtmak üzere bir Azure Resource Manager şablonu (ARM şablonu) kullanabilirsiniz. Örneğin, abonelikleriniz için [ilkeler](../../governance/policy/overview.md) ve [Azure rol tabanlı erişim denetımı (Azure RBAC)](../../role-based-access-control/overview.md) dağıtabilirsiniz. Ayrıca, abonelik içinde kaynak grupları oluşturabilir ve kaynakları abonelikte kaynak gruplarına dağıtabilirsiniz.

> [!NOTE]
> Abonelik düzeyi dağıtımında 800 farklı kaynak grubuna dağıtım yapabilirsiniz.

Şablonları abonelik düzeyinde dağıtmak için Azure CLı, PowerShell, REST API veya portal kullanın.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Tüm kaynak türleri abonelik düzeyine dağıtılamaz. Bu bölümde hangi kaynak türlerinin desteklendiği listelenmektedir.

Azure şemaları için şunu kullanın:

* [Yapıt](/azure/templates/microsoft.blueprint/blueprints/artifacts)
* [Blueprint](/azure/templates/microsoft.blueprint/blueprints)
* [Şema tasmi](/azure/templates/microsoft.blueprint/blueprintassignments)
* [sürümler (planlar)](/azure/templates/microsoft.blueprint/blueprints/versions)

Azure Ilkeleri için şunu kullanın:

* [Poliyasatamaları](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [düzeltmeler](/azure/templates/microsoft.policyinsights/remediations)

Rol tabanlı erişim denetimi için şunu kullanın:

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

Kaynak gruplarına dağıtan iç içe şablonlar için şunu kullanın:

* [dağıtımlar](/azure/templates/microsoft.resources/deployments)

Yeni kaynak grupları oluşturmak için şunu kullanın:

* [resourceGroups](/azure/templates/microsoft.resources/resourcegroups)

Aboneliğinizi yönetmek için şunu kullanın:

* [bütçelerinin](/azure/templates/microsoft.consumption/budgets)
* [supportPlanTypes](/azure/templates/microsoft.addons/supportproviders/supportplantypes)
* [lerimi](/azure/templates/microsoft.resources/tags)

Desteklenen diğer türler şunlardır:

* [Scopeasyleri](/azure/templates/microsoft.managednetwork/scopeassignments)
* [Eventabonelikleri](/azure/templates/microsoft.eventgrid/eventsubscriptions)
* [peerAsns](/azure/templates/microsoft.peering/2019-09-01-preview/peerasns)

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

Dağıtım için bir ad verebilir veya varsayılan dağıtım adını kullanabilirsiniz. Varsayılan ad şablon dosyasının adıdır. Örneğin, ** üzerindeazuredeploy.js** adlı bir şablon dağıtmak, **azuredeploy**varsayılan dağıtım adını oluşturur.

Her dağıtım adı için konum sabittir. Farklı bir konumda aynı ada sahip mevcut bir dağıtım olduğunda tek bir konumda dağıtım oluşturamazsınız. Hata kodunu alırsanız `InvalidDeploymentLocation` , bu ad için önceki dağıtımla farklı bir ad veya aynı konumu kullanın.

## <a name="deployment-scopes"></a>Dağıtım kapsamları

Bir aboneliğe dağıtım yaparken, bir aboneliği ve abonelik içindeki kaynak gruplarını hedefleyebilirsiniz. Hedef abonelikten farklı bir aboneliğe dağıtamazsınız. Şablonu dağıtan kullanıcının belirtilen kapsama erişimi olmalıdır.

Şablonun kaynaklar bölümünde tanımlanan kaynaklar aboneliğe uygulanır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        subscription-level-resources
    ],
    "outputs": {}
}
```

Abonelik içindeki bir kaynak grubunu hedeflemek için, iç içe geçmiş bir dağıtım ekleyin ve `resourceGroup` özelliğini ekleyin. Aşağıdaki örnekte, iç içe dağıtım adlı bir kaynak grubunu hedefler `rg2` .

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-06-01",
            "name": "nestedDeployment",
            "resourceGroup": "rg2",
            "properties": {
                "mode": "Incremental",
                "template": {
                    nested-template-with-resource-group-resources
                }
            }
        }
    ],
    "outputs": {}
}
```

Bu makalede, farklı kapsamlara kaynakların nasıl dağıtılacağını gösteren şablonlar bulabilirsiniz. Kaynak grubu oluşturan ve buna bir depolama hesabı dağıtan bir şablon için bkz. [kaynak grubu ve kaynaklar oluşturma](#create-resource-group-and-resources). Kaynak grubu oluşturan, buna bir kilit uygulayan ve kaynak grubu için bir rol atayan bir şablon için bkz. [erişim denetimi](#access-control).

## <a name="use-template-functions"></a>Şablon işlevlerini kullanma

Abonelik düzeyindeki dağıtımlar için, Şablon işlevleri kullanılırken bazı önemli noktalar vardır:

* [ResourceGroup ()](template-functions-resource.md#resourcegroup) **işlevi desteklenmiyor.**
* [Reference ()](template-functions-resource.md#reference) ve [List ()](template-functions-resource.md#list) işlevleri desteklenir.
* Abonelik düzeyinde dağıtılan kaynakların kaynak KIMLIĞINI almak için [RESOURCEID ()](template-functions-resource.md#resourceid) kullanmayın.

  Bunun yerine, [Subscriptionresourceıd ()](template-functions-resource.md#subscriptionresourceid) işlevini kullanın.

  Örneğin, bir aboneliğe dağıtılan bir ilke tanımının kaynak KIMLIĞINI almak için şunu kullanın:

  ```json
  subscriptionResourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))
  ```

  Döndürülen kaynak KIMLIĞI şu biçimdedir:

  ```json
  /subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="resource-groups"></a>Kaynak grupları

### <a name="create-resource-groups"></a>Kaynak grupları oluşturma

ARM şablonunda bir kaynak grubu oluşturmak için kaynak grubu için bir ad ve konum içeren bir [Microsoft. resources/resourceGroups](/azure/templates/microsoft.resources/allversions) kaynağı tanımlayın.

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
      "apiVersion": "2020-06-01",
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
      "apiVersion": "2020-06-01",
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

### <a name="create-resource-group-and-resources"></a>Kaynak grubu ve kaynakları oluşturma

Kaynak grubu oluşturmak ve kaynaklarına kaynak dağıtmak için, iç içe geçmiş bir şablon kullanın. İç içe şablon, kaynak grubuna dağıtılacak kaynakları tanımlar. Kaynak grubunun kaynakları dağıtımdan önce mevcut olduğundan emin olmak için, iç içe geçmiş şablonu kaynak grubuna bağımlı olarak ayarlayın. En fazla 800 kaynak grubuna dağıtım yapabilirsiniz.

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
      "apiVersion": "2020-06-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "storageDeployment",
      "resourceGroup": "[parameters('rgName')]",
      "dependsOn": [
        "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2019-06-01",
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
        "policyDefinitionId": "[subscriptionResourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
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

:::code language="json" source="~/quickstart-templates/subscription-deployments/blueprints-new-blueprint/azuredeploy.json":::

Aboneliğinizde şema tanımını oluşturmak için aşağıdaki CLI komutunu kullanın:

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/blueprints-new-blueprint/azuredeploy.json"
```

Bu şablonu PowerShell ile dağıtmak için şunu kullanın:

```azurepowershell
New-AzSubscriptionDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/blueprints-new-blueprint/azuredeploy.json"
```

## <a name="access-control"></a>Erişim denetimi

Rol atama hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonları kullanarak Azure rol atamaları ekleme](../../role-based-access-control/role-assignments-template.md).

Aşağıdaki örnek, bir kaynak grubu oluşturur, buna bir kilit uygular ve bir sorumlusu bir rol atar.

:::code language="json" source="~/quickstart-templates/subscription-deployments/create-rg-lock-role-assignment/azuredeploy.json":::

## <a name="next-steps"></a>Sonraki adımlar

* Azure Güvenlik Merkezi için çalışma alanı ayarlarını dağıtmaya ilişkin bir örnek için bkz. [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Örnek Şablonlar [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-deployments)' da bulunabilir.
* Ayrıca, şablonları [Yönetim grubu düzeyinde](deploy-to-management-group.md) ve [kiracı düzeyinde](deploy-to-tenant.md)dağıtabilirsiniz.
