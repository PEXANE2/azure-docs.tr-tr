---
title: Arama sonuçlarını haritada göster | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak arama isteğinin nasıl gerçekleştirileceğinizi ve sonuçları haritada nasıl görüntüleyeceğinizi öğreneceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: e82a1daee381c7bad19c83fa735d0028bef2010e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371392"
---
# <a name="show-search-results-on-the-map"></a>Arama sonuçlarını haritada göster

Bu makalede, ilgi çekici yeri arama ve haritaüzerinde arama sonuçlarını göstermek için nasıl gösterir.

İlgi çekici bir yeri aramanın iki yolu vardır. Bir yolu bir arama isteği yapmak için bir hizmet modülü kullanmaktır. Diğer yol, [Getir API'si](https://fetch.spec.whatwg.org/)aracılığıyla [Azure Haritalar Bulanık arama API'sine](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) arama isteğinde bulunmaktır. Her iki yol da aşağıda tartışılmıştır.

## <a name="make-a-search-request-via-service-module"></a>Servis modülü üzerinden arama isteğinde bulunun

<iframe height='500' scrolling='no' title='Arama sonuçlarını haritada göster (Servis Modülü)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>bir haritada (Hizmet Modülü) Kalem Göster arama sonuçlarına</a> bakın.
</iframe>

Yukarıdaki kodda, ilk blok bir harita nesnesi oluşturur ve erişim belirteci kullanmak için kimlik doğrulama mekanizmasını ayarlar. Talimatlar için [bir harita oluşturduğunu](./map-create.md) görebilirsiniz.

İkinci kod bloğu, erişim `TokenCredential` belirteciyle Azure Haritalar'daki HTTP isteklerini doğrulamak için bir kod oluşturur. Daha sonra `TokenCredential` geçer `atlas.service.MapsURL.newPipeline()` ve bir [Pipeline](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) örneği oluşturur. Azure `searchURL` Haritalar [Arama](https://docs.microsoft.com/rest/api/maps/search) işlemlerinin URL'sini temsil eder.

Üçüncü kod bloğu, [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfını kullanarak bir veri kaynağı nesnesi oluşturur ve ona arama sonuçları ekler. [Sembol katmanı,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) [Veri Kaynağı'na](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sarılmış nokta tabanlı verileri haritadaki semboller olarak işlemek için metin veya simgeler kullanır.  Daha sonra bir sembol katmanı oluşturulur. Veri kaynağı, daha sonra haritaya eklenen sembol katmanına eklenir.

Dördüncü kod [bloğu, hizmet modülündeki](how-to-use-services-module.md) [SearchFuzzy](/javascript/api/azure-maps-rest/atlas.service.models.searchgetsearchfuzzyoptionalparams) yöntemini kullanır. Bu ilgi noktası aramak için Arama [Fuzzy dinlenme API ile](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) ücretsiz bir form metin arama yapmanızı sağlar. Arama Bulanık API'sine istekalmak bulanık girişlerin herhangi bir birleşimini işleyebilir. Yanıttan bir GeoJSON özellik koleksiyonu daha `geojson.getFeatures()` sonra yöntem kullanılarak ayıklanır ve veri kaynağına eklenir ve bu da veri nin sembol katmanı üzerinden haritaüzerinde işlenmesiyle otomatik olarak sonuçlanır.

Son kod bloğu, Harita'nın [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) özelliğini kullanarak haritanın kamera sınırlarını ayarlar.

Arama isteği, veri kaynağı, sembol katmanı ve kamera sınırları haritanın [olay dinleyicisinin](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) içindedir. Sonuçların harita tamamen yüklendikten sonra görüntülendiğinden emin olmak istiyoruz.


## <a name="make-a-search-request-via-fetch-api"></a>Getir API üzerinden arama isteği nde bulunma

<iframe height='500' scrolling='no' title='Arama sonuçlarını haritada gösterme' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile bir haritada Kalem <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Göster arama sonuçlarına</a> bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur. Erişim belirteci kullanmak için kimlik doğrulama mekanizmasını ayarlar. Talimatlar için [bir harita oluşturduğunu](./map-create.md) görebilirsiniz.

İkinci kod bloğu, arama isteği nde bulunmak için bir URL oluşturur. Ayrıca, arama sonuçları için sınırları ve pimleri depolamak için iki dizi oluşturur.

Üçüncü kod [bloğu, ApI Getir'i](https://fetch.spec.whatwg.org/)kullanır. [Alma](https://fetch.spec.whatwg.org/) API'si, ilgi çekici noktaları aramak için Azure Haritalar Bulanık [arama API'sine](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) istekte bulunmak için kullanılır. Bulanık arama API bulanık girişlerin herhangi bir kombinasyonu işleyebilir. Daha sonra arama yanıtını işler ve ayrışturır ve sonuç pinlerini searchPins dizisine ekler.

Dördüncü kod [bloğu, DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfını kullanarak bir veri kaynağı nesnesi oluşturur. Kodda, kaynak nesneye arama sonuçları ekleriz. [Sembol katmanı,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) [Veri Kaynağı'na](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sarılmış nokta tabanlı verileri haritadaki semboller olarak işlemek için metin veya simgeler kullanır. Daha sonra bir sembol katmanı oluşturulur. Veri kaynağı, daha sonra haritaya eklenen sembol katmanına eklenir.

Kodun son bloğu bir [BoundingBox nesnesi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) oluşturur. Bu sonuç dizi kullanır ve daha sonra Harita'nın [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-)kullanarak harita için kamera sınırlarını ayarlar. Daha sonra sonuç pimleri işler.

Arama isteği, veri kaynağı, sembol katmanı ve kamera sınırları, sonuçların harita tamamen yüklendikten sonra görüntülendiğinden emin olmak için haritanın [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) içinde ayarlanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Arama hizmetini kullanmak için en iyi uygulamalar](how-to-use-best-practices-for-search.md)

**Bulanık Arama**hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Azure Haritalar Bulanık Arama API'si](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Bir koordinattan bilgi alma](map-get-information-from-coordinate.md)
<!-- Comment added to suppress false positive warning -->
> [!div class="nextstepaction"]
> [A'dan B'ye yönleri gösterme](map-route.md)
