---
title: Sürükleyici Okuyucu Çerez Politikası Nı Ayarla
titleSuffix: Azure Cognitive Services
description: Bu makalede, Immersive Reader için çerez ilkesini nasıl ayarlayacağınızı gösterecektir.
services: cognitive-services
author: pasta
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: pasta
ms.openlocfilehash: 6de651f1eb51ea1bc941c2af675c8a8d5b0f9cd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946116"
---
# <a name="how-to-set-the-cookie-policy-for-the-immersive-reader"></a>Immersive Reader için çerez ilkesi nasıl ayarlanır?

Immersive Reader varsayılan olarak çerez kullanımını devre dışı kalacaktır. Çerez kullanımını etkinleştiriseniz, Immersive Reader kullanıcı tercihlerini korumak ve özellik kullanımını izlemek için tanımlama bilgilerini kullanabilir. Sürükleyici Okuyucu'da çerez kullanımını etkinleştirdiyseniz, lütfen AB Çerez Uyumluluk Politikası'nın gereksinimlerini göz önünde bulundurun. AB Çerez Uyumluluk Politikası uyarınca gerekli kullanıcı onayı almak ev sahibi uygulamanın sorumluluğundadır.

Çerez ilkesi, Sürükleyici Okuyucu [seçenekleri](../reference.md#options)aracılığıyla ayarlanabilir. Daha fazla bilgi için [CookiePolicy enum'a](../reference.md#cookiepolicy-enum) bakın.

## <a name="enable-cookie-usage"></a>Çerez Kullanımını Etkinleştir

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Enable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="disable-cookie-usage"></a>Çerez Kullanımını Devre Dışı

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Disable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="next-steps"></a>Sonraki adımlar

* Node.js kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [Düğüm.js quickstart'ı](../quickstart-nodejs.md) görüntüleyin
* Python kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [Python öğreticisini](../tutorial-python.md) görüntüleyin
* Swift'i kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [Swift öğreticisini](../tutorial-ios-picture-immersive-reader.md) görüntüleyin
* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve [Immersive Reader SDK Referans](../reference.md) keşfedin