---
title: Open Geospatial Consortium (OGC) eşleme katmanı ekleme | Microsoft Azure haritaları
description: Haritadaki bir OGC Haritası katmanını nasıl kaplayacağınızı ve Logcmaplayer sınıfında farklı seçeneklerin nasıl kullanılacağını öğrenin.
author: philmea
ms.author: philmea
ms.date: 03/02/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: b753ecfc07cfb3806838f8a05dbe33ef0bb92730
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80334291"
---
# <a name="add-a-map-layer-from-the-open-geospatial-consortium-ogc"></a>Open Geospatial Consortium eşleme katmanını ekleme (OGC)

`atlas.layer.OgcMapLayer` Sınıfı, haritadaki Web harita HIZMETLERI (WMS) Imagery ve Web harita kutucuk Hizmetleri (WMTS) Imagery ile yer alabilir. WMS, coğrafi olarak başvurulan harita görüntülerini Internet üzerinden sunmak için OGC tarafından geliştirilen standart bir protokoldür. Görüntü coğrafi başvuru, bir görüntüyü coğrafi bir konuma ilişkilendirirken yapılan işlemlerdir. WMTS Ayrıca OGC tarafından geliştirilen standart bir protokoldür. Önceden işlenmiş ve coğrafi olarak başvurulan harita kutucukları sunmak için tasarlanmıştır.

Aşağıdaki bölümlerde, `OgcMapLayer` sınıfı tarafından desteklenen Web harita hizmeti özellikleri ana hatlarıyla verilmiştir.

**Web harita hizmeti (WMS)**

- Desteklenen sürümler: `1.0.0`, `1.1.0`, `1.1.1`ve`1.3.0`
- Hizmet, `EPSG:3857` projeksiyon sistemini desteklemelidir veya yeniden projeksiyonları idare etmelidir.
- Getfeatureınfo, hizmetin yeniden tahminleri desteklemesini `EPSG:4326` veya işlemesini gerektirir. 
- Desteklenen işlemler:

    | | |
    | :-- | :-- |
    | GetCapabilities | Desteklenen yeteneklere sahip hizmet hakkındaki meta verileri alır |
    | GetMap | Belirtilen bölge için bir harita görüntüsü alır |
    | Getfeatureınfo | Özelliği `feature_info`ile ilgili temel verileri içeren alır. |

**Web Haritası kutucuk hizmeti (WMTS)**

- Desteklenen sürümler:`1.0.0`
- Kutucuklar kare olmalıdır, örneğin `TileWidth == TileHeight`.
- Desteklenen CCR 'ler `EPSG:3857` : veya`GoogleMapsCompatible` 
- TileMatrix tanımlayıcısı, eşlemedeki yakınlaştırma düzeyine karşılık gelen bir tamsayı değeri olmalıdır. Azure haritasında yakınlaştırma düzeyi ve `"0"` `"22"`arasında bir değerdir. `"0"` Bu nedenle desteklenir, ancak `"00"` desteklenmez.
- Desteklenen işlemler:

    | | |
    | :-- | :-- |
    | GetCapabilities | Desteklenen işlemleri ve özellikleri alır |
    | GetTile | Belirli bir kutucuk için canlandırın alır |

## <a name="overlay-an-ogc-map-layer"></a>OGC eşleme katmanını kaplama

`url` Hizmetin temel URL 'si veya hizmetin yeteneklerini alma sorgusuna sahıp tam URL olabilir. Belirtilen ayrıntılara bağlı olarak, WFS istemcisi hizmetin ilk olarak nasıl erişebileceğini belirleyebilmek için birkaç standart URL biçimi deneyebilir.

Aşağıdaki kod, haritada bir OGC Haritası katmanının nasıl kaplamasıyla ilgili gösterilmektedir.

<br/>

<iframe height='700' scrolling='no' title='OGC Map katman örneği' src='//codepen.io/azuremaps/embed/xxGLZWB/?height=700&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile kalem <a href='https://codepen.io/azuremaps/pen/xxGLZWB/'>ogc Haritası katman örneğine</a> bakın.
</iframe>

## <a name="ogc-map-layer-options"></a>OGC eşleme katmanı seçenekleri

Aşağıdaki örnek farklı OGC Haritası katman seçeneklerini gösterir. Kod kalemini düzenlemek için sağ üst köşedeki kod kalemi düğmesine tıklayabilirsiniz.

<br/>

<iframe height='700' scrolling='no' title='OGC eşleme katmanı seçenekleri' src='//codepen.io/azuremaps/embed/abOyEVQ/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/abOyEVQ/'>ogc Haritası katman seçeneklerine</a> bakın.
</iframe>

## <a name="ogc-web-map-service-explorer"></a>OGC Web harita hizmet Gezgini

Aşağıdaki araç, Web harita Hizmetleri (WMS) ve Web harita kutucuk Hizmetleri 'nden (WMTS) katman olarak görüntü kaplayan görünümleri. Hizmet içindeki hangi katmanların haritada işleneceğini seçebilirsiniz. Ayrıca, bu katmanların ilişkili göstergelerini de görüntüleyebilirsiniz.

<br/>

<iframe height='750' style='width: 100%;' scrolling='no' title='OGC Web harita hizmet Gezgini' src='//codepen.io/azuremaps/embed/YzXxYdX/?height=750&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/YzXxYdX/'>ogc Web Haritası hizmet Gezgini</a> ' ne bakın.
</iframe>

Ayrıca, bir proxy hizmeti kullanmak için eşleme ayarlarını belirtebilirsiniz. Proxy hizmeti, CORS 'nin etkinleştirildiği etki alanlarında barındırılan kaynakları yüklemenize olanak sağlar.

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
> [Çekirdek işlemlerden yararlanın](spatial-io-core-operations.md)

> [!div class="nextstepaction"]
> [Desteklenen veri biçimi ayrıntıları](spatial-io-supported-data-format-details.md)
