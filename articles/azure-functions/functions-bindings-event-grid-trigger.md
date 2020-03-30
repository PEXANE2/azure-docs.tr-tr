---
title: Azure İşlevler için Azure Olay Izgara tetikleyicisi
description: Azure İşlevlerinde Olay Izgara olayları gönderildiğinde kodu çalıştırmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.custom: fasttrack-edit
ms.openlocfilehash: 2027629e1e9e297c97cbf40485ebe7dc2e3e6c0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277732"
---
# <a name="azure-event-grid-trigger-for-azure-functions"></a>Azure İşlevler için Azure Olay Izgara tetikleyicisi

Olay Izgara konusuna gönderilen bir olaya yanıt vermek için işlev tetikleyicisini kullanın.

Kurulum ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [genel bakışa](./functions-bindings-event-grid.md)bakın.

## <a name="example"></a>Örnek

# <a name="c"></a>[C #](#tab/csharp)

BIR HTTP tetikleyici örneği için, [bkz.](../event-grid/receive-events.md)

### <a name="c-2x-and-higher"></a>C# (2.x ve üzeri)

Aşağıdaki örnekte: [C# function](functions-dotnet-class-library.md) `EventGridEvent`

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

Daha fazla bilgi için Bkz. Paketler, [Öznitelikler,](#attributes-and-annotations) [Yapılandırma](#configuration)ve [Kullanım.](#usage)

### <a name="version-1x"></a>Sürüm 1.x

Aşağıdaki örnekte, aşağıdakilere bağlanan 1.x [C# işlevleri](functions-dotnet-class-library.md) `JObject`gösterilmektedir:

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

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Aşağıdaki örnek, bir *function.json* dosyasında tetikleyici bağlama ve bağlamayı kullanan bir [C# komut dosyası işlevini](functions-reference-csharp.md) gösterir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

### <a name="version-2x-and-higher"></a>Sürüm 2.x ve üzeri

Aşağıda, `EventGridEvent`şuna bağlanan bir örnek verilmiştir:

```csharp
#r "Microsoft.Azure.EventGrid"
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(EventGridEvent eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.Data.ToString());
}
```

Daha fazla bilgi için Bkz. Paketler, [Öznitelikler,](#attributes-and-annotations) [Yapılandırma](#configuration)ve [Kullanım.](#usage)

### <a name="version-1x"></a>Sürüm 1.x

Burada 1.x C# komut dosyası kodu na `JObject`bağlanır:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

Aşağıdaki örnek, bir *function.json* dosyasında tetikleyici bağlama ve bağlamayı kullanan bir [JavaScript işlevini](functions-reference-node.md) gösterir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

İşte JavaScript kodu:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, bir *function.json* dosyasında tetikleyici bağlama ve bağlamayı kullanan bir [Python işlevini](functions-reference-python.md) gösterir.

*Function.json* dosyasındaki bağlayıcı veriler aşağıda veda eder:

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

Python kodu aşağıdavelvere vermiştir:

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

# <a name="java"></a>[Java](#tab/java)

Bu bölümde aşağıdaki örnekler yer almaktadır:

* [Olay Izgara tetikleyici, String parametresi](#event-grid-trigger-string-parameter)
* [Olay Izgara tetikleyicisi, POJO parametresi](#event-grid-trigger-pojo-parameter)

Aşağıdaki örnekler, [Java'da](functions-reference-java.md) bağlamayı kullanan bağlamayı tetiklediğini ve olayı `String` ilk olarak POJO olarak ve ikinci olarak alan bir olayı yazdırdığını gösterir.

### <a name="event-grid-trigger-string-parameter"></a>Olay Izgara tetikleyici, String parametresi

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

### <a name="event-grid-trigger-pojo-parameter"></a>Olay Izgara tetikleyicisi, POJO parametresi

Bu örnek, bir Olay Izgara olayının üst düzey özelliklerini temsil eden aşağıdaki POJO'yu kullanır:

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

Varışta, etkinliğin JSON yükü işlev tarafından kullanılmak üzere ```EventSchema``` POJO'ya seri dışı olarak dönüştürülür. Bu işlem, işlevin olayın özelliklerine nesne yönelimli bir şekilde erişmesine olanak tanır.

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

Java [işlevleri çalışma zamanı kitaplığında,](/java/api/overview/azure/functions/runtime)değeri EventGrid'den gelecek parametrelerdeki `EventGridTrigger` ek açıklamayı kullanın. Bu ek açıklamaları içeren parametreler, bir olay geldiğinde işlevin çalışmasına neden olur.  Bu ek açıklama, yerel Java türleri, POJO'lar veya `Optional<T>`nullable değerleri kullanılarak kullanılabilir.

---

## <a name="attributes-and-annotations"></a>Öznitelikler ve ek açıklamalar

# <a name="c"></a>[C #](#tab/csharp)

[C# sınıfı kitaplıklarında](functions-dotnet-class-library.md) [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/TriggerBinding/EventGridTriggerAttribute.cs) özniteliğini kullanın.

Yöntem imzasındaki bir `EventGridTrigger` öznitelik aşağıda veda eder:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, ILogger log)
{
    ...
}
```

Tam bir örnek için C# örneğine bakın.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Öznitelikler C# Script tarafından desteklenmez.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Öznitelikler JavaScript tarafından desteklenmez.

# <a name="python"></a>[Python](#tab/python)

Öznitelikler Python tarafından desteklenmez.

# <a name="java"></a>[Java](#tab/java)

[EventGridTrigger](https://github.com/Azure/azure-functions-java-library/blob/master/src/main/java/com/microsoft/azure/functions/annotation/EventGridTrigger.java) ek açıklaması, yapılandırma değerleri sağlayarak olay ızgarası bağlamayı bildirimsel olarak yapılandırmanızı sağlar. Daha fazla ayrıntı için [örnek](#example) ve [yapılandırma](#configuration) bölümlerine bakın.

---

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır. Öznitelikte `EventGridTrigger` ayarlanan hiçbir oluşturucu parametre veya özellik yoktur.

|function.json özelliği |Açıklama|
|---------|---------|
| **Türü** | Gerekli - `eventGridTrigger`ayarlanmalıdır. |
| **Yön** | Gerekli - `in`ayarlanmalıdır. |
| **Adı** | Gerekli - olay verilerini alan parametre için işlev kodunda kullanılan değişken adı. |

## <a name="usage"></a>Kullanım

# <a name="c"></a>[C #](#tab/csharp)

Azure İşlevler 1.x'te Olay Izgara tetikleyicisi için aşağıdaki parametre türlerini kullanabilirsiniz:

* `JObject`
* `string`

Azure İşlevler 2.x ve üzeri durumlarda, Olay Izgara tetikleyicisi için aşağıdaki parametre türünü kullanma seçeneğiniz de vardır:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent`- Tüm olay türlerine ortak alanların özelliklerini tanımlar.

> [!NOTE]
> Fonksiyonlar v1'e bağlamaya `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`çalışırsanız, derleyici "amortismana geçmiş" bir ileti görüntüler `Microsoft.Azure.EventGrid.Models.EventGridEvent` ve bunun yerine kullanmanızı önerir. Daha yeni türü kullanmak için [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet paketine `EventGridEvent` başvurun ve tür `Microsoft.Azure.EventGrid.Models`adını önceden hazırlayarak tam olarak nitelendirin.

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

Azure İşlevler 1.x'te Olay Izgara tetikleyicisi için aşağıdaki parametre türlerini kullanabilirsiniz:

* `JObject`
* `string`

Azure İşlevler 2.x ve üzeri durumlarda, Olay Izgara tetikleyicisi için aşağıdaki parametre türünü kullanma seçeneğiniz de vardır:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent`- Tüm olay türlerine ortak alanların özelliklerini tanımlar.

> [!NOTE]
> Fonksiyonlar v1'e bağlamaya `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`çalışırsanız, derleyici "amortismana geçmiş" bir ileti görüntüler `Microsoft.Azure.EventGrid.Models.EventGridEvent` ve bunun yerine kullanmanızı önerir. Daha yeni türü kullanmak için [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet paketine `EventGridEvent` başvurun ve tür `Microsoft.Azure.EventGrid.Models`adını önceden hazırlayarak tam olarak nitelendirin. C# komut dosyası işlevinde NuGet paketlerine nasıl başvurulması hakkında bilgi için Bkz. [NuGet paketlerini kullanma](functions-reference-csharp.md#using-nuget-packages)

# <a name="javascript"></a>[Javascript](#tab/javascript)

Olay Izgara *örneği, function.json* dosyasının `name` özelliğinde yapılandırılan parametre aracılığıyla kullanılabilir.

# <a name="python"></a>[Python](#tab/python)

Olay Izgara *örneği, function.json* dosyasının `name` özelliğinde yapılandırılan ve `func.EventGridEvent`.

# <a name="java"></a>[Java](#tab/java)

Olay Izgara olay örneği, `EventGridTrigger` öznitelik ile ilişkili parametre `EventSchema`aracılığıyla kullanılabilir, bir . Daha fazla ayrıntı için [örneğe](#example) bakın.

---

## <a name="event-schema"></a>Olay şeması

Olay Izgara olayının verileri, bir HTTP isteğinin gövdesinde json nesnesi olarak alınır. JSON aşağıdaki örneğe benzer:

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

Gösterilen örnek, bir öğenin bir dizidir. Olay Grid her zaman bir dizi gönderir ve dizide birden fazla olay gönderebilir. Çalışma zamanı, her dizi öğesi için işlevinizi bir kez çağırır.

JSON verilerinin tüm olay türleri arasında aynı olması durumundaki üst düzey `data` özellikler, özelliğin içeriği her olay türüne özgüyken. Gösterilen örnek bir blob depolama olayı içindir.

Ortak ve olaya özel özelliklerin açıklamaları için Olay Ağı belgelerindeki [Olay özelliklerine](../event-grid/event-schema.md#event-properties) bakın.

Tür `EventGridEvent` yalnızca üst düzey özellikleri tanımlar; `Data` özelliği bir `JObject`.

## <a name="create-a-subscription"></a>Abonelik oluşturma

Olay Izgara http isteklerini almaya başlamak için, işlevi çağıran bitiş noktası URL'sini belirten bir Olay Izgarası aboneliği oluşturun.

### <a name="azure-portal"></a>Azure portalında

Olay Ağı tetikleyicisi ile Azure portalında geliştirdiğiniz işlevler için **Olay Izgara sıyrık ını ekle'yi**seçin.

![Portalda abonelik oluşturma](media/functions-bindings-event-grid/portal-sub-create.png)

Bu bağlantıyı seçtiğinizde, portal, bitiş noktası URL'si önceden doldurulmuş etkinlik **aboneliği oluştur** sayfasını açar.

![Uç nokta URL önceden doldurulmuş](media/functions-bindings-event-grid/endpoint-url.png)

Azure portalını kullanarak abonelik oluşturma hakkında daha fazla bilgi için Olay Ağı belgelerinde [özel etkinlik Oluştur - Azure portalına](../event-grid/custom-event-quickstart-portal.md) bakın.

### <a name="azure-cli"></a>Azure CLI

[Azure CLI'yi](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)kullanarak abonelik oluşturmak için [az eventgrid olay-abonelik oluşturma](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-create) komutunu kullanın.

Komut, işlevi çağıran uç nokta URL'sini gerektirir. Aşağıdaki örnek, sürüme özgü URL deseni gösterir:

#### <a name="version-2x-and-higher-runtime"></a>Sürüm 2.x (ve daha yüksek) çalışma süresi

    https://{functionappname}.azurewebsites.net/runtime/webhooks/eventgrid?functionName={functionname}&code={systemkey}

#### <a name="version-1x-runtime"></a>Sürüm 1.x çalışma süresi

    https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}

Sistem anahtarı, Olay Izgara tetikleyicisi için bitiş noktası URL'sine eklenmesi gereken bir yetkilendirme anahtarıdır. Aşağıdaki bölümde sistem anahtarının nasıl alınılsüreceğini açıklanmaktadır.

Burada bir blob depolama hesabına abone bir örnek (sistem anahtarı için bir yer tutucu ile):

#### <a name="version-2x-and-higher-runtime"></a>Sürüm 2.x (ve daha yüksek) çalışma süresi

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/runtime/webhooks/eventgrid?functionName=imageresizefunc&code=<key>
```

#### <a name="version-1x-runtime"></a>Sürüm 1.x çalışma süresi

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=<key>
```

Abonelik oluşturma hakkında daha fazla bilgi [için, blob depolama hızlı başlat'ına](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) veya diğer Olay Izgara hız başlangıcına bakın.

### <a name="get-the-system-key"></a>Sistem anahtarını alın

Aşağıdaki API'yi (HTTP GET) kullanarak sistem anahtarını alabilirsiniz:

#### <a name="version-2x-and-higher-runtime"></a>Sürüm 2.x (ve daha yüksek) çalışma süresi

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgrid_extension?code={masterkey}
```

#### <a name="version-1x-runtime"></a>Sürüm 1.x çalışma süresi

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={masterkey}
```

Bu bir yönetici API, bu nedenle işlev uygulaması [ana anahtarı](functions-bindings-http-webhook-trigger.md#authorization-keys)gerektirir. Sistem anahtarını (Olay Izgara tetikleyici işlevini çağırmak için) ana anahtarla (işlev uygulamasında yönetim görevlerini gerçekleştirmek için) karıştırmayın. Bir Olay Izgara konusuna abone olduğunuzda, sistem anahtarını kullandığınızdan emin olun.

Sistem anahtarını sağlayan yanıtın bir örneği aşağıda verilmiştir:

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

İşlev uygulamanızın ana anahtarını portaldaki **İşlev uygulaması ayarları** sekmesinden alabilirsiniz.

> [!IMPORTANT]
> Ana anahtar, işlev uygulamanıza yönetici erişimi sağlar. Bu anahtarı üçüncü taraflarla paylaşmayın veya yerel istemci uygulamalarında dağıtmayın.

Daha fazla bilgi için HTTP tetikleyici başvuru makalesindeki [Yetkilendirme tuşlarına](functions-bindings-http-webhook-trigger.md#authorization-keys) bakın.

Alternatif olarak, anahtar değerini kendiniz belirtmek için bir HTTP PUT gönderebilirsiniz.

## <a name="local-testing-with-viewer-web-app"></a>Görüntüleyici web uygulaması ile yerel test

Olay Izgara tetikleyicisini yerel olarak test etmek için, olay grid http isteklerini bulutun kaynağından yerel makinenize teslim etmeniz gerekir. Bunu yapmanın bir yolu, istekleri çevrimiçi olarak yakalamak ve bunları yerel makinenizde el ile yeniden göndermektir:

1. Olay iletilerini yakalayan [bir görüntüleyici web uygulaması oluşturun.](#create-a-viewer-web-app)
1. Olayları görüntüleyici uygulamasına gönderen [bir Olay Izgara aboneliği oluşturun.](#create-an-event-grid-subscription)
1. [Bir istek oluşturun](#generate-a-request) ve istek gövdesini görüntüleyici uygulamasından kopyalayın.
1. [İsteği](#manually-post-the-request) Olay Izgara tetikleyici işlevinin localhost URL'sine el ile gönderin.

Test iniz bittiğinde, bitiş noktasını güncelleyerek aynı aboneliği üretim için kullanabilirsiniz. az [eventgrid olay-abonelik güncelleştirmesi](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update) Azure CLI komutunu kullanın.

### <a name="create-a-viewer-web-app"></a>Görüntüleyici web uygulaması oluşturma

Olay iletilerini yakalamayı kolaylaştırmak için, olay mesajlarını görüntüleyen önceden oluşturulmuş bir [web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtabilirsiniz. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

Siteyi görürsünüz ancak henüz yayımlanmış olay yoktur.

![Yeni siteyi görüntüleme](media/functions-bindings-event-grid/view-site.png)

### <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturma

Test etmek istediğiniz türde bir Olay Izgara aboneliği oluşturun ve olay bildiriminin bitiş noktası olarak web uygulamanızdan URL'yi verin. Web uygulamanızın uç noktası `/api/updates/` sonekini içermelidir. Yani, tam URL`https://<your-site-name>.azurewebsites.net/api/updates`

Azure portalını kullanarak abonelik oluşturma hakkında bilgi için Olay Ağı belgelerinde [özel etkinlik oluştur - Azure portalına](../event-grid/custom-event-quickstart-portal.md) bakın.

### <a name="generate-a-request"></a>İstek oluşturma

Web uygulama bitiş noktanıza HTTP trafiği oluşturacak bir olayı tetikle.  Örneğin, bir blob depolama aboneliği oluşturduysanız, bir blob yükleyin veya silin. Web uygulamanızda bir istek belirdiğinde, istek gövdesini kopyalayın.

Abonelik doğrulama isteği ilk olarak alınacaktır; doğrulama isteklerini yoksayın ve olay isteğini kopyalayın.

![Web uygulamasından istek gövdesini kopyalama](media/functions-bindings-event-grid/view-results.png)

### <a name="manually-post-the-request"></a>İsteği el ile gönderme

Olay Izgara işlevinizi yerel olarak çalıştırın.

Bir HTTP POST isteği oluşturmak için [Postman](https://www.getpostman.com/) veya [curl](https://curl.haxx.se/docs/httpscripting.html) gibi bir araç kullanın:

* Bir `Content-Type: application/json` başlık ayarlayın.
* Bir `aeg-event-type: Notification` başlık ayarlayın.
* İstek Bin verilerini istek gövdesine yapıştırın.
* Olay Izgara tetikleyici işlevinizin URL'sine gönderin.
  * 2.x ve üzeri için aşağıdaki deseni kullanın:

    ```
    http://localhost:7071/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
    ```

  * 1.x kullanımı için:

    ```
    http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
    ```

`functionName` Parametre öznitelikte `FunctionName` belirtilen ad olmalıdır.

Aşağıdaki ekran görüntüleri Postman'da üstbilgi ve istek gövdesini gösterir:

![Postacı'da Başlıklar](media/functions-bindings-event-grid/postman2.png)

![Postacı'da talep organı](media/functions-bindings-event-grid/postman.png)

Olay Izgara tetikleyici işlevi yürütür ve aşağıdaki örneğe benzer günlükleri gösterir:

![Örnek Olay Izgara tetikleme işlevi günlükleri](media/functions-bindings-event-grid/eg-output.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Olay Izgara olayı gönderme](./functions-bindings-event-grid-trigger.md)
