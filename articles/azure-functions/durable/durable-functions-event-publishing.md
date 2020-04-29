---
title: Azure Event Grid yayımlama Dayanıklı İşlevler (Önizleme)
description: Dayanıklı İşlevler için otomatik Azure Event Grid yayımlamayı yapılandırmayı öğrenin.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 671f7bd5221a936ea9dad0f0cece895bdbe9512f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81535494"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Azure Event Grid yayımlama Dayanıklı İşlevler (Önizleme)

Bu makalede, düzenleme yaşam döngüsü olaylarını (oluşturma, tamamlanan ve başarısız gibi) özel bir [Azure Event Grid konusuna](https://docs.microsoft.com/azure/event-grid/overview)yayımlamak için dayanıklı işlevler ayarlama işlemi gösterilmektedir.

Bu özelliğin yararlı olduğu bazı senaryolar aşağıda verilmiştir:

* **Mavi/yeşil dağıtımlar gibi DevOps senaryoları**: [yan yana dağıtım stratejisini](durable-functions-versioning.md#side-by-side-deployments)uygulamadan önce herhangi bir görevin çalışıp çalışmadığını bilmeniz gerekebilir.

* **Gelişmiş izleme ve tanılama desteği**: düzenleme durum BILGILERINI Azure SQL veritabanı veya Azure Cosmos DB gibi sorgular için iyileştirilmiş bir dış depoda izleyebilirsiniz.

* **Uzun süre çalışan arka plan etkinliği**: uzun süre çalışan bir arka plan etkinliği için dayanıklı işlevler kullanıyorsanız, bu özellik geçerli durumu bilmenize yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

* Dayanıklı İşlevler projenize [Microsoft. Azure. WebJobs. Extensions. DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 'ı yükler.
* [Azure depolama öykünücüsünü](../../storage/common/storage-use-emulator.md) (yalnızca Windows) veya mevcut bir Azure Depolama hesabını kullanın.
* [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) 'yı yükleyip [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın

## <a name="create-a-custom-event-grid-topic"></a>Özel bir Event Grid konusu oluşturma

Dayanıklı İşlevler olayları göndermek için bir Event Grid konu başlığı oluşturun. Aşağıdaki yönergelerde, Azure CLı kullanılarak nasıl konu oluşturulacağı gösterilmektedir. Bunu, [PowerShell kullanarak](../../event-grid/custom-event-quickstart-powershell.md) veya [Azure Portal kullanarak](../../event-grid/custom-event-quickstart-portal.md)da yapabilirsiniz.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

`az group create` komutuyla bir kaynak grubu oluşturun. Şu anda Azure Event Grid tüm bölgeleri desteklemez. Hangi bölgelerin desteklendiği hakkında daha fazla bilgi için [Azure Event Grid genel bakış](../../event-grid/overview.md)bölümüne bakın.

```azurecli
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Özel konu oluşturma

Event Grid konusu, olaylarınızı naklettiğiniz Kullanıcı tanımlı bir uç nokta sağlar. `<topic_name>` değerini konunuz için benzersiz bir adla değiştirin. Konu adı bir DNS girdisi haline geldiği için benzersiz olmalıdır.

```azurecli
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>Uç noktasını ve anahtarı al

Konunun uç noktasını alın. Seçtiğiniz `<topic_name>` adla değiştirin.

```azurecli
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Konu anahtarını alın. Seçtiğiniz `<topic_name>` adla değiştirin.

```azurecli
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Artık etkinlikleri konuya gönderebilirsiniz.

## <a name="configure-event-grid-publishing"></a>Event Grid yayımlamayı yapılandırma

Dayanıklı İşlevler projenizde `host.json` dosyasını bulun.

### <a name="durable-functions-1x"></a>Dayanıklı İşlevler 1. x

Bir `eventGridTopicEndpoint` `durableTask` özelliği `eventGridKeySettingName` içine ve ekleyin.

```json
{
  "durableTask": {
    "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName": "EventGridKey"
  }
}
```

### <a name="durable-functions-2x"></a>Dayanıklı İşlevler 2. x

Dosyanın `notifications` `durableTask` özelliğine, seçtiğiniz adla değiştirerek `<topic_name>` bir bölüm ekleyin. `durableTask` Veya `extensions` özellikleri yoksa, aşağıdaki örnekte olduğu gibi oluşturun:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "notifications": {
        "eventGrid": {
          "topicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
          "keySettingName": "EventGridKey"
        }
      }
    }
  }
}
```

Olası Azure Event Grid yapılandırma özellikleri [Host. JSON belgelerinde](../functions-host-json.md#durabletask)bulunabilir. `host.json` Dosyayı yapılandırdıktan sonra, işlev uygulamanız yaşam döngüsü olaylarını Event Grid konusuna gönderir. Bu, işlev uygulamanızı yerel olarak ve Azure 'da çalıştırdığınızda çalışır.

İşlev Uygulaması ve `local.settings.json`içindeki konu anahtarı için uygulama ayarını ayarlayın. Aşağıdaki JSON, `local.settings.json` yerel hata ayıklama için bir örnektir. Konu `<topic_key>` anahtarıyla değiştirin.  

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

[Depolama öykünücüsünü](../../storage/common/storage-use-emulator.md) (yalnızca Windows) kullanıyorsanız, çalıştığından emin olun. Çalıştırmadan önce `AzureStorageEmulator.exe clear all` komutu çalıştırmak iyi bir fikirdir.

Mevcut bir Azure depolama hesabı kullanıyorsanız, ' ın `UseDevelopmentStorage=true` `local.settings.json` bağlantı dizesiyle değiştirin.

## <a name="create-functions-that-listen-for-events"></a>Olayları dinleyen işlevler oluşturma

Azure portal kullanarak, Dayanıklı İşlevler uygulamanız tarafından yayımlanan olayları dinlemek için başka bir işlev uygulaması oluşturun. Event Grid konusuyla aynı bölgede bulmak en iyisidir.

### <a name="create-an-event-grid-trigger-function"></a>Event Grid tetikleyici işlevi oluşturma

Yaşam döngüsü olaylarını almak için bir işlev oluşturun. **Özel işlev**seçin.

![Özel bir işlev oluştur ' u seçin.](./media/durable-functions-event-publishing/functions-portal.png)

Event Grid tetikleyiciyi seçin ve bir dil seçin.

![Event Grid tetikleyiciyi seçin.](./media/durable-functions-event-publishing/eventgrid-trigger.png)

İşlevin adını girip öğesini seçin `Create`.

![Event Grid tetikleyiciyi oluşturun.](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Aşağıdaki koda sahip bir işlev oluşturuldu:

# <a name="c-script"></a>[C# betiği](#tab/csharp-script)

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

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
module.exports = async function(context, eventGridEvent) {
    context.log(typeof eventGridEvent);
    context.log(eventGridEvent);
}
```

