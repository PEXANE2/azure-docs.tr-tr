---
title: Haritaya sembol katmanı ekleme | Microsoft Azure haritaları
description: Bu makalede, sembol katmanını kullanarak bir sembolü özelleştirmeyi ve harita üzerinde sembolleri Microsoft Azure Maps web SDK 'sını kullanarak eklemeyi öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b8d131dcc798fb2fe1d4bb650cd5b0a68903381b
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77209707"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Haritaya sembol katmanı ekleme

Bir sembolü bir veri kaynağına bağlayın ve belirli bir noktada bir simge veya metin işlemek için kullanın. 

Sembol katmanları WebGL kullanılarak işlenir. Haritada büyük noktaları koleksiyonunu işlemek için bir sembol katmanı kullanın. HTML işaretleyicisine kıyasla, sembol katmanı haritada çok sayıda nokta verisi işler ve daha iyi performans sağlar. Ancak, sembol katmanı stil için geleneksel CSS ve HTML öğelerini desteklemez.  

> [!TIP]
> Sembol katmanları varsayılan olarak, bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Katmanı yalnızca nokta geometrisi özelliklerinin `filter`, katmanın özelliğini `['==', ['geometry-type'], 'Point']` veya `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` ayarlamak istiyorsanız, MultiPoint özelliklerini de dahil edebilirsiniz.

Haritalar görüntü Sprite Manager, sembol katmanı tarafından kullanılan özel görüntüleri yükler. Aşağıdaki görüntü biçimlerini destekler:

- JPEG
- PNG
- SVG
- BMP
- GIF (animasyon yok)

## <a name="add-a-symbol-layer"></a>Sembol katmanı ekleme

Haritaya bir sembol katmanı ekleyebilmeniz için önce birkaç adım gerçekleştirmeniz gerekir. İlk olarak, bir veri kaynağı oluşturun ve haritaya ekleyin. Bir sembol katmanı oluşturun. Ardından veri kaynağından veri almak için veri kaynağını sembol katmanına geçirin. Son olarak, veri kaynağına veri ekleyerek işlenecek bir şey olması gerekir. 

Aşağıdaki kod, yüklendikten sonra haritaya ne ekleneceğini gösterir. Bu örnek, bir sembol katmanını kullanarak haritada tek bir nokta oluşturur. 

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a symbol layer to render icons and/or text at points on the map.
var layer = new atlas.layer.SymbolLayer(dataSource);

//Add the layer to the map.
map.layers.add(layer);

//Create a point and add it to the data source.
dataSource.add(new atlas.data.Point([0, 0]));
```

Haritaya eklenebilecek dört farklı tür Point verisi vardır:

- GeoJSON nokta geometrisi-bu nesne yalnızca bir noktanın koordinatını içerir ve başka hiçbir şey yapmaz. `atlas.data.Point` yardımcı sınıfı bu nesneleri kolayca oluşturmak için kullanılabilir.
- GeoJSON MultiPoint geometrisi-bu nesne birden çok noktanın koordinatlarını içerir ve başka hiçbir şey yapmaz. `atlas.data.MultiPoint` yardımcı sınıfı bu nesneleri kolayca oluşturmak için kullanılabilir.
- GeoJSON özelliği-bu nesne, tüm GeoJSON geometrisinden ve geometri ile ilişkili meta verileri içeren bir özellik kümesinden oluşur. `atlas.data.Feature` yardımcı sınıfı bu nesneleri kolayca oluşturmak için kullanılabilir.
- `atlas.Shape` sınıfı, GeoJSON özelliğine benzer. Her ikisi de GeoJSON geometrisini ve geometri ile ilişkili meta verileri içeren bir özellikler kümesini içerir. Bir coğrafi JSON nesnesi bir veri kaynağına eklenirse, bir katmanda kolayca oluşturulabilir. Ancak, bu GeoJSON nesnesinin koordinatlar özelliği güncelleştirilirse, veri kaynağı ve eşleme değişmez. Bunun nedeni, bir güncelleştirmeyi tetiklemenin JSON nesnesinde hiçbir mekanizma olmaması olabilir. Şekil sınıfı, içerdiği verileri güncelleştirmek için işlevler sağlar. Bir değişiklik yapıldığında veri kaynağı ve eşleme otomatik olarak bilgilendirilir ve güncelleştirilir. 

Aşağıdaki kod örneği, bir GeoJSON noktası geometrisi oluşturur ve güncelleştirmeyi kolaylaştırmak için `atlas.Shape` sınıfına geçirir. Haritanın merkezi başlangıçta bir sembolü işlemek için kullanılır. Haritada bir tıklama olayı, ne zaman tetiklendiğinde, fare koordinatları şekil `setCoordinates` işleviyle birlikte kullanılır. Fare koordinatları tıklama olayı sırasında kaydedilir. Sonra `setCoordinates`, haritada simgenin konumunu günceller.

<br/>

<iframe height='500' scrolling='no' title='PIN konumunu değiştir' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>anahtar pin konumuna</a> bakın.
</iframe>

> [!TIP]
> Varsayılan olarak, sembol katmanları örtüşen sembolleri gizleyerek sembol işlemesini iyileştirir. Yakınlaştırma sırasında gizli simgeler görünür hale gelir. Bu özelliği devre dışı bırakmak ve tüm sembolleri her zaman işlemek için `iconOptions` seçeneklerinin `allowOverlap` özelliğini `true`olarak ayarlayın.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Sembol katmanına özel simge ekleme

Sembol katmanları WebGL kullanılarak işlenir. Bu nedenle, simge görüntüleri gibi tüm kaynakların WebGL bağlamına yüklenmesi gerekir. Bu örnek, harita kaynaklarına nasıl özel bir simge ekleneceğini gösterir. Bu simge daha sonra haritada özel bir sembol ile nokta verilerini işlemek için kullanılır. Sembol katmanının `textField` özelliği bir ifadenin belirtilmesini gerektirir. Bu durumda, sıcaklık özelliğini işlemek istiyoruz. Sıcaklık bir sayı olduğundan, bir dizeye dönüştürülmesi gerekir. Ayrıca buna "°F" eklemek istiyoruz. Bu birleştirme yapmak için bir ifade kullanılabilir; `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Özel sembol resmi simgesi' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>özel sembol görüntüsü simgesine</a> bakın.
</iframe>

> [!TIP]
> Azure Haritalar Web SDK 'Sı, sembol katmanıyla birlikte kullanabileceğiniz çeşitli özelleştirilebilir görüntü şablonları sağlar. Daha fazla bilgi için bkz. [görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md) belgesi.

## <a name="customize-a-symbol-layer"></a>Sembol katmanını özelleştirme 

Sembol katmanında birçok stil seçeneği mevcuttur. Bu çeşitli stil seçeneklerini test etmek için bir araç aşağıda verilmiştir.

<br/>

<iframe height='700' scrolling='no' title='Sembol katmanı seçenekleri' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/PxVXje/'>sembol katmanı seçeneklerine</a> bakın.
</iframe>

> [!TIP]
> Yalnızca bir sembol katmanıyla birlikte metin işlemek istediğinizde, simge seçeneklerinin `image` özelliğini `'none'`olarak ayarlayarak simgeyi gizleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TextOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Açılan pencere Ekle](map-add-popup.md)

> [!div class="nextstepaction"]
> [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı Ekle](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı Ekle](map-add-shape.md)

> [!div class="nextstepaction"]
> [Kabarcık katmanı ekleme](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [HTML üreticileri ekleme](map-add-bubble-layer.md)
