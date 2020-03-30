---
title: Haritaya döşeme katmanı ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak haritaüzerinde bir döşeme Katmanı'nı nasıl yerleştirirsiniz öğreneceksiniz. Döşeme katmanları, görüntüleri harita üzerinde oluşturmanıza olanak sağlar.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 61d7a11df499e6b740adb45968721b6a9bb1af22
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76988609"
---
# <a name="add-a-tile-layer-to-a-map"></a>Haritaya kutucuk katmanı ekleme

Bu makalede, haritaüzerinde bir Kiremit katmanı nın nasıl yerle bir edilebildiğini gösterir. Kutucuk katmanları, Görüntüleri Azure Haritalar temel harita kutucuklarının üzerine eklemenize olanak tanır. Azure Haritalar döşeme sistemi hakkında daha fazla bilgi için [Yakınlaştırma düzeyleri ve döşeme ızgarası'na](zoom-levels-and-tile-grid.md)bakın.

Bir Kiremit katmanı, bir sunucudan kutucuklarda yüklenir. Bu görüntüler önceden işlenmiş veya dinamik olarak işlenebilir. Önceden işlenmiş görüntüler, döşeme katmanının anladığı bir adlandırma kuralı kullanılarak sunucudaki diğer görüntüler gibi depolanır. Dinamik olarak işlenen görüntüler, görüntüleri gerçek zamanlıya yakın yüklemek için bir hizmet kullanır. Azure Haritalar [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) sınıfı tarafından desteklenen üç farklı döşeme hizmeti adlandırma kuralı vardır: 

* X, Y, Zoom gösterimi - X sütundur, Y döşeme ızgarasındaki döşemenin satır konumudur ve Yakınlaştırma gösterimi yakınlaştırma düzeyine dayalı bir değerdir.
* Dört tuşlu gösterim - X, y ve yakınlaştırma bilgilerini tek bir dize değerinde birleştirir. Bu dize değeri, tek bir döşeme için benzersiz bir tanımlayıcı olur.
* Sınırlayıcı Kutu - Bağlama kutusu koordinat biçiminde bir `{west},{south},{east},{north}`resim belirtin: . Bu biçim genellikle [Web Eşleme Hizmetleri (WMS)](https://www.opengeospatial.org/standards/wms)tarafından kullanılır.

> [!TIP]
> [Bir TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) harita üzerinde büyük veri kümeleri görselleştirmek için harika bir yoldur. Bir görüntüden yalnızca bir döşeme katmanı oluşturulamaz, vektör verileri de bir döşeme katmanı olarak işlenebilir. Vektör verilerini bir döşeme katmanı olarak işleyerek, harita denetiminin yalnızca dosya boyutunda temsil ettikleri vektör verilerinden daha küçük olan kutucukları yüklemesi gerekir. Bu teknik genellikle haritaüzerinde milyonlarca veri satırı işlemek için kullanılır.

Döşeme katmanına geçirilen döşeme URL'si, bir TileJSON kaynağının http veya https URL'si veya aşağıdaki parametreleri kullanan bir döşeme URL şablonu olmalıdır: 

* `{x}`- Kiremitin X konumu. Ayrıca `{y}` ihtiyaçları `{z}`ve .
* `{y}`- Kiremitin Y konumu. Ayrıca `{x}` ihtiyaçları `{z}`ve .
* `{z}`- Döşemenin yakınlaştırma seviyesi. Ayrıca `{x}` ihtiyaçları `{y}`ve .
* `{quadkey}`- Bing Maps döşeme sistemi adlandırma kuralına dayalı döşeme dörtanahtar tanımlayıcısı.
* `{bbox-epsg-3857}`- EPSG 3857 `{west},{south},{east},{north}` Mekansal Referans Sistemi formatında bir sınırlayıcı kutu dizesi.
* `{subdomain}`- Alt etki alanı değerleri için bir `subdomain` yer tutucu, belirtilirse eklenecektir.

## <a name="add-a-tile-layer"></a>Kutucuk katmanı ekleme

 Bu örnek, bir kutucuk kümesini işaret eden bir döşeme katmanının nasıl oluşturulutur gösteriş gösterir. Bu örnekx, y, zoom fayans sistemini kullanır. Bu kiremit tabakasının kaynağı [Iowa Devlet Üniversitesi Iowa Çevre Mesonet](https://mesonet.agron.iastate.edu/ogc/)bir hava radar bindirme olduğunu. Radar verilerini görüntülerken, ideal olarak kullanıcılar haritada gezinirken şehirlerin etiketlerini açıkça görürler. Bu davranış, `labels` katmanın altına döşeme katmanı ekleyerek uygulanabilir.

```javascript
//Create a tile layer and add it to the map below the label layer.
//Weather radar tiles from Iowa Environmental Mesonet of Iowa State University.
map.layers.add(new atlas.layer.TileLayer({
    tileUrl: 'https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png',
    opacity: 0.8,
    tileSize: 256
}), 'labels');
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='X, Y ve Z kullanan Kiremit Tabakası' src='//codepen.io/azuremaps/embed/BGEQjG/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre <a href='https://codepen.io/azuremaps/pen/BGEQjG/'>X, Y ve Z kullanarak</a> Kalem Döşeme Katmanı'na bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

## <a name="customize-a-tile-layer"></a>Döşeme katmanını özelleştirme

Döşeme katmanı sınıfıbirçok stil seçeneğine sahiptir. Burada bunları denemek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='Çini Katman Seçenekleri' src='//codepen.io/azuremaps/embed/xQeRWX/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Döşeme Katmanı <a href='https://codepen.io/azuremaps/pen/xQeRWX/'>Seçeneklerine</a> bakın .
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Görüntü katmanı ekleme](./map-add-image-layer.md)
