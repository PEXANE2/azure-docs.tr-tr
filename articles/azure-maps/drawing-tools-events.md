---
title: Haritaya Çizim araç çubuğu ekleme | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Web SDK 'sını kullanarak bir haritaya Çizim araç çubuğu ekleme hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 12/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: cf9c79f608aa3ffd1137be41ff3348f62b890867
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198318"
---
# <a name="drawing-tool-events"></a>Çizim aracı olayları

Bir haritada çizim araçları kullanılırken, Kullanıcı haritada çizilirken belirli olaylara yanıt vermek yararlıdır. Bu tablo `DrawingManager` sınıfı tarafından desteklenen tüm olayları listeler.

| Olay | Açıklama |
|-------|-------------|
| `drawingchanged` | Bir şekildeki koordinat eklendiğinde veya değiştirildiğinde harekete geçirilir. | 
| `drawingchanging` | Bir şekil için herhangi bir önizleme koordinatı görüntülenirken tetiklenir. Örneğin, bir koordinat sürüklenirken bu olay birden çok kez harekete geçmeyecektir. | 
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

Çizim araçları olaylarını kullanan bazı yaygın senaryolar görelim.

### <a name="select-points-in-polygon-area"></a>Çokgen alanında noktaları seçin

Bu kod, Kullanıcı çizimi şekillerindeki bir olayın nasıl izleneceğini gösterir. Bu örnekte, kod çokgenler, dikdörtgenler ve dairelerin şekillerini izler. Ardından, haritadaki hangi veri noktalarının çizilen alanın içinde olduğunu belirler. `drawingcomplete` olayı, select mantığını tetiklemek için kullanılır. Select mantığındaki kod, haritadaki tüm veri noktalarında döngü yapılır. Noktanın ve çizilen şeklin alanının kesişimi olup olmadığını denetler. Bu örnek, uzamsal bir kesişim hesaplamayı gerçekleştirmek için açık kaynaklı [Turf. js](https://turfjs.org/) kitaplığını kullanır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizilmiş Çokgen alanındaki verileri seçme" src="https://codepen.io/azuremaps/embed/XWJdeja?height=500&theme-id=default&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'Da Azure haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/XWJdeja'>çizilen Çokgen alanındaki verileri seçme</a> kalemine bakın.
</iframe>

<br/>

### <a name="draw-and-search-in-polygon-area"></a>Çokgen alanında çizim ve arama

Bu kod, Kullanıcı şekli çizmeyi tamamladıktan sonra bir şeklin alanındaki ilgi alanlarını arar. Çerçevenin sağ üst köşesindeki ' kod kalemlerini Düzenle ' seçeneğine tıklayarak kodu değiştirebilir ve yürütebilirsiniz. `drawingcomplete` olayı, arama mantığını tetiklemek için kullanılır. Kullanıcı bir dikdörtgen veya Çokgen çizdiğinde, geometri içinde bir arama gerçekleştirilir. Bir daire çizildiyse, bir faiz arama noktası gerçekleştirmek için yarıçap ve orta konum kullanılır. `drawingmodechanged` olayı, kullanıcının çizim moduna ne zaman geçiş yaptığında ve bu olayın çizim tuvalini temizleyeceğini tespit etmek için kullanılır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen alanında çizim ve arama" src="https://codepen.io/azuremaps/embed/eYmZGNv?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Bkz. kalemin <a href='https://codepen.io/azuremaps/pen/eYmZGNv'>çizimi ve çokgen alanında</a> Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io'>birlikte</a>arama.
</iframe>

<br/>

### <a name="create-a-measuring-tool"></a>Ölçüm aracı oluşturma

Aşağıdaki kod, bir ölçüm aracı oluşturmak için çizim olaylarının nasıl kullanılabileceğini gösterir. `drawingchanging`, şekli çizilmek üzere izlemek için kullanılır. Kullanıcı fareyi taşırken, şeklin boyutları hesaplanır. `drawingcomplete` olay, çizdikten sonra şeklin üzerinde son bir hesaplama yapmak için kullanılır. `drawingmodechanged` olayı, kullanıcının bir çizim moduna ne zaman geçiş yaparken olduğunu tespit etmek için kullanılır. Ayrıca, `drawingmodechanged` olay çizim tuvalini temizler ve eski ölçüm bilgilerini temizler.

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
