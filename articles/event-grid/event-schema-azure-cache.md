---
title: Redsıs için Azure önbelleği Event Grid kaynak olarak
description: Azure Event Grid ile Redsıs olayları için Azure önbelleği için sunulan özellikleri açıklar
ms.topic: conceptual
ms.date: 12/21/2020
author: curib
ms.author: cauribeg
ms.openlocfilehash: f446f3f469a7404e6e74ba67ee24bf32578fe9d8
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99056224"
---
# <a name="azure-cache-for-redis-as-an-event-grid-source"></a>Reda için Azure önbelleği Event Grid kaynak olarak

Bu makalede redin olayları için Azure önbelleği özellikleri ve şeması sağlanmaktadır. Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md). 

## <a name="event-grid-event-schema"></a>Event Grid olay şeması

### <a name="list-of-events-for-azure-cache-for-redis-rest-apis"></a>Redıs REST API 'Leri için Azure önbelleği olaylarının listesi

Bu olaylar, istemci Redsıs REST API 'Leri için Azure önbelleği çağırarak dışarı aktardığınızda, içeri aktardığında veya ölçekleyerek tetiklenir. Düzeltme eki uygulama Redsıs Update tarafından tetiklenir.

 |Olay adı |Description|
 |----------|-----------|
 |**Microsoft. cache. ExportRDBCompleted** |Önbellek verileri verildiğinde tetiklenir. |
 |**Microsoft. cache. ımportrdbcompleted** |Önbellek verileri içeri aktarıldığında tetiklenir. |
 |**Microsoft. cache. Patchıncompleted** |Düzeltme Eki tamamlandığında tetiklenir. |
 |**Microsoft. cache. ScalingCompleted** |Ölçeklendirme tamamlandığında tetiklenir. |

<a name="example-event"></a>
### <a name="the-contents-of-an-event-response"></a>Olay yanıtının içeriği

Bir olay tetiklendiğinde, Event Grid hizmeti bu olayla ilgili verileri bu olay ile abone olacak şekilde gönderir.

Bu bölüm, redin olayı için her bir Azure önbelleği için verilerin nasıl görüneceğine ilişkin bir örnek içerir.

### <a name="microsoftcachepatchingcompleted-event"></a>Microsoft. cache. PatchingCompleted olayı

```json
[{
"id":"9b87886d-21a5-4af5-8e3e-10c4b8dac73b",
"eventType":"Microsoft.Cache.PatchingCompleted",
"topic":"/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
"data":{
    "name":"PatchingCompleted",
    "timestamp":"2020-12-09T21:50:19.9995668+00:00",
    "status":"Succeeded"},
"subject":"PatchingCompleted",
"dataversion":"1.0",
"metadataVersion":"1",
"eventTime":"2020-12-09T21:50:19.9995668+00:00"}]
```

### <a name="microsoftcacheimportrdbcompleted-event"></a>Microsoft. cache. ımportrdbcompleted olayı

```json
[{
"id":"9b87886d-21a5-4af5-8e3e-10c4b8dac73b",
"eventType":"Microsoft.Cache.ImportRDBCompleted",
"topic":"/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
"data":{
    "name":"ImportRDBCompleted",
    "timestamp":"2020-12-09T21:50:19.9995668+00:00",
    "status":"Succeeded"},
"subject":"ImportRDBCompleted",
"dataversion":"1.0",
"metadataVersion":"1",
"eventTime":"2020-12-09T21:50:19.9995668+00:00"}]
```

### <a name="microsoftcacheexportrdbcompleted-event"></a>Microsoft. cache. ExportRDBCompleted olayı

```json
[{
"id":"9b87886d-21a5-4af5-8e3e-10c4b8dac73b",
"eventType":"Microsoft.Cache.ExportRDBCompleted",
"topic":"/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
"data":{
    "name":"ExportRDBCompleted",
    "timestamp":"2020-12-09T21:50:19.9995668+00:00",
    "status":"Succeeded"},
"subject":"ExportRDBCompleted",
"dataversion":"1.0",
"metadataVersion":"1",
"eventTime":"2020-12-09T21:50:19.9995668+00:00"}]
```

### <a name="microsoftcachescalingcompleted"></a>Microsoft. cache. ScalingCompleted

```json
[{
"id":"9b87886d-21a5-4af5-8e3e-10c4b8dac73b",
"eventType":"Microsoft.Cache.ScalingCompleted",
"topic":"/subscriptions/{subscription_id}/resourceGroups/{resource_group_name}/providers/Microsoft.Cache/Redis/{cache_name}",
"data":{
    "name":"ScalingCompleted",
    "timestamp":"2020-12-09T21:50:19.9995668+00:00",
    "status":"Succeeded"},
"subject":"ScalingCompleted",
"dataversion":"1.0",
"metadataVersion":"1",
"eventTime":"2020-12-09T21:50:19.9995668+00:00"}]
```

### <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki en üst düzey verilere sahiptir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| subject | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Etkinliğin UTC saatine göre oluşturulduğu zaman. |
| kimlik | string | Etkinliğin benzersiz tanımlayıcısı. |
| veriler | object | Redsıs olay verileri için Azure önbelleği. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Description |
| -------- | ---- | ----------- |
| timestamp | string | Olayın gerçekleştiği zaman. |
| name | string | Olayın adı. |
| durum | string | Etkinliğin durumu. Başarısız veya başarılı oldu. |


## <a name="quickstarts"></a>Hızlı Başlangıçlar

Redsıs olayları için Azure önbelleği 'ni denemek istiyorsanız, bu hızlı başlangıç makalelerinden herhangi birine bakın:

|Bu aracı kullanmak istiyorsanız:    |Şu makaleye bakın: |
|--|-|
|Azure portalı    |[Hızlı başlangıç: Azure portal ile Redsıs olayları için Azure önbelleğini Web uç noktasına yönlendirme](../azure-cache-for-redis/cache-event-grid-quickstart-portal.md)|
|PowerShell    |[Hızlı başlangıç: PowerShell ile Redsıs olayları için Azure önbelleğini Web uç noktasına yönlendirme](../azure-cache-for-redis/cache-event-grid-quickstart-powershell.md)|
|Azure CLI’si    |[Hızlı başlangıç: Azure CLı ile Redsıs olayları için Azure önbelleğini Web uç noktasına yönlendirme](../azure-cache-for-redis/cache-event-grid-quickstart-cli.md)|

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).

