---
title: Azure haritalar ile harita oluşturma | Microsoft Azure haritaları
description: Bu makalede, bir Web sayfasında Microsoft Azure haritaları Web SDK 'sını kullanarak bir haritanın nasıl işleneceğini öğreneceksiniz.
author: jingjing-z
ms.author: jinzh
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 49c86f3e6c654ecbfcd07809f42a1b038ca3f8ab
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911119"
---
# <a name="create-a-map"></a>Harita oluşturma

Bu makalede harita oluşturma ve Haritayı canlandırma yolları gösterilmektedir.  

## <a name="loading-a-map"></a>Harita yükleme

Eşleme yüklemek için, [Map sınıfının](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)yeni bir örneğini oluşturun. Harita başlatılırken, Haritayı işlemek için bir DIV öğe KIMLIĞI ve eşleme yüklenirken kullanılacak bir seçenek kümesi geçirilir. `atlas` ad alanında varsayılan kimlik doğrulama bilgileri belirtilmemişse, eşleme yüklenirken bu bilgilerin eşleme seçeneklerinde belirtilmesi gerekir. Eşleme, performans için birkaç kaynağı zaman uyumsuz olarak yükler. Bu nedenle, eşleme örneğini oluşturduktan sonra haritaya bir `ready` veya `load` olayı ekleyin ve ardından bu olay işleyicisindeki eşlemele etkileşim kuran ek kodu ekleyin. `ready` olayı, haritada programlı olarak bir şekilde etkileşim altına almak için yeterli miktarda kaynak yüklendiği anda ateşlenir. İlk harita görünümünün tamamen yüklenmesi tamamlandıktan sonra `load` olay ateşlenir. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Temel harita yüklemesi" src="//codepen.io/azuremaps/embed/rXdBXx/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/rXdBXx/'>temel harita yükleme</a> bölümüne bakın.
</iframe>

> [!TIP]
> Aynı sayfada birden çok eşleme yüklenebilir ve her biri aynı veya farklı kimlik doğrulama ve dil ayarlarını kullanabilir.

## <a name="show-a-single-copy-of-the-world"></a>Dünyanın tek bir kopyasını göster

Harita geniş bir ekranda yakınlaştırıldığında dünyanın birden fazla kopyası yatay olarak görünür. Bu çoğu senaryo için idealdir, ancak bazı uygulamalarda yalnızca dünyanın tek bir kopyasını görmek istenebilir. Bu, haritalar `renderWorldCopies` seçeneği `false`olarak ayarlanarak yapılabilir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Renderworldkopyaları = false" src="//codepen.io/azuremaps/embed/eqMYpZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Için bkz. <a href='https://codepen.io/azuremaps/pen/eqMYpZ/'>renderworldkopyaların = false</a> .
</iframe>

## <a name="controlling-the-map-camera"></a>Harita kamerasını denetleme

Kamerayı kullanarak haritanın görüntülenen alanını ayarlayabileceğiniz iki yol vardır. Harita yüklenirken merkez ve yakınlaştırma gibi kamera seçeneklerini ayarlayabilir veya eşleme görünümünü program aracılığıyla güncelleştirmek için haritanın her zaman `setCamera` seçeneğini çağırabilirsiniz.  

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Kamerayı ayarlama

Aşağıdaki kodda bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) oluşturulur ve Ortala ve Yakınlaştır seçenekleri ayarlanır. Merkez ve yakınlaştırma düzeyi gibi harita özellikleri [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions)'in bir parçasıdır.

<br/>

<iframe height='500' scrolling='no' title='CameraOptions aracılığıyla harita oluşturma' src='//codepen.io/azuremaps/embed/qxKBMN/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure konum tabanlı hizmetler (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>`CameraOptions` aracılığıyla bir harita oluşturma </a>kalemine bakın.
</iframe>

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Kamera sınırlarını ayarlama

Aşağıdaki kodda, bir [Map nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) `new atlas.Map()`aracılığıyla oluşturulur. `CameraBoundsOptions` gibi eşleme özellikleri, Map sınıfının [Setcamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) işlevi aracılığıyla tanımlanabilir. Sınırlar ve doldurma özellikleri `setCamera`kullanılarak ayarlanır.

<br/>

<iframe height='500' scrolling='no' title='CameraBoundsOptions aracılığıyla harita oluşturma' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>`CameraBoundsOptions` bir harita oluşturma </a>kalemine bakın.
</iframe>

### <a name="animate-map-view"></a>Harita görünümüne animasyon ekleme

Aşağıdaki kodda, ilk kod bloğu bir harita oluşturur ve harita stilini, merkezi ve yakınlaştırma değerlerini ayarlar. İkinci kod bloğunda, animasyon düğmesi için bir tıklama olayı işleyicisi oluşturulur. Bu düğmeye tıklandığında, setCamera işlevi [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions), [animationoptions](/javascript/api/azure-maps-control/atlas.animationoptions)için bazı rastgele değerlerle çağırılır.

<br/>

<iframe height='500' scrolling='no' title='Harita görünümüne animasyon ekleme' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile ilgili <a href='https://codepen.io/azuremaps/pen/WayvbO/'>harita görünümüne</a> bakın.
</iframe>

## <a name="try-out-the-code"></a>Kodu deneyin

Yukarıdaki örnek koda göz atın. Sol taraftaki **js sekmesinde** JavaScript kodunu düzenleyebilir ve sağ taraftaki **sonuç sekmesinde** harita görünümü değişiklikleri ' ne bakabilirsiniz. Ayrıca, **birlikte Düzenle** düğmesine tıklayıp kodu CodePen ' de düzenleyebilirsiniz.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions)

> [!div class="nextstepaction"]
> [AnimationOptions](/javascript/api/azure-maps-control/atlas.animationoptions)

Uygulamanıza işlevsellik eklemek için bkz. kod örnekleri:

> [!div class="nextstepaction"]
> [Haritanın stilini değiştir](choose-map-style.md)

> [!div class="nextstepaction"]
> [Haritaya denetim ekleme](map-add-controls.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)