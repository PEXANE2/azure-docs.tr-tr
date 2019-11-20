---
title: Azure Uygulama yapılandırması anahtar-değer olaylarına yeniden davranıyor | Microsoft Docs
description: Uygulama yapılandırma olaylarına abone olmak için Azure Event Grid kullanın.
services: azure-app-configuration,event-grid
author: jimmyca
ms.author: jimmyca
ms.date: 05/30/2019
ms.topic: article
ms.service: azure-app-configuration
ms.openlocfilehash: 5da64155f2823712eee7a60427b1c1e80abec068
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185294"
---
# <a name="reacting-to-azure-app-configuration-events"></a>Azure uygulama yapılandırma olaylarına yeniden davranıma

Azure uygulama yapılandırma olayları, uygulamaların anahtar değerlerinde değişikliklere tepki vermesini sağlar. Bu, karmaşık kod veya pahalı ve verimsiz yoklama Hizmetleri gerekmeden yapılır. Bunun yerine, olaylar [Azure işlevleri](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), hatta kendi özel http dinleyiciniz gibi abonelere [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) gönderilir ve yalnızca kullandığınız kadar ödersiniz.

Azure uygulama yapılandırma olayları, zengin yeniden deneme ilkeleri ve atılacak mektup teslimi aracılığıyla uygulamalarınıza güvenilir teslim hizmetleri sağlayan Azure Event Grid gönderilir. Daha fazla bilgi edinmek için bkz. [ileti teslimi Event Grid ve yeniden deneyin](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

Ortak uygulama yapılandırma olay senaryoları, uygulama yapılandırmasını yenilemeyi, dağıtımları tetiklemeyi veya herhangi bir yapılandırmaya dayalı iş akışını içerir. Değişiklikler seyrek olduğunda, ancak senaryonuz anında yanıt vermeyi gerektirdiğinde, olay tabanlı mimari özellikle etkili olabilir.

[Azure uygulama yapılandırma olaylarını hızlı bir örnek için özel bir Web uç noktasına yönlendir-CLI](./howto-app-configuration-event.md) bölümüne göz atın. 

![Event Grid modeli](./media/event-grid-functional-model.png)

## <a name="available-azure-app-configuration-events"></a>Kullanılabilir Azure uygulama yapılandırma olayları
Olay Kılavuzu, olay iletilerini abonelere yönlendirmek için [olay abonelikleri](../event-grid/concepts.md#event-subscriptions) kullanır. Azure uygulama yapılandırma olay abonelikleri iki tür olay içerebilir:  

> |Olay adı|Açıklama|
> |----------|-----------|
> |`Microsoft.AppConfiguration.KeyValueModified`|Anahtar-değer oluşturulduğunda veya değiştirildiğinde tetiklenir|
> |`Microsoft.AppConfiguration.KeyValueDeleted`|Anahtar-değer silindiğinde harekete geçirilir|

## <a name="event-schema"></a>Olay şeması
Azure uygulama yapılandırma olayları, verilerdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği "Microsoft. AppConfiguration" ile başladığı için bir uygulama yapılandırma olayını tanımlayabilirsiniz. Event Grid olay özelliklerinin kullanımı hakkında ek bilgiler [Event Grid olay şeması](../event-grid/event-schema.md)'nda belgelenmiştir.  

> |Özellik|Tür|Açıklama|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |konu başlığı|string|Olayı gösteren uygulama yapılandırmasının tam Azure Resource Manager kimliği.|
> |subject|string|Olayın konusu olan anahtar-değer URI 'SI.|
> |eventTime|string|Olayın oluşturulduğu tarih/saat, ISO 8601 biçiminde.|
> |eventType|string|"Microsoft. AppConfiguration. KeyValueModified" veya "Microsoft. AppConfiguration. KeyValueDeleted".|
> |Kimlik|string|Bu olayın benzersiz tanımlayıcısı.|
> |dataVersion|string|Veri nesnesinin şema sürümü.|
> |metadataVersion|string|Üst düzey özelliklerin şema sürümü.|
> |data|object|Azure Uygulama yapılandırmasına özgü olay verileri koleksiyonu|
> |Data. Key|string|Değiştirilen veya silinen anahtar-değer anahtarı.|
> |Data. Label|string|Değiştirilen veya silinen anahtar-değer etiketi.|
> |Data. ETag|string|`KeyValueModified` için, yeni anahtar-değer ETag ' i. `KeyValueDeleted` için, silinen anahtar değerinin ETag 'i.|

KeyValueModified olayına bir örnek aşağıda verilmiştir:
```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]

```

Daha fazla bilgi için bkz. [Azure uygulama yapılandırma olayları şeması](../event-grid/event-schema-app-configuration.md).

## <a name="practices-for-consuming-events"></a>Olayları tüketen uygulamalar
Uygulama yapılandırma olaylarını işleyen uygulamalar birkaç önerilen uygulamayı izlemelidir:
> [!div class="checklist"]
> * Birden çok abonelik olayları aynı olay işleyicisine yönlendirmek üzere yapılandırılabilmesine rağmen, olayların belirli bir kaynaktan olduğunu varsaymamak, ancak beklediğiniz uygulama yapılandırmasından geldiğinden emin olmak için iletinin konusunu denetlemek önemlidir.
> * Benzer şekilde, eventType için hazırlanmakta olan bir olay olduğunu ve aldığınız tüm olayların istediğiniz tür olacağını kabul edin.
> * İletiler sıraya alınır ve bir gecikmeden sonra, nesneler hakkındaki bilgilerinizin güncel olup olmadığını anlamak için ETag alanlarını kullanın.  Ayrıca, belirli bir nesne üzerindeki olayların sırasını anlamak için sıralayıcı alanlarını kullanın.
> * Değiştirilen anahtar değere erişmek için konu alanını kullanın.


## <a name="next-steps"></a>Sonraki adımlar

Event Grid hakkında daha fazla bilgi edinin ve Azure uygulama yapılandırma olaylarına bir deneme hakkı verin:

- [Event Grid Hakkında](../event-grid/overview.md)
- [Azure uygulama yapılandırma olaylarını özel bir Web uç noktasına yönlendirme](./howto-app-configuration-event.md)