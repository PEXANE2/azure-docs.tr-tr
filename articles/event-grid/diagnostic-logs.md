---
title: Konular veya etki alanları için Azure Event Grid tanılama günlükleri
description: Bu makalede bir Azure olay Kılavuzu konusu veya etki alanı için tanılama günlükleri hakkında kavramsal bilgiler sağlanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/29/2020
ms.author: spelluru
ms.openlocfilehash: 93e7e47cbcc1ab9542ba333b89f7dd655a412489
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82630258"
---
#  <a name="diagnostic-logs-for-azure-event-grid-topicsdomains"></a>Azure Event Grid konuları/etki alanları için tanılama günlükleri
Tanılama ayarları Event Grid kullanıcıların bir depolama hesabında, Olay Hub 'ında veya bir Log Analytics çalışma alanında **Yayımlama ve teslim hatası** günlüklerini yakalayıp görüntülemesine olanak tanır. Bu makalede Günlükler ve örnek günlük girdisi için şema sağlanmaktadır.


## <a name="schema-for-publishdelivery-failure-logs"></a>Yayımlama/teslim hatası günlükleri için şema

| Özellik adı | Veri türü | Description |
| ------------- | --------- | ----------- | 
| Saat | DateTime | Günlük girişinin oluşturulduğu saat <p>**Örnek değer:** 01-29-2020 09:52:02.700</p> |
| EventSubscriptionName | Dize | Olay aboneliğinin adı <p>**Örnek değer:** "EVENTSUB1"</p> <p>Bu özellik yalnızca teslim hatası günlükleri için geçerlidir.</p>  |
| Kategori | Dize | Günlük kategorisi adı. <p>**Örnek değerler:** "Deliveryarızaları" veya "Publishhatalarıyla" | 
| ThrottledRequests | Dize | Hatayla karşılaşıldığında gerçekleştirilen işlemin adı.<p>**Örnek değerler:** Teslim hatalarının "teslim edilmesi". |
| İleti | Dize | Kullanıcının hatanın nedenini ve diğer ek ayrıntıları açıklayan günlük iletisi. |
| ResourceId | Dize | Konu/etki alanı kaynağı için kaynak KIMLIĞI<p>**Örnek değerler:**`/SUBSCRIPTIONS/SAMPLE-SUBSCRIPTION-ID/RESOURCEGROUPS/SAMPLE-RESOURCEGROUP/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/TOPIC1` |

## <a name="example"></a>Örnek

```json
{
    "time": "2019-11-01T00:17:13.4389048Z",
    "resourceId": "/SUBSCRIPTIONS/SAMPLE-SUBSCTIPTION-ID /RESOURCEGROUPS/SAMPLE-RESOURCEGROUP-NAME/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/SAMPLE-TOPIC-NAME ",
    "eventSubscriptionName": "SAMPLEDESTINATION",
    "category": "DeliveryFailures",
    "operationName": "Deliver",
    "message": "Message:outcome=NotFound, latencyInMs=2635, systemId=17284f7c-0044-46fb-84b7-59fda5776017, state=FilteredFailingDelivery, deliveryTime=11/1/2019 12:17:10 AM, deliveryCount=0, probationCount=0, deliverySchema=EventGridEvent, eventSubscriptionDeliverySchema=EventGridEvent, fields=InputEvent, EventSubscriptionId, DeliveryTime, State, Id, DeliverySchema, LastDeliveryAttemptTime, SystemId, fieldCount=, requestExpiration=1/1/0001 12:00:00 AM, delivered=False publishTime=11/1/2019 12:17:10 AM, eventTime=11/1/2019 12:17:09 AM, eventType=Type, deliveryTime=11/1/2019 12:17:10 AM, filteringState=FilteredWithRpc, inputSchema=EventGridEvent, publisher=DIAGNOSTICLOGSTEST-EASTUS.EASTUS-1.EVENTGRID.AZURE.NET, size=363, fields=Id, PublishTime, SerializedBody, EventType, Topic, Subject, FilteringHashCode, SystemId, Publisher, FilteringTopic, TopicCategory, DataVersion, MetadataVersion, InputSchema, EventTime, fieldCount=15, url=sb://diagnosticlogstesting-eastus.servicebus.windows.net/, deliveryResponse=NotFound: The messaging entity 'sb://diagnosticlogstesting-eastus.servicebus.windows.net/eh-diagnosticlogstest' could not be found. TrackingId:c98c5af6-11f0-400b-8f56-c605662fb849_G14, SystemTracker:diagnosticlogstesting-eastus.servicebus.windows.net:eh-diagnosticlogstest, Timestamp:2019-11-01T00:17:13, referenceId: ac141738a9a54451b12b4cc31a10dedc_G14:"
}
```

## <a name="next-steps"></a>Sonraki adımlar
Konular veya etki alanları için tanılama günlüklerini etkinleştirme hakkında bilgi edinmek için bkz. [tanılama günlüklerini etkinleştirme](enable-diagnostic-logs-topic.md).
