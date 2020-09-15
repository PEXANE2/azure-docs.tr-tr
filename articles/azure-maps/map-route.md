---
title: Harita üzerindeki rota yönlerini göster | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Web SDK 'sını kullanarak bir haritadaki iki konum arasındaki yönleri görüntülemeyi öğreneceksiniz.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 54a196cc8323e676dfb054a5fad260302833fa53
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090695"
---
# <a name="show-directions-from-a-to-b"></a>A'dan B'ye yönleri gösterme

Bu makalede yol isteği oluşturma ve Haritayı haritada gösterme gösterilmektedir.

Bunu iki şekilde yapabilirsiniz. İlk yöntem, [Azure Maps Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) 'sini bir hizmet modülü aracılığıyla sorgulamanızı sağlar. İkinci yol, [Azure Maps Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)'sine bir arama isteği yapmak IÇIN [getirme API](https://fetch.spec.whatwg.org/) 'sini kullanmaktır. Her iki yol da aşağıda ele alınmıştır.

## <a name="query-the-route-via-service-module"></a>Yönlendirme hizmetini hizmet modülü aracılığıyla sorgulama

<iframe height='500' scrolling='no' title='Bir haritadaki A 'dan B 'ye yönlendirmeler göster (hizmet modülü)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/RBZbep/'>haritada (hizmet modülü) bir haritadaki a 'Dan B 'ye yönlendirmeler göster</a> bölümüne bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

Yukarıdaki kodda, ilk blok bir harita nesnesi oluşturur ve kimlik doğrulama mekanizmasını erişim belirtecini kullanacak şekilde ayarlar. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu, `TokenCredential` erişim belirteciyle Azure Maps 'A http isteklerinin kimliğini doğrulamak için bir oluşturur. Ardından `TokenCredential` `atlas.service.MapsURL.newPipeline()` ' a geçirir ve bir işlem [hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline) örneği oluşturur. , `routeURL` Azure Maps [yönlendirme](https://docs.microsoft.com/rest/api/maps/route) işlemlerine yönelik bir URL 'yi temsil eder.

Üçüncü kod bloğu, haritaya bir [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource) nesnesi oluşturur ve ekler.

Dördüncü kod bloğu başlangıç ve bitiş [noktaları](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point) nesneleri oluşturur ve bunları DataSource nesnesine ekler.

Çizgi, LineString için bir [özelliktir](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature) . Bir [Linelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer) ,  [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource) kaydırılan satır nesnelerini haritada satırlar olarak işler. Dördüncü kod bloğu oluşturur ve haritaya bir çizgi katmanı ekler. [Linestringlayeroptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions)içindeki bir çizgi katmanının özelliklerine bakın.

Bir [sembol katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer) , [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource)Sarmalanan nokta tabanlı verileri işlemek için metinleri veya simgeleri kullanır. Metinler veya simgeler haritada semboller olarak işlenir. Beşinci kod bloğu oluşturur ve haritaya bir sembol katmanı ekler.

Altıncı kod bloğu, [hizmet modülünün](how-to-use-services-module.md)bir parçası olan Azure Maps yönlendirme hizmeti 'ni sorgular. RouteURL 'nin [Hesaplayısınuyönleri](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl#methods) yöntemi başlangıç ve bitiş noktaları arasında bir yol almak için kullanılır. Yanıttan bir GeoJSON Özellik koleksiyonu daha sonra yöntemi kullanılarak ayıklanır `geojson.getFeatures()` ve veri kaynağına eklenir. Ardından yanıtı haritada bir yol olarak işler. Haritaya satır ekleme hakkında daha fazla bilgi için bkz. [haritaya satır ekleme](map-add-line-layer.md).

Son kod bloğu haritanın [Setcamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak haritanın sınırlarını ayarlar.

Rota sorgusu, veri kaynağı, simge, çizgi katmanları ve kamera sınırları [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map#events)içinde oluşturulur. Bu kod yapısı, sonuçların yalnızca haritanın tamamen yüklenmesi durumunda görüntülenmesini sağlar.

## <a name="query-the-route-via-fetch-api"></a>Fetch API aracılığıyla rotayı sorgulama

<iframe height='500' scrolling='no' title='Haritada A 'dan B 'ye yönlendirmeler gösterme' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>haritada bir harita üzerinde a 'Dan B 'ye yönlendirmeler göster</a> bölümüne bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur ve kimlik doğrulama mekanizmasını erişim belirtecini kullanacak şekilde ayarlar. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu oluşturur ve haritaya bir [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource) nesnesi ekler.

Üçüncü kod bloğu, yol için başlangıç ve hedef noktaları oluşturur. Ardından, bunları veri kaynağına ekler. [Addpın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)'leri kullanma hakkında yönergeler için [haritada PIN ekleme '](map-add-pin.md) ye bakabilirsiniz.

Bir [Linelayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer) ,  [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource) kaydırılan satır nesnelerini haritada satırlar olarak işler. Dördüncü kod bloğu oluşturur ve haritaya bir çizgi katmanı ekler. [Linelayeroptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions)'da bir çizgi katmanının özelliklerine bakın.

Bir [sembol katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer) , [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource) kaydırılan nokta tabanlı verileri haritada semboller olarak oluşturmak için metin veya simgeleri kullanır. Beşinci kod bloğu oluşturur ve haritaya bir sembol katmanı ekler. Bkz. [Symbollayeroptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions)'da bir sembol katmanının özelliklerine bakın.

Sonraki kod bloğu, `SouthWest` Başlangıç ve hedef noktalarından oluşur ve bu noktaları oluşturur ve haritanın `NorthEast` [setcamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak haritanın sınırlarını ayarlar.

Son kod bloğu, [Azure Maps Route API](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)'sine bir arama isteği yapmak IÇIN [getirme API](https://fetch.spec.whatwg.org/) 'sini kullanır. Yanıt daha sonra ayrıştırılır. Yanıt başarılı olduysa, enlem ve boylam bilgileri, bu noktaları bağlayarak bir satırı dizi oluşturmak için kullanılır. Daha sonra, satır verileri haritada yolu işlemek için veri kaynağına eklenir. Yönergeler için [haritada satır ekleme '](map-add-line-layer.md) ye bakabilirsiniz.

Rota sorgusu, veri kaynağı, simge, çizgi katmanları ve kamera sınırları [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map#events)içinde oluşturulur. Yine, eşlemenin tam olarak yüklendikten sonra sonuçların görüntülendiğinden emin olmak istiyoruz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yönlendirme hizmetini kullanmaya yönelik en iyi uygulamalar](how-to-use-best-practices-for-search.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Haritada trafiği göster](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Harita-fare olaylarıyla etkileşim kurma](./map-events.md)
