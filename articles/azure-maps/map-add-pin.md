---
title: Haritaya Sembol katmanı ekleme | Microsoft Azure Haritaları
description: Bu makalede, bir sembolü özelleştirmek ve Microsoft Azure Haritalar Web SDK'sını kullanarak haritaya sembol katmanını nasıl kullanacağınızı öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b8d131dcc798fb2fe1d4bb650cd5b0a68903381b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77209707"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Haritaya sembol katmanı ekleme

Bir simgeyi bir veri kaynağına bağlayın ve belirli bir noktada bir simge veya metin işlemek için kullanın. 

Sembol katmanları WebGL kullanılarak işlenir. Haritada büyük puan koleksiyonlarını işlemek için bir sembol katmanı kullanın. HTML işaretçisiyle karşılaştırıldığında, sembol katmanı haritada çok sayıda nokta verisi işler ve daha iyi performans gösterir. Ancak, sembol katmanı stil için geleneksel CSS ve HTML öğelerini desteklemez.  

> [!TIP]
> Sembol katmanları varsayılan olarak bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Yalnızca nokta geometrisi özelliklerini işleyen katmanı sınırlamak `filter` için `['==', ['geometry-type'], 'Point']` katmanın `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` özelliğini ayarlayabilir veya isterseniz, MultiPoint özelliklerini de ekleyebilirsiniz.

Haritalar görüntü sprite yöneticisi sembol katmanı tarafından kullanılan özel görüntüler yükler. Aşağıdaki görüntü biçimlerini destekler:

- JPEG
- PNG
- Svg
- BMP
- GIF (animasyon yok)

## <a name="add-a-symbol-layer"></a>Sembol katmanı ekleme

Haritaya bir sembol katmanı eklemeden önce birkaç adım atmanız gerekir. İlk olarak, bir veri kaynağı oluşturun ve haritaya ekleyin. Bir sembol katmanı oluşturun. Ardından, verileri veri kaynağından almak için veri kaynağını sembol katmanına geçirin. Son olarak, veri kaynağına veri ekleyin, böylece işlenecek bir şey vardır. 

Aşağıdaki kod, yüklendikten sonra haritaya ne eklenmesi gerektiğini gösterir. Bu örnek, bir sembol katmanı kullanarak haritaüzerinde tek bir nokta işler. 

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

Haritaya eklenebilecek dört farklı nokta veritürü vardır:

- GeoJSON Point geometrisi - Bu nesne sadece bir noktanın koordinatını içerir ve başka bir şey içermez. `atlas.data.Point` Yardımcı sınıf kolayca bu nesneleri oluşturmak için kullanılabilir.
- GeoJSON MultiPoint geometrisi - Bu nesne birden fazla noktanın koordinatlarını içerir ve başka bir şey değil. `atlas.data.MultiPoint` Yardımcı sınıf kolayca bu nesneleri oluşturmak için kullanılabilir.
- GeoJSON Özelliği - Bu nesne herhangi bir GeoJSON geometrisi ve geometriile ilişkili meta veriler içeren bir dizi özellikten oluşur. `atlas.data.Feature` Yardımcı sınıf kolayca bu nesneleri oluşturmak için kullanılabilir.
- `atlas.Shape`sınıf GeoJSON özelliğine benzer. Her ikisi de GeoJSON geometrisi ve geometriyle ilişkili meta veriler içeren bir dizi özellikten oluşur. Bir GeoJSON nesnesi bir veri kaynağına eklenirse, katmanda kolayca işlenebilir. Ancak, bu GeoJSON nesnesinin koordinat özelliği güncelleştirilirse, veri kaynağı ve harita değişmez. Çünkü JSON nesnesinde bir güncelleştirmeyi tetikleyen bir mekanizma yok. Şekil sınıfı, içerdiği verileri güncelleştirmek için işlevler sağlar. Bir değişiklik yapıldığında, veri kaynağı ve harita otomatik olarak bildirilir ve güncelleştirilir. 

Aşağıdaki kod örneği bir GeoJSON Point geometrisi `atlas.Shape` oluşturur ve güncelleştirmeyi kolaylaştırmak için sınıfa aktarın. Haritanın merkezi başlangıçta bir sembol işlemek için kullanılır. Eşmeye bir tıklama olayı eklenir, çünkü ateşlendiğinde farenin koordinatları şekiller `setCoordinates` işleviyle birlikte kullanılır. Fare koordinatları tıklama olayı sırasında kaydedilir. Daha sonra, haritadaki sembolün konumunu `setCoordinates` güncelleştirir.

<br/>

<iframe height='500' scrolling='no' title='Pin konumunu değiştirme' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a göre Kalem <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>Anahtarı pin konumuna</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

> [!TIP]
> Varsayılan olarak, sembol katmanları çakışan sembolleri gizleyerek sembollerin işlenmesini en iyi duruma getirir. Yakınlaştırdıkça, gizli semboller görünür hale gelir. Bu özelliği devre dışı bırakıp tüm sembolleri her `allowOverlap` zaman `iconOptions` işlemek `true`için seçeneklerin özelliğini .

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Sembol katmanına özel simge ekleme

Sembol katmanları WebGL kullanılarak işlenir. Bu nedenle simge görüntüleri gibi tüm kaynaklar WebGL bağlamına yüklenmelidir. Bu örnek, harita kaynaklarına özel bir simgenin nasıl ekleyeceğini gösterir. Bu simge daha sonra haritaüzerinde özel bir simge ile nokta verilerini işlemek için kullanılır. Sembol `textField` katmanının özelliği nin belirtilmesi için bir ifade gerekir. Bu durumda, sıcaklık özelliğini işlemek istiyoruz. Sıcaklık bir sayı olduğundan, bir dize dönüştürülmesi gerekir. Ayrıca biz buna "°F" eklemek istiyorum. Bir ifade bu concatenation yapmak için kullanılabilir; `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Özel Sembol Resim Simgesi' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Özel Simge Resim <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>Simgesine</a> bakın .
</iframe>

> [!TIP]
> Azure Haritalar web SDK, sembol katmanıyla kullanabileceğiniz birkaç özelleştirilebilir resim şablonu sağlar. Daha fazla bilgi için resim şablonları belgesini [nasıl kullanılır'](how-to-use-image-templates-web-sdk.md) a bakın.

## <a name="customize-a-symbol-layer"></a>Sembol katmanını özelleştirme 

Sembol katmanı birçok stil seçeneği vardır. Burada bu çeşitli stil seçenekleri test etmek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='Sembol Katmanı Seçenekleri' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>CodePen'de Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Sembolü Katman <a href='https://codepen.io/azuremaps/pen/PxVXje/'>Seçenekleri</a> <a href='https://codepen.io'>'ne</a>bakın .
</iframe>

> [!TIP]
> Yalnızca bir sembol katmanı yla metni işlemek istediğinizde, simge `image` seçeneklerinin özelliğini `'none'`' e ayarlayarak simgeyi gizleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Sembol Katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Simge Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Textoptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Açılır pencere ekleme](map-add-popup.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](map-add-shape.md)

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [HTML Üreticileri Ekle](map-add-bubble-layer.md)
