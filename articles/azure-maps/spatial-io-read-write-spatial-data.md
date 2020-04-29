---
title: Uzamsal verileri okuma ve yazma | Microsoft Azure haritaları
description: Azure Haritalar Web SDK 'Sı tarafından sunulan uzamsal GÇ modülünü kullanarak verileri okumayı ve yazmayı öğrenin.
author: philmea
ms.author: philmea
ms.date: 03/01/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 4c47335689401ebce98224992c74c3396821a1dd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80334150"
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

`atlas.io.read` İşlevi, uzamsal verilerle KML, GPX, GeoRSS, GEOJSON ve CSV dosyaları gibi yaygın uzamsal veri biçimlerini okumak için kullanılan ana işlevdir. Bu işlev, ZIP dosyası veya bir KMZ dosyası olarak bu biçimlerin sıkıştırılmış sürümlerini de okuyabilir. KMZ dosya biçimi, görüntü gibi varlıkları da içerebilen, KML 'in sıkıştırılmış bir sürümüdür. Alternatif olarak, Read işlevi bu biçimlerden herhangi birinde bir dosyaya işaret eden bir URL 'yi alabilir. URL 'Ler CORS etkin bir uç noktada barındırılmalıdır veya okuma seçeneklerinde bir proxy hizmeti sağlanmalıdır. Proxy hizmeti, CORS 'nin etkinleştirildiği etki alanlarında kaynakları yüklemek için kullanılır. Read işlevi, haritaya görüntü simgeleri eklemek için bir Promise döndürür ve UI iş parçacığı üzerindeki etkiyi en aza indirmek için verileri zaman uyumsuz olarak işler.

Sıkıştırılmış bir dosyayı ZIP veya bir KMZ olarak okurken, dosyanın sıkıştırması kaldırılır ve ilk geçerli dosya için taranmaz. Örneğin, Doc. kml veya geçerli uzantıya sahip bir dosya; örneğin:. kml,. xml, GeoJSON,. JSON,. csv,. tsv veya. txt. Daha sonra, KML ve GeoRSS dosyalarında başvurulan görüntüler erişilebilir olduklarından emin olmak için önceden yüklenir. Erişilemeyen görüntü verileri alternatif bir geri dönüş görüntüsü yükleyebilir veya stillerden kaldırılacak. KMZ dosyalarından ayıklanan görüntüler, veri URI 'Leri olarak dönüştürülür.

Read işlevinin sonucu bir `SpatialDataSet` nesnedir. Bu nesne GeoJSON FeatureCollection sınıfını genişletir. Özelliklerini bir haritada işlemek için kolayca olarak `DataSource` bir olarak geçirilebilir. `SpatialDataSet` Yalnızca özellik bilgilerini içermez, ancak aşağıdaki tabloda özetlenen KML zemin yer paylaşımlarını, işleme ölçümlerini ve diğer ayrıntıları da içerebilir.

| Özellik adı | Tür | Açıklama | 
|---------------|------|-------------|
| `bbox` | `BoundingBox` | Veri kümesindeki tüm verilerin sınırlayıcı kutusu. |
| `features` | `Feature[]` | Veri kümesi içindeki GeoJSON özellikleri. |
| `groundOverlays` | `(atlas.layer.ImageLayer | atlas.layers.OgcMapLayer)[]` | Bir KML Groundoveryerleştirme dizisi. |
| `icons` | Kayıt&lt;dizesi, dize&gt; | Bir simge URL 'Si kümesi. Anahtar = simge adı, değer = URL. |
| properties | kaydedilmemiş | Uzamsal veri kümesinin belge düzeyinde sunulan özellik bilgileri. |
| `stats` | `SpatialDataSetStats` | Uzamsal veri kümesinin içerik ve işlem süresi hakkında istatistikler. |
| `type` | `'FeatureCollection'` | Salt okunurdur GeoJSON türü değeri. |

## <a name="examples-of-reading-spatial-data"></a>Uzamsal verileri okuma örnekleri

Aşağıdaki kod, uzamsal veri kümesinin nasıl okunacağını ve `SimpleDataLayer` sınıfını kullanarak haritada nasıl işleneceğini gösterir. Kod, URL tarafından işaret edilen bir GPX dosyası kullanır.

<br/>

