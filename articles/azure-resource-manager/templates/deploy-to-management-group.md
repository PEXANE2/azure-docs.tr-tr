---
title: Kaynakları yönetim grubuna dağıtma
description: Azure Resource Manager şablonundaki yönetim grubu kapsamındaki kaynakların nasıl dağıtılacağını açıklar.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 084ab69f463334569d37efd9187bfe587bfc524d
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92668942"
---
# <a name="management-group-deployments-with-arm-templates"></a>ARM şablonlarıyla yönetim grubu dağıtımları

Kuruluşunuz geliştikçe, yönetim grubu düzeyinde kaynak oluşturmak için bir Azure Resource Manager şablonu (ARM şablonu) dağıtabilirsiniz. Örneğin, bir yönetim grubu için [ilkeler](../../governance/policy/overview.md) veya [Azure rol tabanlı erişim denetımı (Azure RBAC)](../../role-based-access-control/overview.md) tanımlamanız ve atamanız gerekebilir. Yönetim grubu düzeyi şablonları ile, ilkeleri yönetim grubu düzeyinde bildirimli olarak uygulayabilir ve roller atayabilirsiniz.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Tüm kaynak türleri yönetim grubu düzeyine dağıtılamaz. Bu bölümde hangi kaynak türlerinin desteklendiği listelenmektedir.

Azure şemaları için şunu kullanın:

* [Yapıt](/azure/templates/microsoft.blueprint/blueprints/artifacts)
* [Blueprint](/azure/templates/microsoft.blueprint/blueprints)
* [Şema tasmi](/azure/templates/microsoft.blueprint/blueprintassignments)
* [ün](/azure/templates/microsoft.blueprint/blueprints/versions)

Azure Ilkeleri için şunu kullanın:

* [Poliyasatamaları](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [düzeltmeler](/azure/templates/microsoft.policyinsights/remediations)

Azure rol tabanlı erişim denetimi (Azure RBAC) için şunu kullanın:

* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

Abonelikler veya kaynak gruplarına dağıtan iç içe şablonlar için şunu kullanın:

* [dağıtımlar](/azure/templates/microsoft.resources/deployments)

Kaynaklarınızı yönetmek için şunu kullanın:

* [lerimi](/azure/templates/microsoft.resources/tags)

## <a name="schema"></a>Şema

Yönetim grubu dağıtımları için kullandığınız şema, kaynak grubu dağıtımları için şemadan farklıdır.

Şablonlar için şunu kullanın:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    ...
}
```

Bir parametre dosyasının şeması, tüm dağıtım kapsamları için aynıdır. Parametre dosyaları için şunu kullanın:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    ...
}
```

## <a name="deployment-commands"></a>Dağıtım komutları

