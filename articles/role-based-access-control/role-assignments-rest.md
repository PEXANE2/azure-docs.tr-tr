---
title: REST API-Azure RBAC kullanarak Azure rol atamaları ekleme veya kaldırma
description: REST API ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak kullanıcılar, gruplar, hizmet sorumluları veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim sağlayacağınızı öğrenin.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: how-to
ms.date: 05/06/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: d66b4c8e9f41f661cfc399f72a9ad97405a860fc
ms.sourcegitcommit: 6571e34e609785e82751f0b34f6237686470c1f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84790855"
---
# <a name="add-or-remove-azure-role-assignments-using-the-rest-api"></a>REST API kullanarak Azure rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, REST API kullanarak rollerin nasıl atanacağı açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmanız gerekir:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

Azure RBAC 'de, erişim izni vermek için bir rol ataması eklersiniz. Rol ataması eklemek için, [rol atamalarını kullanın-REST API oluşturun](/rest/api/authorization/roleassignments/create) ve güvenlik sorumlusunu, rol tanımını ve kapsamı belirtin. Bu API 'yi çağırmak için işleme erişiminizin olması gerekir `Microsoft.Authorization/roleAssignments/write` . Yerleşik rollerde, yalnızca [sahip](built-in-roles.md#owner) ve [Kullanıcı erişim yöneticisine](built-in-roles.md#user-access-administrator) bu işleme erişim izni verilir.

1. Atamak istediğiniz rol tanımının tanımlayıcısını almak için [rol tanımları-liste](/rest/api/authorization/roledefinitions/list) REST API kullanın veya [yerleşik roller](built-in-roles.md) ' e bakın.

1. Rol atama tanımlayıcısı için kullanılacak benzersiz bir tanımlayıcı oluşturmak için bir GUID aracı kullanın. Tanımlayıcının biçimi vardır:`00000000-0000-0000-0000-000000000000`

1. Aşağıdaki istek ve gövdeyi başlatın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```

1. URI içinde, *{scope}* yerine rol atamasının kapsamını koyun.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Kaynak |

    Önceki örnekte, Microsoft. Web bir App Service örneğine başvuran bir kaynak sağlayıcıdır. Benzer şekilde, diğer herhangi bir kaynak sağlayıcısını kullanabilir ve kapsamı belirtebilirsiniz. Daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md) ve desteklenen [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md).  

1. *{Roleatamamentıd}* değerini rol atamasının GUID tanımlayıcısı ile değiştirin.

1. İstek gövdesi içinde, *{scope}* değerini rol atamasının kapsamıyla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Kaynak |

1. *{Roledefinitionıd}* değerini rol tanımı tanımlayıcısı ile değiştirin.

1. *{PrincipalId}* öğesini, role atanacak olan Kullanıcı, Grup veya hizmet sorumlusunun nesne tanımlayıcısıyla değiştirin.

Aşağıdaki istek ve gövde, abonelik kapsamındaki bir kullanıcıya [yedekleme okuyucusu](built-in-roles.md#backup-reader) rolünü atar:

```http
PUT https://management.azure.com/subscriptions/{subscriptionId1}/providers/microsoft.authorization/roleassignments/{roleAssignmentId1}?api-version=2015-07-01
```

```json
{
  "properties": {
    "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
    "principalId": "{objectId1}"
  }
}
```

Aşağıda çıktının bir örneği gösterilmektedir:

```json
{
    "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
        "principalId": "{objectId1}",
        "scope": "/subscriptions/{subscriptionId1}",
        "createdOn": "2020-05-06T23:55:23.7679147Z",
        "updatedOn": "2020-05-06T23:55:23.7679147Z",
        "createdBy": null,
        "updatedBy": "{updatedByObjectId1}"
    },
    "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId1}",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "{roleAssignmentId1}"
}
```

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

Azure RBAC 'de, erişimi kaldırmak için bir rol atamasını kaldırırsınız. Rol atamasını kaldırmak için, [atamaları-sil REST API rol atamalarını](/rest/api/authorization/roleassignments/delete) kullanın. Bu API 'yi çağırmak için işleme erişiminizin olması gerekir `Microsoft.Authorization/roleAssignments/delete` . Yerleşik rollerde, yalnızca [sahip](built-in-roles.md#owner) ve [Kullanıcı erişim yöneticisine](built-in-roles.md#user-access-administrator) bu işleme erişim izni verilir.

1. Rol atama tanımlayıcısını (GUID) alın. Bu tanımlayıcı, rol atamasını ilk oluşturduğunuzda döndürülür veya rol atamalarını listeleyerek alabilir.

1. Aşağıdaki istekle başlayın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}?api-version=2015-07-01
    ```

1. URI içinde, *{scope}* değerini rol atamasını kaldırma kapsamı ile değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Kaynak |

1. *{Roleatamamentıd}* değerini rol atamasının GUID tanımlayıcısı ile değiştirin.

Aşağıdaki istek, abonelik kapsamında belirtilen rol atamasını kaldırır:

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId1}/providers/microsoft.authorization/roleassignments/{roleAssignmentId1}?api-version=2015-07-01
```

Aşağıda çıktının bir örneği gösterilmektedir:

```json
{
    "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
        "principalId": "{objectId1}",
        "scope": "/subscriptions/{subscriptionId1}",
        "createdOn": "2020-05-06T23:55:24.5379478Z",
        "updatedOn": "2020-05-06T23:55:24.5379478Z",
        "createdBy": "{createdByObjectId1}",
        "updatedBy": "{updatedByObjectId1}"
    },
    "id": "/subscriptions/{subscriptionId1}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId1}",
    "type": "Microsoft.Authorization/roleAssignments",
    "name": "{roleAssignmentId1}"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [REST API kullanarak Azure rol atamalarını listeleyin](role-assignments-list-rest.md)
- [Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma](../azure-resource-manager/templates/deploy-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
- [REST API kullanarak Azure özel rolleri oluşturun veya güncelleştirin](custom-roles-rest.md)
