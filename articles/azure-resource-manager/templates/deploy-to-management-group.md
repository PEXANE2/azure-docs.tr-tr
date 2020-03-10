---
title: Kaynakları yönetim grubuna dağıtma
description: Azure Resource Manager şablonundaki yönetim grubu kapsamındaki kaynakların nasıl dağıtılacağını açıklar.
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: dc46762755718c798b4a7eed6f2dc6b8afce9b98
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78942760"
---
# <a name="create-resources-at-the-management-group-level"></a>Yönetim grubu düzeyinde kaynaklar oluşturma

Kuruluşunuz geliştikçe, bir yönetim grubu için [ilke](../../governance/policy/overview.md) veya [rol tabanlı erişim denetimleri](../../role-based-access-control/overview.md) tanımlamanız ve atamanız gerekebilir. Yönetim grubu düzeyi şablonları ile, ilkeleri yönetim grubu düzeyinde bildirimli olarak uygulayabilir ve roller atayabilirsiniz.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Yönetim grubu düzeyinde aşağıdaki kaynak türlerini dağıtabilirsiniz:

* [dağıtımlar](/azure/templates/microsoft.resources/deployments) -abonelikler veya kaynak gruplarına dağıtan iç içe şablonlar için.
* [Poliyasatamaları](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>Şema

Yönetim grubu dağıtımları için kullandığınız şema, kaynak grubu dağıtımları için şemadan farklıdır.

Şablonlar için şunu kullanın:

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#
```

Bir parametre dosyasının şeması, tüm dağıtım kapsamları için aynıdır. Parametre dosyaları için şunu kullanın:

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="deployment-commands"></a>Dağıtım komutları

Yönetim grubu dağıtımları için komutlar, kaynak grubu dağıtımları komutlarından farklıdır.

Azure PowerShell için [New-AzManagementGroupDeployment](/powershell/module/az.resources/new-azmanagementgroupdeployment)kullanın. 

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -ManagementGroupId "myMG" `
  -Location "West US" `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json
```

REST API için [dağıtımlar-yönetim grubu kapsamında oluştur](/rest/api/resources/deployments/createorupdateatmanagementgroupscope)' u kullanın.

## <a name="deployment-location-and-name"></a>Dağıtım konumu ve adı

Yönetim grubu düzeyinde dağıtımlar için, dağıtım için bir konum sağlamanız gerekir. Dağıtımın konumu, dağıttığınız kaynakların konumundan ayrıdır. Dağıtım konumu, dağıtım verilerinin depolanacağı konumu belirtir.

Dağıtım için bir ad verebilir veya varsayılan dağıtım adını kullanabilirsiniz. Varsayılan ad şablon dosyasının adıdır. Örneğin, **azuredeploy. JSON** adlı bir şablon dağıtmak, **azuredeploy**varsayılan dağıtım adını oluşturur.

Her dağıtım adı için konum sabittir. Farklı bir konumda aynı ada sahip mevcut bir dağıtım olduğunda tek bir konumda dağıtım oluşturamazsınız. `InvalidDeploymentLocation`hata kodunu alırsanız, bu adın önceki dağıtımıyla farklı bir ad veya aynı konumu kullanın.

## <a name="use-template-functions"></a>Şablon işlevlerini kullanma

Yönetim grubu dağıtımları için, Şablon işlevleri kullanılırken bazı önemli noktalar vardır:

* [ResourceGroup ()](template-functions-resource.md#resourcegroup) **işlevi desteklenmiyor.**
* [Subscription ()](template-functions-resource.md#subscription) **işlevi desteklenmiyor.**
* [Reference ()](template-functions-resource.md#reference) ve [List ()](template-functions-resource.md#list) işlevleri desteklenir.
* [RESOURCEID ()](template-functions-resource.md#resourceid) işlevi desteklenir. Yönetim grubu düzeyi dağıtımlarında kullanılan kaynakların kaynak KIMLIĞINI almak için kullanın. Kaynak grubu parametresi için bir değer sağlamamayın.

  Örneğin, bir ilke tanımının kaynak KIMLIĞINI almak için şunu kullanın:
  
  ```json
  resourceId('Microsoft.Authorization/policyDefinitions/', parameters('policyDefinition'))
  ```
  
  Döndürülen kaynak KIMLIĞI şu biçimdedir:
  
  ```json
  /providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-policies"></a>İlkeleri oluşturma

### <a name="define-policy"></a>İlke tanımlama

Aşağıdaki örnekte, yönetim grubu düzeyinde bir ilkenin nasıl [tanımlanacağı](../../governance/policy/concepts/definition-structure.md) gösterilmektedir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
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
    }
  ]
}
```

### <a name="assign-policy"></a>İlke ata

Aşağıdaki örnek, var olan bir ilke tanımını yönetim grubuna atar. İlke parametreleri alırsa, bunları bir nesne olarak sağlayın. İlke parametre almadıysanız varsayılan boş nesneyi kullanın.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
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
        "policyDefinitionId": "[parameters('policyDefinitionID')]",
        "parameters": "[parameters('policyParameters')]"
      }
    }
  ]
}
```

## <a name="template-sample"></a>Şablon örneği

* [Kaynak grubu, ilke ve ilke ataması oluşturun](https://github.com/Azure/azure-docs-json-samples/blob/master/management-level-deployment/azuredeploy.json).

## <a name="next-steps"></a>Sonraki adımlar

* Rol atama hakkında bilgi edinmek için bkz. [RBAC ve Azure Resource Manager şablonlarını kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-template.md).
* Azure Güvenlik Merkezi için çalışma alanı ayarlarını dağıtmaya ilişkin bir örnek için bkz. [Deployascwithworkspace Settings. JSON](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Ayrıca, [abonelik düzeyinde](deploy-to-subscription.md) ve [kiracı düzeyinde](deploy-to-tenant.md)şablonlar dağıtabilirsiniz.
