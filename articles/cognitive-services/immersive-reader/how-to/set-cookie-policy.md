---
title: Modern okuyucu tanımlama bilgisi Ilkesini ayarlama
titleSuffix: Azure Cognitive Services
description: Bu makalede, tam ekran okuyucu için tanımlama bilgisi ilkesini ayarlama gösterilmektedir.
services: cognitive-services
author: pasta
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: pasta
ms.openlocfilehash: 6de651f1eb51ea1bc941c2af675c8a8d5b0f9cd7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75946116"
---
# <a name="how-to-set-the-cookie-policy-for-the-immersive-reader"></a>Derinlikli okuyucu için tanımlama bilgisi ilkesini ayarlama

Tam ekran okuyucu, varsayılan olarak tanımlama bilgisi kullanımını devre dışı bırakır. Tanımlama bilgisi kullanımını etkinleştirirseniz, tam ekran okuyucu Kullanıcı tercihlerini korumak ve özellik kullanımını izlemek için tanımlama bilgilerini kullanabilir. Derinlikli okuyucuda tanımlama bilgisi kullanımını etkinleştirirseniz, lütfen AB tanımlama bilgisi uyumluluk Ilkesinin gereksinimlerini göz önünde bulundurun. Bu, ana bilgisayar uygulamasının, AB tanımlama bilgisi uyumluluk Ilkesiyle uyumlu olarak gerekli Kullanıcı onayını elde etmek için sorumluluğudur.

Tanımlama bilgisi İlkesi, derinlikli okuyucu [seçenekleri](../reference.md#options)aracılığıyla ayarlanabilir. Daha fazla bilgi için bkz. The [ıepolicy enum](../reference.md#cookiepolicy-enum) .

## <a name="enable-cookie-usage"></a>Tanımlama bilgisi kullanımını etkinleştir

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Enable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="disable-cookie-usage"></a>Tanımlama bilgisi kullanımını devre dışı bırak

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Disable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="next-steps"></a>Sonraki adımlar

* Node. js kullanarak modern Okuyucu SDK 'Sı ile neler yapabileceğinizi görmek için [Node. js hızlı](../quickstart-nodejs.md) başlangıcını görüntüleyin
* Python kullanarak modern Okuyucu SDK 'Sı ile neler yapabileceğinizi öğrenmek için [Python öğreticisini](../tutorial-python.md) görüntüleyin
* Swift [öğreticisini](../tutorial-ios-picture-immersive-reader.md) kullanarak tam ekran okuyucu SDK 'sı ile neler yapabileceğinizi görün.
* [Modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) ve [tam ekran okuyucu SDK başvurusunu](../reference.md) keşfet