---
title: Azure RBAC ve REST API kullanarak rol atamalarını listeleyin
description: Kullanıcıların, grupların, hizmet sorumlularının veya yönetilen kimliklerin Azure rol tabanlı erişim denetimi (RBAC) ve REST API kullanarak hangi kaynakların erişimi olduğunu nasıl belirleyebileceğinizi öğrenin.
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
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: e20edcb5e2406c216711a2e0f696ef06e19fe21e
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74710405"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-rest-api"></a>Azure RBAC ve REST API kullanarak rol atamalarını listeleyin

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] Bu makalede, REST API kullanılarak rol atamalarının nasıl listeleneceğini açıklanmaktadır.

## <a name="list-role-assignments"></a>Rol atamalarını listeleme

RBAC 'de, erişimi listelemek için rol atamalarını listeleyin. Rol atamalarını listelemek için, [rol atamaları-liste](/rest/api/authorization/roleassignments/list) REST API 'lerinden birini kullanın. Sonuçlarınızı iyileştirmek için bir kapsam ve isteğe bağlı bir filtre belirtirsiniz.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde, *{scope}* değerini rol atamalarını listelemek istediğiniz kapsamla değiştirin.

    | Kapsam | Tür |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    | `subscriptions/{subscriptionId1}` | Abonelik |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

    Önceki örnekte, Microsoft. Web bir App Service örneğine başvuran bir kaynak sağlayıcıdır. Benzer şekilde, diğer herhangi bir kaynak sağlayıcısını kullanabilir ve kapsamı belirtebilirsiniz. Daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/resource-manager-supported-services.md) ve desteklenen [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md).  
     
1. *{Filter}* değerini, rol atama listesini filtrelemek için uygulamak istediğiniz koşulla değiştirin.

    | Filtrele | Açıklama |
    | --- | --- |
    | `$filter=atScope()` | Alt kapsamlardaki rol atamalarını dahil etmez, yalnızca belirtilen kapsam için rol atamalarını listeler. |
    | `$filter=principalId%20eq%20'{objectId}'` | Belirtilen kullanıcı, Grup veya hizmet sorumlusu için rol atamalarını listeler. |
    | `$filter=assignedTo('{objectId}')` | Belirtilen kullanıcı veya hizmet sorumlusu için rol atamalarını listeler. Kullanıcı, rol ataması olan bir grubun üyesiyse, bu rol ataması da listelenir. Bu filtre, Kullanıcı bir grubun üyesiyse ve bu grup rol ataması olan başka bir grubun üyesiyse, bu rol atamasının de listelenmediği anlamına gelen gruplar için geçişlidir. Bu filtre yalnızca bir kullanıcı veya hizmet sorumlusu için bir nesne KIMLIĞI kabul eder. Bir grup için nesne KIMLIĞI geçirilemez. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve REST API kullanarak rol atamaları ekleme veya kaldırma](role-assignments-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
