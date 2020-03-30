---
title: Azure Uygulama Yapılandırması anahtar değeri olaylarına tepki verme
description: Uygulama Yapılandırma olaylarına abone olmak için Azure Olay Ağıt'ı kullanın.
services: azure-app-configuration,event-grid
author: jimmyca
ms.author: jimmyca
ms.date: 02/20/2020
ms.topic: article
ms.service: azure-app-configuration
ms.openlocfilehash: a4f61d147ba1abf73ada6360b8d0d965d8e063a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523807"
---
# <a name="reacting-to-azure-app-configuration-events"></a>Azure Uygulama Yapılandırma sı olaylarına tepki verme

Azure Uygulama Yapılandırma olayları, uygulamaların anahtar değerlerdeki değişikliklere tepki göstermesini sağlar. Bu karmaşık kod veya pahalı ve verimsiz yoklama hizmetleri gerek kalmadan yapılır. Bunun yerine, etkinlikler [Azure Etkinlik Izgarası](https://azure.microsoft.com/services/event-grid/) üzerinden [Azure İşlevleri,](https://azure.microsoft.com/services/functions/) [Azure Mantık Uygulamaları](https://azure.microsoft.com/services/logic-apps/)ve hatta kendi özel http dinleyiciniz gibi abonelere itilir. Kritik olarak, sadece kullandığınız kadar ödeme.

Azure Uygulama Yapılandırma etkinlikleri, zengin yeniden deneme ilkeleri ve ölü harf teslimatı yoluyla uygulamalarınız için güvenilir teslimat hizmetleri sağlayan Azure Etkinlik Ağıtı'na gönderilir. Daha fazla bilgi için [Olay Izgara iletisi teslimi ve yeniden deneyin.](https://docs.microsoft.com/azure/event-grid/delivery-and-retry)

Yaygın Uygulama Yapılandırması olay senaryoları, uygulama yapılandırmasının yenilenmesi, dağıtımların tetiklemesine veya yapılandırmaya yönelik iş akışını içerir. Değişiklikler seyrek olduğunda, ancak senaryonuz anında yanıt verme gerektiriyorsa, olay tabanlı mimari özellikle verimli olabilir.

Hızlı bir örnek için Özel bir web bitiş noktasına Route Azure Uygulama Yapılandırma olaylarına bir göz atın [- CLI.](./howto-app-configuration-event.md) 

![Olay Izgara Modeli](./media/event-grid-functional-model.png)

## <a name="available-azure-app-configuration-events"></a>Kullanılabilir Azure Uygulama Yapılandırma etkinlikleri
Olay [ızgarası,](../event-grid/concepts.md#event-subscriptions) olay iletilerini abonelere yönlendirmek için olay aboneliklerini kullanır. Azure Uygulama Yapılandırma sı, iki tür olay içerebilir:  

> |Olay Adı|Açıklama|
> |----------|-----------|
> |`Microsoft.AppConfiguration.KeyValueModified`|Anahtar değeri oluşturulduğunda veya değiştirildiğinde ateşlendi|
> |`Microsoft.AppConfiguration.KeyValueDeleted`|Anahtar değeri silindiğinde ateşlendi|

## <a name="event-schema"></a>Olay şeması
Azure Uygulama Yapılandırma etkinlikleri, verilerinizdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.AppConfiguration" ile başladığı için bir Uygulama Yapılandırması olayını tanımlayabilirsiniz. Olay Izgara olay özelliklerinin kullanımı hakkında ek bilgiler [Olay Grid olay şemasında](../event-grid/event-schema.md)belgelenmiştir.  

> |Özellik|Tür|Açıklama|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |konu başlığı|string|Etkinliği yayıtan Uygulama Yapılandırmasının Tam Azure Kaynak Yöneticisi kimliği.|
> |Konu|string|Olayın konusu olan anahtar değerinin URI'si.|
> |eventTime|string|Olayın oluşturulduğu tarih/saat, ISO 8601 biçiminde.|
> |Eventtype|string|"Microsoft.AppConfiguration.KeyValueModified" veya "Microsoft.AppConfiguration.KeyValueDeleted".|
> |Kimlik|string|Bu olayın benzersiz bir tanımlayıcısı.|
> |dataVersion|string|Veri nesnesinin şema sürümü.|
> |metadataVersion|string|Üst düzey özelliklerin şema sürümü.|
> |veri|object|Azure Uygulama Yapılandırması'na özgü olay verilerinin toplanması|
> |data.key|string|Değiştirilen veya silinen anahtar değerinin anahtarı.|
> |data.label|string|Değiştirilen veya silinen anahtar değerinin etiketi.|
> |data.etag|string|Yeni `KeyValueModified` anahtar değerinin eetiketi için. Silinen anahtar değerinin eetiketi için. `KeyValueDeleted`|

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

Daha fazla bilgi için Azure [Uygulama Yapılandırma etkinlikleri şemasına](../event-grid/event-schema-app-configuration.md)bakın.

## <a name="practices-for-consuming-events"></a>Etkinlikleri tüketme uygulamaları
Uygulama Yapılandırma olaylarını işleyen uygulamalar aşağıdaki uygulamaları izlemelidir:
> [!div class="checklist"]
> * Olayları aynı olay işleyicisine yönlendirmek için birden çok abonelik yapılandırılabilir, bu nedenle olayların belirli bir kaynaktan geldiğini varsaymayın. Bunun yerine, uygulamayı yapılandırma örneğinin olayı gönderdiğinden emin olmak için iletinin konusunu denetleyin.
> * eventType'ı denetleyin ve aldığınız tüm olayların beklediğiniz türler olacağını düşünmeyin.
> * Nesneler hakkındaki bilgilerinizin hala güncel olup olmadığını anlamak için etag alanlarını kullanın.  
> * Belirli bir nesnedeki olayların sırasını anlamak için sıralayıcı alanlarını kullanın.
> * Değiştirilen anahtar değerine erişmek için konu alanını kullanın.


## <a name="next-steps"></a>Sonraki adımlar

Olay Ağı'sı hakkında daha fazla bilgi edinin ve Azure Uygulama Yapılandırma etkinliklerini deneyin:

- [Event Grid Hakkında](../event-grid/overview.md)
- [Azure Uygulama Yapılandırma olaylarını özel bir web bitiş noktasına yönlendirme](./howto-app-configuration-event.md)