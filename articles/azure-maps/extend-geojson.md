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
ms.openlocfilehash: 98db10f0fc7a417f39d4bb00e77af6bdea034a03
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79276406"
---
# <a name="extended-geojson-geometries"></a>Genişletilmiş GeoJSON geometrileri

Azure Maps, coğrafi özelliklerin içinde ve üzerinde arama yapmak için güçlü API 'lerin bir listesini sağlar. Bu API 'Ler, coğrafi özellikleri temsil eden standart [geojson][1] belirtimine uyar.  

[Geojson belirtimi][1] yalnızca aşağıdaki geometrileri destekler:

* GeometryCollection
* LineString
* MultiLineString
* Çok nokta
* MultiPolygon
* Seçeneğinin
* Gen

Bazı Azure Maps API 'Leri, [geojson belirtiminin][1]parçası olmayan geometrileri kabul eder. Örneğin, geometri API 'si [Içinde arama](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) , daire ve çokgenler kabul eder.

Bu makalede, Azure eşlemelerinin belirli geometrileri göstermek için [geojson belirtimini][1] nasıl genişlettiği hakkında ayrıntılı bir açıklama sunulmaktadır.

## <a name="circle"></a>Daire

`Circle` geometrisi [geojson belirtimi][1]tarafından desteklenmiyor. Bir daireyi temsil etmek için bir `GeoJSON Point Feature` nesnesi kullanıyoruz.

`GeoJSON Feature` nesnesi kullanılarak temsil edilen bir `Circle` geometrisi aşağıdaki koordinatları ve __özellikleri içermelidir:__

- Center

    Dairenin merkezi bir `GeoJSON Point` nesnesi kullanılarak temsil edilir.

- La

    Dairenin `radius` `GeoJSON Feature`özellikleri kullanılarak temsil edilir. Yarıçap değeri _Ölçü_ cinsindendir ve `double`türünde olmalıdır.

- SubType

    Daire geometrisi `subType` özelliğini de içermelidir. Bu özellik `GeoJSON Feature`özelliklerinin bir parçası olmalıdır ve değeri _daire_ olmalıdır

#### <a name="example"></a>Örnek

Bir `GeoJSON Feature` nesnesini kullanarak bir daireyi nasıl temsil ettiğini aşağıda bulabilirsiniz. Bu çemberi Enlem: 47,639754 ve Boylam:-122,126986 ' ye kadar daire ortalayalım ve bu değere bir Radius atamak için 100.

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

`GeoJSON Polygon Feature` nesnesi kullanılarak temsil edilen bir `Rectangle` geometrisi aşağıdaki koordinatları ve __özellikleri içermelidir:__

- Elerinden

    Dikdörtgenin köşeleri, bir `GeoJSON Polygon` nesnesinin koordinatları kullanılarak temsil edilir. Her köşede birer tane olmak üzere beş koordinat olmalıdır. Ve çokgen halkasını kapatmak için ilk koordinat ile aynı olan beşinci bir koordinat. Bu koordinatların hizalandığından ve geliştiricinin istenen şekilde döndürebildiğinden emin olacak.

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
