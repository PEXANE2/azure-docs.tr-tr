---
title: Desteklenen veri biçimi ayrıntıları | Microsoft Azure haritaları
description: Sınırlı uzamsal verilerin uzamsal GÇ modülünde nasıl ayrıştırılacağınızı öğrenin.
author: farah-alyasari
ms.author: v-faalya
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: fff801731c3c3a94b4039a8c65ad8ccaab7cc725
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78402733"
---
# <a name="supported-data-format-details"></a>Desteklenen veri biçimi ayrıntıları

Bu makalede, tüm XML etiketleri ve Iyi bilinen metin geometrisi türleri için okuma ve yazma desteği hakkında bilgi sağlanır. Ayrıca, sınırlı uzamsal verilerin uzamsal GÇ modülünde ayrıştırılma şeklini de ayrıntılarıyla görebilirsiniz.

## <a name="supported-xml-namespaces"></a>Desteklenen XML ad alanları

Uzamsal GÇ modülü aşağıdaki ad alanlarından XML etiketlerini destekler.

| Ad alanı öneki | Ad alanı URI 'SI   | Notlar                                                                    |
|:------------------|:-----------------|:----------------------------------------|
| `atom`           | `http://www.w3.org/2005/Atom`   |                                         |
| `geo`            | `http://www.w3.org/2003/01/geo/wgs84_pos#`  | GeoRSS dosyalarında salt okuma desteği.           |
| `georss`         | `http://www.georss.org/georss`  |                                                |
| `geourl`         | `http://geourl.org/rss/module/` | GeoRSS dosyalarında salt okuma desteği.                       |
| `gml`            | `http://www.opengis.net/gml`    |                                                        |
| `gpx`            | `http://www.topografix.com/GPX/1/1` |                                                   |
| `gpxx`           | `http://www.garmin.com/xmlschemas/GpxExtensions/v3` | GPX dosyalarında salt okuma desteği. DisplayColor öğesini ayrıştırır ve kullanır. Şekil meta verilerine eklenen diğer tüm özellikler. |
| `gpx_style`      | `http://www.topografix.com/GPX/gpx_style/0/2`      | GPX dosyalarında desteklenir. Çizgi rengini kullanır. |
| `gx`             | `http://www.google.com/kml/ext/2.2` |                                                      |
| `kml`            | `http://www.opengis.net/kml/2.2`    |                                                      |
| `rss`            |                                 | Salt okunurdur. GeoRSS, atom biçimini kullanarak yazar.              |

## <a name="supported-xml-elements"></a>Desteklenen XML öğeleri

Uzamsal GÇ modülü aşağıdaki XML öğelerini destekler. Desteklenmeyen herhangi bir XML etiketi, JSON nesnesine dönüştürülür. Ardından, her etiket üst şeklin veya katmanın `properties` alanına bir özellik olarak eklenecektir.

### <a name="kml-elements"></a>KML öğeleri

Uzamsal GÇ modülü aşağıdaki KML öğelerini destekler.

