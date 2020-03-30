---
title: Azure Haritalar'da haritanın stilini değiştirme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar web SDK'da bulunan stille ilgili işlevler hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: b1c5d9b5cd2b6b9bfecf8a0af79699061003eec1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335682"
---
# <a name="change-the-style-of-the-map"></a>Haritanın stilini değiştirme

Harita, harita [style options](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions) başharfe para yla başolarak verilirken veya daha `setStyle` sonra haritalar işlevini kullanırken ayarlanabilecek birkaç farklı stil seçeneğini destekler. Bu makalede, haritalar görünümünü özelleştirmek için bu stil seçeneklerinin nasıl kullanılacağı gösterilmektedir. Harita yüklenmeye bir stil ayarlamayı ve stil seçici denetimini kullanarak yeni bir harita stili ayarlamayı öğrenin.

## <a name="set-the-style-options"></a>Stil seçeneklerini ayarlama 

Stil seçenekleri, haritalar `setStyle` işlevi kullanılarak başharflere dönüştürüldüğünde veya daha sonra güncelleştirildiğinde haritaya aktarılabilir.

```javascript
//Set the style options when creating the map.
var map = new atlas.Map('map', {
    renderWorldCopies: false,
    showBuildingModels: true

    //Additional map options.
};

//Update the style options at anytime using setStyle function.
map.setStyle({
    renderWorldCopies: true,
    showBuildingModels: false
});
```

Aşağıdaki araç, farklı stil seçeneklerinin haritanın nasıl işlenirşekilde değiştireceğini gösterir. 3B binaları görmek için, büyük bir şehre yakın yakın yakınlaştırmak. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Harita stili seçenekleri" src="https://codepen.io/azuremaps/embed/eYNMjPb?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre Kalem <a href='https://codepen.io/azuremaps/pen/eYNMjPb'>Haritası stili seçeneklerine</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

## <a name="choose-a-base-map-style"></a>Temel harita stilini seçin

En yaygın harita stili seçeneklerinden biri, stilde olan temel haritanın stilini değiştirmek için kullanılır. [Azure Haritalar'da desteklenen harita stillerinin](supported-map-styles.md) çoğu Web SDK'da kullanılabilir. 

### <a name="set-base-map-style-on-map-load"></a>Harita yükünde temel harita stilini ayarlama


Harita, `style` seçeneği ayarlayarak haritayı başlatmada belirtilebilir. Aşağıdaki kodda, `style` haritanın seçeneği başlatma `grayscale_dark` üzerinde ayarlanır.

```javascript
var map = new atlas.Map('map', {
    style: 'grayscale_dark',

    //Additiona map options
);
```

<br/>

<iframe height='500' scrolling='no' title='Harita yükünde stili ayarlama' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>harita yükündeki stili ayarlayarak Kalem ayarı'na</a> bakın.
</iframe>

### <a name="update-the-base-map-style"></a>Temel harita stilini güncelleştirme

 Harita stili `setStyle` işlevi kullanılarak güncellenebilir ve `style` seçeneği istenilen harita stiline ayarlayabilir.

```javascript
map.setStyle({ style: 'satellite' });
```

Aşağıdaki kodda, bir harita örneği yüklendikten sonra, `road` harita `satellite` stili [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-maps-typescript-latest#setstyle-styleoptions-) işlevini kullanmaya kadar güncelleştirilir.

<br/>

<iframe height='500' scrolling='no' title='Stili güncelleştirme' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre stili <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>güncelleyen</a> Kaleme bakın ( )
</iframe>

### <a name="add-the-style-picker"></a>Stil seçiciyi ekle

Stil seçici denetimi, harita stilini değiştirmek için son kullanıcı tarafından kullanılabilecek uçuş paneli ile kullanımı kolay bir düğme sağlar. Stil seçiciiki farklı düzen seçeneği vardır. Varsayılan olarak stil seçici `icons` düzeni kullanır ve tüm harita stilini yatay bir simge satırı olarak görüntüler. 

<center>

![Stil seçici simgesi düzeni](media/choose-map-style/style-picker-icon-layout.png)</center>

İkinci düzen seçeneği `list` çağrılır ve kaydırılabilir harita stilleri listesini görüntüler.  

<center>

![Stil seçici liste düzeni](media/choose-map-style/style-picker-list-layout.png)</center>


Aşağıdaki kod, stil seçici denetiminin bir örneğinin nasıl oluşturulacağı ve haritanın sağ üst köşesine nasıl ekleyeceğini gösterir. Stil seçici koyu bir stil ve liste katmanı nı kullanarak seçilen birkaç harita stilleri göstermek için ayarlanır.

```javascript
map.controls.add(new atlas.control.StyleControl({
    mapStyles: ['road', 'night', 'grayscale_dark', 'grayscale_light'],
    layout: 'list',
    style: 'dark'
}), {
    position: 'top-right'
}); 
```

Aşağıdaki kod, varsayılan ayarlarıyla birlikte bir stil seçici denetimi ekler, böylece kullanıcı farklı harita stilleri arasında kolayca geçiş yapabilir. Sağ üst köşeye yakın harita stili denetimini kullanarak harita stilini geçişin.

<br/>

<iframe height='500' scrolling='no' title='Stil seçiciekleme' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>stil seçiciyi ekleme</a> kalemine bakın (<a href='https://codepen.io/azuremaps'>@azuremaps</a>)
</iframe>

> [!TIP]
> Varsayılan olarak, Azure Haritalar'ın S0 fiyatlandırma katmanını kullanırken stil seçici denetimi tüm kullanılabilir stilleri listeler. Bu listedeki stil sayısını azaltmak istiyorsanız, listede görünmesini istediğiniz stildizisini stil seçici `mapStyle` seçeneğine geçirin. S1 kullanıyorsanız ve tüm kullanılabilir stilleri göstermek istiyorsanız, stil seçici seçeneğini `"all"`. `mapStyles`

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinmek için:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [Stil Seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions)

> [!div class="nextstepaction"]
> [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol)

> [!div class="nextstepaction"]
> [StyleControlOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.stylecontroloptions)

Haritalarınıza denetimler ekleyin:

> [!div class="nextstepaction"]
> [Harita denetimleri ekleme](map-add-controls.md)

> [!div class="nextstepaction"]
> [Pin ekleme](map-add-pin.md)
