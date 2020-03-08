---
title: Kaynakları kiracıya dağıtma
description: Azure Resource Manager şablonundaki kiracı kapsamındaki kaynakların nasıl dağıtılacağını açıklar.
ms.topic: conceptual
ms.date: 03/06/2020
ms.openlocfilehash: d63697f3c140b5ad374607f1ecb00dad20e697de
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78899145"
---
# <a name="create-resources-at-the-tenant-level"></a>Kiracı düzeyinde kaynaklar oluşturma

Genellikle Azure kaynaklarını Azure aboneliğinizdeki bir kaynak grubuna dağıtırsınız. Bununla birlikte, kaynakları da oluşturabilirsiniz:

* [abonelik düzeyi](deploy-to-subscription.md)
* [Yönetim grubu düzeyi](deploy-to-management-group.md)
* Kiracı düzeyi (Bu makalede ele alınmıştır)

Bu düzeyde, [rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) atama ya da [ilke](../../governance/policy/overview.md)uygulama gibi işlemler gerçekleştirmek için kiracı düzeyinde dağıtımlar kullanırsınız.

## <a name="supported-resources"></a>Desteklenen kaynaklar

Aşağıdaki kaynak türlerini kiracı düzeyinde dağıtabilirsiniz:

* [dağıtımlar](/azure/templates/microsoft.resources/deployments) -yönetim gruplarına veya aboneliklerine dağıtan iç içe şablonlar için.
* [Poliyasatamaları](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)

### <a name="schema"></a>Şema

Kiracı dağıtımları için kullandığınız şema, kaynak grubu dağıtımları için şemadan farklıdır.

Şablonlar için şunu kullanın:

```json
https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentTemplate.json#
```

Parametre dosyaları için şunu kullanın:

```json
https://schema.management.azure.com/schemas/2019-08-01/tenantDeploymentParameters.json#
```

## <a name="required-access"></a>Gerekli erişim

Şablonu dağıtmanın asıl, kiracı kapsamında kaynak oluşturmak için izinlere sahip olmalıdır. Sorumlu, dağıtım eylemlerini (`Microsoft.Resources/deployments/*`) yürütme ve şablonda tanımlanan kaynakları oluşturma iznine sahip olmalıdır. Örneğin, bir yönetim grubu oluşturmak için sorumlu, kiracı kapsamında katkıda bulunan iznine sahip olmalıdır. Rol atamaları oluşturmak için sorumlunun sahip iznine sahip olması gerekir.

Azure Active Directory genel yöneticisinin rol atama izni otomatik olarak yoktur. Şablon dağıtımlarını kiracı kapsamında etkinleştirmek için, genel yönetici aşağıdaki adımları vermelidir:

1. Genel yöneticinin rol atayabilmesi için hesap erişimini yükseltin. Daha fazla bilgi için bkz. [tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimi yükseltme](../../role-based-access-control/elevate-access-global-admin.md).

1. Şablonları dağıtmak için gereken sorumlu sahibine veya katkıda bulunanı atayın.

   ```azurepowershell-interactive
   New-AzRoleAssignment -SignInName "[userId]" -Scope "/" -RoleDefinitionName "Owner"
   ```

   ```azurecli-interactive
   az role assignment create --assignee "[userId]" --scope "/" --role "Owner"
   ```

Asıl öğe artık şablonu dağıtmak için gerekli izinlere sahiptir.

## <a name="deployment-commands"></a>Dağıtım komutları

Kiracı dağıtımları komutları, kaynak grubu dağıtımları için komutlardan farklıdır.

Azure PowerShell için [New-AzTenantDeployment](/powershell/module/az.resources/new-aztenantdeployment)kullanın.

```azurepowershell-interactive
New-AzTenantDeployment `
  -Location "West US" `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/tenant-level-deployments/new-mg/azuredeploy.json
```

REST API için dağıtımları kullanın [-kiracı kapsamında oluşturun veya güncelleştirin](/rest/api/resources/deployments/createorupdateattenantscope).

## <a name="deployment-location-and-name"></a>Dağıtım konumu ve adı

Kiracı düzeyinde dağıtımlar için dağıtım için bir konum sağlamanız gerekir. Dağıtımın konumu, dağıttığınız kaynakların konumundan ayrıdır. Dağıtım konumu, dağıtım verilerinin depolanacağı konumu belirtir.

Dağıtım için bir ad verebilir veya varsayılan dağıtım adını kullanabilirsiniz. Varsayılan ad şablon dosyasının adıdır. Örneğin, **azuredeploy. JSON** adlı bir şablon dağıtmak, **azuredeploy**varsayılan dağıtım adını oluşturur.

Her dağıtım adı için konum sabittir. Farklı bir konumda aynı ada sahip mevcut bir dağıtım olduğunda tek bir konumda dağıtım oluşturamazsınız. `InvalidDeploymentLocation`hata kodunu alırsanız, bu adın önceki dağıtımıyla farklı bir ad veya aynı konumu kullanın.

## <a name="use-template-functions"></a>Şablon işlevlerini kullanma

Kiracı dağıtımları için, Şablon işlevleri kullanılırken bazı önemli noktalar vardır:

* [ResourceGroup ()](template-functions-resource.md#resourcegroup) **işlevi desteklenmiyor.**
* [Subscription ()](template-functions-resource.md#subscription) **işlevi desteklenmiyor.**
* [Reference ()](template-functions-resource.md#reference) ve [List ()](template-functions-resource.md#list) işlevleri desteklenir.
* Kiracı düzeyinde dağıtılan kaynakların kaynak KIMLIĞINI almak için [Tenantresourceıd ()](template-functions-resource.md#tenantresourceid) işlevini kullanın.

  Örneğin, bir ilke tanımının kaynak KIMLIĞINI almak için şunu kullanın:
  
  ```json
  tenantResourceId('Microsoft.Authorization/policyDefinitions/', parameters('policyDefinition'))
  ```
  
  Döndürülen kaynak KIMLIĞI şu biçimdedir:
  
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

## <a name="assign-role"></a>Rol ata

[Aşağıdaki şablon](https://github.com/Azure/azure-quickstart-templates/tree/master/tenant-level-deployments/tenant-role-assignment) , kiracı kapsamına bir rol atar.

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

* Rol atama hakkında bilgi edinmek için bkz. [RBAC ve Azure Resource Manager şablonlarını kullanarak Azure kaynaklarına erişimi yönetme](../../role-based-access-control/role-assignments-template.md).
* Azure Resource Manager şablonları oluşturma hakkında bilgi edinmek için bkz. [yazma şablonları](template-syntax.md).
* Bir şablondaki kullanılabilir işlevlerin listesi için bkz. [Şablon işlevleri](template-functions.md).