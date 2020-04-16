---
title: Olay Izgara kaynağı olarak Azure Uygulama Yapılandırması
description: Bu makalede, Olay Ağı olay kaynağı olarak Azure Uygulama Yapılandırması nasıl kullanılacağı açıklanmaktadır. Bu şema ve öğretici ve nasıl-to-to makalelere bağlantılar sağlar.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: babanisa
ms.openlocfilehash: adb548ef8531698a2cb075fbc742bb20a02a434b
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393425"
---
# <a name="azure-app-configuration-as-an-event-grid-source"></a>Olay Izgara kaynağı olarak Azure Uygulama Yapılandırması
Bu makalede, Azure Uygulama Yapılandırma sı için özellikler ve şema sağlanmaktadır. Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın. Ayrıca, Azure Uygulama Yapılandırmasını etkinlik kaynağı olarak kullanmanız için hızlı başlangıçların ve öğreticilerin bir listesini de verir.

## <a name="event-grid-event-schema"></a>Olay Izgara olay şeması

### <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Azure Uygulama Yapılandırması aşağıdaki olay türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.appconfiguration.keyvaluemodified | Anahtar değeri oluşturulduğunda veya değiştirildiğinde yükseltilir. |
| Microsoft.AppConfiguration.keyValueDeleted | Anahtar değeri silindiğinde yükseltilir. |

### <a name="example-event"></a>Örnek olay

Aşağıdaki örnek, anahtar değeri değiştirilmiş bir olayın şema gösterir: 

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

Anahtar değeri silinen bir olayın şeması benzer: 

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

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| Kimlik | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Uygulama Yapılandırma olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| anahtar | string | Değiştirilen veya silinen anahtar değerinin anahtarı. |
| etiket | string | Değiştirilen veya silinen anahtar değerinin etiketi. |
| Etag | string | Yeni `KeyValueModified` anahtar değerinin eetiketi için. Silinen anahtar değerinin eetiketi için. `KeyValueDeleted` |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları

|Başlık | Açıklama |
|---------|---------|
| [Olay Kılavuz'u kullanarak Azure Uygulama Yapılandırma olaylarına tepki verme](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure Uygulama Yapılandırması'nı Olay Ağıla tümleştirmeye genel bakış. |
| [Hızlı başlangıç: Azure Uygulama Yapılandırma olaylarını Azure CLI ile özel bir web bitiş noktasına yönlendirin](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure Uygulama Yapılandırma olaylarını Bir WebHook'a göndermek için Azure CLI'nin nasıl kullanılacağını gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
* Azure Uygulama Yapılandırma etkinlikleri ile çalışmaya giriş için Bkz. [Azure Uygulama Yapılandırma etkinlikleri - Azure CLI](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 