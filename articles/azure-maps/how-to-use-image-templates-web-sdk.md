---
title: Azure Haritalar Web SDK'daki resim şablonları | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sında HTML işaretçileri ve çeşitli katmanlarla resim şablonlarının nasıl kullanılacağını öğreneceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 8/6/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: ee8e8ee4ca64de0390b6fa34e36fb4d06348a8ac
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804818"
---
# <a name="how-to-use-image-templates"></a>Görüntü şablonlarını kullanma

Görüntüler, Azure Haritalar web SDK'sında HTML işaretçileri ve çeşitli katmanlarla kullanılabilir:

 - Sembol katmanları haritadaki noktaları bir resim simgesiyle işleyebilir. Semboller bir çizgi yolu boyunca da işlenebilir.
 - Çokgen katmanları dolgu deseni görüntüsü ile işlenebilir. 
 - HTML işaretçileri, görüntüleri ve diğer HTML öğelerini kullanarak noktaları işleyebilir.

Katmanlarla iyi performans sağlamak için, görüntüleri oluşturmadan önce harita görüntüsü sprite kaynağına yükleyin. [SymbolLayer'ın IconOptions'u,](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions)varsayılan olarak bir avuç renkte birkaç işaret görüntünün üzerine önceden yüklenir. Bu işaretleyici görüntüleri ve daha fazlası SVG şablonları olarak kullanılabilir. Bunlar özel ölçeklere sahip görüntüler oluşturmak için kullanılabilir veya müşteri birincil ve ikincil renk olarak kullanılabilir. Toplamda 42 görüntü şablonu sağlanmıştır: 27 sembol simgesi ve 15 çokgen dolgu deseni.

Görüntü şablonları `map.imageSprite.createFromTemplate` işlevi kullanılarak harita görüntüsü sprite kaynaklarına eklenebilir. Bu işlev en fazla beş parametrenin geçirilmesini sağlar;

```javascript
createFromTemplate(id: string, templateName: string, color?: string, secondaryColor?: string, scale?: number): Promise<void>
```

Oluşturduğunuz `id` benzersiz bir tanımlayıcıdır. Harita `id` görüntüsüne eklendiğinde görüntüye atanır. Hangi görüntü kaynağının işleneğini belirtmek için katmanlarda bu tanımlayıcıyı kullanın. Hangi `templateName` görüntü şablonunun kullanılacağını belirtir. Seçenek `color` görüntünün birincil rengini ayarlar `secondaryColor` ve seçenekler görüntünün ikincil rengini ayarlar. Seçenek, `scale` görüntü sprite'ına uygulamadan önce görüntü şablonuna ölçeklenir. Görüntü sprite'a uygulandığında, png'ye dönüştürülür. Net görüntüleme sağlamak için, görüntüyü bir katmanhalinde ölçeklendirmek yerine, sprite eklemeden önce görüntü şablonuna ölçeklendirmek daha iyidir.

Bu işlev, görüntüyü eş senkronize bir şekilde görüntü sprite yükler. Böylece, bu işlevin tamamlanmasını bekleyebileceğiniz bir Söz döndürür.

Aşağıdaki kod, yerleşik şablonlardan birinden bir görüntünün nasıl oluşturulup bir sembol katmanıyla nasıl kullanılacağını gösterir.

```javascript
map.imageSprite.createFromTemplate('myTemplatedIcon', 'marker-flat', 'teal', '#fff').then(function () {

    //Add a symbol layer that uses the custom created icon.
    map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            image: 'myTemplatedIcon'
        }
    }));
});
```

## <a name="use-an-image-template-with-a-symbol-layer"></a>Sembol katmanı içeren bir resim şablonu kullanma

Bir görüntü şablonu harita görüntüsüne yüklendikten sonra, görüntü kaynağı kimliğine `image` ". `iconOptions`

Aşağıdaki örnekte, `marker-flat` bir teal birincil renk ve beyaz ikincil renk ile görüntü şablonu kullanarak bir sembol katmanı görüntüler. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Yerleşik simge şablonu içeren simge katmanı" src="//codepen.io/azuremaps/embed/VoQMPp/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre yerleşik<a href='https://codepen.io/azuremaps'>@azuremaps</a>simge şablonu içeren Kalem Simgesi <a href='https://codepen.io/azuremaps/pen/VoQMPp/'>katmanına</a> bakın .
</iframe>

