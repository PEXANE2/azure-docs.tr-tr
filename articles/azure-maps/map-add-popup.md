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
ms.openlocfilehash: 92d44ef3d0db8e93d4babd7441238c7fa105dbd5
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639012"
---
# <a name="add-a-popup-to-the-map"></a>Haritaya bir açılan pencere ekleyin

Bu makalede, haritada bir noktaya açılan pencerenin nasıl ekleneceği gösterilmektedir.

## <a name="understand-the-code"></a>Kodu anlama

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Azure haritalar 'ı kullanarak bir açılan pencere ekleme' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>'da Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) ile <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Azure haritaları kullanarak bir açılan pencere ekleme</a> kalemine bakın.
</iframe>

Yukarıdaki kodda, ilk kod bloğu bir harita nesnesi oluşturur. Yönergeler için [bir harita oluşturma](./map-create.md) ' ya bakabilirsiniz. Ayrıca, açılan pencerede görüntülenecek HTML içeriğini de oluşturur.

İkinci kod bloğu, [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) sınıfını kullanarak bir veri kaynağı nesnesi oluşturur. Nokta, [nokta](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) sınıfının bir [özelliğidir](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) . Daha sonra ad ve açıklama özelliklerine sahip bir nokta nesnesi oluşturulup veri kaynağına eklenir.

Bir [sembol katmanı](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) , [veri kaynağında](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) kaydırılan nokta tabanlı verileri haritada semboller olarak oluşturmak için metin veya simgeleri kullanır.  Bir sembol katmanı, üçüncü kod bloğunda oluşturulur. Veri kaynağı, daha sonra haritaya eklenen sembol katmanına eklenir.

Dördüncü kod bloğu aracılığıyla `new atlas.Popup()`bir [açılan nesne](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) oluşturur. Position ve Pixelsapmayı gibi açılan Özellikler [PopupOptions](/javascript/api/azure-maps-control/atlas.popupoptions)'in bir parçasıdır. PopupOptions, açılan oluşturucuda veya açılan sınıfın [SetOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) işlevi aracılığıyla tanımlanabilir. Daha `mouseover` sonra sembol katmanı için bir olay dinleyicisi oluşturulur.

Son kod bloğu `mouseover` olay dinleyicisi tarafından tetiklenen bir işlev oluşturur. Açılan pencerenin içeriğini ve özelliklerini ayarlar ve açılan nesneyi haritaya ekler.

## <a name="reusing-a-popup-with-multiple-points"></a>Birden çok noktayla açılan pencereyi yeniden kullanma

Çok sayıda noktanız olduğunda ve tek seferde bir açılan pencere göstermek istediğinizde, en iyi yaklaşım bir açılan pencere oluşturmak ve her nokta özelliği için bir açılan pencere oluşturmak yerine onu yeniden kullanmaktır. Bunu yaptığınızda, uygulama tarafından oluşturulan DOM öğelerinin sayısı büyük ölçüde azalır ve bu sayede daha iyi performans sağlayabiliriz. Bu örnek 3 nokta özellik oluşturur. Bunlardan birine tıklarsanız, bu nokta özelliği için içerik ile bir açılan pencere görüntülenir.

<br/>

<iframe height='500' scrolling='no' title='Çoklu PIN ile açılan pencereyi yeniden kullanma' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'><a href='https://codepen.io'>Codepen</a>üzerinde Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) tarafından <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>birden fazla PIN ile açılan pencereyi yeniden kullanma</a> kalemine bakın.
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