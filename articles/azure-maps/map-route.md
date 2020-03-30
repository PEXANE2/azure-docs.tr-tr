---
title: Rota yönlerini haritada göster | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak bir haritada iki konum arasındaki yol tariflerini nasıl görüntülediğinizi öğreneceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: dde9264d0cb65726b624b918982cfa01985b63ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371418"
---
# <a name="show-directions-from-a-to-b"></a>A'dan B'ye yönleri gösterme

Bu makalede, rota isteğinasıl yapacağınızı ve rotayı haritada nasıl gösterilebilirsiniz.

Bunu yapmanın iki yolu vardır. İlk yol, Bir hizmet modülü üzerinden [Azure Haritalar Rota API'sini](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) sorgulamaktır. İkinci yol, [Azure Haritalar Rota API'sinde](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)arama isteği nde bulunmak için [API'yi getir'i](https://fetch.spec.whatwg.org/) kullanmaktır. Her iki yol da aşağıda tartışılmıştır.

## <a name="query-the-route-via-service-module"></a>Servis modülü üzerinden rotayı sorgula

<iframe height='500' scrolling='no' title='Haritada A'dan B'ye yol tariflerini göster (Servis Modülü)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a><a href='https://codepen.io/azuremaps'>@azuremaps</a>() Azure Haritalar tarafından <a href='https://codepen.io/azuremaps/pen/RBZbep/'>yapılan bir haritada (Hizmet Modülü) A'dan B'ye yol tariflerini göster'e</a> bakın.
</iframe>

Yukarıdaki kodda, ilk blok bir harita nesnesi oluşturur ve erişim belirteci kullanmak için kimlik doğrulama mekanizmasını ayarlar. Talimatlar için [bir harita oluşturduğunu](./map-create.md) görebilirsiniz.

İkinci kod bloğu, erişim `TokenCredential` belirteciyle Azure Haritalar'daki HTTP isteklerini doğrulamak için bir kod oluşturur. Daha sonra `TokenCredential` geçer `atlas.service.MapsURL.newPipeline()` ve bir [Pipeline](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) örneği oluşturur. Azure `routeURL` Haritalar [Rotası](https://docs.microsoft.com/rest/api/maps/route) işlemlerinin URL'sini temsil eder.

Üçüncü kod bloğu, haritaya bir [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) nesnesi oluşturur ve ekler.

Dördüncü kod bloğu başlangıç ve bitiş [noktaları](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) nesneleri oluşturur ve bunları dataSource nesnesine ekler.

Çizgi LineString için bir [özelliktir.](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) [LineLayer,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) [DataSource'a](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sarılmış çizgi nesnelerini haritadaki satırlar olarak işler. Dördüncü kod bloğu oluşturur ve eşmeye bir çizgi katmanı ekler. [LinestringLayerOptions'da](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)bir satır katmanının özelliklerini görün.

Sembol [katmanı,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) [DataSource'a](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)sarılmış nokta tabanlı verileri işlemek için metinleri veya simgeleri kullanır. Metinler veya simgeler haritada sembol olarak işlenir. Beşinci kod bloğu oluşturur ve haritaya bir sembol katmanı ekler.

Altıncı kod bloğu, [hizmet modülünün](how-to-use-services-module.md)bir parçası olan Azure Haritalar yönlendirme hizmetini sorgular. RouteURL'nin [hesaplayan RouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest#methods) yöntemi, başlangıç ve bitiş noktaları arasında bir rota elde etmek için kullanılır. Yanıttan bir GeoJSON özellik koleksiyonu daha `geojson.getFeatures()` sonra yöntem kullanılarak ayıklanır ve veri kaynağına eklenir. Daha sonra yanıtı haritaüzerinde bir rota olarak işler. Haritaya satır ekleme hakkında daha fazla bilgi için [bkz.](map-add-line-layer.md)

Son kod bloğu, Haritanın [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak haritanın sınırlarını ayarlar.

Olay [dinleyicisinin](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)içinde rota sorgusu, veri kaynağı, sembol, çizgi katmanları ve kamera sınırları oluşturulur. Bu kod yapısı, sonuçların yalnızca harita tamamen yüklendikten sonra görüntülenmesini sağlar.

## <a name="query-the-route-via-fetch-api"></a>Rotayı Getir API üzerinden sorgula

<iframe height='500' scrolling='no' title='Haritada A'dan B'ye yol tariflerini göster' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>ile haritada A'dan B'ye yol tariflerini</a> görün.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur ve erişim belirteci kullanmak için kimlik doğrulama mekanizmasını ayarlar. Talimatlar için [bir harita oluşturduğunu](./map-create.md) görebilirsiniz.

İkinci kod bloğu, haritaya bir [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) nesnesi oluşturur ve ekler.

Üçüncü kod bloğu, rotanın başlangıç ve hedef noktalarını oluşturur. Daha sonra, bunları veri kaynağına ekler. [AddPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)kullanma ile ilgili talimatlar için [haritaüzerinde bir pin eklemek](map-add-pin.md) görebilirsiniz.

[LineLayer,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) [DataSource'a](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sarılmış çizgi nesnelerini haritadaki satırlar olarak işler. Dördüncü kod bloğu oluşturur ve eşmeye bir çizgi katmanı ekler. [LineLayerOptions'da](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)bir satır katmanının özelliklerini görün.

[Sembol katmanı,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) [Veri Kaynağı'na](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sarılmış nokta tabanlı verileri haritadaki semboller olarak işlemek için metin veya simgeler kullanır. Beşinci kod bloğu oluşturur ve haritaya bir sembol katmanı ekler. [SymbolLayerOptions'da](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)bir sembol katmanının özelliklerini görün.

Bir sonraki kod `SouthWest` bloğu, başlangıç ve hedef noktalarından gelir ve `NorthEast` haritanın [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak haritanın sınırlarını ayarlar.

Son kod bloğu, [Azure Haritalar Rota API'sine](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)arama isteği nde bulunmak için [API'yi getir'i](https://fetch.spec.whatwg.org/) kullanır. Yanıt daha sonra ayrıştırılır. Yanıt başarılı olduysa, enlem ve boylam bilgileri, bu noktaları bağlayarak bir dizi oluşturmak için kullanılır. Satır verileri daha sonra haritaüzerinde rota işlemek için veri kaynağına eklenir. Talimatlar için [haritada bir satır ekle'yi](map-add-line-layer.md) görebilirsiniz.

Olay [dinleyicisinin](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)içinde rota sorgusu, veri kaynağı, sembol, çizgi katmanları ve kamera sınırları oluşturulur. Yine, harita tamamen yüklendikten sonra sonuçların görüntülendiğinden emin olmak istiyoruz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yönlendirme hizmetini kullanmak için en iyi uygulamalar](how-to-use-best-practices-for-search.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Haritada trafiği göster](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Harita ile etkileşim - fare olayları](./map-events.md)
