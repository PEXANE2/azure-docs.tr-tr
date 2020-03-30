---
title: Uzamsal verileri okuma ve yazma | Microsoft Azure Haritaları
description: Azure Maps Web SDK tarafından sağlanan Uzamsal IO modüllerini kullanarak verileri nasıl okuyup yazarak okuyup yazarak öğrenin.
author: philmea
ms.author: philmea
ms.date: 03/01/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 4c47335689401ebce98224992c74c3396821a1dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334150"
---
# <a name="read-and-write-spatial-data"></a>Uzamsal verileri okuma ve yazma

Aşağıdaki tabloda, Uzamsal IO modülü ile okuma ve yazma işlemleri için desteklenen uzamsal dosya biçimleri listelenmektedir.

| Veri Biçimi       | Okuma | Yazma |
|-------------------|------|-------|
| GeoJSON           | ✓  |  ✓  |
| Georss            | ✓  |  ✓  |
| Gml               | ✓  |  ✓  |
| Gpx               | ✓  |  ✓  |
| Kml               | ✓  |  ✓  |
| Kmz               | ✓  |  ✓  |
| Mekansal CSV       | ✓  |  ✓  |
| Tanınmış Metin   | ✓  |  ✓  |

Bu sonraki bölümlerde uzamsal IO modüllerini kullanarak uzamsal verileri okumak ve yazmak için tüm farklı araçlar sıralanmİşolur.

## <a name="read-spatial-data"></a>Uzamsal verileri okuma

İşlev, `atlas.io.read` KML, GPX, GeoRSS, GeoJSON ve CSV dosyaları gibi ortak uzamsal veri biçimlerini uzaysal verilerle okumak için kullanılan ana işlevdir. Bu işlev, zip dosyası veya KMZ dosyası olarak bu biçimlerin sıkıştırılmış sürümlerini de okuyabilir. KMZ dosya biçimi, KML'nin görüntüler gibi varlıkları da içerebilen sıkıştırılmış bir sürümüdür. Alternatif olarak, okuma işlevi, bu biçimlerden herhangi birinde bir dosyayı işaret eden bir URL alabilir. URL'ler CORS özellikli bir uç noktada barındırılmalı veya okuma seçeneklerinde bir proxy hizmeti sağlanmalıdır. Proxy hizmeti, CORS etkin olmayan etki alanlarında kaynak yüklemek için kullanılır. Okuma işlevi, görüntü simgelerini eşe ekleme sözü verir ve Kullanıcı Bira iş parçacığına etkisini en aza indirmek için verileri eşzamanlı olarak işler.

Sıkıştırılmış bir dosyayı zip veya KMZ olarak okurken, sıkıştırılmış dosya nın fermuarı çözülecek ve ilk geçerli dosya için taranacaktır. Örneğin, doc.kml veya .kml, .xml, geojson, .json, .csv, .tsv veya .txt gibi diğer geçerli uzantılı bir dosya. Ardından, KML ve GeoRSS dosyalarında başvurulan görüntüler erişilebilir olduğundan emin olmak için önceden yüklenir. Erişilemeyen görüntü verileri alternatif bir geri dönüş görüntüsü yükleyebilir veya stiller kaldırılacak. KMZ dosyalarından çıkarılan görüntüler veri URI'lerine dönüştürülür.

Okuma işlevinin sonucu bir `SpatialDataSet` nesnedir. Bu nesne GeoJSON FeatureCollection sınıfını genişletir. Kolayca bir harita üzerinde `DataSource` özelliklerini işlemek için bir as-is içine geçirilebilir. Yalnızca `SpatialDataSet` özellik bilgileri içermez, ancak aşağıdaki tabloda belirtildiği gibi KML yer kaplamaları, işleme ölçümleri ve diğer ayrıntıları da içerebilir.

| Özellik adı | Tür | Açıklama | 
|---------------|------|-------------|
| `bbox` | `BoundingBox` | Veri kümesindeki tüm verilerin sınırlayıcı kutusu. |
| `features` | `Feature[]` | GeoJSON veri kümesi içinde özellikleri. |
| `groundOverlays` | `(atlas.layer.ImageLayer | atlas.layers.OgcMapLayer)[]` | KML GroundOverlays bir dizi. |
| `icons` | Kayıt&lt;dizesi, dize&gt; | Simge URL'leri kümesi. Anahtar = simge adı, Değer = URL. |
| properties | herhangi bir | Uzamsal bir veri kümesinin belge düzeyinde sağlanan özellik bilgileri. |
| `stats` | `SpatialDataSetStats` | Uzamsal bir veri kümesinin içeriği ve işleme süresi yle ilgili istatistikler. |
| `type` | `'FeatureCollection'` | Salt okunur GeoJSON türü değeri. |

## <a name="examples-of-reading-spatial-data"></a>Uzamsal verileri okuma örnekleri

Aşağıdaki kod, uzamsal bir veri kümesinin nasıl okunduğunu `SimpleDataLayer` ve sınıfı kullanarak haritada nasıl işlenirolduğunu gösterir. Kod, BIR URL tarafından işaret edilen bir GPX dosyası kullanır.

