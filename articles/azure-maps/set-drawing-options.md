---
title: Çizim araçları modülü | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Web SDK 'sını kullanarak çizim seçenekleri verilerini ayarlamayı öğreneceksiniz.
author: farah-alyasari
ms.author: v-faalya
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: f3634149b744b9a03f0ed89aafbc20932701bdbc
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77208191"
---
# <a name="use-the-drawing-tools-module"></a>Çizim araçları modülünü kullanma

Azure Haritalar Web SDK 'Sı bir *çizim araçları modülü*sağlar. Bu modül, fare veya dokunmatik ekran gibi bir giriş cihazını kullanarak haritada şekil çizmeyi ve düzenlemenizi kolaylaştırır. Bu modülün çekirdek sınıfı, [Çizim yöneticisidir](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-). Çizim Yöneticisi, haritada şekil çizmek ve düzenlemek için gereken tüm özellikleri sağlar. Doğrudan kullanılabilir ve özel bir araç çubuğu kullanıcı arabirimiyle tümleşiktir. Yerleşik [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) sınıfını da kullanabilirsiniz. 

## <a name="loading-the-drawing-tools-module-in-a-webpage"></a>Bir Web sayfasında çizim araçları modülünü yükleme

1. Yeni bir HTML dosyası oluşturun ve [Haritayı her zamanki gibi uygulayın](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).
2. Azure haritalar çizim araçları modülünü yükleyin. Bunu iki şekilde yükleyebilirsiniz:
    - Azure haritalar Hizmetleri modülünün küresel olarak barındırılan, Azure Content Delivery Network sürümünü kullanın. Dosyanın `<head>` öğesinde JavaScript ve CSS stil sayfasına başvuru ekleyin:

        ```html
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/drawing/0.1/atlas-drawing.min.js"></script>
        ```

    - Ya da Azure Haritalar Web SDK 'Sı kaynak kodu için çizim araçları modülünü [Azure-Maps-Drawing-Tools](https://www.npmjs.com/package/azure-maps-drawing-tools) NPM paketini kullanarak yerel olarak yükleyebilir ve sonra uygulamanızla birlikte barındırabilirsiniz. Bu paket TypeScript tanımlarını da içerir. Şu komutu kullanın:
    
        > **NPM yüklemesi Azure-Maps-çizim-Araçlar**
    
        Ardından, dosyanın `<head>` öğesinde JavaScript ve CSS stil sayfasına bir başvuru ekleyin:

         ```html
        <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.css" type="text/css" />
        <script src="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.js"></script>
         ```

## <a name="use-the-drawing-manager-directly"></a>Çizim yöneticisini doğrudan kullanma

Çizim araçları modülü uygulamanıza yüklendikten sonra, çizim [Yöneticisi 'ni](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-)kullanarak çizim ve Düzenle özelliklerini etkinleştirebilirsiniz. Çizim Yöneticisi seçeneklerini örneklarken veya alternatif olarak `drawingManager.setOptions()` işlevini kullanarak belirtebilirsiniz.

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

 Bu kod örneği, haritada çokgen çizme işlevlerini uygular. Sol fare düğmesini basılı tutup, dilediğiniz gibi sürükleyerek, ücretsiz olarak sürüklemeniz yeterlidir.

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