---

`Add Event Grid Subscription` öğesini seçin. Bu işlem, oluşturduğunuz Event Grid konu için bir Event Grid aboneliği ekler. Daha fazla bilgi için bkz. [Azure Event Grid kavramları](https://docs.microsoft.com/azure/event-grid/concepts)

![Event Grid tetikleyici bağlantısını seçin.](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

`Event Grid Topics` **Konu türü**için seçin. Event Grid konusu için oluşturduğunuz kaynak grubunu seçin. Sonra Event Grid konusunun örneğini seçin. Tuşuna `Create`basın.

![Event Grid aboneliği oluşturun.](./media/durable-functions-event-publishing/eventsubscription.png)

Artık yaşam döngüsü olaylarını almaya hazır olursunuz.

## <a name="run-durable-functions-app-to-send-the-events"></a>Olayları göndermek için Dayanıklı İşlevler uygulaması çalıştırma

Daha önce yapılandırdığınız Dayanıklı İşlevler projesinde, yerel makinenizde hata ayıklamayı başlatın ve bir düzenleme başlatın. Uygulama, Event Grid için Dayanıklı İşlevler yaşam döngüsü olaylarını yayımlar. Event Grid, Azure portal günlüklerini denetleyerek oluşturduğunuz dinleyici işlevini tetikleyeceğini doğrulayın.

```
2019-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2019-04-20T09:28:21.104 [Info] {
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
    "eventTime": "2019-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2019-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2019-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2019-04-20T09:28:37.098 [Info] {
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
    "eventTime": "2019-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2019-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Olay şeması

Aşağıdaki listede yaşam döngüsü olayları şeması açıklanmaktadır:

* **`id`**: Event Grid olayı için benzersiz tanımlayıcı.
* **`subject`**: Olay konusunun yolu. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}`,, ve `Terminated` `Running` `Completed` `Failed`  
* **`data`**: Belirli parametreleri Dayanıklı İşlevler.
  * **`hubName`**: [Taskhub](durable-functions-task-hubs.md) adı.
  * **`functionName`**: Orchestrator işlev adı.
  * **`instanceId`**: InstanceId Dayanıklı İşlevler.
  * **`reason`**: İzleme olayı ile ilişkili ek veriler. Daha fazla bilgi için bkz. [dayanıklı işlevler 'de tanılama (Azure işlevleri)](durable-functions-diagnostics.md)
  * **`runtimeStatus`**: Orchestration çalışma zamanı durumu. Çalışıyor, tamamlandı, başarısız, Iptal edildi.
* **`eventType`**: "orchestratorEvent"
* **`eventTime`**: Olay saati (UTC).
* **`dataVersion`**: Yaşam döngüsü olay şemasının sürümü.
* **`metadataVersion`**: Meta verilerin sürümü.
* **`topic`**: Olay Kılavuzu konu kaynağı.

## <a name="how-to-test-locally"></a>Yerel olarak test etme

Yerel olarak test etmek için [Azure işlevini okuyun Event Grid yerel hata ayıklamayı tetikleyin](../functions-debug-event-grid-trigger-local.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı İşlevler 'de örnek yönetimini öğrenin](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Dayanıklı İşlevler sürüm oluşturmayı öğrenin](durable-functions-versioning.md)
