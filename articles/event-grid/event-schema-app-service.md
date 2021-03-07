---
title: Event Grid kaynak olarak Azure App Service
description: Bu makalede, Azure App Service Event Grid olay kaynağı olarak nasıl kullanılacağı açıklanır. Bu, şemayı ve öğretici ve nasıl yapılır makalelerini bağlar ve bağlantılar sağlar.
author: jasonfreeberg
ms.topic: conceptual
ms.date: 03/06/2021
ms.author: jafreebe
ms.openlocfilehash: 404fac634a628da49dee72b10b52785820fe1bf6
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102443786"
---
# <a name="azure-app-service-as-an-event-grid-source"></a>Event Grid kaynak olarak Azure App Service

Bu makalede Azure App Service olayları için özellikler ve şema sağlanmaktadır. Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](event-schema.md). Ayrıca, bir olay kaynağı olarak Azure App Service kullanmak için hızlı başlayan ve öğreticilerin bir listesini de sağlar.

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure App Service aşağıdaki olay türlerini yayar

|    Olay Türü                                             |    Açıklama                                                     |
|-----------------------------------------------------------|--------------------------------------------------------------------|
|    Microsoft. Web/Sites. Backupoperationbaşlatıldı             |    Bir yedekleme başlatıldığında tetiklendi                             |
|    Microsoft. Web/Sites. BackupOperationCompleted           |    Bir yedekleme tamamlandığında tetiklenir                           |
|    Microsoft. Web/Sites. Backupoperationbaşarısız oldu              |    Bir yedekleme başarısız olduğunda tetiklenir                              |
|    Microsoft. Web/Sites. RestoreOperationStarted            |    Yedekten geri yükleme başlatıldığında tetiklenir        |
|    Microsoft. Web/Sites. RestoreOperationCompleted          |    Yedekten geri yükleme tamamlandığında tetiklenir      |
|    Microsoft. Web/Sites. RestoreOperationFailed             |    Yedekten geri yükleme başarısız olduğunda tetiklenir         |
|    Microsoft. Web/Sites. SlotSwapStarted                    |    Yuva takası başlatıldığında tetiklenir                          |
|    Microsoft. Web/Sites. Slotswaptamamlandı                  |    Yuva takası tamamlandığında tetiklenir                      |
|    Microsoft. Web/Sites. Slotswapbaşarısız oldu                     |    Yuva değiştirme başarısız olduğunda tetiklenir                           |
|    Microsoft. Web/Sites. Slotswapwithönizleme başlatıldı         |    Önizleme ile bir yuva değiştirme başladığında tetiklenir           |
|    Microsoft. Web/Sites. Slotswapwithönizlemesi Iptal edildi       |    Önizleme ile bir yuva değiştirme iptal edildiğinde tetiklenir    |
|    Microsoft. Web/Sites. AppUpdated. yeniden başlatıldı               |    Bir site yeniden başlatıldığında tetiklenir                      |
|    Microsoft. Web/Sites. AppUpdated. durduruldu                 |    Bir site durdurulduğunda tetiklenir                          |
|    Microsoft. Web/Sites. AppUpdated. ChangedAppSettings      |    Bir sitenin uygulama ayarları değiştiğinde tetiklenir             |
|    Microsoft. Web/sunucugrupları. AppServicePlanUpdated        |    App Service planı güncelleştirilirken tetiklenir                 |

## <a name="properties-common-to-all-events"></a>Tüm olaylarda ortak olan özellikler

# <a name="event-grid-event-schema"></a>[Event Grid olay şeması](#tab/event-grid-event-schema)
Bir olay tetiklendiğinde, Event Grid hizmeti bu olayla ilgili verileri bu olay ile abone olacak şekilde gönderir.
Bu bölüm, verilerin her olay için nasıl görüneceğine ilişkin bir örnek içerir. Her olay aşağıdaki en üst düzey verilere sahiptir:

