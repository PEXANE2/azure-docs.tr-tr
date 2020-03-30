---
title: RBAC ve REST API ile rol atamaları ekleme veya kaldırma
description: Azure rol tabanlı erişim denetimi (RBAC) ve REST API'yi kullanarak kullanıcılar, gruplar, hizmet müdürleri veya yönetilen kimlikler için Azure kaynaklarına nasıl erişim tanıyın öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063004"
---
# <a name="add-or-remove-role-assignments-using-azure-rbac-and-the-rest-api"></a>Azure RBAC ve REST API'sini kullanarak rol atamaları ekleme veya kaldırma

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)]Bu makalede, REST API kullanarak rollerin nasıl atanacakları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Rol atamaları eklemek veya kaldırmak için şunları yapmış olmalısınız:

- `Microsoft.Authorization/roleAssignments/write`ve `Microsoft.Authorization/roleAssignments/delete` Kullanıcı Erişim [Yöneticisi](built-in-roles.md#user-access-administrator) veya [Sahibi](built-in-roles.md#owner) gibi izinler

## <a name="add-a-role-assignment"></a>Rol ataması ekleyin

RBAC'da, erişim sağlamak için bir rol ataması eklersiniz. Rol ataması eklemek için Rol Atamaları - REST [API'sini oluşturun](/rest/api/authorization/roleassignments/create) ve güvenlik ilkesini, rol tanımını ve kapsamını belirtin. Bu API'yi aramak için, `Microsoft.Authorization/roleAssignments/write` operasyona erişiminiz olması gerekir. Yerleşik rollerden yalnızca [Sahibi](built-in-roles.md#owner) ve [Kullanıcı Erişim Yöneticisi'ne](built-in-roles.md#user-access-administrator) bu işlem için erişim izni verilir.

1. Rol [Tanımlarını](/rest/api/authorization/roledefinitions/list) kullanın - REST API'sini listele veya atamak istediğiniz rol tanımının tanımlayıcısını almak için [Yerleşik rolleri](built-in-roles.md) görün.

1. Rol atama tanımlayıcısı için kullanılacak benzersiz bir tanımlayıcı oluşturmak için bir GUID aracı kullanın. Tanımlayıcının biçimi vardır:`00000000-0000-0000-0000-000000000000`

1. Aşağıdaki istek ve gövde ile başlayın:

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

1. URI *içinde{scope}* rolünün kapsamıyla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Kaynak |

    Önceki örnekte, microsoft.web bir Uygulama Hizmeti örneğine başvuran bir kaynak sağlayıcısıdır. Benzer şekilde, diğer kaynak sağlayıcılarını kullanabilir ve kapsamı belirtebilirsiniz. Daha fazla bilgi için Azure [Kaynak sağlayıcıları ve türleri ve](../azure-resource-manager/management/resource-providers-and-types.md) desteklenen Azure Kaynak Yöneticisi kaynak sağlayıcısı [işlemlerine](resource-provider-operations.md)bakın.  

1. *{roleAssignmentName}* ile rol atamasının GUID tanımlayıcısını değiştirin.

1. İstek gövdesi *içinde,{scope}* rolünün kapsamıyla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Kaynak |

1. *{roleDefinitionId}* ile rol tanımı tanımlayıcısını değiştirin.

1. *{principalId}* rolünün atanacak kullanıcı, grup veya hizmet sorumlusunun nesne tanımlayıcısıyla değiştirin.

## <a name="remove-a-role-assignment"></a>Rol atamasını kaldırma

RBAC'de erişimi kaldırmak için rol atamasını kaldırmanız gerekir. Rol atamasını kaldırmak için [Rol Atamaları - DELETE](/rest/api/authorization/roleassignments/delete) REST API'yi kullanın. Bu API'yi aramak için, `Microsoft.Authorization/roleAssignments/delete` operasyona erişiminiz olması gerekir. Yerleşik rollerden yalnızca [Sahibi](built-in-roles.md#owner) ve [Kullanıcı Erişim Yöneticisi'ne](built-in-roles.md#user-access-administrator) bu işlem için erişim izni verilir.

1. Rol atama tanımlayıcısını (GUID) alın. Bu tanımlayıcı, rol atamasını ilk oluşturduğunuzda veya rol atamalarını listeleyerek elde edebilirsiniz.

1. Aşağıdaki istekle başlayın:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. URI içinde, rol atamasını kaldırmak için *{kapsam} yerine {kapsam}* değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/microsoft.web/sites/mysite1` | Kaynak |

1. *{roleAssignmentName}* ile rol atamasının GUID tanımlayıcısını değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve REST API'sini kullanarak rol atamalarını listele](role-assignments-list-rest.md)
- [Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma](../azure-resource-manager/templates/deploy-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
- [REST API'sini kullanarak Azure kaynakları için özel roller oluşturma](custom-roles-rest.md)
