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
ms.openlocfilehash: 578abae5b206b31674b00b9d27ef34174b93759f
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76988592"
---
# <a name="create-a-map"></a>Harita oluşturma

Bu makalede harita oluşturma ve Haritayı canlandırma yolları gösterilmektedir.  

## <a name="loading-a-map"></a>Harita yükleme

Eşleme yüklemek için, [Map sınıfının](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)yeni bir örneğini oluşturun. Haritayı başlatırken, Haritayı işlemek için bir DIV öğe KIMLIĞI geçirin ve eşleme yüklenirken kullanılacak bir seçenek kümesi geçirin. `atlas` ad alanında varsayılan kimlik doğrulama bilgileri belirtilmemişse, eşleme yüklenirken bu bilgilerin eşleme seçeneklerinde belirtilmesi gerekir. Eşleme, performans için birkaç kaynağı zaman uyumsuz olarak yükler. Bu nedenle, eşleme örneğini oluşturduktan sonra haritaya bir `ready` veya `load` olayı ekleyin ve ardından eşleme ile etkileşimde bulunan ek kodu olay işleyicisine ekleyin. `ready` olayı, haritada programlı olarak bir şekilde etkileşim altına almak için yeterli miktarda kaynak yüklendiği anda ateşlenir. İlk harita görünümünün tamamen yüklenmesi tamamlandıktan sonra `load` olay ateşlenir. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Temel harita yüklemesi" src="//codepen.io/azuremaps/embed/rXdBXx/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/rXdBXx/'>temel harita yükleme</a> bölümüne bakın.
</iframe>

> [!TIP]
> Aynı sayfada birden çok eşleme yükleyebilirsiniz. Aynı sayfada birden çok eşleme aynı veya farklı kimlik doğrulama ve dil ayarlarını kullanabilir.

## <a name="show-a-single-copy-of-the-world"></a>Dünyanın tek bir kopyasını göster

Harita geniş bir ekranda yakınlaştırıldığında dünyanın birden fazla kopyası yatay olarak görünür. Bu seçenek bazı senaryolar için idealdir, ancak diğer uygulamalar için dünyanın tek bir kopyasını görmeniz istenebilir. Bu davranış, haritalar `renderWorldCopies` seçeneği `false`olarak ayarlanarak uygulanır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Renderworldkopyaları = false" src="//codepen.io/azuremaps/embed/eqMYpZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Için bkz. <a href='https://codepen.io/azuremaps/pen/eqMYpZ/'>renderworldkopyaların = false</a> .
</iframe>

## <a name="controlling-the-map-camera"></a>Harita kamerasını denetleme

Harita kamerasını kullanarak haritanın görüntülenen alanını ayarlamak için iki yol vardır. Haritayı yüklerken kamera seçeneklerini ayarlayabilirsiniz. Ya da eşleme görünümünü program aracılığıyla güncelleştirmek için eşleme yüklendikten sonra dilediğiniz zaman `setCamera` seçeneğini çağırabilirsiniz.  

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Kamerayı ayarlama

Aşağıdaki kodda bir [harita nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) oluşturulur ve Ortala ve Yakınlaştır seçenekleri ayarlanır. Orta ve yakınlaştırma düzeyi gibi harita özellikleri [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions)'in bir parçasıdır.

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

Aşağıdaki kodda, ilk kod bloğu bir harita oluşturur ve ENTER ve zoom harita stillerini ayarlar. İkinci kod bloğunda, animasyon düğmesi için bir tıklama olayı işleyicisi oluşturulur. Bu düğme tıklandığında `setCamera` işlevi, [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions) ve [animationoptions](/javascript/api/azure-maps-control/atlas.animationoptions)için bazı rastgele değerlerle çağırılır.

<br/>

<iframe height='500' scrolling='no' title='Harita görünümüne animasyon ekleme' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile ilgili <a href='https://codepen.io/azuremaps/pen/WayvbO/'>harita görünümüne</a> bakın.
</iframe>

## <a name="try-out-the-code"></a>Kodu deneyin

Kod örneklerine bakın. **Js sekmesinin** içindeki JavaScript kodunu düzenleyebilir ve **sonuç sekmesinde**harita görünümü değişikliklerini görebilirsiniz. Ayrıca, sağ üst köşede yer **alan CodePen 'Da Düzenle**' yi tıklatabilir ve kodu CodePen ' da değiştirebilirsiniz.

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
