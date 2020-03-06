---
title: Open Geospatial Consortium (OGC) eşleme katmanı ekleme | Microsoft Azure haritaları
description: Haritadaki bir OGC Haritası katmanını nasıl kaplayacağınızı ve Logcmaplayer sınıfında farklı seçeneklerin nasıl kullanılacağını öğrenin.
author: farah-alyasari
ms.author: v-faalya
ms.date: 03/02/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: c59376344d2df7e9e9c76e630a4462b26343d187
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371018"
---
# <a name="add-a-map-layer-from-the-open-geospatial-consortium-ogc"></a>Open Geospatial Consortium eşleme katmanını ekleme (OGC)

`atlas.layer.OgcMapLayer` sınıfı, haritadaki Web eşleme Hizmetleri (WMS) Imagery ve Web eşleme kutucuk Hizmetleri (WMTS) Imagery 'yi kaplamış olabilir. WMS, coğrafi olarak başvurulan harita görüntülerini Internet üzerinden sunmak için OGC tarafından geliştirilen standart bir protokoldür. Görüntü coğrafi başvuru, bir görüntüyü coğrafi bir konuma ilişkilendirirken yapılan işlemlerdir. WMTS Ayrıca OGC tarafından geliştirilen standart bir protokoldür. Önceden işlenmiş ve coğrafi olarak başvurulan harita kutucukları sunmak için tasarlanmıştır.

Aşağıdaki bölümlerde, `OgcMapLayer` sınıfı tarafından desteklenen Web eşleme hizmeti özellikleri ana hatlarıyla verilmiştir.

**Web eşleme hizmeti (WMS)**

- Desteklenen sürümler: `1.0.0`, `1.1.0`, `1.1.1`ve `1.3.0`
- Hizmet, `EPSG:3857` projeksiyon sistemini desteklemelidir veya hizmetin yeniden tahminleri işleyebilmelidir.
- Getfeatureınfo, hizmetin `EPSG:4326` desteklemesini veya yeniden projeksiyonlarını işlemesini gerektirir. 
- Desteklenen işlemler:

    | | |
    | :-- | :-- |
    | GetCapabilities | Desteklenen yeteneklere sahip hizmet hakkındaki meta verileri alır |
    | GetMap | Belirtilen bölge için bir harita görüntüsü alır |
    | Getfeatureınfo | Özelliği hakkında temel alınan verileri içeren `feature_info`alır. |

**Web eşleme kutucuk hizmeti (WMTS)**

- Desteklenen sürümler: `1.0.0`
- Kutucuklar, `TileWidth == TileHeight`gibi kare olmalıdır.
- Desteklenen CCR 'ler: `EPSG:3857` veya `GoogleMapsCompatible` 
- TileMatrix tanımlayıcısı, eşlemedeki yakınlaştırma düzeyine karşılık gelen bir tamsayı değeri olmalıdır. Azure haritasında, yakınlaştırma düzeyi `"0"` ve `"22"`arasında bir değerdir. Bu nedenle `"0"` desteklenir, ancak `"00"` desteklenmez.
- Desteklenen işlemler:

    | | |
    | :-- | :-- |
    | GetCapabilities | Desteklenen işlemleri ve özellikleri alır |
    | GetTile | Belirli bir kutucuk için canlandırın alır |

## <a name="overlay-an-ogc-map-layer"></a>OGC eşleme katmanını kaplama

`url` hizmetin temel URL 'SI veya hizmetin yeteneklerini alma sorgusuna sahip tam URL olabilir. Belirtilen ayrıntılara bağlı olarak, WFS istemcisi hizmetin ilk olarak nasıl erişebileceğini belirleyebilmek için birkaç standart URL biçimi deneyebilir.

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

<iframe height='700' scrolling='no' title='OGC Web harita hizmet Gezgini' src='//codepen.io/azuremaps/embed/YzXxYdX/?height=700&theme-id=0&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından bulunan kalem <a href='https://codepen.io/azuremaps/pen/YzXxYdX/'>ogc Web Haritası hizmet Gezgini</a> ' ne bakın.
</iframe>

Ayrıca, bir proxy hizmeti kullanmak için eşleme ayarlarını belirtebilirsiniz. Proxy hizmeti, CORs 'nin etkinleştirildiği etki alanlarında barındırılan kaynakları yüklemenize olanak sağlar.

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
