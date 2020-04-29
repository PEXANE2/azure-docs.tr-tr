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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79276406"
---
# <a name="extended-geojson-geometries"></a>Genişletilmiş GeoJSON geometrileri

Azure Maps, coğrafi özelliklerin içinde ve üzerinde arama yapmak için güçlü API 'lerin bir listesini sağlar. Bu API 'Ler, coğrafi özellikleri temsil eden standart [geojson][1] belirtimine uyar.  

[Geojson belirtimi][1] yalnızca aşağıdaki geometrileri destekler:

* GeometryCollection
* LineString
* MultiLineString
* Noktalı
* MultiPolygon
* Seçeneğinin
* Gen

Bazı Azure Maps API 'Leri, [geojson belirtiminin][1]parçası olmayan geometrileri kabul eder. Örneğin, geometri API 'si [Içinde arama](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) , daire ve çokgenler kabul eder.

Bu makalede, Azure eşlemelerinin belirli geometrileri göstermek için [geojson belirtimini][1] nasıl genişlettiği hakkında ayrıntılı bir açıklama sunulmaktadır.

## <a name="circle"></a>Daire

`Circle` Geometri, [geojson belirtimi][1]tarafından desteklenmiyor. Bir daireyi temsil `GeoJSON Point Feature` etmek için bir nesnesi kullanıyoruz.

Nesnesini kullanarak temsil edilen bir `Circle` geometri aşağıdaki koordinatları ve __özellikleri içermelidir:__ `GeoJSON Feature`

- Merkez

    Dairenin merkezi bir `GeoJSON Point` nesne kullanılarak temsil edilir.

- Yarıçap

    Dairenin `radius` özellikleri kullanılarak `GeoJSON Feature`temsil edilir. Yarıçap değeri _Ölçü_ cinsindendir ve türünde `double`olmalıdır.

- SubType

    Daire geometrisi `subType` özelliği de içermelidir. Bu özellik, `GeoJSON Feature`özelliklerinin bir parçası olmalıdır ve değeri _daire_ olmalıdır

#### <a name="example"></a>Örnek

Bir `GeoJSON Feature` nesneyi kullanarak bir daireyi nasıl temsil ettiğini aşağıda bulabilirsiniz. Bu çemberi Enlem: 47,639754 ve Boylam:-122,126986 ' ye kadar daire ortalayalım ve bu değere bir Radius atamak için 100.

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

`Rectangle` Geometri, [geojson belirtimi][1]tarafından desteklenmiyor. Bir dikdörtgeni temsil `GeoJSON Polygon Feature` etmek için bir nesnesi kullanıyoruz. Dikdörtgen uzantısı öncelikle web SDK 'nın çizim araçları modülü tarafından kullanılır.

Nesnesini kullanarak temsil edilen bir `Rectangle` geometri aşağıdaki koordinatları ve __özellikleri içermelidir:__ `GeoJSON Polygon Feature`

- Elerinden

    Dikdörtgenin köşeleri `GeoJSON Polygon` nesnenin koordinatları kullanılarak temsil edilir. Her köşede birer tane olmak üzere beş koordinat olmalıdır. Ve çokgen halkasını kapatmak için ilk koordinat ile aynı olan beşinci bir koordinat. Bu koordinatların hizalandığından ve geliştiricinin istenen şekilde döndürebildiğinden emin olacak.

- SubType

    Dikdörtgen geometrisi `subType` özelliği de içermelidir. Bu özellik, özelliklerinin bir parçası `GeoJSON Feature`olmalıdır ve değeri _dikdörtgen_ olmalıdır

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
> [Azure Haritalar sözlüğü](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
