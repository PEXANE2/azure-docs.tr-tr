---
title: Basit bir veri katmanı ekleme | Microsoft Azure Haritaları
description: Azure Maps Web SDK tarafından sağlanan Uzamsal IO modüllerini kullanarak basit bir veri katmanı eklemeyi öğrenin.
author: philmea
ms.author: philmea
ms.date: 02/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 7671d07a468a9f67a4851ec828fe18896d7a6c66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334272"
---
# <a name="add-a-simple-data-layer"></a>Basit bir veri katmanı ekleme

Uzamsal IO `SimpleDataLayer` modülü bir sınıf sağlar. Bu sınıf, stil özelliklerinin haritaüzerinde işlenmesini kolaylaştırır. Stil özelliklerine ve karışık geometri türleri içeren veri kümelerine sahip veri kümelerini bile işleyebilir. Basit veri katmanı, birden çok işleme katmanını saran ve stil ifadelerini kullanarak bu işlevselliği sağlar. Stil ifadeleri, bu sarılmış katmanların içindeki özelliklerin ortak stil özelliklerini arar. İşlev `atlas.io.read` ve `atlas.io.write` işlev, stilleri desteklenen bir dosya biçimine okumak ve yazmak için bu özellikleri kullanır. Desteklenen bir dosya biçimine özellikleri ekledikten sonra, dosya çeşitli amaçlar için kullanılabilir. Örneğin, dosya haritada stil özellikleri görüntülemek için kullanılabilir.

Stil özelliklerine ek olarak, bir `SimpleDataLayer` pop-up şablonu ile yerleşik bir pop-up özelliği sağlar. Bir özellik tıklandığında açılır pencere görüntülenir. İstenirse varsayılan açılır pencere özelliği devre dışı alınabilir. Bu katman kümelenmiş verileri de destekler. Bir küme tıklatıldığında, harita kümeye yakınlaştırır ve onu tek tek noktalara ve alt kümelere genişletir.

Sınıf, `SimpleDataLayer` birçok geometri türü ve özelliklere uygulanan birçok stiliçeren büyük veri kümelerinde kullanılmak üzere tasarlanmıştır. Kullanıldığında, bu sınıf stil ifadeleri içeren altı katmandan oluşan bir ek yükü ekler. Yani, çekirdek işleme katmanlarını kullanmanın daha verimli olduğu durumlar vardır. Örneğin, bir özellik üzerinde birkaç geometri türü ve birkaç stil oluşturmak için bir çekirdek katmanı kullanın

## <a name="use-a-simple-data-layer"></a>Basit bir veri katmanı kullanma

Sınıf, `SimpleDataLayer` diğer işleme katmanları nın kullanıldığı gibi kullanılır. Aşağıdaki kod, haritada basit bir veri katmanının nasıl kullanılacağını gösterir:

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Add a simple data layer for rendering data.
var layer = new atlas.layer.SimpleDataLayer(datasource);
map.layers.add(layer);
```

Veri kaynağına özellikler ekleyin. Ardından, basit veri katmanı özellikleri en iyi nasıl işleyeceklerini bulur. Tek tek özellikler için stiller özelliği özellikleri olarak ayarlanabilir. Aşağıdaki kod, `color` `red`bir özellik olarak ayarlanmış bir GeoJSON noktası özelliğini gösterir. 

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [0, 0]
    },
    "properties": {
        "color": "red"
    }
}
```

Aşağıdaki kod, basit veri katmanını kullanarak yukarıdaki nokta özelliğini işler. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Basit veri katmanını kullanma" src="//codepen.io/azuremaps/embed/zYGzpQV/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true"> Bkz. Kalem <a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar'a göre <a href='https://codepen.io/azuremaps/pen/zYGzpQV/'>basit veri katmanını kullanın</a> .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

Basit veri katmanının gerçek gücü aşağıdaki zaman gelir:

- Bir veri kaynağında birkaç farklı özellik türü vardır; Veya
- Veri kümesindeki özelliklerin üzerinde ayrı ayrı ayarlanmış birkaç stil özelliği vardır; Veya
- Veri kümesinin tam olarak ne içerdiğinden emin değilseniz.

