---
title: Azure CLı kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme | Microsoft Docs
description: Azure CLı kullanarak Azure kaynakları için rol tabanlı erişim denetimi (RBAC) ile özel rolleri listeleme, oluşturma, güncelleştirme veya silme hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/18/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 44676f7b92c2bcd30612295840054ab2f0c0cf12
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80062223"
---
# <a name="create-or-update-custom-roles-for-azure-resources-using-azure-cli"></a>Azure CLı kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme

> [!IMPORTANT]
> ' Ye `AssignableScopes` bir yönetim grubu eklemek Şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Azure kaynaklarına yönelik yerleşik roller](built-in-roles.md) , kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, Azure CLı kullanılarak özel rolleri listeleme, oluşturma, güncelleştirme veya silme işlemlerinin nasıl yapılacağı açıklanır.

Özel rol oluşturma hakkında adım adım bir öğretici için bkz. [öğretici: Azure CLI kullanarak Azure kaynakları için özel rol oluşturma](tutorial-custom-role-cli.md).

## <a name="prerequisites"></a>Ön koşullar

Özel Roller oluşturmak için şunlar gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- [Azure Cloud Shell](../cloud-shell/overview.md) veya [Azure CLI](/cli/azure/install-azure-cli)

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Atama için kullanılabilen özel rolleri listelemek için [az role Definition List](/cli/azure/role/definition#az-role-definition-list)kullanın. Aşağıdaki örneklerde, geçerli abonelikteki tüm özel roller listelenmektedir.

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.roleName, "roleType":.roleType}'
```

```azurecli
az role definition list --output json | jq '.[] | if .roleType == "CustomRole" then {"roleName":.roleName, "roleType":.roleType} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}
{
  "roleName": "Virtual Machine Operator",
  "type": "CustomRole"
}

...
```

## <a name="list-a-custom-role-definition"></a>Özel bir rol tanımı listeleme

Özel bir rol tanımı listelemek için [az role Definition List](/cli/azure/role/definition#az-role-definition-list)kullanın. Bu, yerleşik bir rol için kullandığınız komuttur.

```azurecli
az role definition list --name <role_name>
```

Aşağıdaki örnek, *sanal makine operatörü* rol tanımını listeler:

```azurecli
az role definition list --name "Virtual Machine Operator"
```

```Output
[
  {
    "assignableScopes": [
      "/subscriptions/11111111-1111-1111-1111-111111111111"
    ],
    "description": "Can monitor and restart virtual machines.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/00000000-0000-0000-0000-000000000000",
    "name": "00000000-0000-0000-0000-000000000000",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/*/read",
          "Microsoft.Network/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action",
          "Microsoft.Authorization/*/read",
          "Microsoft.ResourceHealth/availabilityStatuses/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Insights/diagnosticSettings/*",
          "Microsoft.Support/*"
        ],
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Virtual Machine Operator",
    "roleType": "CustomRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Aşağıdaki örnek yalnızca *sanal makine operatörü* rolünün eylemlerini listeler:

```azurecli
az role definition list --name "Virtual Machine Operator" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Storage/*/read",
  "Microsoft.Network/*/read",
  "Microsoft.Compute/*/read",
  "Microsoft.Compute/virtualMachines/start/action",
  "Microsoft.Compute/virtualMachines/restart/action",
  "Microsoft.Authorization/*/read",
  "Microsoft.ResourceHealth/availabilityStatuses/read",
  "Microsoft.Resources/subscriptions/resourceGroups/read",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Insights/diagnosticSettings/*",
  "Microsoft.Support/*"
]
```

## <a name="create-a-custom-role"></a>Özel rol oluşturma

Özel bir rol oluşturmak için [az role Definition Create](/cli/azure/role/definition#az-role-definition-create)kullanın. Rol tanımı bir JSON açıklaması veya JSON açıklaması içeren bir dosyanın yolu olabilir.

```azurecli
az role definition create --role-definition <role_definition>
```

Aşağıdaki örnek, *sanal makine işleci*adlı özel bir rol oluşturur. Bu özel rol, *Microsoft. COMPUTE*, *Microsoft. Storage*ve *Microsoft. Network* kaynak sağlayıcılarının tüm okuma işlemlerine erişim atar ve sanal makinelere başlatma, yeniden başlatma ve izleme erişimi atar. Bu özel rol iki abonelik için kullanılabilir. Bu örnek, bir JSON dosyasını girdi olarak kullanır.

vmoperator. JSON

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition create --role-definition ~/roles/vmoperator.json
```

## <a name="update-a-custom-role"></a>Özel rolü güncelleştirme

Özel bir rolü güncelleştirmek için öncelikle rol tanımını almak üzere [az role Definition List](/cli/azure/role/definition#az-role-definition-list) öğesini kullanın. İkinci olarak, rol tanımında istenen değişiklikleri yapın. Son olarak, güncelleştirilmiş rol tanımını kaydetmek için [az role Definition Update](/cli/azure/role/definition#az-role-definition-update) kullanın.

```azurecli
az role definition update --role-definition <role_definition>
```

Aşağıdaki örnek, `Actions` ' a *Microsoft. Insights/diagnosticsettings/* Operation öğesini ekler ve *sanal makine operatörü* özel `AssignableScopes` rolü için öğesine bir yönetim grubu ekler. ' Ye `AssignableScopes` bir yönetim grubu eklemek Şu anda önizlemededir.

vmoperator. JSON

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.ResourceHealth/availabilityStatuses/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333",
    "/providers/Microsoft.Management/managementGroups/marketing-group"
  ]
}
```

```azurecli
az role definition update --role-definition ~/roles/vmoperator.json
```

## <a name="delete-a-custom-role"></a>Özel rolü silme

Özel bir rolü silmek için [az role Definition Delete](/cli/azure/role/definition#az-role-definition-delete)kullanın. Silinecek rolü belirtmek için rol adını veya rol KIMLIĞINI kullanın. Rol KIMLIĞINI öğrenmek için [az role Definition List](/cli/azure/role/definition#az-role-definition-list)kullanın.

```azurecli
az role definition delete --name <role_name or role_id>
```

Aşağıdaki örnek, *sanal makine operatörü* özel rolünü siler.

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure CLı kullanarak Azure kaynakları için özel bir rol oluşturma](tutorial-custom-role-cli.md)
- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md)