<br/>

<iframe height='500' scrolling='no' title='Uzaysal Verileri Basit Yükle' src='//codepen.io/azuremaps/embed/yLNXrZx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Basit Kalem <a href='https://codepen.io/azuremaps/pen/yLNXrZx/'>Yükü Uzamsal Veri</a> ()
</iframe>

Sonraki kod demosu, Haritaya KML veya KMZ'nin nasıl okunup yüklenirken nasıl yüklenirken gösterilir. KML bir `ImageLyaer` veya `OgcMapLayer`şeklinde olacak zemin bindirmeleri içerebilir. Bu bindirmeler haritaya özelliklerden ayrı olarak eklenmelidir. Ayrıca, veri kümesinde özel simgeler varsa, özellikler yüklenmeden önce bu simgelerin haritalar kaynaklarına yüklenmesi gerekir.

<br/>

<iframe height='500' scrolling='no' title='KML'yi Haritaya Yükleyin' src='//codepen.io/azuremaps/embed/XWbgwxX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile Haritaya Kalem <a href='https://codepen.io/azuremaps/pen/XWbgwxX/'>Yükleme KML'ye</a> bakın.
</iframe>

Cors etkin olmayabilir çapraz etki alanı varlıklarına erişmek için isteğe bağlı olarak bir proxy hizmeti sağlayabilir. Okuma işlevi önce CORS'u kullanarak başka bir etki alanındaki dosyalara erişmeye çalışır. CORS'i kullanarak başka bir etki alanında herhangi bir kaynağa erişemediği ilk seferden sonra, yalnızca bir proxy hizmeti sağlanmışsa ek dosyalar talep eder. Okuma işlevi, dosya URL'sini sağlanan proxy URL'sinin sonuna ekler. Bu kod snippet okuma işlevine bir proxy hizmeti geçmek için nasıl gösterir:

```javascript
//Read a file from a URL or pass in a raw data as a string.
atlas.io.read('https://nonCorsDomain.example.com/mySuperCoolData.xml', {
    //Provide a proxy service
    proxyService: window.location.origin + '/YourCorsEnabledProxyService.ashx?url='
}).then(async r => {
    if (r) {
        // Some code goes here . . .
    }
});

```

Aşağıdaki demo, sınırlı bir dosyanın nasıl okunup haritada nasıl işlenirolduğunu gösterir. Bu durumda, kod uzamsal veri sütunları olan bir CSV dosyası kullanır.

<br/>

<iframe height='500' scrolling='no' title='Sınırlı Dosya Ekleme' src='//codepen.io/azuremaps/embed/ExjXBEb/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Kalem Ekle Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io/azuremaps/pen/ExjXBEb/'>'a</a> Göre Sınırlı Dosya Ekle ( )
</iframe>

## <a name="write-spatial-data"></a>Uzamsal veri yazma

Uzamsal IO modülünde iki ana yazma işlevi vardır. İşlev `atlas.io.write` sıkıştırılmış bir zip `atlas.io.writeCompressed` dosyası oluştururken, işlev bir dize oluşturur. Sıkıştırılmış zip dosyası, içinde uzamsal veriler bulunan metin tabanlı bir dosya içerir. Bu işlevlerin her ikisi de dosyaya veri eklemek için bir söz döndürün. Ve, her ikisi de aşağıdaki verilerden `DataSource` `ImageLayer`herhangi `OgcMapLayer`birini yazabilirsiniz: `SpatialDataSet`, , , özellik toplama, özellik, geometri, ya da bu veri türlerinin herhangi bir kombinasyonu bir dizi. Her iki işlevi kullanarak yazarken, istenen dosya biçimini belirtebilirsiniz. Dosya biçimi belirtilmemişse, veriler KML olarak yazılır.

Aşağıdaki `atlas.io.write` araç, işlevle birlikte kullanılabilecek yazma seçeneklerinin çoğunluğunu gösterir.

<br/>

<iframe height='700' scrolling='no' title='Uzamsal veri yazma seçenekleri' src='//codepen.io/azuremaps/embed/YzXxXPG/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/YzXxXPG/'>Uzamsal veri yazma seçeneklerine</a> bakın .
</iframe>

## <a name="example-of-writing-spatial-data"></a>Uzamsal veri yazma örneği

Aşağıdaki örnek, uzamsal dosyaları sürüklemenizi, bırakmanızı ve ardından haritaya yüklemenizi sağlar. GeoJSON verilerini haritadan dışa aktarabilir ve desteklenen uzamsal veri biçimlerinden birine dize olarak veya sıkıştırılmış dosya olarak yazabilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='Uzamsal dosyaları haritaya sürükleyin ve bırakın' src='//codepen.io/azuremaps/embed/zYGdGoO/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Kalem Sürükleme ve Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a>( ) ile <a href='https://codepen.io/azuremaps/pen/zYGdGoO/'>haritaya uzaysal dosyaları bırakın.</a>
</iframe>

