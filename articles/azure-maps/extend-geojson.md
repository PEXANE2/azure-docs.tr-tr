---
title: Genişletilmiş GeoJSON geometrileri | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar'ın GeoJSON spektrumlarını belirli geometrileri temsil edecek şekilde nasıl genişlettiklerini öğreneceksiniz.
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 98db10f0fc7a417f39d4bb00e77af6bdea034a03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276406"
---
# <a name="extended-geojson-geometries"></a>Genişletilmiş GeoJSON geometrileri

Azure Haritalar, coğrafi özellikler içinde ve boyunca arama yapmak için güçlü API'lerin bir listesini sağlar. Bu API'ler coğrafi özellikleri temsil eden standart [GeoJSON spec'e][1] bağlıdır.  

[GeoJSON spec][1] yalnızca aşağıdaki geometrileri destekler:

* Geometrycollection
* Linestring
* Multilinestring
* Çoklu
* Multipolygon
* Nokta
* Çokgen

Bazı Azure Haritalar API'ları [GeoJSON spec'in][1]bir parçası olmayan geometrileri kabul eder. Örneğin, [İç Geometri](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) IÇINDE Ara API Daire ve Çokgenler kabul eder.

Bu makalede, Azure Haritalar'ın [GeoJSON spektrumlarını][1] belirli geometrileri temsil edecek şekilde nasıl genişlettiklerine ilişkin ayrıntılı bir açıklama sağlanmaktadır.

## <a name="circle"></a>Daire

Geometri `Circle` [GeoJSON spec][1]tarafından desteklenmez. Bir daireyi temsil etmek için bir `GeoJSON Point Feature` nesne kullanırız.

Nesne kullanılarak temsil edilen bir `Circle` geometri aşağıdaki koordinatları ve özellikleri __içermelidir:__ `GeoJSON Feature`

- Merkez

    Dairenin ortası bir `GeoJSON Point` nesne kullanılarak temsil edilir.

- Yarıçap

    Dairenin özellikleri `radius` kullanılarak `GeoJSON Feature`temsil edilir. Yarıçap değeri _metre_ olarak ve türünde `double`olmalıdır.

- SubType

    Daire geometrisi de `subType` özelliği içermelidir. Bu özellik `GeoJSON Feature`'özelliklerinin bir parçası olmalı ve değeri _Circle_ olmalıdır

#### <a name="example"></a>Örnek

Bir `GeoJSON Feature` nesneyi kullanarak daireyi şu şekilde temsil eve göndereceğiz. Daireyi enlemde ortalayalım: 47.639754 ve boylam: -122.126986 ve 100 metreye eşit bir yarıçap atayalım:

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

Geometri `Rectangle` [GeoJSON spec][1]tarafından desteklenmez. Dikdörtgeni `GeoJSON Polygon Feature` temsil etmek için bir nesne kullanırız. Dikdörtgen uzantısı öncelikle Web SDK'nın çizim araçları modülü tarafından kullanılır.

Nesne kullanılarak temsil edilen bir `Rectangle` geometri aşağıdaki koordinatları ve özellikleri __içermelidir:__ `GeoJSON Polygon Feature`

- Köşe

    Dikdörtgenin köşeleri bir `GeoJSON Polygon` nesnenin koordinatları kullanılarak temsil edilir. Her köşe için bir tane olmak üzere beş koordinat olmalı. Ve, çokgen halkasını kapatmak için ilk koordinatla aynı olan beşinci koordinat. Bu koordinatların hizalanmış olduğu ve geliştiricinin bunları istendiği gibi döndürebileceği varsayılır.

- SubType

    Dikdörtgen geometrisi de `subType` özelliği içermelidir. Bu özellik `GeoJSON Feature`'özelliklerinin bir parçası olmalı ve değeri _Dikdörtgen_ olmalıdır

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

Azure Haritalar'da GeoJSON verileri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Geofence GeoJSON biçimi](geofence-geojson.md)

Azure Haritalar ve konum zekası uygulamalarıyla ilişkili yaygın teknik terimler sözlüğü gözden geçirin:

> [!div class="nextstepaction"]
> [Azure Haritalar sözlüğü](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