|     Özellik          |     Tür     |     Description                                                                                                                                |
|-----------------------|--------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|    `topic`              |    dize    |    Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar.                                      |
|    `subject`            |    string    |    Olay konusunun yayımcı tanımlı yolu.                                                                                              |
|    `eventType`          |    string    |    Bu olay kaynağı için kayıtlı olay türlerinden biri.                                                                                  |
|    `eventTime`          |    string    |    Etkinliğin UTC saatine göre oluşturulduğu zaman.                                                                         |
|    `id`                 |    string    |    Etkinliğin benzersiz tanımlayıcısı.                                                                                                            |
|    `data`               |    object    |    BLOB depolama olay verileri.                                                                                                                    |
|    `dataVersion`        |    string    |    Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar.                                                          |
|    `metadataVersion`    |    string    |    Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar.    |

# <a name="cloud-event-schema"></a>[Bulut olay şeması](#tab/cloud-event-schema)

Bir olay tetiklendiğinde, Event Grid hizmeti bu olayla ilgili verileri bu olay ile abone olacak şekilde gönderir.
Bu bölüm, verilerin her olay için nasıl görüneceğine ilişkin bir örnek içerir. Her olay aşağıdaki en üst düzey verilere sahiptir:

|     Özellik          |     Tür     |     Description                                                                                                                                |
|-----------------------|--------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|    `source`              |    dize    |    Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar.                                      |
|    `subject`            |    string    |    Olay konusunun yayımcı tanımlı yolu.                                                                                              |
|    `type`          |    string    |    Bu olay kaynağı için kayıtlı olay türlerinden biri.                                                                                  |
|    `time`          |    string    |    Etkinliğin UTC saatine göre oluşturulduğu zaman.                                                                         |
|    `id`                 |    string    |    Etkinliğin benzersiz tanımlayıcısı.                                                                                                            |
|    `data`               |    object    |    BLOB depolama olay verileri.                                                                                                                    |
| `specversion` | string | CloudEvents şema belirtimi sürümü. |

---

## <a name="example-events"></a>Örnek olaylar

### <a name="backupoperationstarted-backupoperationcompleted-backupoperationfailed"></a>BackupOperationStarted, BackupOperationCompleted, Backupoperationbaşarısız oldu

# <a name="event-grid-event-schema"></a>[Event Grid olay şeması](#tab/event-grid-event-schema)

```json
{
    "id": "7c5d6de5-eb70-4de2-b788-c52a544e68b8",
    "topic": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "eventType": "Microsoft.Web.BackupOperationStarted",
    "eventTime": "2020-01-28T18:26:51.7194887Z",
    "data": {
        "appEventTypeDetail": {
            "action": "Started"
        },
        "name": "<site-name>",
        "clientRequestId": "None",
        "correlationRequestId": "None",
        "requestId": "292f499d-04ee-4066-994d-c2df57b99198",
        "address": "None",
        "verb": "None"
    },
    "dataVersion": "1",
    "metaDataVersion": "1"
}
```
# <a name="cloud-event-schema"></a>[Bulut olay şeması](#tab/cloud-event-schema)

```json
{
    "id": "7c5d6de5-eb70-4de2-b788-c52a544e68b8",
    "source": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "type": "Microsoft.Web.BackupOperationStarted",
    "time": "2020-01-28T18:26:51.7194887Z",
    "data": {
        "appEventTypeDetail": {
            "action": "Started"
        },
        "name": "<site-name>",
        "clientRequestId": "None",
        "correlationRequestId": "None",
        "requestId": "292f499d-04ee-4066-994d-c2df57b99198",
        "address": "None",
        "verb": "None"
    },
    "specversion": "1.0"
}
```

---

Veri nesnesi aşağıdaki özellikleri içerir:

|    Özellik                |    Tür      |    Açıklama                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    `appEventTypeDetail`      |    object    |    Uygulamadaki eylemin ayrıntıları                                                                                       |
|    `action`                  |    string    |    İşlem eyleminin türü                                                                                   |
|    `name`                    |    string    |    Bu olaya sahip olan Web sitesinin adı                                                                          |
|    `clientRequestId`         |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istemci istek KIMLIĞI         |
|    `correlationRequestId`    |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan bağıntı istek KIMLIĞI    |
|    `requestId`               |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istek KIMLIĞI                |
|    `address`                 |    string    |    Bu işlemin HTTP isteği URL 'SI                                                                                |
|    `verb`                    |    string    |    Bu işlemin HTTP fiili                                                                                       |

