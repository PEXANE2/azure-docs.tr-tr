---
title: Haritaya ısı haritası katmanı ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak haritaya ısı haritası katmanı ekleme hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 19765bd28f365cc6f6d5b06646896613dd3e3e87
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804563"
---
# <a name="add-a-heat-map-layer"></a>Isı haritası katmanı ekleme

Nokta yoğunluğu haritaları olarak da bilinen ısı haritaları, bir veri görselleştirme türüdür. Bir renk aralığı kullanarak verilerin yoğunluğunu temsil etmek ve haritadaki verileri "etkin noktalar" göstermek için kullanılırlar. Isı haritaları, çok sayıda noktaya sahip veri kümelerini işlemek için harika bir yoldur. 

On binlerce noktayı sembol olarak işlemek harita alanının çoğunu kapsayabilir. Bu durumda büyük olasılıkla birçok sembol örtüşen sonuçlanır. Verilerin daha iyi anlaşılmasını zorlaştırıyor. Ancak, bu aynı veri kümesini Bir ısı haritası olarak görselleştirmek, her veri noktasının yoğunluğunu ve göreli yoğunluğunu görmenizi kolaylaştırır.

Isı haritalarını, şunları dahil olmak üzere birçok farklı senaryoda kullanabilirsiniz:

- **Sıcaklık verileri**: İki veri noktası arasındaki sıcaklığın ne olduğu hakkında yaklaşımlar sağlar.
- **Gürültü sensörleri için veriler**: Sadece sensörün bulunduğu gürültünün yoğunluğunu göstermez, aynı zamanda bir mesafeden dağılma hakkında bilgi de verebilir. Herhangi bir sitedeki gürültü seviyesi yüksek olmayabilir. Birden çok sensörden gelen gürültü kapsama alanı çakışıyorsa, bu çakışan alanın daha yüksek gürültü seviyeleriyle karşılaşabileceği mümkündür. Bu nedenle, örtüşen alan ısı haritasında görünür olacaktır.
- **GPS izleme**: Her veri noktasının yoğunluğunun hızıtemel aldığı ağırlıklı yükseklik haritası olarak hızı içerir. Örneğin, bu işlev, bir aracın nerede hız landığını görmenin bir yolunu sağlar.

> [!TIP]
> Isı haritası katmanları varsayılan olarak bir veri kaynağındaki tüm geometrilerin koordinatlarını işler. Katmanı yalnızca nokta geometrisi özelliklerini işletmesi `filter` için sınırlamak için, `['==', ['geometry-type'], 'Point']`katmanın özelliğini . MultiPoint özelliklerini de eklemek istiyorsanız, katmanın özelliğini `filter` `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]`.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Heat-Maps-and-Image-Overlays-in-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="add-a-heat-map-layer"></a>Isı haritası katmanı ekleme

Bir veri kaynağını bir ısı haritası olarak işlemek için, veri `HeatMapLayer` kaynağınızı sınıfın bir örneğine aktarın ve haritaya ekleyin.

