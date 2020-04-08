---
title: Azure Haritalar ile harita oluşturma | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak bir web sayfasında haritaoluşturmayı öğreneceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c85d6078fce7fc8e5a5b5d8485517a8b262044a9
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80802340"
---
# <a name="create-a-map"></a>Harita oluşturma

Bu makalede, bir harita oluşturmak ve bir harita animasyon yolları gösterir.  

## <a name="loading-a-map"></a>Harita yükleme

Harita yüklemek [için, Harita sınıfının](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)yeni bir örneğini oluşturun. Haritayı başlatmada, haritayı işlemek için bir DIV eleman kimliği geçirin ve haritayı yüklerken kullanılacak bir dizi seçeneği geçirin. `atlas` Ad alanında varsayılan kimlik doğrulama bilgileri belirtilmemişse, haritayı yüklerken bu bilgilerin harita seçeneklerinde belirtilmesi gerekir. Harita, performans için eşzamanlı olarak çeşitli kaynaklar yükler. Bu nedenle, harita örneğini oluşturduktan sonra, haritaya bir `ready` veya `load` olay ekleyin ve ardından olay işleyicisine haritayla etkileşimde olan ek kodekleyin. Olay, `ready` haritanın programlı olarak etkileşime girebilmesi için yeterli kaynağa sahip olduğu anda devreye girer girmez devreye girer. Olay, `load` ilk harita görünümü tamamen yüklendikten sonra yanar. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Temel harita yükü" src="//codepen.io/azuremaps/embed/rXdBXx/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a göre Kalem <a href='https://codepen.io/azuremaps/pen/rXdBXx/'>Temel harita yüküne</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

> [!TIP]
> Aynı sayfaya birden çok harita yükleyebilirsiniz. Aynı sayfadaki birden çok harita aynı veya farklı kimlik doğrulama ve dil ayarlarını kullanabilir.

## <a name="show-a-single-copy-of-the-world"></a>Dünyanın tek bir kopyasını göster

Harita geniş bir ekranda uzaklaştırıldığında, dünyanın birden çok kopyası yatay olarak görünür. Bu seçenek bazı senaryolar için harikadır, ancak diğer uygulamalar için dünyanın tek bir kopyasını görmek istenir. Bu davranış, haritalar `renderWorldCopies` seçeneğini . `false`

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="renderWorldCopys = yanlış" src="//codepen.io/azuremaps/embed/eqMYpZ/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
CodePen'de Kalem <a href='https://codepen.io/azuremaps/pen/eqMYpZ/'>renderWorldCopys</a> =<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Haritalar tarafından false ( ) <a href='https://codepen.io'>bakın.</a>
</iframe>


## <a name="map-options"></a>Harita seçenekleri

Orada bir harita oluştururken, haritanın aşağıda listelenen gibi nasıl çalıştığını özelleştirmek için geçirilebilen birkaç farklı seçenek türü vardır.

- [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraoptions) ve [CameraBoundOptions,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraboundsoptions) haritanın görüntülemesi gereken alanı belirtmek için kullanılır.
- [ServiceOptions,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.serviceoptions) haritaya güç veren hizmetlerle nasıl etkileşimde bulunmalı olduğunu belirtmek için kullanılır.
- [StyleOptions,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions) haritanın şekillendirilmesi ve işlenmesi gerektiğini belirtmek için kullanılır.
- [KullanıcıEtkileşimSeçenekleri,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.userinteractionoptions) kullanıcı haritayla etkileşimde yken haritanın nasıl ulaşması gerektiğini belirtmek için kullanılır. 

Bu seçenekler, harita yüklendikten sonra da `setCamera`güncelleştirilebilir, , `setServiceOptions` `setStyle`, ve `setUserInteraction` işlevler. 

## <a name="controlling-the-map-camera"></a>Harita kamerasını denetleme

Haritanın kamerasını kullanarak haritanın görüntülenen alanını ayarlamanın iki yolu vardır. Haritayı yüklerken kamera seçeneklerini ayarlayabilirsiniz. Veya, harita nın `setCamera` görünümünü programlı olarak güncelleştirmek için harita yüklendikten sonra istediğiniz zaman bu seçeneği arayabilirsiniz.  

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Kamerayı ayarlama

