---
title: Genişletilmiş GeoJSON geometrileri | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure eşlemelerinin coğrafi JSON belirtimini belirli geometriler temsil edecek şekilde nasıl genişlettiğini öğreneceksiniz.
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 75ad83411edfdfe7545e8f80df17fea56e317ee0
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911647"
---
# <a name="extended-geojson-geometries"></a>Genişletilmiş GeoJSON geometrileri

Azure Maps, coğrafi özellikler içinde/içinde arama yapmak için güçlü API 'lerin bir listesini sağlar.
Bu API 'Ler coğrafi özellikleri temsil etmek için [geojson spec][1] üzerinde standartlaştırın (örneğin: durum sınırları, rotalar).  

[Geojson özelliği][1] yalnızca aşağıdaki geometrileri destekler:

* GeometryCollection
* LineString
* MultiLineString
* Çok nokta
* MultiPolygon
* Seçeneğinin
* Gen

Bazı Azure Maps API 'Leri (örneğin, [geometri Içinde arama](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry)), [geojson belirtiminin][1]parçası olmayan "Circle" gibi geometriler kabul eder.

Bu makalede, Azure eşlemelerinin belirli geometrileri göstermek için [geojson belirtimini][1] nasıl genişlettiği hakkında ayrıntılı bir açıklama sunulmaktadır.

## <a name="circle"></a>Daire

`Circle` geometrisi [geojson belirtimi][1]tarafından desteklenmiyor. Bir daireyi temsil etmek için bir `GeoJSON Point Feature` nesnesi kullanıyoruz.

`GeoJSON Feature` nesnesi kullanılarak temsil edilen bir `Circle` geometrisi __şunları içermelidir:__

- Center

    Dairenin merkezi bir `GeoJSON Point` nesnesi kullanılarak temsil edilir.

- Yarıçap

    Dairenin `radius` `GeoJSON Feature`özellikleri kullanılarak temsil edilir. Yarıçap değeri _Ölçü_ cinsindendir ve `double`türünde olmalıdır.

- SubType

    Daire geometrisi `subType` özelliğini de içermelidir. Bu özellik `GeoJSON Feature`özelliklerinin bir parçası olmalıdır ve değeri _daire_ olmalıdır

#### <a name="example"></a>Örnek

`GeoJSON Feature` bir nesne kullanarak, bir yarıçapı (Latitude: 47,639754, Boylam:-122,126986) ile bir Radius 100 eşit olan bir daire olarak temsil edebilirsiniz:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

## <a name="rectangle"></a>Dikdörtgen

`Rectangle` geometrisi [geojson belirtimi][1]tarafından desteklenmiyor. Bir dikdörtgeni temsil etmek için bir `GeoJSON Polygon Feature` nesnesi kullanıyoruz. Dikdörtgen uzantısı öncelikle web SDK 'nın çizim araçları modülü tarafından kullanılır.

`GeoJSON Polygon Feature` nesnesi kullanılarak temsil edilen bir `Rectangle` geometrisi __şunları içermelidir:__

- Elerinden

    Dikdörtgenin köşeleri, bir `GeoJSON Polygon` nesnesinin koordinatları kullanılarak temsil edilir. Her köşede biri olan beş koordinat ve çokgen halkasını kapatmak için 1 ile aynı olan beşinci bir koordinat olmalıdır. Bu koordinatların, geliştirici tarafından istendiği şekilde hizalanıp döndürülmeyeceğini varsayacaktır.

- SubType

    Dikdörtgen geometrisi `subType` özelliğini de içermelidir. Bu özellik `GeoJSON Feature`özelliklerinin bir parçası olmalıdır ve değeri _dikdörtgen_ olmalıdır

### <a name="example"></a>Örnek

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [[[5,25],[14,25],[14,29],[5,29],[5,25]]]
    },
    "properties": {
        "subType": "Rectangle"
    }
}

```
## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar 'da coğrafi JSON verileri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Geofence GeoJSON biçimi](geofence-geojson.md)

Azure haritalar ve konum zekası uygulamalarıyla ilişkili ortak teknik terimlerin sözlüğünü gözden geçirin:

> [!div class="nextstepaction"]
> [Azure haritalar sözlüğü](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
