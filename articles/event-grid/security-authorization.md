---
title: Azure Event Grid güvenliğini ve kimlik doğrulaması
description: Azure Event Grid ve kavramlarını açıklar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: babanisa
ms.openlocfilehash: 03bc2f9de6f50f08c9f62f86a3d1791a067cecd0
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78899288"
---
# <a name="authorizing-access-to-event-grid-resources"></a>Event Grid kaynaklarına erişimi yetkilendirme
Azure Event Grid olay abonelikleri listesi gibi çeşitli yönetim işlemlerini yapmak için yenilerini oluşturun ve anahtarlar oluşturmak için farklı kullanıcılara verilen erişim düzeyini denetlemenize olanak tanır. Event Grid, Azure'nın rol tabanlı erişim denetimi (RBAC) kullanır.

## <a name="operation-types"></a>İşlem türleri

Event Grid, aşağıdaki eylemleri destekler:

* Microsoft.EventGrid/*/read
* Microsoft.EventGrid/*/write
* Microsoft.EventGrid/*/delete
* Microsoft.EventGrid/eventSubscriptions/getFullUrl/action
* Microsoft.EventGrid/topics/listKeys/action
* Microsoft.EventGrid/topics/regenerateKey/action

Son üç işlemi dışında normal okuma işlemleri filtrelenmiş büyük olasılıkla gizli bilgi döndürür. Bu işlemler için erişimi kısıtlamak önerilir. 

## <a name="built-in-roles"></a>Yerleşik roller

Event Grid Olay Aboneliklerini yönetmek için iki yerleşik rol sağlar. Kullanıcılara olay etki alanındaki konulara abone olmaları için ihtiyaç duydukları izinleri verdiklerinden, bunlar [olay etki alanlarını](event-domains.md) uygularken önemlidir. Bu roller, olay abonelikleri üzerinde odaklanır ve konuları oluşturma gibi eylemler için erişim izni yok.

[Bu rolleri bir kullanıcıya veya gruba atayabilirsiniz](../role-based-access-control/quickstart-assign-role-user-portal.md).

**Eventgrid EventSubscription katılımcısı**: Event Grid abonelik işlemlerini yönetme

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

**Eventgrid EventSubscription okuyucusu**: Event Grid abonelikleri okuma

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

Yerleşik roller farklı olan izinleri belirtmeniz gerekiyorsa, özel roller oluşturabilirsiniz.

Farklı eylemlerde bulunmak kullanıcıların örnek Event Grid rol tanımları aşağıda verilmiştir. Bunlar yalnızca olay abonelikleri daha geniş erişim vermek için bu özel roller yerleşik rollerden farklıdır.

**Eventgridreadonlyrole. JSON**: yalnızca salt okuma işlemlerine izin verir.

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

**Eventgridnodeletelistkeysrole. JSON**: kısıtlı gönderi eylemlerine izin verin ancak silme eylemlerine izin vermeyin.

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

**EventGridContributorRole. JSON**: tüm olay Kılavuzu eylemlerine izin verir.

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

[PowerShell](../role-based-access-control/custom-roles-powershell.md), [Azure CLI](../role-based-access-control/custom-roles-cli.md)ve [rest](../role-based-access-control/custom-roles-rest.md)ile özel roller oluşturabilirsiniz.



### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Event Grid hizmeti tarafından diske yazılan tüm olaylar veya veriler, bekleyen bir şekilde şifrelendiğinden emin olmak için Microsoft tarafından yönetilen bir anahtarla şifrelenir. Ayrıca, olayların veya verilerin saklanacağı en uzun süre, [Event Grid yeniden deneme ilkesiyle](delivery-and-retry.md)ilgili olarak 24 saat olur. Event Grid, 24 saat sonra tüm olayları veya verileri otomatik olarak siler veya olayın yaşam süresi (hangisi daha az) olur.

## <a name="next-steps"></a>Sonraki adımlar

* Event Grid giriş için bkz. [Event Grid hakkında](overview.md)
