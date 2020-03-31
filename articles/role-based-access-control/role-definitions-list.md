---
title: Azure portalı, Azure PowerShell, Azure CLI veya REST API'sını kullanarak Azure RBAC'daki rol tanımlarını listele | Microsoft Dokümanlar
description: Azure portalı, Azure PowerShell, Azure CLI veya REST API'yi kullanarak Azure RBAC'da yerleşik ve özel rolleri nasıl listeleyeceğimiz öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: aa888eedc81ceb3188f801e273c70722207bf512
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062997"
---
# <a name="list-role-definitions-in-azure-rbac"></a>Azure RBAC'da rol tanımlarını listele

Rol tanımı, okuma, yazma ve silme gibi gerçekleştirilebilecek izinler topluluğudur. Genelde buna rol denir. [Azure rol tabanlı erişim denetiminde (RBAC)](overview.md) 120'den fazla [yerleşik role](built-in-roles.md) sahiptir veya kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, Azure kaynaklarına erişim izni vermek için kullanabileceğiniz yerleşik ve özel rollerin nasıl listelenebildiği açıklanmaktadır.

Azure Etkin Dizini için yönetici rollerinin listesini görmek için [Azure Etkin Dizini'nde Yönetici rol izinlerine](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bakın.

## <a name="azure-portal"></a>Azure portalında

### <a name="list-all-roles"></a>Tüm rolleri listele

Azure portalındaki tüm rolleri listelemek için aşağıdaki adımları izleyin.

1. Azure portalında **Tüm hizmetler'i** tıklatın ve ardından herhangi bir kapsamı seçin. Örneğin, **Yönetim gruplarını,** **Abonelikleri,** **Kaynak gruplarını**veya bir kaynağı seçebilirsiniz.

1. Belirli kaynağı tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Tüm yerleşik ve özel rollerin listesini görmek için **Roller** sekmesini tıklatın.

   Geçerli kapsamda her role atanan kullanıcı ve grup sayısını görebilirsiniz.

   ![Rolleri listesi](./media/role-definitions-list/roles-list.png)

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="list-all-roles"></a>Tüm rolleri listele

Azure PowerShell'deki tüm rolleri listelemek için [Get-AzRoleDefinition'ı](/powershell/module/az.resources/get-azroledefinition)kullanın.

```azurepowershell
Get-AzRoleDefinition | FT Name, Description
```

```Example
AcrImageSigner                                    acr image signer
AcrQuarantineReader                               acr quarantine data reader
AcrQuarantineWriter                               acr quarantine data writer
API Management Service Contributor                Can manage service and the APIs
API Management Service Operator Role              Can manage service but not the APIs
API Management Service Reader Role                Read-only access to service and APIs
Application Insights Component Contributor        Can manage Application Insights components
Application Insights Snapshot Debugger            Gives user permission to use Application Insights Snapshot Debugge...
Automation Job Operator                           Create and Manage Jobs using Automation Runbooks.
Automation Operator                               Automation Operators are able to start, stop, suspend, and resume ...
...
```

### <a name="list-a-role-definition"></a>Rol tanımını listelama

