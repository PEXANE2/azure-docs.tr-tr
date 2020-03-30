---
title: Desteklenen veri biçimi ayrıntıları | Microsoft Azure Haritaları
description: Uzamsal IO modülünde uzamsal verilerin nasıl ayrıştırılmış olduğunu öğrenin.
author: philmea
ms.author: philmea
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 3353620f1751e939a04543115fe704555fb3bc21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334093"
---
# <a name="supported-data-format-details"></a>Desteklenen veri biçimi ayrıntıları

Bu makalede, tüm XML etiketleri ve Bilinen Metin geometritürleri için okuma ve yazma desteği ayrıntıları nı sağlar. Ayrıca uzamsal iO modülünde sınırlı uzamsal verilerin nasıl ayrıştırılan ayrıntıları.

## <a name="supported-xml-namespaces"></a>Desteklenen XML ad alanları

Uzamsal IO modülü aşağıdaki ad alanlarından XML etiketlerini destekler.

| Ad Alanı Öneki | Namespace URI   | Notlar                                                                    |
|:------------------|:-----------------|:----------------------------------------|
| `atom`           | `http://www.w3.org/2005/Atom`   |                                         |
| `geo`            | `http://www.w3.org/2003/01/geo/wgs84_pos#`  | Yalnızca GeoRSS dosyalarında destek okuyun.           |
| `georss`         | `http://www.georss.org/georss`  |                                                |
| `geourl`         | `http://geourl.org/rss/module/` | Yalnızca GeoRSS dosyalarında destek okuyun.                       |
| `gml`            | `http://www.opengis.net/gml`    |                                                        |
| `gpx`            | `http://www.topografix.com/GPX/1/1` |                                                   |
| `gpxx`           | `http://www.garmin.com/xmlschemas/GpxExtensions/v3` | Yalnızca GPX dosyalarında destek okuyun. Ayrışma ve DisplayColor kullanır. Meta verileri şekillendirmek için eklenen diğer tüm özellikler. |
| `gpx_style`      | `http://www.topografix.com/GPX/gpx_style/0/2`      | GPX dosyalarında desteklenir. Çizgi rengini kullanır. |
| `gx`             | `http://www.google.com/kml/ext/2.2` |                                                      |
| `kml`            | `http://www.opengis.net/kml/2.2`    |                                                      |
| `rss`            |                                 | Sadece okuyun. GeoRSS Atom biçimini kullanarak yazar.              |

## <a name="supported-xml-elements"></a>Desteklenen XML elemanları

Uzamsal IO modülü aşağıdaki XML elemanlarını destekler. Desteklenmeyen tüm XML etiketleri JSON nesnesine dönüştürülür. Daha sonra, her etiket üst şekil `properties` veya katman alanında bir özellik olarak eklenir.

### <a name="kml-elements"></a>KML elemanları

Uzamsal IO modülü aşağıdaki KML elemanlarını destekler.

