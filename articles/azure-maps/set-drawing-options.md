---
title: Azure haritalar 'da çizim araçları modülünü kullanma | Microsoft Docs
description: Azure Haritalar Web SDK 'sını kullanarak çizim seçenekleri verilerini ayarlama
author: walsehgal
ms.author: v-musehg
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 0ac9bc775798a14e6431718bc602d8ff41288c10
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75408163"
---
# <a name="use-the-drawing-tools-module"></a>Çizim araçları modülünü kullanma

Azure Haritalar Web SDK 'Sı bir *çizim araçları modülü*sağlar. Bu modül, dokunma ekranımız fare gibi bir giriş cihazını kullanarak haritada şekil çizmeyi ve düzenlemenizi kolaylaştırır. Bu modülün çekirdek sınıfı, [Çizim yöneticisidir](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-) ve haritada şekil çizmek ve düzenlemek için gereken tüm özellikleri sağlar. Çizim Yöneticisi doğrudan veya özel bir araç çubuğu kullanıcı arabirimi ile tümleştirilebilir veya yerleşik [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) sınıfını kullanabilirsiniz. 

## <a name="loading-the-drawing-tools-module-in-a-webpage"></a>Bir Web sayfasında çizim araçları modülünü yükleme

1. Yeni bir HTML dosyası oluşturun ve [Haritayı her zamanki gibi uygulayın](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).
2. Azure haritalar çizim araçları modülünü yükleyin. Bunu iki şekilde yükleyebilirsiniz:
    - Azure haritalar Hizmetleri modülünün küresel olarak barındırılan, Azure Content Delivery Network sürümünü kullanın. Dosyanın `<head>` öğesinde JavaScript ve CSS stil sayfasına başvuru ekleyin:

        ```html
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.js"></script>
        ```

    - Alternatif olarak, Azure Haritalar Web SDK 'Sı kaynak kodu için çizim araçları modülünü [Azure-Maps-çizim-araçlar](https://www.npmjs.com/package/azure-maps-drawing-tools) NPM paketini kullanarak yerel olarak yükleyin ve ardından uygulamanızla birlikte barındırın. Bu paket TypeScript tanımlarını da içerir. Şu komutu kullanın:
    
        > **NPM yüklemesi Azure-Maps-çizim-Araçlar**
    
        Ardından, dosyanın `<head>` öğesinde JavaScript ve CSS stil sayfasına bir başvuru ekleyin:

         ```html
        <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.css" type="text/css" />
        <script src="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.js"></script>
         ```

## <a name="use-the-drawing-manager-directly"></a>Çizim yöneticisini doğrudan kullanma

Çizim araçları modülü uygulamanıza yüklendikten sonra, harita içinde çizim ve düzenlenme özelliklerini etkinleştirmek için [Çizim Yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-) 'ni kullanabilirsiniz. Çizim Yöneticisi seçeneklerini örneklarken veya alternatif olarak `drawingManager.setOptions()` işlevini kullanarak belirtebilirsiniz.

### <a name="set-the-drawing-mode"></a>Çizim modunu ayarlama

Aşağıdaki kod, çizim Yöneticisi 'nin bir örneğini oluşturur ve çizim **modu** seçeneğini ayarlar. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon"
});
```

Aşağıdaki kod, çizim Yöneticisi 'nin çizim modunun nasıl ayarlanacağı hakkında tamamen çalışan bir örnektir. Bir çokgen çizmeye başlamak için haritaya tıklayın.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen çizme" src="//codepen.io/azuremaps/embed/YzKVKRa/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile <a href='https://codepen.io/azuremaps/pen/YzKVKRa/'>bir çokgen çizme</a> kalemine bakın.
</iframe>


### <a name="set-the-interaction-type"></a>Etkileşim türünü ayarlama

Çizim Yöneticisi, şekil çizmek için harita ile etkileşimde bulunmanın üç farklı yolunu destekler.

* `click` koordinatları, fare veya dokunma tıklandığında eklenir.
* `freehand ` koordinatları, fare veya dokunma haritada sürüklendiğinde eklenir. 
* `hybrid` koordinatları, fare veya dokunma tıklandığında veya sürüklendiğinde eklenir.

Aşağıdaki kod, çokgen çizim modunu sağlar ve çizim etkileşiminin `freehand`bağlı olması gereken çizim etkileşimi türünü ayarlar. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

Aşağıda, haritada bir çokgen çizmenizi sağlayan, sol fare düğmesini basılı tutarak ve sürükleyerek, bu işlevselliği uygulayan kod örneği verilmiştir. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Ücretsiz çizim" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>ücretsiz çizim</a> kalemine bakın.
</iframe>


### <a name="customizing-drawing-options"></a>Çizim seçeneklerini özelleştirme

Önceki örneklerde çizim seçeneklerinin örnekleme sırasında nasıl özelleştirileceği gösterilmiştir. Çizim Yöneticisi seçeneklerini `drawingManager.setOptions()` işlevini kullanarak da ayarlayabilirsiniz. Aşağıda, setOptions işlevini kullanarak çizim Yöneticisi için tüm seçeneklerin özelleştirilmesini test eden bir araç verilmiştir.

<br/>

<iframe height="685" title="Çizim yöneticisini özelleştirme" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) Ile <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>şekil verilerini alın</a> kalemine bakın.
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini kullanmayı öğrenin:

> [!div class="nextstepaction"]
> [Çizim araç çubuğu ekleme](map-add-drawing-toolbar.md)

> [!div class="nextstepaction"]
> [Şekil verilerini al](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Çizim olaylarına tepki verme](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Etkileşim türleri ve klavye kısayolları](drawing-tools-interactions-keyboard-shortcuts.md)

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Çizim Yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
