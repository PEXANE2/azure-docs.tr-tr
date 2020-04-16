---
title: Olay Izgara kaynağı olarak Azure Haritalar
description: Azure Etkinlik Ağıtı ile Azure Haritalar etkinlikleri için sağlanan özellikleri ve şema açıklanır
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: babanisa
ms.openlocfilehash: e879ec3442f2e7912acb450a97079d80d7d95a01
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393413"
---
# <a name="azure-maps-as-an-event-grid-source"></a>Olay Izgara kaynağı olarak Azure Haritalar

Bu makalede, Azure Haritalar olaylarının özellikleri ve şeması sağlanmaktadır. Etkinlik şemalarına giriş için [Azure Olay Izgara olay şemasına](https://docs.microsoft.com/azure/event-grid/event-schema)bakın. Ayrıca, Azure Haritalar'ı etkinlik kaynağı olarak kullanmanız için hızlı başlangıçların ve öğreticilerin bir listesini de verir.

## <a name="event-grid-event-schema"></a>Olay Izgara olay şeması

### <a name="available-event-types"></a>Kullanılabilir etkinlik türleri

Azure Haritalar hesabı aşağıdaki olay türlerini yayır:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Maps.GeofenceEntered | Alınan koordinatlar belirli bir geofence dışından içinde taşındı zaman yükseltilmiş |
| Microsoft.Maps.GeofenceÇıktı | Alınan koordinatlar belirli bir coğrafi çitin içinden dışarıya taşındığında yükseltildi |
| Microsoft.Maps.GeofenceResult | Duruma bakılmaksızın, geofencing sorgusu her sonuç döndürdüğünde yükseltildi |

### <a name="event-examples"></a>Olay örnekleri

Aşağıdaki örnek, bir **GeofenceEntered** olayının şema gösterir

```JSON
{   
   "id":"7f8446e2-1ac7-4234-8425-303726ea3981", 
   "topic":"/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Maps/accounts/{accountName}", 
   "subject":"/spatial/geofence/udid/{udid}/id/{eventId}", 
   "data":{   
      "geometries":[   
         {   
            "deviceId":"device_1", 
            "udId":"1a13b444-4acf-32ab-ce4e-9ca4af20b169", 
            "geometryId":"2", 
            "distance":-999.0, 
            "nearestLat":47.618786, 
            "nearestLon":-122.132151 
         } 
      ], 
      "expiredGeofenceGeometryId":[   
      ], 
      "invalidPeriodGeofenceGeometryId":[   
      ] 
   }, 
   "eventType":"Microsoft.Maps.GeofenceEntered", 
   "eventTime":"2018-11-08T00:54:17.6408601Z", 
   "metadataVersion":"1", 
   "dataVersion":"1.0" 
}
```

Aşağıdaki örnek **GeofenceResult** için şema göstermek 

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

### <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağına tam kaynak yolu. Bu alan yazılamaz. Event Grid bu değeri sağlar. |
| Konu | string | Olay konusunun yayımcı tarafından tanımlanan yolu. |
| Eventtype | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın sağlayıcının UTC zamanına bağlı olarak oluşturulan süre. |
| id | string | Etkinlik için benzersiz tanımlayıcı. |
| veri | object | Olay verilerini geofencing. |
| dataVersion | string | Veri nesnesinin şema sürümü. Şema sürümünü yayımcı tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid en üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| apiKategori | string | Olayın API kategorisi. |
| apiName | string | Olayın API adı. |
| sorunlar | object | İşlemsırasında karşılaşılan sorunları listeler. Herhangi bir sorun döndürülürse, yanıtla birlikte döndürülen geometriler olmaz. |
| responseCode | number | HTTP yanıt kodu |
| Geometri | object | Koordinat konumunu içeren veya aramaBuffer'ın konumu etrafındaki çakışan çit geometrilerini listeler. |

Haritalar API'sında bir hata oluştuğunda hata nesnesi döndürülür. Hata nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| error | ErrorDetails |Bu nesne, Haritalar API'sında bir hata oluştuğunda döndürülür  |

Hatalar Ayrıntıları nesnesi, Haritalar API'sında bir hata oluştuğunda döndürülür. Hata Ayrıntıları veya nesneaşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| kod | string | HTTP durum kodu. |
| message | string | Varsa, hatanın okunabilir bir açıklaması. |
| innererror | İç Hata | Varsa, hata hakkında hizmete özgü bilgiler içeren bir nesne. |

InnerError hata hakkında hizmete özgü bilgiler içeren bir nesnedir. InnerError nesnesi aşağıdaki özelliklere sahiptir: 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| kod | string | Hata iletisi. |

Geometriler nesnesi, istekteki kullanıcı süresine göre süresi dolmuş geofences geometri künyelerini listeler. Geometriler nesnesi aşağıdaki özelliklere sahip geometri öğeleri vardır: 

| Özellik | Tür | Açıklama |
|:-------- |:---- |:----------- |
| Deviceıd | string | Cihazın kimliği. |
| Mesafe | string | <p>Koordinattan geofence en yakın sınıra mesafe. Pozitif, koordinatın geofence'in dışında olduğu anlamına gelir. Koordinat geofence dışında, ancak arama Buffer değerinden daha uzak en yakın geofence sınırından ise, o zaman değeri 999 olduğunu. Negatif, koordinatın geofence'in içinde olduğu anlamına gelir. Koordinat çokgenin içindeyse, ancak aramanın değerinden daha fazlaysaBuffer en yakın geofencing sınırından uzaktaysa, değeri -999'dur. 999 değeri, koordinatın geofence'in çok dışında olduğuna dair büyük bir güven olduğu anlamına gelir. -999 değeri, koordinatın geofence içinde iyi olduğu büyük bir güven olduğu anlamına gelir.<p> |
| geometriid |string | Benzersiz kimlik geofence geometrisi tanımlar. |
| nearestlat | number | Geometrinin en yakın noktasının enlemi. |
| nearestlon | number | Geometrinin en yakın noktasının boylam. |
| udId | string | Benzersiz kimlik, bir geofence yüklerken kullanıcı yükleme hizmetinden döndürülür. Geofencing sonrası API'ye dahil edilmez. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| süresi dolmuşGeofenceGeometryId | dize[] | İsteğe göre süresi dolan geofence geometri kimliğinin listeleri. |
| Geometri | geometriler[] |Koordinat konumunu içeren veya aramaBuffer'ın konumu etrafındaki çakışan çit geometrilerini listeler. |
| geçersizPeriodGeofenceGeometryId | dize[]  | İsteğe göre geçersiz bir dönemde olan geofence geometri kimliği listeleri. |
| isOlayPublished | boole | Azure Haritalar etkinlik abonesine en az bir olay yayınlanırsa, Azure Haritalar etkinlik abonesine hiçbir olay yayınlanmamışsa yanlış olur. |

## <a name="tutorials-and-how-tos"></a>Öğreticiler ve nasıl yapılır kılavuzları
|Başlık  |Açıklama  |
|---------|---------|
| [Etkinlik Ağıt'ı kullanarak Azure Haritalar etkinliklerine tepki verme](../azure-maps/azure-maps-event-grid-integration.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Azure Haritalarını Olay Ağıtı ile tümleştirmeye genel bakış. |
| [Öğretici: Geofence'i ayarlama](../azure-maps/tutorial-geofence.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Bu öğretici, Azure Haritalar'ı kullanarak coğrafi çit oluşturmanız için temel adımlardan geçmenizi sağlar. Geofence sonuçlarını akış ve geofence sonuçlarına dayalı bir bildirim ayarlamak için Azure Event Grid'i kullanırsınız. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Etkinlik Izgarasına giriş için [olay ızgarası nedir?](overview.md)
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.