## <a name="use-an-image-template-along-a-lines-path"></a>Çizgi yolu boyunca görüntü şablonu kullanma

Bir görüntü şablonu harita görüntüsü sprite yüklendiğinde, bir veri kaynağına lineString ekleyerek ve bir `lineSpacing`seçenek ile bir sembol katmanı kullanarak ve th `image` `iconOptions`seçeneğinde görüntü kaynağının kimliğine başvurarak bir çizgi yolu boyunca işlenebilir . 

Aşağıdaki örnek haritaüzerinde pembe bir çizgi işler ve bir `car` dodger mavi birincil renk ve beyaz ikincil renk ile görüntü şablonu kullanarak bir sembol katmanı kullanır. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Yerleşik simge şablonu yla çizgi katmanı" src="//codepen.io/azuremaps/embed/KOQvJe/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre yerleşik<a href='https://codepen.io/azuremaps'>@azuremaps</a>simge şablonu olan Kalem Hattı <a href='https://codepen.io/azuremaps/pen/KOQvJe/'>katmanına</a> bakın .
</iframe>

> [!TIP]
> Resim şablonu işaret ederse, `rotation` satırla aynı yönde işaret etmesini istiyorsanız sembol katmanısimge seçeneğini 90'a ayarlayın.

## <a name="use-an-image-template-with-a-polygon-layer"></a>Çokgen katmanı olan bir resim şablonu kullanma

Bir görüntü şablonu harita görüntüsüse yüklendikten sonra, katman `fillPattern` seçeneğinde görüntü kaynağı kimliğine başvurarak çokgen katmanında dolgu deseni olarak işlenebilir.

Aşağıdaki örnek, resim şablonunu `dot` kırmızı birincil renk ve saydam ikincil renkle kullanarak çokgen katmanı oluşturur.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgeni yerleşik simge şablonuyla doldurun" src="//codepen.io/azuremaps/embed/WVMEmz/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'da yerleşik simge şablonu yla Kalem <a href='https://codepen.io/azuremaps/pen/WVMEmz/'>Dolgu çokgenine</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

> [!TIP]
> Dolgu desenlerinin ikincil renginin ayarlanması, temel eşenin birincil deseni sağlamaya devam edeceğini görmenizi kolaylaştırır. 

## <a name="use-an-image-template-with-an-html-marker"></a>HTML işaretçisi olan bir resim şablonu kullanma

Görüntü şablonu `altas.getImageTemplate` işlevi kullanılarak alınabilir ve html işaretçisinin içeriği olarak kullanılabilir. Şablon işaretçi `htmlContent` seçeneğine geçirilebilir ve daha sonra `color`, `secondaryColor`, `text` ve seçenekleri kullanılarak özelleştirilebilir.

Aşağıdaki örnekte `marker-arrow` şablonkırmızı birincil renk, pembe ikincil renk ve "00" metin değeri kullanır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Yerleşik simge şablonu ile HTML İşaretçisi" src="//codepen.io/azuremaps/embed/EqQvzq/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
CodePen'de Azure Haritalar 'a göre yerleşik<a href='https://codepen.io/azuremaps'>@azuremaps</a>simge şablonu bulunan Kalem <a href='https://codepen.io/azuremaps/pen/EqQvzq/'>HTML İşaretçisi</a> 'ne bakın . <a href='https://codepen.io'>CodePen</a>
</iframe>

## <a name="create-custom-reusable-templates"></a>Özel yeniden kullanılabilir şablonlar oluşturma

Uygulamanız farklı simgelerle aynı simgeyi kullanıyorsa veya ek resim şablonları ekleyen bir modül oluşturuyorsanız, bu simgeleri Azure Haritalar web SDK'sından kolayca ekleyebilir ve alabilirsiniz. `atlas` Ad alanında aşağıdaki statik işlevleri kullanın.

