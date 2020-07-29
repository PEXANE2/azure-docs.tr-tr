---
title: Uç noktaları ve yolları yönetme
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS verileri için uç noktaları ve olay yollarını ayarlama ve yönetme.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 6/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 8c9fbf7bc45ed2070570faf0d1dfdb15b5fd98ee
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373275"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins"></a>Azure dijital TWINS 'de uç noktaları ve yolları yönetme

Azure dijital TWINS 'de, [olay bildirimlerini](how-to-interpret-event-data.md) aşağı akış hizmetlerine yönlendirebilir veya işlem kaynaklarına bağlanabilirsiniz. Bu, önce olayları alabilen **uç noktalar** ayarlanarak, ardından Azure dijital TWINS tarafından oluşturulan olayların hangi uç noktalara teslim edildiğini belirten [**olay rotaları**](concepts-route-events.md) tarafından yapılır.

Desteklenen uç nokta türleri şunlardır:
* [Olay Hub’ı](../event-hubs/event-hubs-about.md)
* [Event Grid](../event-grid/overview.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Farklı uç noktalar hakkında daha fazla bilgi için bkz. [*Azure mesajlaşma hizmetleri arasında seçim*](https://docs.microsoft.com/azure/event-grid/compare-messaging-services)yapın.

Uç noktalar ve rotalar [**Eventroutes API 'leri**](how-to-use-apis-sdks.md), [.net (C#) SDK 'Sı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)veya [Azure dijital TWINS CLI](how-to-use-cli.md)ile yönetilir. Ayrıca, [Azure Portal](https://portal.azure.com)aracılığıyla da yönetilebilecek.

## <a name="create-an-endpoint-for-azure-digital-twins"></a>Azure dijital TWINS için uç nokta oluşturma

Bir uç noktayı Azure dijital TWINS 'e bağlamak için, uç nokta için kullanmakta olduğunuz olay hub 'ı, olay Kılavuzu konusu veya Service Bus zaten mevcut olması gerekir. 

Aşağıdaki örnek, Azure CLı kullanarak bir olay Kılavuzu konusunun nasıl oluşturulacağını göstermektedir:

```azurecli
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> Azure CLı 'de komutlara geçirilebilecek Azure bölge adlarının bir listesini çıkarmak için şu komutu çalıştırın:
> ```azurecli
> az account list-locations -o table
> ```

Konuyu oluşturduktan sonra, aşağıdaki komutla Azure Digital TWINS 'e bağlayabilirsiniz:

```azurecli
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

Bu, olay Kılavuzu konusunu bağımsız değişkenle belirtilen ad altında Azure dijital TWINS 'in içinde kullanılabilir hale getirir `--endpoint-name` . Genellikle bu adı, Azure Digital TWINS hizmet API 'sini kullanarak bir sonraki bölümde oluşturacağınız bir **olay yolunun**hedefi olarak kullanacaksınız.

Olay Hub 'ı ve Service Bus uç noktaları için eşdeğer komutlar mevcuttur:

* Service Bus konu uç noktası ekleyin (önceden oluşturulmuş bir Service Bus kaynağı gerektirir)
```azurecli 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* Olay Hub 'ı uç noktası Ekle (önceden oluşturulmuş Olay Hub kaynağı gerektirir)
```azurecli
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

## <a name="manage-event-routes-with-apis"></a>API 'lerle olay yollarını yönetme

Azure dijital TWINS 'den bir uç noktaya veri göndermek için bir olay yolu tanımlamanız gerekir. Azure dijital TWINS **Eventroutes API 'leri** , geliştiricilerin sistem genelinde ve aşağı akış hizmetlerinde olay akışını bir şekilde yönetmesine olanak tanır. Kavramlar bölümünde olay yolları hakkında daha fazla bilgi edinin [*: Azure dijital TWINS olaylarını yönlendirme*](concepts-route-events.md).

Uç noktalarınız kurulum tamamlandıktan sonra bir olay rotası oluşturmaya devam edebilirsiniz.

>[!NOTE]
>Son noktalarınızı dağıttıysanız, yeni bir olay yolu için kullanmayı denemeden **önce** bunların dağıtımını tamamladığınızı doğrulayın. Uç noktalar kullanılamadığından yönlendirme dağıtımı başarısız olursa birkaç dakika bekleyip yeniden deneyin.
>
> Bu akışı komut dosyası olarak belirlerseniz, uç nokta hizmeti 'nin yönlendirme kurulumuna geçmeden önce dağıtımı tamamlaması için 2-3 dakika boyunca bir süre oluşturarak bu işlemi hesaba eklemek isteyebilirsiniz.

Bu makaledeki örnekler C# SDK 'SıNı kullanır.

Olay yolları veri düzlemi API 'Leri kullanılarak tanımlanır. Bir yol tanımı şu öğeleri içerebilir:
* Kullanmak istediğiniz yol KIMLIĞI
* Kullanmak istediğiniz uç noktanın adı
* Uç noktaya hangi olayların gönderileceğini tanımlayan bir filtre 

Rota KIMLIĞI yoksa, Azure dijital TWINS dışında hiçbir ileti yönlendirilmez. Bir yol KIMLIĞI varsa ve filtre ise `true` , tüm iletiler uç noktaya yönlendirilir. Bir yol KIMLIĞI varsa ve farklı bir filtre eklenirse, iletiler filtreye göre filtrelenecektir.

Bir yol birden çok bildirimin ve olay türünün seçilebilmelidir. 

`CreateEventRoute`, bir olay yolu eklemek için kullanılan SDK çağrıdır. Kullanım örneği aşağıda verilmiştir:

```csharp
EventRoute er = new EventRoute("endpointName");
er.Filter("true"); //Filter allows all messages
await client.CreateEventRoute("routeName", er);
```

> [!TIP]
> Tüm SDK işlevleri, zaman uyumlu ve zaman uyumsuz sürümlerde gelir.

### <a name="event-route-sample-code"></a>Olay rotası örnek kodu

Aşağıdaki kod örneği, bir olay yolunu oluşturma, listeleme ve silme işlemlerinin nasıl yapılacağını göstermektedir:
```csharp
try
{
    Console.WriteLine("Create a route: testRoute1");
    EventRoute er = new EventRoute("< your - endpoint - name >");
    // Make a filter that passes everything
    er.Filter = "true";
    client.CreateEventRoute("< your - route - name >", er);
    Console.WriteLine("Create route succeeded. Now listing routes:");
    Pageable <EventRoute> result = client.GetEventRoutes();
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Route {r.Id} to endpoint {r.EndpointId} with filter {r.Filter} ");
    }
    Console.WriteLine("Deleting routes:");
    foreach (EventRoute r in result)
    {
        Console.WriteLine($"Deleting route {r.Id}:");
        client.DeleteEventRoute(r.Id);
    }
}
catch (RequestFailedException e)
{
    Console.WriteLine($"*** Error in event route processing ({e.ErrorCode}):\n${e.Message}");
}
```

### <a name="filter-events"></a>Olayları filtreleme

Bitiş noktaları, filtrelemeden Azure dijital TWINS 'ten çeşitli olaylar alır:
* Azure Digital TWINS hizmet API 'SI kullanılarak [dijital TWINS](concepts-twins-graph.md) tarafından tetiklenen telemetri
* İkizi özellik değişikliği bildirimleri, Azure dijital TWINS örneğindeki herhangi bir ikizi için özellik değişikliklerinde harekete geçirilir
* Yaşam döngüsü olayları, TWINS veya ilişkiler oluşturulduğunda veya silindiğinde tetiklenir
* Model değişiklik olayları, bir Azure dijital TWINS örneğinde yapılandırılan [modeller](concepts-models.md) eklendiğinde veya silindiğinde tetiklenir

Bir uç noktaya filtre ekleyerek gönderilmekte olan olayları kısıtlayabilirsiniz.

Bir filtre eklemek için, aşağıdaki gövdeyi kullanarak *https://{YourHost}/Eventrotalar/myNewRoute? api-Version = 2020-05 -31-Preview* IÇIN bir PUT isteği kullanabilirsiniz:

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

Desteklenen yol filtreleri aşağıda verilmiştir.

| Filtre adı | Açıklama | Filtre şeması | Desteklenen değerler | 
| --- | --- | --- | --- |
| Tür | Dijital ikizi örneğiniz aracılığıyla akan [olay türü](./concepts-route-events.md#types-of-event-messages) | `"filter" : "type = '<eventType>'"` | `Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| Kaynak | Azure dijital TWINS örneğinin adı | `"filter" : "source = '<hostname>'"`|  **Bildirimler için**:`<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net` <br> **Telemetri için**:`<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net/digitaltwins/<twinId>`|
| Konu | Yukarıdaki olay kaynağı bağlamındaki olay açıklaması | `"filter": " subject = '<subject>'"` | **Bildirimler için**: konu`<twinid>` <br> ya da birden çok parça veya kimlik tarafından benzersiz şekilde tanımlanan konular için URI biçimi:<br>`<twinid>/relationships/<relationshipid>`<br> **Telemetri için**: konu bileşen yoludur (telemetri bir ikizi bileşeninden yayıldıysanız), gibi `comp1.comp2` . Telemetri bir bileşenden yayılmadığından, konu alanı boş olur. |
| Veri şeması | DTDL model KIMLIĞI | `"filter": "dataschema = 'dtmi:example:com:floor4;2'"` | **Telemetri için**: veri şeması, ikizi veya Telemetriyi gösteren BILEŞENIN model kimliğidir <br>**Bildirimler için**: veri şeması desteklenmez|
| İçerik türü | Veri değerinin içerik türü | `"filter": "datacontenttype = '<contentType>'"` | `application/json` |
| Spec sürümü | Kullandığınız olay şemasının sürümü | `"filter": "specversion = '<version>'"` | Olmalıdır `1.0` . Bu, CloudEvents şema sürüm 1,0 ' i gösterir |
| Doğru/yanlış | Filtre olmadan veya bir yolu devre dışı bırakarak yol oluşturulmasına izin verir | `"filter" : "<true/false>"` | `true`= Route filtre olmadan etkinleştirildi <br> `false`= yol devre dışı |
<!--
| ID | *Not implemented for public preview* | "filter": "id = '…'" | |
| Schema | *Not implemented for public preview*  | "filter": "dataschema = '…'" | |
-->

Filtreleri aşağıdaki işlemleri kullanarak birleştirmek de mümkündür:

| Filtre adı | Filtre şeması | Örnek | 
| --- | --- | --- |
| VE/VEYA | `"filter": "<filter1> AND <filter2>"` | `"filter": "type != 'microsoft.iot.telemetry' AND datacontenttype = 'application/json'"` |
| VE/VEYA | `"filter": "<filter1> OR <filter2>"` | `"filter": "type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json'"` |
| İç içe işlemler | `"filter": "(<Comparison1>) AND (<Comparison2>)"` | `"filter": "(type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json') OR (specversion != '1.0')"` |

> [!NOTE]
> Önizleme sırasında yalnızca dize eşitlik desteklenir (=,! =).

Bir filtreyi uyguladığınızda veya güncelleştirdiğinizde, değişikliğin veri ardışık düzeninde yansıtılması birkaç dakika sürebilir.

## <a name="manage-endpoints-and-routes-with-cli"></a>CLı ile uç noktaları ve yolları yönetme

Uç noktalar ve rotalar Ayrıca Azure dijital TWINS CLı kullanılarak yönetilebilir. Komutları [*nasıl yapılır: Azure dijital TWINS CLI 'Sını kullanma*](how-to-use-cli.md)bölümünde bulabilirsiniz.

## <a name="monitor-event-routes"></a>Olay yollarını izle

Sayı, gecikme ve hata oranı gibi yönlendirme ölçümleri [Azure Portal](https://portal.azure.com/)görüntülenebilir. 

Portal giriş sayfasında, ayrıntılarını almak için Azure dijital TWINS örneğinizi arayın. *Ölçümler* sayfasını açmak Için Azure Digital TWINS örneğinin menüsündeki **ölçümler** seçeneğini belirleyin.

:::image type="content" source="media/how-to-view-metrics/azure-digital-twins-metrics.png" alt-text="Azure dijital TWINS için ölçüm sayfasını gösteren ekran görüntüsü":::

Buradan, örneğiniz için ölçümleri görüntüleyebilir ve özel görünümler oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Alabilmeniz için farklı olay iletisi türleri hakkında bilgi edinin:
* [*Nasıl yapılır: olay verilerini yorumlama*](how-to-interpret-event-data.md)
