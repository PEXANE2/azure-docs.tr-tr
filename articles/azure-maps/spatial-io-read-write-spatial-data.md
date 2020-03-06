---
title: Uzamsal verileri okuma ve yazma | Microsoft Azure haritaları
description: Azure Haritalar Web SDK 'Sı tarafından sunulan uzamsal GÇ modülünü kullanarak verileri okumayı ve yazmayı öğrenin.
author: farah-alyasari
ms.author: v-faalya
ms.date: 03/01/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 458b307cf1158c467100e032e3f789462e8fdcca
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370914"
---
# <a name="read-and-write-spatial-data"></a>Uzamsal verileri okuma ve yazma

Aşağıdaki tabloda, uzamsal GÇ modülü ile okuma ve yazma işlemleri için desteklenen uzamsal dosya biçimleri listelenmektedir.

| Veri biçimi       | Okuma | Yazma |
|-------------------|------|-------|
| GeoJSON           | ✓  |  ✓  |
| GeoRSS            | ✓  |  ✓  |
| GML               | ✓  |  ✓  |
| GPX               | ✓  |  ✓  |
| KML               | ✓  |  ✓  |
| KMZ               | ✓  |  ✓  |
| Uzamsal CSV       | ✓  |  ✓  |
| İyi bilinen metin   | ✓  |  ✓  |

Bu sonraki bölümlerde, uzamsal GÇ modülünü kullanarak uzamsal verileri okumak ve yazmak için tüm farklı araçlar ana hatlarıyla verilmektedir.

## <a name="read-spatial-data"></a>Uzamsal verileri okuma

`atlas.io.read` işlevi, uzamsal verilerle KML, GPX, GeoRSS, GeoJSON ve CSV dosyaları gibi yaygın uzamsal veri biçimlerini okumak için kullanılan ana işlevdir. Bu işlev, ZIP dosyası veya bir KMZ dosyası olarak bu biçimlerin sıkıştırılmış sürümlerini de okuyabilir. KMZ dosya biçimi, görüntü gibi varlıkları da içerebilen, KML 'in sıkıştırılmış bir sürümüdür. Alternatif olarak, Read işlevi bu biçimlerden herhangi birinde bir dosyaya işaret eden bir URL 'yi alabilir. URL 'Ler CORs etkin bir uç noktada barındırılmalıdır veya okuma seçeneklerinde bir proxy hizmeti sağlanmalıdır. Proxy hizmeti, CORs 'nin etkinleştirildiği etki alanlarında kaynakları yüklemek için kullanılır. Read işlevi, haritaya görüntü simgeleri eklemek için bir Promise döndürür ve UI iş parçacığı üzerindeki etkiyi en aza indirmek için verileri zaman uyumsuz olarak işler.

Sıkıştırılmış bir dosyayı ZIP veya bir KMZ olarak okurken, dosyanın sıkıştırması kaldırılır ve ilk geçerli dosya için taranmaz. Örneğin, Doc. kml veya geçerli uzantıya sahip bir dosya; örneğin:. kml,. xml, GeoJSON,. JSON,. csv,. tsv veya. txt. Daha sonra, KML ve GeoRSS dosyalarında başvurulan görüntüler erişilebilir olduklarından emin olmak için önceden yüklenir. Erişilemeyen görüntü verileri alternatif bir geri dönüş görüntüsü yükleyebilir veya stillerden kaldırılacak. KMZ dosyalarından ayıklanan görüntüler, veri URI 'Leri olarak dönüştürülür.

Read işlevinin sonucu bir `SpatialDataSet` nesnesidir. Bu nesne GeoJSON FeatureCollection sınıfını genişletir. Bir harita üzerinde özelliklerini işlemek için `DataSource` olduğu gibi kolayca geçirilebilir. `SpatialDataSet` yalnızca özellik bilgilerini içermez, ancak aşağıdaki tabloda özetlenen KML kara yer paylaşımlarını, işleme ölçümlerini ve diğer ayrıntıları da içerebilir.

| Özellik adı | Tür | Açıklama | 
|---------------|------|-------------|
| `bbox` | `BoundingBox` | Veri kümesindeki tüm verilerin sınırlayıcı kutusu. |
| `features` | `Feature[]` | Veri kümesi içindeki GeoJSON özellikleri. |
| `groundOverlays` | `(atlas.layer.ImageLayer | atlas.layers.OgcMapLayer)[]` | Bir KML Groundoveryerleştirme dizisi. |
| `icons` | &lt;dize, dize&gt; Kaydet | Bir simge URL 'Si kümesi. Anahtar = simge adı, değer = URL. |
| properties | kaydedilmemiş | Uzamsal veri kümesinin belge düzeyinde sunulan özellik bilgileri. |
| `stats` | `SpatialDataSetStats` | Uzamsal veri kümesinin içerik ve işlem süresi hakkında istatistikler. |
| `type` | `'FeatureCollection'` | Salt okunabilir coğrafi JSON türü değeri. |

## <a name="examples-of-reading-spatial-data"></a>Uzamsal verileri okuma örnekleri