| Adı | Dönüş Türü | Açıklama | 
|-|-|-|
| `addImageTemplate(templateName: string, template: string, override: boolean)` | | Atlas ad alanına özel bir SVG görüntü şablonu ekler. |
|  `getImageTemplate(templateName: string, scale?: number)`| string | Ada göre bir SVG şablonu alır. |
| `getAllImageTemplateNames()` | dize[] |  Ada göre bir SVG şablonu alır. |

SVG görüntü şablonları aşağıdaki yer tutucu değerlerini destekler:

| Yer tutucu | Açıklama |
|-|-|
| `{color}` | Birincil renk. | 
| `{secondaryColor}` | İkincil renk. | 
| `{scale}` | SVG görüntüsü, harita görüntüsüne eklendiğinde png görüntüye dönüştürülür. Bu yer tutucu, şablonun net bir şekilde işlediğinden emin olmak için dönüştürülmeden önce ölçeklendirmek için kullanılabilir. | 
| `{text}` | HTML İşaretçisi ile kullanıldığında metni işletecek konum. |

Aşağıdaki örnek, bir SVG şablonu nasıl alınıp yeniden kullanılabilir bir simge şablonu olarak Azure Haritalar web SDK'sına nasıl ekleyeceğinizgösterilmektedir. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Atlas namespace'e özel simge şablonu ekleme" src="//codepen.io/azuremaps/embed/NQyvEX/?height=500&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
CodePen'de Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>ile atlas <a href='https://codepen.io/azuremaps/pen/NQyvEX/'>namespace'e</a> Özel Ekle simgesi <a href='https://codepen.io'>şablonuna</a>bakın.
</iframe>

## <a name="list-of-image-templates"></a>Resim şablonları listesi

Bu tablo, Şu anda Azure Haritalar web SDK'sında bulunan tüm resim şablonlarını listeler. Şablon adı her görüntünün üzerindedir. Varsayılan olarak, birincil renk mavi ve ikincil renk beyazdır. İkincil rengin beyaz arka planda daha kolay görülmesini sağlamak için, aşağıdaki resimlerde ikincil renk siyaha ayar yapılır.

**Sembol simgesi şablonları**

|||||
|:-:|:-:|:-:|:-:|
| Işaretleyici | marker kalınlığında | işaretleyici çemberi | marker-düz |
|![işaretsimgesi](./media/image-templates/marker.png)|![işaretçi kalınlığında simge](./media/image-templates/marker-thick.png)|![işaretleyici-daire simgesi](./media/image-templates/marker-circle.png)|![işaretleyici-düz simgesi](./media/image-templates/marker-flat.png)|
||||
| işaretçi-kare | işaretçi-kare-küme | işaretçi-ok | marker-ball-pin | 
|![işaretçi-kare simgesi](./media/image-templates/marker-square.png)|![işaretçi-kare-küme simgesi](./media/image-templates/marker-square-cluster.png)|![işaretçi-ok simgesi](./media/image-templates/marker-arrow.png)|![işaretleyici-top-pin simgesi](./media/image-templates/marker-ball-pin.png)|
||||
| işaretçi-kare yuvarlak | işaretçi-kare-yuvarlak-küme | flag | bayrak üçgeni |
| ![işaretçi-kare yuvarlak simgesi](./media/image-templates/marker-square-rounded.png) | ![işaretçi-kare-yuvarlak küme simgesi](./media/image-templates/marker-square-rounded-cluster.png) | ![bayrak simgesi](./media/image-templates/flag.png) | ![bayrak üçgeni simgesi](./media/image-templates/flag-triangle.png) |
||||
| üçgen | üçgen kalınlığında | üçgen ok yukarı | üçgen ok-sol |
| ![üçgen simgesi](./media/image-templates/triangle.png) | ![üçgen kalınlığında simge](./media/image-templates/triangle-thick.png) | ![üçgen ok yukarı simgesi](./media/image-templates/triangle-arrow-up.png) | ![üçgen ok-sol simgesi](./media/image-templates/triangle-arrow-left.png) |
||||
| Altıgen | altıgen kalınlığında | altıgen yuvarlak | altıgen yuvarlak-kalın |
| ![altıgen simgesi](./media/image-templates/hexagon.png) | ![altıgen kalınlığında simge](./media/image-templates/hexagon-thick.png) | ![altıgen yuvarlak simgesi](./media/image-templates/hexagon-rounded.png) | ![altıgen yuvarlak-kalın simge](./media/image-templates/hexagon-rounded-thick.png) |
||||
| sabitleme | pin-yuvarlak | yuvarlak kare | yuvarlak kare kalınlığında |
| ![raptiye simgesi](./media/image-templates/pin.png) | ![pin-yuvarlak simgesi](./media/image-templates/pin-round.png) | ![yuvarlak kare simgesi](./media/image-templates/rounded-square.png) | ![yuvarlak kare kalınlığında simge](./media/image-templates/rounded-square-thick.png) |
||||
| ok yukarı | ok-yukarı ince | car ||
| ![ok yukarı simgesi](./media/image-templates/arrow-up.png) | ![ok-up-ince simgesi](./media/image-templates/arrow-up-thin.png) | ![araba simgesi](./media/image-templates/car.png) | |

