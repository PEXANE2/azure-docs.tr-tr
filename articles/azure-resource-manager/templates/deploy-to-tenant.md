---
title: Kaynakları kiracıya dağıtma
description: Azure Kaynak Yöneticisi şablonunda kiracı kapsamındaki kaynakların nasıl dağıtılancaya kadar dağıtılabildiğini açıklar.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: fcdfc5b1c4333a0d7eeec80a09ad85579a1f8b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460271"
---
# <a name="create-resources-at-the-tenant-level"></a>Kiracı düzeyinde kaynak oluşturma

Kuruluşunuz olgunlaştıkça, Azure AD kiracınızda ilkeler veya [rol tabanlı erişim denetimleri](../../role-based-access-control/overview.md) tanımlamanız ve atamanız gerekebilir. [policies](../../governance/policy/overview.md) Kiracı düzeyi şablonları ile, bildirimsel olarak ilkeleri uygulayabilir ve genel düzeyde roller atayabilirsiniz.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Aşağıdaki kaynak türlerini kiracı düzeyinde dağıtabilirsiniz:

* [dağıtımlar](/azure/templates/microsoft.resources/deployments) - yönetim gruplarına veya aboneliklere dağıtılan iç içe yönelik şablonlar için.
* [ilkeAtamalar](/azure/templates/microsoft.authorization/policyassignments)
* [policyTanımlar](/azure/templates/microsoft.authorization/policydefinitions)
* [politikaSetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAtamaları](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>Şema

Kiracı dağıtımları için kullandığınız şema, kaynak grubu dağıtımları için şemafarklıdır.

Şablonlar için şunları kullanın:

```json
https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#
```

Parametre dosyasının şeması tüm dağıtım kapsamları için aynıdır. Parametre dosyaları için şunları kullanın:

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="required-access"></a>Gerekli erişim

Şablonu dağıtan asıl kaynağın, kiracı kapsamında kaynak oluşturmak için izinleri olmalıdır. Asıl, dağıtım eylemlerini yürütmek (`Microsoft.Resources/deployments/*`) ve şablonda tanımlanan kaynakları oluşturmak için izin almalıdır. Örneğin, bir yönetim grubu oluşturmak için, asıl yöneticinin kiracı kapsamında Katılımcı iznine sahip olması gerekir. Rol atamaları oluşturmak için, asıl yöneticinin Sahibinin izni olmalıdır.

Azure Etkin Dizini Için Genel Yöneticinin otomatik olarak rol atama izni yoktur. Şablon dağıtımlarını kiracı kapsamında etkinleştirmek için, Genel Yönetici aşağıdaki adımları yapmalıdır:

1. Genel Yönetici'nin roller atayabilmesi için hesap erişimini yükseltin. Daha fazla bilgi için [bkz.](../../role-based-access-control/elevate-access-global-admin.md)

1. Şablonları dağıtması gereken ana müdüre Sahibi veya Katılımcıyı atayın.

   ```azurepowershell-interactive
   New-AzRoleAssignment -SignInName "[userId]" -Scope "/" -RoleDefinitionName "Owner"
   ```

   ```azurecli-interactive
   az role assignment create --assignee "[userId]" --scope "/" --role "Owner"
   ```

Asıl şimdi şablonu dağıtmak için gerekli izinlere sahiptir.

## <a name="deployment-commands"></a>Dağıtım komutları

Kiracı dağıtımları için komutlar kaynak grubu dağıtımları için komutlardan farklıdır.

Azure CLI için [az dağıtım kiracı oluşturma kullanın:](/cli/azure/deployment/tenant?view=azure-cli-latest#az-deployment-tenant-create)

```azurecli-interactive
az deployment tenant create \
  --name demoTenantDeployment \
  --location WestUS \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-level-deployments/new-mg/azuredeploy.json"
```

Azure PowerShell için [Yeni-AzTenantDeployment'ı](/powershell/module/az.resources/new-aztenantdeployment)kullanın.

```azurepowershell-interactive
New-AzTenantDeployment `
  -Name demoTenantDeployment `
  -Location "West US" `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-level-deployments/new-mg/azuredeploy.json"
```

REST API için Dağıtımları kullanın [- Kiracı Kapsamında Oluştur veya Güncelleştir.](/rest/api/resources/deployments/createorupdateattenantscope)

## <a name="deployment-location-and-name"></a>Dağıtım konumu ve adı

Kiracı düzeyindeki dağıtımlar için dağıtım için bir konum sağlamanız gerekir. Dağıtımın konumu, dağıttığınız kaynakların konumundan ayrıdır. Dağıtım konumu dağıtım verilerinin nerede depolanır olduğunu belirtir.

Dağıtım için bir ad sağlayabilir veya varsayılan dağıtım adını kullanabilirsiniz. Varsayılan ad şablon dosyasının adıdır. Örneğin, **azuredeploy.json** adlı bir şablonu dağıtmak **azuredeploy**varsayılan dağıtım adı oluşturur.

Her dağıtım adı için konum değişmez. Farklı bir konumda aynı ada sahip varolan bir dağıtım olduğunda, tek bir konumda dağıtım oluşturamazsınız. Hata kodunu `InvalidDeploymentLocation`alırsanız, bu ad için farklı bir ad veya önceki dağıtımla aynı konumu kullanın.

## <a name="use-template-functions"></a>Şablon işlevlerini kullanma

Kiracı dağıtımları için şablon işlevlerini kullanırken bazı önemli noktalar vardır:

* [ResourceGroup()](template-functions-resource.md#resourcegroup) işlevi **desteklenmez.**
* [Abonelik()](template-functions-resource.md#subscription) işlevi **desteklenmez.**
* [Başvuru()](template-functions-resource.md#reference) ve [list()](template-functions-resource.md#list) işlevleri desteklenir.
* Kiracı düzeyinde dağıtılan kaynaklar için kaynak kimliğini almak için [tenantResourceId()](template-functions-resource.md#tenantresourceid) işlevini kullanın.

  Örneğin, bir ilke tanımı için kaynak kimliğini almak için şunları kullanın:
  
  ```json
  tenantResourceId('Microsoft.Authorization/policyDefinitions/', parameters('policyDefinition'))
  ```
  
  Döndürülen kaynak kimliği aşağıdaki biçime sahiptir:
  
  ```json
  /providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-management-group"></a>Yönetim grubu oluşturma

[Aşağıdaki şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-level-deployments/new-mg) bir yönetim grubu oluşturur.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "mgName": {
      "type": "string",
      "defaultValue": "[concat('mg-', uniqueString(newGuid()))]"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Management/managementGroups",
      "apiVersion": "2019-11-01",
      "name": "[parameters('mgName')]",
      "properties": {
      }
    }
  ]
}
```

## <a name="assign-role"></a>Rol atama

[Aşağıdaki şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-level-deployments/tenant-role-assignment) kiracı kapsamında bir rol atar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "principalId if the user that will be given contributor access to the resourceGroup"
      }
    },
    "roleDefinitionId": {
      "type": "string",
      "defaultValue": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
      "metadata": {
        "description": "roleDefinition for the assignment - default is owner"
      }
    }
  },
  "variables": {
    // This creates an idempotent guid for the role assignment
    "roleAssignmentName": "[guid('/', parameters('principalId'), parameters('roleDefinitionId'))]"
  },
  "resources": [
    {
      "name": "[variables('roleAssignmentName')]",
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2019-04-01-preview",
      "properties": {
        "roleDefinitionId": "[tenantResourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "/"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Roller atama hakkında bilgi edinmek [için RBAC ve Azure Kaynak Yöneticisi şablonlarını kullanarak Azure kaynaklarına erişimi yönet'e](../../role-based-access-control/role-assignments-template.md)bakın.
* Şablonları [abonelik düzeyinde](deploy-to-subscription.md) veya yönetim [grubu düzeyinde](deploy-to-management-group.md)de dağıtabilirsiniz.
