---
title: Azure Maps olay şemasını Azure Event Grid
description: Azure Event Grid ile Azure Maps olayları için belirtilen özellikleri ve şemayı açıklar
services: event-grid
author: femila
ms.service: event-grid
ms.topic: reference
ms.date: 02/08/2019
ms.author: femila
ms.openlocfilehash: 9acef524521e8fac6ce6f8f61e5ff3fbbb81d18d
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77486368"
---
# <a name="azure-event-grid-event-schema-for-azure-maps"></a>Azure haritalar için Azure Event Grid olay şeması

Bu makalede, Azure Maps olayları için özellikler ve şema sağlanmaktadır. Olay şemalarına giriş için bkz. [Azure Event Grid olay şeması](https://docs.microsoft.com/azure/event-grid/event-schema).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

Azure haritalar hesabı aşağıdaki olay türlerini yayar:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft. maps. Geofencegirildi | Alınan Koordinatlar, belirli bir bölge sınırı 'ın içinden içine arasında taşındığında tetiklenir |
| Microsoft. maps. Geofenceçıkıldı | Alınan koordinatlar belirli bir bölge içinden dışarıya taşındığında tetiklenir |
| Microsoft. maps. GeofenceResult | Bölge sınırlama sorgusu, durumdan bağımsız olarak bir sonuç döndürdüğünde tetiklenir |

## <a name="event-examples"></a>Olay örnekleri

Aşağıdaki örnek, **Geofencegirildi** olayının şemasını gösterir

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

Aşağıdaki örnek **GeofenceResult** için şemayı gösterir 

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

## <a name="event-properties"></a>Olay özellikleri

Bir olay aşağıdaki en üst düzey verilere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağının tam kaynak yolu. Bu alan yazılabilir değil. Event Grid bu değeri sağlar. |
| subject | string | Olay konusunun yayımcı tanımlı yolu. |
| eventType | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Etkinliğin UTC saatine göre oluşturulduğu zaman. |
| id | string | Etkinliğin benzersiz tanımlayıcısı. |
| data | object | Bölge sınırlaması olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümünü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid üst düzey özelliklerin şemasını tanımlar. Event Grid bu değeri sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| apiCategory | string | Etkinliğin API kategorisi. |
| apiName | string | Etkinliğin API adı. |
| sorunlar | object | İşlem sırasında karşılaşılan sorunları listeler. Herhangi bir sorun döndürülürse, yanıtla birlikte hiçbir geometriler döndürülmeyecektir. |
| responseCode | number | HTTP yanıt kodu |
| geometriler | object | Koordinat konumunu içeren sınır geometrileri listeler veya konumun çevresindeki searchBuffer ile çakışıyor. |

Maps API 'sinde bir hata oluştuğunda hata nesnesi döndürülür. Hata nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| error | ErrorDetails |Bu nesne, Haritalar API 'sinde bir hata oluştuğunda döndürülür  |

Maps API 'sinde bir hata oluştuğunda ErrorDetails nesnesi döndürülür. ErrorDetails veya nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| code | string | HTTP durum kodu. |
| message | string | Kullanılabiliyorsa, hatanın okunabilir bir açıklaması. |
| ınnererror | Innererror | Varsa, hatayla ilgili hizmete özel bilgileri içeren bir nesne. |

Innererror, hatayla ilgili hizmete özel bilgileri içeren bir nesnedir. Innererror nesnesi aşağıdaki özelliklere sahiptir: 

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| code | string | Hata iletisi. |

Geometriler nesnesi, istekteki Kullanıcı saatine göre süresi geçen bölge sayısının geometri kimliklerini listeler. Geometriler nesnesi, aşağıdaki özelliklere sahip geometri öğelerine sahiptir: 

| Özellik | Tür | Açıklama |
|:-------- |:---- |:----------- |
| deviceid | string | Cihazın KIMLIĞI. |
| distance | string | <p>Koordinatlardan bölge alanının en yakın kenarlığına olan uzaklık. Pozitif, koordinat bölge sayısının dışında olduğu anlamına gelir. Koordinat Bölge sınırının dışında, ancak en yakın bölge genişliğinden daha fazla searchBuffer değeri varsa, değer 999 ' dir. Negatif, koordinat bölge sayısının içinde olduğu anlamına gelir. Koordinat Çokgen içindeyse, ancak en yakın bölge sınırlaması kenarından daha fazla searchBuffer değerinden fazla olursa değer-999 olur. 999 değeri, koordinat bölge alanının dışında iyi bir güvenilirlik olduğu anlamına gelir. -999 değeri, koordinatın bölge kapsamında iyi olduğu anlamına gelir.<p> |
| geometryıd |string | Benzersiz kimlik, bölge sınırı geometrisini tanımlar. |
| nearestlat | number | En yakın geometri noktası enlem. |
| nearestlon | number | Geometri 'nın en yakın noktasının boylam. |
| udId | string | Bölge, karşıya yüklenirken Kullanıcı karşıya yükleme hizmetinden döndürülen benzersiz kimlik. , Bölge sınırlaması Post API 'sine dahil edilmez. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| expiredGeofenceGeometryId | String [] | İstekteki Kullanıcı saatine göre süresi dolmak üzere olan bölge sınırı 'ın geometri kimliği listesi. |
| geometriler | geometriler [] |Koordinat konumunu içeren sınır geometrileri listeler veya konumun çevresindeki searchBuffer ile çakışıyor. |
| ınvalidperiodgeofencegeometryıd | String []  | İstekteki Kullanıcı saatine göre geçersiz süre içinde olan bölge sınırı 'ın geometri kimliği listesi. |
| ıventventyayımlandı | boole | Azure Maps olay abonesi 'nda en az bir olay yayımlandıysa true, Azure Maps olay abonesi 'nda hiçbir olay yayımlanmamışsa false. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid abonelik şeması](subscription-creation-schema.md).