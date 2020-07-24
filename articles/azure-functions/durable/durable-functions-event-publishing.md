---
title: Azure Event Grid yayımlama Dayanıklı İşlevler (Önizleme)
description: Dayanıklı İşlevler için otomatik Azure Event Grid yayımlamayı yapılandırmayı öğrenin.
ms.topic: conceptual
ms.date: 04/25/2020
ms.openlocfilehash: e4651dd7548ba76380bfc2d1b314e67d7abe63d9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87081755"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Azure Event Grid yayımlama Dayanıklı İşlevler (Önizleme)

Bu makalede, düzenleme yaşam döngüsü olaylarını (oluşturma, tamamlanan ve başarısız gibi) özel bir [Azure Event Grid konusuna](../../event-grid/overview.md)yayımlamak için dayanıklı işlevler ayarlama işlemi gösterilmektedir.

Bu özelliğin yararlı olduğu bazı senaryolar aşağıda verilmiştir:

* **Mavi/yeşil dağıtımlar gibi DevOps senaryoları**: [yan yana dağıtım stratejisini](durable-functions-versioning.md#side-by-side-deployments)uygulamadan önce herhangi bir görevin çalışıp çalışmadığını bilmeniz gerekebilir.

* **Gelişmiş izleme ve tanılama desteği**: düzenleme durum BILGILERINI Azure SQL veritabanı veya Azure Cosmos DB gibi sorgular için iyileştirilmiş bir dış depoda izleyebilirsiniz.

* **Uzun süre çalışan arka plan etkinliği**: uzun süre çalışan bir arka plan etkinliği için dayanıklı işlevler kullanıyorsanız, bu özellik geçerli durumu bilmenize yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

* Dayanıklı İşlevler projenize [Microsoft. Azure. WebJobs. Extensions. DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 'ı yükler.
* [Azure depolama öykünücüsünü](../../storage/common/storage-use-emulator.md) (yalnızca Windows) veya mevcut bir Azure Depolama hesabını kullanın.
* [Azure CLI](/cli/azure/?view=azure-cli-latest) 'yı yükleyip [Azure Cloud Shell](../../cloud-shell/overview.md) kullanın

## <a name="create-a-custom-event-grid-topic"></a>Özel bir Event Grid konusu oluşturma

Dayanıklı İşlevler olayları göndermek için bir Event Grid konu başlığı oluşturun. Aşağıdaki yönergelerde, Azure CLı kullanılarak nasıl konu oluşturulacağı gösterilmektedir. Konuyu, [PowerShell kullanarak](../../event-grid/custom-event-quickstart-powershell.md) veya [Azure Portal kullanarak](../../event-grid/custom-event-quickstart-portal.md)da oluşturabilirsiniz.

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

Konunun uç noktasını alın. `<topic_name>`Seçtiğiniz adla değiştirin.

```azurecli
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Konu anahtarını alın. `<topic_name>`Seçtiğiniz adla değiştirin.

```azurecli
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Artık etkinlikleri konuya gönderebilirsiniz.

## <a name="configure-event-grid-publishing"></a>Event Grid yayımlamayı yapılandırma

Dayanıklı İşlevler projenizde `host.json` dosyasını bulun.

### <a name="durable-functions-1x"></a>Dayanıklı İşlevler 1. x

`eventGridTopicEndpoint` `eventGridKeySettingName` Bir özelliği içine ve ekleyin `durableTask` .

```json
{
  "durableTask": {
    "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
    "eventGridKeySettingName": "EventGridKey"
  }
}
```

### <a name="durable-functions-2x"></a>Dayanıklı İşlevler 2. x

`notifications` `durableTask` Dosyanın özelliğine, seçtiğiniz adla değiştirerek bir bölüm ekleyin `<topic_name>` . `durableTask`Veya `extensions` özellikleri yoksa, aşağıdaki örnekte olduğu gibi oluşturun:

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

Olası Azure Event Grid yapılandırma özellikleri [host.jsbelgelerinde](../functions-host-json.md#durabletask)bulunabilir. Dosyayı yapılandırdıktan sonra `host.json` , işlev uygulamanız yaşam döngüsü olaylarını Event Grid konusuna gönderir. Bu eylem, işlev uygulamanızı yerel olarak ve Azure 'da çalıştırdığınızda başlatılır.

İşlev Uygulaması ve içindeki konu anahtarı için uygulama ayarını ayarlayın `local.settings.json` . Aşağıdaki JSON, `local.settings.json` yerel hata ayıklama için bir örnektir. `<topic_key>`Konu anahtarıyla değiştirin.  

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

[Depolama öykünücüsünü](../../storage/common/storage-use-emulator.md) (yalnızca Windows) kullanıyorsanız, çalıştığından emin olun. Çalıştırmadan önce komutu çalıştırmak iyi bir fikirdir `AzureStorageEmulator.exe clear all` .

Mevcut bir Azure depolama hesabı kullanıyorsanız, `UseDevelopmentStorage=true` ' ın `local.settings.json` bağlantı dizesiyle değiştirin.

## <a name="create-functions-that-listen-for-events"></a>Olayları dinleyen işlevler oluşturma

Azure portal kullanarak, Dayanıklı İşlevler uygulamanız tarafından yayımlanan olayları dinlemek için başka bir işlev uygulaması oluşturun. Event Grid konusuyla aynı bölgede bulmak en iyisidir.

### <a name="create-an-event-grid-trigger-function"></a>Event Grid tetikleyici işlevi oluşturma

1. İşlev uygulamanızda **işlevler**' i seçin ve **+ Ekle** ' yi seçin. 

   :::image type="content" source="./media/durable-functions-event-publishing/function-add-function.png" alt-text="Azure portal bir işlev ekleyin." border="true":::

1. **Event Grid**araması yapın ve **Azure Event Grid tetikleyici** şablonunu seçin. 

    :::image type="content" source="./media/durable-functions-event-publishing/function-select-event-grid-trigger.png" alt-text="Azure portal olay Kılavuzu tetikleyici şablonunu seçin." border="true":::

1. Yeni tetikleyiciyi adlandırın ve ardından **Işlev oluştur**' u seçin.

    :::image type="content" source="./media/durable-functions-event-publishing/function-name-event-grid-trigger.png" alt-text="Azure portal olay Kılavuzu tetikleyicisini adlandırın." border="true":::


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

### <a name="add-an-event-grid-subscription"></a>Event Grid aboneliği ekleme

Artık oluşturduğunuz Event Grid konu için bir Event Grid aboneliği ekleyebilirsiniz. Daha fazla bilgi için bkz. [Azure Event Grid kavramları](../../event-grid/concepts.md).

1. Yeni işlevinizde **tümleştirme** ' i seçin ve ardından **Event Grid tetikleyicisi (eventgridevent)** öğesini seçin. 

    :::image type="content" source="./media/durable-functions-event-publishing/eventgrid-trigger-link.png" alt-text="Event Grid tetikleyici bağlantısını seçin." border="true":::

1. **Event Grid açıklaması oluştur**' u seçin.

    :::image type="content" source="./media/durable-functions-event-publishing/create-event-grid-subscription.png" alt-text="Event Grid aboneliğini oluşturun." border="true":::

1. Olay aboneliğinizi adlandırın ve **Event Grid konular** konu türünü seçin. 

1. Aboneliği seçin. Ardından, Event Grid konusu için oluşturduğunuz kaynak grubunu ve kaynağı seçin. 

1. **Oluştur**’u seçin.

    :::image type="content" source="./media/durable-functions-event-publishing/event-grid-subscription-details.png" alt-text="Event Grid aboneliği oluşturun." border="true":::

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
* **`subject`**: Olay konusunun yolu. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}`,, `Running` `Completed` ve olur `Failed` `Terminated` .  
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
