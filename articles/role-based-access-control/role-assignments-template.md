---
title: RBAC ve Azure Resource Manager şablonlarını kullanarak Azure kaynaklarına erişimi yönetme | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) ve Azure Resource Manager şablonları kullanarak kullanıcılar, gruplar ve uygulamalar için Azure kaynaklarına erişimi yönetmeyi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e6511ff84c251577a5ff483f892387ab7d3d4d41
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68360483"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-resource-manager-templates"></a>RBAC ve Azure Resource Manager şablonlarını kullanarak Azure kaynaklarına erişimi yönetme

[Rol tabanlı erişim denetimi (RBAC)](overview.md) , Azure kaynaklarına erişimi yönetme yöntemidir. Azure PowerShell veya Azure CLı kullanmaya ek olarak, Azure kaynaklarına erişimi RBAC ve [Azure Resource Manager şablonlarını](../azure-resource-manager/resource-group-authoring-templates.md)kullanarak yönetebilirsiniz. Kaynakları sürekli ve sürekli olarak dağıtmanız gerektiğinde şablonlar yararlı olabilir. Bu makalede, RBAC ve şablonlar kullanarak erişimi nasıl yönetebileceğinizi anlatmaktadır.

## <a name="assign-role-to-resource-group-or-subscription"></a>Kaynak grubuna veya aboneliğe rol atama

RBAC'de erişim vermek için bir rol ataması oluşturmanız gerekir. Aşağıdaki şablonda şunları gösterilmektedir:
- Kaynak grubunda veya abonelik kapsamında bir Kullanıcı, Grup veya uygulamaya rol atama
- Sahip, katkıda bulunan ve okuyucu rollerini parametre olarak belirtme

Şablonu kullanmak için aşağıdaki girişleri belirtmeniz gerekir:
- Rolün atanacağı bir kullanıcının, grubun veya uygulamanın benzersiz tanımlayıcısı
- Atanacak rol
- Rol ataması için kullanılacak benzersiz bir tanımlayıcı ya da varsayılan tanımlayıcıyı kullanabilirsiniz

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

Aşağıda, şablonu dağıttıktan sonra bir kaynak grubu için kullanıcıya bir okuyucu rolü atama örneği gösterilmektedir.

![Şablon kullanarak rol atama](./media/role-assignments-template/role-assignment-template.png)

Rol atamasının kapsamı, dağıtımın düzeyinden belirlenir. Kaynak grubu ve abonelik düzeyi dağıtım komutları bu makalede gösterilmektedir.

## <a name="assign-role-to-resource"></a>Kaynağa rol atama

Bir kaynak düzeyinde bir rol ataması oluşturmanız gerekiyorsa, rol atamasının biçimi farklıdır. Rolün atanacağı kaynağın kaynak sağlayıcısı ad alanını ve kaynak türünü sağlarsınız. Ayrıca, rol atamasının adına kaynağın adını da dahil edersiniz.

Rol atamasının türü ve adı için aşağıdaki biçimi kullanın:

```json
"type": "{resource-provider-namespace}/{resource-type}/providers/roleAssignments",
"name": "{resource-name}/Microsoft.Authorization/{role-assign-GUID}"
```

Aşağıdaki şablon bir depolama hesabı dağıtır ve buna bir rol atar. Kaynak grubu komutlarıyla dağıtırsınız.

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
      "name": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleNameGuid'))]",
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

Aşağıda, şablonu dağıttıktan sonra bir depolama hesabı için kullanıcıya katkıda bulunan rol atamasının bir örneği gösterilmektedir.

![Şablon kullanarak rol atama](./media/role-assignments-template/role-assignment-template-resource.png)

## <a name="deploy-template-using-azure-powershell"></a>Azure PowerShell kullanarak şablon dağıtma

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Önceki şablonu, Azure PowerShell kullanarak bir kaynak grubuna veya aboneliğe dağıtmak için aşağıdaki adımları izleyin.

1. RBAC-RG. JSON adlı yeni bir dosya oluşturun ve önceki şablonu kopyalayın.

1. [Azure PowerShell](/powershell/azure/authenticate-azureps) oturumu açın.

1. Bir kullanıcının, grubun veya uygulamanın benzersiz tanımlayıcısını alın. Örneğin, Azure AD kullanıcılarını listelemek için [Get-AzADUser](/powershell/module/az.resources/get-azaduser) komutunu kullanabilirsiniz.

    ```azurepowershell
    $userid = (Get-AzADUser -DisplayName "{name}").id
    ```

1. Şablon, rol atamasını tanımlamak için kullanılan GUID için varsayılan bir değer oluşturur. Belirli bir GUID belirtmeniz gerekiyorsa, bu değeri roleNameGuid parametresi için geçirin. Tanımlayıcının biçimi vardır:`11111111-1111-1111-1111-111111111111`

