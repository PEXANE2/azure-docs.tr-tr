---
title: Azure Olay Izgara güvenliği ve kimlik doğrulaması
description: Azure Event Grid ve kavramlarını açıklar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: babanisa
ms.openlocfilehash: 03bc2f9de6f50f08c9f62f86a3d1791a067cecd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78899288"
---
# <a name="authorizing-access-to-event-grid-resources"></a>Olay Izgara kaynaklarına erişim yetkisi
Azure Olay İzlemi, farklı kullanıcılara verilen erişim düzeyini kontrol etmenizi ve etkinlik aboneliklerini listele, yenilerini oluşturmave anahtar oluşturma gibi çeşitli yönetim işlemleri yapmanızı sağlar. Olay Ağı, Azure'un rol tabanlı erişim denetimini (RBAC) kullanır.

## <a name="operation-types"></a>İşlem türleri

Olay Grid aşağıdaki eylemleri destekler:

* Microsoft.EventGrid/*/oku
* Microsoft.EventGrid/*/yazma
* Microsoft.EventGrid/*/silme
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Son üç işlem, normal okuma işlemlerinden filtrelenen gizli bilgileri döndürür. Bu işlemlere erişimi kısıtlamanız önerilir. 

## <a name="built-in-roles"></a>Yerleşik roller

Olay Grid, olay aboneliklerini yönetmek için iki yerleşik rol sağlar. Kullanıcılara etkinlik etki alanınızdaki konulara abone olmaları için gereken izinleri verdikleri için [olay etki alanlarını](event-domains.md) uygularken önemlidirler. Bu roller olay aboneliklerine odaklanır ve konu oluşturma gibi eylemlere erişim vermez.

Bu [rolleri bir kullanıcıya veya gruba atayabilirsiniz.](../role-based-access-control/quickstart-assign-role-user-portal.md)

**EventGrid EventSubscription Katılımcısı**: Event Grid abonelik işlemlerini yönetin

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

**EventGrid EventAbonelik Okuyucu**: Olay Izgara aboneliklerini okuyun

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="custom-roles"></a>Özel roller

Yerleşik rollerden farklı izinler belirtmeniz gerekiyorsa, özel roller oluşturabilirsiniz.

Aşağıda, kullanıcıların farklı eylemlerde yer almasına olanak tanıyan örnek Olay Izgarası rol tanımları verilmiştir. Bu özel roller yerleşik rollerden farklıdır, çünkü olay aboneliklerinden daha geniş erişim verirler.

**EventGridReadOnlyRole.json**: Yalnızca salt okunur işlemlere izin verir.

```json
{
  "Name": "Event grid read only role",
  "Id": "7C0B6B59-A278-4B62-BA19-411B70753856",
  "IsCustom": true,
  "Description": "Event grid read only role",
  "Actions": [
    "Microsoft.EventGrid/*/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription Id>"
  ]
}
```

**EventGridNoDeleteListKeysRole.json**: Sınırlı gönderi eylemlerine izin verin, ancak eylemleri silmeme izin verin.

```json
{
  "Name": "Event grid No Delete Listkeys role",
  "Id": "B9170838-5F9D-4103-A1DE-60496F7C9174",
  "IsCustom": true,
  "Description": "Event grid No Delete Listkeys role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action"
  ],
  "NotActions": [
    "Microsoft.EventGrid/*/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

**EventGridContributorRole.json**: Tüm olay ızgara eylemleri sağlar.

```json
{
  "Name": "Event grid contributor role",
  "Id": "4BA6FB33-2955-491B-A74F-53C9126C9514",
  "IsCustom": true,
  "Description": "Event grid contributor role",
  "Actions": [
    "Microsoft.EventGrid/*/write",
    "Microsoft.EventGrid/*/delete",
    "Microsoft.EventGrid/topics/listkeys/action",
    "Microsoft.EventGrid/topics/regenerateKey/action",
    "Microsoft.EventGrid/eventSubscriptions/getFullUrl/action"
  ],
  "NotActions": [],
  "AssignableScopes": [
    "/subscriptions/<Subscription id>"
  ]
}
```

[PowerShell,](../role-based-access-control/custom-roles-powershell.md) [Azure CLI](../role-based-access-control/custom-roles-cli.md)ve [REST](../role-based-access-control/custom-roles-rest.md)ile özel roller oluşturabilirsiniz.



### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Olay Izgara hizmeti tarafından diske yazılan tüm olaylar veya veriler, microsoft tarafından yönetilen bir anahtarla şifrelenir ve bu durum istirahatte şifrelenir. Ayrıca, olayların veya verilerin tutulduğu maksimum [süre, Olay Izgarayeniden deneme ilkesine](delivery-and-retry.md)bağlı olarak 24 saattir. Olay Grid, 24 saat sonra tüm olayları veya verileri veya hangisi daha azsa, etkinlik süresini otomatik olarak siler.

## <a name="next-steps"></a>Sonraki adımlar

* Olay Izgarasına giriş için [Olay Izgarası Hakkında'ya](overview.md) bakın
