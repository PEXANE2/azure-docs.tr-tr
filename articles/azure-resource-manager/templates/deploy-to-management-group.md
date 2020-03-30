---
title: Kaynakları yönetim grubuna dağıtma
description: Azure Kaynak Yöneticisi şablonunda yönetim grubu kapsamındaki kaynakların nasıl dağıtılancaya kadar dağıtılabildiğini açıklar.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: 863d1330412fa238b820eb0f1f05351fc723de6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460322"
---
# <a name="create-resources-at-the-management-group-level"></a>Yönetim grubu düzeyinde kaynak oluşturma

Kuruluşunuz olgunlaştıkça, bir yönetim grubu için ilkeler veya [rol tabanlı erişim denetimleri](../../role-based-access-control/overview.md) tanımlamanız ve atamanız gerekebilir. [policies](../../governance/policy/overview.md) Yönetim grubu düzeyi şablonları ile, bildirimsel olarak ilkeleri uygulayabilir ve yönetim grubu düzeyinde roller atayabilirsiniz.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Aşağıdaki kaynak türlerini yönetim grubu düzeyinde dağıtabilirsiniz:

* [dağıtımlar](/azure/templates/microsoft.resources/deployments) - aboneliklere veya kaynak gruplarına dağıtılan iç içe yönelik şablonlar için.
* [ilkeAtamalar](/azure/templates/microsoft.authorization/policyassignments)
* [policyTanımlar](/azure/templates/microsoft.authorization/policydefinitions)
* [politikaSetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAtamaları](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>Şema

Yönetim grubu dağıtımları için kullandığınız şema, kaynak grubu dağıtımları için şemadan farklıdır.

Şablonlar için şunları kullanın:

```json
https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#
```

Parametre dosyasının şeması tüm dağıtım kapsamları için aynıdır. Parametre dosyaları için şunları kullanın:

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="deployment-commands"></a>Dağıtım komutları

Yönetim grubu dağıtımları için komutlar kaynak grubu dağıtımları için komutlardan farklıdır.

Azure CLI için [az dağıtım mg oluştur](/cli/azure/deployment/mg?view=azure-cli-latest#az-deployment-mg-create)kullanın:

```azurecli-interactive
az deployment mg create \
  --name demoMGDeployment \
  --location WestUS \
  --management-group-id myMG \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

Azure PowerShell için [Yeni-AzManagementGroupDeployment'ı](/powershell/module/az.resources/new-azmanagementgroupdeployment)kullanın.

```azurepowershell-interactive
New-AzManagementGroupDeployment `
  -Name demoMGDeployment `
  -Location "West US" `
  -ManagementGroupId "myMG" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/management-level-deployment/azuredeploy.json"
```

REST API için Dağıtımları kullanın [- Yönetim Grubu Kapsamı'nda Oluştur.](/rest/api/resources/deployments/createorupdateatmanagementgroupscope)

## <a name="deployment-location-and-name"></a>Dağıtım konumu ve adı

Yönetim grubu düzeyindeki dağıtımlar için dağıtım için bir konum sağlamanız gerekir. Dağıtımın konumu, dağıttığınız kaynakların konumundan ayrıdır. Dağıtım konumu dağıtım verilerinin nerede depolanır olduğunu belirtir.

Dağıtım için bir ad sağlayabilir veya varsayılan dağıtım adını kullanabilirsiniz. Varsayılan ad şablon dosyasının adıdır. Örneğin, **azuredeploy.json** adlı bir şablonu dağıtmak **azuredeploy**varsayılan dağıtım adı oluşturur.

Her dağıtım adı için konum değişmez. Farklı bir konumda aynı ada sahip varolan bir dağıtım olduğunda, tek bir konumda dağıtım oluşturamazsınız. Hata kodunu `InvalidDeploymentLocation`alırsanız, bu ad için farklı bir ad veya önceki dağıtımla aynı konumu kullanın.

## <a name="use-template-functions"></a>Şablon işlevlerini kullanma

Yönetim grubu dağıtımları için şablon işlevlerini kullanırken bazı önemli noktalar vardır:

* [ResourceGroup()](template-functions-resource.md#resourcegroup) işlevi **desteklenmez.**
* [Abonelik()](template-functions-resource.md#subscription) işlevi **desteklenmez.**
* [Başvuru()](template-functions-resource.md#reference) ve [list()](template-functions-resource.md#list) işlevleri desteklenir.
* [ResourceId()](template-functions-resource.md#resourceid) işlevi desteklenir. Yönetim grubu düzeyindeki dağıtımlarda kullanılan kaynaklar için kaynak kimliğini almak için kullanın. Kaynak grubu parametresi için bir değer sağlamayın.

  Örneğin, bir ilke tanımı için kaynak kimliğini almak için şunları kullanın:
  
  ```json
  resourceId('Microsoft.Authorization/policyDefinitions/', parameters('policyDefinition'))
  ```
  
  Döndürülen kaynak kimliği aşağıdaki biçime sahiptir:
  
  ```json
  /providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-policies"></a>İlkeleri oluşturma

### <a name="define-policy"></a>İlkeyi tanımla

Aşağıdaki örnek, yönetim grubu düzeyinde bir ilkenin nasıl [tanımlandığını](../../governance/policy/concepts/definition-structure.md) gösterir.

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

### <a name="assign-policy"></a>İlke atama

Aşağıdaki örnek, yönetim grubuna varolan bir ilke tanımı atar. İlke parametreleri alıyorsa, bunları nesne olarak sağlayın. İlke parametreleri almazsa, varsayılan boş nesneyi kullanın.

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

* [Kaynak grubu, ilke ve ilke ataması oluşturun.](https://github.com/Azure/azure-docs-json-samples/blob/master/management-level-deployment/azuredeploy.json)

## <a name="next-steps"></a>Sonraki adımlar

* Roller atama hakkında bilgi edinmek [için RBAC ve Azure Kaynak Yöneticisi şablonlarını kullanarak Azure kaynaklarına erişimi yönet'e](../../role-based-access-control/role-assignments-template.md)bakın.
* Azure Güvenlik Merkezi için çalışma alanı ayarlarını dağıtma örneği [için](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json)bkz.
* Şablonları [abonelik düzeyinde](deploy-to-subscription.md) ve [kiracı düzeyinde](deploy-to-tenant.md)de dağıtabilirsiniz.
