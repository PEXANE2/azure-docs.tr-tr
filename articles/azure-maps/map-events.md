---
title: Harita olaylarını işleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar web SDK'sını kullanarak harita olaylarıyla etkileşimli bir Web SDK haritası nın nasıl yapılacağını öğreneceksiniz.
author: jinzh-azureiot
ms.author: jinzh
ms.date: 09/10/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b97371d1b63ad4abfe1635e426df1449ab5f3f14
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534907"
---
# <a name="interact-with-the-map"></a>Haritayla etkileşim

Bu makalede, harita [olayları sınıfnasıl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?#events)kullanılacağını gösterir. Özellik, haritadaki ve haritanın farklı katmanlarındaki olayları vurgular. Bir HTML işaretçisiyle etkileşimde de olduğunuzda olayları vurgulayabilirsiniz.

## <a name="interact-with-the-map"></a>Haritayla etkileşim

Aşağıdaki haritayla oynayın ve sağdaki ilgili fare olaylarını görün. JavaScript kodunu görüntülemek ve görüntülemek için **JS sekmesine** tıklayabilirsiniz. CodePen'deki kodu değiştirmek için **CodePen'de Edit'e** de tıklayabilirsiniz.

<br/>

<iframe height='600' scrolling='no' title='Harita ile etkileşim – fare olayları' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a> <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>Kalem Le Etkileşimi 'ne</a> bakın – Azure Haritalar'a göre fare olayları (<a href='https://codepen.io/azuremaps'>@azuremaps</a>)
</iframe>

## <a name="interact-with-map-layers"></a>Harita katmanlarıyla etkileşim

Aşağıdaki kod, Sembol Katmanı ile etkileşimde olarak ateşlenen olayı vurgular. Sembol, kabarcık, çizgi ve çokgen katmanı aynı olaylar kümesini destekler. Isı haritası ve kiremit katmanları bu olayların hiçbirini desteklemez.

<br/>

<iframe height='600' scrolling='no' title='Harita ile etkileşim – Katman Olayları' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a> <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>Haritayla Etkileşimde Olan</a> Kaleme<a href='https://codepen.io/azuremaps'>@azuremaps</a>Bakın – Azure Haritalarına Göre Katman Olayları ( )
</iframe>

## <a name="interact-with-html-marker"></a>HTML İşaretçisi ile etkileşim

Aşağıdaki kod, bir HTML işaretçisine Javascript harita olaylarını ekler. Ayrıca, HTML işaretçisi ile etkileşimde olarak ateşlenen olayların adını vurgular.

<br/>

<iframe height='500' scrolling='no' title='Harita ile etkileşim - HTML İşaretçi olaylar' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a> <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>Haritayla Etkileşimde Olan</a> Kaleme<a href='https://codepen.io/azuremaps'>@azuremaps</a>Bakın - Azure Haritalar'a göre HTML İşaretleyici olayları ( )
</iframe>

Aşağıdaki tabloda desteklenen tüm harita sınıfı olayları listelenir.

| Olay               | Açıklama |
|---------------------|-------------|
| `boxzoomend`        | Bir "kutu yakınlaştırma" etkileşimi sona erdiğinde ateşlendi.|
| `boxzoomstart`      | Bir "kutu yakınlaştırma" etkileşimi başladığında ateşlendi.|
| `click`             | Haritaüzerinde aynı noktada bir işaretleme aygıtı basıldığında ve serbest bırakıldığında ateşlenir.|
| `close`             | Açılır pencere el ile veya programlı olarak kapatıldığında ateşlenir.|
| `contextmenu`       | Farenin sağ düğmesine tıklandığında ateşlenir.|
| `data`              | Herhangi bir harita verisi yüklendiğinde veya değiştiğinde ateşlenir. |
| `dataadded`         | Şekiller eklendiğinde `DataSource`ateşlenir.|
| `dataremoved`       | Şekiller kaldırıldığında `DataSource`ateşlenir.|
| `datasourceupdated` | `DataSource` Nesne güncelleştirildiğinde ateşlenir.|
| `dblclick`          | Haritadaki aynı noktada bir işaretleme aygıtı iki kez tıklandığında ateşlenir.|
| `drag`              | Harita, açılır pencere veya HTML işaretçisi üzerindeki "sürükle kaydırma" etkileşimi sırasında art arda ateşlenir.|
| `dragend`           | Haritada, açılır pencerede veya HTML işaretçisi üzerinde "sürükle kaydırma" etkileşimi sona erdiğinde ateşlendi.|
| `dragstart`         | Haritada, açılır pencerede veya HTML işaretçisi üzerinde "sürükle kaydırma" etkileşimi başladığında ateşlendi.|
| `error`             | Bir hata oluştuğunda ateşlendi.|
| `idle`              | <p>Harita "boşta" durumuna girmeden önce işlenen son kareden sonra ateşlenir:<ul><li>Kamera geçişi devam ediyor.</li><li>Şu anda istenen tüm kutucuklar yüklendi.</li><li>Tüm soluk/geçiş animasyonları tamamlandı.</li></ul></p>|
| `keydown`           | Bir tuşa basıldığında ateşlenir.|
| `keypress`          | Tiplenebilir bir karakter (ANSI tuşu) üreten bir tuşa basıldığında ateşlenir.|
| `keyup`             | Bir anahtar serbest bırakıldığında ateşlenmiş.|
| `layeradded`        | Haritaya bir katman eklendiğinde ateşlenir.|
| `layerremoved`      | Bir katman haritadan çıkarıldığında ateşlenir.|
| `load`              | Gerekli tüm kaynaklar indirildikten ve haritanın görsel olarak tamamlanmış ilk işlemesinin ardından hemen ateşlendi.|
| `mousedown`         | Harita içinde veya bir öğenin üstüne bir işaretleme aygıtı basıldığında ateşlenir.|
| `mouseenter`        | Bir işaretleme aygıtı başlangıçta harita veya öğe üzerinde hareket ettirildiğinde ateşlenir. |
| `mouseleave`        | İşaretleme aygıtı haritadan veya öğeden taşındığında ateşlenir. |
| `mousemove`         | Bir işaretleme aygıtı harita veya öğe içinde hareket ettirildiğinde ateşlenir.|
| `mouseout`          | Bir nokta cihazı haritanın tuvalinden ayrıldığında ateşlenir, bizim bir element bırakır.|
| `mouseover`         | Bir işaretleme aygıtı harita veya öğe üzerinde hareket ettirildiğinde ateşlenir.|
| `mouseup`           | Harita içinde veya bir öğenin üstünde bir işaretleme aygıtı serbest bırakıldığında ateşlenir.|
| `move`              | Kullanıcı etkileşimi veya yöntemleri nin sonucu olarak, bir görünümden diğerine animasyonlu bir geçiş sırasında art arda ateşlenir.|
| `moveend`           | Kullanıcı etkileşimi veya yöntemleri nin sonucu olarak, harita bir görünümden diğerine geçişi tamamladıktan hemen sonra ateşlendi.|
| `movestart`         | Kullanıcı etkileşimi veya yöntemleri nin sonucu olarak, harita bir görünümden diğerine geçiş başlamadan hemen önce ateşlendi.|
| `open`              | Açılır pencere el ile veya programlı olarak açıldığında ateşlenir.|
| `pitch`             | Kullanıcı etkileşimi veya yöntemleri sonucunda haritanın eğimi (eğimi) değiştiğinde ateşlenen ateş.|
| `pitchend`          | Haritanın eğimi (eğimi) kullanıcı etkileşimi veya yöntemleri sonucunda değişen bitirdikten hemen sonra ateşlendi.|
| `pitchstart`        | Haritanın eğimi (eğimi) kullanıcı etkileşimi veya yöntemleri sonucunda bir değişiklik başladığında ateşlenen.|
| `ready`             | Harita programlanabilir şekilde etkileşime girmeye hazır olmadan önce gerekli en az harita kaynağı yüklendiğinde ateşlenir.|
| `render`            | <p>Harita ekrana çizildiğinde aşağıdakiler sonucu olarak ateşlenir:<ul><li>Haritanın konumu, yakınlaştırma, eğim veya rulman değişikliği.</li><li>Haritanın stilinde bir değişiklik.</li><li>Bir `DataSource` kaynağa geçiş.</li><li>Vektör döşemesi, GeoJSON dosyası, glyph veya sprite yükleme.</li></ul></p>|
| `resize`            | Harita yeniden boyutlandıktan hemen sonra ateşlendi.|
| `rotate`            | "Döndürmek için sürükleme" etkileşimi sırasında defalarca ateşlenir.|
| `rotateend`         | "Döndürmek için sürükleme" etkileşimi sona erdiğinde ateşlendi.|
| `rotatestart`       | "Döndürmek için sürükleme" etkileşimi başladığında ateşlendi.|
| `shapechanged`      | Şekil nesnesi özelliği değiştirildiğinde ateşlenir.|
| `sourcedata`        | Bir kaynağa ait bir kiremit yüklendiğinde veya değişse de dahil olmak üzere, haritanın kaynaklarından biri yüklendiğinde veya değiştiğinde ateşlenir. |
| `sourceadded`       | Haritaya `DataSource` bir `VectorTileSource` veya eklendiğinde ateşlenir.|
| `sourceremoved`     | A `DataSource` veya `VectorTileSource` haritadan çıkarıldığında ateşlenir.|
| `styledata`         | Haritanın stili yüklendiğinde veya değiştiğinde ateşlenir.|
| `styleimagemissing` | Bir katman, var olmayan görüntü sprite'den görüntü yüklemeye çalıştığında ateşlendi |
| `tokenacquired`     | Bir AAD erişim jetonu elde edildiğinde ateşlenir.|
| `touchcancel`       | Harita içinde bir touchcancel olayı oluştuğunda ateşlendi.|
| `touchend`          | Harita içinde bir dokunmatik uç olayı oluştuğunda ateşlendi.|
| `touchmove`         | Harita içinde bir touchmove olayı oluştuğunda ateşlendi.|
| `touchstart`        | Harita içinde bir touchstart olayı oluştuğunda ateşlendi.|
| `wheel`             | Harita içinde bir fare tekerleği olayı oluştuğunda ateşlenir.|
| `zoom`              | Kullanıcı etkileşimi veya yöntemleri sonucunda, bir yakınlaştırma seviyesinden diğerine animasyonlu bir geçiş sırasında art arda ateşlenir.|
| `zoomend`           | Kullanıcı etkileşimi veya yöntemleri sonucunda, harita bir yakınlaştırma düzeyinden diğerine geçişi tamamladıktan hemen sonra ateşlendi.|
| `zoomstart`         | Kullanıcı etkileşimi veya yöntemleri nin sonucu olarak, harita bir yakınlaştırma düzeyinden diğerine geçiş başlamadan hemen önce ateşlendi.|


## <a name="next-steps"></a>Sonraki adımlar

Tam kod örnekleri için aşağıdaki makalelere bakın:

> [!div class="nextstepaction"]
> [Azure Haritalar Hizmetleri modüllerini kullanma](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Kod örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-maps)
