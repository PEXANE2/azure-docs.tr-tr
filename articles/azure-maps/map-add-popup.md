---
title: Haritada bir noktaya açılan pencere Ekle | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure Maps web SDK 'sını kullanarak bir noktaya açılan pencere eklemeyi öğreneceksiniz.
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 45d210725f7f09663b126528479655d7f4d9c19f
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76933346"
---
# <a name="add-a-popup-to-the-map"></a>Haritaya bir açılan pencere ekleyin

Bu makalede, haritada bir noktaya açılan pencerenin nasıl ekleneceği gösterilmektedir.

## <a name="understand-the-code"></a>Kodu anlama

Aşağıdaki kod, bir sembol katmanını kullanarak eşlemeye `name` ve `description` özelliklerine sahip bir nokta özelliği ekler. [Açılan sınıfın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) bir örneği oluşturulur, ancak gösterilmez. Fare olayları, açılan menüyü açmak ve kapatmak için sembol katmanına eklenir. İşaret simgesi üzerine gelindiğinde, açılan pencerenin `position` özelliği işaretin konumuyla güncellenir ve `content` seçeneği, nokta özelliğinin `name` ve `description` özelliklerini sarmalayan bazı HTML ile güncelleştirilir. Açılır pencere daha sonra `open` işlevi kullanılarak haritada görüntülenir.

```javascript
//Define an HTML template for a custom popup content laypout.
var popupTemplate = '<div class="customInfobox"><div class="name">{name}</div>{description}</div>';

//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

dataSource.add(new atlas.data.Feature(new atlas.data.Point([-122.1333, 47.63]), {
  name: 'Microsoft Building 41', 
  description: '15571 NE 31st St, Redmond, WA 98052'
}));

//Create a layer to render point data.
var symbolLayer = new atlas.layer.SymbolLayer(dataSource);

//Add the polygon and line the symbol layer to the map.
map.layers.add(symbolLayer);

//Create a popup but leave it closed so we can update it and display it later.
popup = new atlas.Popup({
  pixelOffset: [0, -18],
  closeButton: false
});

//Add a hover event to the symbol layer.
map.events.add('mouseover', symbolLayer, function (e) {
  //Make sure that the point exists.
  if (e.shapes && e.shapes.length > 0) {
    var content, coordinate;
    var properties = e.shapes[0].getProperties();
    content = popupTemplate.replace(/{name}/g, properties.name).replace(/{description}/g, properties.description);
    coordinate = e.shapes[0].getCoordinates();

    popup.setOptions({
      //Update the content of the popup.
      content: content,

      //Update the popup's position with the symbol's coordinate.
      position: coordinate

    });
    //Open the popup.
    popup.open(map);
  }
});

map.events.add('mouseleave', symbolLayer, function (){
  popup.close();
});
```

Aşağıda, yukarıdaki işlevselliğin tamamen çalışan kod örneği verilmiştir.

<br/>

<iframe height='500' scrolling='no' title='Azure haritalar 'ı kullanarak bir açılan pencere ekleme' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Azure Maps kullanarak bir açılan pencere ekleme</a> kalemine bakın.
</iframe>

## <a name="reusing-a-popup-with-multiple-points"></a>Birden çok noktayla açılan pencereyi yeniden kullanma

Çok sayıda noktanız olduğunda ve tek seferde yalnızca bir açılan pencere göstermek istiyorsanız, en iyi yaklaşım bir açılan pencere oluşturmak ve yeniden kullanmaktır. Açılan pencereyi yeniden kullanmaya göre, uygulama tarafından oluşturulan DOM öğelerinin sayısı büyük ölçüde azalır ve bu da daha iyi performans sağlayabilir. Aşağıdaki örnekte 3 puntoluk Özellikler oluşturulur. Bunlardan birine tıklarsanız, bu nokta özelliği için içerik ile bir açılan pencere görüntülenir.

<br/>

<iframe height='500' scrolling='no' title='Çoklu PIN ile açılan pencereyi yeniden kullanma' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>birden çok PIN Ile açılan pencereyi yeniden kullanma</a> kalemine bakın.
</iframe>

## <a name="customizing-a-popup"></a>Açılan pencereyi özelleştirme

Varsayılan olarak, açılan pencerede beyaz bir arka plan, altta bir işaretçi ok ve sağ üst köşedeki bir Kapat düğmesi bulunur. Aşağıdaki örnek, açılan pencerenin `fillColor` seçeneğini kullanarak arka plan rengini siyaha dönüştürür. Kapat düğmesi, `CloseButton` seçeneğinin false olarak ayarlanarak kaldırılır. Açılan menünün HTML içeriği, açılan pencerenin kenarlarından 10 piksel doldurulmuş kullanır. Metin beyaz yapılır, bu nedenle siyah arka planda düzgün şekilde görünür.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Özelleştirilmiş açılan pencere" src="//codepen.io/azuremaps/embed/ymKgdg/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/ymKgdg/'>özelleştirilmiş açılan pencere</a> başlığına bakın.
</iframe>

## <a name="popup-events"></a>Açılan olaylar

Açılanlar açılabilir, kapatılabilir ve sürüklenip kapatılabilir. Açılan sınıf, geliştiricilerin bu olaylara tepki vermesini sağlamak için olaylar sağlar. Aşağıdaki örnek, Kullanıcı açılan pencereyi açtığında, kapattığında veya sürüklediğinde hangi olayların tetiklediği vurgulanmıştır. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Açılan olaylar" src="//codepen.io/azuremaps/embed/BXrpvB/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/BXrpvB/'>açılan olayları</a> ' na bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Kutu](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki harika makalelere göz atın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](./map-add-pin.md)

> [!div class="nextstepaction"]
> [HTML işaretleyicisi ekleme](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı Ekle](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı Ekle](map-add-shape.md)