| Öğe Adı         | Okuma    | Yazma   | Notlar                                                                                                                      |
|----------------------|---------|---------|----------------------------------------------------------------------------------------------------------------------------|
| `address`            | partial | evet     | Nesne ayrıştırılır, ancak konumlandırma şekli için kullanılmaz.                                                                    |
| `AddressDetails`     | partial | hayır      | Nesne ayrıştırılır, ancak konumlandırma şekli için kullanılmaz.                                                                    |
| `atom:author`        | evet     | evet     |                                                                                                                            |
| `atom:link`          | evet     | evet     |                                                                                                                            |
| `atom:name`          | evet     | evet     |                                                                                                                            |
| `BalloonStyle`       | partial | partial | `displayMode`desteklenmez. `PopupTemplate`Dönüştürülür. Yazmak için, `popupTemplate` yazmak istediğiniz özelliğin özelliği olarak bir özellik ekleyin. |
| `begin`              | evet     | evet     |                                                                                                                            |
| `color`              | evet     | evet     | Içerir `#AABBGGRR` `#BBGGRR`ve . Bir CSS renk dizesi içine ayrıştı                                                           |
| `colorMode`          | evet     | hayır      |                                                                                                                            |
| `coordinates`        | evet     | evet     |                                                                                                                            |
| `Data`               | evet     | evet     |                                                                                                                            |
| `description`        | evet     | evet     |                                                                                                                            |
| `displayName`        | evet     | evet     |                                                                                                                            |
| `Document`           | evet     | evet     |                                                                                                                            |
| `drawOrder`          | partial | hayır      | Zemin bindirmeleri için okuyun ve bunları sıralamak için kullanılır. 
| `east`               | evet     | evet     |                                                                                                                            |
| `end`                | evet     | evet     |                                                                                                                            |
| `ExtendedData`       | evet     | evet     | Formun `Data` `$[dataName]`yazılmamış `SimpleData` `Schema`veya , ve varlık değiştirmelerini destekler.                      |
| `extrude`            | partial | partial | Sadece çokgenler için desteklenir. Farklı yüksekliklerde çokgenlere sahip MultiGeometri tek tek özelliklere bölünecektir. Satır stilleri desteklenmez. 0 yüksekliğe sahip çokgenler düz çokgen olarak işlenecektir. Okurken, dış halkadaki ilk koordinatın yüksekliği çokgenin yükseklik özelliği olarak eklenecektir. Daha sonra, ilk koordinatın yüksekliği haritaüzerinde çokgen işlemek için kullanılacaktır. |
| `fill`               | evet     | evet     |                                                                                                                            |
| `Folder`             | evet     | evet     |                                                                                                                            |
| `GroundOverlay`      | evet     | evet     | `color`desteklenmiyor                                                                                                   |
| `heading`            | partial | hayır      | Ayrışmış ama tarafından `SimpleDataLayer`işlenmez. Yalnızca veri şeklin özelliğinde depolanırsa yazar.                 |
| `hotSpot`            | evet     | partial | Yalnızca veri şeklin özelliğinde depolanırsa yazar. Birimler yalnızca "piksel" olarak çıkarılır.                         |
| `href`               | evet     | evet     |                                                                                                                            |
| `Icon`               | partial | partial | Ayrışmış ama tarafından `SimpleDataLayer`işlenmez. Şeklin simge özelliğini yalnızca URI verileri içeriyorsa yazar. Yalnızca `href` desteklenir. |
| `IconStyle`          | partial | partial | `icon`, `heading` `colorMode`, `hotspots` ve değerler ayrıştırılır, ancak bunlar`SimpleDataLayer`         |
| `innerBoundaryIs`    | evet     | evet     |                                                                                                                            |
| `kml`                | evet     | evet     |                                                                                                                            |
| `LabelStyle`         | hayır      | hayır      |                                                                                                                            |
| `LatLonBox`          | evet     | evet     |                                                                                                                            |
| `gx:LatLonQuad`      | evet     | evet     |                                                                                                                            |
| `LinearRing`         | evet     | evet     |                                                                                                                            |
| `LineString`         | evet     | evet     |                                                                                                                            |
| `LineStyle`          | evet     | evet     | `colorMode`desteklenmez.                                                                                         |
| `Link`               | evet     | hayır      | Yalnızca `href` özellik ağ bağlantıları için desteklenir.                                                                   |
| `MultiGeometry`      | partial | partial | Okunduğunda tek tek özelliklere bölünebilir.                                                                     |
| `name`               | evet     | evet     |                                                                                                                            |
| `NetworkLink`        | evet     | hayır      | Bağlantıların belgeyle aynı etki alanında olması gerekir.                                                                  |
| `NetworkLinkControl` | hayır      | hayır      |                                                                                                                            |
| `north`              | evet     | evet     |                                                                                                                            |
| `open`               | evet     | evet     |                                                                                                                            |
| `outerBoundaryIs`    | evet     | evet     |                                                                                                                            |
| `outline`            | evet     | evet     |                                                                                                                            |
| `overlayXY`          | hayır      | hayır      |                                                                                                                            |
| `Pair`               | partial | hayır      | Yalnızca `normal` a'daki `StyleMap` stil desteklenir. `highlight`desteklenmez.                                   |
| `phoneNumber`        | evet     | evet     |                                                                                                                            |
| `PhotoOverlay`       | hayır      | hayır      |                                                                                                                            |
| `Placemark`          | evet     | evet     |                                                                                                                            |
| `Point`              | evet     | evet     |                                                                                                                            |
| `Polygon`            | evet     | evet     |                                                                                                                            |
| `PolyStyle`          | evet     | evet     |                                                                                                                            |
| `Region`             | partial | partial | `LatLongBox`belge düzeyinde desteklenir.                                                                      |
| `rotation`           | hayır      | hayır      |                                                                                                                            |
| `rotationXY`         | hayır      | hayır      |                                                                                                                            |
| `scale`              | hayır      | hayır      |                                                                                                                            |
| `Schema`             | evet     | evet     |                                                                                                                            |
| `SchemaData`         | evet     | evet     |                                                                                                                            |
| `schemaUrl`          | partial | evet     | KMZ'ye dahil olmayan harici belgelerden yükleme stillerini desteklemez.                             |
| `ScreenOverlay`      | hayır      | hayır      |                                                                                                                            |
| `screenXY`           | hayır      | hayır      |                                                                                                                            |
| `SimpleData`         | evet     | evet     |                                                                                                                            |
| `SimpleField`        | evet     | evet     |                                                                                                                            |
| `size`               | hayır      | hayır      |                                                                                                                            |
| `Snippet`            | partial | partial | `maxLines`öznitelik yoksayılır.                                                                                  |
| `south`              | evet     | evet     |                                                                                                                            |
| `Style`              | evet     | evet     |                                                                                                                            |
| `StyleMap`           | partial | hayır      | Yalnızca normal `StyleMap` bir stil desteklenir.                                                                        |
| `styleUrl`           | partial | evet     | Dış stil URL'leri desteklenmez.                                                                         |
| `text`               | evet     | evet     | `$[geDirections]` Değiştirme desteklenmiyor                                                                          |
| `textColor`          | evet     | evet     |                                                                                                                            |
| `TimeSpan`           | evet     | evet     |                                                                                                                            |
| `TimeStamp`          | evet     | evet     |                                                                                                                            |
| `value`              | evet     | evet     |                                                                                                                            |
| `viewRefreshMode`    | partial | hayır      |  Bir WMS hizmetine işaret `onStop` ediyorsa, yalnızca yer kaplamaları için desteklenir. URL'ye `BBOX=[bboxWest],[bboxSouth],[bboxEast],[bboxNorth]` eklenecek ve harita hareket ettikçe güncellenecektir.  |
| `visibility`         | evet     | evet     |                                                                                                                            |
| `west`               | evet     | evet     |                                                                                                                            |
| `when`               | evet     | evet     |                                                                                                                            |
| `width`              | evet     | evet     |                                                                                                                            |