<iframe height='500' scrolling='no' title='Uzamsal verileri basit yükleme' src='//codepen.io/azuremaps/embed/yLNXrZx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından basit olan kalem <a href='https://codepen.io/azuremaps/pen/yLNXrZx/'>Yük uzamsal verilerine</a> bakın.
</iframe>

Sonraki kod tanıtımı, KML veya KMZ 'in haritaya nasıl okunacağını ve yükleneceğini gösterir. KML, `ImageLyaer` veya `OgcMapLayer`biçiminde olacak olan zemin Yerpaylaşımları içerebilir. Bu yer paylaşımları eşlemde özelliklerden ayrı eklenmelidir. Ayrıca, veri kümesinde özel simgeler varsa, Özellikler yüklenmeden önce bu simgelerin haritalar kaynaklarına yüklenmesi gerekir.

<br/>

<iframe height='500' scrolling='no' title='KML 'i haritaya yükle' src='//codepen.io/azuremaps/embed/XWbgwxX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile harita Için bkz. Pen <a href='https://codepen.io/azuremaps/pen/XWbgwxX/'>Load KML</a> .
</iframe>

İsteğe bağlı olarak, CORS 'nin etkinleştirildiği etki alanları arası varlıklara erişim için bir proxy hizmeti sağlayabilirsiniz. Read işlevi, önce CORS kullanarak başka bir etki alanındaki dosyalara erişmeye çalışacaktır. CORS kullanarak başka bir etki alanındaki herhangi bir kaynağa ilk kez erişemediğinde, bu, yalnızca bir proxy hizmeti sağlanmışsa ek dosyalar ister. Read işlevi, belirtilen proxy URL 'sinin sonuna dosya URL 'sini ekler. Bu kod parçacığı, bir Proxy hizmetinin okuma işlevine nasıl geçirileceğini gösterir:

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

Aşağıdaki demo, ayrılmış bir dosyanın nasıl okunacağını ve haritada nasıl işleneceğini gösterir. Bu durumda, kod uzamsal veri sütunları olan bir CSV dosyası kullanır.

<br/>

<iframe height='500' scrolling='no' title='Ayrılmış bir dosya ekleyin' src='//codepen.io/azuremaps/embed/ExjXBEb/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile birlikte, <a href='https://codepen.io'>codepen</a>Ile <a href='https://codepen.io/azuremaps/pen/ExjXBEb/'>ayrılmış bir dosya ekleme</a> .
</iframe>

## <a name="write-spatial-data"></a>Uzamsal verileri yazma

Uzamsal GÇ modülünde iki ana yazma işlevi vardır. `atlas.io.write` İşlev bir dize oluşturur, ancak `atlas.io.writeCompressed` işlev sıkıştırılmış bir ZIP dosyası oluşturur. Sıkıştırılmış ZIP dosyası, içindeki uzamsal verileri içeren metin tabanlı bir dosya içerir. Bu işlevlerin her ikisi de verileri dosyaya eklemek için bir Promise döndürür. Ayrıca, her ikisi de şu verilerden birini yazabilir `SpatialDataSet`:, `DataSource`, `ImageLayer`, `OgcMapLayer`, özellik koleksiyonu, özellik, geometri veya bu veri türlerinin herhangi bir birleşiminin dizisi. Her iki işlevi kullanarak yazarken istenen dosya biçimini belirtebilirsiniz. Dosya biçimi belirtilmemişse, veriler KML olarak yazılır.

Aşağıdaki araç, `atlas.io.write` işleviyle kullanılabilecek yazma seçeneklerinin çoğunluğunu gösterir.

<br/>

<iframe height='700' scrolling='no' title='Uzamsal veri yazma seçenekleri' src='//codepen.io/azuremaps/embed/YzXxXPG/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/YzXxXPG/'>uzamsal verileri yazma seçeneklerine</a> bakın.
</iframe>

## <a name="example-of-writing-spatial-data"></a>Uzamsal veri yazma örneği

Aşağıdaki örnek, haritada uzamsal dosyaları sürükleyip bırakmayı ve daha sonra yüklemeyi sağlar. Formatjson verilerini eşlemden dışa aktarabilir ve desteklenen uzamsal veri biçimlerinden birine dize veya sıkıştırılmış bir dosya olarak yazabilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='Uzamsal dosyaları harita üzerine sürükleyip bırakma' src='//codepen.io/azuremaps/embed/zYGdGoO/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Padepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/zYGdGoO/'>haritalara sürükleme ve bırakma uzamsal dosyalarını</a> görüntüleyin.
</iframe>

