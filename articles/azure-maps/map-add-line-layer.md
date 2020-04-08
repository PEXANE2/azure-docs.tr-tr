---
title: Haritaya çizgi katmanı ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak haritaya satır katmanı eklemeyi öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c473be25907eb3a761fbccd598bb9b732e5be5b9
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80802357"
---
# <a name="add-a-line-layer-to-the-map"></a>Eşmeye çizgi katmanı ekleme

Bir çizgi katmanı, haritaüzerinde `MultiLineString` yol veya yol olarak işlemek ve özellikler yapmak `LineString` için kullanılabilir. Bir çizgi katmanı da anahat `Polygon` ve `MultiPolygon` özellikleri işlemek için kullanılabilir. Veri kaynağı, işletilene kadar veri sağlamak için bir satır katmanına bağlanır. 

> [!TIP]
> Satır katmanları varsayılan olarak çokgenlerin koordinatlarını ve bir veri kaynağındaki çizgileri işler. Yalnızca LineString özelliklerini işleyen katmanı sınırlamak için, `filter` katmanın özelliğini `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]` ayarlamak veya `['==', ['geometry-type'], 'LineString']` MultiLineString özelliklerini de eklemek istiyorsanız.

Aşağıdaki kod, bir satırın nasıl oluşturulurolduğunu gösterir. Satırı bir veri kaynağına ekleyin ve [linelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) sınıfını kullanarak bir satır katmanıyla işleyin.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a line and add it to the data source.
dataSource.add(new atlas.data.LineString([[-73.972340, 40.743270], [-74.004420, 40.756800]]));
  
//Create a line layer to render the line to the map.
map.layers.add(new atlas.layer.LineLayer(dataSource, null, {
    strokeColor: 'blue',
    strokeWidth: 5
}));
```

Aşağıda yukarıdaki işlevselliğin tam çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Haritaya satır ekleme' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Kalem <a href='https://codepen.io'>CodePen'de</a>Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a>() ile <a href='https://codepen.io/azuremaps/pen/qomaKv/'>haritaya satır ekleme</a> .
</iframe>

Satır katmanları [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest) ve [Veri odaklı stil ifadeleri](data-driven-style-expressions-web-sdk.md)kullanarak stilolabilir.

## <a name="add-symbols-along-a-line"></a>Satır boyunca semboller ekleme

Bu örnek, haritadaki bir çizgi boyunca ok simgelerinin nasıl ekleyeceğini gösterir. Bir sembol katmanı kullanırken, "yerleşim" seçeneğini "satır" olarak ayarlayın. Bu seçenek, sembolleri satır boyunca işleyecek ve simgeleri döndürür (0 derece = sağa).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizgi boyunca oku göster" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile birlikte Kalem <a href='https://codepen.io/azuremaps/pen/drBJwX/'>Göster oku'na</a> bakın.
</iframe>

> [!TIP]
> Azure Haritalar web SDK, sembol katmanıyla kullanabileceğiniz birkaç özelleştirilebilir resim şablonu sağlar. Daha fazla bilgi için resim şablonları belgesini [nasıl kullanılır'](how-to-use-image-templates-web-sdk.md) a bakın.

<a name="line-stroke-gradient"></a>

## <a name="add-a-stroke-gradient-to-a-line"></a>Satıra kontur degradesi ekleme

Bir çizgiye tek bir kontur rengi uygulayabilirsiniz. Bir satır segmentinden sonraki satır kesimine geçişi göstermek için bir satırı renk degradesiyle de doldurabilirsiniz. Örneğin, satır degradeleri zaman ve mesafe deki değişiklikleri veya bağlı nesneler satırındaki farklı sıcaklıkları temsil etmek için kullanılabilir. Bu özelliği bir satıra uygulamak için, veri `lineMetrics` kaynağının doğru olarak ayarlanmış seçeneği olması gerekir ve `strokeColor` ardından bir renk gradyan ifadesi satır ın seçeneğine geçirilebilir. Kontur gradyan ifadesi, `['line-progress']` hesaplanan satır ölçümlerini ifadeye gösteren veri ifadesine başvurmak zorundadır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Kontur Gradyan lı Çizgi" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>Kontur Gradienti ile</a> Kalem Satırına bakın ( )
</iframe>

## <a name="customize-a-line-layer"></a>Satır katmanını özelleştirme

Çizgi katmanının çeşitli stil seçenekleri vardır. Burada bunları denemek için bir araçtır.

<br/>

<iframe height='700' scrolling='no' title='Çizgi Katman Seçenekleri' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>CodePen'de Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Çizgisi Katman <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>Seçenekleri</a> <a href='https://codepen.io'>'ne</a>bakın .
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Çizgi Katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) 

> [!div class="nextstepaction"]
> [LineLayer Seçenekleri](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)

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
> [Çokgen katmanı ekleme](map-add-shape.md)
