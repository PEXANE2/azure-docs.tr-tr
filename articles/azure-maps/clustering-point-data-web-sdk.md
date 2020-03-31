---
title: Nokta verilerini haritada kümeleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak veri kümeleme ve harita üzerinde nasıl işleneceksiniz öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: codepen
ms.openlocfilehash: e65681aefc047ba540d4ad0d91ef6e4d2af5f3ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190272"
---
# <a name="clustering-point-data"></a>Nokta verilerini kümeleme

Haritada birçok veri noktası görselleştirildiğinde, veri noktaları birbiriyle çakışabilir. Çakışma, haritanın okunamayan ve kullanımı zor hale gelmesine neden olabilir. Nokta verilerini kümeleme, birbirine yakın olan nokta verilerini birleştirme ve bunları haritada tek bir kümelenmiş veri noktası olarak temsil etme işlemidir. Kullanıcı haritaya yakınlaştıkça, kümeler kendi veri noktalarına ayrılır. Çok sayıda veri noktasıyla çalışırken, kullanıcı deneyiminizi geliştirmek için kümeleme işlemlerini kullanın.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Clustering-point-data-in-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="enabling-clustering-on-a-data-source"></a>Veri kaynağında kümeleme yi etkinleştirme

Seçeneği doğru olarak `DataSource` ayarlayarak sınıfta kümelemayı etkinleştirin. `cluster` Yakındaki `ClusterRadius` noktaları seçecek şekilde ayarlayın ve bunları bir kümede birleştirir. Değeri piksel `ClusterRadius` olarak. Kümeleme mantığını devre dışı bırakan bir yakınlaştırma düzeyi belirtmek için kullanın. `clusterMaxZoom` Aşağıda, bir veri kaynağında kümelemenin nasıl etkinleştirilen bir örneği verilmiştir.

```javascript
//Create a data source and enable clustering.
var datasource = new atlas.source.DataSource(null, {
    //Tell the data source to cluster point data.
    cluster: true,

    //The radius in pixels to cluster points together.
    clusterRadius: 45,

    //The maximum zoom level in which clustering occurs.
    //If you zoom in more than this, all points are rendered as symbols.
    clusterMaxZoom: 15
});
```

> [!TIP]
> İki veri noktası yerde birbirine yakınsa, kullanıcı yakınlaştırırsa yakınlaştırırsa yakınsın küme asla parçalanmaz. Bunu gidermek için, kümeleme mantığını devre dışı bırakıp her şeyi görüntüleme `clusterMaxZoom` seçeneğini ayarlayabilirsiniz.

`DataSource` Sınıfın kümeleme için sağladığı ek yöntemler şunlardır:

| Yöntem | Dönüş türü | Açıklama |
|--------|-------------|-------------|
| getClusterChildren(clusterId: sayı) | Söz&lt;&lt;Dizisi&lt;Özellik&gt; \| Geometrisi, herhangi bir Şekil&gt;&gt; | Verilen kümenin alt larını bir sonraki yakınlaştırma düzeyinde alır. Bu çocuklar şekiller ve alt kümelerin bir birleşimi olabilir. Alt kümeler Clustered Properties eşleşen özelliklere sahip özellikler olacaktır. |
| getClusterExpansionZoom(clusterId: sayı) | Söz&lt;numarası&gt; | Kümenin genişlemeye başlayacağı veya parçalanacağı bir yakınlaştırma düzeyi hesaplar. |
| getClusterLeaves(clusterId: sayı, limit: sayı, ofset: sayı) | Söz&lt;&lt;Dizisi&lt;Özellik&gt; \| Geometrisi, herhangi bir Şekil&gt;&gt; | Kümedeki tüm noktaları alır. Noktaların `limit` bir alt kümesini döndürmek için `offset` ayarlayın ve noktaları n için sayfayı kullanın. |

## <a name="display-clusters-using-a-bubble-layer"></a>Kabarcık katmanı kullanarak kümeleri görüntüleme

Kabarcık katmanı kümelenmiş noktaları işlemek için harika bir yoldur. Yarıçapı ölçeklendirmek ve kümedeki nokta sayısına göre rengi değiştirmek için ifadeleri kullanın. Kümeleri bir kabarcık katmanı kullanarak görüntülerseniz, kümelenmemiş veri noktalarını işlemek için ayrı bir katman kullanmanız gerekir.

Kümenin boyutunu kabarcığın üstünde görüntülemek için metin içeren bir sembol katmanı kullanın ve simge kullanmayın.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Temel kabarcık katmanı kümeleme" src="//codepen.io/azuremaps/embed/qvzRZY/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
CodePen'de Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Temel kabarcık katmanı <a href='https://codepen.io/azuremaps/pen/qvzRZY/'>kümeleme</a> () <a href='https://codepen.io'>bakın.</a>
</iframe>

## <a name="display-clusters-using-a-symbol-layer"></a>Sembol katmanı kullanarak kümeleri görüntüleme

Veri noktalarını görselleştirirken, sembol katmanı daha temiz bir kullanıcı arabirimi sağlamak için birbiriyle çakışan simgeleri otomatik olarak gizler. Bu varsayılan davranış, haritada veri noktaları yoğunluğunu göstermek istiyorsanız istenmeyen olabilir. Ancak, bu ayarlar değiştirilebilir. Tüm sembolleri görüntülemek `allowOverlap` için, `true`Sembol `iconOptions` katmanları özelliğinin seçeneğini . 

Temiz bir kullanıcı arabirimi tutarken veri noktaları yoğunluğunu göstermek için kümeleme kullanın. Aşağıdaki örnekte, sembol katmanını kullanarak özel sembollerin nasıl ekleyeceğiniz ve kümeleri ve tek tek veri noktalarını nasıl temsil ettiğiniz gösterilmektedir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Kümelenmiş Sembol katmanı" src="//codepen.io/azuremaps/embed/Wmqpzz/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/Wmqpzz/'>Kümelenmiş Sembol katmanına</a> bakın .
</iframe>

