---
title: Azure haritalar ile olayları işleme | Microsoft Docs
description: Harita olayları ile etkileşimli Web SDK eşlemesi yapma
author: jingjing-z
ms.author: jinzh
ms.date: 09/10/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 99ef5aa0ee8fa542b0aa807cc536ebfbee369e10
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74484283"
---
# <a name="interact-with-the-map"></a>Harita ile etkileşim kurma

Bu makalede haritadaki olayları ve haritanın farklı katmanlarını vurgulamak için [harita sınıfı olayları](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?#events) özelliğinin nasıl kullanılacağı gösterilir. Ayrıca, bir HTML işaretleyicisi ile etkileşim kurarken olayları vurgulamak için harita sınıfı olayları özelliğini nasıl kullanacağınızı gösterir.

## <a name="interact-with-the-map"></a>Harita ile etkileşim kurma

Aşağıdaki haritada yürütün ve sağ tarafta vurgulanan ilgili fare olaylarına bakın. JavaScript kodunu görüntülemek ve düzenlemek için **js sekmesine** tıklayabilirsiniz. Ayrıca, **birlikte Düzenle** ' yi tıklatın ve kodu CodePen ' da düzenleyebilirsiniz.

<br/>

<iframe height='600' scrolling='no' title='Harita ile etkileşim kurma – fare olayları' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Azure haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io'>birlikte, codepen</a>'da <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>harita – fare olayları ile etkileşime</a> geçme bölümüne bakın.
</iframe>

## <a name="interact-with-map-layers"></a>Harita katmanları ile etkileşim kurma

Aşağıdaki kod, sembol katmanıyla etkileşime geçerek harekete geçen olayların adını vurgular. Sembol, kabarcık, çizgi ve çokgen katmanı, hepsi aynı olay kümesini destekler. Isı haritası ve döşeme katmanları bu olayların hiçbirini desteklemez.

<br/>

<iframe height='600' scrolling='no' title='Harita-katman olayları ile etkileşim kurma' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>harita-katman olayları</a> (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından Azure Maps ile etkileşime geçen kaleme bakın.
</iframe>

## <a name="interact-with-html-marker"></a>HTML Işaretleyicisi ile etkileşim kurma

Aşağıdaki kod, bir HTML işaretine JavaScript harita olayları ekler. Ayrıca, HTML işaretleyicisi ile etkileşimde bulunana kadar harekete geçen olayların adını vurgular.

<br/>

<iframe height='500' scrolling='no' title='Map-HTML Işaretleyici olayları ile etkileşim kurma' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>harita-HTML Işaretçisi olayları ile etkileşime</a> geçen kaleme bakın.
</iframe>

Aşağıdaki tabloda desteklenen tüm harita sınıfı olayları listelenmektedir.

| Olay               | Açıklama |
|---------------------|-------------|
| `boxzoomend`        | Bir "Box Zoom" etkileşimi sona erdiğinde tetiklenir.|
| `boxzoomstart`      | Bir "Box Zoom" etkileşimi başladığında tetiklenir.|
| `click`             | Bir işaret cihazına basıldığında ve haritadaki aynı noktada serbest bırakıldığında harekete geçirilir.|
| `close`             | Popup el ile veya program aracılığıyla kapatıldığında harekete geçirilir.|
| `contextmenu`       | Farenin sağ düğmesine tıklandığında harekete geçirilir.|
| `data`              | Herhangi bir harita verisi yüklendiğinde veya değiştiğinde harekete geçirilir. |
| `dataadded`         | `DataSource`şekiller eklendiğinde harekete geçirildi.|
| `dataremoved`       | `DataSource`şekiller kaldırıldığında tetiklenir.|
| `datasourceupdated` | `DataSource` nesnesi güncelleştirilirken tetiklenir.|
| `dblclick`          | Bir işaret aygıtı, haritada aynı noktada iki kez tıklandığında harekete geçirilir.|
| `drag`              | Harita, açılan menü veya HTML işaretçisi üzerinde "kaydırmak için sürükleyin" etkileşimi sırasında sürekli olarak harekete geçirildi.|
| `dragend`           | Harita, açılan menü veya HTML işaretçisi üzerinde "kaydırmak için sürükle" etkileşimi sona erdiğinde tetiklenir.|
| `dragstart`         | Harita, açılan menü veya HTML işaretçisi üzerinde "Pan 'a sürükleyin" etkileşimi başladığında tetiklenir.|
| `error`             | Bir hata oluştuğunda harekete geçirildi.|
| `idle`              | <p>Eşleme "boşta" durumuna girmeden önce oluşturulan son çerçeveden sonra harekete geçirilir:<ul><li>Devam eden bir kamera geçişi yok.</li><li>Şu anda istenen tüm kutucuklar yüklendi.</li><li>Tüm belirme/geçiş animasyonları tamamlandı.</li></ul></p>|
| `keydown`           | Bir tuşa basıldığında harekete geçirilir.|
| `keypress`          | Bir tyrilebilir karakter (ANSI anahtarı) üreten bir anahtar basıldığında tetiklenir.|
| `keyup`             | Bir anahtar serbest bırakıldığında harekete geçirilir.|
| `layeradded`        | Haritaya bir katman eklendiğinde harekete geçirildi.|
| `layerremoved`      | Eşlemden bir katman kaldırıldığında harekete geçirilir.|
| `load`              | Tüm gerekli kaynaklar indirildikten ve haritanın ilk görsel olarak işlenmesi tamamlandıktan hemen sonra harekete geçirilir.|
| `mousedown`         | Harita içinde veya bir öğenin üstünde olduğunda bir işaret cihazına basıldığında tetiklenir.|
| `mouseenter`        | Başlangıçta harita veya bir öğe üzerinden bir işaret aygıtı taşındığında tetiklenir. |
| `mouseleave`        | Bir işaret aygıtı haritanın veya bir öğenin dışına taşındığında tetiklenir. |
| `mousemove`         | Harita veya öğe içinde bir işaret aygıtı taşındığında tetiklenir.|
| `mouseout`          | Bir nokta aygıtı haritanın Tuvalimizi bir öğeden ayrıldığında tetiklenir.|
| `mouseover`         | Bir işaret aygıtı haritanın veya bir öğenin üzerine taşındığında tetiklenir.|
| `mouseup`           | Harita içinde bir işaret aygıtı serbest bırakıldığında veya bir öğenin üstünde olduğunda tetiklenir.|
| `move`              | Bir görünümden diğerine animasyon eklenmiş bir geçiş sırasında, Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak tekrar tekrar tetiklenir.|
| `moveend`           | Eşleme, Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak bir görünümden diğerine geçişi tamamladıktan hemen sonra harekete geçirilir.|
| `movestart`         | Eşleme, Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak bir görünümden diğerine geçiş yapmaya başlamadan hemen önce tetiklenir.|
| `open`              | Açılan pencere el ile veya program aracılığıyla açıldığında harekete geçirilir.|
| `pitch`             | Her iki kullanıcı etkileşimi ya da yönteminin sonucu olarak haritanın sıklığı (eğimi) değiştiğinde harekete geçirilir.|
| `pitchend`          | Haritanın sıklığı (eğimi), Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak değiştirmeyi tamamladıktan hemen sonra harekete geçirilir.|
| `pitchstart`        | Haritanın sıklığı (eğim) her iki kullanıcı etkileşimi veya yöntemlerinin sonucu olarak bir değişikliğe başladığında tetiklenir.|
| `ready`             | Eşleme, bir program aracılığıyla ile etkileşim için hazırlanmadan önce en düşük gerekli eşleme kaynakları yüklendiğinde tetiklenir.|
| `render`            | <p>Şu sonucu olarak harita ekrana çizildiğinde tetiklenir:<ul><li>Haritanın konumunu, yakınlaştırmasını, perdesini veya yataya bir değişikliği.</li><li>Haritanın stilinde değişiklik.</li><li>`DataSource` bir kaynakta değişiklik.</li><li>Vektör kutucuğu, GeoJSON dosyası, glif veya sprite yüklemesi.</li></ul></p>|
| `resize`            | Eşleme yeniden boyutlandırılırken hemen sonra harekete geçirildi.|
| `rotate`            | "Döndürmek için sürükleyin" etkileşimi sırasında sürekli olarak harekete geçirildi.|
| `rotateend`         | "Döndürmek için sürükle" etkileşim uçlarını harekete geçirilir.|
| `rotatestart`       | "Döndürmek için sürükle" etkileşimi başladığında tetiklenir.|
| `shapechanged`      | Şekil nesnesi özelliği değiştirildiğinde harekete geçirilir.|
| `sourcedata`        | Bir kaynak veya değişiklik olmasına ait bir kutucuk da dahil olmak üzere, haritanın kaynaklarından biri yüklediğinde veya değiştiğinde harekete geçirilir. |
| `sourceadded`       | Haritaya bir `DataSource` veya `VectorTileSource` eklendiğinde harekete geçirilir.|
| `sourceremoved`     | Eşlemden bir `DataSource` veya `VectorTileSource` kaldırıldığında tetiklenir.|
| `styledata`         | Haritanın stili yüklenirken veya değiştiğinde harekete geçirilir.|
| `styleimagemissing` | Bir katman mevcut olmayan görüntü Sprite öğesinden bir görüntü yüklemeye çalıştığında tetiklenir |
| `tokenacquired`     | AAD erişim belirteci edinildiği zaman tetiklenir.|
| `touchcancel`       | Eşleme içinde bir touchcancel olayı oluştuğunda harekete geçirildi.|
| `touchend`          | Eşleme içinde bir touchEnd olayı oluştuğunda harekete geçirildi.|
| `touchmove`         | Eşleme içinde bir TouchMove olayı oluştuğunda harekete geçirildi.|
| `touchstart`        | Eşleme içinde bir touchstart olayı oluştuğunda harekete geçirildi.|
| `wheel`             | Eşleme içinde fare tekerleği olayı oluştuğunda harekete geçirildi.|
| `zoom`              | Bir yakınlaştırma düzeyinden diğerine animasyon eklenmiş bir geçiş sırasında, Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak tekrar tekrar tetiklenir.|
| `zoomend`           | Eşleme, Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak, bir yakınlaştırma düzeyinden diğerine geçişi tamamladıktan hemen sonra harekete geçirilir.|
| `zoomstart`         | Eşleme, Kullanıcı etkileşimi ya da yöntemlerinin sonucu olarak bir yakınlaştırma düzeyinden diğerine geçiş yapmaya başlamadan hemen önce tetiklenir.|


## <a name="next-steps"></a>Sonraki adımlar

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Azure haritalar Hizmetleri modülünü kullanma](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)