| Öğe adı         | Okuma    | Yazma   | Notlar                                                                                                                      |
|----------------------|---------|---------|----------------------------------------------------------------------------------------------------------------------------|
| `address`            | partial | evet     | Nesne ayrıştırıldı, ancak şekil konumlandırma için kullanılmaz.                                                                    |
| `AddressDetails`     | partial | hayır      | Nesne ayrıştırıldı, ancak şekil konumlandırma için kullanılmaz.                                                                    |
| `atom:author`        | evet     | evet     |                                                                                                                            |
| `atom:link`          | evet     | evet     |                                                                                                                            |
| `atom:name`          | evet     | evet     |                                                                                                                            |
| `BalloonStyle`       | partial | partial | `displayMode` desteklenmez. `PopupTemplate`dönüştürülür. Yazmak için bir `popupTemplate` özelliğini yazmak istediğiniz özelliğin özelliği olarak ekleyin. |
| `begin`              | evet     | evet     |                                                                                                                            |
| `color`              | evet     | evet     | `#AABBGGRR` ve `#BBGGRR`içerir. Bir CSS renk dizesine ayrıştırıldı                                                           |
| `colorMode`          | evet     | hayır      |                                                                                                                            |
| `coordinates`        | evet     | evet     |                                                                                                                            |
| `Data`               | evet     | evet     |                                                                                                                            |
| `description`        | evet     | evet     |                                                                                                                            |
| `displayName`        | evet     | evet     |                                                                                                                            |
| `Document`           | evet     | evet     |                                                                                                                            |
| `drawOrder`          | partial | hayır      | Arka üste yazılanları okuyun ve bunları sıralamak için kullanılır. 
| `east`               | evet     | evet     |                                                                                                                            |
| `end`                | evet     | evet     |                                                                                                                            |
| `ExtendedData`       | evet     | evet     | Türsüz `Data`, `SimpleData` veya `Schema`ve form `$[dataName]`varlık değiştirmeler destekler.                      |
| `extrude`            | partial | partial | Yalnızca çokgenler için desteklenir. Farklı yüksekliklerdeki çokgenler olan MultiGeometry, bireysel özelliklere göre parçalanacaktır. Çizgi stilleri desteklenmez. Yüksekliği 0 olan çokgenler düz bir çokgen olarak oluşturulur. Okurken, Dış halkadaki ilk koordinat yüksekliği, çokgenin Height özelliği olarak eklenir. Sonra, haritada çokgeni işlemek için ilk koordinat yüksekliği kullanılacaktır. |
| `fill`               | evet     | evet     |                                                                                                                            |
| `Folder`             | evet     | evet     |                                                                                                                            |
| `GroundOverlay`      | evet     | evet     | `color` desteklenmez                                                                                                   |
| `heading`            | partial | hayır      | Ayrıştırılmış ancak `SimpleDataLayer`tarafından işlenmiyor. Yalnızca verilerin şeklin özelliğinde depolanıyorsa yazar.                 |
| `hotSpot`            | evet     | partial | Yalnızca verilerin şeklin özelliğinde depolanıyorsa yazar. Birimler yalnızca "piksel" olarak silinir.                         |
| `href`               | evet     | evet     |                                                                                                                            |
| `Icon`               | partial | partial | Ayrıştırılmış ancak `SimpleDataLayer`tarafından işlenmiyor. Yalnızca bir URI verisi içeriyorsa şeklin Icon özelliğini yazar. Yalnızca `href` desteklenir. |
| `IconStyle`          | partial | partial | `icon`, `heading`, `colorMode`ve `hotspots` değerleri ayrıştırılır, ancak bunlar tarafından işlenmemektedir `SimpleDataLayer`         |
| `innerBoundaryIs`    | evet     | evet     |                                                                                                                            |
| `kml`                | evet     | evet     |                                                                                                                            |
| `LabelStyle`         | hayır      | hayır      |                                                                                                                            |
| `LatLonBox`          | evet     | evet     |                                                                                                                            |
| `gx:LatLonQuad`      | evet     | evet     |                                                                                                                            |
| `LinearRing`         | evet     | evet     |                                                                                                                            |
| `LineString`         | evet     | evet     |                                                                                                                            |
| `LineStyle`          | evet     | evet     | `colorMode` desteklenmez.                                                                                         |
| `Link`               | evet     | hayır      | Yalnızca `href` özelliği ağ bağlantıları için desteklenir.                                                                   |
| `MultiGeometry`      | partial | partial | , Okuma sırasında ayrı ayrı özelliklerle ayrılabilir.                                                                     |
| `name`               | evet     | evet     |                                                                                                                            |
| `NetworkLink`        | evet     | hayır      | Bağlantıların, belgeyle aynı etki alanında olması gerekir.                                                                  |
| `NetworkLinkControl` | hayır      | hayır      |                                                                                                                            |
| `north`              | evet     | evet     |                                                                                                                            |
| `open`               | evet     | evet     |                                                                                                                            |
| `outerBoundaryIs`    | evet     | evet     |                                                                                                                            |
| `outline`            | evet     | evet     |                                                                                                                            |
| `overlayXY`          | hayır      | hayır      |                                                                                                                            |
| `Pair`               | partial | hayır      | Yalnızca bir `StyleMap` `normal` stili desteklenir. `highlight` desteklenmez.                                   |
| `phoneNumber`        | evet     | evet     |                                                                                                                            |
| `PhotoOverlay`       | hayır      | hayır      |                                                                                                                            |
| `Placemark`          | evet     | evet     |                                                                                                                            |
| `Point`              | evet     | evet     |                                                                                                                            |
| `Polygon`            | evet     | evet     |                                                                                                                            |
| `PolyStyle`          | evet     | evet     |                                                                                                                            |
| `Region`             | partial | partial | `LatLongBox` belge düzeyinde desteklenir.                                                                      |
| `rotation`           | hayır      | hayır      |                                                                                                                            |
| `rotationXY`         | hayır      | hayır      |                                                                                                                            |
| `scale`              | hayır      | hayır      |                                                                                                                            |
| `Schema`             | evet     | evet     |                                                                                                                            |
| `SchemaData`         | evet     | evet     |                                                                                                                            |
| `schemaUrl`          | partial | evet     | , Bir KMZ 'ye dahil olmayan dış belgelerden stil yüklemeyi desteklemez.                             |
| `ScreenOverlay`      | hayır      | hayır      |                                                                                                                            |
| `screenXY`           | hayır      | hayır      |                                                                                                                            |
| `SimpleData`         | evet     | evet     |                                                                                                                            |
| `SimpleField`        | evet     | evet     |                                                                                                                            |
| `size`               | hayır      | hayır      |                                                                                                                            |
| `Snippet`            | partial | partial | `maxLines` öznitelik yoksayıldı.                                                                                  |
| `south`              | evet     | evet     |                                                                                                                            |
| `Style`              | evet     | evet     |                                                                                                                            |
| `StyleMap`           | partial | hayır      | Yalnızca `StyleMap` Normal stili destekleniyor.                                                                        |
| `styleUrl`           | partial | evet     | Dış stil URL 'Leri desteklenmez.                                                                         |
| `text`               | evet     | evet     | `$[geDirections]` değiştirme desteklenmiyor                                                                          |
| `textColor`          | evet     | evet     |                                                                                                                            |
| `TimeSpan`           | evet     | evet     |                                                                                                                            |
| `TimeStamp`          | evet     | evet     |                                                                                                                            |
| `value`              | evet     | evet     |                                                                                                                            |
| `viewRefreshMode`    | partial | hayır      |  Bir WMS hizmetine işaret ederseniz, yalnızca `onStop` taban üste bindirme için desteklenir. Eşleme taşırken URL 'ye `BBOX=[bboxWest],[bboxSouth],[bboxEast],[bboxNorth]` ekler ve güncelleştirme olur.  |
| `visibility`         | evet     | evet     |                                                                                                                            |
| `west`               | evet     | evet     |                                                                                                                            |
| `when`               | evet     | evet     |                                                                                                                            |
| `width`              | evet     | evet     |                                                                                                                            |

