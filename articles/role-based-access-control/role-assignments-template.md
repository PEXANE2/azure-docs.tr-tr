---
title: RBAC ve Azure Kaynak Yöneticisi şablonları ile rol atamaları ekleme
description: Azure rol tabanlı erişim denetimi (RBAC) ve Azure Kaynak Yöneticisi şablonlarını kullanarak kullanıcılar, gruplar, hizmet müdürleri veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim tanıyın öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 9f817880f938f5d03024e3aacd9b84817a5ac721
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77138296"
---
# <a name="add-role-assignments-using-azure-rbac-and-azure-resource-manager-templates"></a>Azure RBAC ve Azure Kaynak Yöneticisi şablonlarını kullanarak rol atamaları ekleme

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Azure PowerShell veya Azure CLI'yi kullanmanın yanı sıra, [Azure Kaynak Yöneticisi şablonlarını](../azure-resource-manager/templates/template-syntax.md)kullanarak roller atayabilirsiniz. Kaynakları tutarlı ve sürekli olarak dağıtmanız gerekiyorsa şablonlar yararlı olabilir. Bu makalede, şablonları kullanarak rollerin nasıl atayılanın açıklanmaktadır.

## <a name="get-object-ids"></a>Nesne tünelerini alma

Bir rol atamak için, rolü atamak istediğiniz kullanıcının, grubun veya uygulamanın kimliğini belirtmeniz gerekir. Kimlik biçimi vardır: `11111111-1111-1111-1111-111111111111`. Kimliği Azure portalını, Azure PowerShell'i veya Azure CLI'yi kullanarak alabilirsiniz.

### <a name="user"></a>Kullanıcı

Bir kullanıcının kimliğini almak için [Get-AzADUser](/powershell/module/az.resources/get-azaduser) veya [az reklam kullanıcı göster](/cli/azure/ad/user#az-ad-user-show) komutlarını kullanabilirsiniz.

```azurepowershell
$objectid = (Get-AzADUser -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad user show --id "{email}" --query objectId --output tsv)
```

### <a name="group"></a>Grup

Bir grubun kimliğini almak için [Get-AzADGroup](/powershell/module/az.resources/get-azadgroup) veya [az reklam grubu gösteri](/cli/azure/ad/group#az-ad-group-show) komutlarını kullanabilirsiniz.

```azurepowershell
$objectid = (Get-AzADGroup -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad group show --group "{name}" --query objectId --output tsv)
```

### <a name="application"></a>Uygulama

Bir hizmet sorumlusunun kimliğini (bir uygulama tarafından kullanılan kimlik) almak için [Get-AzADServicePrincipal](/powershell/module/az.resources/get-azadserviceprincipal) veya [az reklam sp listesi](/cli/azure/ad/sp#az-ad-sp-list) komutlarını kullanabilirsiniz. Bir hizmet sorumlusu için, uygulama kimliğini **değil,** nesne kimliğini kullanın.

```azurepowershell
$objectid = (Get-AzADServicePrincipal -DisplayName "{name}").id
```

```azurecli
objectid=$(az ad sp list --display-name "{name}" --query [].objectId --output tsv)
```

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

RBAC'da, erişim sağlamak için bir rol ataması eklersiniz.

### <a name="resource-group-without-parameters"></a>Kaynak grubu (parametreler olmadan)

Aşağıdaki şablon, rol ataması eklemenin temel bir yolunu gösterir. Şablon içinde bazı değerler belirtilir. Aşağıdaki şablon şunları gösterir:

-  [Okuyucu](built-in-roles.md#reader) rolünü kaynak grubu kapsamında bir kullanıcıya, gruba veya uygulamaya atama

Şablonu kullanmak için aşağıdakileri yapmanız gerekir:

- Yeni bir JSON dosyası oluşturun ve şablonu kopyalayın
- Rolü `<your-principal-id>` atamak için bir kullanıcının, grubun veya uygulamanın kimliğiyle değiştirin

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[guid(resourceGroup().id)]",
            "properties": {
                "roleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
                "principalId": "<your-principal-id>"
            }
        }
    ]
}
```

Örnek [Yeni-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) ve [az group deployment,](/cli/azure/group/deployment#az-group-deployment-create) ExampleGroup adlı bir kaynak grubunda dağıtımı başlatma komutları oluşturabilirsiniz.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json
```

Aşağıda, şablonu dağıttıktan sonra bir kaynak grubu için kullanıcıya Reader rol atamasının bir örneği gösterilmektedir.

![Kaynak grubu kapsamında rol ataması](./media/role-assignments-template/role-assignment-template.png)

### <a name="resource-group-or-subscription"></a>Kaynak grubu veya abonelik

Önceki şablon çok esnek değil. Aşağıdaki şablon parametreleri kullanır ve farklı kapsamlarda kullanılabilir. Aşağıdaki şablon şunları gösterir:

- Bir kaynak grubundaki veya abonelik kapsamında bir kullanıcıya, gruba veya uygulamaya nasıl rol atayacağım?
- Parametre olarak Sahip, Katılımcı ve Okuyucu rollerini belirtme

Şablonu kullanmak için aşağıdaki girdileri belirtmeniz gerekir:

- Rolü atamak için bir kullanıcının, grubun veya uygulamanın kimliği
- Rol ataması için kullanılacak veya varsayılan kimliği kullanabilirsiniz benzersiz bir kimlik

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "roleNameGuid": {
            "type": "string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "A new GUID used to identify the role assignment"
            }
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    },
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[parameters('roleNameGuid')]",
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

