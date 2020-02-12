---
title: REST API ile Azure kaynakları için atamaları reddetme listesi
description: Azure kaynakları ve REST API için rol tabanlı erişim denetimi (RBAC) kullanarak kullanıcılara, gruplara ve uygulamalara yönelik reddetme atamalarını nasıl listeleyeceğinizi öğrenin.
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
ms.date: 06/10/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 9e6214b3cb2cdca2d80ebae43771b206e3396d8b
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137330"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-rest-api"></a>REST API kullanarak Azure kaynakları için atamaları reddetme listesi

Bir rol ataması erişime izin verirse, [atamaları Reddet](deny-assignments.md) , kullanıcıların belirli Azure Kaynak eylemlerini gerçekleştirmesini engeller. Bu makalede, REST API kullanarak reddedilen atamaları listeleme açıklanmaktadır.

> [!NOTE]
> Kendi reddetme atamalarınızı doğrudan oluşturamazsınız. Atamaları reddetme hakkında daha fazla bilgi için bkz. [atamaları reddetme](deny-assignments.md).

## <a name="prerequisites"></a>Önkoşullar

Reddetme atama hakkında bilgi almak için, şunları yapmanız gerekir:

- [Azure kaynakları için en yerleşik rollerde](built-in-roles.md)bulunan `Microsoft.Authorization/denyAssignments/read` izni.

## <a name="list-a-single-deny-assignment"></a>Tek bir reddetme ataması listeleyin

1. Aşağıdaki istekle başlayın:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. URI içinde, *{scope}* değerini reddetme atamalarını listelemek istediğiniz kapsamla değiştirin.

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

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

    | Kapsam | Tür |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Abonelik |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Kaynak grubu |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Kaynak |

1. *{Filter}* öğesini reddetme atama listesini filtrelemek için uygulamak istediğiniz koşulla değiştirin.

    | Filtre | Açıklama |
    | --- | --- |
    | (filtre yok) | Belirtilen kapsamın üzerinde, üzerinde ve altında tüm reddetme atamalarını listeleyin. |
    | `$filter=atScope()` | Yalnızca belirtilen kapsam ve yukarıya ait atamaları Reddet listesini listeleyin. , Alt kapsamlardaki reddetme atamalarını içermez. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Belirtilen ada sahip atamaları Reddet listesini listeleyin. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Kök kapsamda (/) atamaları Reddet listesi

1. [Azure Active Directory, genel yönetici için erişimi yükseltme](elevate-access-global-admin.md)bölümünde açıklandığı gibi erişiminizi yükseltin.

1. Aşağıdaki isteği kullanın:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. *{Filter}* öğesini reddetme atama listesini filtrelemek için uygulamak istediğiniz koşulla değiştirin. Filtre gerekiyor.

    | Filtre | Açıklama |
    | --- | --- |
    | `$filter=atScope()` | Yalnızca kök kapsamı için atamaları Reddet listesini listeleyin. , Alt kapsamlardaki reddetme atamalarını içermez. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Belirtilen ada sahip atamaları Reddet listesini listeleyin. |

1. Yükseltilmiş erişimi kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynakları için reddedilen atamaları anlama](deny-assignments.md)
- [Azure Active Directory'de Genel Yönetici erişimini yükseltme](elevate-access-global-admin.md)
- [Azure REST API Başvurusu](/rest/api/azure/)