Bir yönetim grubuna dağıtmak için yönetim grubu dağıtım komutlarını kullanın.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı için [az Deployment mg Create](/cli/azure/deployment/mg#az-deployment-mg-create)kullanın:

```azurecli-interactive
az deployment mg create \
  --name demoMGDeployment \
  --location WestUS \
  --management-group-id myMG \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Azure PowerShell için [New-AzManagementGroupDeployment](/powershell/module/az.resources/new-azmanagementgroupdeployment)kullanın.

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -Name demoMGDeployment `
  -Location "West US" `
  -ManagementGroupId "myMG" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

---

ARM şablonları dağıtmaya yönelik dağıtım komutları ve seçenekleri hakkında daha ayrıntılı bilgi için bkz.:

* [ARM şablonları ve Azure portal kaynak dağıtma](deploy-portal.md)
* [ARM şablonları ve Azure CLı ile kaynak dağıtma](deploy-cli.md)
* [ARM şablonları ve Azure PowerShell kaynak dağıtma](deploy-powershell.md)
* [ARM şablonlarıyla kaynakları dağıtma ve Azure Resource Manager REST API](deploy-rest.md)
* [GitHub deposundan şablon dağıtmak için bir dağıtım düğmesi kullanın](deploy-to-azure-button.md)
* [ARM şablonlarını Cloud Shell dağıtma](deploy-cloud-shell.md)

## <a name="deployment-scopes"></a>Dağıtım kapsamları

Bir yönetim grubuna dağıtırken, kaynakların dağıtımını yapabilirsiniz:

* işlemin hedef yönetim grubu
* Kiracıdaki başka bir yönetim grubu
* Yönetim grubundaki abonelikler
* Yönetim grubundaki kaynak grupları (iki iç içe dağıtım aracılığıyla)
* [uzantı kaynakları](scope-extension-resources.md) , kaynaklara uygulanabilir

Şablonu dağıtan kullanıcının belirtilen kapsama erişimi olmalıdır.

Bu bölümde, farklı kapsamların nasıl ayarlanacağı gösterilmektedir. Bu farklı kapsamları tek bir şablonda birleştirebilirsiniz.

### <a name="scope-to-target-management-group"></a>Hedef yönetim grubuna kapsam

Şablonun kaynaklar bölümünde tanımlanan kaynaklar, dağıtım komutundan yönetim grubuna uygulanır.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/default-mg.json" highlight="5":::

### <a name="scope-to-another-management-group"></a>Kapsamı başka bir yönetim grubuna

Başka bir yönetim grubunu hedeflemek için, iç içe geçmiş bir dağıtım ekleyin ve `scope` özelliğini belirtin. `scope`Özelliğini, biçiminde bir değer olarak ayarlayın `Microsoft.Management/managementGroups/<mg-name>` .

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/scope-mg.json" highlight="10,17,22":::

### <a name="scope-to-subscription"></a>Abonelik kapsamı

Ayrıca, abonelikleri bir yönetim grubu içinde de hedefleyebilirsiniz. Şablonu dağıtan kullanıcının belirtilen kapsama erişimi olmalıdır.

Yönetim grubu içinde bir aboneliği hedeflemek için, iç içe geçmiş bir dağıtım ve `subscriptionId` özelliğini kullanın.

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/mg-to-subscription.json" highlight="10,18":::

### <a name="scope-to-resource-group"></a>Kapsam-kaynak grubu

Bu abonelik içindeki bir kaynak grubunu hedeflemek için, iki iç içe dağıtım ekleyin. İlki kaynak grubuna sahip aboneliği hedefler. İkincisi, özelliği ayarlayarak kaynak grubunu hedefler `resourceGroup` .

:::code language="json" source="~/resourcemanager-templates/azure-resource-manager/scope/mg-to-resource-group.json" highlight="10,21,25":::

Bir abonelikte kaynak grubu oluşturmaya ve bu kaynak grubuna bir depolama hesabı dağıtmaya yönelik bir yönetim grubu dağıtımı kullanmak için, bkz. [aboneliğe ve kaynak grubuna dağıtım](#deploy-to-subscription-and-resource-group).

## <a name="deployment-location-and-name"></a>Dağıtım konumu ve adı

Yönetim grubu düzeyinde dağıtımlar için, dağıtım için bir konum sağlamanız gerekir. Dağıtımın konumu, dağıttığınız kaynakların konumundan ayrıdır. Dağıtım konumu, dağıtım verilerinin depolanacağı konumu belirtir.

Dağıtım için bir ad verebilir veya varsayılan dağıtım adını kullanabilirsiniz. Varsayılan ad şablon dosyasının adıdır. Örneğin, **üzerindeazuredeploy.js** adlı bir şablon dağıtmak, **azuredeploy** varsayılan dağıtım adını oluşturur.

Her dağıtım adı için konum sabittir. Farklı bir konumda aynı ada sahip mevcut bir dağıtım olduğunda tek bir konumda dağıtım oluşturamazsınız. Hata kodunu alırsanız `InvalidDeploymentLocation` , bu ad için önceki dağıtımla farklı bir ad veya aynı konumu kullanın.

## <a name="azure-policy"></a>Azure İlkesi

Yönetim grubuna dağıtılan özel ilke tanımları, yönetim grubunun uzantılarıdır. Özel bir ilke tanımının KIMLIĞINI almak için [Extensionresourceıd ()](template-functions-resource.md#extensionresourceid) işlevini kullanın. Yerleşik ilke tanımları, kiracı düzeyi kaynaklarıdır. Yerleşik bir ilke tanımının KIMLIĞINI almak için, [Tenantresourceıd](template-functions-resource.md#tenantresourceid) işlevini kullanın.

Aşağıdaki örnek, yönetim grubu düzeyinde bir ilkenin nasıl [tanımlanacağını](../../governance/policy/concepts/definition-structure.md) gösterir ve bunu atar.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "targetMG": {
            "type": "string",
            "metadata": {
                "description": "Target Management Group"
            }
        },
        "allowedLocations": {
            "type": "array",
            "defaultValue": [
                "australiaeast",
                "australiasoutheast",
                "australiacentral"
            ],
            "metadata": {
                "description": "An array of the allowed locations, all other locations will be denied by the created policy."
            }
        }
    },
    "variables": {
        "mgScope": "[tenantResourceId('Microsoft.Management/managementGroups', parameters('targetMG'))]",
        "policyDefinition": "LocationRestriction"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "[variables('policyDefinition')]",
            "apiVersion": "2019-09-01",
            "properties": {
                "policyType": "Custom",
                "mode": "All",
                "parameters": {
                },
                "policyRule": {
                    "if": {
                        "not": {
                            "field": "location",
                            "in": "[parameters('allowedLocations')]"
                        }
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
            "apiVersion": "2019-09-01",
            "dependsOn": [
                "[variables('policyDefinition')]"
            ],
            "properties": {
                "scope": "[variables('mgScope')]",
                "policyDefinitionId": "[extensionResourceId(variables('mgScope'), 'Microsoft.Authorization/policyDefinitions', variables('policyDefinition'))]"
            }
        }
    ]
}
```

## <a name="deploy-to-subscription-and-resource-group"></a>Aboneliğe ve kaynak grubuna dağıtın

Yönetim grubu düzeyi dağıtımından, yönetim grubu içinde bir aboneliği hedefleyebilirsiniz. Aşağıdaki örnek, bir abonelik içinde bir kaynak grubu oluşturur ve bu kaynak grubuna bir depolama hesabı dağıtır.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nestedsubId": {
      "type": "string"
    },
    "nestedRG": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    "nestedLocation": {
      "type": "string"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2020-06-01",
      "name": "nestedSub",
      "location": "[parameters('nestedLocation')]",
      "subscriptionId": "[parameters('nestedSubId')]",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
            {
              "type": "Microsoft.Resources/resourceGroups",
              "apiVersion": "2020-06-01",
              "name": "[parameters('nestedRG')]",
              "location": "[parameters('nestedLocation')]",
            },
            {
              "type": "Microsoft.Resources/deployments",
              "apiVersion": "2020-06-01",
              "name": "nestedSubRG",
              "resourceGroup": "[parameters('nestedRG')]",
              "dependsOn": [
                "[parameters('nestedRG')]"
              ],
              "properties": {
                "mode": "Incremental",
                "template": {
                  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                  "contentVersion": "1.0.0.0",
                  "resources": [
                    {
                      "type": "Microsoft.Storage/storageAccounts",
                      "apiVersion": "2019-04-01",
                      "name": "[parameters('storageAccountName')]",
                      "location": "[parameters('nestedLocation')]",
                      "sku": {
                        "name": "Standard_LRS"
                      }
                    }
                  ]
                }
              }
            }
          ]
        }
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Rol atama hakkında bilgi edinmek için bkz. [Azure Resource Manager şablonları kullanarak Azure rol atamaları ekleme](../../role-based-access-control/role-assignments-template.md).
* Azure Güvenlik Merkezi için çalışma alanı ayarlarını dağıtmaya ilişkin bir örnek için bkz. [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Ayrıca, [abonelik düzeyinde](deploy-to-subscription.md) ve [kiracı düzeyinde](deploy-to-tenant.md)şablonlar dağıtabilirsiniz.
