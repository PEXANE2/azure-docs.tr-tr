---
title: REST API-Azure RBAC kullanarak Azure reddetme atamalarını listeleme
description: REST API ve Azure rol tabanlı erişim denetimi (Azure RBAC) kullanarak kullanıcılar, gruplar ve uygulamalar için Azure reddetme atamalarını nasıl listeleyeceğinizi öğrenin.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: dae0352566e6cb4f8ed1384ca12213e2aaa07f9d
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82733879"
---
# <a name="list-azure-deny-assignments-using-the-rest-api"></a>REST API kullanarak Azure reddetme atamalarını listeleyin

[Azure reddetme atamaları](deny-assignments.md) , bir rol ataması erişim izni veriyorsa bile kullanıcıların belirli Azure Kaynak eylemlerini gerçekleştirmesini engeller. Bu makalede, REST API kullanarak reddedilen atamaları listeleme açıklanmaktadır.

> [!NOTE]
> Kendi reddetme atamalarınızı doğrudan oluşturamazsınız. Atamaları reddetme hakkında daha fazla bilgi için bkz. [Azure atamaları reddetme](deny-assignments.md).

## <a name="prerequisites"></a>Ön koşullar

Reddetme atama hakkında bilgi almak için, şunları yapmanız gerekir:

- `Microsoft.Authorization/denyAssignments/read`[Azure yerleşik rollerinin](built-in-roles.md)çoğunda bulunan izin.

## <a name="list-a-single-deny-assignment"></a>Tek bir reddetme ataması listeleyin

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. URI içinde, *{scope}* değerini reddetme atamalarını listelemek istediğiniz kapsamla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId}` | Abonelik |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Deny-atama-kimliği}* değerini almak istediğiniz reddetme atama tanımlayıcısı ile değiştirin.

## <a name="list-multiple-deny-assignments"></a>Birden çok reddetme atamasını listeleme

1. Aşağıdaki isteklerden biriyle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    İsteğe bağlı parametrelerle:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. URI içinde, *{scope}* değerini reddetme atamalarını listelemek istediğiniz kapsamla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId}` | Abonelik |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Filter}* öğesini reddetme atama listesini filtrelemek için uygulamak istediğiniz koşulla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | (filtre yok) | Belirtilen kapsamın üzerinde, üzerinde ve altında tüm reddetme atamalarını listeler. |
    > | `$filter=atScope()` | Yalnızca belirtilen kapsam ve yukarıya yönelik atamaları Reddet listeler. , Alt kapsamlardaki reddetme atamalarını içermez. |
    > | `$filter=assignedTo('{objectId}')` | Belirtilen kullanıcı veya hizmet sorumlusu için atamaları Reddet listeler.<br/>Kullanıcı reddetme ataması olan bir grubun üyesiyse, bu reddetme atamasını de listelenir. Bu filtre, Kullanıcı bir grubun üyesiyse ve bu grubun reddetme ataması olan başka bir grubun üyesi olduğu durumlarda, bu filtre, reddetme atamasını de listelenmiş olduğu anlamına gelen gruplar için geçişlidir.<br/>Bu filtre yalnızca bir kullanıcı veya hizmet sorumlusu için bir nesne KIMLIĞI kabul eder. Bir grup için nesne KIMLIĞI geçirilemez. |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | Belirtilen kullanıcı veya hizmet sorumlusu için ve belirtilen kapsamda atamaları Reddet ' i listeler. |
    > | `$filter=denyAssignmentName+eq+'{deny-assignment-name}'` | Belirtilen ada sahip atamaları Reddet listeler. |
    > | `$filter=principalId+eq+'{objectId}'` | Belirtilen kullanıcı, Grup veya hizmet sorumlusu için atamaları Reddet ' i listeler. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Kök kapsamda (/) atamaları Reddet listesi

1. [Tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimi yükseltme](elevate-access-global-admin.md)' de açıklandığı gibi erişiminizi yükseltin.

1. Aşağıdaki isteği kullanın:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. *{Filter}* öğesini reddetme atama listesini filtrelemek için uygulamak istediğiniz koşulla değiştirin. Filtre gerekiyor.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | `$filter=atScope()` | Yalnızca kök kapsamı için atamaları Reddet listesini listeleyin. , Alt kapsamlardaki reddetme atamalarını içermez. |
    > | `$filter=denyAssignmentName+eq+'{deny-assignment-name}'` | Belirtilen ada sahip atamaları Reddet listesini listeleyin. |

1. Yükseltilmiş erişimi kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure reddetme atamalarını anlama](deny-assignments.md)
- [Tüm Azure aboneliklerini ve yönetim gruplarını yönetmek için erişimi yükseltme](elevate-access-global-admin.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
