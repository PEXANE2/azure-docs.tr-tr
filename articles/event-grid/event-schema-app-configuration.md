---
title: Event Grid kaynak olarak Azure Uygulama yapılandırması
description: Bu makalede, Azure Uygulama yapılandırması 'nın Event Grid olay kaynağı olarak nasıl kullanılacağı açıklanır. Bu, şemayı ve öğretici ve nasıl yapılır makalelerini bağlar ve bağlantılar sağlar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: d305236e8408052be4be28ec003f4e545119fc59
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550683"
---
# <a name="azure-app-configuration-as-an-event-grid-source"></a>Event Grid kaynağı olarak Azure Uygulama yapılandırması
Bu makalede, Azure uygulama yapılandırma olayları için özellikler ve şema sağlanmaktadır. Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md). Ayrıca, Azure Uygulama yapılandırması 'nı olay kaynağı olarak kullanmak için hızlı başlayan ve öğreticilerin bir listesini sağlar.

## <a name="event-grid-event-schema"></a>Event Grid olay şeması

### <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure Uygulama yapılandırması aşağıdaki olay türlerini yayar:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft. AppConfiguration. KeyValueModified | Anahtar-değer oluşturulduğunda veya değiştirildiğinde tetiklenir. |
| Microsoft. AppConfiguration. KeyValueDeleted | Anahtar-değer silindiğinde tetiklenir. |

### <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, anahtar-değer değiştirilmiş olayının şemasını gösterir: 

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

Anahtar-değer Deleted olayının şeması benzerdir: 

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
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
 
### <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki en üst düzey verilere sahiptir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| subject | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Etkinliğin UTC saatine göre oluşturulduğu zaman. |
| ID | string | Etkinliğin benzersiz tanımlayıcısı. |
| veriler | object | Uygulama yapılandırma olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| anahtar | string | Değiştirilen veya silinen anahtar-değer anahtarı. |
| etiket | string | Değiştirilen veya silinen anahtar-değer etiketi. |
| özelliği | string | `KeyValueModified`Yeni anahtar-değer ETag için. `KeyValueDeleted`Silinen anahtar değerinin ETag 'i için. |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları

|Başlık | Açıklama |
|---------|---------|
| [Event Grid kullanarak Azure uygulama yapılandırma olaylarına tepki verme](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid ile Azure uygulama yapılandırmasını tümleştirmeyle ilgili genel bakış. |
| [Veri değişikliği bildirimleri için Event Grid kullanma](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure uygulama yapılandırma olay aboneliklerini kullanarak bir Web uç noktasına anahtar-değer değiştirme olayları gönderme hakkında bilgi edinin. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).
* Azure uygulama yapılandırma olayları ile çalışmaya giriş için bkz. [veri değişikliği bildirimleri için Event Grid kullanma](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 