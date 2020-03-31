---
title: Olay Izgarasını kullanarak harita olaylarına tepki verme | Microsoft Azure Haritaları
description: Bu makalede, Olay Grid'i kullanarak Microsoft Azure Haritalar etkinliklerine nasıl tepki vereceğinizi öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 02/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 9c9483af191e5439af0c0b5e433187d6475c178c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335725"
---
# <a name="react-to-azure-maps-events-by-using-event-grid"></a>Etkinlik Ağıt'ı kullanarak Azure Haritalar etkinliklerine tepki verme 

Azure Haritalar, kullanıcıların diğer hizmetlere olay bildirimleri gönderebilmeleri ve akış aşağı işlemlerini tetikleyebilmeleri için Azure Etkinlik Ağıt'ı ile tümleşir. Bu makalenin amacı, azure haritalar olaylarını dinleyecek şekilde iş uygulamalarınızı yapılandırmanıza yardımcı olmaktır. Bu, kullanıcıların kritik olaylara güvenilir, ölçeklenebilir ve güvenli bir şekilde tepki vermelerini sağlar. Örneğin, kullanıcılar bir veritabanını güncelleştirmek, bilet oluşturmak ve bir aygıt coğrafi çitlere her girdiğinde bir e-posta bildirimi sunmak için bir uygulama oluşturabilir.

Azure Olay Ağıt, yayımlama-abone etme modelini kullanan tam olarak yönetilen bir olay yönlendirme hizmetidir. Olay Ağı, [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) ve [Azure Mantıksal Uygulamaları](https://docs.microsoft.com/azure/azure-functions/functions-overview)gibi Azure hizmetleri için yerleşik destek içerir. Web hooks kullanarak Azure olmayan hizmetlere olay uyarıları sunabilir. Olay Izgarası'nın desteklediği olay işleyicilerinin tam listesi için Azure [Olay Izgarasına giriş bölümüne](https://docs.microsoft.com/azure/event-grid/overview)bakın.


![Azure Olay Izgarası fonksiyonel modeli](./media/azure-maps-event-grid-integration/azure-event-grid-functional-model.png)


## <a name="azure-maps-events-types"></a>Azure Haritalar etkinlik türleri

Olay [ızgarası,](https://docs.microsoft.com/azure/event-grid/concepts#event-subscriptions) olay iletilerini abonelere yönlendirmek için olay aboneliklerini kullanır. Azure Haritalar hesabı aşağıdaki olay türlerini yayır: 

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | Alınan koordinatlar belirli bir geofence dışından içinde taşındı zaman yükseltilmiş |
| Microsoft.Maps.GeofenceÇıktı | Alınan koordinatlar belirli bir coğrafi çitin içinden dışarıya taşındığında yükseltilmiş |
| Microsoft.Maps.GeofenceResult | Duruma bakılmaksızın, geofencing sorgusu her sonuç döndürdüğünde yükseltildi |

## <a name="event-schema"></a>Olay şeması

Aşağıdaki örnek GeofenceResult için şema gösterir:

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

## <a name="tips-for-consuming-events"></a>Etkinlikleri tüketmek için ipuçları

Azure Haritalar coğrafi çit olaylarını işleyen uygulamalar, önerilen birkaç uygulamayı izlemelidir:

* Olayları aynı olay işleyicisine yönlendirmek için birden çok abonelik yapılandırın. Olayların belirli bir kaynaktan geldiğini varsaymamak önemlidir. İletinin beklediğiniz kaynaktan geldiğinden emin olmak için her zaman ileti konusunu denetleyin.
* Nesneler `X-Correlation-id` hakkındaki bilgilerinizin güncel olup olmadığını anlamak için yanıt üstbilgisindeki alanı kullanın. İletiler sıradışı veya gecikmeden sonra gelebilir.
* Geofence API'daki bir GET veya POST isteği mod parametresi kümesi yle `EnterAndExit`çağrıldığında, durumu önceki Geofence API çağrısından değiştirilen geofence'deki her geometri için bir Enter veya Exit olayı oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

Bir şantiyedeki işlemleri denetlemek için geofencing'i nasıl kullanacağınız hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"] 
> [Azure Haritalar'ı kullanarak coğrafi çit oluşturma](tutorial-geofence.md)
