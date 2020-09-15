---
title: Haritada bir koordinat hakkındaki bilgileri göster | Microsoft Azure haritaları
description: Kullanıcı bir koordinat seçtiğinde haritadaki bir adres hakkındaki bilgileri görüntülemeyi öğrenin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-javascript
ms.openlocfilehash: 0026b3714be4cf0a59d348f6708a3840aaddf11b
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90085340"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makalede, tıklanan bir açılan konumun adresini gösteren bir ters adres aramasının nasıl yapılacağı gösterilmektedir.

Ters adres araması yapmak için iki yol vardır. Bir yol, [Azure Maps ters adres ARAMASı API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 'sini bir hizmet modülü aracılığıyla sorgulamanızı sağlar. Diğer bir deyişle, bir adres bulmak için [Azure Maps ters adres ARAMASı API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 'sine bir istek yapmak üzere [Fetch API](https://fetch.spec.whatwg.org/) 'sini kullanmak bu şekilde kullanılır. Her iki yol da aşağıda verilmiştir.

## <a name="make-a-reverse-search-request-via-service-module"></a>Hizmet modülü aracılığıyla ters arama isteği oluşturma

<iframe height='500' scrolling='no' title='Bir koordinat (hizmet modülü) bilgilerini al' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>bir koordinat (hizmet modülü) ile bilgi edinme</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>

Yukarıdaki kodda, ilk blok bir harita nesnesi oluşturur ve kimlik doğrulama mekanizmasını erişim belirtecini kullanacak şekilde ayarlar. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu, `TokenCredential` erişim belirteciyle Azure Maps 'A http isteklerinin kimliğini doğrulamak için bir oluşturur. Ardından `TokenCredential` `atlas.service.MapsURL.newPipeline()` ' a geçirir ve bir işlem [hattı](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline) örneği oluşturur. , `searchURL` Azure Maps [arama](https://docs.microsoft.com/rest/api/maps/search) işlemlerine yönelik bir URL 'yi temsil eder.

Üçüncü kod bloğu fare imlecinin stilini bir işaretçiye güncelleştirir ve bir [açılan](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup#open) nesne oluşturur. Yönergeler için [haritada açılan pencere Ekle '](./map-add-popup.md) ye bakabilirsiniz.

Dördüncü kod bloğu, fare tıklaması [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map#events)ekler. Tetiklendiğinde, tıklanan noktanın koordinatları ile bir arama sorgusu oluşturur. Daha sonra, koordinat adresi için [arama adresini al ters API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 'sini sorgulamak üzere [getsearchaddressreverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-)yöntemini kullanır. Daha sonra bir GeoJSON Özellik koleksiyonu, `geojson.getFeatures()` yanıttan yöntemi kullanılarak ayıklanır.

Beşinci kod bloğu, tıklanan koordinat konumunun yanıt adresini görüntüleyen HTML açılan içeriğini ayarlar.

İmleç değişikliği, açılan nesne ve Click olayının hepsi haritanın [yükleme olayı dinleyicisinde](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map#events)oluşturulur. Bu kod yapısı, koordinat bilgilerini almadan önce haritanın tam olarak yüklenmesini sağlar.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>Fetch API aracılığıyla ters arama isteği oluşturma

Fetch kullanarak bu konum için ters coğrafi kod isteği oluşturmak üzere haritaya tıklayın.

<iframe height='500' scrolling='no' title='Bir koordinattan bilgi alma' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' loading="lazy" allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Codepen 'da Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>bir koordinat hakkında bilgi edinme</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur ve kimlik doğrulama mekanizmasını erişim belirtecini kullanacak şekilde ayarlar. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz.

İkinci kod bloğu fare imlecinin stilini bir işaretçiye güncelleştirir. Bir [açılır](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup#open) nesne oluşturur. Yönergeler için [haritada açılan pencere Ekle '](./map-add-popup.md) ye bakabilirsiniz.

Üçüncü kod bloğu fare tıklamaları için bir olay dinleyicisi ekler. Fare tıklamasından sonra, tıklanan koordinat adresi için [Azure Maps ters adres ARAMASı API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) 'sini sorgulamak üzere [Fetch API](https://fetch.spec.whatwg.org/) 'sini kullanır. Başarılı bir yanıt için, tıklanan konumun adresini toplar. Açılır sınıfın [SetOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup#setoptions-popupoptions-) işlevini kullanarak açılan içeriği ve konumu tanımlar.

İmleç değişikliği, açılan nesne ve Click olayının hepsi haritanın [yükleme olayı dinleyicisinde](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map#events)oluşturulur. Bu kod yapısı, koordinat bilgilerini almadan önce haritanın tam olarak yüklenmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arama hizmetini kullanmaya yönelik en iyi uygulamalar](how-to-use-best-practices-for-search.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [Açılan Pencere](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [A'dan B'ye yönleri gösterme](./map-route.md)

> [!div class="nextstepaction"]
> [Trafiği gösterme](./map-show-traffic.md)
