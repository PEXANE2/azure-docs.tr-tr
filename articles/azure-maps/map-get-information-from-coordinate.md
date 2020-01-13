---
title: Haritada bir koordinat hakkındaki bilgileri göster | Microsoft Azure haritaları
description: Kullanıcı bir koordinat seçtiğinde haritadaki bir adres hakkındaki bilgileri görüntülemeyi öğrenin.
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 5450ada04a1af44a3fff0402b30540e899cc4dd5
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911038"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makalede, tıklanan bir açılan konumun adresini gösteren bir ters adres aramasının nasıl yapılacağı gösterilmektedir.

Ters adres araması yapmak için iki yol vardır. Bir yol, [Azure Maps ters adres ARAMASı API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 'sini bir hizmet modülü aracılığıyla sorgulamanızı sağlar. Diğer bir deyişle, bir adres bulmak için [Azure Maps ters adres ARAMASı API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 'sine bir istek yapmak üzere [getirme API](https://fetch.spec.whatwg.org/) 'sini kullanmak daha da olur. Her iki yol da aşağıda verilmiştir.

## <a name="make-a-reverse-search-request-via-service-module"></a>Hizmet modülü aracılığıyla ters arama isteği oluşturma

<iframe height='500' scrolling='no' title='Bir koordinat (hizmet modülü) bilgilerini al' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>bir koordinat (hizmet modülü) ile bilgi edinme</a> kalemine bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur ve kimlik doğrulama mekanizmasını erişim belirtecini kullanacak şekilde ayarlar. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu, erişim belirteci ile Azure Maps 'a HTTP isteklerinin kimliğini doğrulamak için bir `TokenCredential` oluşturur. Ardından `TokenCredential` `atlas.service.MapsURL.newPipeline()` geçirir ve bir işlem [hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) örneği oluşturur. `searchURL`, Azure Maps [arama](https://docs.microsoft.com/rest/api/maps/search) işlemlerine yönelik bir URL 'yi temsil eder.

Üçüncü kod bloğu fare imlecinin stilini bir işaretçiye güncelleştirir ve bir [açılan](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) nesne oluşturur. Yönergeler için [haritada açılan pencere Ekle '](./map-add-popup.md) ye bakabilirsiniz.

Dördüncü kod bloğu, fare tıklaması [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)ekler. Tetiklendiğinde, tıklanan noktanın koordinatları ile bir arama sorgusu oluşturur. Daha sonra, koordinat adresi için [arama adresini al ters API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 'sini sorgulamak üzere hizmet modülünün [getsearchaddressreverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-) yöntemini kullanır. Yanıttan bir GeoJSON Özellik koleksiyonu daha sonra `geojson.getFeatures()` yöntemi kullanılarak ayıklanır.

Beşinci kod bloğu, tıklanan koordinat konumunun yanıt adresini görüntüleyen HTML açılan içeriğini ayarlar.

İmleç, bir açılan nesne ve tıklama olayının değiştirilmesi, koordinat bilgilerinin alınabilmesi için eşlemenin [yük olay dinleyicisinde](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) oluşturulur.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>Fetch API aracılığıyla ters arama isteği oluşturma

Fetch kullanarak bu konum için ters coğrafi kod isteği oluşturmak üzere haritaya tıklayın.

<iframe height='500' scrolling='no' title='Bir koordinattan bilgi alma' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>bir koordinat hakkında bilgi edinme</a> kalemine bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur ve kimlik doğrulama mekanizmasını erişim belirtecini kullanacak şekilde ayarlar. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu, fare imlecinin stilini işaretçi ve [açılır](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) nesne olarak güncelleştirir. Yönergeler için [haritada açılan pencere Ekle '](./map-add-popup.md) ye bakabilirsiniz.

Üçüncü kod bloğu fare tıklamaları için bir olay dinleyicisi ekler. Fare tıklamasından sonra, tıklanan koordinat adresi için [Azure Maps ters adres ARAMASı API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 'sini sorgulamak üzere [getirme API](https://fetch.spec.whatwg.org/) 'sini kullanır. Başarılı bir yanıt için, tıklanan konumun adresini toplar ve açılan sınıfın [SetOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) işlevi aracılığıyla açılan içeriği ve konumu tanımlar.

İmleç, bir açılan nesne ve tıklama olayının değiştirilmesi, koordinat bilgilerinin alınabilmesi için eşlemenin [yük olay dinleyicisinde](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [A 'dan B 'ye yönlendirmeler göster](./map-route.md)

> [!div class="nextstepaction"]
> [Trafiği göster](./map-show-traffic.md)