Harita kamerası, harita tuvalinin görünüm portunda görüntülenenleri denetler. Kamera seçenekleri, parayla başharflere geçirilirken veya `setCamera` haritalar işlevine aktarılırken harita seçeneklerine aktarılabilir.

```javascript
//Set the camera options when creating the map.
var map = new atlas.Map('map', {
    center: [-122.33, 47.6],
    zoom: 12

    //Additional map options.
};

//Update the map camera at anytime using setCamera function.
map.setCamera({
    center: [-110, 45],
    zoom: 5 
});
```

Aşağıdaki kodda, bir [Harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map) oluşturulur ve orta ve yakınlaştırma seçenekleri ayarlanır. Orta ve yakınlaştırma düzeyi gibi harita özellikleri [CameraOptions'ın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraoptions)bir parçasıdır.

<br/>

<iframe height='500' scrolling='no' title='CameraOptions ile harita oluşturma' src='//codepen.io/azuremaps/embed/qxKBMN/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Konum Tabanlı Hizmetler<a href='https://codepen.io/azuremaps'>@azuremaps</a>( ) <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>üzerinden `CameraOptions` Bir harita Oluştur </a>Kalemi'ne bakın.
</iframe>

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Kamera sınırlarını ayarlama

Harita kamerasını güncellemek için bir sınırlama kutusu kullanılabilir. Sınırlayıcı kutu nokta verilerinden hesaplanmışsa, simge boyutunu hesaba katmak için kamera seçeneklerinde piksel dolgu değeri belirtmek de yararlıdır. Bu, noktaların harita görüş alanının kenarından düşmemesini sağlamaya yardımcı olur.

```javascript
map.setCamera({
    bounds: [-122.4, 47.6, -122.3, 47.7],
    padding: 10
});
```

Aşağıdaki kodda, bir [Harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map) . `new atlas.Map()` Harita `CameraBoundsOptions` gibi özellikler, Harita sınıfının [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map) fonksiyonu ile tanımlanabilir. Ciltler ve dolgu özellikleri `setCamera`kullanılarak ayarlanır.

<br/>

<iframe height='500' scrolling='no' title='CameraBoundsOptions ile harita oluşturma' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>üzerinden <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'> `CameraBoundsOptions` </a>Bir harita oluştur'a bakın.
</iframe>

### <a name="animate-map-view"></a>Harita görünümünü canlandırma

Haritanın kamera seçeneklerini ayarlarken, [animasyon seçenekleri](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.animationoptions) de ayarlanabilir. Bu seçenekler, kamerayı hareket ettirebilmek için gereken animasyon türünü ve süresini belirtir.

```javascript
map.setCamera({
    center: [-122.33, 47.6],
    zoom: 12,
    duration: 1000,
    type: 'fly'  
});
```

Aşağıdaki kodda, ilk kod bloğu bir harita oluşturur ve haritayı gir ve yakınlaştır stillerini ayarlar. İkinci kod bloğunda, animasyon düğmesi için bir tıklama olay işleyicisi oluşturulur. Bu düğme tıklandığında, `setCamera` işlev [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions) ve [AnimationOptions](/javascript/api/azure-maps-control/atlas.animationoptions)için bazı rasgele değerlerle çağrılır.

<br/>

<iframe height='500' scrolling='no' title='Harita Görünümü'ne Animasyon' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar'a göre<a href='https://codepen.io/azuremaps'>@azuremaps</a>Kalem Animasyon Harita <a href='https://codepen.io/azuremaps/pen/WayvbO/'>Görünümüne</a> bakın .
</iframe>

## <a name="try-out-the-code"></a>Kodu deneyin

Kod örneklerine bak. **JS sekmesindeki** JavaScript kodunu ve **Sonuç sekmesindeki**harita görünümü değişikliklerini görebilirsiniz. Ayrıca sağ üst köşedeki **CodePen'de Edit'i**tıklatabilir ve CodePen'deki kodu değiştirebilirsiniz.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [Kamera Seçenekleri](/javascript/api/azure-maps-control/atlas.cameraoptions)

> [!div class="nextstepaction"]
> [AnimasyonSeçenekleri](/javascript/api/azure-maps-control/atlas.animationoptions)

Uygulamanıza işlevsellik eklemek için kod örneklerine bakın:

> [!div class="nextstepaction"]
> [Haritanın stilini değiştirme](choose-map-style.md)

> [!div class="nextstepaction"]
> [Haritaya denetim ekleme](map-add-controls.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)
