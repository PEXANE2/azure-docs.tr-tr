---
title: Haritaya çizgi katmanı ekleme | Microsoft Azure haritaları
description: Haritalara satır eklemeyi öğrenin. Haritalara çizgi katmanları eklemek ve semboller ve renk degradeleriyle satırları özelleştirmek için Azure Maps web SDK 'sını kullanan örneklere bakın.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-javascript
ms.openlocfilehash: 11783ceae1b2a2f9782df74976d8ea6d66573a1c
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90086037"
---
# <a name="add-a-line-layer-to-the-map"></a>Haritaya bir çizgi katmanı ekleyin

Bir çizgi katmanı `LineString` `MultiLineString` , haritada yollar veya rotalar olarak işlemek ve özellikleri oluşturmak için kullanılabilir. Bir çizgi katmanı, ve özelliklerinin anahatlarını işlemek için de kullanılabilir `Polygon` `MultiPolygon` . Veri kaynağı, işlenecek verileri sağlamak için bir çizgi katmanına bağlanır. 

> [!TIP]
> Çizgi katmanları, varsayılan olarak çokgenler koordinatlarını ve bir veri kaynağındaki satırları de işler. Katmanı yalnızca, LineString özelliklerinin, `filter` katman özelliğini olarak ayarlayın `['==', ['geometry-type'], 'LineString']` veya `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]` çok kanallı dize özelliklerini de dahil etmek istiyorsanız, bu katmanı sınırlamak için.

Aşağıdaki kod, bir satırın nasıl oluşturulacağını gösterir. Satırı bir veri kaynağına ekleyin ve ardından [linelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer) sınıfını kullanarak bir çizgi katmanı ile işleme koyun.

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

Aşağıda, yukarıdaki işlevselliğin tamamen çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Haritaya çizgi ekleme' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/qomaKv/'>bir haritaya çizgi ekleme</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

Çizgi katmanları, [Linelayeroptions](/javascript/api/azure-maps-control/atlas.linelayeroptions) kullanılarak biçimlendirilebilir ve [veri odaklı stil ifadeleri kullanabilir](data-driven-style-expressions-web-sdk.md).

## <a name="add-symbols-along-a-line"></a>Çizgi üzerinde semboller ekleme

Bu örnek, haritada bir çizgi üzerinde ok simgelerinin nasıl ekleneceğini gösterir. Sembol katmanını kullanırken, "yerleştirme" seçeneğini "Line" olarak ayarlayın. Bu seçenek, simgeleri satır üzerinde işler ve simgeleri döndürür (0 derece = sağ).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Oku çizgi üzerinde göster" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder='no' loading="lazy" loading="lazy" allowtransparency="true" allowfullscreen="true">
Codepen üzerinde Azure Maps () ile birlikte, kalemin <a href='https://codepen.io/azuremaps/pen/drBJwX/'>göster okuna</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

> [!TIP]
> Azure Haritalar Web SDK 'Sı, sembol katmanıyla birlikte kullanabileceğiniz çeşitli özelleştirilebilir görüntü şablonları sağlar. Daha fazla bilgi için bkz. [görüntü şablonlarını kullanma](how-to-use-image-templates-web-sdk.md) belgesi.

<a name="line-stroke-gradient"></a>

## <a name="add-a-stroke-gradient-to-a-line"></a>Çizgiye vuruş gradyanı ekleme

Bir çizgiye tek bir kontur rengi uygulayabilirsiniz. Ayrıca, bir çizgi segmentinden sonraki satır segmentine geçişi göstermek için bir renkler degradeyle bir çizgi doldurabilirsiniz. Örneğin, çizgi degradeleri zaman ve mesafe üzerinde yapılan değişiklikleri veya bağlı bir nesne satırındaki farklı sıcaklıkları temsil etmek için kullanılabilir. Bu özelliği bir satıra uygulamak için veri kaynağı `lineMetrics` seçeneğinin true olarak ayarlanmış olması gerekir ve ardından çizgi seçeneğine bir renk gradyanı ifadesi geçirilebilir `strokeColor` . Vuruş gradyanı ifadesi, `['line-progress']` hesaplanan satır ölçümlerini ifade eden veri ifadesine başvurmalıdır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Kontur degradesi olan çizgi" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder='no' loading="lazy" loading="lazy" allowtransparency="true" allowfullscreen="true">
Codepen 'da Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>vuruş degradesi Ile kalem hattına</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="customize-a-line-layer"></a>Çizgi katmanını özelleştirme

Çizgi katmanının çeşitli stil seçenekleri vardır. İşte deneyebileceğiniz bir araç.

<br/>

<iframe height='700' scrolling='no' title='Çizgi katmanı seçenekleri' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>çizgisi katmanı seçeneklerine</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer) 

> [!div class="nextstepaction"]
> [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions)

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