Belirli bir rolün ayrıntılarını listelemek için [Get-AzRoleDefinition'ı](/powershell/module/az.resources/get-azroledefinition)kullanın.

```azurepowershell
Get-AzRoleDefinition <role_name>
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor"

Name             : Contributor
Id               : b24988ac-6180-42a0-ab88-20f7382dd24c
IsCustom         : False
Description      : Lets you manage everything except access to resources.
Actions          : {*}
NotActions       : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
                   Microsoft.Authorization/elevateAccess/Action}
DataActions      : {}
NotDataActions   : {}
AssignableScopes : {/}
```

### <a name="list-a-role-definition-in-json-format"></a>JSON biçiminde bir rol tanımı listele

JSON biçiminde bir rol listelemek için [Get-AzRoleDefinition'ı](/powershell/module/az.resources/get-azroledefinition)kullanın.

```azurepowershell
Get-AzRoleDefinition <role_name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor" | ConvertTo-Json

{
  "Name": "Contributor",
  "Id": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "IsCustom": false,
  "Description": "Lets you manage everything except access to resources.",
  "Actions": [
    "*"
  ],
  "NotActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action",
    "Microsoft.Blueprint/blueprintAssignments/write",
    "Microsoft.Blueprint/blueprintAssignments/delete"
  ],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/"
  ]
}
```

### <a name="list-permissions-of-a-role-definition"></a>Rol tanımının izinlerini listele

Belirli bir rolün izinlerini listelemek için [Get-AzRoleDefinition'ı](/powershell/module/az.resources/get-azroledefinition)kullanın.

```azurepowershell
Get-AzRoleDefinition <role_name> | FL Actions, NotActions
```

```Example
PS C:\> Get-AzRoleDefinition "Contributor" | FL Actions, NotActions

Actions    : {*}
NotActions : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
             Microsoft.Authorization/elevateAccess/Action,
             Microsoft.Blueprint/blueprintAssignments/write...}
```

```azurepowershell
(Get-AzRoleDefinition <role_name>).Actions
```

```Example
PS C:\> (Get-AzRoleDefinition "Virtual Machine Contributor").Actions

Microsoft.Authorization/*/read
Microsoft.Compute/availabilitySets/*
Microsoft.Compute/locations/*
Microsoft.Compute/virtualMachines/*
Microsoft.Compute/virtualMachineScaleSets/*
Microsoft.DevTestLab/schedules/*
Microsoft.Insights/alertRules/*
Microsoft.Network/applicationGateways/backendAddressPools/join/action
Microsoft.Network/loadBalancers/backendAddressPools/join/action
...
```

## <a name="azure-cli"></a>Azure CLI

### <a name="list-all-roles"></a>Tüm rolleri listele

Azure CLI'deki tüm rolleri listelemek için [az rol tanım listesini](/cli/azure/role/definition#az-role-definition-list)kullanın.

```azurecli
az role definition list
```

Aşağıdaki örnekte, kullanılabilir tüm rol tanımlarının adı ve açıklaması listelenmektedir:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

Aşağıdaki örnekte yerleşik rollerin tümü listelenir.

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

### <a name="list-a-role-definition"></a>Rol tanımını listelama

Bir rolün ayrıntılarını listelemek için [az rol tanım listesini](/cli/azure/role/definition#az-role-definition-list)kullanın.

```azurecli
az role definition list --name <role_name>
```

Aşağıdaki örnekte *Katılımcı* rol tanımı listelenebilmektedir:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

### <a name="list-permissions-of-a-role-definition"></a>Rol tanımının izinlerini listele

Aşağıdaki örnekte, Yalnızca Katılımcı rolün *eylemleri* ve *eylemleri* listelenebvardır. *Contributor*

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

Aşağıdaki örnekte, *Sanal Makine Katılımcısı* rolünün yalnızca eylemleri listeleneb.'dir.

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="rest-api"></a>REST API

### <a name="list-role-definitions"></a>Rol tanımlarını listeleme

Rol tanımlarını listelemek için [Rol Tanımları - Liste](/rest/api/authorization/roledefinitions/list) REST API'sını kullanın. Sonuçlarınızı hassaslaştırmak için bir kapsam ve isteğe bağlı bir filtre belirtirsiniz.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?$filter={$filter}&api-version=2015-07-01
    ```

1. URI *içinde{scope}* rolünün tanımlarını listelemek istediğiniz kapsamla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Kaynak |

    Önceki örnekte, microsoft.web bir Uygulama Hizmeti örneğine başvuran bir kaynak sağlayıcısıdır. Benzer şekilde, diğer kaynak sağlayıcılarını kullanabilir ve kapsamı belirtebilirsiniz. Daha fazla bilgi için Azure [Kaynak sağlayıcıları ve türleri ve](../azure-resource-manager/management/resource-providers-and-types.md) desteklenen Azure Kaynak Yöneticisi kaynak sağlayıcısı [işlemlerine](resource-provider-operations.md)bakın.  
     
1. Rol tanımı listesini filtrelemek için uygulamak istediğiniz koşulla *{filter}* değiştirin.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | `$filter=atScopeAndBelow()` | Belirtilen kapsam ve alt kapsamlar için rol tanımlarını listeler. |
    > | `$filter=type+eq+'{type}'` | Belirtilen türdeki rol tanımlarını listeler. Rol türü olabilir `CustomRole` `BuiltInRole`veya . |

### <a name="list-a-role-definition"></a>Rol tanımını listelama

Belirli bir rolün ayrıntılarını listelemek için [Rol Tanımlarını - Alın](/rest/api/authorization/roledefinitions/get) veya Rol Tanımlarını - Id REST API'ye göre [alın'](/rest/api/authorization/roledefinitions/getbyid) ı kullanın.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    Dizin düzeyinde bir rol tanımı için bu isteği kullanabilirsiniz:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. URI *içinde,{scope}* rolünü nisması listelemek istediğiniz kapsamla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Kaynak |
     
1. *{roleDefinitionId}* ile rol tanımı tanımlayıcısını değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için yerleşik roller](built-in-roles.md)
- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure RBAC ve Azure portalı kullanarak rol atamalarını listele](role-assignments-list-portal.md)
- [Azure RBAC ve Azure portalını kullanarak rol atamaları ekleme veya kaldırma](role-assignments-portal.md)
