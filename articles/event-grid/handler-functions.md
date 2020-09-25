---
title: Azure Event Grid olaylar için olay işleyicisi olarak Azure işlevi
description: Azure işlevlerini Event Grid olayları için olay işleyicileri olarak nasıl kullanabileceğinizi açıklar.
ms.topic: conceptual
ms.date: 09/18/2020
ms.openlocfilehash: 87aeb78729dcc7bec9f193fab389e5c0952e63d5
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91270345"
---
# <a name="azure-function-as-an-event-handler-for-event-grid-events"></a>Event Grid olaylar için olay işleyicisi olarak Azure işlevi

Olay işleyicisi, olayın gönderildiği yerdir. İşleyici, olayı işlemek için bir eylem gerçekleştirir. Birkaç Azure hizmeti, olayları işleyecek şekilde otomatik olarak yapılandırılır ve **Azure işlevleri** bunlardan biridir. 

Event Grid olaylara yanıt vermek için sunucusuz bir mimaride **Azure işlevlerini** kullanın. İşleyici olarak bir Azure işlevi kullanırken, genel HTTP tetikleyicisi yerine Event Grid tetikleyicisini kullanın. Event Grid Event Grid Tetikleyicileri otomatik olarak doğrular. Genel HTTP Tetikleyicileri ile [doğrulama yanıtını](webhook-event-delivery.md) kendiniz uygulamanız gerekir.

Daha fazla bilgi için bkz. işlevlerde Event Grid tetikleyicisini kullanmaya genel bakış için [Azure işlevleri Event Grid tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) .

## <a name="tutorials"></a>Öğreticiler

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: işlev ile olayları Işleme](custom-event-to-function.md) | İşlemek için bir işleve özel bir olay gönderir. |
| [Öğretici: Event Grid kullanarak karşıya yüklenen görüntüleri yeniden boyutlandırmayı otomatikleştirme](resize-images-on-storage-blob-upload-event.md) | Kullanıcılar, görüntüleri Web uygulamasına depolama hesabına yükler. Bir Depolama Blobu oluşturulduğunda, Event Grid işlev uygulamasına bir olay göndererek karşıya yüklenen görüntüyü yeniden boyutlandırır. |
| [Öğretici: veri ambarına büyük veri akışı](event-grid-event-hubs-integration.md) | Event Hubs bir yakalama dosyası oluşturduğunda, Event Grid bir işlev uygulamasına bir olay gönderir. Uygulama yakalama dosyasını alır ve verileri bir veri ambarına geçirir. |
| [Öğretici: Azure Event Grid tümleştirme örneklerine Azure Service Bus](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid, Service Bus konu başlığından bir işlev uygulamasına ve Logic App 'e ileti gönderir. |

## <a name="rest-example-for-put"></a>REST örneği (PUT için)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "AzureFunction",
            "properties": 
            {
                "resourceId": "/subscriptions/<AZURE SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.Web/sites/<FUNCTION APP NAME>/functions/<FUNCTION NAME>",
                "maxEventsPerBatch": 10,
                "preferredBatchSizeInKilobytes": 6400
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="enable-batching"></a>Toplu işlemeyi etkinleştir
Daha yüksek bir verimlilik için abonelikte toplu işlemeyi etkinleştirin. Azure portal kullanıyorsanız, toplu iş başına en fazla olay ve cihazının saniyede bayt olarak tercih edilen toplu iş boyutunu bir abonelik oluşturma sırasında veya oluşturma işleminden sonra ayarlayabilirsiniz. 

Batch ayarlarını Azure portal, PowerShell, CLı veya Kaynak Yöneticisi şablonunu kullanarak yapılandırabilirsiniz. 

### <a name="azure-portal"></a>Azure portal
Kullanıcı arabiriminde abonelik oluşturma sırasında, **olay aboneliği oluştur** sayfasında, **Gelişmiş Özellikler** sekmesine geçin ve **toplu iş başına en fazla olay** ve **tercih edilen toplu iş boyutu için değerleri kilobayt cinsinden**ayarlayın. 
    
:::image type="content" source="./media/custom-event-to-function/enable-batching.png" alt-text="Abonelik oluşturma sırasında toplu işlemeyi etkinleştir":::

Bu değerleri, **Event Grid konu** sayfasının **Özellikler** sekmesinde varolan bir abonelik için güncelleştirebilirsiniz. 

:::image type="content" source="./media/custom-event-to-function/features-batch-settings.png" alt-text="Oluşturulduktan sonra toplu işlemeyi etkinleştir":::

### <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Azure Resource Manager şablonunda **Maxeventsperbatch** ve **Preferredbatchsizeınkilobayt** ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Microsoft. EventGrid Eventabonelikler şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/eventsubscriptions).

### <a name="azure-cli"></a>Azure CLI’si
Aşağıdaki parametreleri kullanarak Batch ile ilgili ayarları yapılandırmak için [az eventgrid Event-Subscription Create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create&preserve-view=true) veya [az eventgrid Event-Subscription Update](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update&preserve-view=true) komutunu kullanabilirsiniz: `--max-events-per-batch` veya `--preferred-batch-size-in-kilobytes` .

### <a name="azure-powershell"></a>Azure PowerShell
Aşağıdaki parametreleri kullanarak Batch ile ilgili ayarları yapılandırmak için [New-azeventgridsubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/new-azeventgridsubscription) veya [Update-azeventgridsubscription](https://docs.microsoft.com/powershell/module/az.eventgrid/update-azeventgridsubscription) cmdlet 'ini kullanabilirsiniz: `-MaxEventsPerBatch` veya `-PreferredBatchSizeInKiloBytes` .

> [!NOTE]
> **Başka bir Kiracıdaki** bir Azure işlevine olay sunma desteklenmez. 

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen olay işleyicilerinin bir listesi için bkz. [olay işleyicileri](event-handlers.md) makalesi. 