Aşağıdaki kodda, her ısı noktasının tüm yakınlaştırma düzeylerinde 10 piksel yarıçapı vardır. Daha iyi bir kullanıcı deneyimi sağlamak için, ısı haritası etiket katmanının altındadır. Etiketler açıkça görülebilir kalır. Bu örnekteki veriler [USGS Deprem Tehlikeleri Programı'ndan](https://earthquake.usgs.gov/)dır. Bu son 30 gün içinde meydana gelen önemli depremler içindir.

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Load a dataset of points, in this case earthquake data from the USGS.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson');

//Create a heat map and add it to the map.
map.layers.add(new atlas.layer.HeatMapLayer(datasource, null, {
  radius: 10,
  opacity: 0.8
}), 'labels');
```

Burada, önceki kodun tam çalışan kod örneği vereyim.

<br/>

<iframe height='500' scrolling='no' title='Basit Isı Haritası Katmanı' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>Basit Isı Haritası Katmanına</a> bakın .
</iframe>

## <a name="customize-the-heat-map-layer"></a>Isı haritası katmanını özelleştirme

Önceki örnek, yarıçap ve donukluk seçeneklerini ayarlayarak ısı haritasını özelleştirilmiştir. Isı haritası katmanı özelleştirme için çeşitli seçenekler sunar:

* `radius`: Her veri noktasını işlemek için bir piksel yarıçapı tanımlar. Yarıçapı sabit bir sayı veya ifade olarak ayarlayabilirsiniz. Bir ifade kullanarak, yarıçapı yakınlaştırma düzeyine göre ölçeklendirebilir ve haritada tutarlı bir uzamsal alanı (örneğin, 5 mil yarıçapı) temsil edebilirsiniz.
* `color`: Isı haritasının nasıl renklendirilir olduğunu belirtir. Renk degradesi ısı haritalarının yaygın bir özelliğidir. Bir `interpolate` ifade ile etkisi elde edebilirsiniz. Ayrıca, ısı `step` haritasını renklendirmek, yoğunluğu görsel olarak bir kontur veya radar stili haritasına benzeyen aralıklara ayırmak için bir ifade kullanabilirsiniz. Bu renk paletleri renkleri en düşük yoğunluk değerinden maksimum yoğunluk değerine tanımlar. 

  Isı haritaları için renk değerlerini değer `heatmap-density` üzerinde bir ifade olarak belirtirsiniz. Veri olmayan alanın rengi, "Enterpolasyon" ifadesinin dizin 0'ında veya "Adımadım" ifadesinin varsayılan renginde tanımlanır. Bir arka plan rengi tanımlamak için bu değeri kullanabilirsiniz. Genellikle, bu değer saydam veya yarı saydam siyah olarak ayarlanır. 
   
  Renk ifadelerine örnekler aşağıda verilmiştir:

  | Enterpolasyon renk ifadesi | Basamaklı renk ifadesi | 
  |--------------------------------|--------------------------|
  | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['doğrusal'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['ısı haritası yoğunluğu'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'saydam',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'mor',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'adım',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['ısı haritası yoğunluğu'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'şeffaf',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'donanma',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.25, 'yeşil',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.50, 'sarı',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.75, 'kırmızı'<br/>\] |   

- `opacity`: Isı haritası tabakasının ne kadar opak veya saydam olduğunu belirtir.
- `intensity`: Isı haritasının genel yoğunluğunu artırmak için her veri noktasının ağırlığına bir çarpan uygular. Veri noktalarının ağırlığında bir farka neden olur ve görselleştirmeyi kolaylaştırır.
- `weight`: Varsayılan olarak, tüm veri noktalarının ağırlığı 1'dir ve eşit olarak ağırlıklanır. Ağırlık seçeneği çarpan gibi davranır ve bunu bir sayı veya ifade olarak ayarlayabilirsiniz. Bir sayı ağırlık olarak ayarlanırsa, bu, her veri noktasını haritaya iki kez yerleştirmenin eşdeğerliğidir. Örneğin, ağırlık 2 ise, yoğunluk iki katına çıkar. Ağırlık seçeneğini bir sayıya ayarlamak, ısı eşlesini yoğunluk seçeneğini kullanmaya benzer bir şekilde işler. 

  Ancak, bir ifade kullanırsanız, her veri noktasının ağırlığı her veri noktasının özelliklerine dayalı olabilir. Örneğin, her veri noktasının bir depremi temsil diğini varsayalım. Büyüklük değeri her deprem veri noktası için önemli bir metrik olmuştur. Depremler her zaman olur, ama çoğu düşük bir büyüklüğü var, ve fark edilmez. Ağırlığı her veri noktasına atamak için bir ifadedeki büyüklük değerini kullanın. Ağırlığı atamak için büyüklük değerini kullanarak, ısı haritası içinde depremlerin önemini daha iyi bir temsil olsun.
- `source`ve `source-layer`: Veri kaynağını güncelleştirmenizi sağlar.

Burada farklı ısı haritası katmanı seçenekleri test etmek için bir araç.

<br/>

<iframe height='700' scrolling='no' title='Isı Haritası Katman Seçenekleri' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>CodePen'de Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>Isı Haritası Katman Seçenekleri</a> <a href='https://codepen.io'>'ne</a>bakın .
</iframe>

## <a name="consistent-zoomable-heat-map"></a>Tutarlı yakınlaştırılabilir ısı haritası

Varsayılan olarak, ısı haritası katmanında işlenen veri noktalarının yarıçapı, tüm yakınlaştırma düzeyleri için sabit bir piksel yarıçapına sahiptir. Haritayı yakınlaştırdıkça, veriler bir araya toplanır ve ısı haritası katmanı farklı görünür. 

Her `zoom` zum düzeyi için yarıçapı ölçeklendirmek için bir ifade kullanın, böylece her veri noktası haritanın aynı fiziksel alanını kaplar. Bu ifade, ısı haritası katmanının daha statik ve tutarlı görünmesini sağlar. Haritanın her yakınlaştırma düzeyi, önceki yakınlaştırma düzeyinin iki katı dikey ve yatay piksele sahiptir. 

Yarıçapı, her yakınlaştırma düzeyiyle iki katına çıkaracak şekilde ölçekleme, tüm yakınlaştırma düzeylerinde tutarlı görünen bir ısı haritası oluşturur. Bu ölçeklendirmeyi uygulamak `zoom` için, piksel `exponential interpolation` yarıçapı minimum yakınlaştırma düzeyi için ayarlanmış bir taban 2 ifadesi ve `2 * Math.pow(2, minZoom - maxZoom)` aşağıdaki örnekte gösterildiği gibi hesaplanan maksimum yakınlaştırma düzeyi için ölçeklenmiş yarıçap ile kullanın. Isı haritasının yakınlaştırma düzeyiyle nasıl ölçeklendirildirini görmek için haritayı yakınlaştırın.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Tutarlı yakınlaştırılabilir ısı haritası" src="//codepen.io/azuremaps/embed/OGyMZr/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/OGyMZr/'>Tutarlı yakınlaştırılabilir ısı haritasına</a> bakın .
</iframe>

> [!TIP]
> Veri kaynağında kümelenmeyi etkinleştirdiğinizde, birbirine yakın noktalar kümelenmiş nokta olarak gruplandırılır. Isı haritasının ağırlık ifadesi olarak her kümenin nokta sayısını kullanabilirsiniz. Bu, oluşturulacak puan sayısını önemli ölçüde azaltabilir. Bir kümenin nokta sayısı nokta `point_count` özelliğinin bir özelliğinde depolanır: 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Kümeleme yarıçapı yalnızca birkaç pikselise, işlemede küçük bir görsel fark olacaktır. Daha büyük bir yarıçap her kümeye daha fazla nokta ayırır ve ısı haritasının performansını artırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [HeatMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerSeçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Haritalarınıza ekleyeceğiniz daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Veri kaynağı oluşturma](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)
