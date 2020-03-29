---
title: Azure Olay Izgarasından http bitiş noktasına olayları alma
description: BIR HTTP bitiş noktasını nasıl doğrulayasınız, ardından Azure Olay Idamı'ndan Etkinlikleri nasıl alacağıve deserialize edin
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: babanisa
ms.openlocfilehash: cb38fd17c0c1bfbe3e5957d8f432f0a43b285c93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60803767"
---
# <a name="receive-events-to-an-http-endpoint"></a>HTTP uç noktasına olayları alma

Bu makalede, olay aboneliğinden olayları almak ve sonra olayları almak ve deserialize etmek için [bir HTTP bitiş noktasının](security-authentication.md#webhook-event-delivery) nasıl doğrulayacakları açıklanmaktadır. Bu makalede, gösterim amacıyla bir Azure İşlevi kullanılır, ancak uygulamanın nerede barındırıldığına bakılmaksızın aynı kavramlar geçerlidir.

> [!NOTE]
> Olay Izgaralı bir Azure İşi'ni tetiklerken [bir Olay Izgara Tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) kullanmanız **önerilir.** Burada genel bir WebHook tetikleyici kullanımı gösterici.

## <a name="prerequisites"></a>Ön koşullar

BIR HTTP tetiklenen işlevi olan bir işlev uygulaması gerekir.

## <a name="add-dependencies"></a>Bağımlılıkekleme

.NET'te geliştiriyorsanız, `Microsoft.Azure.EventGrid` [Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)için işlevinize [bir bağımlılık ekleyin.](../azure-functions/functions-reference-csharp.md#referencing-custom-assemblies) Bu makaledeki örnekler sürüm 1.4.0 veya daha sonra gerektirir.

Diğer diller için [SDK'lar SDK'lar](./sdk-overview.md#data-plane-sdks) yayımlama başvurusu aracılığıyla kullanılabilir. Bu paketler gibi `EventGridEvent`yerel olay türleri `StorageBlobCreatedEventData`için `EventHubCaptureFileCreatedEventData`modeller var , , ve .

Azure İşlevinizdeki "Dosyaları Görüntüle" bağlantısına tıklayın (Azure işlevleri portalındaki en çok bölme) ve project.json adlı bir dosya oluşturun. Aşağıdaki içeriği dosyaya `project.json` ekleyin ve kaydedin:

 ```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.Azure.EventGrid": "2.0.0"
      }
    }
   }
}
```

![NuGet paketi eklendi](./media/receive-events/add-dependencies.png)

## <a name="endpoint-validation"></a>Bitiş noktası doğrulaması

Yapmak istediğiniz ilk şey olayları `Microsoft.EventGrid.SubscriptionValidationEvent` ele almaktır. Bir kişi bir etkinliğe her abone olduğunda, Olay Izgarası veri yükünde bir `validationCode` doğrulama olayıyla bitiş noktasına gönderir. Bitiş noktası, [bitiş noktasının geçerli olduğunu ve size ait olduğunu kanıtlamak](security-authentication.md#webhook-event-delivery)için yanıt gövdesinde bunu yankılamak için gereklidir. WebHook tetiklenen İşlev yerine [Olay Izgara Tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) kullanıyorsanız, uç nokta doğrulaması sizin için işlenir. Bir üçüncü taraf API hizmeti [(Zapier](https://zapier.com) veya [IFTTT](https://ifttt.com/)gibi) kullanıyorsanız, doğrulama kodunu programlı bir şekilde yansıtamayabilirsiniz. Bu hizmetler için, abonelik doğrulama olayında gönderilen bir doğrulama URL'sini kullanarak aboneliği el ile doğrulayabilirsiniz. Tesisteki url'yi kopyalayın `validationUrl` ve bir REST istemcisi veya web tarayıcınız aracılığıyla bir GET isteği gönderin.

C#'da `DeserializeEventGridEvents()` işlev Olay Izgara olaylarını deserialize eder. Olay verilerini StorageBlobCreatedEventData gibi uygun türe dönüştürür. Desteklenen `Microsoft.Azure.EventGrid.EventTypes` olay türlerini ve adlarını almak için sınıfı kullanın.

Doğrulama kodunu programlı bir şekilde yankılamak için aşağıdaki kodu kullanın. İlgili örnekleri Event [Grid Consumer örneğinde](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/tree/master/EventGridConsumer)bulabilirsiniz.

```csharp
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();

    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }
    }
    context.done();
};
```

### <a name="test-validation-response"></a>Test doğrulama yanıtı

Çözümlü olay, işlev için test alanına yapıştırarak doğrulama yanıtı işlevini test edin:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Çalıştır'ı tıklattığınızda, Çıktı 200 `{"ValidationResponse":"512d38b6-c7b8-40c8-89fe-f46f9e9622b6"}` Tamam ve gövdede olmalıdır:

![doğrulama yanıtı](./media/receive-events/validation-response.png)

## <a name="handle-blob-storage-events"></a>Blob depolama olaylarını işleme

Şimdi, işlevi işlemek `Microsoft.Storage.BlobCreated`için genişletelim:

```cs
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();

    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
        else if (eventGridEvent.Data is StorageBlobCreatedEventData)
        {
            var eventData = (StorageBlobCreatedEventData)eventGridEvent.Data;
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type  
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }
    }
    context.done();
};

```

### <a name="test-blob-created-event-handling"></a>Test Blob Oluşturulan olay işleme

Test alanına bir [Blob depolama olayı](./event-schema-blob-storage.md#example-event) koyup çalıştırarak işlevin yeni işlevselliğini test edin:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

İşlev günlüğündeki blob URL çıktısını görmeniz gerekir:

![Çıkış günlüğü](./media/receive-events/blob-event-response.png)

Ayrıca bir Blob depolama hesabı veya Genel Amaçlı V2 (GPv2) Depolama hesabı [oluşturarak, ekleme ve olay aboneliği](../storage/blobs/storage-blob-event-quickstart.md)ve işlev URL'sinin bitiş noktasını ayarlayarak test edebilirsiniz:

![İşlev URL'si](./media/receive-events/function-url.png)

## <a name="handle-custom-events"></a>Özel olayları işleme

Son olarak, özel olayları da işleyebilmek için işlevi bir kez daha genişletin. 

C#'da, SDK olay veri türü türüne bir olay türü adı eşleme destekler. Özel `AddOrUpdateCustomEventMapping()` olayı eşlemek için işlevi kullanın.

Etkinliğiniz `Contoso.Items.ItemReceived`için bir çek ekleyin. Son kodunuz şu şekilde görünmelidir:

```cs
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

class ContosoItemReceivedEventData
{
    [JsonProperty(PropertyName = "itemSku")]
    public string ItemSku { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();
    eventGridSubscriber.AddOrUpdateCustomEventMapping("Contoso.Items.ItemReceived", typeof(ContosoItemReceivedEventData));
    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
        else if (eventGridEvent.Data is StorageBlobCreatedEventData)
        {
            var eventData = (StorageBlobCreatedEventData)eventGridEvent.Data;
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
        else if (eventGridEvent.Data is ContosoItemReceivedEventData)
        {
            var eventData = (ContosoItemReceivedEventData)eventGridEvent.Data;
            log.Info($"Got ContosoItemReceived event data, item SKU {eventData.ItemSku}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    var customEventType = "Contoso.Items.ItemReceived";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }

        else if (body.data && body.eventType == customEventType) {
            var payload = body.data;
            context.log("Relaying received custom payload:" + JSON.stringify(payload));
        }
    }
    context.done();
};
```

### <a name="test-custom-event-handling"></a>Özel olay işlemeyi test etme

Son olarak, işlevinizin artık özel olay türünü işleyebileni test edin:

```json
[{
    "subject": "Contoso/foo/bar/items",
    "eventType": "Contoso.Items.ItemReceived",
    "eventTime": "2017-08-16T01:57:26.005121Z",
    "id": "602a88ef-0001-00e6-1233-1646070610ea",
    "data": { 
            "itemSku": "Standard"
            },
    "dataVersion": "",
    "metadataVersion": "1"
}]
```

Ayrıca, [Portal'dan CURL ile özel bir etkinlik göndererek](./custom-event-quickstart-portal.md) veya [Postacı](https://www.getpostman.com/)gibi bir bitiş noktasına gönderebilen herhangi bir hizmeti veya uygulamayı kullanarak [özel bir konuya göndererek](./post-to-custom-topic.md) bu işlevselliği canlı olarak test edebilirsiniz. İşlev URL'si olarak bitiş noktası ayarlanmış özel bir konu ve etkinlik aboneliği oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* Azure [Olay Izgara Yönetimini keşfedin ve SDK'ları Yayımlayın](./sdk-overview.md)
* Özel bir konuya nasıl [yayınlayılamayı](./post-to-custom-topic.md) öğrenin
* Blob depolama alanına [yüklenen görüntüleri yeniden boyutlandırma](resize-images-on-storage-blob-upload-event.md) gibi ayrıntılı Olay Izgara ve Fonksiyonlar öğreticilerinden birini deneyin
