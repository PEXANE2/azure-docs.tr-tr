---
title: Çizim araçları modülü | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'sını kullanarak çizim seçenekleri verilerini nasıl ayarlayacağımı öğreneceksiniz
author: philmea
ms.author: philmea
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 711609f9382e2153cbc738d544933796dbbe2e99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334303"
---
# <a name="use-the-drawing-tools-module"></a>Çizim araçları modülünü kullanma

Azure Haritalar Web SDK bir *çizim araçları modülü*sağlar. Bu modül, fare veya dokunmatik ekran gibi bir giriş aygıtı kullanarak haritaüzerinde şekil çizmeyi ve yeniden oluşturmayı kolaylaştırır. Bu modülün çekirdek sınıfı [çizim yöneticisidir.](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-) Çizim yöneticisi, haritaüzerinde şekil çizmek ve bunları yapmak için gereken tüm yetenekleri sağlar. Doğrudan kullanılabilir ve özel bir araç çubuğu kullanıcı arabirimi ile entegre edilmiştir. Yerleşik [çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) sınıfını da kullanabilirsiniz. 

## <a name="loading-the-drawing-tools-module-in-a-webpage"></a>Çizim araçları modüllerini bir web sayfasına yükleme

1. Yeni bir HTML dosyası oluşturun ve [haritayı her zamanki gibi uygulayın.](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control)
2. Azure Haritalar çizim araçları modüllerini yükleyin. İki şekilde yükleyebilirsiniz:
    - Azure Haritalar hizmetleri modülünün genel olarak barındırılan Azure İçerik Dağıtım Ağı sürümünü kullanın. Dosya `<head>` öğesindeki JavaScript ve CSS stil sayfasına başvuru ekleyin:

        ```html
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/drawing/0/atlas-drawing.min.css" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/javascript/drawing/0/atlas-drawing.min.js"></script>
        ```

    - Veya Azure Haritalar Web SDK kaynak kodu için çizim araçları modüllerini [azure-haritalar çizim araçları](https://www.npmjs.com/package/azure-maps-drawing-tools) npm paketini kullanarak yerel olarak yükleyebilir ve ardından uygulamanızla barındırabilirsiniz. Bu paket, TypeScript tanımlarını da içerir. Bu komutu kullanın:
    
        > **npm yükleme azure-haritalar-çizim-araçları**
    
        Ardından, dosyanın `<head>` öğesindeki JavaScript ve CSS stil sayfasına bir başvuru ekleyin:

         ```html
        <link rel="stylesheet" href="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.css" type="text/css" />
        <script src="node_modules/azure-maps-drawing-tools/dist/atlas-drawing.min.js"></script>
         ```

## <a name="use-the-drawing-manager-directly"></a>Çizim yöneticisini doğrudan kullanma

Çizim araçları modülü uygulamanıza yüklendikten sonra, [çizim yöneticisini](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest#setoptions-drawingmanageroptions-)kullanarak çizim ve düzenleme özelliklerini etkinleştirebilirsiniz. Çizim yöneticisi için seçenekleri anında belirtirken belirtebilir veya alternatif `drawingManager.setOptions()` olarak işlevi kullanabilirsiniz.

### <a name="set-the-drawing-mode"></a>Çizim modunu ayarlama

Aşağıdaki kod çizim yöneticisinin bir örneğini oluşturur ve çizim **modu** seçeneğini ayarlar. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon"
});
```

Aşağıdaki kod, çizim yöneticisinin çizim modunu nasıl ayarlayacağine yürütülüyor. Çokgen çizmeye başlamak için haritayı tıklatın.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen çizin" src="//codepen.io/azuremaps/embed/YzKVKRa/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
CodePen'de Kalemi Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a>'a göre <a href='https://codepen.io/azuremaps/pen/YzKVKRa/'>çokgen çizin</a> ( ) <a href='https://codepen.io'>bakın.</a>
</iframe>


### <a name="set-the-interaction-type"></a>Etkileşim türünü ayarlama

Çizim yöneticisi, şekil çizmek için haritayla etkileşimkurmanın üç farklı yolunu destekler.

* `click`- Fare veya dokunma tıklatıldığında koordinatlar eklenir.
* `freehand `- Haritada fare veya dokunma sürüklendiğinde koordinatlar eklenir. 
* `hybrid`- Fare veya dokunma tıklatıldığında veya sürüklendiğinde koordinatlar eklenir.

Aşağıdaki kod çokgen çizim modunu etkinleştirir ve çizim yöneticisinin uyması `freehand`gereken çizim etkileşiminin türünü ayarlar. 

```Javascript
//Create an instance of the drawing manager and set drawing mode.
drawingManager = new atlas.drawing.DrawingManager(map,{
    mode: "draw-polygon",
    interactionType: "freehand"
});
```

 Bu kod örneği, haritaüzerinde çokgen çizme işlevini uygular. Sadece sol fare düğmesini basılı tutun ve etrafında sürükleyerek, serbestçe.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Serbest el çizimi" src="//codepen.io/azuremaps/embed/ZEzKoaj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar ()<a href='https://codepen.io/azuremaps'>@azuremaps</a>tarafından <a href='https://codepen.io/azuremaps/pen/ZEzKoaj/'>kalemserbest çizimine</a> bakın.
</iframe>


### <a name="customizing-drawing-options"></a>Çizim seçeneklerini özelleştirme

Önceki örnekler, Çizim Yöneticisi'ni anında yaparken çizim seçeneklerini nasıl özelleştireceğimi göstermiştir. `drawingManager.setOptions()` İşlevi kullanarak Çizim Yöneticisi seçeneklerini de ayarlayabilirsiniz. Aşağıda setOptions işlevini kullanarak çizim yöneticisi için tüm seçeneklerin özelleştirme test etmek için bir araçtır.

<br/>

<iframe height="685" title="Çizim yöneticisini özelleştirin" src="//codepen.io/azuremaps/embed/LYPyrxR/?height=600&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true" style='width: 100%;'><a href='https://codepen.io'>CodePen'de</a>Kalem Verisi<a href='https://codepen.io/azuremaps'>@azuremaps</a>'ne göre Şekil <a href='https://codepen.io/azuremaps/pen/LYPyrxR/'>Verilerini</a> Azure Haritaları 'na ( ) görün.
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini nasıl kullanacağınızı öğrenin:

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
> [Harita](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Çizim yöneticisi](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Çizim araç çubuğu](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)