İsteğe bağlı olarak, CORS 'nin etkinleştirildiği etki alanları arası varlıklara erişim için bir proxy hizmeti sağlayabilirsiniz. Bu kod parçacığı, bir proxy hizmetini birleştirebilmeniz gerektiğini gösterir:

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

## <a name="read-and-write-well-known-text-wkt"></a>Iyi bilinen metin (WKT) okuma ve yazma

[Iyi bilinen metin](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) (wkt), uzamsal geometrilerin metin olarak temsil edilmesi için bir Open Geospatial Consortium (ogc) standardıdır. Birçok Jeo uzamsal sistem PostGIS eklentisini kullanarak Azure SQL ve Azure PostgreSQL gibi WKT 'yi destekler. Çoğu OGC standartları gibi, koordinatlar "x y" kuralına uyum sağlamak için "Boylam Enlem" olarak biçimlendirilir. Örnek olarak, Boylam-110 ve Latitude 45 ' deki bir nokta, WKT biçimi `POINT(-110 45)` kullanılarak yazılabilir.

İyi bilinen metin `atlas.io.ogc.WKT.read` işlevini kullanarak okunabilir ve `atlas.io.ogc.WKT.write` işlevi kullanılarak yazılabilir.

## <a name="examples-of-reading-and-writing-well-known-text-wkt"></a>Iyi bilinen metin (WKT) okuma ve yazma örnekleri

Aşağıdaki kod, iyi bilinen metin dizesinin `POINT(-122.34009 47.60995)` nasıl okunacağını ve bir kabarcık katmanı kullanarak haritada nasıl işleneceğini gösterir.

<br/>

<iframe height='500' scrolling='no' title='Iyi bilinen metni oku' src='//codepen.io/azuremaps/embed/XWbabLd/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile <a href='https://codepen.io/azuremaps/pen/XWbabLd/'>birlikte bilinen metin okuma</a> kalemine bakın.
</iframe>

Aşağıdaki kod, iyi bilinen metinleri okumayı ve okumayı ve geri yazmayı gösterir.

<br/>

<iframe height='700' scrolling='no' title='Iyi bilinen metni okuma ve yazma' src='//codepen.io/azuremaps/embed/JjdyYav/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/JjdyYav/'>okuma ve yazma iyi bilinen metinler</a> bölümüne bakın.
</iframe>

## <a name="read-and-write-gml"></a>GML oku ve yaz

GML, genellikle diğer XML belirtimlerine uzantı olarak kullanılan bir uzamsal XML dosya belirtimidir. GeoJSON verileri, `atlas.io.core.GmlWriter.write` işlevi kullanılarak GML etiketleriyle XML olarak yazılabilir. GML içeren XML, `atlas.io.core.GmlReader.read` işlevi kullanılarak okunabilir. Read işlevinin iki seçeneği vardır:

- `isAxisOrderLonLat` Seçenek-"enlem, Boylam" veya "boylam, enlem" koordinatlarının eksen sırası veri kümeleri arasında farklılık gösterebilir ve her zaman iyi tanımlı değildir. Varsayılan olarak, GML okuyucu koordinat verileri "enlem, Boylam" olarak okur, ancak bu seçeneğin true olarak ayarlanması bunu "boylam, enlem" olarak okuyacaktır.
- `propertyTypes` Seçenek-bu seçenek, anahtarın veri kümesindeki bir özelliğin adı olduğu bir anahtar değer arama tablosudur. Değer, ayrıştırılırken değeri dönüştürmek için nesne türüdür. Desteklenen tür değerleri şunlardır `string`:, `number`, `boolean`, ve. `date` Bir özellik arama tablosunda değilse veya tür tanımlanmamışsa, özelliği bir dize olarak ayrıştırılacaktır.

`atlas.io.read` İşlevi, GIRIŞ verilerinin XML olduğunu `atlas.io.core.GmlReader.read` algıladığında işlevi varsayılan olarak görür, ancak VERILER diğer destek uzamsal XML biçimlerinden biri değildir.

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
