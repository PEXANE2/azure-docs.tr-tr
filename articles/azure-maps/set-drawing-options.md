---
title: Çizim araçları modülü | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Web SDK 'sını kullanarak çizim seçenekleri verilerini ayarlamayı öğreneceksiniz.
author: anastasia-ms
ms.author: v-stharr
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: devx-track-javascript
ms.openlocfilehash: 82c5d87be084e85b6de9f890bd042babca9df476
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090508"
---
# <a name="use-the-drawing-tools-module"></a>Çizim araçları modülünü kullanma

Azure Haritalar Web SDK 'Sı bir *çizim araçları modülü*sağlar. Bu modül, fare veya dokunmatik ekran gibi bir giriş cihazını kullanarak haritada şekil çizmeyi ve düzenlemenizi kolaylaştırır. Bu modülün çekirdek sınıfı, [Çizim yöneticisidir](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager#setoptions-drawingmanageroptions-). Çizim Yöneticisi, haritada şekil çizmek ve düzenlemek için gereken tüm özellikleri sağlar. Doğrudan kullanılabilir ve özel bir araç çubuğu kullanıcı arabirimiyle tümleşiktir. Yerleşik [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar) sınıfını da kullanabilirsiniz. 

## <a name="loading-the-drawing-tools-module-in-a-webpage"></a>Bir Web sayfasında çizim araçları modülünü yükleme

1. Yeni bir HTML dosyası oluşturun ve [Haritayı her zamanki gibi uygulayın](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).
2. Azure haritalar çizim araçları modülünü yükleyin. Bunu iki şekilde yükleyebilirsiniz:
    - Azure haritalar Hizmetleri modülünün küresel olarak barındırılan, Azure Content Delivery Network sürümünü kullanın. Dosyanın öğesinde JavaScript ve CSS stil sayfasına başvuru ekleyin `<head>` :

        ```html
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0/atlas-drawing.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/drawing/0/atlas-drawing.min.js"></script>
        ```

    - Ya da Azure Haritalar Web SDK 'Sı kaynak kodu için çizim araçları modülünü [Azure-Maps-Drawing-Tools](https://www.npmjs.com/package/azure-maps-drawing-tools) NPM paketini kullanarak yerel olarak yükleyebilir ve sonra uygulamanızla birlikte barındırabilirsiniz. Bu paket TypeScript tanımlarını da içerir. Şu komutu çalıştırın:
    
        > **NPM yüklemesi Azure-Maps-çizim-Araçlar**
    
        Ardından, dosyanın öğesinde JavaScript ve CSS stil sayfasına bir başvuru ekleyin `<head>` :

         ```html
        <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.css" type="text/css" />
        <script src="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.js"></script>
         ```

## <a name="use-the-drawing-manager-directly"></a>Çizim yöneticisini doğrudan kullanma

Çizim araçları modülü uygulamanıza yüklendikten sonra, çizim [Yöneticisi 'ni](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager#setoptions-drawingmanageroptions-)kullanarak çizim ve Düzenle özelliklerini etkinleştirebilirsiniz. Çizim Yöneticisi seçeneklerini örneklarken veya alternatif olarak işlevini kullanarak belirtebilirsiniz `drawingManager.setOptions()` .

### <a name="set-the-drawing-mode"></a>Çizim modunu ayarlama

Aşağıdaki kod, çizim Yöneticisi 'nin bir örneğini oluşturur ve çizim **modu** seçeneğini ayarlar. 

```javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon"
});
```

Aşağıdaki kod, çizim Yöneticisi 'nin çizim modunun nasıl ayarlanacağı hakkında tamamen çalışan bir örnektir. Bir çokgen çizmeye başlamak için haritaya tıklayın.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen çizme" src="//codepen.io/azuremaps/embed/YzKVKRa/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Bodepen üzerinde Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/YzKVKRa/'>bir çokgen çizme</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>


### <a name="set-the-interaction-type"></a>Etkileşim türünü ayarlama

Çizim Yöneticisi, şekil çizmek için harita ile etkileşimde bulunmanın üç farklı yolunu destekler.

* `click` -, Fare veya dokunma tıklandığında koordinatlar eklenir.
* `freehand ` -Koordinat, fare veya dokunma haritada sürüklendiğinde eklenir. 
* `hybrid` -, Fare veya dokunma tıklandığında veya sürüklendiğinde koordinatlar eklenir.

Aşağıdaki kod, çokgen çizim modunu sağlar ve çizim etkileşiminin bağlı olması gereken çizim etkileşiminin türünü ayarlar `freehand` . 

```javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

 Bu kod örneği, haritada çokgen çizme işlevlerini uygular. Sol fare düğmesini basılı tutup, dilediğiniz gibi sürükleyerek, ücretsiz olarak sürüklemeniz yeterlidir.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Ücretsiz çizim" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Codepen 'da Azure Maps () tarafından <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>ücretsiz çizim</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> . <a href='https://codepen.io'>CodePen</a>
</iframe>


### <a name="customizing-drawing-options"></a>Çizim seçeneklerini özelleştirme

Önceki örneklerde çizim seçeneklerinin örnekleme sırasında nasıl özelleştirileceği gösterilmiştir. Ayrıca, işlevini kullanarak çizim Yöneticisi seçeneklerini ayarlayabilirsiniz `drawingManager.setOptions()` . Aşağıda, setOptions işlevini kullanarak çizim Yöneticisi için tüm seçeneklerin özelleştirilmesini test eden bir araç verilmiştir.

<br/>

<iframe height="685" title="Çizim yöneticisini özelleştirme" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'>Codepen üzerinde Azure Maps () ile <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>Şekil verileri alın</a> kalemine bakın <a href='https://codepen.io/azuremaps'>@azuremaps</a> <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini kullanmayı öğrenin:

> [!div class="nextstepaction"]
> [Çizim araç çubuğu ekleme](map-add-drawing-toolbar.md)

> [!div class="nextstepaction"]
> [Şekil verilerini alma](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Çizim olaylarına tepki verme](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Etkileşim türleri ve klavye kısayolları](drawing-tools-interactions-keyboard-shortcuts.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [Çizim Yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager)

> [!div class="nextstepaction"]
> [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar)