### <a name="restoreoperationstarted-restoreoperationcompleted-restoreoperationfailed"></a>RestoreOperationStarted, RestoreOperationCompleted, RestoreOperationFailed

# <a name="event-grid-event-schema"></a>[Event Grid olay şeması](#tab/event-grid-event-schema)

```json
{
    "id": "7c5d6de5-eb70-4de2-b788-c52a544e68b8",
    "topic": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "eventType": "Microsoft.Web.RestoreOperationStarted",
    "eventTime": "2020-01-28T18:26:51.7194887Z",
    "data": {
        "appEventTypeDetail": {
            "action": "Started"
        },
        "name": "<site-name>",
        "clientRequestId": "None",
        "correlationRequestId": "None",
        "requestId": "292f499d-04ee-4066-994d-c2df57b99198",
        "address": "None",
        "verb": "POST"
    },
    "dataVersion": "1",
    "metaDataVersion": "1"
}
```

# <a name="cloud-event-schema"></a>[Bulut olay şeması](#tab/cloud-event-schema)

```json
{
    "id": "7c5d6de5-eb70-4de2-b788-c52a544e68b8",
    "source": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "type": "Microsoft.Web.RestoreOperationStarted",
    "time": "2020-01-28T18:26:51.7194887Z",
    "data": {
        "appEventTypeDetail": {
            "action": "Started"
        },
        "name": "<site-name>",
        "clientRequestId": "None",
        "correlationRequestId": "None",
        "requestId": "292f499d-04ee-4066-994d-c2df57b99198",
        "address": "None",
        "verb": "POST"
    },
    "specversion": "1.0"
}
```

---

Veri nesnesi aşağıdaki özellikleri içerir:

|    Özellik                |    Tür      |    Açıklama                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    `appEventTypeDetail`      |    object    |    Uygulamadaki eylemin ayrıntıları                                                                                       |
|    `action`                  |    string    |    İşlem eyleminin türü                                                                                   |
|    `name`                    |    string    |    Bu olaya sahip olan Web sitesinin adı                                                                          |
|    `clientRequestId`         |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istemci istek KIMLIĞI         |
|    `correlationRequestId`    |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan bağıntı istek KIMLIĞI    |
|    `requestId`               |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istek KIMLIĞI                |
|    `address`                 |    string    |    Bu işlemin HTTP isteği URL 'SI                                                                                |
|    `verb`                    |    string    |    Bu işlemin HTTP fiili                                                                                       |

### <a name="slotswapstarted-slotswapcompleted-slotswapfailed"></a>SlotSwapStarted, SlotSwapCompleted, Slotswapbaşarısız oldu

# <a name="event-grid-event-schema"></a>[Event Grid olay şeması](#tab/event-grid-event-schema)

```json
{
    "id": "7c5d6de5-eb70-4de2-b788-c52a544e68b8",
    "topic": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "eventType": "Microsoft.Web.SlotSwapStarted",
    "eventTime": "2020-01-28T18:26:51.7194887Z",
    "data": {
        "appEventTypeDetail": null,
        "name": "<site-name>",
        "clientRequestId": "922f4841-20d9-4dd6-8c5b-23f0d85e5592",
        "correlationRequestId": "9ac46505-2b8a-4e06-834c-05ffbe2e8c3a",
        "requestId": "765117aa-eaf8-4bd2-a644-1dbf69c7b0fd",
        "address": "/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/slots?Command=SWAP&targetSlot=production",
        "verb": "POST",
        "sourceSlot": "staging",
        "targetSlot": "production"
    },
    "dataVersion": "1",
    "metaDataVersion": "1"
}
```

# <a name="cloud-event-schema"></a>[Bulut olay şeması](#tab/cloud-event-schema)