### <a name="georss-elements"></a>GeoRSS öğeleri

Uzamsal GÇ modülü aşağıdaki GeoRSS öğelerini destekler.

| Öğe adı             | Okuma    | Yazma | Notlar                                                                                          |
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
| `geo:lat`                | evet     | hayır    | `georss:point`olarak yazılmıştır.                                                                   |
| `geo:lon`                | evet     | hayır    | `georss:point`olarak yazılmıştır.                                                                   |
| `geo:long`               | evet     | hayır    | `georss:point`olarak yazılmıştır.                                                                   |
| `georss:box`             | evet     | hayır    | Çokgen olarak oku ve "Rectangle" öğesinin `subType` özelliği verildi                                |
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
| `geourl:latitude`        | evet     | hayır    | `georss:point`olarak yazılmıştır.                                                                   |
| `geourl:longitude`       | evet     | hayır    | `georss:point`olarak yazılmıştır.                                                                   |
| `position`               | evet     | hayır    | Bazı XML akışları GML 'yi bir GeoRSS: WHERE etiketiyle sarmalamak yerine bir konum etiketi ile sarmalacaktır. Bu etiketi okuyacak, ancak bir GeoRSS: WHERE etiketi kullanılarak yazılacak. |
| `rss`                    | evet     | hayır    | GeoRSS, ATOM biçiminde yazılmıştır.                                                                 |
| `rss:author`             | evet     | partial | `atom:author`olarak yazılmıştır.                                                                 |
| `rss:category`           | evet     | partial | `atom:category`olarak yazılmıştır.                                                               |
| `rss:channel`            | evet     | hayır    |                                                                                                |
| `rss:cloud`              | evet     | hayır    |                                                                                                |
| `rss:comments`           | evet     | hayır    |                                                                                                |
| `rss:copyright`          | evet     | partial | Şekil zaten bir `rights` `properties` özelliği içermiyorsa `atom:rights` olarak yazılır.       |
| `rss:description`        | evet     | partial | Şekil zaten bir `content` `properties` özelliği içermiyorsa `atom:content` olarak yazılır.      |
| `rss:docs`               | evet     | hayır    |                                                                                                |
| `rss:enclosure`          | evet     | hayır    |                                                                                                |
| `rss:generator`          | evet     | hayır    |                                                                                                |
| `rss:guid`               | evet     | partial | Şekil zaten bir `id` `properties` özelliği içermiyorsa `atom:id` olarak yazılır.         |
| `rss:image`              | evet     | partial | Şekil zaten bir `logo` `properties` özelliği içermiyorsa `atom:logo` olarak yazılır.      |
| `rss:item`               | evet     | partial | `atom:entry`olarak yazılmıştır.                                                                  |
| `rss:language`           | evet     | hayır    |                                                                                                |
| `rss:lastBuildDate`      | evet     | partial | Şekil zaten bir `updated` `properties` özelliği içermiyorsa `atom:updated` olarak yazılır.     |
| `rss:link`               | evet     | partial | `atom:link`olarak yazılmıştır.                                                                   |
| `rss:managingEditor`     | evet     | partial | `atom:contributor`olarak yazılmıştır.                                                            |
| `rss:pubDate`            | evet     | partial | Şekil zaten bir `published` `properties` özelliği içermiyorsa `atom:published` olarak yazılır.  |
| `rss:rating`             | evet     | hayır    |                                                                                                |
| `rss:skipDays`           | evet     | hayır    |                                                                                                |
| `rss:skipHours`          | evet     | hayır    |                                                                                                |
| `rss:source`             | evet     | partial | Bir `atom:link`içeren `atom:source` olarak yazılmıştır.                                       |
| `rss:textInput`          | evet     | hayır    |                                                                                                |
| `rss:title`              | evet     | partial | `atom:title`olarak yazılmıştır.                                                                  |
| `rss:ttl`                | evet     | hayır    |                                                                                                |
| `rss:webMaster`          | evet     | hayır    |                                                                                                |

