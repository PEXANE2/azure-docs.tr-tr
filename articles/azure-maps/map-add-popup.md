---
title: Azure Maps ile bir açılan pencere ekleyin | Microsoft Docs
description: JavaScript eşlemesine açılan pencere ekleme
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: caf661faf00d1d32664b7958a14a8719a37ab36e
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882108"
---
# <a name="add-a-popup-to-the-map"></a>Haritaya bir açılan pencere ekleyin

Bu makalede, haritada bir noktaya açılan pencerenin nasıl ekleneceği gösterilmektedir.

## <a name="understand-the-code"></a>Kodu anlama

<a id="addAPopup"></a>

Aşağıdaki kod, bir sembol katmanını kullanarak eşlemeye ve `name` `description` özelliklerine sahip bir nokta özelliği ekler. [Açılan sınıfın](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) bir örneği oluşturulur, ancak gösterilmez. Fare olayları simge katmanına eklendiğinde, fare simge işaretinin üzerine geldiğinde ve devre dışı bırakıldığında açılan pencerenin açılmasını ve kapatılmasını tetikler. İşaret simgesi üzerine `position` gelindiğinde, açılan menünün özelliği işaret konumuyla güncellenir `content` ve seçenek, nokta özelliğinin `name` ve `description` özelliklerini sarmalanmış olan bir HTML ile güncelleştirilir. Açılan pencere daha sonra kendi `open` işlevini kullanarak haritada görüntülenir.

<br/>

<iframe height='500' scrolling='no' title='Azure haritalar 'ı kullanarak bir açılan pencere ekleme' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Azure haritaları kullanarak bir açılan pencere ekleme</a> kalemine bakın.
</iframe>

## <a name="reusing-a-popup-with-multiple-points"></a>Birden çok noktayla açılan pencereyi yeniden kullanma

Çok sayıda noktanız olduğunda ve tek seferde bir açılan pencere göstermek istiyorsanız, en iyi yaklaşım bir açılan pencere oluşturup her bir nokta özelliği için bir açılan pencere oluşturmak yerine onu yeniden kullanmaktır. Açılan pencereyi yeniden kullanmaya göre, uygulama tarafından oluşturulan DOM öğelerinin sayısı büyük ölçüde azalır ve bu da daha iyi performans sağlayabilir. Aşağıdaki örnek 3 nokta özellik oluşturur. Bunlardan birine tıklarsanız, bu nokta özelliği için içerik ile bir açılan pencere görüntülenir.

<br/>

<iframe height='500' scrolling='no' title='Çoklu PIN ile açılan pencereyi yeniden kullanma' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>birden fazla PIN ile açılan pencereyi yeniden kullanma</a> kalemine bakın.
</iframe>

## <a name="customizing-a-popup"></a>Açılan pencereyi özelleştirme

Varsayılan olarak, açılan pencerede beyaz bir arka plan, altta bir işaretçi ok ve sağ üst köşedeki bir Kapat düğmesi bulunur. Aşağıdaki örnek, açılan pencere `fillColor` seçeneğini kullanarak arka plan rengini siyah olarak değiştirir. Kapat düğmesi, `shoCloseButton` seçeneği false olarak ayarlanarak kaldırılır. Açılan menünün HTML içeriği, açılan pencerenin kenarlarından doldurulmuş 10 piksel kullanır ve metin siyah arka planda beyaz bir şekilde görünür.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Özelleştirilmiş açılan pencere" src="//codepen.io/azuremaps/embed/ymKgdg/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/ymKgdg/'>özelleştirilmiş açılan pencere</a> başlığına bakın.
</iframe>

## <a name="popup-events"></a>Açılan olaylar

Açılanlar açılabilir, kapatılabilir ve sürüklenip kapatılabilir. Açılan sınıf, yardım geliştiricilerin bu eylemlere tepki vermesini sağlayan olayları sağlar. Aşağıdaki örnek, açılan pencereyi açtığınızda, kapatırken veya sürüklediğinizde hangi olayların tetikleyeceğinizi vurgulamaktadır. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Açılan olaylar" src="//codepen.io/azuremaps/embed/BXrpvB/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile kalem <a href='https://codepen.io/azuremaps/pen/BXrpvB/'>açılan olayları</a> ' na bakın.
</iframe>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede kullanılan sınıflar ve yöntemler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Kutu](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

Tam kod örnekleri için aşağıdaki harika makalelere göz atın:

> [!div class="nextstepaction"]
> [Sembol katmanı ekleme](./map-add-pin.md)

> [!div class="nextstepaction"]
> [HTML işaretleyicisi ekleme](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Şekil ekleme](./map-add-shape.md)