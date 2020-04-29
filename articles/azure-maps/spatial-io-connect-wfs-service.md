---
title: Web özellik hizmeti (WFS) hizmetine bağlanma | Microsoft Azure haritaları
description: WFS hizmetine nasıl bağlanacağınızı öğrenin ve ardından Azure Maps web SDK 'sını ve uzamsal GÇ modülünü kullanarak WFS hizmetini sorgulayın.
author: philmea
ms.author: philmea
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 8b511395eb61e8845aaa11e5ca7a490dc461424d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80334214"
---
# <a name="connect-to-a-wfs-service"></a>WFS hizmetine bağlanma

Web özellik hizmeti (WFS), Open Geospatial Consortium (OGC) tarafından tanımlanan standartlaştırılmış bir API 'ye sahip uzamsal verileri sorgulamak için bir Web hizmetidir. Uzamsal `WfsClient` GÇ modülündeki sınıf, geliştiricilerin bir WFS hizmetine bağlanmasına ve hizmetten veri sorgulayamasına olanak tanır.

Aşağıdaki özellikler `WfsClient` sınıfı tarafından desteklenir:

- Desteklenen sürümler: `1.0.0`, `1.1.0`ve`2.0.0`
- Desteklenen filtre işleçleri: ikili karşılaştırmalar, Logic, Math, değer ve `bbox`.
- İstekler yalnızca kullanılarak `HTTP GET` yapılır.
- Desteklenen işlemler:

    | | |
    | :-- | :-- |
    | GetCapabilities | Geçerli WFS işlemlerine ve parametrelerine sahip bir meta veri belgesi oluşturur |
    | GetFeature | Bir veri kaynağından özelliklerin bir seçimini döndürür |
    | DescribeFeatureType | Desteklenen özellik türlerini döndürür |

## <a name="using-the-wfs-client"></a>WFS istemcisini kullanma

Uzamsal `atlas.io.ogc.WfsClient` GÇ modülündeki sınıf bir WFS hizmetini sorgulamayı ve yanıtları geojson nesnelerine dönüştürmeyi kolaylaştırır. Bu coğrafi JSON nesnesi, diğer eşleme amaçları için kullanılabilir.

Aşağıdaki kod bir WFS hizmetini sorgular ve eşleme üzerinde döndürülen özellikleri işler.

<br/>

<iframe height='700' scrolling='no' title='Basit WFS örneği' src='//codepen.io/azuremaps/embed/MWwvVYY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/MWwvVYY/'>basit WFS örneğine</a> bakın.
</iframe>

## <a name="supported-filters"></a>Desteklenen filtreler

WFS standardına yönelik belirtim OGC filtrelerinin kullanımını sağlar. Aşağıdaki filtreler WFS istemcisi tarafından desteklenir, ancak çağrılan hizmetin da bu filtreleri desteklediği varsayılır. Özel Filtre dizeleri `CustomFilter` sınıfına geçirilebilir.

**Mantıksal işleçler**

- `And`
- `Or`
- `Not`

**Değer işleçleri**

- `GmlObjectId`
- `ResourceId`

**Matematik işleçleri**

- `Add`
- `Sub`
- `Mul`
- `Div`

**Karşılaştırma işleçleri**

- `PropertyIsEqualTo`
- `PropertyIsNotEqualTo`
- `PropertyIsLessThan`
- `PropertyIsGreaterThan`
- `PropertyIsLessThanOrEqualTo`
- `PropertyIsGreaterThanOrEqualTo`
- `PropertyIsLike`
- `PropertyIsNull`
- `PropertyIsNil`
- `PropertyIsBetween`

Aşağıdaki kod, WFS istemcisiyle farklı filtrelerin kullanımını gösterir.

<br/>

<iframe height='500' scrolling='no' title= 'WFS filtre örnekleri' src='//codepen.io/azuremaps/embed/NWqvYrV/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan Pen <a href='https://codepen.io/azuremaps/pen/NWqvYrV/'>WFS filtre örneklerine</a> bakın.
</iframe>

## <a name="wfs-service-explorer"></a>WFS hizmet Gezgini

Aşağıdaki kod WFS hizmetini araştırmak için WFS istemcisini kullanır. Hizmet içinde bir özellik türü katmanı seçin ve ilişkili göstergeyi görüntüleyin.

<br/>

<iframe height='700' style='width: 100%;' scrolling='no' title= 'WFS hizmet Gezgini' src='//codepen.io/azuremaps/embed/bGdrvmG/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan Pen <a href='https://codepen.io/azuremaps/pen/bGdrvmG/'>WFS hizmet Gezgini</a> ' ne bakın.
</iframe>

CORS özellikli olmayan uç noktalarında barındırılan WFS hizmetlerine erişmek için, aşağıda gösterildiği gibi bir CORS etkin proxy hizmeti, WFS `proxyService` istemcisi seçeneğine geçirilebilir. 

```JavaScript
//Create the WFS client to access the service and use the proxy service settings
client = new atlas.io.ogc.WfsClient({
    url: url,
    proxyService: window.location.origin + '/YourCorsEnabledProxyService.ashx?url='
});
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [WfsClient](https://docs.microsoft.com/JavaScript/api/azure-maps-spatial-io/atlas.io.ogc.wfsclient)

> [!div class="nextstepaction"]
> [WfsServiceOptions](https://docs.microsoft.com/JavaScript/api/azure-maps-spatial-io/atlas.wfsserviceoptions)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Çekirdek işlemlerden yararlanın](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)