**Çokgen dolgu deseni şablonları**

|||||
|:-:|:-:|:-:|:-:|
| Denetleyicisi | dama döndürülmüş | Daire | daireler aralıklı |
| ![denetleyici simgesi](./media/image-templates/checker.png) | ![dama döndürülmüş simgesi](./media/image-templates/checker-rotated.png) | ![çevreler simgesi](./media/image-templates/circles.png) | ![daireler aralıklı simge](./media/image-templates/circles-spaced.png) |
|||||
| diyagonal çizgiler-yukarı | diyagonal çizgiler-aşağı | diyagonal çizgili-yukarı | diyagonal çizgili-aşağı |
| ![diyagonal çizgiler-yukarı simgesi](./media/image-templates/diagonal-lines-up.png) | ![diyagonal çizgiler aşağı simgesi](./media/image-templates/diagonal-lines-down.png) | ![diyagonal çizgili-yukarı simgesi](./media/image-templates/diagonal-stripes-up.png) | ![diyagonal çizgili-aşağı simgesi](./media/image-templates/diagonal-stripes-down.png) |
|||||
| ızgara çizgileri | döndürülmüş ızgara çizgileri | döndürülmüş ızgara şeritleri | x-dolgu |
| ![ızgara çizgileri simgesi](./media/image-templates/grid-lines.png) | ![döndürülmüş ızgara çizgileri simgesi](./media/image-templates/rotated-grid-lines.png) | ![döndürülmüş ızgara çizgili simgesi](./media/image-templates/rotated-grid-stripes.png) | ![x dolgu simgesi](./media/image-templates/x-fill.png) |
|||||
| zig-zag | zig-zag-dikey | Nokta |  |
| ![zig-zag simgesi](./media/image-templates/zig-zag.png) | ![zig-zag-dikey simge](./media/image-templates/zig-zag-vertical.png) | ![nokta simgesi](./media/image-templates/dots.png) | |

## <a name="try-it-now-tool"></a>Şimdi aracı deneyin

Aşağıdaki araçla, farklı yerleşik görüntü şablonlarını çeşitli şekillerde işleyebilir ve birincil ve ikincil renkleri ve ölçeği özelleştirebilirsiniz.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Simge şablonseçenekleri" src="//codepen.io/azuremaps/embed/NQyaaO/?height=500&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre Kalem <a href='https://codepen.io/azuremaps/pen/NQyaaO/'>Simgesi şablonu seçeneklerine</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [GörüntüSpriteManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagespritemanager)

> [!div class="nextstepaction"]
> [atlas isim alanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-maps-typescript-latest#functions
)

Resim şablonlarının kullanılabildiği daha fazla kod örneği için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](map-add-pin.md)

> [!div class="nextstepaction"]
> [Çizgi katmanı ekleme](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Çokgen katmanı ekleme](map-add-shape.md)

> [!div class="nextstepaction"]
> [HTML Üreticileri Ekle](map-add-bubble-layer.md)
