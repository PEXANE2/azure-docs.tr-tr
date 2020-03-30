---
title: Haritaya çizim araç çubuğu ekleme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK'yı kullanarak haritaya çizim araç çubuğu eklemeyi öğreneceksiniz
author: rbrundritt
ms.author: richbrun
ms.date: 12/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: cf9c79f608aa3ffd1137be41ff3348f62b890867
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198318"
---
# <a name="drawing-tool-events"></a>Çizim aracı olayları

Haritaüzerinde çizim araçları kullanırken, kullanıcı haritaüzerinde çizerken belirli olaylara tepki vermek yararlıdır. Bu tablo, `DrawingManager` sınıf tarafından desteklenen tüm olayları listeler.

| Olay | Açıklama |
|-------|-------------|
| `drawingchanged` | Bir şekle herhangi bir koordinat eklendiğinde veya değiştirildiğinde ateşlenir. | 
| `drawingchanging` | Bir şeklin herhangi bir önizleme koordinatı görüntülendiğinde ateşlenir. Örneğin, bu olay bir koordinat sürüklendi olarak birden çok kez ateş edecektir. | 
| `drawingcomplete` | Bir şekil çizilmeveya edit modundan çıkarıldığında ateşlenir. |
| `drawingmodechanged` | Çizim modu değiştiğinde ateşlendi. Yeni çizim modu olay işleyicisine aktarılır. |
| `drawingstarted` | Kullanıcı bir şekil çizmeye başladığında veya bir şekli edit moduna koyduğunda ateşlenir.  |

Aşağıdaki kod, Çizim Araçları modülündeki olayların nasıl çalıştığını gösterir. Harita üzerinde şekiller çizin ve olaylar ateş olarak izleyin.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizim araçları etkinlikleri" src="https://codepen.io/azuremaps/embed/dyPMRWo?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar 'a göre Kalem <a href='https://codepen.io/azuremaps/pen/dyPMRWo'>Çizimi araçları etkinliklerine</a> bakın .<a href='https://codepen.io/azuremaps'>@azuremaps</a>
</iframe>

<br/>

## <a name="examples"></a>Örnekler

Çizim araçları olaylarını kullanan bazı yaygın senaryoları görelim.

### <a name="select-points-in-polygon-area"></a>Çokgen alanında noktaları seçin

Bu kod, bir kullanıcı çizim şekilleri olay nasıl izlenir gösterir. Bu örnekte, kod çokgenlerin, dikdörtgenlerin ve dairelerin şekillerini izler. Daha sonra, haritadaki hangi veri noktalarının çizilen alan içinde olduğunu belirler. Olay, `drawingcomplete` select mantığını tetiklemek için kullanılır. Seçme mantığında, kod haritadaki tüm veri noktaları arasında döngüler. Çizilen şeklin noktasının ve alanının kesişme noktasını kontrol eder. Bu örnek, uzamsal bir kesişim hesaplaması gerçekleştirmek için açık kaynak [Turf.js](https://turfjs.org/) kitaplığını kullanır.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çizilen çokgen alandaki verileri seçme" src="https://codepen.io/azuremaps/embed/XWJdeja?height=500&theme-id=default&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/XWJdeja'>çizilen çokgen alandaki</a> Kalem Seç verilerine bakın.
</iframe>

<br/>

### <a name="draw-and-search-in-polygon-area"></a>Çokgen alanda çizim ve arama

Bu kod, kullanıcı şekli çizmeyi bitirdikten sonra şeklin alanı içindeki ilgi alanlarını arar. Çerçevenin sağ üst köşesindeki 'Kod kaleminde Düzenleme'yi tıklatarak kodu değiştirebilir ve yürütebilirsiniz. Olay, `drawingcomplete` arama mantığını tetiklemek için kullanılır. Kullanıcı bir dikdörtgen veya çokgen çizerse, geometri içinde bir arama yapılır. Bir daire çizilirse, yarıçap ve merkez konumu ilgi alanı araması yapmak için kullanılır. Olay, `drawingmodechanged` kullanıcının çizim moduna ne zaman geçtiğini belirlemek için kullanılır ve bu olay çizim tuvalini temizler.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Çokgen alanda çizim ve arama" src="https://codepen.io/azuremaps/embed/eYmZGNv?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından çokgen alanda kalem çizin ve <a href='https://codepen.io/azuremaps/pen/eYmZGNv'>arama yapın.</a>
</iframe>

<br/>

### <a name="create-a-measuring-tool"></a>Bir ölçme aracı oluşturma

Aşağıdaki kod, çizim olaylarının bir ölçme aracı oluşturmak için nasıl kullanılabileceğini gösterir. Şekil `drawingchanging` çizilirken şekli izlemek için kullanılır. Kullanıcı fareyi hareket ettikçe şeklin boyutları hesaplanır. Olay `drawingcomplete` çizildikten sonra şekil üzerinde son bir hesaplama yapmak için kullanılır. Olay, `drawingmodechanged` kullanıcının çizim moduna ne zaman geçtiğini belirlemek için kullanılır. Ayrıca, `drawingmodechanged` olay çizim tuvali temizler ve eski ölçüm bilgilerini temizler.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Ölçüm aleti" src="https://codepen.io/azuremaps/embed/RwNaZXe?height=500&theme-id=default&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'de</a>Azure Haritalar<a href='https://codepen.io/azuremaps'>@azuremaps</a>'a göre Kalem <a href='https://codepen.io/azuremaps/pen/RwNaZXe'>Ölçme aracına</a> bakın .
</iframe>

<br/>

## <a name="next-steps"></a>Sonraki adımlar

Çizim araçları modülünün ek özelliklerini nasıl kullanacağınızı öğrenin:

> [!div class="nextstepaction"]
> [Şekil verilerini alma](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Etkileşim türleri ve klavye kısayolları](drawing-tools-interactions-keyboard-shortcuts.md)

Hizmetler modülü hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Hizmetler modülü](how-to-use-services-module.md)

Daha fazla kod örneğine göz atın:

> [!div class="nextstepaction"]
> [Kod örnek sayfası](https://aka.ms/AzureMapsSamples)