> [!NOTE]
> Şablonun her dağıtımı için parametre `roleNameGuid` olarak aynı değer sağlanmadıkça, bu şablon idempotent değildir. Hayır `roleNameGuid` sağlandığı takdirde, varsayılan olarak her dağıtımda yeni bir GUID `Conflict: RoleAssignmentExists` oluşturulur ve sonraki dağıtımlar bir hatayla başarısız olur.

Rol atamasının kapsamı dağıtım düzeyinden belirlenir. Aşağıda, kaynak grubu kapsamında dağıtımı başlatmanın nasıl başlatılacak larına yönelik yeni kaynak grubu [dağıtımı](/powershell/module/az.resources/new-azresourcegroupdeployment) ve az grup dağıtımı örnek komutları [verilmiştir.](/cli/azure/group/deployment#az-group-deployment-create)

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Reader
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Reader
```

Aşağıda, dağıtımın abonelik kapsamında nasıl başlatılalabilen ve konumu nasıl belirtilen [yeni-AzDeployment](/powershell/module/az.resources/new-azdeployment) ve [az deployment komutları](/cli/azure/deployment#az-deployment-create) örnek verilmiştir.

```azurepowershell
New-AzDeployment -Location centralus -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Reader
```

```azurecli
az deployment create --location centralus --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Reader
```

### <a name="resource"></a>Kaynak

Bir kaynak düzeyinde bir rol ataması eklemeniz gerekiyorsa, rol atamabiçimi farklıdır. Rolü atamak için kaynak sağlayıcısı ad alanı ve kaynak türünü sağlarsınız. Ayrıca, rol ataması adına kaynağın adını da eklersiniz.

Rol atamasının türü ve adı için aşağıdaki biçimi kullanın:

```json
"type": "{resource-provider-namespace}/{resource-type}/providers/roleAssignments",
"name": "{resource-name}/Microsoft.Authorization/{role-assign-GUID}"
```

Aşağıdaki şablon şunları gösterir:

- Yeni depolama hesabı oluşturma
- Depolama hesabı kapsamında bir kullanıcıya, gruba veya uygulamaya nasıl rol atayabilen
- Parametre olarak Sahip, Katılımcı ve Okuyucu rollerini belirtme

Şablonu kullanmak için aşağıdaki girdileri belirtmeniz gerekir:

- Rolü atamak için bir kullanıcının, grubun veya uygulamanın kimliği

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string",
            "metadata": {
                "description": "The principal to assign the role to"
            }
        },
        "builtInRoleType": {
            "type": "string",
            "allowedValues": [
                "Owner",
                "Contributor",
                "Reader"
            ],
            "metadata": {
                "description": "Built-in role to assign"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
        "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
        "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageName')]",
            "location": "[parameters('location')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        },
        {
            "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', guid(uniqueString(variables('storageName'))))]",
            "dependsOn": [
                "[variables('storageName')]"
            ],
            "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Önceki şablonu dağıtmak için kaynak grubu komutlarını kullanırsınız. Aşağıda, kaynak kapsamında dağıtımı başlatmanın nasıl başlatılacak larına yönelik [yeni-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) ve [az group deployment](/cli/azure/group/deployment#az-group-deployment-create) komutları örnek verilmiştir.

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-test.json -principalId $objectid -builtInRoleType Contributor
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file rbac-test.json --parameters principalId=$objectid builtInRoleType=Contributor
```

Aşağıda, şablonu dağıttıktan sonra bir depolama hesabı için kullanıcıya Katılımcı rol atamasının bir örneği gösterilmektedir.

![Kaynak kapsamında rol ataması](./media/role-assignments-template/role-assignment-template-resource.png)

### <a name="new-service-principal"></a>Yeni hizmet müdürü

Yeni bir hizmet ilkesi oluşturur ve hemen bu hizmet ilkesine bir rol atamaya çalışırsanız, bu rol ataması bazı durumlarda başarısız olabilir. Örneğin, yeni yönetilen bir kimlik oluşturun ve aynı Azure Kaynak Yöneticisi şablonundaki bu hizmet yöneticisine bir rol atamaya çalışırsanız, rol ataması başarısız olabilir. Bu hatanın nedeni büyük olasılıkla bir çoğaltma gecikmesidir. Hizmet ilkesi tek bir bölgede oluşturulur; ancak, rol ataması hizmet ilkesini henüz çoğaltmamış farklı bir bölgede oluşabilir. Bu senaryoyu gidermek için, `principalType` rol `ServicePrincipal` ataması oluştururken özelliği ayarlamanız gerekir.

Aşağıdaki şablon şunları gösterir:

- Yeni yönetilen kimlik hizmeti ilkesi nasıl oluşturulur?
- Nasıl belirtilir`principalType`
- Bir kaynak grubu kapsamında Katılımcı rolünü niçin bu hizmet ilkesine atama

Şablonu kullanmak için aşağıdaki girdileri belirtmeniz gerekir:

- Yönetilen kimliğin temel adı veya varsayılan dize kullanabilirsiniz

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "baseName": {
            "type": "string",
            "defaultValue": "msi-test"
        }
    },
    "variables": {
        "identityName": "[concat(parameters('baseName'), '-bootstrap')]",
        "bootstrapRoleAssignmentId": "[guid(concat(resourceGroup().id, 'contributor'))]",
        "contributorRoleDefinitionId": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]"
    },
    "resources": [
        {
            "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
            "name": "[variables('identityName')]",
            "apiVersion": "2018-11-30",
            "location": "[resourceGroup().location]"
        },
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "apiVersion": "2018-09-01-preview",
            "name": "[variables('bootstrapRoleAssignmentId')]",
            "dependsOn": [
                "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName'))]"
            ],
            "properties": {
                "roleDefinitionId": "[variables('contributorRoleDefinitionId')]",
                "principalId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities', variables('identityName')), '2018-11-30').principalId]",
                "scope": "[resourceGroup().id]",
                "principalType": "ServicePrincipal"
            }
        }
    ]
}
```

Aşağıda, kaynak grubu kapsamında dağıtımı başlatmanın nasıl başlatılacak larına yönelik yeni kaynak grubu [dağıtımı](/powershell/module/az.resources/new-azresourcegroupdeployment) ve az grup dağıtımı örnek komutları [verilmiştir.](/cli/azure/group/deployment#az-group-deployment-create)

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup2 -TemplateFile rbac-test.json
```

```azurecli
az group deployment create --resource-group ExampleGroup2 --template-file rbac-test.json
```

Aşağıda, şablonu dağıttıktan sonra yeni yönetilen bir kimlik hizmeti ilkesine Katkıda Bulunan rol atamasının bir örneği gösterilmektedir.

![Yeni yönetilen kimlik hizmeti ilkesi için rol ataması](./media/role-assignments-template/role-assignment-template-msi.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure portalı kullanarak Azure Resource Manager şablonu oluşturma ve dağıtma](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md)
- [Azure Kaynak Yöneticisi Şablonlarının yapısını ve sözdizimini anlama](../azure-resource-manager/templates/template-syntax.md)
- [Abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](../azure-resource-manager/templates/deploy-to-subscription.md)
- [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/?term=rbac)