Rolü bir kaynak veya kaynak grubu düzeyinde atamak için aşağıdaki adımları izleyin:

1. Örnek bir kaynak grubu oluşturun.

    ```azurepowershell
    New-AzResourceGroup -Name ExampleGroup -Location "Central US"
    ```

1. Dağıtımı başlatmak için [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) komutunu kullanın.

    ```azurepowershell
    New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
    ```

    Aşağıda çıktının bir örneği gösterilmektedir.

    ```Output
    PS /home/user> New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
    
    DeploymentName          : rbac-rg
    ResourceGroupName       : ExampleGroup
    ProvisioningState       : Succeeded
    Timestamp               : 7/17/2018 7:46:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              principalId      String                     22222222-2222-2222-2222-222222222222
                              builtInRoleType  String                     Reader
                              roleNameGuid     String                     11111111-1111-1111-1111-111111111111
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

Rolü bir abonelik düzeyinde atamak için, [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) komutunu kullanın ve dağıtım için bir konum belirtin.

```azurepowershell
New-AzDeployment -Location centralus -TemplateFile rbac-rg.json -principalId $userid -builtInRoleType Reader
```

Kaynak grupları için dağıtım komutuna benzer bir çıkışı vardır.

## <a name="deploy-template-using-the-azure-cli"></a>Azure CLı kullanarak şablon dağıtma

Azure CLı kullanarak önceki şablonu bir kaynak grubuna veya aboneliğe dağıtmak için aşağıdaki adımları izleyin.

1. RBAC-RG. JSON adlı yeni bir dosya oluşturun ve önceki şablonu kopyalayın.

1. [Azure CLI](/cli/azure/authenticate-azure-cli)'da oturum açın.

1. Bir kullanıcının, grubun veya uygulamanın benzersiz tanımlayıcısını alın. Örneğin, [az ad User Show](/cli/azure/ad/user#az-ad-user-show) komutunu kullanarak BIR Azure AD kullanıcısını gösterebilirsiniz.

    ```azurecli
    userid=$(az ad user show --upn-or-object-id "{email}" --query objectId --output tsv)
    ```

1. Şablon, rol atamasını tanımlamak için kullanılan GUID için varsayılan bir değer oluşturur. Belirli bir GUID belirtmeniz gerekiyorsa, bu değeri roleNameGuid parametresi için geçirin. Tanımlayıcının biçimi vardır:`11111111-1111-1111-1111-111111111111`

Rolü bir kaynak veya kaynak grubu düzeyinde atamak için aşağıdaki adımları izleyin:

1. Örnek bir kaynak grubu oluşturun.

    ```azurecli
    az group create --name ExampleGroup --location "Central US"
    ```

1. Dağıtımı başlatmak için [az Group Deployment Create](/cli/azure/group/deployment#az-group-deployment-create) komutunu kullanın.

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
    ```

    Aşağıda çıktının bir örneği gösterilmektedir.

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
    
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/rbac-rg",
      "name": "rbac-rg",
      "properties": {
        "additionalProperties": {
          "duration": "PT9.5323924S",
          "outputResources": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
              "resourceGroup": "ExampleGroup"
            }
          ],
          "templateHash": "0000000000000000000"
        },
        "correlationId": "33333333-3333-3333-3333-333333333333",
        "debugSetting": null,
        "dependencies": [],
        "mode": "Incremental",
        "outputs": null,
        "parameters": {
          "builtInRoleType": {
            "type": "String",
            "value": "Reader"
          },
          "principalId": {
            "type": "String",
            "value": "22222222-2222-2222-2222-222222222222"
          },
          "roleNameGuid": {
            "type": "String",
            "value": "11111111-1111-1111-1111-111111111111"
          }
        },
        "parametersLink": null,
        "providers": [
          {
            "id": null,
            "namespace": "Microsoft.Authorization",
            "registrationState": null,
            "resourceTypes": [
              {
                "aliases": null,
                "apiVersions": null,
                "locations": [
                  null
                ],
                "properties": null,
                "resourceType": "roleAssignments"
              }
            ]
          }
        ],
        "provisioningState": "Succeeded",
        "template": null,
        "templateLink": null,
        "timestamp": "2018-07-17T19:00:31.830904+00:00"
      },
      "resourceGroup": "ExampleGroup"
    }
    ```

Rolü bir abonelik düzeyinde atamak için [az Deployment Create](/cli/azure/deployment#az-deployment-create) komutunu kullanın ve dağıtım için bir konum belirtin.

```azurecli
az deployment create --location centralus --template-file rbac-rg.json --parameters principalId=$userid builtInRoleType=Reader
```

Kaynak grupları için dağıtım komutuna benzer bir çıkışı vardır.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı Başlangıç: Azure portal kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)
- [Azure Resource Manager şablonlarının yapısını ve söz dizimini anlama](../azure-resource-manager/resource-group-authoring-templates.md)
- [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/?term=rbac)
