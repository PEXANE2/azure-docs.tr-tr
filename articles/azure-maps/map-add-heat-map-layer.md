---
title: Azure haritalar 'a bir ısı haritası katmanı ekleme | Microsoft Docs
description: Azure Maps web SDK 'sına bir ısı haritası katmanı ekleme.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b0d07f18682bf39558180753de38a9c5ff106ee3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75408530"
---
# <a name="add-a-heat-map-layer"></a>Isı haritası katmanı ekleme

Nokta yoğunluğu haritaları olarak da bilinen ısı haritaları, bir dizi renk kullanarak verilerin yoğunluğunu temsil etmek için kullanılan bir veri görselleştirmesi türüdür. Genellikle bir haritada "etkin noktalar" verilerini göstermek için kullanılır ve büyük nokta veri kümelerini işlemek için harika bir yoldur. 

Örneğin, harita görünümü içindeki on binlerce noktayı, harita alanının çoğunu simgeler olarak kaplayan bir şekilde işleme. Bu, büyük olasılıkla birbirleriyle çakışan birçok sembol ile sonuçlanır, böylece veriler hakkında daha ayrıntılı bilgi elde etmek zordur. Bununla birlikte, bir ısı haritası olarak aynı veri kümesini görselleştirmede, nokta verilerinin nersest olduğunu ve diğer alanlara göreli yoğunluğu görmenizi kolaylaştırır.

Isı haritalarını birçok farklı senaryoda kullanabilirsiniz; örneğin:

- **Sıcaklık verileri**: sıcaklığın iki veri noktası arasında olduğu yaklaşık bir değer sağlar.
- **Gürültü sensörlerine yönelik veriler**: yalnızca sensörün olduğu gürültü şiddette değil, ancak aynı zamanda bir mesafe üzerinde Öngörüler elde edebilir. Herhangi bir sitedeki gürültü düzeyi yüksek olmayabilir. Ancak, birden çok sensörden gürültü kapsam alanı çakışırsa, bu örtüşen alanın daha yüksek gürültü düzeylerine sahip olması ve bu nedenle ısı haritasında görünebilmesini mümkün olabilir.
- **GPS izleme**: her bir veri noktasının yoğunluğu hızlı temel alınarak ağırlıklı bir yükseklik eşlemesi olarak hızı içerir. Örneğin, bu, bir aracın hızlandırmakta olduğunu görmek için bir yol sağlar.

> [!TIP]
> Isı haritası katmanları varsayılan olarak bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Katmanı yalnızca nokta geometrisi özelliklerini gösterecek şekilde sınırlamak için katmanın `filter` özelliğini `['==', ['geometry-type'], 'Point']`olarak ayarlayın. MultiPoint özelliklerini de dahil etmek istiyorsanız, katmanın `filter` özelliğini `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]`olarak ayarlayın.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Heat-Maps-and-Image-Overlays-in-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="add-a-heat-map-layer"></a>Isı haritası katmanı ekleme

Bir veri kaynağını bir ısı haritası olarak işlemek için, veri kaynağınızı `HeatMapLayer` sınıfının bir örneğine geçirin ve Haritayı haritaya ekleyin.

