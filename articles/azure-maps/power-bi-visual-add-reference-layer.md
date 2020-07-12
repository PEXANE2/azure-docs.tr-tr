---
title: Visual Power BI Azure Maps 'a bir başvuru katmanı ekleyin | Microsoft Azure haritaları
description: Bu makalede, Power BI için Microsoft Azure haritaları görselindeki başvuru katmanını nasıl kullanacağınızı öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 06/26/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: eb6c9fed42f263ca6cfaa1ea975d31cb8f4a75cf
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86262162"
---
# <a name="add-a-reference-layer"></a>Başvuru katmanı ekleme

Başvuru katmanı özelliği, ikincil bir uzamsal veri kümesinin görsele yüklenmesini ve ek bağlam sağlamak üzere haritada yer devralmanızı sağlar. Bu veri kümesi Power BI tarafından barındırılır ve bir veya dosya uzantısına sahip bir [geojson dosyası](https://wikipedia.org/wiki/GeoJSON) olmalıdır `.json` `.geojson` .

Bir **coğrafi JSON** dosyasını başvuru katmanı olarak eklemek Için, **Biçim** bölmesine gidin, **başvuru katmanı** bölümünü genişletin ve **+ yerel dosya Ekle** düğmesine basın.

Bir GeoJSON dosyası başvuru katmanına eklendikten sonra, dosyanın adı **+ yerel dosya Ekle** düğmesinin yanında bir **X** işareti olacak şekilde görünür. Görselden verileri kaldırmak ve Power BI coğrafi json dosyasını silmek için **X** düğmesine basın.

Aşağıdaki haritada, [Colorado için 2016 görselleştirmenizdeki tracts](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Common/data/geojson), popülasyon ile renkli olarak görüntülenir.

> [!div class="mx-imgBorder"]
> ![Colorado için 2016 görselleştirmenizdeki tascts 'yi görüntüleyen bir harita, başvuru katmanı olarak popülasyon ile renklendirilir](media/power-bi-visual/reference-layer-CO-census-tract.png)

Aşağıda, **başvuru katmanı** bölümünde bulunan **Biçim** bölmesinde bulunan tüm ayarlar verilmiştir.

| Ayar              | Açıklama   |
|----------------------|---------------|
| Başvuru katmanı verileri | Harita içinde ek bir katman olarak görsele yüklenecek veri coğrafi JSON dosyası. **+ Yerel dosya Ekle** düğmesi, kullanıcının `.json` veya dosya uzantısına sahip bir geojson dosyasını seçmek için kullanabileceği bir dosya iletişim kutusu açar `.geojson` . |

> [!NOTE]
> Azure haritalar görselini Bu önizlemede, başvuru katmanı yalnızca ilk 5.000 şekil özelliklerini haritaya yükler. Bu sınır gelecekteki bir güncelleştirmede artacaktır.

## <a name="styling-data-in-a-reference-layer"></a>Başvuru katmanındaki verileri Stillendirme

Özellikler, haritada nasıl stillendirilmiş olduğunu özelleştirmek için GeoJSON dosyasındaki her özelliğe eklenebilir. Bu özellik, Azure Maps web SDK 'sında basit veri katmanı özelliğini kullanır. Daha fazla bilgi için [desteklenen stil özelliklerindeki](spatial-io-add-simple-data-layer.md#default-supported-style-properties)bu belgeye bakın. Özel simge görüntüleri, güvenlik önlemi olarak Azure Maps görseli içinde desteklenmez.

Aşağıda, görüntülenen rengini kırmızı olarak ayarlayan bir GeoJSON noktası özelliğine örnek verilmiştir.

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.13284, 47.63699]
    },
    "properties": {
        "color": "red"
    }
}
```
## <a name="next-steps"></a>Sonraki adımlar

Haritaya daha fazla bağlam ekleyin:

> [!div class="nextstepaction"]
> [Kutucuk katmanı ekleme](power-bi-visual-add-tile-layer.md)

> [!div class="nextstepaction"]
> [Gerçek zamanlı trafiği göster](power-bi-visual-show-real-time-traffic.md)
