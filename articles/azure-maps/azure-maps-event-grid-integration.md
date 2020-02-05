---
title: Event Grid kullanarak eşleme olaylarına tepki verme | Microsoft Azure haritaları
description: Bu makalede, Event Grid kullanarak Microsoft Azure Maps olaylarına nasıl tepki alabileceğinizi öğreneceksiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 02/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a89983a9ae45f21deb7a823de049373b4ff9b935
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989068"
---
# <a name="react-to-azure-maps-events-by-using-event-grid"></a>Event Grid kullanarak Azure Maps olaylarına tepki verme 

Azure Maps, kullanıcıların diğer hizmetlere olay bildirimleri gönderebilmesi ve aşağı akış süreçlerini tetikleyebilmeleri için Azure Event Grid ile tümleşir. Bu makalenin amacı, Azure Maps olaylarını dinlemek için iş uygulamalarınızı yapılandırmanıza yardımcı olur. Bu hizmet, kritik olaylara güvenilir, ölçeklenebilir ve güvenli bir şekilde tepki vermenize olanak tanır. Örneğin, kullanıcılar bir cihaz bölge alanı her girdiğinde bir veritabanını güncelleştirmek, Bilet oluşturmak ve bir e-posta bildirimi göndermek için bir uygulama oluşturabilir.

Azure Event Grid, yayımlama-abonelik modeli kullanan, tam olarak yönetilen bir olay yönlendirme hizmetidir. Event Grid [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) ve [Azure Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-overview)gibi Azure hizmetleri için yerleşik desteğe sahiptir. Web kancalarını kullanarak Azure olmayan hizmetlere olay uyarıları sunabilir. Event Grid desteklediği olay işleyicilerinin tüm listesi için bkz. [Azure Event Grid giriş](https://docs.microsoft.com/azure/event-grid/overview).


![Azure Event Grid işlevsel modeli](./media/azure-maps-event-grid-integration/azure-event-grid-functional-model.png)


## <a name="azure-maps-events-types"></a>Azure haritalar olay türleri

Olay Kılavuzu, olay iletilerini abonelere yönlendirmek için [olay abonelikleri](https://docs.microsoft.com/azure/event-grid/concepts#event-subscriptions) kullanır. Azure haritalar hesabı aşağıdaki olay türlerini yayar: 

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft. maps. Geofencegirildi | Alınan Koordinatlar, belirli bir bölge sınırı 'ın içinden içine arasında taşındığında tetiklenir |
| Microsoft. maps. Geofenceçıkıldı | Alınan koordinatlar belirli bir bölge içinden dışarıya taşındığında tetiklenir |
| Microsoft. maps. GeofenceResult | Bölge sınırlama sorgusu, durumdan bağımsız olarak bir sonuç döndürdüğünde tetiklenir |

## <a name="event-schema"></a>Olay şeması

Aşağıdaki örnek, GeofenceResult için şemayı gösterir:

```JSON
{   
   "id":"451675de-a67d-4929-876c-5c2bf0b2c000", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"1", 
            "distance":999.0, 
            "nearestLat":47.609833, 
            "nearestLon":-122.148274 
         }, 
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":999.0, 
            "nearestLat":47.621954, 
            "nearestLon":-122.131841 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceResult", 
   "eventTime":"2018-11-08T00:52:08.0954283Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

## <a name="tips-for-consuming-events"></a>Olayları tüketme ipuçları

Azure haritalar bölge olaylarını işleyen uygulamalar, önerilen birkaç uygulamayı izlemelidir:

* Aynı olay işleyicisine olayları yönlendirmek için birden çok abonelik yapılandırılabilir. Olayların belirli bir kaynaktan olduğunu varsaymamak önemlidir. Beklenen kaynaktan geldiğinden emin olmak için ileti konusunu her zaman denetleyin.
* İletiler bir gecikmeden veya bir gecikmeden sonra gelebilir. Nesneler hakkında bilgilerinizin güncel olup olmadığını anlamak için yanıt üstbilgisindeki `X-Correlation-id` alanını kullanın.
* Get ve post bölge sınırı API 'si `EnterAndExit`olarak ayarlanan mod parametresi ile çağrıldığında, durumun önceki bölge API çağrısından değiştiği bölge alanındaki her bir geometri için bir Enter veya Exit olayı oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

Bir oluşturma sitesindeki işlemleri denetlemek için bölge sınırlaması kullanma hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"] 
> [Azure haritalar 'ı kullanarak bölge kümesi oluşturma](tutorial-geofence.md)
