---
title: Açık Jeouzamsal Konsorsiyum (OGC) harita katmanı ekle | Microsoft Azure Haritaları
description: Haritaya bir OGC harita katmanını nasıl üst üst eküle edin ve OgcMapLayer sınıfındaki farklı seçenekleri nasıl kullanacağınızı öğrenin.
author: philmea
ms.author: philmea
ms.date: 03/02/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: b753ecfc07cfb3806838f8a05dbe33ef0bb92730
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334291"
---
# <a name="add-a-map-layer-from-the-open-geospatial-consortium-ogc"></a>Açık Jeouzamsal Konsorsiyum (OGC) bir harita katmanı ekle

Sınıf, `atlas.layer.OgcMapLayer` Web Harita Hizmetleri (WMS) görüntülerini ve Web Harita Döşeme Hizmetleri (WMTS) görüntülerini haritaüzerinde kaplayabilir. WMS, OGC tarafından internet üzerinden coğrafi referanslı harita görüntüleri ne amaçla geliştirilen standart bir protokoldür. Görüntü coğrafi başvuru, bir görüntüyü coğrafi bir konuma ilişkilendirme işlemidir. WMTS aynı zamanda OGC tarafından geliştirilen standart bir protokoldür. Önceden işlenmiş ve coğrafi referanslı harita kutucukları için tasarlanmıştır.

Aşağıdaki bölümlerde `OgcMapLayer` sınıf tarafından desteklenen web haritası hizmet özellikleri sıralanır.

**Web Harita Servisi (WMS)**

- Desteklenen `1.0.0`sürümler: `1.1.0` `1.1.1`, , , ve`1.3.0`
- Hizmet, `EPSG:3857` projeksiyon sistemini desteklemeli veya yeniden projeksiyonları işlemelidir.
- GetFeatureInfo, hizmetin `EPSG:4326` yeniden projeksiyonları desteklemesini veya işlemesini gerektirir. 
- Desteklenen operasyonlar:

    | | |
    | :-- | :-- |
    | GetYetenekleri | Desteklenen özelliklerle hizmetle ilgili meta verileri alır |
    | GetMap | Belirli bir bölge için harita görüntüsü alır |
    | FeatureInfo'u Alın | Özellik `feature_info`hakkında temel verileri içeren alır |

**Web Haritası Karo Servisi (WMTS)**

- Desteklenen sürümler:`1.0.0`
- Fayans kare olmalıdır, `TileWidth == TileHeight`öyle ki .
- CRS desteklenen: `EPSG:3857` veya`GoogleMapsCompatible` 
- TileMatrix tanımlayıcısı, haritadaki yakınlaştırma düzeyine karşılık gelen bir tamsayı değeri olmalıdır. Masmavi bir haritada, yakınlaştırma `"0"` düzeyi `"22"`ile . Yani, `"0"` desteklenir, `"00"` ancak desteklenmez.
- Desteklenen operasyonlar:

    | | |
    | :-- | :-- |
    | GetYetenekleri | Desteklenen işlemleri ve özellikleri alır |
    | GetTile | Belirli bir döşeme için görüntüleri alır |

## <a name="overlay-an-ogc-map-layer"></a>OGC harita katmanını yerle bir edin

Hizmetin `url` temel URL'si veya hizmetin özelliklerini almak için sorguyu içeren tam bir URL olabilir. Sağlanan ayrıntılara bağlı olarak, WFS istemcisi hizmete başlangıçta nasıl erişeceklerini belirlemek için birkaç standart URL biçimi deneyebilir.

Aşağıdaki kod, haritaüzerinde bir OGC harita katmanının nasıl yerlebir edilebildiğini gösterir.

<br/>

<iframe height='700' scrolling='no' title='OGC Harita katmanı örneği' src='//codepen.io/azuremaps/embed/xxGLZWB/?height=700&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Pen <a href='https://codepen.io/azuremaps/pen/xxGLZWB/'>OGC Harita katmanı örneğine</a> bakın .
</iframe>

## <a name="ogc-map-layer-options"></a>OGC harita katman seçenekleri

Aşağıdaki örnek, farklı OGC harita katmanı seçeneklerini göstermektedir. Kod kalemini düzeltmek için sağ üst köşedeki kod kalemi düğmesine tıklayabilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='OGC harita katman seçenekleri' src='//codepen.io/azuremaps/embed/abOyEVQ/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'a<a href='https://codepen.io/azuremaps'>@azuremaps</a>göre Pen <a href='https://codepen.io/azuremaps/pen/abOyEVQ/'>OGC harita katmanı seçeneklerine</a> bakın .
</iframe>

## <a name="ogc-web-map-service-explorer"></a>OGC Web Harita Servisi kaşifi

Aşağıdaki araç, Web Harita Hizmetleri (WMS) ve Web Harita Döşeme Hizmetleri'nden (WMTS) görüntüleri katman olarak kaplar. Hizmetteki hangi katmanların haritada işlenmesini seçebilirsiniz. Ayrıca bu katmanlar için ilişkili göstergeleri görüntüleyebilirsiniz.

<br/>

<iframe height='750' style='width: 100%;' scrolling='no' title='OGC Web Harita Servisi kaşifi' src='//codepen.io/azuremaps/embed/YzXxYdX/?height=750&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'deki</a>Azure Haritalar 'daki<a href='https://codepen.io/azuremaps'>@azuremaps</a>Pen <a href='https://codepen.io/azuremaps/pen/YzXxYdX/'>OGC Web Harita Hizmeti kaşifine</a> bakın .
</iframe>

Proxy hizmetini kullanmak için harita ayarlarını da belirtebilirsiniz. Proxy hizmeti, CORS etkin olmayan etki alanlarında barındırılan kaynakları yüklemenize olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [OgcMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.layer.ogcmaplayer)

> [!div class="nextstepaction"]
> [OgcMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-spatial-io/atlas.ogcmaplayeroptions)

Haritalarınıza ekleyebileceğiniz kod örneklerini içeren aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [WFS hizmetine bağlanma](spatial-io-connect-wfs-service.md)

> [!div class="nextstepaction"]
> [Temel operasyonlardan yararlanın](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)
