---
title: RBAC ve REST API rol atamaları ekleme veya kaldırma
description: Azure rol tabanlı erişim denetimi (RBAC) ve REST API kullanarak kullanıcılar, gruplar, hizmet sorumluları veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim sağlayacağınızı öğrenin.
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
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 9beda6589c03f1b14fc9756af86a9ce0711894c0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80063004"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-the-rest-api"></a>Azure RBAC ve REST API kullanarak rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, REST API kullanarak rollerin nasıl atanacağı açıklanır.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmanız gerekir:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` [Kullanıcı erişimi Yöneticisi](built-in-roles.md#user-access-administrator) veya [sahibi](built-in-roles.md#owner) gibi izinler

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

RBAC 'de, erişim izni vermek için bir rol ataması eklersiniz. Rol ataması eklemek için, [rol atamalarını kullanın-REST API oluşturun](/rest/api/authorization/roleassignments/create) ve güvenlik sorumlusunu, rol tanımını ve kapsamı belirtin. Bu API 'yi çağırmak için `Microsoft.Authorization/roleAssignments/write` işleme erişiminizin olması gerekir. Yerleşik rollerde, yalnızca [sahip](built-in-roles.md#owner) ve [Kullanıcı erişim yöneticisine](built-in-roles.md#user-access-administrator) bu işleme erişim izni verilir.

1. Atamak istediğiniz rol tanımının tanımlayıcısını almak için [rol tanımları-liste](/rest/api/authorization/roledefinitions/list) REST API kullanın veya [yerleşik roller](built-in-roles.md) ' e bakın.

1. Rol atama tanımlayıcısı için kullanılacak benzersiz bir tanımlayıcı oluşturmak için bir GUID aracı kullanın. Tanımlayıcının biçimi vardır:`00000000-0000-0000-0000-000000000000`

1. Aşağıdaki istek ve gövdeyi başlatın:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
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

1. *{Roleatamaadı}* değerini rol atamasının GUID tanımlayıcısı ile değiştirin.

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

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir. Rol atamasını kaldırmak için, [atamaları-sil REST API rol atamalarını](/rest/api/authorization/roleassignments/delete) kullanın. Bu API 'yi çağırmak için `Microsoft.Authorization/roleAssignments/delete` işleme erişiminizin olması gerekir. Yerleşik rollerde, yalnızca [sahip](built-in-roles.md#owner) ve [Kullanıcı erişim yöneticisine](built-in-roles.md#user-access-administrator) bu işleme erişim izni verilir.

1. Rol atama tanımlayıcısını (GUID) alın. Bu tanımlayıcı, rol atamasını ilk oluşturduğunuzda döndürülür veya rol atamalarını listeleyerek alabilir.

1. Aşağıdaki istekle başlayın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. URI içinde, *{scope}* değerini rol atamasını kaldırma kapsamı ile değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Kaynak |

1. *{Roleatamaadı}* değerini rol atamasının GUID tanımlayıcısı ile değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve REST API kullanarak rol atamalarını listeleyin](role-assignments-list-rest.md)
- [Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma](../azure-resource-manager/templates/deploy-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
- [REST API kullanarak Azure kaynakları için özel roller oluşturma](custom-roles-rest.md)
