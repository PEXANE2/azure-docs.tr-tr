---
title: Azure CLI kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme | Microsoft Dokümanlar
description: Azure CLI'yi kullanarak Azure kaynakları için rol tabanlı erişim denetimi (RBAC) ile özel rolleri nasıl listeleyiştir, oluşturabilirsiniz, güncelleştirin veya silebilirsiniz.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062223"
---
# <a name="create-or-update-custom-roles-for-azure-resources-using-azure-cli"></a>Azure CLI'yi kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme

> [!IMPORTANT]
> Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Azure [kaynakları için yerleşik roller](built-in-roles.md) kuruluşunuzun özel gereksinimlerini karşılamazsa, kendi özel rollerinizi oluşturabilirsiniz. Bu makalede, Azure CLI kullanarak özel rollerin nasıl listelenebildiğini, oluşturularak, güncelleştirilenveya silininin açıklanmaktadır.

Özel bir rolün nasıl oluşturulaca biliyor olun adım adım öğretici için [Bkz.](tutorial-custom-role-cli.md)

## <a name="prerequisites"></a>Ön koşullar

Özel roller oluşturmak için şunları yapmanız gerekir:

- [Sahip](built-in-roles.md#owner) veya [Kullanıcı Erişimi Yöneticisi](built-in-roles.md#user-access-administrator) gibi özel rol oluşturma izni
- [Azure Bulut Kabuğu](../cloud-shell/overview.md) veya [Azure CLI](/cli/azure/install-azure-cli)

## <a name="list-custom-roles"></a>Özel rolleri listeleme

Atama için kullanılabilen özel rolleri listelemek için [az rol tanım listesini](/cli/azure/role/definition#az-role-definition-list)kullanın. Aşağıdaki örnekler, geçerli abonelikteki tüm özel rolleri listeleyir.

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

## <a name="list-a-custom-role-definition"></a>Özel rol tanımını listelama

Özel bir rol tanımı nı listelemek için [az rol tanımı listesini](/cli/azure/role/definition#az-role-definition-list)kullanın. Bu, yerleşik bir rol için kullanacağınız komutun aynısI.

```azurecli
az role definition list --name <role_name>
```

Aşağıdaki örnekte *Sanal Makine Operatörü* rol tanımı listeleilmektedir:

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

Aşağıdaki örnekte, *Sanal Makine Operatörü* rolünün yalnızca eylemleri listelenebedilir:

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

Özel bir rol oluşturmak için [az rol tanımı nı](/cli/azure/role/definition#az-role-definition-create)kullanın. Rol tanımı JSON açıklaması veya JSON açıklaması içeren bir dosyaya giden bir yol olabilir.

```azurecli
az role definition create --role-definition <role_definition>
```

Aşağıdaki örnek, Sanal Makine *Operatörü*adlı özel bir rol oluşturur. Bu özel rol, *Microsoft.Compute,* *Microsoft.Storage*ve *Microsoft.Network* kaynak sağlayıcılarının tüm okuma işlemlerine erişim atar ve sanal makineleri başlatmak, yeniden başlatmak ve izlemek için erişim atar. Bu özel rol iki abonelikte kullanılabilir. Bu örnek, giriş olarak bir JSON dosyakullanır.

vmoperator.json

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

Özel bir rolü güncelleştirmek için, önce rol tanımını almak için [az rol tanımı listesini](/cli/azure/role/definition#az-role-definition-list) kullanın. İkinci olarak, rol tanımında istenen değişiklikleri yapın. Son olarak, güncelleştirilmiş rol tanımını kaydetmek için [az rol tanımı güncelleştirmesini](/cli/azure/role/definition#az-role-definition-update) kullanın.

```azurecli
az role definition update --role-definition <role_definition>
```

Aşağıdaki örnek, *Microsoft.Insights/diagnosticSettings/işlemini* *Sanal Makine* Operatörü `AssignableScopes` özel rolüne `Actions` bir yönetim grubu ekler ve ekler. Bir yönetim grubu `AssignableScopes` eklemek şu anda önizlemededir.

vmoperator.json

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

Özel bir rolü silmek için [az rol tanımı silme'yi](/cli/azure/role/definition#az-role-definition-delete)kullanın. Silmek için rolü belirtmek için rol adını veya rol kimliğini kullanın. Rol kimliğini belirlemek için [az rol tanım listesini](/cli/azure/role/definition#az-role-definition-list)kullanın.

```azurecli
az role definition delete --name <role_name or role_id>
```

Aşağıdaki örnek, *Sanal Makine Operatörü* özel rolünü siler.

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure CLI'yi kullanarak Azure kaynakları için özel bir rol oluşturma](tutorial-custom-role-cli.md)
- [Azure kaynakları için özel roller](custom-roles.md)
- [Azure Kaynak Yöneticisi kaynak sağlayıcısı işlemleri](resource-provider-operations.md)