---
title: Azure Etkinlik Izgarasına Yayımlananması Dayanıklı İşlevler (önizleme)
description: Dayanıklı Işlevler için otomatik Azure Olay Izgara yayımlamasını nasıl yapılandırıyarıştırmayı öğrenin.
ms.topic: conceptual
ms.date: 03/14/2019
ms.openlocfilehash: 52ffcd4eb81936ffcfa61580288c60bd59ffb744
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78249755"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Azure Etkinlik Izgarasına Yayımlananması Dayanıklı İşlevler (önizleme)

Bu makalede, özel bir [Azure Olay Izgara Konusu'nda](https://docs.microsoft.com/azure/event-grid/overview)düzenleme yaşam döngüsü olaylarını (oluşturulmuş, tamamlanmış ve başarısız) yayımlayacak Dayanıklı İşlevler nasıl ayarlanacağını gösterilmektedir.

Bu özelliğin yararlı olduğu bazı senaryolar şunlardır:

* **Mavi/yeşil dağıtımlar gibi DevOps senaryoları**: Yan yana dağıtım stratejisini uygulamadan önce herhangi bir görevin çalışır olup olmadığını bilmek [isteyebilirsiniz.](durable-functions-versioning.md#side-by-side-deployments)

* **Gelişmiş izleme ve tanılama desteği**: Azure SQL Veritabanı veya Azure Cosmos DB gibi sorgular için en iyi duruma getirilmiş harici bir mağazada düzenleme durumu bilgilerini izleyebilirsiniz.

* **Uzun süren arka plan etkinliği**: Uzun süren bir arka plan etkinliği için Dayanıklı İşlevler kullanıyorsanız, bu özellik geçerli durumu bilmenize yardımcı olur.

## <a name="prerequisites"></a>Ön koşullar

* Dayanıklı İşlevler projenizde [Microsoft.Azure.WebJobs.Extensions.DurableTask'ı](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) yükleyin.
* [Azure Depolama Emülatörü'ni](../../storage/common/storage-use-emulator.md) (yalnızca Windows) yükleyin veya varolan bir Azure Depolama hesabı kullanın.
* [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) yükleme veya [Azure Bulut Bulutu'nu](../../cloud-shell/overview.md) kullanma

## <a name="create-a-custom-event-grid-topic"></a>Özel bir Olay Izgara konusu oluşturma

Dayanıklı İşlevlerden olay göndermek için bir Olay Izgara sı oluştur. Aşağıdaki yönergeler, Azure CLI'yi kullanarak bir konunun nasıl oluşturulacağını gösterir. Bunu [PowerShell'i kullanarak](../../event-grid/custom-event-quickstart-powershell.md) veya [Azure portalını kullanarak](../../event-grid/custom-event-quickstart-portal.md)da yapabilirsiniz.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

`az group create` komutuyla bir kaynak grubu oluşturun. Şu anda Azure Olay Ağıtı tüm bölgeleri desteklemez. Hangi bölgelerin desteklendiği hakkında bilgi için [Azure Olay Ağı'na genel bakış'a](../../event-grid/overview.md)bakın.

```azurecli
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Özel konu oluşturma

Olay Izgara konusu, etkinliğinizi yayınladığınız kullanıcı tanımlı bir bitiş noktası sağlar. `<topic_name>` değerini konunuz için benzersiz bir adla değiştirin. DNS girişi olduğundan konu adı benzersiz olmalıdır.

```azurecli
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>Bitiş noktasını ve anahtarı alın

Konunun bitiş noktasını alın. Seçtiğiniz `<topic_name>` adla değiştirin.

```azurecli
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Konu anahtarını alın. Seçtiğiniz `<topic_name>` adla değiştirin.

```azurecli
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Artık konuya etkinlik gönderebilirsiniz.

## <a name="configure-event-grid-publishing"></a>Olay Izgara yayımlama yapılandırma

Dayanıklı Işlevler projenizde `host.json` dosyayı bulun.

Bir `eventGridTopicEndpoint` `eventGridKeySettingName` `durableTask` özellik ekleyin ve.

```json
{
    "durableTask": {
        "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "eventGridKeySettingName": "EventGridKey"
    }
}
```

Olası Azure Olay Ağı yapılandırma özellikleri [ana bilgisayar.json belgelerinde](../functions-host-json.md#durabletask)bulunabilir. Dosyayı `host.json` yapılandırdıktan sonra, işlev uygulamanız Olay Izgara konusuna yaşam döngüsü olayları gönderir. Bu, işlev uygulamanızı hem yerel hem de Azure'da çalıştırdığınızda çalışır.

İşlev Uygulamasında ki konu anahtarı için `local.settings.json`uygulama ayarını ayarlayın ve . Aşağıdaki JSON yerel hata `local.settings.json` ayıklama için bir örnektir. Konu `<topic_key>` anahtarıyla değiştirin.  

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

[Depolama Emülatörü](../../storage/common/storage-use-emulator.md) 'ni (yalnızca Windows) kullanıyorsanız, çalıştığından emin olun. Yürütmeden önce komutu `AzureStorageEmulator.exe clear all` çalıştırmak iyi bir fikirdir.

Varolan bir Azure Depolama hesabı `UseDevelopmentStorage=true` kullanıyorsanız, bağlantı dizesiyle değiştirin. `local.settings.json`

## <a name="create-functions-that-listen-for-events"></a>Olayları dinleyen işlevler oluşturma

Azure portalını kullanarak, Dayanıklı İşlevler uygulamanız tarafından yayınlanan etkinlikleri dinlemek için başka bir işlev uygulaması oluşturun. Olay Izgara sıbaşlığıyla aynı bölgede bulmak en iyisidir.

### <a name="create-an-event-grid-trigger-function"></a>Olay Izgara tetikleyici işlevi oluşturma

Yaşam döngüsü olaylarını almak için bir işlev oluşturun. **Özel İşlev'i**seçin.

![Özel bir işlev oluştur'u seçin.](./media/durable-functions-event-publishing/functions-portal.png)

Olay Izgara Tetikleyici'yi seçin ve bir dil seçin.

![Olay Izgara Tetikleyicisini seçin.](./media/durable-functions-event-publishing/eventgrid-trigger.png)

İşlevin adını girin ve `Create`sonra .

![Olay Izgara Tetikleyicisi oluşturun.](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Aşağıdaki koda sahip bir işlev oluşturulur:

# <a name="c-script"></a>[C# Komut Dosyası](#tab/csharp-script)

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

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
module.exports = async function(context, eventGridEvent) {
    context.log(typeof eventGridEvent);
    context.log(eventGridEvent);
}
```

---

`Add Event Grid Subscription` öğesini seçin. Bu işlem, oluşturduğunuz Olay Izgara sıcağı için bir Olay Izgara aboneliği ekler. Daha fazla bilgi için Azure [Etkinlik Kılavuz'undaki Kavramlar'a](https://docs.microsoft.com/azure/event-grid/concepts) bakın

![Olay Izgara Tetikleyici bağlantısını seçin.](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Konu `Event Grid Topics` **Türü**için seçin. Olay Izgarası konusu için oluşturduğunuz kaynak grubunu seçin. Ardından Olay Izgara sıcağı konusunun örneğini seçin. Basın `Create`.

![Event Grid aboneliği oluşturun.](./media/durable-functions-event-publishing/eventsubscription.png)

Artık yaşam döngüsü etkinliklerialmaya hazırsınız.

## <a name="run-durable-functions-app-to-send-the-events"></a>Olayları göndermek için Dayanıklı Fonksiyonlar uygulamasını çalıştırın

Daha önce yapılandırdığınız Dayanıklı İşlevler projesinde, yerel makinenizde hata ayıklamaya başlayın ve bir orkestrasyon başlatın. Uygulama, Etkin İşlevler yaşam döngüsü olaylarını Event Grid'e yayınlar. Olay Izgara'sının, Azure portalındaki günlüklerini denetleyerek oluşturduğunuz dinleyici işlevini tetiklediğini doğrulayın.

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

## <a name="event-schema"></a>Olay Şeması

Aşağıdaki liste yaşam döngüsü olayları şema açıklar:

* **`id`**: Olay Izgara olayı için benzersiz tanımlayıcı.
* **`subject`**: Olay konusuna giden yol. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}`olacak `Running`, `Completed` `Failed`, `Terminated`, ve .  
* **`data`**: Dayanıklı Fonksiyonlar Belirli Parametreler.
  * **`hubName`**: [TaskHub](durable-functions-task-hubs.md) adı.
  * **`functionName`**: Orchestrator fonksiyon adı.
  * **`instanceId`**: Dayanıklı Fonksiyonlar instanceId.
  * **`reason`**: İzleme olayıyla ilişkili ek veriler. Daha fazla bilgi için [Bkz. Dayanıklı İşlevler Tanılama (Azure İşlevleri)](durable-functions-diagnostics.md)
  * **`runtimeStatus`**: Orkestrasyon Çalışma Süresi Durumu. Çalışan, Tamamlandı, Başarısız, İptal Edildi.
* **`eventType`**: "orchestratorEvent"
* **`eventTime`**: Olay zamanı (UTC).
* **`dataVersion`**: Yaşam döngüsü olay şeması nın sürümü.
* **`metadataVersion`**: Meta verilerin sürümü.
* **`topic`**: Olay ızgarası konu kaynağı.

## <a name="how-to-test-locally"></a>Yerel olarak nasıl test edile

Yerel olarak test etmek için [Azure İşi Olay Izgara tetikleyici Yerel Hata Ayıklama'yı](../functions-debug-event-grid-trigger-local.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı İşlevler'de örnek yönetimini öğrenin](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Dayanıklı Işlevler sürümü öğrenin](durable-functions-versioning.md)