### <a name="georss-elements"></a>GeoRSS elemanları

Uzamsal IO modülü aşağıdaki GeoRSS elemanlarını destekler.

| Öğe Adı             | Okuma    | Yazma | Notlar                                                                                          |
|--------------------------|---------|-------|------------------------------------------------------------------------------------------------|
| `atom:author`            | evet     | evet   |                                                                                                |
| `atom:category`          | evet     | evet   |                                                                                                |
| `atom:content`           | evet     | evet   |                                                                                                |
| `atom:contributor`       | evet     | evet   |                                                                                                |
| `atom:email`             | evet     | evet   |                                                                                                |
| `atom:entry`             | evet     | evet   |                                                                                                |
| `atom:feed`              | evet     | evet   |                                                                                                |
| `atom:icon`              | evet     | evet   |                                                                                                |
| `atom:id`                | evet     | evet   |                                                                                                |
| `atom:link`              | evet     | evet   |                                                                                                |
| `atom:logo`              | evet     | evet   |                                                                                                |
| `atom:name`              | evet     | evet   |                                                                                                |
| `atom:published`         | evet     | evet   |                                                                                                |
| `atom:rights`            | evet     | evet   |                                                                                                |
| `atom:source`            | evet     | evet   |                                                                                                |
| `atom:subtitle`          | evet     | evet   |                                                                                                |
| `atom:summary`           | evet     | evet   |                                                                                                |
| `atom:title`             | evet     | evet   |                                                                                                |
| `atom:updated`           | evet     | evet   |                                                                                                |
| `atom:uri`               | evet     | evet   |                                                                                                |
| `geo:lat`                | evet     | hayır    | Olarak `georss:point`yazıldı.                                                                   |
| `geo:lon`                | evet     | hayır    | Olarak `georss:point`yazıldı.                                                                   |
| `geo:long`               | evet     | hayır    | Olarak `georss:point`yazıldı.                                                                   |
| `georss:box`             | evet     | hayır    | Çokgen olarak okuyun ve `subType` "Dikdörtgen" özelliği verildi                                |
| `georss:circle`          | evet     | evet   |                                                                                                |
| `georss:elev`            | evet     | evet   |                                                                                                |
| `georss:featurename`     | evet     | evet   |                                                                                                |
| `georss:featuretypetag`  | evet     | evet   |                                                                                                |
| `georss:floor`           | evet     | evet   |                                                                                                |
| `georss:line`            | evet     | evet   |                                                                                                |
| `georss:point`           | evet     | evet   |                                                                                                |
| `georss:polygon`         | evet     | evet   |                                                                                                |
| `georss:radius`          | evet     | evet   |                                                                                                |
| `georss:relationshiptag` | evet     | evet   |                                                                                                |
| `georss:where`           | evet     | evet   |                                                                                                |
| `geourl:latitude`        | evet     | hayır    | Olarak `georss:point`yazıldı.                                                                   |
| `geourl:longitude`       | evet     | hayır    | Olarak `georss:point`yazıldı.                                                                   |
| `position`               | evet     | hayır    | Bazı XML akışları GML'yi `georss:where` etiketle sarmak yerine konum etiketiyle sarar. Bu etiketi okuyacak, ancak `georss:where` bir etiket kullanarak yazacaktır. |
| `rss`                    | evet     | hayır    | ATOM formatında yazılmış GeoRSS.                                                                 |
| `rss:author`             | evet     | partial | Olarak `atom:author`yazıldı.                                                                 |
| `rss:category`           | evet     | partial | Olarak `atom:category`yazıldı.                                                               |
| `rss:channel`            | evet     | hayır    |                                                                                                |
| `rss:cloud`              | evet     | hayır    |                                                                                                |
| `rss:comments`           | evet     | hayır    |                                                                                                |
| `rss:copyright`          | evet     | partial | `atom:rights` Eğer şekli zaten bir `rights` `properties` özelliği yok olarak yazılır.       |
| `rss:description`        | evet     | partial | `atom:content` Eğer şekli zaten bir `content` `properties` özelliği yok olarak yazılır.      |
| `rss:docs`               | evet     | hayır    |                                                                                                |
| `rss:enclosure`          | evet     | hayır    |                                                                                                |
| `rss:generator`          | evet     | hayır    |                                                                                                |
| `rss:guid`               | evet     | partial | `atom:id` Eğer şekli zaten bir `id` `properties` özelliği yok olarak yazılır.         |
| `rss:image`              | evet     | partial | `atom:logo` Eğer şekli zaten bir `logo` `properties` özelliği yok olarak yazılır.      |
| `rss:item`               | evet     | partial | Olarak `atom:entry`yazıldı.                                                                  |
| `rss:language`           | evet     | hayır    |                                                                                                |
| `rss:lastBuildDate`      | evet     | partial | `atom:updated` Eğer şekli zaten bir `updated` `properties` özelliği yok olarak yazılır.     |
| `rss:link`               | evet     | partial | Olarak `atom:link`yazıldı.                                                                   |
| `rss:managingEditor`     | evet     | partial | Olarak `atom:contributor`yazıldı.                                                            |
| `rss:pubDate`            | evet     | partial | `atom:published` Eğer şekli zaten bir `published` `properties` özelliği yok olarak yazılır.  |
| `rss:rating`             | evet     | hayır    |                                                                                                |
| `rss:skipDays`           | evet     | hayır    |                                                                                                |
| `rss:skipHours`          | evet     | hayır    |                                                                                                |
| `rss:source`             | evet     | partial | Içeren bir `atom:source` `atom:link`.                                       |
| `rss:textInput`          | evet     | hayır    |                                                                                                |
| `rss:title`              | evet     | partial | Olarak `atom:title`yazıldı.                                                                  |
| `rss:ttl`                | evet     | hayır    |                                                                                                |
| `rss:webMaster`          | evet     | hayır    |                                                                                                |

