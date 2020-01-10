---
title: Azure haritalar 'a bir çizim araç çubuğu ekleme | Microsoft Docs
description: Azure Haritalar Web SDK 'sını kullanarak bir haritaya Çizim araç çubuğu ekleme
author: rbrundritt
ms.author: richbrun
ms.date: 12/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: 63848ecf92daecf28f6e0046e87da15355aa35c1
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75561839"
---
# <a name="drawing-tool-events"></a>Çizim aracı olayları

Bir haritada çizim araçları kullanılırken, Kullanıcı haritada çizilirken belirli olaylara yanıt vermek genellikle yararlı olur. Aşağıdaki tabloda `DrawingManager` sınıfı tarafından desteklenen tüm olaylar listelenmektedir.

| Olay | Açıklama |
|-------|-------------|
| `drawingchanged` | Bir şekildeki koordinat eklendiğinde veya değiştirildiğinde harekete geçirilir. | 
| `drawingchanging` | Bir şekil için herhangi bir önizleme koordinatı görüntülenirken tetiklenir. Örneğin, bir koordinat sürüklenirken birden çok kez harekete geçmeyecektir. | 
| `drawingcomplete` | Şekil, düzenleme modundan çekilmek veya kullanıma alınması bittiğinde tetiklenir. |
| `drawingmodechanged` | Çizim modu değiştiğinde harekete geçirilir. Yeni çizim modu olay işleyicisine geçirilir. |
| `drawingstarted` | Kullanıcı bir şekil çizmeye başladığında veya şekil düzenleme moduna geçirildiğinde tetiklenir.  |

Aşağıdaki kod, çizim araçları modülündeki olayların nasıl çalıştığını gösterir. Harita üzerinde şekiller çizin ve olayların tetikleneceği şekilde izleyin.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizim araçları olayları" src="https://codepen.io/azuremaps/embed/dyPMRWo?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/dyPMRWo'>çizimi araçları olaylarına</a> bakın.
</iframe>

<br/>

## <a name="examples"></a>Örnekler

Aşağıda, çizim araçları olaylarını kullanan bazı yaygın senaryoların örnekleri verilmiştir.

### <a name="select-points-in-polygon-area"></a>Çokgen alanında noktaları seçin

Aşağıdaki kod, çokgen alanını (çokgenler, dikdörtgenler ve daireler) temsil eden şekillerin çizimini nasıl izleyeceğinizi ve haritadaki hangi veri noktalarının çizili alanın içinde olduğunu belirlemenizi gösterir. `drawingcomplete` olayı, select mantığını tetiklemek için kullanılır. Select mantığındaki haritadaki tüm veri noktaları, çizilen şeklin Çokgen alanıyla kesişmeyle birlikte işlenir ve test edilir. Bu örnek, uzamsal bir kesişim hesaplamayı gerçekleştirmek için açık kaynaklı [Turf. js](https://turfjs.org/) kitaplığını kullanır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizilmiş Çokgen alanındaki verileri seçme" src="https://codepen.io/azuremaps/embed/XWJdeja?height=500&theme-id=default&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/XWJdeja'>çizilen Çokgen alanındaki verileri seçme</a> kalemine bakın.
</iframe>

<br/>

### <a name="draw-and-search-in-polygon-area"></a>Çokgen alanında çizim ve arama

Aşağıdaki kod, Kullanıcı şekli çizmeyi tamamladıktan sonra bir şekil alanı içindeki ilgi alanları için bir aramanın nasıl gerçekleştirileceğini gösterir. `drawingcomplete` olayı, arama mantığını tetiklemek için kullanılır. Kullanıcı bir dikdörtgen veya Çokgen çizdiğinde, geometri içinde bir arama gerçekleştirilir. Bir daire çizildiyse, bir faiz arama noktası gerçekleştirmek için yarıçap ve orta konum kullanılır. `drawingmodechanged` olayı, kullanıcının çizim moduna ne zaman geçiş yaparken belirlenmesi için kullanılır ve çizim tuvalini temizler.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen alanında çizim ve arama" src="https://codepen.io/azuremaps/embed/eYmZGNv?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Bkz. kalemin <a href='https://codepen.io/azuremaps/pen/eYmZGNv'>çizimi ve çokgen alanında</a> Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io'>birlikte</a>arama.
</iframe>

<br/>

### <a name="create-a-measuring-tool"></a>Ölçüm aracı oluşturma

Aşağıdaki kod, bir ölçüm aracı oluşturmak için çizim olaylarının nasıl kullanılabileceğini gösterir. `drawingchanging` şekli çizilmekte olduğundan izlemek için kullanılır. Kullanıcı fareyi taşırken, şeklin boyutları hesaplanır. `drawingcomplete` olay, çizdikten sonra şeklin üzerinde son bir hesaplama yapmak için kullanılır. `drawingmodechanged` olayı, kullanıcının çizim moduna ne zaman geçiş yaparken belirlenmesi için kullanılır ve çizim tuvalini ve eski ölçüm bilgilerini temizler.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Ölçüm Aracı" src="https://codepen.io/azuremaps/embed/RwNaZXe?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/RwNaZXe'>ölçme aracına</a> bakın.
</iframe>

<br/>

## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini kullanmayı öğrenin:

> [!div class="nextstepaction"]
> [Şekil verilerini al](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Etkileşim türleri ve klavye kısayolları](drawing-tools-interactions-keyboard-shortcuts.md)

Hizmetler modülü hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Hizmetler modülü](how-to-use-services-module.md)

Daha fazla kod örneği inceleyin:

> [!div class="nextstepaction"]
> [Kod örnek sayfası](https://aka.ms/AzureMapsSamples)
