---
title: Bir haritadaki kümeleme noktası verileri | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Web SDK 'sını kullanarak bir haritada nasıl küme verileri kümeleyeceğinizi ve bunu nasıl işleyebileceğinizi öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: codepen
ms.openlocfilehash: ce2891201331ee1efd861d2f13cec78c0551b6ba
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80804580"
---
# <a name="clustering-point-data"></a>Kümeleme noktası verileri

Haritada çok sayıda veri noktasını görselleştirirken, veri noktaları birbirleriyle çakışabilir. Çakışma, eşlemenin okunamaz hale gelmesine ve kullanılması zor olmasına neden olabilir. Kümeleme noktası verileri, birbirine yakın olan nokta verisini birleştirme ve bunları haritada tek bir kümelenmiş veri noktası olarak temsil etme işlemidir. Kullanıcı haritada yakınlaştırdıktan sonra, kümeler kendi bireysel veri noktalarıyla kesilir. Çok sayıda veri noktasıyla çalışırken, Kullanıcı deneyiminizi geliştirmek için kümeleme süreçlerini kullanın.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Clustering-point-data-in-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="enabling-clustering-on-a-data-source"></a>Veri kaynağında kümeleme etkinleştiriliyor

Seçeneğini true olarak ayarlayarak `DataSource` sınıfta kümelendirmeyi etkinleştirin. `cluster` Yakın `ClusterRadius` noktaları seçecek şekilde ayarlayın ve bunları bir kümede birleştirir. Değeri piksel cinsinden `ClusterRadius` olur. Kümeleme `clusterMaxZoom` mantığının devre dışı bırakılacağı yakınlaştırma düzeyini belirtmek için kullanın. Veri kaynağında kümelemenin nasıl etkinleştirileceği hakkında bir örnek aşağıda verilmiştir.

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
> İki veri noktası arka planda yakınsa, kullanıcının ne kadar yakın yakınlaştırılmadığına bakılmaksızın kümenin hiçbir şekilde hiçbir şekilde kesintiye uğramaması mümkündür. Bunu çözmek için, kümeleme mantığını devre dışı `clusterMaxZoom` bırakma ve her şeyi görüntüleme seçeneğini belirleyebilirsiniz.

`DataSource` Sınıfın kümeleme için sağladığı ek yöntemler şunlardır:

| Yöntem | Dönüş türü | Açıklama |
|--------|-------------|-------------|
| getClusterChildren (Clusterıd: Number) | Promise&lt;dizi&lt;özelliği&lt;geometrisi, herhangi&gt; \| bir şekil&gt;&gt; | Sonraki yakınlaştırma düzeyinde verilen kümenin alt öğelerini alır. Bu alt öğeler şekil ve alt kümelerin bir birleşimi olabilir. Alt kümeler, ClusteredProperties ile eşleşen özelliklerle özellik olacaktır. |
| getClusterExpansionZoom (Clusterıd: Number) | Promise&lt;numarası&gt; | Kümenin genişlemekte veya parçalanmasına başlayacağı yakınlaştırma düzeyini hesaplar. |
| Getclusteryaprakları (Clusterıd: Number, limit: Number, kayması: Number) | Promise&lt;dizi&lt;özelliği&lt;geometrisi, herhangi&gt; \| bir şekil&gt;&gt; | Kümedeki tüm noktaları alır. ' İ `limit` , noktaların bir alt kümesini döndürecek şekilde ayarlayın ve ' ı `offset` noktaları üzerinden kullanın. |

## <a name="display-clusters-using-a-bubble-layer"></a>Balon bir katman kullanarak kümeleri görüntüleme

Kabarcık katmanı, kümelenmiş noktaları işlemek için harika bir yoldur. Yarıçapı ölçeklendirmek için ifadeleri kullanın ve kümedeki noktaların sayısına göre rengi değiştirin. Kümeleri bir kabarcık katmanı kullanarak görüntülediğinizde, kümelenmemiş veri noktalarını işlemek için ayrı bir katman kullanmanız gerekir.

Kabarcığun üzerine kümenin boyutunu göstermek için, metinle birlikte bir simge katmanı kullanın ve simge kullanmayın.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Temel kabarcık katmanı Kümelemesi" src="//codepen.io/azuremaps/embed/qvzRZY/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/qvzRZY/'>temel balon katmanı kümelemesine</a> bakın.
</iframe>

## <a name="display-clusters-using-a-symbol-layer"></a>Sembol katmanı kullanarak kümeleri görüntüleme

Veri noktalarını görselleştirirken sembol katmanı, temizleyici bir kullanıcı arabirimi sağlamak için birbirleriyle çakışan sembolleri otomatik olarak gizler. Haritada veri noktaları yoğunluğunu göstermek istiyorsanız bu varsayılan davranış istenmeyen bir durum olabilir. Ancak, bu ayarlar değiştirilebilir. Tüm sembolleri göstermek için sembol katmanları `allowOverlap` `iconOptions` özelliğinin seçeneğini olarak `true`ayarlayın. 

Temiz bir kullanıcı arabirimi tutarken veri noktaları yoğunluğunu göstermek için kümeleme kullanın. Aşağıdaki örnekte, sembol katmanını kullanarak özel semboller ekleme ve kümeleri ve bireysel veri noktalarını temsil etme adımları gösterilmektedir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Kümelenmiş sembol katmanı" src="//codepen.io/azuremaps/embed/Wmqpzz/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından kullanılan kalem <a href='https://codepen.io/azuremaps/pen/Wmqpzz/'>kümelenmiş sembol katmanını</a> görüntüleyin.
</iframe>