## <a name="clustering-and-the-heat-maps-layer"></a>Kümeleme ve ısı haritaları katmanı

Isı haritaları haritadaki veri yoğunluğunu görüntülemek için harika bir yoldur. Bu görselleştirme yöntemi, çok sayıda veri noktasını tek başına işleyebilir. Veri noktaları kümelenmişse ve küme boyutu ısı haritasının ağırlığı olarak kullanılırsa, ısı haritası daha fazla veriyi işleyebilir. Bu seçeneği elde etmek `weight` için, `['get', 'point_count']`ısı haritası katmanı seçeneğini . Küme yarıçapı küçük olduğunda, ısı haritası kümelenmemiş veri noktalarını kullanarak Bir ısı haritasıyla neredeyse aynı görünür, ancak çok daha iyi performans gösterir. Ancak, küme yarıçapı ne kadar küçükse, ısı haritası o kadar doğru olur, ancak daha az performans avantajı sağlar.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme ağırlıklı Isı Haritası" src="//codepen.io/azuremaps/embed/VRJrgO/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem Kümesi ağırlıklı <a href='https://codepen.io/azuremaps/pen/VRJrgO/'>Isı Haritası'na</a> bakın .
</iframe>

## <a name="mouse-events-on-clustered-data-points"></a>Kümelenmiş veri noktalarındaki fare olayları

Fare olayları kümelenmiş veri noktaları içeren bir katmanda oluştuğunda, kümelenmiş veri noktası olaya GeoJSON noktası özelliği nesnesi olarak geri döner. Bu nokta özelliği aşağıdaki özelliklere sahip olacaktır:

| Özellik adı             | Tür    | Açıklama   |
|---------------------------|---------|---------------|
| `cluster`                 | boole | Özellik bir kümeyi temsil edip temsil eder gösterir. |
| `cluster_id`              | string  | Veri Kaynağı `getClusterExpansionZoom`, `getClusterChildren`ve `getClusterLeaves` yöntemleri ile kullanılabilecek küme için benzersiz bir kimlik. |
| `point_count`             | number  | Kümenin içerdiği nokta sayısı.  |
| `point_count_abbreviated` | string  | Uzunsa `point_count` değeri kısaltan bir dize. (örneğin, 4.000 4K olur)  |

Bu örnek, küme noktaları işleyen bir kabarcık katmanı alır ve bir tıklama olayı ekler. Tıklatma olayı tetiklendiğinde, kod haritayı hesaplar ve kümenin parçalandığı bir sonraki yakınlaştırma düzeyine yakınlaştırır. Bu işlevsellik `getClusterExpansionZoom` `DataSource` sınıfın yöntemi ve `cluster_id` tıklatılmış kümelenmiş veri noktası özelliği kullanılarak uygulanır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme getClusterExpansionZoom" src="//codepen.io/azuremaps/embed/moZWeV/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
CodePen'de Kalem <a href='https://codepen.io/azuremaps/pen/moZWeV/'>Kümesi getClusterExpansionZoom</a> <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>'a</a>( ) Azure Haritalar'a bakın.
</iframe>

## <a name="display-cluster-area"></a>Görüntü küme alanı 

Bir kümenin temsil ettiği nokta verileri bir alana yayılır. Bu örnekte fare bir kümeüzerinde gezinildiğinde iki ana davranış oluşur. İlk olarak, kümede bulunan tek tek veri noktaları bir konveks gövdesini hesaplamak için kullanılacaktır. Daha sonra, bir alanı göstermek için konveks gövdesi haritada görüntülenir.  Konveks gövde, elastik bir bant gibi bir dizi noktayı saran ve `atlas.math.getConvexHull` yöntem kullanılarak hesaplanabilen bir çokgendir. Kümede bulunan tüm noktalar `getClusterLeaves` yöntem kullanılarak veri kaynağından alınabilir.

<br/>

 <iframe height="500" style="width: 100%;" scrolling="no" title="Küme alanı konveks gövdesi" src="//codepen.io/azuremaps/embed/QoXqWJ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile Kalem <a href='https://codepen.io/azuremaps/pen/QoXqWJ/'>Kümesi alanı eksveç gövdesine</a> bakın.
</iframe>

## <a name="aggregating-data-in-clusters"></a>Verileri kümeler halinde toplama

Genellikle kümeler küme içinde nokta sayısı ile bir sembol kullanılarak temsil edilir. Ancak, bazen ek ölçümlerle kümelerin stilini özelleştirmek istenir. Küme toplamları ile, özel özellikler oluşturulabilir ve [bir toplu ifade](data-driven-style-expressions-web-sdk.md#aggregate-expression) hesaplaması kullanılarak doldurulabilir.  Küme agregaları `clusterProperties` `DataSource`.

Aşağıdaki örnekte toplu bir ifade kullanır. Kod, bir kümedeki her veri noktasının varlık türü özelliğini temel alan bir sayım hesaplar. Bir kullanıcı bir kümeyi tıklattığında, bir açılır pencere küme hakkında ek bilgilerle birlikte gösterir.

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme toplamları" src="//codepen.io/azuremaps/embed/jgYyRL/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre Kalem <a href='https://codepen.io/azuremaps/pen/jgYyRL/'>Kümesi toplamlarına</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [DataSource sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [atlas.matematik isim uzayı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)

Uygulamanıza işlevsellik eklemek için kod örneklerine bakın:

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"]
> [Isı haritası katmanı ekleme](map-add-heat-map-layer.md)
