---
title: Eşharitada koordinat la ilgili bilgileri göster | Microsoft Azure Haritaları
description: Bir kullanıcı koordinat ı seçtiğinde haritada bir adresle ilgili bilgileri nasıl görüntüleyeceklerini öğrenin.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 174bdc496e52a6ac8f2a2d631db92e0f21a819be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371429"
---
# <a name="get-information-from-a-coordinate"></a>Bir koordinattan bilgi alma

Bu makalede, tıklanan açılır pencere konumunun adresini gösteren ters adres araması nasıl yapılacağını gösterir.

Ters adres araması yapmanın iki yolu vardır. Bunun bir yolu, [Azure Haritalar Ters Adres Arama API'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) bir hizmet modülü üzerinden sorgulamaktır. Diğer yol, bir adres bulmak için [Azure Haritalar Ters Adres Arama API'sine](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) istekte bulunmak için [API'yi getir'i](https://fetch.spec.whatwg.org/) kullanmaktır. Her iki yol da aşağıda incelenmiştir.

## <a name="make-a-reverse-search-request-via-service-module"></a>Servis modülü üzerinden ters arama isteğinde bulunun

<iframe height='500' scrolling='no' title='Koordinattan bilgi alın (Servis Modülü)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Kalem <a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>koordinat (Hizmet Modülü) bilgi alın.</a>
</iframe>

Yukarıdaki kodda, ilk blok bir harita nesnesi oluşturur ve erişim belirteci kullanmak için kimlik doğrulama mekanizmasını ayarlar. Talimatlar için [bir harita oluşturduğunu](./map-create.md) görebilirsiniz.

İkinci kod bloğu, `TokenCredential` erişim belirteciyle Azure Haritalar'daki HTTP isteklerini doğrulamak için bir kod oluşturur. Daha sonra `TokenCredential` geçer `atlas.service.MapsURL.newPipeline()` ve bir [Pipeline](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) örneği oluşturur. Azure `searchURL` Haritalar [Arama](https://docs.microsoft.com/rest/api/maps/search) işlemlerinin URL'sini temsil eder.

Üçüncü kod bloğu fare imleci stilini bir işaretçiye güncelleştirir ve bir [açılır pencere nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) oluşturur. Talimatlar için [haritada bir açılır pencere ekle'yi](./map-add-popup.md) görebilirsiniz.

Kodun dördüncü bloğu bir fare tıklama [olay dinleyici](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)ekler. Tetiklendiğinde, tıklatılan noktanın koordinatlarını içeren bir arama sorgusu oluşturur. Daha sonra koordinatların adresi için [Arama Adresi Ters API'yi sorgulamak](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) için [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-)yöntemini kullanır. Bir GeoJSON özellik koleksiyonu daha `geojson.getFeatures()` sonra yanıt yöntemi kullanılarak ayıklanır.

Beşinci kod bloğu, tıklanan koordinat pozisyonunun yanıt adresini görüntülemek için HTML açılır içeriğini ayarlar.

İmleç, açılır pencere nesnesi ve tıklama olayının değiştirilmesi, haritanın [yük olayı dinleyicisinde](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)oluşturulur. Bu kod yapısı, koordinat bilgilerini almadan önce haritanın tam yüklerini sağlar.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>ApI Getir üzerinden ters arama isteği nde bulunma

Getir'i kullanarak o konum için ters coğrafi kod isteği nde bulunmak için haritayı tıklatın.

<iframe height='500' scrolling='no' title='Bir koordinattan bilgi alma' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Bkz. Kalem <a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>koordinatından bilgi alın</a> ( )
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur ve erişim belirteci kullanmak için kimlik doğrulama mekanizmasını ayarlar. Talimatlar için [bir harita oluşturduğunu](./map-create.md) görebilirsiniz.

İkinci kod bloğu fare imlecinin stilini bir işaretçiye güncelleştirir. Bir [açılır](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) nesneyi anında ortaya çıkar. Talimatlar için [haritada bir açılır pencere ekle'yi](./map-add-popup.md) görebilirsiniz.

Üçüncü kod bloğu fare tıklamaları için bir olay dinleyicisi ekler. Fare tıklaması üzerine, tıklanan koordinat adresi için [Azure Haritalar Ters Adres Arama API'sini](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) sorgulamak için [API'yi getir'i](https://fetch.spec.whatwg.org/) kullanır. Başarılı bir yanıt için, tıklanan konumun adresini toplar. Açılır pencere sınıfının [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) işlevini kullanarak açılır içeriği ve konumu tanımlar.

İmleç, açılır pencere nesnesi ve tıklama olayının değiştirilmesi, haritanın [yük olayı dinleyicisinde](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)oluşturulur. Bu kod yapısı, koordinat bilgilerini almadan önce haritanın tam yükolmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arama hizmetini kullanmak için en iyi uygulamalar](how-to-use-best-practices-for-search.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Açılan Pencere](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [A'dan B'ye yönleri gösterme](./map-route.md)

> [!div class="nextstepaction"]
> [Trafiği gösterme](./map-show-traffic.md)
