---
title: Azure RBAC ve REST API'sini kullanarak rol atamalarını listele
description: Kullanıcıların, grupların, hizmet ilkelerinin veya yönetilen kimliklerin Azure rol tabanlı erişim denetimi (RBAC) ve REST API'yi kullanarak hangi kaynaklara erişebildiğini nasıl belirleyeceğimiz öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062148"
---
# <a name="list-role-assignments-using-azure-rbac-and-the-rest-api"></a>Azure RBAC ve REST API'sini kullanarak rol atamalarını listele

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)]Bu makalede, REST API'yi kullanarak rol atamalarının nasıl listeleniyi açıklanmaktadır.

> [!NOTE]
> Kuruluşunuz, [Azure temsilcili kaynak yönetimini](../lighthouse/concepts/azure-delegated-resource-management.md)kullanan bir hizmet sağlayıcısına dış kaynak yönetim işlevleri veriyorsa, bu hizmet sağlayıcısı tarafından yetkilendirilen rol atamaları burada gösterilmez.

## <a name="list-role-assignments"></a>Rol atamalarını listeleme

RBAC'da, erişimi listelemek için rol atamalarını listelersiniz. Rol atamalarını listelemek için [Rol Atamaları - Liste](/rest/api/authorization/roleassignments/list) REST API'lerinden birini kullanın. Sonuçlarınızı hassaslaştırmak için bir kapsam ve isteğe bağlı bir filtre belirtirsiniz.

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. URI *içinde,{scope}* rolünü atamalarını listelemek istediğiniz kapsamla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `providers/Microsoft.Management/managementGroups/{groupId1}` | Yönetim grubu |
    > | `subscriptions/{subscriptionId1}` | Abonelik |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Kaynak |

    Önceki örnekte, microsoft.web bir Uygulama Hizmeti örneğine başvuran bir kaynak sağlayıcısıdır. Benzer şekilde, diğer kaynak sağlayıcılarını kullanabilir ve kapsamı belirtebilirsiniz. Daha fazla bilgi için Azure [Kaynak sağlayıcıları ve türleri ve](../azure-resource-manager/management/resource-providers-and-types.md) desteklenen Azure Kaynak Yöneticisi kaynak sağlayıcısı [işlemlerine](resource-provider-operations.md)bakın.  
     
1. Rol atama listesini filtrelemek için uygulamak istediğiniz koşulla *{filter}* değiştirin.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | `$filter=atScope()` | Alt kapsamlarda rol atamaları hariç, yalnızca belirtilen kapsam için rol atamaları listeler. |
    > | `$filter=assignedTo('{objectId}')` | Belirli bir kullanıcı veya hizmet sorumlusu için rol atamalarını listeler.<br/>Kullanıcı rol ataması olan bir grubun üyesiyse, bu rol ataması da listelenir. Bu filtre gruplar için geçişli dir, bu da kullanıcı bir grubun üyesiyse ve bu grup rol ataması olan başka bir grubun üyesiyse, bu rol atamasının da listelendiği anlamına gelir.<br/>Bu filtre yalnızca bir kullanıcı veya hizmet sorumlusu için bir nesne kimliği kabul eder. Bir grup için nesne kimliği geçemezsiniz. |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | Belirtilen kullanıcı veya hizmet sorumlusu için ve belirtilen kapsamda rol atamalarını listeler. |
    > | `$filter=principalId+eq+'{objectId}'` | Belirli bir kullanıcı, grup veya hizmet sorumlusu için rol atamalarını listeler. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure RBAC ve REST API'sini kullanarak rol atamaları ekleme veya kaldırma](role-assignments-rest.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