Örneğin, XML veri akışlarını ayrıştirırken, özelliklerin tam stillerini ve geometri türlerini bilmiyor olabilirsiniz. Aşağıdaki örnek, bir KML dosyasının özelliklerini işleyerek basit veri katmanının gücünü gösterir. Ayrıca, basit veri katmanı sınıfının sağladığı çeşitli seçenekleri de gösterir.

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Basit veri katmanı seçenekleri" src="//codepen.io/azuremaps/embed/gOpRXgy/?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true"> <a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Basit veri katmanı <a href='https://codepen.io/azuremaps/pen/gOpRXgy/'>seçeneklerine</a> bakın .
</iframe>


> [!NOTE]
> Bu basit veri [katmanı,](map-add-popup.md#add-popup-templates-to-the-map) KML balonlarını görüntülemek için açılır şablon sınıfını kullanır veya özellik özelliklerini tablo olarak görüntüler. Varsayılan olarak, açılır pencerede işlenen tüm içerik bir güvenlik özelliği olarak bir iframe içinde sandboxed olacaktır. Ancak, sınırlamalar vardır:
>
> - Tüm komut dosyaları, formlar, işaretçi kilidi ve üst gezinti işlevselliği devre dışı bırakılır. Tıklandığında bağlantıların yeni bir sekmede açılmasına izin verilir. 
> - iframe'lerde `srcdoc` parametreyi desteklemeyen eski tarayıcılar, az miktarda içerik işlemekle sınırlı olacaktır.
> 
> Açılan pencerelere yüklenen verilere güveniyorsanız ve bu komut dosyalarının açılır pencerelere yüklenmesini istiyorsanız, açılır pencere şablonları `sandboxContent` seçeneğini yanlış olarak ayarlayarak bunu devre dışı bırakabilirsiniz. 

## <a name="default-supported-style-properties"></a>Varsayılan desteklenen stil özellikleri

Daha önce de belirtildiği gibi, basit veri katmanı çekirdek işleme katmanları birkaç sarar: kabarcık, sembol, çizgi, çokgen ve ekstrüde çokgen. Daha sonra tek tek özelliklerüzerinde geçerli stil özelliklerini aramak için ifadeler kullanır.

Azure Haritalar ve GitHub stili özellikleri desteklenen iki özellik adı kümesidir. Farklı azure eşler katmanı seçeneklerinin çoğu özellik adları, basit veri katmanındaki özelliklerin stil özellikleri olarak desteklenir. İfadeler, GitHub tarafından yaygın olarak kullanılan stil özellik adlarını desteklemek için bazı katman seçeneklerine eklenmiştir. Bu özellik adları [GitHub'ın GeoJSON harita desteği](https://help.github.com/en/github/managing-files-in-a-repository/mapping-geojson-files-on-github)tarafından tanımlanır ve platform içinde depolanan ve işlenen GeoJSON dosyalarını şekillendirmek için kullanılır. GitHub'ın tüm stil özellikleri, stil özellikleri dışında basit `marker-symbol` veri katmanında desteklenir.

Okuyucu daha az yaygın bir stil özelliğiyle karşılaşırsa, okuyucuyu en yakın Azure Haritalar stili özelliğine dönüştürür. Ayrıca, varsayılan stil ifadeleri basit veri katmanının `getLayers` işlevini kullanarak ve katmanların herhangi birinde seçenekleri güncelleştirerek geçersiz kılınabilir.

Aşağıdaki bölümler, basit veri katmanı tarafından desteklenen varsayılan stil özellikleri hakkında ayrıntılar sağlar. Desteklenen özellik adının sırası da özelliğin önceliğidir. Aynı katman seçeneği için iki stil özelliği tanımlanırsa, listedeki ilk özellik daha yüksek önceliğe sahiptir.

### <a name="bubble-layer-style-properties"></a>Kabarcık katmanı stili özellikleri

Bir özellik bir `Point` veya `MultiPoint`bir ise ve özellik `image` bir sembol olarak nokta işlemek için özel bir simge olarak kullanılacak bir özelliği `BubbleLayer`yoksa, o zaman özellik bir .

| Katman seçeneği | Desteklenen özellik adı(lar) | Varsayılan değer |
|--------------|----------------------------|---------------|
| `color` | `color`, `marker-color` | `'#1A73AA'` |
| `radius` | `size`<sup>1</sup> `marker-size` <sup>2</sup>, `scale`2 , <sup>1</sup> | `8` |
| `strokeColor` | `strokeColor`, `stroke` | `'#FFFFFF'` |

\[1\] `size` Ve `scale` değerleri skaler değerler olarak kabul edilir ve bunlar ile çarpılır`8`

\[2\] GitHub `marker-size` seçeneği belirtilirse, yarıçap için aşağıdaki değerler kullanılır.

| İşaretçi boyutu | Yarıçap |
|-------------|--------|
| `small`     | `6`    |
| `medium`    | `8`    |
| `large`     | `12`   |

Kümeler de kabarcık katmanı kullanılarak işlenir. Varsayılan olarak bir kümenin yarıçapı `16`. Kümenin rengi, aşağıda tanımlandığı gibi kümedeki nokta sayısına bağlı olarak değişir:

| # puan | Renk    |
|-------------|----------|
| &gt;= 100   | `red`    |
| &gt;= 10    | `yellow` |
| &lt;10     | `green`  |

### <a name="symbol-style-properties"></a>Sembol stili özellikleri

Bir özellik bir `Point` veya `MultiPoint`bir , ve `image` özellik ve özelliği ve bir sembol olarak nokta işlemek için özel bir simge `SymbolLayer`olarak kullanılacak bir özelliği varsa, o zaman özellik bir .

| Katman seçeneği | Desteklenen özellik adı(lar) | Varsayılan değer |
|--------------|----------------------------|---------------|
| `image` | `image` | ``none`` |
| `size` | `size`, `marker-size` <sup>1,5,5</sup> 00 | `1` |
| `rotation` | `rotation` | `0` |
| `offset` | `offset` | `[0, 0]` |
| `anchor` | `anchor` | `'bottom'` |

\[1\] GitHub `marker-size` seçeneği belirtilirse, simge boyutu seçeneği için aşağıdaki değerler kullanılır.

| İşaretçi boyutu | Sembol boyutu |
|-------------|-------------|
| `small`     | `0.5`       |
| `medium`    | `1`         |
| `large`     | `2`         |

Nokta özelliği bir kümeise, `point_count_abbreviated` özellik metin etiketi olarak işlenir. Görüntü oluşturulmayacak.

### <a name="line-style-properties"></a>Çizgi stili özellikleri

`LineString`Özellik , , `MultiLineString`, `Polygon`veya `MultiPolygon`, ise, o zaman `LineLayer`özellik bir .

| Katman seçeneği | Desteklenen özellik adı(lar) | Varsayılan değer |
|--------------|----------------------------|---------------|
| `strokeColor` | `strokeColor`, `stroke` | `'#1E90FF'` |
| `strokeWidth` | `strokeWidth`, `stroke-width`, `stroke-thickness` | `3` |
| `strokeOpacity` | `strokeOpacity`, `stroke-opacity` | `1` |

### <a name="polygon-style-properties"></a>Çokgen tarzı özellikleri

Özellik `Polygon` bir veya bir `MultiPolygon`ise ve özellik ya bir `height` özelliği `height` yoksa ya da özelliği sıfırise, özellik `PolygonLayer`bir .

| Katman seçeneği | Desteklenen özellik adı(lar) | Varsayılan değer |
|--------------|----------------------------|---------------|
| `fillColor` | `fillColor`, `fill` | `'#1E90FF'` |
| `fillOpacity` | `fillOpacity`, '`fill-opacity` | `0.5` |

### <a name="extruded-polygon-style-properties"></a>Ekstrüde çokgen tarzı özellikleri

Özellik bir `Polygon` veya bir `MultiPolygon`, ve `height` 0'dan büyük bir değere sahip bir `PolygonExtrusionLayer`özelliğe sahipse, özellik bir .

| Katman seçeneği | Desteklenen özellik adı(lar) | Varsayılan değer |
|--------------|----------------------------|---------------|
| `base` | `base` | `0` |
| `fillColor` | `fillColor`, `fill` | `'#1E90FF'` |
| `height` | `height` | `0` |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [SimpleDataLayer](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.layer.simpledatalayer)

> [!div class="nextstepaction"]
> [SimpleDataLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.simpledatalayeroptions)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Uzamsal verileri okuma ve yazma](spatial-io-read-write-spatial-data.md)

> [!div class="nextstepaction"]
> [OGC harita katmanı ekleme](spatial-io-add-ogc-map-layer.md)

> [!div class="nextstepaction"]
> [WFS hizmetine bağlanma](spatial-io-connect-wfs-service.md)

> [!div class="nextstepaction"]
> [Temel operasyonlardan yararlanın](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)