Aşağıdaki kod, basit bir uzamsal veri kümesinin nasıl okunacağını ve `SimpleDataLayer` sınıfını kullanarak haritada nasıl işleneceğini gösterir. Kod, URL tarafından işaret edilen bir GPX dosyası kullanır.

<br/>

<iframe height='500' scrolling='no' title='Uzamsal verileri basit yükleme' src='//codepen.io/azuremaps/embed/yLNXrZx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından basit olan kalem <a href='https://codepen.io/azuremaps/pen/yLNXrZx/'>Yük uzamsal verilerini</a> inceleyin.
</iframe>

Sonraki kod tanıtımı, KML veya KMZ 'in haritaya nasıl okunacağını ve yükleneceğini gösterir. KML, `ImageLyaer` veya `OgcMapLayer`biçiminde olacak arka üste bindirme içerebilir. Bu yer paylaşımları eşlemde özelliklerden ayrı eklenmelidir. Ayrıca, veri kümesinde özel simgeler varsa, Özellikler yüklenmeden önce bu simgelerin haritalar kaynaklarına yüklenmesi gerekir.

<br/>

<iframe height='500' scrolling='no' title='KML 'i haritaya yükle' src='//codepen.io/azuremaps/embed/XWbgwxX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (@azuremaps) Ile Ilgili <a href='https://codepen.io/azuremaps/pen/XWbgwxX/'>KML Pen Load</a> (<a href='https://codepen.io/azuremaps'> </a>) sayfasına bakın.
</iframe>

İsteğe bağlı olarak, CORs 'nin etkinleştirildiği etki alanları arası varlıklara erişim için bir proxy hizmeti sağlayabilirsiniz. Bu kod parçacığı, bir proxy hizmeti sağlayabilmeniz gerektiğini gösterir:

```javascript

//Set the location of your proxyServiceUrl file 
var proxyServiceUrl = window.location.origin + '/CorsEnabledProxyService.ashx?url=';

//Read a KML file from a URL or pass in a raw KML string.
atlas.io.read('https://s3-us-west-2.amazonaws.com/s.cdpn.io/1717245/2007SanDiegoCountyFires.kml', {
    //Provide a proxy service
    proxyService: proxyServiceUrl
}).then(async r => {
    if (r) {
        // Some code goes here . . .
    }
});

```

Aşağıdaki son demo, ayrılmış bir dosyanın nasıl okunacağını ve haritada nasıl işleneceğini gösterir. Bu durumda, kod uzamsal veri sütunları olan bir CSV dosyası kullanır.

<br/>

<iframe height='500' scrolling='no' title='Ayrılmış bir dosya ekleyin' src='//codepen.io/azuremaps/embed/ExjXBEb/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile <a href='https://codepen.io/azuremaps/pen/ExjXBEb/'>ayrılmış bir dosya ekleme</a> kalemine bakın.
</iframe>

## <a name="write-spatial-data"></a>Uzamsal verileri yazma

Uzamsal GÇ modülünde iki ana yazma işlevi vardır. `atlas.io.write` işlevi bir dize oluşturur, ancak `atlas.io.writeCompressed` işlevi sıkıştırılmış bir ZIP dosyası oluşturur. Sıkıştırılmış ZIP dosyası, içindeki uzamsal verileri içeren metin tabanlı bir dosya içerir. Bu işlevlerin her ikisi de verileri dosyaya eklemek için bir Promise döndürür. Ayrıca, her ikisi de şu verilerden birini yazabilir: `SpatialDataSet`, `DataSource`, `ImageLayer`, `OgcMapLayer`, özellik koleksiyonu, özellik, geometri veya bu veri türlerinin herhangi bir birleşiminin dizisi. Her iki işlevi kullanarak yazarken istenen dosya biçimini belirtebilirsiniz. Dosya biçimi belirtilmemişse, veriler KML olarak yazılır.

Aşağıdaki araç, `atlas.io.write` işleviyle kullanılabilecek yazma seçeneklerinin çoğunluğunu gösterir.

<br/>

<iframe height='700' scrolling='no' title='Uzamsal veri yazma seçenekleri' src='//codepen.io/azuremaps/embed/YzXxXPG/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/YzXxXPG/'>uzamsal verileri yazma seçeneklerine</a> bakın.
</iframe>

## <a name="example-of-writing-spatial-data"></a>Uzamsal veri yazma örneği

Aşağıdaki örnek, haritada uzamsal dosyaları sürükleyip bırakmayı ve daha sonra yüklemeyi sağlar. Formatjson verilerini eşlemden dışa aktarabilir ve desteklenen uzamsal veri biçimlerinden birine dize veya sıkıştırılmış bir dosya olarak yazabilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='Uzamsal dosyaları harita üzerine sürükleyip bırakma' src='//codepen.io/azuremaps/embed/zYGdGoO/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/zYGdGoO/'>haritalara sürükleme ve bırakma uzamsal dosyalarını</a> görüntüleyin.
</iframe>

