---
title: Geofence için GeoJSON veri formatı | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar GET ve POST Geofence API'sında kullanılabilecek geofence verilerinin nasıl hazırlanacağı hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 02/14/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 7b9860908dd3bdf3dcda727f350578a97b890cac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335612"
---
# <a name="geofencing-geojson-data"></a>GeoJSON verilerinin geofencing

Azure Haritalar [GET Geofence](/rest/api/maps/spatial/getgeofence) ve POST [Geofence](/rest/api/maps/spatial/postgeofence) API'leri, sağlanan bir coğrafi çit veya çit kümesine göre bir koordinatın yakınlığını geri almanıza olanak tanır. Bu makalede, Azure Haritalar GET ve POST API'sinde kullanılabilecek coğrafi çit verilerinin nasıl hazırlanacağı ayrıntılı olarak açıklanmaktadır.

Geofence veya geofences kümesi için veri `Feature` `FeatureCollection` [rfc7946](https://tools.ietf.org/html/rfc7946)tanımlanan `GeoJSON` biçiminde Nesne ve Nesne tarafından temsil edilir. Buna ek olarak:

* GeoJSON Nesne türü bir `Feature` Nesne `FeatureCollection` veya Nesne olabilir.
* Geometri Nesnesi türü `Point` `MultiPoint`, `LineString` `MultiLineString`, `Polygon` `MultiPolygon`, `GeometryCollection`, , ve .
* Tüm özellik özellikleri `geometryId`geofence tanımlamak için kullanılan bir , içermelidir.
* Özelliği `Point`ile `MultiPoint` `LineString`, `MultiLineString` , `radius` , özellikleri içermelidir. `radius`değeri metre cinsinden ölçülür, `radius` değer 1 ile 10000 arasında değişir.
* Özellik `polygon` ve `multipolygon` geometri türü ile bir yarıçap özelliği yoktur.
* `validityTime`kullanıcının geofence verileri için süresi dolmuş süre ve geçerlilik süresini ayarlamasına olanak tanıyan isteğe bağlı bir özelliktir. Belirtilmemişse, verilerin süresi asla dolmaz ve her zaman geçerlidir.
* Geofencing `expiredTime` verilerinin son kullanma tarihi ve saatidir. `userTime` İstekteki değer bu değerden daha geç ise, ilgili geofence verileri süresi dolmuş veri olarak kabul edilir ve sorgulanmaz. Bunun üzerine, bu geofence verilerinin geometriKimliği `expiredGeofenceGeometryId` geofence yanıtı içinde dizi ye dahil edilecektir.
* Geofence `validityPeriod` geçerlilik süresi listesidir. `userTime` İstekteki değer geçerlilik süresinin dışına düşerse, ilgili geofence verileri geçersiz sayılır ve sorgulanmaz. Bu geofence verilerinin geometriKimliği geofence yanıtı içinde dizi ye `invalidPeriodGeofenceGeometryId` dahildir. Aşağıdaki tablo geçerlilik Dönem öğesiözelliklerini gösterir.

| Adı | Tür | Gerekli  | Açıklama |
| :------------ |:------------: |:---------------:| :-----|
| startTime | Tarih saat  | true | Geçerlilik süresinin başlangıç tarihi. |
| endTime   | Tarih saat  | true |  Geçerlilik süresinin bitiş tarihi. |
| nüksYazın | string | yanlış |   Dönemin yineleme türü. Değer `Daily`, , `Weekly` `Monthly`, `Yearly`veya . Varsayılan değer. `Daily`|
| businessDayOnly | Boole | yanlış |  Verilerin yalnızca iş günlerinde geçerli olup olmadığını belirtin. Varsayılan değer. `false`|


* Tüm koordinat değerleri [boylam, enlem] olarak `WGS84`tanımlanır.
* Özellikleri `MultiPoint`içeren `MultiLineString` `MultiPolygon` `GeometryCollection`her Özellik için tüm öğelere uygulanır. örneğin: Tüm noktalar, `MultiPoint` birden çok daireli bir geofence oluşturmak için aynı yarıçapı kullanır.
* Nokta-daire senaryosunda, bir daire geometrisi `Point` [GeoJSON geometrilerini Genişletme](https://docs.microsoft.com/azure/azure-maps/extend-geojson)özellikleri yle ayrıntılı bir geometri nesnesi kullanılarak temsil edilebilir.      

Aşağıda bir merkez noktası ve yarıçapı `GeoJSON` kullanarak bir daire geofence geometri olarak temsil edilen bir geofence için bir örnek istek gövdesidir. Geofence verilerinin geçerli dönemi 2018-10-22, 09:00-17:00, hafta sonu hariç her gün tekrarlanır başlar. `expiredTime`bu geofence verileri süresi dolmuş `userTime` olarak kabul edilecektir gösterir, istek daha sonra ise `2019-01-01`.  

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