### <a name="gml-elements"></a>GML öğeleri

Uzamsal GÇ modülü aşağıdaki GML öğelerini destekler. 

| Öğe adı            | Okuma | Yazma | Notlar                                                                                  |
|-------------------------|------|-------|----------------------------------------------------------------------------------------|
| `gml:coordinates`       | evet  | hayır    | `gml:posList`olarak yazılmıştır.                                                              |
| `gml:curveMember`       | evet  | hayır    |                                                                                        |
| `gml:curveMembers`      | evet  | hayır    |                                                                                        |
| `gml:Box`               | evet  | hayır    | `gml:Envelope`olarak yazılmıştır.                                                             |
| `gml:description`       | evet  | evet   |                                                                                        |
| `gml:Envelope`          | evet  | evet   |                                                                                        |
| `gml:exterior`          | evet  | evet   |                                                                                        |
| `gml:Feature`           | evet  | hayır    | Şekil olarak yazılmıştır.                                                                    |
| `gml:FeatureCollection` | evet  | hayır    | Geometri koleksiyonu olarak yazılmıştır.                                                      |
| `gml:featureMember`     | evet  | hayır    | Geometri koleksiyonu olarak yazılmıştır.                                                      |
| `gml:geometry`          | evet  | hayır    | Şekil olarak yazılmıştır.                                                                    |
| `gml:geometryMember`    | evet  | evet   |                                                                                        |
| `gml:geometryMembers`   | evet  | evet   |                                                                                        |
| `gml:identifier`        | evet  | evet   |                                                                                        |
| `gml:innerBoundaryIs`   | evet  | hayır    | `gml.interior`kullanılarak yazılmıştır.                                                          |
| `gml:interior`          | evet  | evet   |                                                                                        |
| `gml:LinearRing`        | evet  | evet   |                                                                                        |
| `gml:LineString`        | evet  | evet   |                                                                                        |
| `gml:lineStringMember`  | evet  | evet   |                                                                                        |
| `gml:lineStringMembers` | evet  | hayır    |                                                                                        |
| `gml:MultiCurve`        | evet  | hayır    | Yalnızca `gml:LineString` üyelerini okur. `gml.MultiLineString` olarak yazılmıştır                  |
| `gml:MultiGeometry`     | partial  | partial   | Yalnızca Korturecollection olarak oku.                                              |
| `gml:MultiLineString`   | evet  | evet   |                                                                                        |
| `gml:MultiPoint`        | evet  | evet   |                                                                                        |
| `gml:MultiPolygon`      | evet  | evet   |                                                                                        |
| `gml:MultiSurface`      | evet  | hayır    | Yalnızca `gml:Polygon` üyelerini okur. `gml.MultiPolygon` olarak yazılmıştır                        |
| `gml:name`              | evet  | evet   |                                                                                        |
| `gml:outerBoundaryIs`   | evet  | hayır    | `gml.exterior`kullanılarak yazılmıştır.                                                          |
| `gml:Point`             | evet  | evet   |                                                                                        |
| `gml:pointMember`       | evet  | evet   |                                                                                        |
| `gml:pointMembers`      | evet  | hayır    |                                                                                        |
| `gml:Polygon`           | evet  | evet   |                                                                                        |
| `gml:polygonMember`     | evet  | evet   |                                                                                        |
| `gml:polygonMembers`    | evet  | hayır    |                                                                                        |
| `gml:pos`               | evet  | evet   |                                                                                        |
| `gml:posList`           | evet  | evet   |                                                                                        |
| `gml:surfaceMember`     | evet  | evet   |                                                                                        |

