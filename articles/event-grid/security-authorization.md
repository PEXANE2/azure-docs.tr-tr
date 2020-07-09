---
title: Azure Event Grid güvenlik ve kimlik doğrulaması
description: Azure Event Grid ve kavramlarını açıklar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 09317b310a5934d27b82d265ec7f9b276135f882
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119098"
---
# <a name="authorizing-access-to-event-grid-resources"></a>Event Grid kaynaklarına erişimi yetkilendirme
Azure Event Grid, farklı kullanıcılara verilen erişim düzeyini, olay abonelikleri listeleme, yenilerini oluşturma ve anahtar oluşturma gibi çeşitli yönetim işlemlerini yapmak için denetlemenizi sağlar. Event Grid, Azure 'un rol tabanlı erişim denetimi 'ni (RBAC) kullanır.


## <a name="operation-types"></a>İşlem türleri

Event Grid aşağıdaki eylemleri destekler:

* Microsoft. EventGrid/*/Read
* Microsoft. EventGrid/*/Write
* Microsoft. EventGrid/*/Delete
* Microsoft. EventGrid/Eventabonelikler/getFullUrl/eylem
* Microsoft. EventGrid/konular/listKeys/ACTION
* Microsoft. EventGrid/konular/regenerateKey/Action

Son üç işlem, normal okuma işlemlerinden filtre uygulanan olası gizli bilgileri döndürür. Bu işlemlere erişimi kısıtlamanız önerilir. 

## <a name="built-in-roles"></a>Yerleşik roller

Event Grid, olay aboneliklerini yönetmek için iki yerleşik rol sağlar. Kullanıcılara olay etki alanındaki konulara abone olmaları için ihtiyaç duydukları izinleri verdiklerinden, bunlar [olay etki alanlarını](event-domains.md) uygularken önemlidir. Bu roller olay aboneliklerine odaklanılmıştır ve konu başlıkları oluşturma gibi eylemler için erişim vermez.

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

Yerleşik rollerden farklı izinleri belirtmeniz gerekiyorsa, özel roller oluşturabilirsiniz.

Aşağıda, kullanıcıların farklı eylemler yapmasına izin veren örnek Event Grid rol tanımları verilmiştir. Bu özel roller, yalnızca olay aboneliklerinden daha geniş erişim verdiklerinden, yerleşik rollerden farklıdır.

**EventGridReadOnlyRole.js**:: yalnızca salt okuma işlemlerine izin verin.

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

**EventGridNoDeleteListKeysRole.js**: kısıtlanmış gönderme eylemlerine izin ver, ancak silme eylemlerine izin verme.

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

**EventGridContributorRole.js**: tüm olay Kılavuzu eylemlerine izin verir.

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

## <a name="permissions-for-event-subscriptions"></a>Olay abonelikleri için izinler
Web kancası olmayan bir olay işleyicisi (örneğin, bir olay hub 'ı veya kuyruk depolaması) kullanıyorsanız, bu kaynağa yazma erişimi gerekir. Bu izin denetimi yetkisiz bir kullanıcının kaynağa olay göndermesini engeller.

Olay kaynağı olan kaynakta **Microsoft. EventGrid/Eventabonelikleriniz/Write** izninizin olması gerekir. Kaynak kapsamına yeni bir abonelik yazıyorsanız, bu izne ihtiyacınız vardır. Gerekli kaynak, bir sistem konusuna veya özel konuya abone olup olmadığınız temel alınarak farklılık gösterir. Her iki tür de bu bölümde açıklanmaktadır.

### <a name="system-topics-azure-service-publishers"></a>Sistem konuları (Azure hizmet yayımcıları)
Sistem konuları için, olayı yayımlayan kaynağın kapsamına yeni bir olay aboneliği yazma izninizin olması gerekir. Kaynağın biçimi:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Örneğin, **myacct**adlı bir depolama hesabındaki bir olaya abone olmak için Microsoft. Eventgrid/Eventaboneliklerin/Write izninizin olması gerekir:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Özel konular
Özel konularda, olay Kılavuzu konusunun kapsamına yeni bir olay aboneliği yazma izninizin olması gerekir. Kaynağın biçimi:`/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Örneğin, **MyTopic**adlı özel bir konuya abone olmak için Microsoft. Eventgrid/Eventaboneliklerin/Write izninizin olması gerekir:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`



## <a name="next-steps"></a>Sonraki adımlar

* Event Grid giriş için bkz. [Event Grid hakkında](overview.md)
