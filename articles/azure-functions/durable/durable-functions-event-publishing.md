---
title: Azure Event Grid yayımlama Dayanıklı İşlevler (Önizleme)
description: Dayanıklı İşlevler için otomatik Azure Event Grid yayımlamayı yapılandırmayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: glenga
ms.openlocfilehash: 837e29731b617fcb8da95b89668403638c4d049a
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70087397"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Azure Event Grid yayımlama Dayanıklı İşlevler (Önizleme)

Bu makalede, düzenleme yaşam döngüsü olaylarını (oluşturma, tamamlanan ve başarısız gibi) özel bir [Azure Event Grid konusuna](https://docs.microsoft.com/azure/event-grid/overview)yayımlamak için dayanıklı işlevler ayarlama işlemi gösterilmektedir.

Bu özelliğin yararlı olduğu bazı senaryolar aşağıda verilmiştir:

* **Mavi/yeşil dağıtımlar gibi DevOps senaryoları**: [Yan yana dağıtım stratejisini](durable-functions-versioning.md#side-by-side-deployments)uygulamadan önce herhangi bir görevin çalışıp çalışmadığını bilmeniz gerekebilir.

* **Gelişmiş izleme ve tanılama desteği**: Orchestration durum bilgilerini SQL Database veya CosmosDB gibi sorgular için iyileştirilmiş bir dış depoda izleyebilirsiniz.

* **Uzun süre çalışan arka plan etkinliği**: Uzun süre çalışan bir arka plan etkinliği için Dayanıklı İşlevler kullanıyorsanız, bu özellik geçerli durumu bilmenize yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

* Dayanıklı İşlevler projenize [Microsoft. Azure. WebJobs. Extensions. DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 1.3.0-RC veya sonrasını yükler.
* [Azure Storage öykünücüsü](https://docs.microsoft.com/azure/storage/common/storage-use-emulator)'nü yükler.
* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) 'yı yükleyip [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) kullanın

## <a name="create-a-custom-event-grid-topic"></a>Özel bir olay kılavuzu oluşturma konusu

Dayanıklı İşlevler olayları göndermek için bir olay Kılavuzu konusu oluşturun. Aşağıdaki yönergelerde, Azure CLı kullanılarak nasıl konu oluşturulacağı gösterilmektedir. PowerShell 'i veya Azure portal kullanarak nasıl yapılacağı hakkında bilgi için aşağıdaki makalelere bakın:

* [EventGrid hızlı başlangıç: Özel olay oluşturma-PowerShell](https://docs.microsoft.com/azure/event-grid/custom-event-quickstart-powershell)
* [EventGrid hızlı başlangıç: Özel olay oluşturma-Azure portal](https://docs.microsoft.com/azure/event-grid/custom-event-quickstart-portal)

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

`az group create` komutuyla bir kaynak grubu oluşturun. Şu anda Azure Event Grid tüm bölgeleri desteklemez. Hangi bölgelerin desteklendiği hakkında daha fazla bilgi için [Azure Event Grid genel bakış](https://docs.microsoft.com/azure/event-grid/overview)bölümüne bakın.

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Özel konu oluşturma

Olay Kılavuzu konusu, olaylarınızı naklettiğiniz Kullanıcı tanımlı bir uç nokta sağlar. `<topic_name>` değerini konunuz için benzersiz bir adla değiştirin. Konu adı bir DNS girdisi haline geldiği için benzersiz olmalıdır.

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>Uç noktasını ve anahtarı al

Konunun uç noktasını alın. Seçtiğiniz `<topic_name>` adla değiştirin.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Konu anahtarını alın. Seçtiğiniz `<topic_name>` adla değiştirin.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Artık etkinlikleri konuya gönderebilirsiniz.

## <a name="configure-azure-event-grid-publishing"></a>Azure Event Grid yayımlamayı yapılandırma

Dayanıklı işlevler projenizde `host.json` dosyasını bulun.

`eventGridKeySettingName` Bir `eventGridTopicEndpoint` özelliğiiçineveekleyin`durableTask` .

```json
{
    "durableTask": {
        "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "eventGridKeySettingName": "EventGridKey"
    }
}
```

Olası Azure Event Grid yapılandırma özellikleri [Host. JSON belgelerinde](../functions-host-json.md#durabletask)bulunabilir. `host.json` Dosyayı yapılandırdıktan sonra, işlev uygulamanız yaşam döngüsü olaylarını Event Grid konusuna gönderir. Bu, işlev uygulamanızı yerel olarak ve Azure 'da çalıştırdığınızda çalışır. ' ' '

İşlev Uygulaması ve `local.setting.json`içindeki konu anahtarı için uygulama ayarını ayarlayın. Aşağıdaki JSON, `local.settings.json` yerel hata ayıklama için bir örnektir. Konu `<topic_key>` anahtarıyla değiştirin.  

```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
        "EventGridKey": "<topic_key>"
    }
}
```

[Depolama öykünücüsünün](https://docs.microsoft.com/azure/storage/common/storage-use-emulator) çalıştığından emin olun. Çalıştırmadan önce `AzureStorageEmulator.exe clear all` komutu çalıştırmak iyi bir fikirdir.

## <a name="create-functions-that-listen-for-events"></a>Olayları dinleyen işlevler oluşturma

İşlev Uygulaması oluşturun. Bunu, Event Grid konusuyla aynı bölgede bulmak en iyisidir.

### <a name="create-an-event-grid-trigger-function"></a>Event Grid tetikleyici işlevi oluşturma

Yaşam döngüsü olaylarını almak için bir işlev oluşturun. **Özel işlev**seçin.

![Özel bir işlev oluştur ' u seçin.](./media/durable-functions-event-publishing/functions-portal.png)

Event Grid tetikleyicisi ' ni seçin ve `C#`öğesini seçin.

![Event Grid tetikleyiciyi seçin.](./media/durable-functions-event-publishing/eventgrid-trigger.png)

İşlevin adını girip öğesini seçin `Create`.

![Event Grid tetikleyiciyi oluşturun.](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Aşağıdaki koda sahip bir işlev oluşturuldu:

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

public static void Run(JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

`Add Event Grid Subscription` öğesini seçin. Bu işlem, oluşturduğunuz olay Kılavuzu konusu için bir Event Grid aboneliği ekler. Daha fazla bilgi için bkz. [Azure Event Grid kavramları](https://docs.microsoft.com/azure/event-grid/concepts)

![Event Grid tetikleyici bağlantısını seçin.](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

`Event Grid Topics` **Konu türü**için seçin. Olay Kılavuzu konusu için oluşturduğunuz kaynak grubunu seçin. Ardından Event Grid konusunun örneğini seçin. Tuşuna `Create`basın.

![Event Grid aboneliği oluşturun.](./media/durable-functions-event-publishing/eventsubscription.png)

Artık yaşam döngüsü olaylarını almaya hazır olursunuz.

## <a name="create-durable-functions-to-send-the-events"></a>Olayları göndermek için Dayanıklı İşlevler oluşturma

Dayanıklı İşlevler projenizde, yerel makinenizde hata ayıklamayı başlatın.  Aşağıdaki kod Dayanıklı İşlevler şablon kodu ile aynıdır. Yerel makinenizde zaten `host.json` yapılandırmış `local.settings.json` olmanız gerekir.

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace LifeCycleEventSpike
{
    public static class Sample
    {
        [FunctionName("Sample")]
        public static async Task<List<string>> RunOrchestrator(
            [OrchestrationTrigger] DurableOrchestrationContext context)
        {
            var outputs = new List<string>();

            // Replace "hello" with the name of your Durable Activity Function.
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Tokyo"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Seattle"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "London"));

            // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
            return outputs;
        }

        [FunctionName("Sample_Hello")]
        public static string SayHello([ActivityTrigger] string name, ILogger log)
        {
            log.LogInformation($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
            [OrchestrationClient]DurableOrchestrationClient starter,
            ILogger log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

' I Postman `Sample_HttpStart` veya tarayıcınızla çağırırsanız, dayanıklı işlev yaşam döngüsü olayları gönderilmeye başlar. Uç nokta genellikle `http://localhost:7071/api/Sample_HttpStart` yerel hata ayıklama içindir.

Azure portal oluşturduğunuz işlevdeki günlüklere bakın.

```
2018-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2018-04-20T09:28:21.104 [Info] {
    "id": "054fe385-c017-4ce3-b38a-052ac970c39d",
    "subject": "durable/orchestrator/Running",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Running"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2018-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2018-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2018-04-20T09:28:37.098 [Info] {
    "id": "8cf17246-fa9c-4dad-b32a-5a868104f17b",
    "subject": "durable/orchestrator/Completed",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Completed"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2018-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Olay Şeması

Aşağıdaki listede yaşam döngüsü olayları şeması açıklanmaktadır:

* **`id`** : Event Grid olayı için benzersiz tanımlayıcı.
* **`subject`** : Olay konusunun yolu. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}`,, ve`Terminated`olur. `Running` `Completed` `Failed`  
* **`data`** : Belirli parametreleri Dayanıklı İşlevler.
  * **`hubName`** : [Taskhub](durable-functions-task-hubs.md) adı.
  * **`functionName`** : Orchestrator işlev adı.
  * **`instanceId`** : InstanceId Dayanıklı İşlevler.
  * **`reason`** : İzleme olayı ile ilişkili ek veriler. Daha fazla bilgi için bkz. [dayanıklı işlevler 'de tanılama (Azure işlevleri)](durable-functions-diagnostics.md)
  * **`runtimeStatus`** : Orchestration çalışma zamanı durumu. Çalışıyor, tamamlandı, başarısız, Iptal edildi.
* **`eventType`** : "orchestratorEvent"
* **`eventTime`** : Olay saati (UTC).
* **`dataVersion`** : Yaşam döngüsü olay şemasının sürümü.
* **`metadataVersion`** :  Meta veri sürümü.
* **`topic`** : Olay Kılavuzu konu kaynağı.

## <a name="how-to-test-locally"></a>Yerel olarak test etme

Yerel olarak test etmek için [ngrok](../functions-bindings-event-grid.md#local-testing-with-ngrok)kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı İşlevler 'de örnek yönetimini öğrenin](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Dayanıklı İşlevler sürüm oluşturmayı öğrenin](durable-functions-versioning.md)