Cors etkin olmayabilir çapraz etki alanı varlıklarına erişmek için isteğe bağlı olarak bir proxy hizmeti sağlayabilir. Bu kod snippet bir proxy hizmeti dahil olabilir gösterir:

```javascript
atlas.io.read(data, {
    //Provide a proxy service
    proxyService: window.location.origin + '/YourCorsEnabledProxyService.ashx?url='
}).then(
    //Success
    function(r) {
        //some code goes here ...
    }
);
```

## <a name="read-and-write-well-known-text-wkt"></a>Tanınmış Metni (WKT) okuma ve yazma

[Bilinen Metin](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) (WKT), uzamsal geometrileri metin olarak temsil eden bir Açık Jeouzamsal Konsorsiyum (OGC) standardıdır. Birçok coğrafi sistem, PostGIS eklentisini kullanarak Azure SQL ve Azure PostgreSQL gibi WKT'yi destekler. Çoğu OGC standardı gibi koordinatlar da "x y" kuralıyla uyumlu olacak şekilde "boylam enlemi" olarak biçimlendirilir. Örnek olarak, boylam -110 ve enlem 45'teki bir nokta WKT biçimi `POINT(-110 45)` kullanılarak yazılabilir.

İyi bilinen metin `atlas.io.ogc.WKT.read` işlevi kullanılarak okunabilir ve `atlas.io.ogc.WKT.write` işlevi kullanılarak yazılabilir.

## <a name="examples-of-reading-and-writing-well-known-text-wkt"></a>Tanınmış Metin (WKT) okuma ve yazma örnekleri

Aşağıdaki kod, bilinen metin dizesini `POINT(-122.34009 47.60995)` nasıl okuyup bir kabarcık katmanı kullanarak haritada nasıl işleyiniz gösterir.

<br/>

<iframe height='500' scrolling='no' title='Tanınmış Metni Oku' src='//codepen.io/azuremaps/embed/XWbabLd/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar tarafından Bilinen<a href='https://codepen.io/azuremaps'>@azuremaps</a>Metni <a href='https://codepen.io/azuremaps/pen/XWbabLd/'>Okuyun</a> ( )
</iframe>

Aşağıdaki kod, iyi bilinen metinleri ileri geri okumayı ve yazmayı gösterir.

<br/>

<iframe height='700' scrolling='no' title='Tanınmış Metni Okuma ve Yazma' src='//codepen.io/azuremaps/embed/JjdyYav/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Kalem Okuma ve Azure Haritaları<a href='https://codepen.io/azuremaps'>@azuremaps</a>() tarafından <a href='https://codepen.io/azuremaps/pen/JjdyYav/'>Bilinen Metni Okuyun ve yazın.</a>
</iframe>

## <a name="read-and-write-gml"></a>GML'yi okuma ve yazma

GML genellikle diğer XML belirtimlerine uzantı olarak kullanılan uzamsal bir XML dosya belirtimidir. GeoJSON verileri `atlas.io.core.GmlWriter.write` işlevi kullanılarak GML etiketleri ile XML olarak yazılabilir. GML içeren XML `atlas.io.core.GmlReader.read` işlevi kullanılarak okunabilir. Okuma işlevinin iki seçeneği vardır:

- Seçenek `isAxisOrderLonLat` - "enlem, boylam" veya "boylam, enlem" koordinatlarının eksen sırası veri kümeleri arasında değişebilir ve her zaman iyi tanımlanmamıştır. Varsayılan olarak GML okuyucu "enlem, boylam" olarak koordinat verileri okur, ancak doğru bu seçeneği ayarı "boylam, enlem" olarak okuyacaktır.
- Seçenek `propertyTypes` - Bu seçenek, anahtarın veri kümesindeki bir özelliğin adı olduğu anahtar değer arama tablosudur. Değer, ayrıştırma yaparken değeri döküm nesne türüdür. Desteklenen tür değerleri `string`şunlardır: , `number`, `boolean`, ve `date`. Bir özellik arama tablosunda değilse veya tür tanımlanmamışsa, özellik dize olarak ayrıştırılır.

Giriş `atlas.io.read` verilerinin XML olduğunu algıladığında `atlas.io.core.GmlReader.read` işlev varsayılan olarak işleve varsayılan olarak verilir, ancak veriler diğer destek uzamsal XML biçimlerinden biri değildir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [atlas.io statik fonksiyonlar](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io)

> [!div class="nextstepaction"]
> [UzamsalDataSet](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.spatialdataset)

> [!div class="nextstepaction"]
> [MekansalDataSetStats](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.spatialdatasetstats)

> [!div class="nextstepaction"]
> [GmlReader](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.core.gmlreader?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [GmlYazar](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.core.gmlwriter?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [atlas.io.ogc.WKT fonksiyonları](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.ogc.wkt)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [OGC harita katmanı ekleme](spatial-io-add-ogc-map-layer.md)

> [!div class="nextstepaction"]
> [WFS hizmetine bağlanma](spatial-io-connect-wfs-service.md)

> [!div class="nextstepaction"]
> [Temel operasyonlardan yararlanın](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)
