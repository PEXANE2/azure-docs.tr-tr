---
title: Azure portal, Azure PowerShell veya Azure CLı kullanarak Azure RBAC 'de rol tanımlarını listeleyin | Microsoft Docs
description: Azure portal, Azure PowerShell veya Azure CLı kullanarak Azure RBAC 'te yerleşik ve özel rolleri nasıl listeleyeceğinizi öğrenin.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 839393d7535de530a27752f77e311c87c75825d9
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74710353"
---
# <a name="list-role-definitions-in-azure-rbac"></a>Azure RBAC 'de rol tanımlarını listeleyin

Rol tanımı, okuma, yazma ve silme gibi gerçekleştirilebileceği izinlerin bir koleksiyonudur. Genellikle bir rol olarak adlandırılır. [Azure rol tabanlı erişim denetimi (RBAC)](overview.md) , 120 [yerleşik rolüne](built-in-roles.md) sahiptir veya kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, Azure kaynaklarına erişim izni vermek için kullanabileceğiniz yerleşik ve özel rollerin nasıl listeleneceğini açıklanmaktadır.

Azure Active Directory için Yönetici rollerinin listesini görmek için, [Azure Active Directory Içindeki yönetici rolü izinleri](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bölümüne bakın.

## <a name="azure-portal"></a>Azure portalı

### <a name="list-all-roles"></a>Tüm rolleri Listele

Azure portal tüm rolleri listelemek için aşağıdaki adımları izleyin.

1. Azure portal, **tüm hizmetler** ' e tıklayın ve ardından herhangi bir kapsamı seçin. Örneğin, **Yönetim grupları**, **abonelikler**, **kaynak grupları**veya bir kaynak seçebilirsiniz.

1. Belirli kaynağa tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Tüm yerleşik ve özel rollerin listesini görmek için **Roller** sekmesine tıklayın.

   Geçerli kapsamdaki her bir role atanan kullanıcı ve grup sayısını görebilirsiniz.

   ![Rolleri listesi](./media/role-definitions-list/roles-list.png)

## <a name="azure-powershell"></a>Azure PowerShell

### <a name="list-all-roles"></a>Tüm rolleri Listele

Azure PowerShell tüm rolleri listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition)komutunu kullanın.

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

### <a name="list-a-role-definition"></a>Rol tanımı listeleme

Belirli bir rolün ayrıntılarını listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition)komutunu kullanın.

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

### <a name="list-a-role-definition-in-json-format"></a>JSON biçiminde bir rol tanımı listeleme

JSON biçiminde bir rol listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition)komutunu kullanın.

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

### <a name="list-permissions-of-a-role-definition"></a>Rol tanımının listeleme izinleri

Belirli bir rolün izinlerini listelemek için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition)komutunu kullanın.

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

### <a name="list-all-roles"></a>Tüm rolleri Listele

Azure CLı 'deki tüm rolleri listelemek için [az role Definition List](/cli/azure/role/definition#az-role-definition-list)kullanın.

```azurecli
az role definition list
```

Aşağıdaki örnekte, tüm kullanılabilir rol tanımlarının adı ve açıklaması listelenmektedir:

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

Aşağıdaki örnek tüm yerleşik rolleri listeler.

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

### <a name="list-a-role-definition"></a>Rol tanımı listeleme

Rolün ayrıntılarını listelemek için [az role Definition List](/cli/azure/role/definition#az-role-definition-list)kullanın.

```azurecli
az role definition list --name <role_name>
```

Aşağıdaki örnek, *katkıda bulunan* rol tanımını listeler:

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

### <a name="list-permissions-of-a-role-definition"></a>Rol tanımının listeleme izinleri

Aşağıdaki örnek, *katkıda bulunan* rolünün yalnızca *eylemlerini* ve *notActions* listeler.

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

Aşağıdaki örnek yalnızca *sanal makine katılımcısı* rolünün eylemlerini listeler.

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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için yerleşik roller](built-in-roles.md)
- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure RBAC ve Azure portal kullanarak rol atamalarını listeleyin](role-assignments-list-portal.md)
- [Azure RBAC ve Azure portal kullanarak rol atamaları ekleme veya kaldırma](role-assignments-portal.md)
