---
title: Uç noktaları ve yolları yönetme (API 'Ler ve CLı)
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS verileri için uç noktaları ve olay yollarını ayarlama ve yönetme.
author: alexkarcher-msft
ms.author: alkarche
ms.date: 10/12/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 14edc97115735f8b6763171a07b5f739fc745e9f
ms.sourcegitcommit: dbe434f45f9d0f9d298076bf8c08672ceca416c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92151241"
---
# <a name="manage-endpoints-and-routes-in-azure-digital-twins-apis-and-cli"></a>Azure dijital TWINS 'te uç noktaları ve yolları yönetme (API 'Ler ve CLı)

[!INCLUDE [digital-twins-route-selector.md](../../includes/digital-twins-route-selector.md)]

Azure dijital TWINS 'de, [olay bildirimlerini](how-to-interpret-event-data.md) aşağı akış hizmetlerine veya bağlı işlem kaynaklarına yönlendirebilirsiniz. Bu, önce olayları alabilen **uç noktalar** ayarlanarak yapılır. Daha sonra, Azure dijital TWINS tarafından oluşturulan olayların hangi uç noktalara teslim edildiğini belirten  [**olay yolları**](concepts-route-events.md) oluşturabilirsiniz.

Uç noktalar ve rotalar [Eventroutes API 'leri](how-to-use-apis-sdks.md), [.net (C#) SDK 'Sı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core)veya [Azure dijital TWINS CLI](how-to-use-cli.md)ile yönetilebilir. Bu makalede, bu mekanizmalarda uç noktalar ve rotalar oluşturma işlemi adım adım açıklanmaktadır.

Ayrıca, [Azure Portal](https://portal.azure.com)aracılığıyla da yönetilebilecek. Bunun yerine portalı kullanan Bu makalenin bir sürümü için bkz. [*nasıl yapılır: uç noktaları ve yolları yönetme (portal)*](how-to-manage-routes-portal.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure hesabınızın** olması gerekir ( [buradan](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)birini ücretsiz olarak ayarlayabilirsiniz)
* Azure aboneliğinizde bir **Azure dijital TWINS örneği** gerekir. Zaten bir örneğiniz yoksa, [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama*](how-to-set-up-instance-portal.md)bölümündeki adımları kullanarak bir tane oluşturabilirsiniz. Bu makalede daha sonra kullanmak için kurulum 'un aşağıdaki değerlerini kullanın:
    - Örnek adı
    - Kaynak grubu
    
## <a name="create-an-endpoint-for-azure-digital-twins"></a>Azure dijital TWINS için uç nokta oluşturma

Bunlar, örneğiniz için oluşturabileceğiniz desteklenen uç nokta türleridir:
* [Event Grid](../event-grid/overview.md)
* [Event Hubs](../event-hubs/event-hubs-about.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Farklı uç nokta türleri hakkında daha fazla bilgi için bkz. [*Azure mesajlaşma hizmetleri arasında seçim yapın*](../event-grid/compare-messaging-services.md).

Bir uç noktayı Azure dijital TWINS 'e bağlamak için, uç nokta için kullanmakta olduğunuz olay Kılavuzu konusu, Olay Hub 'ı veya Service Bus zaten mevcut olması gerekir. 

### <a name="create-an-event-grid-endpoint"></a>Event Grid uç noktası oluşturma

Aşağıdaki örnek, Azure CLı kullanarak olay Kılavuzu türü uç noktasının nasıl oluşturulacağını göstermektedir. [Azure Cloud Shell](https://shell.azure.com)KULLANABILIR veya [CLI 'yi yerel olarak yükleyebilirsiniz](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest).

İlk olarak, bir olay Kılavuzu konusu oluşturun. Aşağıdaki komutu kullanabilir veya *özel olay* hızlı başlangıcı Event Grid [ *özel konu oluştur* bölümünü](../event-grid/custom-event-quickstart-portal.md#create-a-custom-topic) ziyaret ederek adımları daha ayrıntılı şekilde görüntüleyebilirsiniz.

```azurecli
az eventgrid topic create -g <your-resource-group-name> --name <your-topic-name> -l <region>
```

> [!TIP]
> Azure CLı 'de komutlara geçirilebilecek Azure bölge adlarının bir listesini çıkarmak için şu komutu çalıştırın:
> ```azurecli
> az account list-locations -o table
> ```

Konuyu oluşturduktan sonra, aşağıdaki [Azure dijital TWıNS CLI komutuyla](how-to-use-cli.md)Azure dijital TWINS 'e bağlayabilirsiniz:

```azurecli
az dt endpoint create eventgrid --endpoint-name <Event-Grid-endpoint-name> --eventgrid-resource-group <Event-Grid-resource-group-name> --eventgrid-topic <your-Event-Grid-topic-name> -n <your-Azure-Digital-Twins-instance-name>
```

Şimdi, olay Kılavuzu konusu bağımsız değişkenle belirtilen ad altında Azure dijital TWINS 'in içindeki bir uç nokta olarak sunulmaktadır `--endpoint-name` . Genellikle bu adı, Azure Digital TWINS hizmet API 'sini kullanarak [Bu makalenin ilerleyen kısımlarında](#event-routes-with-apis-and-the-c-sdk) oluşturacağınız bir **olay yolunun**hedefi olarak kullanacaksınız.

### <a name="create-an-event-hubs-or-service-bus-endpoint"></a>Event Hubs veya Service Bus uç noktası oluşturma

Event Hubs veya Service Bus uç noktaları oluşturma işlemi yukarıda gösterilen Event Grid işlemine benzerdir.

İlk olarak, uç nokta olarak kullanacağınız kaynaklarınızı oluşturun. Gerekli olan özellikler şunlardır:
* Service Bus: _Service Bus ad alanı_, _Service Bus konu_, _Yetkilendirme kuralı_
* Event Hubs: _Event Hubs ad alanı_, _Olay Hub_'ı, _Yetkilendirme kuralı_

Ardından, Azure dijital TWINS 'de uç noktaları oluşturmak için aşağıdaki komutları kullanın: 

* Service Bus konu uç noktası ekleyin (önceden oluşturulmuş bir Service Bus kaynağı gerektirir)
```azurecli 
az dt endpoint create servicebus --endpoint-name <Service-Bus-endpoint-name> --servicebus-resource-group <Service-Bus-resource-group-name> --servicebus-namespace <Service-Bus-namespace> --servicebus-topic <Service-Bus-topic-name> --servicebus-policy <Service-Bus-topic-policy> -n <your-Azure-Digital-Twins-instance-name>
```

* Event Hubs uç noktası Ekle (önceden oluşturulmuş Event Hubs kaynak gerektirir)
```azurecli
az dt endpoint create eventhub --endpoint-name <Event-Hub-endpoint-name> --eventhub-resource-group <Event-Hub-resource-group> --eventhub-namespace <Event-Hub-namespace> --eventhub <Event-Hub-name> --eventhub-policy <Event-Hub-policy> -n <your-Azure-Digital-Twins-instance-name>
```

### <a name="create-an-endpoint-with-dead-lettering"></a>Etkin olmayan bir uç nokta oluşturma

Bir uç nokta belirli bir süre içinde bir olayı teslim edimezse veya olayı belirli bir sayıda sunmaya çalıştıktan sonra, teslim edilmemiş olayı bir depolama hesabına gönderebilir. Bu işlem, **atılacak**olarak bilinir.

Etkin olmayan bir uç nokta oluşturmak için, uç noktanızı oluşturmak için [ARM API 'lerini](https://docs.microsoft.com/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate) kullanmanız gerekir. 

Atılacak mektup konumunu ayarlamadan önce, kapsayıcısı olan bir depolama hesabınız olmalıdır. Uç nokta oluştururken bu kapsayıcının URL 'sini sağlarsınız. Atılacak mektup, bir SAS belirtecine sahip kapsayıcı URL 'SI olarak sağlanır. Bu belirteç yalnızca `write` depolama hesabındaki hedef kapsayıcı için izne ihtiyaç duyuyor. Tamamen oluşturulmuş URL şu biçimde olacaktır: `https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>`

SAS belirteçleri hakkında daha fazla bilgi edinmek için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](https://docs.microsoft.com/azure/storage/common/storage-sas-overview)

Atılacak alma hakkında daha fazla bilgi edinmek için bkz [. kavramlar: olay yolları](./concepts-route-events.md#dead-letter-events)

#### <a name="configuring-the-endpoint"></a>Uç noktayı yapılandırma

Uç nokta oluştururken, `deadLetterSecret` `properties` isteğin gövdesinde bir kapsayıcı URL 'si ve depolama HESABıNıZ için SAS belirteci içeren nesnesine bir ekleyin.

```json
{
  "properties": {
    "endpointType": "EventGrid",
    "TopicEndpoint": "https://contosoGrid.westus2-1.eventgrid.azure.net/api/events",
    "accessKey1": "xxxxxxxxxxx",
    "accessKey2": "xxxxxxxxxxx",
    "deadLetterSecret":"https://<storageAccountname>.blob.core.windows.net/<containerName>?<SASToken>"
  }
}
```

Daha fazla bilgi için bkz. Azure Digital TWINS REST API belgeleri: [uç noktalar-DigitalTwinsEndpoint CreateOrUpdate](https://docs.microsoft.com/rest/api/digital-twins/controlplane/endpoints/digitaltwinsendpoint_createorupdate).

### <a name="message-storage-schema"></a>İleti depolama şeması

Kullanılmayan iletiler depolama hesabınızda aşağıdaki biçimde depolanır:

`{container}/{endpointName}/{year}/{month}/{day}/{hour}/{eventId}.json`

Kullanılmayan iletiler, özgün uç noktanıza teslim edilmesi amaçlanan özgün olayın şemasıyla eşleşir.

Aşağıda, bir [ikizi Create bildirimi](./how-to-interpret-event-data.md#digital-twin-life-cycle-notifications)için atılacak ileti iletisine bir örnek verilmiştir:

```json
{
  "specversion": "1.0",
  "id": "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "<yourInstance>.api.<yourregion>.da.azuredigitaltwins-test.net",
  "data": {
    "$dtId": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "$etag": "W/\"xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
    "TwinData": "some sample",
    "$metadata": {
      "$model": "dtmi:test:deadlettermodel;1",
      "room": {
        "lastUpdateTime": "2020-10-14T01:11:49.3576659Z"
      }
    }
  },
  "subject": "<yourInstance>xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "time": "2020-10-14T01:11:49.3667224Z",
  "datacontenttype": "application/json",
  "traceparent": "00-889a9094ba22b9419dd9d8b3bfe1a301-f6564945cb20e94a-01"
}
```

## <a name="event-routes-with-apis-and-the-c-sdk"></a>Olay yolları (API 'Ler ve C# SDK 'Sı ile)

Azure dijital TWINS 'den bir uç noktaya gerçek veri göndermek için bir **olay yolu**tanımlamanız gerekir. Azure dijital TWINS **Eventroutes API 'leri** , geliştiricilerin sistem genelinde ve aşağı akış hizmetlerinde olay akışını bir şekilde yönetmesine olanak tanır. Kavramlar bölümünde olay yolları hakkında daha fazla bilgi edinin [*: Azure dijital TWINS olaylarını yönlendirme*](concepts-route-events.md).

Bu bölümdeki örnekler [.net (C#) SDK 'sını](https://www.nuget.org/packages/Azure.DigitalTwins.Core)kullanır.

**Önkoşul**: bir yol oluşturmak için geçiş yapabilmeniz için önce Bu makalenin önceki kısımlarında açıklandığı gibi uç noktalar oluşturmanız gerekir. Uç noktalarınız kurulum tamamlandıktan sonra bir olay rotası oluşturmaya devam edebilirsiniz.

>[!NOTE]
>Son noktalarınızı dağıttıysanız, yeni bir olay yolu için kullanmayı denemeden **önce** bunların dağıtımını tamamladığınızı doğrulayın. Uç noktalar kullanılamadığından yönlendirme dağıtımı başarısız olursa birkaç dakika bekleyip yeniden deneyin.
>
> Bu akışı komut dosyası olarak belirlerseniz, uç nokta hizmeti 'nin yönlendirme kurulumuna geçmeden önce dağıtımı tamamlaması için 2-3 dakika boyunca bir süre oluşturarak bu işlemi hesaba eklemek isteyebilirsiniz.

### <a name="create-an-event-route"></a>Olay yolu oluşturma

Olay yolları [veri düzlemi API 'leri](how-to-use-apis-sdks.md#overview-data-plane-apis)kullanılarak tanımlanır. 

Bir yol tanımı şu öğeleri içerebilir:
* Kullanmak istediğiniz yol adı
* Kullanmak istediğiniz uç noktanın adı
* Uç noktaya hangi olayların gönderileceğini tanımlayan bir filtre 

Yol adı yoksa, Azure dijital TWINS dışında hiçbir ileti yönlendirilmez. Bir yol adı varsa ve filtre ise `true` , tüm iletiler uç noktaya yönlendirilir. Bir yol adı varsa ve farklı bir filtre eklenirse, iletiler filtreye göre filtrelenecektir.

Bir yol birden çok bildirimin ve olay türünün seçilebilmelidir. 

`CreateEventRoute` , bir olay yolu eklemek için kullanılan SDK çağrıdır. Kullanım örneği aşağıda verilmiştir:

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

Olay yolıza bir uç nokta için **filtre** ekleyerek gönderilmekte olan olayları kısıtlayabilirsiniz.

Bir filtre eklemek için, aşağıdaki gövdeyi kullanarak *https://{YourHost}/Eventrotalar/myNewRoute? api-Version = 2020-05 -31-Preview* IÇIN bir PUT isteği kullanabilirsiniz:

```json  
{
    "endpointName": "<endpoint-name>",
    "filter": "<filter-text>"
}
``` 

Desteklenen yol filtreleri aşağıda verilmiştir. Yukarıdaki istek gövdesinde yer tutucuyu değiştirmek için *filtre metin şeması* sütunundaki ayrıntıyı kullanın `<filter-text>` .

[!INCLUDE [digital-twins-route-filters](../../includes/digital-twins-route-filters.md)]

## <a name="manage-endpoints-and-routes-with-cli"></a>CLı ile uç noktaları ve yolları yönetme

Uç noktalar ve rotalar Ayrıca Azure dijital TWINS CLı kullanılarak yönetilebilir. CLı ve kullanılabilen komutları kullanma hakkında daha fazla bilgi için bkz. [*nasıl yapılır: Azure dijital TWINS CLI 'Yi kullanma*](how-to-use-cli.md).

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

[!INCLUDE [digital-twins-route-metrics](../../includes/digital-twins-route-metrics.md)]

## <a name="next-steps"></a>Sonraki adımlar

Alabilmeniz için farklı olay iletisi türleri hakkında bilgi edinin:
* [*Nasıl yapılır: olay verilerini yorumlama*](how-to-interpret-event-data.md)