---
title: Azure Işlevleri için Event Grid tetikleyicisi
description: Azure Işlevlerinde Event Grid olaylarının nasıl işleneceğini anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 09/04/2018
ms.author: cshoe
ms.openlocfilehash: 8062428ae63a572b81a5432c8b29910fe8422e24
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76547464"
---
# <a name="event-grid-trigger-for-azure-functions"></a>Azure Işlevleri için Event Grid tetikleyicisi

Bu makalede, Azure Işlevlerinde [Event Grid](../event-grid/overview.md) olaylarının nasıl işleneceği açıklanır. HTTP uç noktasındaki Event Grid iletilerinin nasıl işleneceği hakkında ayrıntılar için, [http uç noktasına alma olaylarını](../event-grid/receive-events.md)okuyun.

Event Grid, *yayımcılardan*oluşan olayları BILDIRMEK üzere http istekleri gönderen bir Azure hizmetidir. Yayımcı, olayı oluşturan hizmet veya kaynaktır. Örneğin, bir Azure Blob depolama hesabı bir yayımcı, [BLOB karşıya yükleme veya silme bir olaydır](../storage/blobs/storage-blob-event-overview.md). Bazı [Azure hizmetlerinde Event Grid olay yayımlamaya yönelik yerleşik destek vardır](../event-grid/overview.md#event-sources).

Olay *işleyicileri* olayları alır ve işler. Azure Işlevleri, [Event Grid olaylarını işlemek için yerleşik desteğe sahip çeşitli Azure hizmetlerinden](../event-grid/overview.md#event-handlers)biridir. Bu makalede, Event Grid bir olay alındığında bir işlevi çağırmak için Event Grid tetikleyicisini nasıl kullanacağınızı öğreneceksiniz.

İsterseniz, Event Grid olaylarını işlemek için bir HTTP tetikleyicisi kullanabilirsiniz; bkz. [http uç noktasına olay alma](../event-grid/receive-events.md). Şu anda, olay [Cloudevents şemasında](../event-grid/cloudevents-schema.md#azure-functions)teslim edildiğinde Azure işlevleri uygulaması için Event Grid tetikleyicisi kullanamazsınız. Bunun yerine, bir HTTP tetikleyicisi kullanın.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

Event Grid tetikleyicisi [Microsoft. Azure. WebJobs. Extensions. EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet paketi, sürüm 2. x ' te sunulmaktadır. Paketin kaynak kodu, [Azure-Functions-eventgrid-Extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/v2.x) GitHub deposunda bulunur.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Event Grid tetikleyicisi [Microsoft. Azure. WebJobs. Extensions. EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet paketi, sürüm 1. x ' te sunulmaktadır. Paketin kaynak kodu, [Azure-Functions-eventgrid-Extension](https://github.com/Azure/azure-functions-eventgrid-extension/tree/master) GitHub deposunda bulunur.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>Örnek

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

HTTP tetikleyicisi örneği için bkz. [http uç noktasına olay alma](../event-grid/receive-events.md).

### <a name="c-2x-and-higher"></a>C#(2. x ve üzeri)

Aşağıdaki örnek, `EventGridEvent`bağlanan bir [ C# işlevi](functions-dotnet-class-library.md) gösterir:

```cs
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
        public static void EventGridTest([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

Daha fazla bilgi için bkz. paketler, [öznitelikler](#attributes), [yapılandırma](#configuration)ve [kullanım](#usage).

### <a name="version-1x"></a>Sürüm 1. x

Aşağıdaki örnek, `JObject`bağlanan bir işlevler 1. x [ C# işlevi](functions-dotnet-class-library.md) gösterir:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir tetikleyici bağlamayı ve bağlamayı kullanan bir [ C# betik işlevini](functions-reference-csharp.md) gösterir.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-script-version-2x-and-higher"></a>C#betik (sürüm 2. x ve üzeri)

`EventGridEvent`bağlanan bir örnek aşağıda verilmiştir:

```csharp
#r "Microsoft.Azure.EventGrid"
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(EventGridEvent eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.Data.ToString());
}
```

Daha fazla bilgi için bkz. paketler, [öznitelikler](#attributes), [yapılandırma](#configuration)ve [kullanım](#usage).

### <a name="version-1x"></a>Sürüm 1. x

`JObject`'e bağlanan 1. x C# komut dosyası kodu aşağıda verilmiştir:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir tetikleyici bağlamayı ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```

# <a name="pythontabpython"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function. JSON* dosyasındaki bir tetikleyici bağlamayı ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "event",
      "direction": "in"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Python kodu aşağıda verilmiştir:

```python
import json
import logging

import azure.functions as func

def main(event: func.EventGridEvent):

    result = json.dumps({
        'id': event.id,
        'data': event.get_json(),
        'topic': event.topic,
        'subject': event.subject,
        'event_type': event.event_type,
    })

    logging.info('Python EventGrid trigger processed an event: %s', result)
```

# <a name="javatabjava"></a>[Java](#tab/java)

Bu bölüm aşağıdaki örnekleri içerir:

* [Event Grid tetikleyicisi, dize parametresi](#event-grid-trigger-string-parameter)
* [Event Grid tetikleyicisi, POJO parametresi](#event-grid-trigger-pojo-parameter)

Aşağıdaki örneklerde, önce olayı bir POJO olarak ```String``` ve ikinci olarak alarak, bir olayı ve bir olayı yazdırmayı kullanan bir *function. JSON* dosyası ve [Java işlevlerinde](functions-reference-java.md) tetikleyici bağlama gösterilmektedir.

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ]
}
```

### <a name="event-grid-trigger-string-parameter"></a>Event Grid tetikleyicisi, dize parametresi

```java
  @FunctionName("eventGridMonitorString")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    String content, 
    final ExecutionContext context) {
      context.getLogger().info("Event content: " + content);      
  }
```

### <a name="event-grid-trigger-pojo-parameter"></a>Event Grid tetikleyicisi, POJO parametresi

Bu örnek, bir Event Grid olayının en üst düzey özelliklerini temsil eden aşağıdaki POJO 'ları kullanır:

```java
import java.util.Date;
import java.util.Map;

public class EventSchema {

  public String topic;
  public String subject;
  public String eventType;
  public Date eventTime;
  public String id;
  public String dataVersion;
  public String metadataVersion;
  public Map<String, Object> data;

}
```

Varış sonrasında, olayın JSON yükü, işlevin kullanması için ```EventSchema``` POJO içine de serileştirilir. Bu, işlevin, nesne yönelimli bir şekilde olay özelliklerine erişmesini sağlar.

```java
  @FunctionName("eventGridMonitor")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    EventSchema event, 
    final ExecutionContext context) {
      context.getLogger().info("Event content: ");
      context.getLogger().info("Subject: " + event.subject);
      context.getLogger().info("Time: " + event.eventTime); // automatically converted to Date by the runtime
      context.getLogger().info("Id: " + event.id);
      context.getLogger().info("Data: " + event.data);
  }
```

[Java işlevleri çalışma zamanı kitaplığı](/java/api/overview/azure/functions/runtime)'nda, değeri eventgrid 'den gelen parametrelerde `EventGridTrigger` ek açıklamasını kullanın. Bu ek açıklamaların bulunduğu parametreler, bir olay geldiğinde işlevin çalışmasına neden olur.  Bu ek açıklama `Optional<T>`kullanılarak yerel Java türleri, POJOs veya null atanabilir değerlerle kullanılabilir.

---

## <a name="attributes"></a>Öznitelikler

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

[ C# Sınıf kitaplıkları](functions-dotnet-class-library.md)' nda [eventgridtrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/TriggerBinding/EventGridTriggerAttribute.cs) özniteliğini kullanın.

Yöntem imzasında bir `EventGridTrigger` özniteliği aşağıda verilmiştir:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, ILogger log)
{
    ...
}
```

Tüm örnek için bkz C# . örnek.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Öznitelikler komut dosyası tarafından C# desteklenmiyor.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="pythontabpython"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="javatabjava"></a>[Java](#tab/java)

[Eventgridtrigger](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/EventGridTrigger.java) ek açıklaması, yapılandırma değerlerini sağlayarak bir Event Grid bağlamasını bildirimli olarak yapılandırmanızı sağlar. Daha ayrıntılı bilgi için bkz. [örnek](#example) ve [yapılandırma](#configuration) bölümleri.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır. `EventGridTrigger` özniteliğinde ayarlanacak bir oluşturucu parametresi veya özellik yok.

|Function.JSON özelliği |Açıklama|
|---------|---------|
| **type** | Gerekli-`eventGridTrigger`olarak ayarlanmalıdır. |
| **direction** | Gerekli-`in`olarak ayarlanmalıdır. |
| **Adı** | Gerekli-olay verilerini alan parametre için işlev kodunda kullanılan değişken adı. |

## <a name="usage"></a>Kullanım

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Azure Işlevleri 1. x içinde Event Grid tetikleyicisi için aşağıdaki parametre türlerini kullanabilirsiniz:

* `JObject`
* `string`

Azure Işlevleri 2. x ve üzeri sürümlerde, Event Grid tetikleyicisi için aşağıdaki parametre türünü kullanma seçeneğiniz de vardır:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent`-tüm olay türlerinde ortak olan alanların özelliklerini tanımlar.

> [!NOTE]
> Işlev v1 ' de `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`bağlamaya çalışırsanız, derleyici "kullanım dışı" iletisini görüntüler ve bunun yerine `Microsoft.Azure.EventGrid.Models.EventGridEvent` kullanmanızı sağlar. Daha yeni türü kullanmak için [Microsoft. Azure. EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet paketine başvurun ve `EventGridEvent` türü adını `Microsoft.Azure.EventGrid.Models`ile önek olarak çağırarak tam olarak nitelendirin.

# <a name="c-scripttabcsharp-script"></a>[C#SCRIPT](#tab/csharp-script)

Azure Işlevleri 1. x içinde Event Grid tetikleyicisi için aşağıdaki parametre türlerini kullanabilirsiniz:

* `JObject`
* `string`

Azure Işlevleri 2. x ve üzeri sürümlerde, Event Grid tetikleyicisi için aşağıdaki parametre türünü kullanma seçeneğiniz de vardır:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent`-tüm olay türlerinde ortak olan alanların özelliklerini tanımlar.

> [!NOTE]
> Işlev v1 ' de `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`bağlamaya çalışırsanız, derleyici "kullanım dışı" iletisini görüntüler ve bunun yerine `Microsoft.Azure.EventGrid.Models.EventGridEvent` kullanmanızı sağlar. Daha yeni türü kullanmak için [Microsoft. Azure. EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet paketine başvurun ve `EventGridEvent` türü adını `Microsoft.Azure.EventGrid.Models`ile önek olarak çağırarak tam olarak nitelendirin. Bir C# betik işlevindeki NuGet paketlerine başvurma hakkında daha fazla bilgi için bkz. [NuGet paketlerini kullanma](functions-reference-csharp.md#using-nuget-packages)

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Event Grid örneği, *function. JSON* dosyasının `name` özelliğinde yapılandırılan parametre aracılığıyla kullanılabilir.

# <a name="pythontabpython"></a>[Python](#tab/python)

Event Grid örneği, *function. JSON* dosyasının `name` özelliğinde yapılandırılmış `func.EventGridEvent`olarak yazılmış parametre aracılığıyla kullanılabilir.

# <a name="javatabjava"></a>[Java](#tab/java)

Event Grid olay örneği, bir `EventSchema`olarak yazılmış `EventGridTrigger` özniteliğiyle ilişkili parametre aracılığıyla kullanılabilir. Daha fazla ayrıntı için [örneğe](#example) bakın.

---

## <a name="event-schema"></a>Olay şeması

Bir Event Grid olayının verileri bir HTTP isteği gövdesinde JSON nesnesi olarak alınır. JSON aşağıdaki örneğe benzer şekilde görünür:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Gösterilen örnek bir öğe dizisidir. Event Grid her zaman bir dizi gönderir ve dizide birden fazla olay gönderebilir. Çalışma zamanı, her dizi öğesi için işlevinizi bir kez çağırır.

Olay JSON verilerinde en üst düzey özellikler tüm olay türleri arasında aynıdır, ancak `data` özelliğinin içeriği her bir olay türüne özeldir. Gösterilen örnek bir BLOB depolama olayı içindir.

Ortak ve olaya özgü özelliklerin açıklamaları için Event Grid belgelerindeki [olay özellikleri](../event-grid/event-schema.md#event-properties) ' ne bakın.

`EventGridEvent` türü yalnızca en üst düzey özellikleri tanımlar; `Data` özelliği bir `JObject`.

## <a name="create-a-subscription"></a>Abonelik oluşturma

Event Grid HTTP isteklerini almaya başlamak için, işlevi çağıran uç nokta URL 'sini belirten bir Event Grid aboneliği oluşturun.

### <a name="azure-portal"></a>Azure portalında

Event Grid tetikleyicisiyle Azure portal geliştirdiğiniz işlevlerde **Event Grid aboneliği Ekle**' yi seçin.

![Portalda abonelik oluşturma](media/functions-bindings-event-grid/portal-sub-create.png)

Bu bağlantıyı seçtiğinizde, Portal Endpoint URL 'SI önceden doldurulmuş olarak **olay aboneliği oluştur** sayfasını açar.

![Endpoint URL önceden dolduruldu](media/functions-bindings-event-grid/endpoint-url.png)

Azure portal kullanarak abonelikler oluşturma hakkında daha fazla bilgi için Event Grid belgelerinde [özel olay oluşturma-Azure Portal](../event-grid/custom-event-quickstart-portal.md) konusuna bakın.

### <a name="azure-cli"></a>Azure CLI

[Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)kullanarak bir abonelik oluşturmak için [az eventgrid olay-abonelik oluştur](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-create) komutunu kullanın.

Komut, işlevi çağıran uç nokta URL 'sini gerektirir. Aşağıdaki örnekte, sürüme özgü URL kalıbı gösterilmektedir:

#### <a name="version-2x-and-higher-runtime"></a>Sürüm 2. x (ve üzeri) çalışma zamanı

    https://{functionappname}.azurewebsites.net/runtime/webhooks/eventgrid?functionName={functionname}&code={systemkey}

#### <a name="version-1x-runtime"></a>Sürüm 1. x çalışma zamanı

    https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}

Sistem anahtarı, Event Grid tetikleyicisi için uç nokta URL 'sine dahil olması gereken bir yetkilendirme anahtarıdır. Aşağıdaki bölümde Sistem anahtarının nasıl alınacağı açıklanmaktadır.

Bir BLOB depolama hesabına abone olan bir örnek (sistem anahtarı için bir yer tutucu ile) aşağıda verilmiştir:

#### <a name="version-2x-and-higher-runtime"></a>Sürüm 2. x (ve üzeri) çalışma zamanı

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/runtime/webhooks/eventgrid?functionName=imageresizefunc&code=<key>
```

#### <a name="version-1x-runtime"></a>Sürüm 1. x çalışma zamanı

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=<key>
```

Abonelik oluşturma hakkında daha fazla bilgi için bkz. [BLOB Storage hızlı](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) başlangıcı veya diğer Event Grid hızlı başlangıçlar.

### <a name="get-the-system-key"></a>Sistem anahtarını al

Aşağıdaki API 'YI kullanarak sistem anahtarını alabilirsiniz (HTTP GET):

#### <a name="version-2x-and-higher-runtime"></a>Sürüm 2. x (ve üzeri) çalışma zamanı

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgrid_extension?code={masterkey}
```

#### <a name="version-1x-runtime"></a>Sürüm 1. x çalışma zamanı

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={masterkey}
```

Bu bir yönetim API 'sidir, bu nedenle işlev uygulama [ana anahtarınızı](functions-bindings-http-webhook.md#authorization-keys)gerektirir. Ana anahtarla (bir Event Grid tetikleyici işlevi çağırmak için) sistem anahtarını karıştırmayın (işlev uygulamasında yönetim görevleri gerçekleştirmek için). Bir Event Grid konusuna abone olduğunuzda, sistem anahtarını kullandığınızdan emin olun.

Aşağıda sistem anahtarını sağlayan yanıta bir örnek verilmiştir:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

İşlev uygulamanızın ana anahtarını portalda **işlev uygulaması ayarları** sekmesinden alabilirsiniz.

> [!IMPORTANT]
> Ana anahtar, işlev uygulamanıza yönetici erişimi sağlar. Bu anahtarı üçüncü taraflarla paylaşmayın veya yerel istemci uygulamalarında dağıtamazsınız.

Daha fazla bilgi için HTTP tetikleyici başvurusu makalesindeki [Yetkilendirme anahtarları](functions-bindings-http-webhook.md#authorization-keys) bölümüne bakın.

Alternatif olarak, anahtar değerini kendiniz belirtmek için bir HTTP PUT da gönderebilirsiniz.

## <a name="local-testing-with-viewer-web-app"></a>Viewer Web App ile yerel test

Event Grid bir tetikleyiciyi yerel olarak test etmek için, buluttaki kaynağından yerel makinenize teslim edilen Event Grid HTTP isteklerini almanız gerekir. Bunu yapmanın bir yolu, istekleri çevrimiçi olarak yakalayıp yerel makinenizde el ile yeniden göndermeyi kullanmaktır:

1. Olay iletilerini yakalayan [bir Görüntüleyici Web uygulaması oluşturun](#create-a-viewer-web-app) .
1. Görüntüleyici uygulamasına olayları Gönderen [Event Grid bir abonelik oluşturun](#create-an-event-grid-subscription) .
1. [Bir Istek oluşturun](#generate-a-request) ve Görüntüleyici uygulamasından istek gövdesini kopyalayın.
1. İsteği Event Grid tetikleyici işlevinizin localhost URL 'sine [el ile gönderin](#manually-post-the-request) .

Testi tamamladıktan sonra, uç noktayı güncelleştirerek üretim için aynı aboneliği kullanabilirsiniz. [Az eventgrid olay-abonelik güncelleştirme](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update) Azure CLI komutunu kullanın.

### <a name="create-a-viewer-web-app"></a>Görüntüleyiciye Web uygulaması oluşturma

Olay iletilerini yakalamayı basitleştirmek için, olay iletilerini görüntüleyen [önceden oluşturulmuş bir Web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtabilirsiniz. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

Siteyi görürsünüz ancak henüz yayımlanmış olay yoktur.

![Yeni siteyi görüntüleme](media/functions-bindings-event-grid/view-site.png)

### <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturma

Test etmek istediğiniz türün Event Grid aboneliğini oluşturun ve olay bildirimi için uç nokta olarak Web uygulamanızdan URL 'YI verin. Web uygulamanızın uç noktası `/api/updates/` sonekini içermelidir. Bu nedenle, tam URL `https://<your-site-name>.azurewebsites.net/api/updates`

Azure portal kullanarak abonelikler oluşturma hakkında daha fazla bilgi için, bkz. Event Grid belgelerinde [özel olay oluşturma-Azure Portal](../event-grid/custom-event-quickstart-portal.md) .

### <a name="generate-a-request"></a>İstek oluştur

Web uygulaması uç noktanıza HTTP trafiği oluşturacak bir olay tetikleyin.  Örneğin, bir BLOB depolama aboneliği oluşturduysanız, blob yükleyin veya silin. Web uygulamanızda bir istek görüntülendiğinde, istek gövdesini kopyalayın.

Abonelik doğrulama isteği ilk olarak alınır; tüm doğrulama isteklerini yoksayın ve olay isteğini kopyalayın.

![Web uygulamasından istek gövdesini Kopyala](media/functions-bindings-event-grid/view-results.png)

### <a name="manually-post-the-request"></a>İsteği el ile gönderin

Event Grid işlevinizi yerel olarak çalıştırın.

HTTP POST isteği oluşturmak için [Postman](https://www.getpostman.com/) veya [kıvrımlı](https://curl.haxx.se/docs/httpscripting.html) gibi bir araç kullanın:

* `Content-Type: application/json` üst bilgisi ayarlayın.
* `aeg-event-type: Notification` üst bilgisi ayarlayın.
* RequestBin verilerini istek gövdesine yapıştırın.
* Event Grid tetikleyici işlevinizin URL 'sine gönderin.
  * 2\. x ve üzeri için aşağıdaki kalıbı kullanın:

    ```
    http://localhost:7071/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
    ```

  * 1\. x kullanımı için:

    ```
    http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
    ```

`functionName` parametresi `FunctionName` özniteliğinde belirtilen ad olmalıdır.

Aşağıdaki ekran görüntülerinde, Postman 'daki üstbilgiler ve istek gövdesi gösterilmektedir:

![Postman 'daki üstbilgiler](media/functions-bindings-event-grid/postman2.png)

![Postman 'da istek gövdesi](media/functions-bindings-event-grid/postman.png)

Event Grid tetikleyici işlevi yürütülür ve aşağıdaki örneğe benzer Günlükler gösterir:

![Örnek Event Grid tetikleyici işlev günlükleri](media/functions-bindings-event-grid/eg-output.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Event Grid hakkında daha fazla bilgi edinin](../event-grid/overview.md)