### <a name="gml-elements"></a>GML elemanları

Uzamsal IO modülü aşağıdaki GML elemanlarını destekler. 

| Öğe Adı            | Okuma | Yazma | Notlar                                                                                  |
|-------------------------|------|-------|----------------------------------------------------------------------------------------|
| `gml:coordinates`       | evet  | hayır    | Olarak `gml:posList`yazılır.                                                              |
| `gml:curveMember`       | evet  | hayır    |                                                                                        |
| `gml:curveMembers`      | evet  | hayır    |                                                                                        |
| `gml:Box`               | evet  | hayır    | Olarak `gml:Envelope`yazılır.                                                             |
| `gml:description`       | evet  | evet   |                                                                                        |
| `gml:Envelope`          | evet  | evet   |                                                                                        |
| `gml:exterior`          | evet  | evet   |                                                                                        |
| `gml:Feature`           | evet  | hayır    | Şekil olarak yazılmış.                                                                    |
| `gml:FeatureCollection` | evet  | hayır    | Geometri koleksiyonu olarak yazılmıştır.                                                      |
| `gml:featureMember`     | evet  | hayır    | Geometri koleksiyonu olarak yazılmıştır.                                                      |
| `gml:geometry`          | evet  | hayır    | Şekil olarak yazılmış.                                                                    |
| `gml:geometryMember`    | evet  | evet   |                                                                                        |
| `gml:geometryMembers`   | evet  | evet   |                                                                                        |
| `gml:identifier`        | evet  | evet   |                                                                                        |
| `gml:innerBoundaryIs`   | evet  | hayır    | Kullanılarak `gml.interior`yazıldı.                                                          |
| `gml:interior`          | evet  | evet   |                                                                                        |
| `gml:LinearRing`        | evet  | evet   |                                                                                        |
| `gml:LineString`        | evet  | evet   |                                                                                        |
| `gml:lineStringMember`  | evet  | evet   |                                                                                        |
| `gml:lineStringMembers` | evet  | hayır    |                                                                                        |
| `gml:MultiCurve`        | evet  | hayır    | Sadece `gml:LineString` üyeleri okur. Olarak yazıldı`gml.MultiLineString`                  |
| `gml:MultiGeometry`     | partial  | partial   | Yalnızca FeatureCollection olarak okuyun.                                              |
| `gml:MultiLineString`   | evet  | evet   |                                                                                        |
| `gml:MultiPoint`        | evet  | evet   |                                                                                        |
| `gml:MultiPolygon`      | evet  | evet   |                                                                                        |
| `gml:MultiSurface`      | evet  | hayır    | Sadece `gml:Polygon` üyeleri okur. Olarak yazıldı`gml.MultiPolygon`                        |
| `gml:name`              | evet  | evet   |                                                                                        |
| `gml:outerBoundaryIs`   | evet  | hayır    | Kullanılarak `gml.exterior`yazıldı.                                                          |
| `gml:Point`             | evet  | evet   |                                                                                        |
| `gml:pointMember`       | evet  | evet   |                                                                                        |
| `gml:pointMembers`      | evet  | hayır    |                                                                                        |
| `gml:Polygon`           | evet  | evet   |                                                                                        |
| `gml:polygonMember`     | evet  | evet   |                                                                                        |
| `gml:polygonMembers`    | evet  | hayır    |                                                                                        |
| `gml:pos`               | evet  | evet   |                                                                                        |
| `gml:posList`           | evet  | evet   |                                                                                        |
| `gml:surfaceMember`     | evet  | evet   |                                                                                        |

