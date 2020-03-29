---
title: REST API ile Azure kaynakları için atamaları reddetme listesi
description: Azure kaynakları ve REST API için rol tabanlı erişim denetimi (RBAC) kullanarak kullanıcılar, gruplar ve uygulamalar için reddedilen atamaları nasıl listeleyeceğimiz öğrenin.
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
ms.openlocfilehash: 0f648405a3d71bf27c64dacbb3fd78f3e9801137
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063027"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>REST API'sini kullanarak Azure kaynakları için atamaları reddetme listesi

[Atamaları reddetme,](deny-assignments.md) bir rol ataması onlara erişim izni verse bile kullanıcıların belirli Azure kaynak eylemlerini gerçekleştirmesini engeller. Bu makalede, REST API kullanarak reddi atamaları nasıl listelenir.

> [!NOTE]
> Doğrudan kendi reddi atamaları oluşturamaz. Reddet atamalarının nasıl oluşturulduğu hakkında bilgi için [bkz.](deny-assignments.md)

## <a name="prerequisites"></a>Ön koşullar

Reddet ataması hakkında bilgi almak için şunları yapmış olmalısınız:

- `Microsoft.Authorization/denyAssignments/read`Azure kaynakları için en [yerleşik rollere](built-in-roles.md)dahil olan izin.

## <a name="list-a-single-deny-assignment"></a>Tek bir reddet atamayı listele

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. URI *içinde{ scope}* yerine, reddet atamalarını listelemek istediğiniz kapsamla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId}` | Abonelik |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{reddet atama-id}* ile almak istediğiniz reddet atama tanımlayıcısını değiştirin.

## <a name="list-multiple-deny-assignments"></a>Birden çok reddet atamalarını listele

1. Aşağıdaki isteklerden biriyle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    İsteğe bağlı parametrelerle:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. URI *içinde{ scope}* yerine, reddet atamalarını listelemek istediğiniz kapsamla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Kapsam | Tür |
    > | --- | --- |
    > | `subscriptions/{subscriptionId}` | Abonelik |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    > | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{filter}* atama listesini filtrelemek için uygulamak istediğiniz koşulla değiştirin.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | (filtre yok) | Belirtilen kapsamdaki, yukarıda ve altındaki atamaları reddeden tüm leri listeler. |
    > | `$filter=atScope()` | Listeler yalnızca belirtilen kapsam ve üzeri için atamaları reddeder. Alt skoplarda reddet atamalarını içermez. |
    > | `$filter=assignedTo('{objectId}')` | Listeler, belirtilen kullanıcı veya hizmet sorumlusunun atamalarını reddeder.<br/>Kullanıcı, reddet ataması olan bir grubun üyesiyse, atamayı reddeden de listelenir. Bu filtre gruplar için geçişli dir, bu da kullanıcı bir grubun üyesiyse ve bu grup reddet ataması olan başka bir grubun üyesiyse, atamayı reddeden inkâr da listelenir.<br/>Bu filtre yalnızca bir kullanıcı veya hizmet sorumlusu için bir nesne kimliği kabul eder. Bir grup için nesne kimliği geçemezsiniz. |
    > | `$filter=atScope()+and+assignedTo('{objectId}')` | Listeler, belirtilen kullanıcı veya hizmet sorumlusuiçin ve belirtilen kapsamda atamaları reddeder. |
    > | `$filter=denyAssignmentName+eq+'{deny-assignment-name}'` | Listeler belirtilen ada sahip atamaları reddeder. |
    > | `$filter=principalId+eq+'{objectId}'` | Listeler belirtilen kullanıcı, grup veya hizmet sorumlusuiçin atamaları reddeder. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Temel kapsamdaki atamaları listele (/)

1. [Azure Etkin Dizini'nde Bir Global Administrator için Erişimi Yükselt'te](elevate-access-global-admin.md)açıklandığı gibi erişiminizi yükseltin.

1. Aşağıdaki isteği kullanın:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. *{filter}* atama listesini filtrelemek için uygulamak istediğiniz koşulla değiştirin. Bir filtre gereklidir.

    > [!div class="mx-tableFixed"]
    > | Filtre | Açıklama |
    > | --- | --- |
    > | `$filter=atScope()` | Yalnızca kök kapsamı için atamaları reddetmeyi listele. Alt skoplarda reddet atamalarını içermez. |
    > | `$filter=denyAssignmentName+eq+'{deny-assignment-name}'` | Belirtilen ada sahip atamaları listele. |

1. Yükseltilmiş erişimi kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için atamaları reddetmeyi anlama](deny-assignments.md)
- [Azure Active Directory'de Genel Yönetici erişimini yükseltme](elevate-access-global-admin.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