```json
{
    "id": "7c5d6de5-eb70-4de2-b788-c52a544e68b8",
    "source": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "type": "Microsoft.Web.SlotSwapStarted",
    "time": "2020-01-28T18:26:51.7194887Z",
    "data": {
        "appEventTypeDetail": null,
        "name": "<site-name>",
        "clientRequestId": "922f4841-20d9-4dd6-8c5b-23f0d85e5592",
        "correlationRequestId": "9ac46505-2b8a-4e06-834c-05ffbe2e8c3a",
        "requestId": "765117aa-eaf8-4bd2-a644-1dbf69c7b0fd",
        "address": "/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/slots?Command=SWAP&targetSlot=production",
        "verb": "POST",
        "sourceSlot": "staging",
        "targetSlot": "production"
    },
    "specversion": "1.0"
}
```

---

Veri nesnesi aşağıdaki özellikleri içerir:

|    Özellik                |    Tür      |    Açıklama                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    `appEventTypeDetail`      |    object    |    Uygulamadaki eylemin ayrıntıları                                                                                       |
|    `action`                 |    string    |    İşlem eyleminin türü                                                                                   |
|    `name`                    |    string    |    Bu olaya sahip olan Web sitesinin adı                                                                          |
|    `clientRequestId`         |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istemci istek KIMLIĞI         |
|    `correlationRequestId`    |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan bağıntı istek KIMLIĞI    |
|   `requestId`               |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istek KIMLIĞI                |
|    `address`                 |    string    |    Bu işlemin HTTP isteği URL 'SI                                                                                |
|    `verb`                    |    string    |    Bu işlemin HTTP fiili                                                                                       |
|    `sourceSlot`              |    string    |    Değiştirmenin kaynak yuvası                                                                                       |

### <a name="slotswapwithpreviewstarted-slotswapwithpreviewcancelled"></a>Slotswapwithönizleme başlatıldı, Slotswapwithönizleme Iptal edildi

# <a name="event-grid-event-schema"></a>[Event Grid olay şeması](#tab/event-grid-event-schema)

```json
{
    "id": "7c5d6de5-eb70-4de2-b788-c52a544e68b8",
    "topic": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "eventType": "Microsoft.Web.SlotSwapWithPreviewStarted",
    "eventTime": "2020-01-28T18:26:51.7194887Z",
    "data": {
        "appEventTypeDetail": null,
        "name": "<site-name>",
        "clientRequestId": "922f4841-20d9-4dd6-8c5b-23f0d85e5592",
        "correlationRequestId": "9ac46505-2b8a-4e06-834c-05ffbe2e8c3a",
        "requestId": "765117aa-eaf8-4bd2-a644-1dbf69c7b0fd",
        "address": "/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/slots?Command=SWAP&targetSlot=production",
        "verb": "POST",
        "sourceSlot": "staging",
        "targetSlot": "production"
    },
    "dataVersion": "1",
    "metaDataVersion": "1"
}
```

# <a name="cloud-event-schema"></a>[Bulut olay şeması](#tab/cloud-event-schema)

```json
{
    "id": "7c5d6de5-eb70-4de2-b788-c52a544e68b8",
    "source": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "type": "Microsoft.Web.SlotSwapWithPreviewStarted",
    "time": "2020-01-28T18:26:51.7194887Z",
    "data": {
        "appEventTypeDetail": null,
        "name": "<site-name>",
        "clientRequestId": "922f4841-20d9-4dd6-8c5b-23f0d85e5592",
        "correlationRequestId": "9ac46505-2b8a-4e06-834c-05ffbe2e8c3a",
        "requestId": "765117aa-eaf8-4bd2-a644-1dbf69c7b0fd",
        "address": "/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/slots?Command=SWAP&targetSlot=production",
        "verb": "POST",
        "sourceSlot": "staging",
        "targetSlot": "production"
    },
    "specversion": "1.0"
}
```

---

Veri nesnesi aşağıdaki özellikleri içerir:

|    Özellik                |    Tür      |    Açıklama                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    `appEventTypeDetail`      |    object    |    Uygulamadaki eylemin ayrıntıları                                                                                       |
|    `action`                 |    string    |    İşlem eyleminin türü                                                                                   |
|    `name`                    |    string    |    Bu olaya sahip olan Web sitesinin adı                                                                          |
|    `clientRequestId`         |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istemci istek KIMLIĞI         |
|    `correlationRequestId`    |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan bağıntı istek KIMLIĞI    |
|    `requestId`               |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istek KIMLIĞI                |
|    `address`                 |    string    |    Bu işlemin HTTP isteği URL 'SI                                                                                |
|    `verb`                    |    string    |    Bu işlemin HTTP fiili                                                                                       |

### <a name="appupdatedrestarted-appupdatedstopped-appupdatedchangedappsettings"></a>AppUpdated. yeniden başlatıldı, AppUpdated. durduruldu, AppUpdated. ChangedAppSettings

# <a name="event-grid-event-schema"></a>[Event Grid olay şeması](#tab/event-grid-event-schema)

```json
{
    "id": "b74ea56b-2a3f-4de5-a5d7-38e60c81cf23",
    "topic": "/subscriptions/<id>/resourceGroups/<group>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "eventType": "Microsoft.Web.AppUpdated",
    "eventTime": "2020-01-28T18:22:30.2760952Z",
    "data": {
        "appEventTypeDetail": {
            "action": "Stopped"
        },
        "name": "<site-name>",
        "clientRequestId": "64a5e0aa-7cee-4ff1-9093-b9197b820014",
        "correlationRequestId": "25bb36a5-8f6c-4f04-b615-e9a0ee045756",
        "requestId": "f2e8eb3f-b190-42de-b99e-6acefe587374",
        "address": "/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/stop",
        "verb": "POST"
    },
    "dataVersion": "1'",
    "metaDataVersion": "1"
}
```

# <a name="cloud-event-schema"></a>[Bulut olay şeması](#tab/cloud-event-schema)

```json
{
    "id": "b74ea56b-2a3f-4de5-a5d7-38e60c81cf23",
    "source": "/subscriptions/<id>/resourceGroups/<group>/providers/Microsoft.Web/sites/<site-name>",
    "subject": "/Microsoft.Web/sites/<site-name>",
    "type": "Microsoft.Web.AppUpdated",
    "time": "2020-01-28T18:22:30.2760952Z",
    "data": {
        "appEventTypeDetail": {
            "action": "Stopped"
        },
        "name": "<site-name>",
        "clientRequestId": "64a5e0aa-7cee-4ff1-9093-b9197b820014",
        "correlationRequestId": "25bb36a5-8f6c-4f04-b615-e9a0ee045756",
        "requestId": "f2e8eb3f-b190-42de-b99e-6acefe587374",
        "address": "/websystems/WebSites/web/subscriptions/<id>/webspaces/<webspace>/sites/<site-name>/stop",
        "verb": "POST"
    },
    "specversion": "1.0"
}
```

---

Veri nesnesi aşağıdaki özelliklere sahiptir:

|    Özellik                |    Tür      |    Açıklama                                                                                                       |
|----------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    `appEventTypeDetail`      |    object    |    Uygulamadaki eylemin ayrıntıları                                                                                       |
|    `action`                  |    string    |    İşlem eyleminin türü                                                                                   |
|    `name`                    |    string    |    Bu olaya sahip olan Web sitesinin adı                                                                          |
|    `clientRequestId`         |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istemci istek KIMLIĞI         |
|    `correlationRequestId`    |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan bağıntı istek KIMLIĞI    |
|    `requestId`               |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istek KIMLIĞI                |
|    `address`                 |    string    |    Bu işlemin HTTP isteği URL 'SI                                                                                |
|    `verb`                    |    string    |    Bu işlemin HTTP fiili                                                                                       |

### <a name="serverfarmsappserviceplanupdated"></a>Sunucugrupları. AppServicePlanUpdated

# <a name="event-grid-event-schema"></a>[Event Grid olay şeması](#tab/event-grid-event-schema)