#### <a name="additional-notes"></a>ek notlar

- Üye elemanlar, alt öğelerin içine gömülebilecek bir geometri için aranacaktır. Bu arama işlemi, GML'den yayılan birçok XML biçiminin bir üye öğenin doğrudan alt öğesi olarak geometri yerleştirmeyebileceğinden gereklidir.
- `srsName`kısmen WGS84 koordinatları ve aşağıdaki kodlar için desteklenir:[EPSG:4326](https://epsg.io/4326)), ve web Mercator ([EPSG:3857](https://epsg.io/3857) veya alternatif kodlarından biri. Diğer koordinat sistemi wgs84 olarak ayrıştırılır.
- XML akışı okurken belirtilmediği sürece, eksen sırası XML akışındaki ipuçlarına göre belirlenir. "Enlem, boylam" ekseni sırası için bir tercih verilir.
- GML dosyasına yazarken özellikler için özel bir GML ad alanı belirtilmediği sürece, ek özellik bilgileri eklenmez.

### <a name="gpx-elements"></a>GPX elemanları

Uzamsal IO modülü aşağıdaki GPX elemanlarını destekler.

| Öğe Adı             | Okuma    | Yazma   | Notlar                                                                                       |
|--------------------------|---------|---------|---------------------------------------------------------------------------------------------|
| `gpx:ageofdgpsdata`      | evet     | evet     |                                                                                             |
| `gpx:author`             | evet     | evet     |                                                                                             |
| `gpx:bounds`             | evet     | evet     | Okunduğunda LocationRect'e dönüştürülür.                                                    |
| `gpx:cmt`                | evet     | evet     |                                                                                             |
| `gpx:copyright`          | evet     | evet     |                                                                                             |
| `gpx:desc`               | evet     | evet     | Diğer XML biçimleriyle hizalamak için okunduğunda açıklama özelliğine kopyalanır.               |
| `gpx:dgpsid`             | evet     | evet     |                                                                                             |
| `gpx:ele`                | evet     | evet     |                                                                                             |
| `gpx:extensions`         | partial | partial | Okunduğunda, stil bilgileri ayıklanır. Diğer tüm uzantılar basit bir JSON nesnesine düzleştirilmiş olacaktır. Yalnızca şekil stili bilgileri yazılır. |
| `gpx:geoidheight`        | evet     | evet     |                                                                                             |
| `gpx:gpx`                | evet     | evet     |                                                                                             |
| `gpx:hdop`               | evet     | evet     |                                                                                             |
| `gpx:link`               | evet     | evet     |                                                                                             |
| `gpx:magvar`             | evet     | evet     |                                                                                             |
| `gpx:metadata`           | evet     | evet     |                                                                                             |
| `gpx:name`               | evet     | evet     |                                                                                             |
| `gpx:pdop`               | evet     | evet     |                                                                                             |
| `gpx:rte`                | evet     | evet     |                                                                                             |
| `gpx:rtept`              | evet     | evet     |                                                                                             |
| `gpx:sat`                | evet     | evet     |                                                                                             |
| `gpx:src`                | evet     | evet     |                                                                                             |
| `gpx:sym`                | evet     | evet     | Değer yakalanır, ancak toka simgesini değiştirmek için kullanılmaz.                               |
| `gpx:text`               | evet     | evet     |                                                                                             |
| `gpx:time`               | evet     | evet     |                                                                                             |
| `gpx:trk`                | evet     | evet     |                                                                                             |
| `gpx:trkpt`              | evet     | evet     |                                                                                             |
| `gpx:trkseg`             | evet     | evet     |                                                                                             |
| `gpx:type`               | evet     | evet     |                                                                                             |
| `gpx:vdop`               | evet     | evet     |                                                                                             |
| `gpx:wpt`                | evet     | evet     |                                                                                             |
| `gpx_style:color`        | evet     | evet     |                                                                                             |
| `gpx_style:line`         | partial | partial | `color`, `opacity` `width`, `lineCap` , desteklenir.                                           |
| `gpx_style:opacity`      | evet     | evet     |                                                                                             |
| `gpx_style:width`        | evet     | evet     |                                                                                             |
| `gpxx:DisplayColor`      | evet     | hayır      | Şeklin rengini belirtmek için kullanılır. Yazarken, `gpx_style:line` bunun yerine renk kullanılacaktır.  |
| `gpxx:RouteExtension`    | partial | hayır      | Tüm özellikler `properties`. Yalnızca `DisplayColor` kullanılır.                     |
| `gpxx:TrackExtension`    | partial | hayır      | Tüm özellikler `properties`. Yalnızca `DisplayColor` kullanılır.                     |
| `gpxx:WaypointExtension` | partial | hayır      | Tüm özellikler `properties`. Yalnızca `DisplayColor` kullanılır.                     |
| `gpx:keywords`           | evet     | evet     |                                                                                             |
| `gpx:fix`                | evet     | evet     |                                                                                             |

#### <a name="additional-notes"></a>ek notlar

Yazarken;

- MultiPoints tek tek ara noktalara bölünecektir.
- Çokgenler ve MultiPolygons parça olarak yazılacaktır. 
  
## <a name="supported-well-known-text-geometry-types"></a>Desteklenen İyi Bilinen Metin geometri türleri

| Geometri türü | Okuma | Yazma |
|--------------|:----:|:-----:|
| Nokta | x | x |
| NOKTA Z | x | x | 
| NOKTA M | x | x<sup>[2]</sup> |
| NOKTA ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| Linestring | x | x |
| LINESTRING Z | x | x | 
| LINESTRING M | x | x<sup>[2]</sup> |
| LINESTRING ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| Çokgen | x | x |
| ÇOKGEN Z | x | x |
| ÇOKGEN M | x | x<sup>[2]</sup> |
| ÇOKGEN ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| Çoklu | x | x |
| ÇOK NOKTALI Z | x | x | 
| ÇOK NOKTALI M | x | x<sup>[2]</sup> |
| POMULTIPOINTINT ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| Multilinestring | x | x |
| MULTILINESTRING Z | x | x | 
| MULTILINESTRING M | x | x<sup>[2]</sup> |
| MULTILINESTRING ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| Multipolygon | x | x |
| MULTIPOLYGON Z | x | x | 
| MULTIPOLYGON M | x | x<sup>[2]</sup> |
| MULTIPOLYGON ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| Geometrycollection | x | x |
| GEOMETRİ TOPLAMA Z | x | x | 
| GEOMETRİ TOPLAMA M | x | x<sup>[2]</sup> | 
| GEOMETRİ TOPLAMA ZM | x<sup>[1]</sup><sup>[2]</sup> | x | 

\[1\] Konum değerine yalnızca Z parametresi yakalanır ve üçüncü bir değer olarak eklenir.

\[2\] M parametre yakalanmaz.

## <a name="delimited-spatial-data-support"></a>Sınırlı uzamsal veri desteği

Virgülle ayrılmış değer dosyaları (CSV) gibi sınırlı uzamsal veriler genellikle uzamsal veri içeren sütunlara sahiptir. Örneğin, enlem ve boylam bilgileri içeren sütunlar olabilir. Bilinen Metin biçiminde, uzamsal geometri verileri içeren bir sütun olabilir.

### <a name="spatial-data-column-detection"></a>Uzamsal veri sütunu algılama

Uzamsal veriler içeren sınırlı bir dosya yıkılırken, üstbilgi hangi sütunların konum alanları içerdiğini belirlemek için çözümlenir. Üstbilgi tür bilgileri içeriyorsa, hücre değerlerini uygun türe dökmek için kullanılır. Üstbilgi belirtilmemişse, ilk satır çözümlenir ve üstbilgi oluşturmak için kullanılır. İlk satırı çözümlürken, sütun adlarını aşağıdaki adlarla duyarsız bir şekilde eşleştirmek için bir denetim yürütülür. Bir dosyada iki veya daha fazla ad varsa, adların sırası önceliktir.

#### <a name="latitude"></a>Enlem

- `latitude`
- `lat`
- `latdd`
- `lat_dd`
- `latitude83`
- `latdecdeg`
- `y`
- `ycenter`
- `point-y`

#### <a name="longitude"></a>Boylam

- `longitude`
- `lon`
- `lng`
- `long`
- `longdd`
- `long_dd`
- `longitude83`
- `longdecdeg`
- `x`
- `xcenter`
- `point-x`

#### <a name="elevation"></a>Yükseklik

- `elevation`
- `elv`
- `altitude`
- `alt`
- `z`

#### <a name="geography"></a>Coğrafya

İlk veri satırı, Bilinen Metin biçiminde olan dizeleri için taranır. 

### <a name="delimited-data-column-types"></a>Sınırlı veri sütunu türleri

Üstbilgi satırı tararken, sütun adındaki tüm tür bilgileri ayıklanır ve bu sütundaki hücreleri dökmek için kullanılır. Burada tür değeri olan bir sütun adı örneği verilmiştir: "ColumnName (typeName)". Aşağıdaki büyük/küçük harf duyarsız tür adları desteklenir:

#### <a name="numbers"></a>Numaralar

- edm.int64
- int
- long
- edm.double
- float
- double
- number

#### <a name="booleans"></a>Booleler

- edm.boolean
- bool
- boole

#### <a name="dates"></a>Dates

- edm.datetime
- date
- datetime

#### <a name="geography"></a>Coğrafya

- edm.coğrafya
- Coğrafya

#### <a name="strings"></a>Dizeler

- edm.string
- varchar
- metin
- string

Üstbilgiden tür bilgisi çıkarılamıyorsa ve okurken dinamik yazma seçeneği etkinse, hangi veri türünün en uygun olduğunu belirlemek için her hücre ayrı ayrı analiz edilir.

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Uzamsal verileri okuma ve yazma](spatial-io-read-write-spatial-data.md)
