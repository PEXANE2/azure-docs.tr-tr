---
title: CloudEvents şemasındaki etkinliklerle Azure Etkinlik Ağıt'ı kullanma
description: Azure Etkinlik Ağıt'taki etkinlikler için CloudEvents şemasını nasıl kullanacağımı açıklar. Hizmet, Bulut Etkinlikleri'nin JSON uygulamasındaki olayları destekler.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: babanisa
ms.openlocfilehash: 0efccd2851885dad209d5548a76737c25777b891
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80372441"
---
# <a name="use-cloudevents-v10-schema-with-event-grid"></a>Olay Izgaraile CloudEvents v1.0 şema kullanın

Varsayılan olay [şemasına](event-schema.md)ek olarak, Azure Event [Grid, CloudEvents v1.0](https://github.com/cloudevents/spec/blob/v1.0/json-format.md) ve [HTTP protokol bağlamanın](https://github.com/cloudevents/spec/blob/v1.0/http-protocol-binding.md)JSON uygulamasındaki olayları doğal olarak destekler. [CloudEvents](https://cloudevents.io/) olay verilerini açıklamak için açık bir [belirtimdir.](https://github.com/cloudevents/spec/blob/v1.0/spec.md)

CloudEvents, yayımlama ve bulut tabanlı etkinlikler tüketmek için ortak bir olay şeması sağlayarak birlikte çalışabilirliği kolaylaştırır. Bu şema, tek tip takımlama, olayları işleme & yönlendirmenin standart yolları ve dış olay şemasını deserialize etmenin evrensel yolları sağlar. Ortak bir şema ile, daha kolay platformlar arasında iş entegre edebilirsiniz.

CloudEvents, Microsoft da dahil olmak üzere birçok [ortak çalışan](https://github.com/cloudevents/spec/blob/master/community/contributors.md)tarafından Cloud Native [Computing Foundation](https://www.cncf.io/)aracılığıyla oluşturulmaktadır. Şu anda sürüm 1.0 olarak kullanılabilir.

Bu makalede, Olay Grid ile CloudEvents şeması nasıl kullanılacağı açıklanmaktadır.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Önizleme özelliğini yükleme

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>CloudEvent şeması

Aşağıda CloudEvents biçiminde bir Azure Blob Depolama olayı örneği verilmiştir:

``` JSON
{
    "specversion": "1.0",
    "type": "Microsoft.Storage.BlobCreated",  
    "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}",
    "id": "9aeb0fdf-c01e-0131-0922-9eb54906e209",
    "time": "2019-11-18T15:13:39.4589254Z",
    "subject": "blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "dataschema": "#",
    "data": {
        "api": "PutBlockList",
        "clientRequestId": "4c5dd7fb-2c48-4a27-bb30-5361b5de920a",
        "requestId": "9aeb0fdf-c01e-0131-0922-9eb549000000",
        "eTag": "0x8D76C39E4407333",
        "contentType": "image/png",
        "contentLength": 30699,
        "blobType": "BlockBlob",
        "url": "https://gridtesting.blob.core.windows.net/testcontainer/{new-file}",
        "sequencer": "000000000000000000000000000099240000000000c41c18",
        "storageDiagnostics": {
            "batchId": "681fe319-3006-00a8-0022-9e7cde000000"
        }
    }
}
```

CloudEvents v1.0'daki kullanılabilir alanların, türlerinin ve tanımlarının ayrıntılı bir [açıklamasına buradan ulaşabilirsiniz.](https://github.com/cloudevents/spec/blob/v1.0/spec.md#required-attributes)

CloudEvents şemasında ve Olay Izgara şemasında teslim edilen olaylar için üstbilgi değerleri `content-type`. CloudEvents şeması için bu üstbilgi `"content-type":"application/cloudevents+json; charset=utf-8"`değeri . Olay Izgara şeması için bu `"content-type":"application/json; charset=utf-8"`üstbilgi değeri .

## <a name="configure-event-grid-for-cloudevents"></a>CloudEvents için Olay Izgara'yı Yapılandır

CloudEvents şemasındaki olayların hem giriş hem de çıktısı için Olay Grid'i kullanabilirsiniz. CloudEvents'i Blob Depolama etkinlikleri, IoT Hub etkinlikleri ve özel olaylar gibi sistem etkinlikleri için kullanabilirsiniz. Ayrıca tel üzerinde ileri geri bu olayları dönüştürebilirsiniz.


| Giriş şeması       | Çıktı şeması
|--------------------|---------------------
| CloudEvents biçimi | CloudEvents biçimi
| Olay Izgara biçimi  | CloudEvents biçimi
| Olay Izgara biçimi  | Olay Izgara biçimi

Tüm olay şemaları için Olay Izgarası, bir olay ızgarası konusuna yayımlarken ve bir olay aboneliği oluştururken doğrulama gerektirir. Daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulaması'na](security-authentication.md)bakın.

### <a name="input-schema"></a>Giriş şeması

Özel konuyu oluştururken giriş şeasını özel bir konu için ayarlarsınız.

Azure CLI için şunu kullanın:

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventschemav1_0
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName gridResourceGroup `
  -Location westcentralus `
  -Name <topic_name> `
  -InputSchema CloudEventSchemaV1_0
```

### <a name="output-schema"></a>Çıktı şeması

Olay aboneliğini oluştururken çıktı şeasını ayarlarsınız.

Azure CLI için şunu kullanın:

```azurecli-interactive
topicID=$(az eventgrid topic show --name <topic-name> -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --source-resource-id $topicID \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventschemav1_0
```

PowerShell için şunu kullanın:
```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeliverySchema CloudEventSchemaV1_0
```

 Şu anda, etkinlik CloudEvents şemasında teslim edildiğinde Azure İşlevler uygulaması için Olay Izgara tetikleyicisini kullanamazsınız. BIR HTTP tetikleyicisi kullanın. CloudEvents şemasında olay alan bir HTTP tetikleyicisi uygulama örnekleri için [bkz.](#azure-functions)

 ## <a name="endpoint-validation-with-cloudevents-v10"></a>CloudEvents v1.0 ile Uç Nokta Doğrulama

Olay Izgara'yı zaten biliyorsanız, kötüye kullanımı önlemek için Olay Grid'in uç nokta doğrulama el sıkışmasını biliyor olabilirsiniz. CloudEvents v1.0, HTTP OPTIONS yöntemini kullanarak kendi [kötüye kullanım koruma semantikini](security-authentication.md#webhook-event-delivery) uygular. Bunun hakkında daha fazla bilgiyi [buradan](https://github.com/cloudevents/spec/blob/v1.0/http-webhook.md#4-abuse-protection) edinebilirsiniz. Çıktı için CloudEvents şemasını kullanırken, Olay Grid, Olay Grid doğrulama olay mekanizması yerine CloudEvents v1.0 kötüye kullanım koruması ile kullanır.

<a name="azure-functions"></a>

## <a name="use-with-azure-functions"></a>Azure İşlevleriyle Kullanım

[Azure İşlemesi Olay Kılavuz bağlaması](../azure-functions/functions-bindings-event-grid.md) CloudEvents'i doğal olarak desteklemez, bu nedenle HTTP tarafından tetiklenen işlevler CloudEvents iletilerini okumak için kullanılır. CloudEvents'i okumak için bir HTTP tetikleyicisi kullanırken, Olay Izgara tetikleyicisinin otomatik olarak yaptığı şey için kod yazmanız gerekir:

* [Abonelik doğrulama isteğine](../event-grid/security-authentication.md#webhook-event-delivery)doğrulama yanıtı gönderir.
* İstek gövdesinde bulunan olay dizisinin öğesi başına bir kez işlevi çağırır.

İşlevi yerel olarak çağırmak için kullanılacak URL hakkında bilgi için veya Azure'da çalıştığında [HTTP tetikleyici bağlayıcı başvuru belgelerine](../azure-functions/functions-bindings-http-webhook.md) bakın

BIR HTTP tetikleyicisi için aşağıdaki örnek C# kodu Olay Izgara tetikleyici davranışını simüle eder.  CloudEvents şemasında teslim edilen olaylar için bu örneği kullanın.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", "options", Route = null)]HttpRequestMessage req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");
    if (req.Method == "OPTIONS")
    {
        // If the request is for subscription validation, send back the validation code
        
        var response = req.CreateResponse(HttpStatusCode.OK);
        response.Add("Webhook-Allowed-Origin", "eventgrid.azure.net");

        return response;
    }

    var requestmessage = await req.Content.ReadAsStringAsync();
    var message = JToken.Parse(requestmessage);

    // The request is not for subscription validation, so it's for an event.
    // CloudEvents schema delivers one event at a time.
    log.LogInformation($"Source: {message["source"]}");
    log.LogInformation($"Time: {message["eventTime"]}");
    log.LogInformation($"Event data: {message["data"].ToString()}");

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Bir HTTP tetikleyicisi için aşağıdaki örnek JavaScript kodu Olay Izgara tetikleyici davranışını simüle eder. CloudEvents şemasında teslim edilen olaylar için bu örneği kullanın.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    
    if (req.method == "OPTIONS) {
        // If the request is for subscription validation, send back the validation code
        
        context.log('Validate request received');
        context.res = { status: 200, body: { "ValidationResponse": code } };
        context.res.headers.append('Webhook-Allowed-Origin', 'eventgrid.azure.net');
    }
    else
    {
        var message = req.body;
        
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        var event = JSON.parse(message);
        context.log('Source: ' + event.source);
        context.log('Time: ' + event.eventTime);
        context.log('Data: ' + JSON.stringify(event.data));
    }
 
    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimlerini izleme hakkında daha fazla bilgi için [Bkz.](monitor-event-delivery.md)
* CloudEvents'i test etmenizi, yorum yapmayı ve [bu etkinliklere katkıda bulunmanızı](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) öneririz.
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