## <a name="clustering-and-the-heat-maps-layer"></a>Kümeleme ve ısı haritaları katmanı

Isı haritaları haritadaki verilerin yoğunluğunu görüntülemenin harika bir yoludur. Bu görselleştirme yöntemi, kendi başına çok sayıda veri noktasını işleyebilir. Veri noktaları kümelenir ve küme boyutu ısı haritasının ağırlığı olarak kullanılıyorsa, ısı haritası daha da fazla veri işleyebilir. Bu seçeneği başarmak için, ısı haritası `weight` katmanının seçeneğini olarak `['get', 'point_count']`ayarlayın. Küme yarıçapı küçük olduğunda, ısı haritası kümelenmemiş veri noktalarını kullanarak bir ısı haritası ile neredeyse özdeş olacaktır, ancak çok daha iyi işlem yapar. Ancak, küme yarıçapı ne kadar küçükse, ısı haritası daha doğru olacaktır, ancak daha az performans avantajları olur.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme ağırlıklı ısı haritası" src="//codepen.io/azuremaps/embed/VRJrgO/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/VRJrgO/'>kümesi ağırlıklı ısı haritasını</a> inceleyin.
</iframe>

## <a name="mouse-events-on-clustered-data-points"></a>Kümelenmiş veri noktalarında fare olayları

Kümelenmiş veri noktaları içeren bir katmanda fare olayları gerçekleştiğinde, kümelenmiş veri noktası olaya GeoJSON Point özellik nesnesi olarak döner. Bu nokta özelliği aşağıdaki özelliklere sahip olacaktır:

| Özellik adı             | Tür    | Açıklama   |
|---------------------------|---------|---------------|
| `cluster`                 | boole | Özelliğin bir kümeyi temsil ettiğini belirtir. |
| `cluster_id`              | string  | Küme için, veri kaynağı `getClusterExpansionZoom`, `getClusterChildren`ve `getClusterLeaves` yöntemleriyle kullanılabilecek benzersiz bir kimlik. |
| `point_count`             | number  | Kümenin içerdiği noktaların sayısı.  |
| `point_count_abbreviated` | string  | Long ise `point_count` değeri abbreviates bir dize. (örneğin, 4.000 4K olur)  |

Bu örnek, küme noktalarını işleyen ve bir tıklama olayı ekleyen bir kabarcık katmanı alır. Click olayı tetiklendiğinde, kod, kümenin parçalama yaptığı sonraki yakınlaştırma düzeyine kadar olan eşlemeyi hesaplar ve büyütür. Bu işlev, `getClusterExpansionZoom` `DataSource` sınıfının yöntemi ve tıklanan kümelenmiş veri noktasının `cluster_id` özelliği kullanılarak uygulanır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme getClusterExpansionZoom" src="//codepen.io/azuremaps/embed/moZWeV/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile Ilgili <a href='https://codepen.io/azuremaps/pen/moZWeV/'>Getclusterexpansionzoom kalem kümesine</a> bakın.
</iframe>

## <a name="display-cluster-area"></a>Küme alanını görüntüle 

Bir kümenin temsil ettiği nokta verileri bir alanın üzerine yayılır. Bu örnekte, fare bir kümenin üzerine gelindiğinde iki ana davranış oluşur. İlk olarak, kümede bulunan bireysel veri noktaları, bir convex hull hesaplamak için kullanılacaktır. Ardından, convex kabuk, bir alanı göstermek için haritada görüntülenir.  Bir convex kabuk, esnek bant gibi bir dizi noktayı sarmalayan ve `atlas.math.getConvexHull` yöntemi kullanılarak hesaplanabilecek bir çokgen. Bir kümede yer alan tüm noktaları, `getClusterLeaves` yöntemi kullanılarak veri kaynağından alınabilir.

<br/>

 <iframe height="500" style="width: 100%;" scrolling="no" title="Küme alanı convex kabuk" src="//codepen.io/azuremaps/embed/QoXqWJ/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/QoXqWJ/'>convex kabuk olan kalem kümesi alanı</a> ' na bakın.
</iframe>

## <a name="aggregating-data-in-clusters"></a>Kümelerdeki verileri toplama

Genellikle kümeler, küme içindeki noktaların sayısıyla birlikte bir sembol kullanılarak temsil edilir. Ancak bazen ek ölçümler içeren kümelerin stilini özelleştirmek tercih edilir. Küme toplamaları ile özel özellikler bir [toplama ifadesi](data-driven-style-expressions-web-sdk.md#aggregate-expression) hesaplaması kullanılarak oluşturulabilir ve doldurulabilir.  Küme toplamaları, `clusterProperties` `DataSource`seçeneğinde tanımlanabilir.

Aşağıdaki örnek bir toplama ifadesi kullanır. Kod, bir kümedeki her bir veri noktasının varlık türü özelliğini temel alarak bir sayıyı hesaplar. Kullanıcı bir kümeye tıkladığında bir açılan pencerede, kümeyle ilgili ek bilgiler içeren bir açılır pencere görüntülenir.

<iframe height="500" style="width: 100%;" scrolling="no" title="Küme toplamaları" src="//codepen.io/azuremaps/embed/jgYyRL/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile ilgili kalem <a href='https://codepen.io/azuremaps/pen/jgYyRL/'>kümesi toplamlarını</a> inceleyin.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [DataSource sınıfı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Atlas. Math ad alanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)

Uygulamanıza işlevsellik eklemek için bkz. kod örnekleri:

> [!div class="nextstepaction"]
> [Baloncuk katmanı ekleme](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"]
> [Isı haritası katmanı ekleme](map-add-heat-map-layer.md)