İsteğe bağlı olarak, CORs 'nin etkinleştirildiği etki alanları arası varlıklara erişim için bir proxy hizmeti sağlayabilirsiniz. Bu kod parçacığı, bir proxy hizmetini birleştirebilmeniz gerektiğini gösterir:

```javascript

//Set the location of your proxyServiceUrl file 
var proxyServiceUrl = window.location.origin + '/CorsEnabledProxyService.ashx?url=';

function readData(data, fileName) {
    loadingIcon.style.display = '';
    fileCount++;
    //Attempt to parse the file and add the shapes to the map.
    atlas.io.read(data, {
        //Provide a proxy service
        proxyService: proxyServiceUrl
    }).then(
        //Success
        function(r) {
            //some code goes here ...
        }
    );
}
```

## <a name="read-and-write-well-known-text-wkt"></a>Iyi bilinen metinleri okuma ve yazma (WKT)

[Iyi bilinen metin](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) (wkt), uzamsal geometrilerin metin olarak temsil edilmesi için bir Open Geospatial Consortium (ogc) standardıdır. Birçok Jeo uzamsal sistem PostGIS eklentisini kullanarak Azure SQL ve Azure PostgreSQL gibi WKT 'yi destekler. Çoğu OGC standartları gibi, koordinatlar "x y" kuralına uyum sağlamak için "Boylam Enlem" olarak biçimlendirilir. Örnek olarak, Boylam-110 ve Latitude 45 ' deki bir nokta, WKT biçimi kullanılarak `POINT(-110 45)` olarak yazılabilir.

İyi bilinen metinler `atlas.io.ogc.WKT.read` işlevi kullanılarak okunabilir ve `atlas.io.ogc.WKT.write` işlevi kullanılarak yazılabilir.

## <a name="examples-of-reading-and-writing-well-known-text-wkt"></a>Iyi bilinen metinleri okuma ve yazma örnekleri (WKT)

Aşağıdaki kod, `POINT(-122.34009 47.60995)` iyi bilinen metin dizesinin nasıl okunacağını ve bir kabarcık katmanı kullanarak haritada nasıl işleneceğini gösterir.

<br/>

<iframe height='500' scrolling='no' title='Bilinen tanınmış metni oku' src='//codepen.io/azuremaps/embed/XWbabLd/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/XWbabLd/'>okunan kalemin yanı sıra</a> bkz.
</iframe>

Aşağıdaki kod, iyi bilinen metinleri okumayı ve okumayı ve geri yazmayı gösterir.

<br/>

<iframe height='700' scrolling='no' title='Iyi bilinen metinleri okuma ve yazma' src='//codepen.io/azuremaps/embed/JjdyYav/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/JjdyYav/'>birlikte okuma ve yazma</a> hakkında daha fazla bilgi alın.
</iframe>

## <a name="read-and-write-gml"></a>GML oku ve yaz

GML, genellikle diğer XML belirtimlerine uzantı olarak kullanılan bir uzamsal XML dosya belirtimidir. GeoJSON verileri, `atlas.io.core.GmlWriter.write` işlevi kullanılarak GML etiketleriyle XML olarak yazılabilir. GML içeren XML, `atlas.io.core.GmlReader.read` işlevi kullanılarak okunabilir. Read işlevinin iki seçeneği vardır:

- `isAxisOrderLonLat` seçeneği-"enlem, Boylam" veya "boylam, enlem" koordinatlarının eksen sırası veri kümeleri arasında farklılık gösterebilir ve her zaman iyi tanımlı değildir. Varsayılan olarak, GML okuyucu koordinat verileri "enlem, Boylam" olarak okur, ancak bu seçeneğin true olarak ayarlanması bunu "boylam, enlem" olarak okuyacaktır.
- `propertyTypes` seçeneği-bu seçenek, anahtarın veri kümesindeki bir özelliğin adı olduğu bir anahtar değer arama tablosudur. Değer, ayrıştırılırken değeri dönüştürmek için nesne türüdür. Desteklenen tür değerleri şunlardır: `string`, `number`, `boolean`ve `date`. Bir özellik arama tablosunda değilse veya tür tanımlı değilse, özellik bir dize olarak ayrıştırılacaktır.

`atlas.io.read` işlevi, giriş verilerinin XML olduğunu algıladığında `atlas.io.core.GmlReader.read` işlevi için varsayılan olur, ancak veriler diğer destek uzamsal XML biçimlerinden biri değildir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [atlas.io statik işlevleri](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io)

> [!div class="nextstepaction"]
> [SpatialDataSet](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.spatialdataset)

> [!div class="nextstepaction"]
> [SpatialDataSetStats](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.spatialdatasetstats)

> [!div class="nextstepaction"]
> [GmlReader](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.core.gmlreader?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [GmlWriter](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.core.gmlwriter?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [Atlas. IO. ogc. WKT işlevleri](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.io.ogc.wkt)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [OGC eşleme katmanı ekleme](spatial-io-add-ogc-map-layer.md)

> [!div class="nextstepaction"]
> [WFS hizmetine bağlanma](spatial-io-connect-wfs-service.md)

> [!div class="nextstepaction"]
> [Çekirdek işlemlerden yararlanın](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)
