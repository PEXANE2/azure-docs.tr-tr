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
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: a494e7fd4c9fb79faa6a1d8cb2c3c871796ccdc5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80062148"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-rest-api"></a>Azure RBAC ve REST API kullanarak rol atamalarını listeleyin

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)]Bu makalede, REST API kullanarak rol atamalarının nasıl listeleneceğini açıklanmaktadır.

> [!NOTE]
> Kuruluşunuzun, [Azure tarafından yetkilendirilen kaynak yönetimi](../lighthouse/concepts/azure-delegated-resource-management.md)kullanan bir hizmet sağlayıcısına dış kaynaklı yönetim işlevleri varsa, bu hizmet sağlayıcısı tarafından yetkilendirilen rol atamaları burada gösterilmez.

## <a name="list-role-assignments"></a>Rol atamalarını listeleme

RBAC 'de, erişimi listelemek için rol atamalarını listeleyin. Rol atamalarını listelemek için, [rol atamaları-liste](/rest/api/authorization/roleassignments/list) REST API 'lerinden birini kullanın. Sonuçlarınızı iyileştirmek için bir kapsam ve isteğe bağlı bir filtre belirtirsiniz.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. URI içinde, *{scope}* değerini rol atamalarını listelemek istediğiniz kapsamla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Kaynak |

    Önceki örnekte, Microsoft. Web bir App Service örneğine başvuran bir kaynak sağlayıcıdır. Benzer şekilde, diğer herhangi bir kaynak sağlayıcısını kullanabilir ve kapsamı belirtebilirsiniz. Daha fazla bilgi için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md) ve desteklenen [Azure Resource Manager kaynak sağlayıcısı işlemleri](resource-provider-operations.md).  
     
1. *{Filter}* değerini, rol atama listesini filtrelemek için uygulamak istediğiniz koşulla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | `$filter=atScope()` | Alt kapsamlardaki rol atamalarını dahil etmez, yalnızca belirtilen kapsam için rol atamalarını listeler. |
    > | `$filter=assignedTo('{objectId}')` | Belirtilen kullanıcı veya hizmet sorumlusu için rol atamalarını listeler.<br/>Kullanıcı, rol ataması olan bir grubun üyesiyse, bu rol ataması da listelenir. Bu filtre, Kullanıcı bir grubun üyesiyse ve bu grup rol ataması olan başka bir grubun üyesiyse, bu rol atamasının de listelenmediği anlamına gelen gruplar için geçişlidir.<br/>Bu filtre yalnızca bir kullanıcı veya hizmet sorumlusu için bir nesne KIMLIĞI kabul eder. Bir grup için nesne KIMLIĞI geçirilemez. |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | Belirtilen kullanıcı veya hizmet sorumlusu veya belirtilen kapsamda rol atamalarını listeler. |
    > | `$filter=principalId+eq+'{objectId}'` | Belirtilen kullanıcı, Grup veya hizmet sorumlusu için rol atamalarını listeler. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve REST API kullanarak rol atamaları ekleme veya kaldırma](role-assignments-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