```json
{
    "id": "56501672-9150-40e1-893a-18420c7fdbf7",
    "topic": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/serverfarms/<serverfarm-name>",
    "subject": "/Microsoft.Web/serverfarms/<plan-name>",
    "eventType": "Microsoft.Web.AppServicePlanUpdated",
    "eventTime": "2020-01-28T18:22:23.5516004Z",
    "data": {
        "serverFarmEventTypeDetail": {
            "stampKind": "Public",
            "action": "Updated",
            "status": "Started"
        },
        "serverFarmId": "0",
        "sku": {
            "name": "P1v2",
            "tier": "PremiumV2",
            "size": "P1v2",
            "family": "Pv2",
            "capacity": 1
        },
        "clientRequestId": "8f880321-a991-45c7-b743-6ff63fe4c004",
        "correlationRequestId": "1995c3be-ba7f-4ccf-94af-516df637ec8a",
        "requestId": "b973a8e6-6949-4783-b44c-ac778be831bb",
        "address": "/websystems/WebSites/serverfarms/subscriptions/<id>/webspaces/<webspace-id>/serverfarms/<plan-name>/async",
        "verb": "PUT"
    },
    "dataVersion": "1",
    "metaDataVersion": "1"
}
```

# <a name="cloud-event-schema"></a>[Bulut olay şeması](#tab/cloud-event-schema)

```json
{
    "id": "56501672-9150-40e1-893a-18420c7fdbf7",
    "source": "/subscriptions/<id>/resourceGroups/<rg>/providers/Microsoft.Web/serverfarms/<serverfarm-name>",
    "subject": "/Microsoft.Web/serverfarms/<plan-name>",
    "type": "Microsoft.Web.AppServicePlanUpdated",
    "time": "2020-01-28T18:22:23.5516004Z",
    "data": {
        "serverFarmEventTypeDetail": {
            "stampKind": "Public",
            "action": "Updated",
            "status": "Started"
        },
        "serverFarmId": "0",
        "sku": {
            "name": "P1v2",
            "tier": "PremiumV2",
            "size": "P1v2",
            "family": "Pv2",
            "capacity": 1
        },
        "clientRequestId": "8f880321-a991-45c7-b743-6ff63fe4c004",
        "correlationRequestId": "1995c3be-ba7f-4ccf-94af-516df637ec8a",
        "requestId": "b973a8e6-6949-4783-b44c-ac778be831bb",
        "address": "/websystems/WebSites/serverfarms/subscriptions/<id>/webspaces/<webspace-id>/serverfarms/<plan-name>/async",
        "verb": "PUT"
    },
    "specversion": "1.0"
}
```

---

Veri nesnesi aşağıdaki özelliklere sahiptir:

|    Özellik                         |    Tür      |    Açıklama                                                                                                       |
|-------------------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|
|    `appServicePlanEventTypeDetail`    |    object    |    App Service planındaki eylemin ayrıntıları                                                                          |
|    `stampKind`                        |    string    |    App Service planının bulunduğu ortam türü                                                                     |
|    `action`                           |    string    |    App Service planındaki eylemin türü                                                                            |
|    `status`                           |    string    |    Uygulama hizmeti planındaki işlemin durumu                                                                   |
|    `sku`                              |    object    |    App Service planının SKU 'su                                                                                       |
|    `name`                             |    string    |    App Service planının adı                                                                                      |
|    `Tier`                             |    string    |    App Service planının katmanı                                                                                      |
|    `Size`                             |    string    |    App Service planının boyutu                                                                                      |
|    `Family`                           |    string    |    uygulama hizmeti planı ailesi                                                                                        |
|    `Capacity`                         |    string    |    App Service planının kapasitesi                                                                                      |
|    `action`                           |    string    |    İşlem eyleminin türü                                                                                   |
|    `name`                             |    string    |    Bu olaya sahip olan Web sitesinin adı                                                                          |
|    `clientRequestId`                  |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istemci istek KIMLIĞI         |
|    `correlationRequestId`             |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan bağıntı istek KIMLIĞI    |
|    `requestId`                        |    string    |    Bu olayı tetikleyen site API 'SI işlemi için App Service tarafından oluşturulan istek KIMLIĞI                |
|    `address`                         |    string    |    Bu işlemin HTTP isteği URL 'SI                                                                                |
|    `verb`                             |    string    |    Bu işlemin HTTP fiili                                                                                       |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md)