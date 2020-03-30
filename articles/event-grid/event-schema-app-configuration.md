---
title: Azure Olay Izgarası Azure Uygulama Yapılandırma olay şeması
description: Azure Etkinlik Ağıtı ile Azure Uygulama Yapılandırma etkinlikleri için sağlanan özellikleri açıklar
services: event-grid
author: jimmyca
ms.service: event-grid
ms.topic: reference
ms.date: 05/30/2019
ms.author: jimmyca
ms.openlocfilehash: fe0274f723692eea3cfd25cc0e9e146b35dce2ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66735789"
---
# <a name="azure-event-grid-event-schema-for-azure-app-configuration"></a>Azure Uygulama Yapılandırması için Azure Olay Izgara olay şeması

Bu makalede, Azure Uygulama Yapılandırma sı için özellikler ve şema sağlanmaktadır. Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](event-schema.md)bakın.

Örnek komut dosyaları ve öğreticilerin listesi için [Azure Uygulama Yapılandırması etkinlik kaynağına](event-sources.md#app-configuration)bakın.

## <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Azure Uygulama Yapılandırması aşağıdaki olay türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.appconfiguration.keyvaluemodified | Anahtar değeri oluşturulduğunda veya değiştirildiğinde yükseltilir. |
| Microsoft.AppConfiguration.keyValueDeleted | Anahtar değeri silindiğinde yükseltilir. |

## <a name="example-event"></a>Örnek olay

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
 
## <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Uygulama Yapılandırma olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| anahtar | string | Değiştirilen veya silinen anahtar değerinin anahtarı. |
| etiket | string | Değiştirilen veya silinen anahtar değerinin etiketi. |
| Etag | string | Yeni `KeyValueModified` anahtar değerinin eetiketi için. Silinen anahtar değerinin eetiketi için. `KeyValueDeleted` |
 
## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
* Azure Uygulama Yapılandırma etkinlikleri ile çalışmaya giriş için Bkz. [Azure Uygulama Yapılandırma etkinlikleri - Azure CLI](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 