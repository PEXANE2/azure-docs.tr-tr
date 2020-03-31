---
title: Web SDK desteklenen tarayıcılar | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Web SDK için desteklenen tarayıcılar ve tarayıcının desteklenen bir tarayıcı olup olmadığını nasıl denetlediğiniz hakkında bilgi edineceksiniz.
author: rbrundritt
ms.author: richbrun
ms.date: 03/25/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.openlocfilehash: e81b15b974469d319384a67b08512130b7876a30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76988796"
---
# <a name="web-sdk-supported-browsers"></a>Web SDK destekleyen tarayıcılar

Azure Haritalar Web SDK [atlas.isSupported](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest#issupported-boolean-)adlı bir yardımcı işlevi sağlar. Bu işlev, bir web tarayıcısının harita denetiminin yüklenmesi ve işlenmesini desteklemek için gereken en az WebGL özelliği kümesine sahip olup olmadığını algılar. İşlevin nasıl kullanılacağına bir örnek aşağıda verilmiştir:

```JavaScript
if (!atlas.isSupported()) {
    alert('Your browser is not supported by Azure Maps');
} else if (!atlas.isSupported(true)) {
    alert('Your browser is supported by Azure Maps, but may have major performance caveats.');
} else {
    // Your browser is supported. Add your map code here.
}
```

## <a name="desktop"></a>Masaüstü

Azure Haritalar Web SDK aşağıdaki masaüstü tarayıcılarını destekler:

- Microsoft Edge (geçerli ve önceki sürüm)
- Google Chrome (geçerli ve önceki sürüm)
- Mozilla Firefox (şimdiki ve önceki sürüm)
- Apple Safari (Mac OS X) (geçerli ve önceki sürüm)

Ayrıca bu makalede daha sonra [hedef eski tarayıcılar](#Target-Legacy-Browsers) bakın.

## <a name="mobile"></a>Mobil

Azure Haritalar Web SDK aşağıdaki mobil tarayıcıları destekler:

- Android
  - Android 6.0 ve sonrası Chrome'un geçerli sürümü
  - Android 6.0 ve sonraki sürümlerinde Chrome WebView
- iOS
  - iOS'un geçerli ve önceki ana sürümünde Mobil Safari
  - iOS'un geçerli ve önceki ana sürümünde UIWebView ve WKWebView
  - iOS için Chrome'un geçerli sürümü

> [!TIP]
> Bir WebView denetimi kullanarak bir mobil uygulamanın içine bir harita katıştırıyorsanız, Azure İçerik Dağıtım Ağı'nda barındırılan SDK sürümüne başvurmak yerine [Azure Haritalar Web SDK'nın npm paketini](https://www.npmjs.com/package/azure-maps-control) kullanmayı tercih edebilirsiniz. SDK zaten kullanıcının aygıtında olduğundan ve çalışma zamanında karşıdan yüklenmesi gerekmediği için bu yaklaşım yükleme süresini kısaltıyor.

## <a name="nodejs"></a>Node.js

Aşağıdaki Web SDK modülleri de Düğüm.js desteklenir:

- Hizmetler modülü ([dokümantasyon](how-to-use-services-module.md) | [npm modülü](https://www.npmjs.com/package/azure-maps-rest))

## <a name="target-legacy-browsers"></a><a name="Target-Legacy-Browsers"></a>Eski tarayıcıları hedefle

WebGL'yi desteklemeyen veya yalnızca sınırlı desteği olan eski tarayıcıları hedeflemek isteyebilirsiniz. Bu gibi durumlarda, Azure Haritalar hizmetlerini [Broşür](https://leafletjs.com/)gibi açık kaynak kodlu bir harita denetimiyle birlikte kullanmanızı öneririz. Bir örneği aşağıda verilmiştir:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Azure Haritalar + Broşür" src="//codepen.io/azuremaps/embed/GeLgyx/?height=500&theme-id=0&default-tab=html,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
<a href='https://codepen.io'>CodePen'deki</a>Azure Haritalarına Göre<a href='https://codepen.io/azuremaps'>@azuremaps</a>Kalem Azure Haritaları + <a href='https://codepen.io/azuremaps/pen/GeLgyx/'>Broşüre</a> bakın .
</iframe>


## <a name="next-steps"></a>Sonraki adımlar

Azure Haritalar Web SDK hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Harita denetimi](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Hizmetler modülü](how-to-use-services-module.md)
