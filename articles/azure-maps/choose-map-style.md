---
title: Azure haritalar 'da haritanın stilini değiştirme | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure Maps web SDK 'sında kullanılabilen stille ilgili işlevler hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: b1c5d9b5cd2b6b9bfecf8a0af79699061003eec1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80335682"
---
# <a name="change-the-style-of-the-map"></a>Haritanın stilini değiştirme

Eşleme, eşlemeler başlatıldığında veya daha sonra haritalar işlevi kullanılarak ayarlanabilede birçok farklı [Stil seçeneğini](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions) destekler `setStyle` . Bu makalede, Haritalar görünümünü özelleştirmek için bu stil seçeneklerinin nasıl kullanılacağı gösterilmektedir. Harita yüklemeden sonra bir stil ayarlamayı öğrenin ve stil seçici denetimini kullanarak yeni bir harita stili ayarlamayı öğrenin.

## <a name="set-the-style-options"></a>Stil seçeneklerini ayarlayın 

Stil seçenekleri, daha sonra haritalar işlevi kullanılarak başlatıldığında veya güncelleştirilirse haritaya geçirilebilir `setStyle` .

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

Aşağıdaki araç, farklı stil seçeneklerinin eşlemenin nasıl işleneceğini gösterir. 3B binaları görmek için, büyük bir şehre yaklaşın. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Harita stili seçenekleri" src="https://codepen.io/azuremaps/embed/eYNMjPb?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Codepen üzerinde Azure Maps () tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/eYNMjPb'>harita stil seçeneklerine</a> bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="choose-a-base-map-style"></a>Temel harita stili seçin

En yaygın harita stili seçeneklerinden biri, stillendirilmiş temel haritanın stilini değiştirmek için kullanılır. [Azure haritalar 'da desteklenen harita stillerinin](supported-map-styles.md) BIRÇOĞU Web SDK 'da bulunabilir. 

### <a name="set-base-map-style-on-map-load"></a>Harita yükünde taban harita stili ayarla


Eşleme stili, bu seçenek ayarlanarak eşleme başlatılırken belirlenebilir `style` . Aşağıdaki kodda, `style` eşleme seçeneği `grayscale_dark` başlatma sırasında olarak ayarlanır.

```javascript
var map = new atlas.Map('map', {
    style: 'grayscale_dark',

    //Additiona map options
);
```

<br/>

<iframe height='500' scrolling='no' title='Harita yükte stili ayarlama' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () tarafından <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>harita yükünün stilini ayarlama</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

### <a name="update-the-base-map-style"></a>Temel harita stilini güncelleştirme

 Harita stili, işlevi kullanılarak güncelleştirilemeyebilir `setStyle` ve `style` seçenek istenen harita stiline ayarlanabilir.

```javascript
map.setStyle({ style: 'satellite' });
```

Aşağıdaki kodda, bir eşleme örneği yüklendikten sonra, eşleme stili, ' den `road` `satellite` [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-maps-typescript-latest#setstyle-styleoptions-) işlevi kullanılarak güncelleştirilir.

<br/>

<iframe height='500' scrolling='no' title='Stili güncelleştirme' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen üzerinde Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>stili güncelleştirme</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

### <a name="add-the-style-picker"></a>Stil seçiciyi ekleme

Stil Seçici denetimi, son kullanıcı tarafından harita stilini değiştirmek için kullanılabilecek, açılır pencere paneli ile kullanımı kolay bir düğme sağlar. Stil seçicisinin iki farklı düzen seçeneği vardır. Varsayılan olarak, stil Seçici düzeni kullanır `icons` ve tüm harita stilini yatay bir simge satırı olarak görüntüler. 

<center>

![Stil Seçici simge düzeni](media/choose-map-style/style-picker-icon-layout.png)</center>

İkinci düzen seçeneği çağrılır `list` ve harita stillerinin kaydırılabilir bir listesini görüntüler.  

<center>

![Stil Seçici liste düzeni](media/choose-map-style/style-picker-list-layout.png)</center>


Aşağıdaki kod, bir stil Seçici denetiminin bir örneğinin nasıl oluşturulacağını ve haritanın sağ üst köşesine nasıl ekleneceğini gösterir. Stil Seçici, koyu bir stile sahip olacak şekilde ayarlanır ve liste katmanını kullanarak seçilen birkaç harita stilini gösterir.

```javascript
map.controls.add(new atlas.control.StyleControl({
    mapStyles: ['road', 'night', 'grayscale_dark', 'grayscale_light'],
    layout: 'list',
    style: 'dark'
}), {
    position: 'top-right'
}); 
```

Aşağıdaki kod, Map 'e varsayılan ayarlarına sahip bir stil Seçici denetimi ekler, böylece kullanıcı farklı harita stilleri arasında kolayca geçiş yapabilir. Harita stilini, sağ üst köşenin yakınında bulunan harita stili denetimini kullanarak değiştirin.

<br/>

<iframe height='500' scrolling='no' title='Stil seçiciyi ekleme' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen üzerinde Azure Maps () tarafından <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Stil seçiciyi ekleme</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Varsayılan olarak, Azure haritalar 'ın S0 fiyatlandırma katmanını kullanırken, stil Seçici denetimi tüm kullanılabilir stilleri listeler. Bu listedeki stil sayısını azaltmak isterseniz, listede görünmesini istediğiniz stillerin bir dizisini `mapStyle` Stil seçicisinin seçeneğine geçirin. S1 kullanıyorsanız ve tüm kullanılabilir stilleri göstermek istiyorsanız, `mapStyles` Stil seçicisinin seçeneğini olarak ayarlayın `"all"` .

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinmek için:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [StyleOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions)

> [!div class="nextstepaction"]
> [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol)

> [!div class="nextstepaction"]
> [StyleControlOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.stylecontroloptions)

Haritalar için denetimler ekleyin:

> [!div class="nextstepaction"]
> [Harita denetimleri ekleme](map-add-controls.md)

> [!div class="nextstepaction"]
> [Pin ekleme](map-add-pin.md)