Aşağıdaki kodda, her ısı noktasının tüm yakınlaştırma düzeylerinde 10 piksellik bir yarıçapı vardır. Bir kullanıcıya ısı haritası katmanını eklediğinizde, daha iyi bir kullanıcı deneyimi oluşturmak için bu örnek etiket katmanının altına ekler. Etiketler, ısı haritasının üzerinde açıkça görülebilir. Bu örnekteki veriler, [USGS deprem hazı programından](https://earthquake.usgs.gov/)kaynaklıdır ve son 30 gün içinde gerçekleşen önemli deprem 'yi temsil eder.

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Load a dataset of points, in this case earthquake data from the USGS.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson');

//Create a heatmap and add it to the map.
map.layers.add(new atlas.layer.HeatMapLayer(datasource, null, {
  radius: 10,
  opacity: 0.8
}), 'labels');
```

Önceki kodun çalışan kod örneğinin tamamı aşağıda verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Basit ısı haritası katmanı' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile Ilgili kalem <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>basit ısı haritası katmanını</a> inceleyin.
</iframe>

## <a name="customize-the-heat-map-layer"></a>Isı haritası katmanını özelleştirme

Önceki örnek, yarıçap ve opaklık seçeneklerini ayarlayarak ısı haritasını özelleştirdi. Isı haritası katmanı, aşağıdakiler dahil olmak üzere çeşitli özelleştirme seçenekleri sunar:

* `radius`: her bir veri noktasının işlenmesi için bir piksel yarıçapı tanımlar. Yarıçapı sabit bir sayı veya bir ifade olarak ayarlayabilirsiniz. Bir ifade kullanarak, önizleme düzeyine göre yarıçapı ölçeklendirebilir ve haritadaki tutarlı bir uzamsal alanı (örneğin, 5-mil yarıçap) temsil edebilirsiniz.
* `color`: ısı haritasının renk oluşturma şeklini belirtir. Renk gradyanı, bir ısı haritaları yaygın bir özelliğidir ve bir `interpolate` ifadesiyle efekti elde edebilirsiniz. Ayrıca, ısı haritasını renklendirime için bir `step` ifadesi kullanabilirsiniz, yoğunluğu bir dağılım veya radar stil eşlemesine benzer aralıklarda görsel olarak ortadan kaldırabilirsiniz. Bu renk paletleri, minimum değeri en yüksek yoğunluk değerine göre tanımlar. 

  Isı haritaları için renk değerlerini `heatmap-density` değerinde bir ifade olarak belirtirsiniz. Bir ilişkilendirme ifadesinde dizin 0 ' daki veya bir adım ifadesinin varsayılan rengi olan renk, veri bulunmayan alanın rengini tanımlar. Bu, bir arka plan rengi tanımlamak için kullanabilirsiniz. Genellikle, bu değer saydam veya yarı saydam bir siyah olarak ayarlanır. 
   
  Renk ifadelerinin örnekleri aşağıda verilmiştir:

  | Enterpolasyon rengi ifadesi | Basamaklı renk ifadesi | 
  |--------------------------------|--------------------------|
  | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;' enterpolageç ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\[' doğrusal '\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\[' heatmap-yoğunluklu '\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, ' Transparent ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, ' mor ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,5, ' #fb00fb ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, ' #00c3ff '<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;' Step ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\[' heatmap-yoğunluklu '\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;' Transparent '<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, ' Navy ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,25, ' yeşil ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,50, ' sarı ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,75, ' Red '<br/>\] |   

- `opacity`: ısı haritası katmanının donuk veya saydam olduğunu belirtir.
- `intensity`: heatmap 'in genel yoğunluğunu artırmak için her bir veri noktasının ağırlığına bir çarpan uygular. Bu, veri noktalarının ağırlığının daha kolay görselleştirilebilmesini sağlamaya yardımcı olur.
- `weight`: varsayılan olarak, tüm veri noktalarında 1 ağırlığı vardır ve eşit olarak ağırlıklı şekilde tanımlanır. Ağırlık seçeneği bir çarpan olarak davranır ve bunu sayı veya bir ifade olarak ayarlayabilirsiniz. Ağırlık olarak bir sayı (örneğin, 2) ayarlandıysa, her bir veri noktasının haritaya iki kez yerleştirilmesi eşdeğerdir. bu nedenle, yoğunluğu ikiye katlandırın. Weight seçeneğini bir sayı olarak ayarlamak, ısı haritasını, yoğunluk seçeneğini kullanmak için benzer bir şekilde işler. 

  Ancak, bir ifade kullanırsanız her bir veri noktasının ağırlığı her bir veri noktasının özelliklerine bağlı olabilir. Örneğin, her bir veri noktasının bir deprem temsil ettiğini varsayalım. Her deprem veri noktasının bir büyüklük değeri, önemli bir ölçümdür. Deprem her zaman gerçekleşir, ancak en az büyüklüğü düşüktür ve hatta keçeli değildir. Her bir veri noktasına ağırlığı atamak için bir ifadede büyüklük değerini kullanarak, ısı haritası içinde deprem 'in önemini daha iyi bir şekilde temsil edersiniz.
- `source` ve `source-layer`: veri kaynağını güncelleştirmenizi sağlar.

Farklı ısı haritası katman seçeneklerini test etmek için bir araç aşağıda verilmiştir.

<br/>

<iframe height='700' scrolling='no' title='Isı haritası katman seçenekleri' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile Ilgili kalem <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>ısı haritası katman seçeneklerine</a> bakın.
</iframe>

## <a name="consistent-zoomable-heat-map"></a>Tutarlı Zoomable ısı haritası

Varsayılan olarak, ısı haritası katmanında işlenen veri noktalarının yarıçapının tüm yakınlaştırma düzeyleri için sabit bir piksel yarıçapı vardır. Haritayı yakınlaştırdığınızda, veriler birlikte toplar ve ısı haritası katmanı farklı görünür. 

Her bir yakınlaştırma düzeyi için yarıçapı ölçeklendirmek üzere bir `zoom` ifadesi kullanın, örneğin her bir veri noktası haritanın aynı fiziksel alanını içerir. Bu, ısı haritası katmanının daha statik ve tutarlı görünmesini sağlar. Haritanın her yakınlaştırma düzeyinin, önceki yakınlaştırma düzeyiyle dikey ve yatay olarak iki piksel daha vardır. 

Her yakınlaştırma düzeyiyle çift olacak şekilde yarıçap ölçekleme, tüm yakınlaştırma düzeylerinde tutarlı görünen bir ısı haritası oluşturur. Bunu yapmak için, aşağıdaki örnekte gösterildiği gibi, temel 2 `exponential interpolation` ifadesiyle `zoom` kullanın. Isı haritasının yakınlaştırma düzeyiyle nasıl ölçeklendirçalıştığını görmek için haritayı yakınlaştırın.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Tutarlı Zoomable ısı haritası" src="//codepen.io/azuremaps/embed/OGyMZr/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalemle <a href='https://codepen.io/azuremaps/pen/OGyMZr/'>tutarlı Zoomable ısı haritasını</a> inceleyin.
</iframe>

> [!TIP]
> Veri kaynağında kümelendirmeyi etkinleştirdiğinizde, birbirine yakın olan noktaları kümelenmiş bir nokta olarak birlikte gruplandırılır. Her kümenin nokta sayısını, ısı haritası için ağırlık ifadesi olarak kullanabilir ve işlenmesi gereken nokta sayısını önemli ölçüde azaltabilirsiniz. Bir kümenin nokta sayısı, nokta özelliğinin `point_count` özelliğinde depolanır: 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Kümeleme yarıçapı yalnızca birkaç pikselden azsa, işleme göre çok önemli bir farklılık vardır. Daha büyük bir yarıçap, her kümeye daha fazla işaret sağlar ve heatmap 'in performansını geliştirir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Heatmaoyuncu](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza eklenecek daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Veri tabanlı stil ifadeleri kullanın](data-driven-style-expressions-web-sdk.md)