#### <a name="additional-notes"></a>Ek notlar

- Üye öğeleri, alt öğeler içinde gömülmüş olabilecek bir geometri için aranır. Bu arama işlemi, GML 'den genişleyen çok sayıda XML biçimi bir geometri öğesinin doğrudan alt öğesi olarak bir geometrisi yerleştirmeyebilir.
- `srsName`, WGS84 koordinatları ve şu kodlar için kısmen desteklenir:[EPSG: 4326](https://epsg.io/4326)) ve Web Mercator ([EPSG: 3857](https://epsg.io/3857) veya alternatif kodlarından biri. Diğer koordinat sistemleri, WGS84 olduğu gibi ayrıştırılacaktır.
- Bir XML akışı okunurken belirtilmedikçe, eksen sırası XML akışındaki ipuçlarına göre belirlenir. "Enlem, Boylam" eksen sırası için bir tercih verilir.
- Bir GML dosyasına yazılırken özellikler için özel bir GML ad alanı belirtilmediği takdirde, ek özellik bilgileri eklenmeyecektir.

### <a name="gpx-elements"></a>GPX öğeleri

Uzamsal GÇ modülü aşağıdaki GPX öğelerini destekler.

| Öğe adı             | Okuma    | Yazma   | Notlar                                                                                       |
|--------------------------|---------|---------|---------------------------------------------------------------------------------------------|
| `gpx:ageofdgpsdata`      | evet     | evet     |                                                                                             |
| `gpx:author`             | evet     | evet     |                                                                                             |
| `gpx:bounds`             | evet     | evet     | Okuma sırasında LocationRect 'e dönüştürüldü.                                                    |
| `gpx:cmt`                | evet     | evet     |                                                                                             |
| `gpx:copyright`          | evet     | evet     |                                                                                             |
| `gpx:desc`               | evet     | evet     | Diğer XML biçimleriyle hizalamak için okundu durumunda açıklama özelliğine kopyalanmış.               |
| `gpx:dgpsid`             | evet     | evet     |                                                                                             |
| `gpx:ele`                | evet     | evet     |                                                                                             |
| `gpx:extensions`         | partial | partial | Okuma sırasında stil bilgileri ayıklanır. Tüm diğer uzantılar basit bir JSON nesnesi olarak düzleştirilir. Yalnızca şekil stili bilgileri yazılır. |
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
| `gpx:sym`                | evet     | evet     | Değer yakalandı, ancak raptiye simgesini değiştirmek için kullanılmaz.                               |
| `gpx:text`               | evet     | evet     |                                                                                             |
| `gpx:time`               | evet     | evet     |                                                                                             |
| `gpx:trk`                | evet     | evet     |                                                                                             |
| `gpx:trkpt`              | evet     | evet     |                                                                                             |
| `gpx:trkseg`             | evet     | evet     |                                                                                             |
| `gpx:type`               | evet     | evet     |                                                                                             |
| `gpx:vdop`               | evet     | evet     |                                                                                             |
| `gpx:wpt`                | evet     | evet     |                                                                                             |
| `gpx_style:color`        | evet     | evet     |                                                                                             |
| `gpx_style:line`         | partial | partial | `color`, `opacity`, `width`, `lineCap` desteklenir.                                           |
| `gpx_style:opacity`      | evet     | evet     |                                                                                             |
| `gpx_style:width`        | evet     | evet     |                                                                                             |
| `gpxx:DisplayColor`      | evet     | hayır      | Bir şeklin rengini belirtmek için kullanılır. Yazarken `gpx_style:line` Color kullanılacaktır.  |
| `gpxx:RouteExtension`    | partial | hayır      | Tüm özellikler `properties`okundu. Yalnızca `DisplayColor` kullanılır.                     |
| `gpxx:TrackExtension`    | partial | hayır      | Tüm özellikler `properties`okundu. Yalnızca `DisplayColor` kullanılır.                     |
| `gpxx:WaypointExtension` | partial | hayır      | Tüm özellikler `properties`okundu. Yalnızca `DisplayColor` kullanılır.                     |
| `gpx:keywords`           | evet     | evet     |                                                                                             |
| `gpx:fix`                | evet     | evet     |                                                                                             |

#### <a name="additional-notes"></a>Ek notlar

Yazarken;

- Çoklu noktaları, tek tek yol noktalarına bölünür.
- Çokgenler ve MultiPolygon, iz olarak yazılır. 
  
## <a name="supported-well-known-text-geometry-types"></a>Desteklenen Iyi bilinen metin geometrisi türleri

| Geometri türü | Okuma | Yazma |
|--------------|:----:|:-----:|
| SEÇENEĞININ | x | x |
| Z NOKTASı | x | x | 
| A NOKTASı | x | x<sup>[2]</sup> |
| ZM NOKTASı | x<sup>[1]</sup><sup>[2]</sup> | | 
| LINESTRING | x | x |
| LINESTRING Z | x | x | 
| LINESTRING D | x | x<sup>[2]</sup> |
| LINESTRING ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| Gen | x | x |
| ÇOKGEN Z | x | x |
| ÇOKGEN A | x | x<sup>[2]</sup> |
| ÇOKGEN ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| NOKTALı | x | x |
| MULTIPOINT Z | x | x | 
| MULTIPOINT A | x | x<sup>[2]</sup> |
| POMULTIPOINTıNT ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| MULTILINESTRING | x | x |
| MULTILINESTRING Z | x | x | 
| MULTILINESTRING D | x | x<sup>[2]</sup> |
| MULTILINESTRING ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| MULTIPOLYGON | x | x |
| MULTIPOLYGON Z | x | x | 
| MULTIPOLYGON | x | x<sup>[2]</sup> |
| MULTIPOLYGON ZM | x<sup>[1]</sup><sup>[2]</sup> | | 
| GEOMETRYCOLLECTION | x | x |
| GEOMETRYCOLLECTION Z | x | x | 
| GEOMETRYCOLLECTION D | x | x<sup>[2]</sup> | 
| GEOMETRYCOLLECTION ZM | x<sup>[1]</sup><sup>[2]</sup> | x | 

\[1\] yalnızca Z parametresi yakalanır ve konum değerinde üçüncü bir değer olarak eklenir.

\[2\] a parametresi yakalanmadı.

## <a name="delimited-spatial-data-support"></a>Sınırlandırılmış uzamsal veri desteği

Virgülle ayrılmış değer dosyaları (CSV) gibi ayrılmış uzamsal veriler genellikle uzamsal verileri içeren sütunlara sahiptir. Örneğin, enlem ve boylam bilgilerini içeren sütunlar olabilir. Iyi bilinen metin biçiminde, uzamsal geometri verisi içeren bir sütun olabilir.

### <a name="spatial-data-column-detection"></a>Uzamsal veri sütunu algılama

Uzamsal verileri içeren ayrılmış bir dosyayı okurken, hangi sütunların konum alanlarını içerdiğini belirleyen üst bilgi analiz edilir. Üst bilgi tür bilgisi içeriyorsa, hücre değerlerini uygun türe dönüştürmek için kullanılır. Üst bilgi belirtilmemişse, ilk satır analiz edilecek ve başlık oluşturmak için kullanılacaktır. İlk satırı analiz edilirken, sütun adlarını aşağıdaki adlarla (büyük/küçük harf duyarsız bir şekilde) eşleştirmek için bir denetim yürütülür. Bir dosyada iki veya daha fazla ad olması durumunda adların sırası önceliktir.

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

#### <a name="elevation"></a>Nedeniyle

- `elevation`
- `elv`
- `altitude`
- `alt`
- `z`

#### <a name="geography"></a>Coğrafya

İlk veri satırı, Iyi bilinen metin biçiminde olan dizeler için taranır. 

### <a name="delimited-data-column-types"></a>Sınırlandırılmış veri sütunu türleri

Üstbilgi satırı taranırken, sütun adındaki herhangi bir tür bilgisi ayıklanır ve bu sütundaki hücreleri dönüştürmek için kullanılır. İşte bir tür değeri olan bir sütun adı örneği: "ColumnName (typeName)". Aşağıdaki büyük/küçük harf duyarsız tür adları desteklenir:

#### <a name="numbers"></a>Numaralar

- EDM. Int64
- int
- long
- EDM. Double
- float
- double
- number

#### <a name="booleans"></a>Boole değerleri

- EDM. Boolean
- bool
- boole

#### <a name="dates"></a>Tarihler

- EDM. DateTime
- date
- datetime

#### <a name="geography"></a>Coğrafya

- EDM. Coğrafya
- Coğrafya

#### <a name="strings"></a>Dizeler

- EDM. String
- varchar
- metin
- Case ' dizesi

Üst bilgiden tür bilgisi ayıklanamaz ve bu, okurken dinamik yazma seçeneği etkinleştirildiyse, her bir hücre, ne kadar uygun olan veri türünü belirlemek için tek tek analiz edilir.

## <a name="next-steps"></a>Sonraki adımlar

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Uzamsal verileri okuma ve yazma](spatial-io-read-write-spatial-data.md)
