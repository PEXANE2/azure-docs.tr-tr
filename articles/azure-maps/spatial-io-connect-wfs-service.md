---
title: Bir Web Özellik Hizmeti (WFS) hizmetine bağlanma | Microsoft Azure Haritaları
description: Bir WFS hizmetine nasıl bağlanıp Azure Haritalar web SDK'sını ve Uzamsal IO modülünü kullanarak WFS hizmetini sorgulayın.
author: philmea
ms.author: philmea
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 8b511395eb61e8845aaa11e5ca7a490dc461424d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334214"
---
# <a name="connect-to-a-wfs-service"></a>WFS hizmetine bağlanma

Web Özellik Hizmeti (WFS), Açık Jeouzamsal Konsorsiyum (OGC) tarafından tanımlanan standartlaştırılmış API'ye sahip uzamsal verileri sorgulamak için bir web hizmetidir. Uzamsal IO modülündeki sınıf, `WfsClient` geliştiricilerin bir WFS hizmetine bağlanmasını ve hizmetten gelen verileri sorgulamasını sağlar.

Aşağıdaki özellikler `WfsClient` sınıf tarafından desteklenir:

- Desteklenen `1.0.0`sürümler: `1.1.0`, , ve`2.0.0`
- Desteklenen filtre işleçleri: ikili karşılaştırmalar, mantık, matematik, değer ve `bbox`.
- İstekler yalnızca `HTTP GET` kullanılarak yapılır.
- Desteklenen operasyonlar:

    | | |
    | :-- | :-- |
    | GetYetenekleri | Geçerli WFS işlemleri ve parametreleri ile bir meta veri belgesi oluşturur |
    | GetFeature | Veri kaynağından bir özellik seçimi verir |
    | FeatureType'ı Tanımla | Desteklenen özellik türlerini verir |

## <a name="using-the-wfs-client"></a>WFS istemcisini kullanma

Uzamsal IO modülündeki sınıf, `atlas.io.ogc.WfsClient` bir WFS hizmetini sorgulamayı ve yanıtları GeoJSON nesnelerine dönüştürmeyi kolaylaştırır. Bu GeoJSON nesnesi daha sonra diğer eşleme amaçları için kullanılabilir.

Aşağıdaki kod bir WFS hizmetini sorgular ve döndürülen özellikleri haritada işler.

<br/>

<iframe height='700' scrolling='no' title='Basit WFS örneği' src='//codepen.io/azuremaps/embed/MWwvVYY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'daki<a href='https://codepen.io/azuremaps'>@azuremaps</a>Kalem <a href='https://codepen.io/azuremaps/pen/MWwvVYY/'>Basit WFS örneğine</a> bakın .
</iframe>

## <a name="supported-filters"></a>Desteklenen filtreler

WFS standardına göre ogc filtreleri kullanılır. Aşağıdaki filtreler WFS istemcisi tarafından desteklenir ve çağrılan hizmetin de bu filtreleri desteklediğini varsayarak. Özel filtre dizeleri `CustomFilter` sınıfa geçirilebilir.

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

Aşağıdaki kod, WFS istemcisi ile farklı filtrelerin kullanımını gösterir.

<br/>

<iframe height='500' scrolling='no' title= 'WFS filtre örnekleri' src='//codepen.io/azuremaps/embed/NWqvYrV/?height=500&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Kalem <a href='https://codepen.io/azuremaps/pen/NWqvYrV/'>WFS filtresi örneklerine</a> bakın .
</iframe>

## <a name="wfs-service-explorer"></a>WFS servis gezgini

Aşağıdaki kod, WFS hizmetlerini keşfetmek için WFS istemcisini kullanır. Hizmet içinde bir özellik türü katmanı seçin ve ilişkili göstergeyi görün.

<br/>

<iframe height='700' style='width: 100%;' scrolling='no' title= 'WFS servis gezgini' src='//codepen.io/azuremaps/embed/bGdrvmG/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'daki<a href='https://codepen.io/azuremaps'>@azuremaps</a>Pen <a href='https://codepen.io/azuremaps/pen/bGdrvmG/'>WFS hizmet gezginine</a> bakın .
</iframe>

CORS özellikli olmayan uç noktalarda barındırılan WFS hizmetlerine erişmek için, `proxyService` aşağıda gösterildiği gibi, CORS özellikli bir proxy hizmeti WFS istemcisinin seçeneğine aktarılabilir. 

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
> [WfsMüşteri](https://docs.microsoft.com/JavaScript/api/azure-maps-spatial-io/atlas.io.ogc.wfsclient)

> [!div class="nextstepaction"]
> [WfsServiceOptions](https://docs.microsoft.com/JavaScript/api/azure-maps-spatial-io/atlas.wfsserviceoptions)

Haritalarınıza eklemek için daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Temel operasyonlardan yararlanın](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)
