---
title: Bölge sınırı için geojson veri biçimi | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure Maps ile bölge oluşturma API 'sinde kullanılabilen bölge verilerinin nasıl hazırlanacağı hakkında bilgi edineceksiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: f853962bba7302affd78d5ef267460893ea80a33
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911587"
---
# <a name="geofencing-geojson-data"></a>Bölge sınırlaması coğrafi JSON verileri

Azure haritalar [bölge](/rest/api/maps/spatial/getgeofence) sınırlaması ve [son bölge](/rest/api/maps/spatial/postgeofence) API 'leri, belirtilen bölge veya bir sınırlama kümesine göre bir koordinat yakınlığını almanızı sağlar. Bu makalede, Azure haritalar GET ve POST API 'sinde kullanılabilen bölge verilerinin nasıl hazırlanacağı anlatılmaktadır.

Bölge veya bölge kümesi için veriler, [rfc7946](https://tools.ietf.org/html/rfc7946)içinde tanımlanan `GeoJSON` biçiminde nesne ve `FeatureCollection` nesnesi `Feature` tarafından temsil edilir. Bunlara ek olarak:

* GeoJSON nesne türü bir `Feature` nesnesi veya bir `FeatureCollection` nesnesi olabilir.
* Geometri nesne türü bir `Point`, `MultiPoint`, `LineString`, `MultiLineString`, `Polygon`, `MultiPolygon`ve `GeometryCollection`olabilir.
* Tüm özellik özellikleri, bölge listesini tanımlamak için kullanılan bir `geometryId`içermelidir.
* `Point`, `MultiPoint`, `LineString``MultiLineString` özelliği, özelliklerde `radius` içermelidir. `radius` değeri ölçü cinsinden ölçülür, `radius` değeri 1 ile 10000 arasında değişir.
* `polygon` ve `multipolygon` geometrisi türündeki özelliğin bir Radius özelliği yok.
* `validityTime`, Kullanıcı bölge verileri için süresi dolmuş süreyi ve geçerlilik süresi dönemini ayarlamaya olanak sağlayan isteğe bağlı bir özelliktir. Belirtilmemişse, verilerin süresi süresiz olur ve her zaman geçerlidir.
* `expiredTime`, bölge sınırlaması verilerinin sona erme tarihidir ve zamanıdır. İstekteki `userTime` değeri bu değerden daha sonra ise, karşılık gelen bölge verileri, süre dolmayan veriler olarak değerlendirilir ve sorgulanmamıştır. Bu durumda, bu bölge verilerinin Geometryıd 'Si, bölge kapsamındaki `expiredGeofenceGeometryId` dizisine dahil edilir.
* `validityPeriod`, geofence 'nin geçerlilik süresi döneminin bir listesidir. İstekteki `userTime` değeri geçerlilik süresinin dışında kalırsa, ilgili bölge verileri geçersiz olarak kabul edilir ve sorgulanmaz. Bu bölge verilerinin Geometryıd 'si, bölge kimliği içindeki `invalidPeriodGeofenceGeometryId` dizisine dahildir. Aşağıdaki tabloda validityPeriod öğesinin özellikleri gösterilmektedir.

| Ad | Tür | Gereklidir  | Açıklama |
| :------------ |:------------: |:---------------:| :-----|
| startTime | Datetime  | doğru | Geçerlilik zaman döneminin başlangıç tarihi saati. |
| endTime   | Datetime  | doğru |  Geçerlilik zaman döneminin bitiş tarihi. |
| Numaralarına | string | yanlış |   Dönemin yinelenme türü. Değer `Daily`, `Weekly`, `Monthly`veya `Yearly`olabilir. Varsayılan değer `Daily`.|
| yalnızca BusinessDay | Boole | yanlış |  Verilerin yalnızca iş günleri için geçerli olup olmadığını belirtin. Varsayılan değer `false`.|


* Tüm koordinat değerleri, `WGS84`tanımlı [Boylam, Enlem] olarak gösterilir.
* `MultiPoint`, `MultiLineString`, `MultiPolygon` veya `GeometryCollection`içeren her özellik için, özellikler tüm öğelere uygulanır. Örneğin: `MultiPoint` tüm noktaları, birden çok daire bölge oluşturmak için aynı yarıçapı kullanacaktır.
* Nokta-daire senaryosunda, bir daire geometrisi, [GeoJSON geometrileri uzatımsundaki](https://docs.microsoft.com/azure/azure-maps/extend-geojson)özellikler ayrıntılı ile bir `Point` Geometry nesnesi kullanılarak temsil edilebilir.      

Aşağıda, bir orta nokta ve yarıçap kullanarak `GeoJSON` bir daire bölge alanı geometrisi olarak temsil edilen bölge için örnek bir istek gövdesi verilmiştir. Bölge verilerinin geçerli dönemi 2018-10-22, 00 ila 17:00, hafta sonu hariç her gün yinelenir. `expiredTime`, istekteki `userTime` `2019-01-01`daha sonra olması durumunda bu bölge verilerinin zaman aşımına alınmayacağını belirtir.  

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "geometryId" : "1",
        "subType": "Circle",
        "radius": 500,
        "validityTime": 
        {
            "expiredTime": "2019-01-01T00:00:00",
            "validityPeriod": [
                {
                    "startTime": "2018-10-22T09:00:00",
                    "endTime": "2018-10-22T17:00:00",
                    "recurrenceType": "Daily",
                    "recurrenceFrequency": 1,
                    "businessDayOnly": true
                }
            ]
        }
    }
}
```
