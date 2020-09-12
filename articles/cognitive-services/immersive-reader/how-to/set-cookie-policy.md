---
title: Modern okuyucu tanımlama bilgisi Ilkesini ayarlama
titleSuffix: Azure Cognitive Services
description: Bu makalede, tam ekran okuyucu için tanımlama bilgisi ilkesini ayarlama gösterilmektedir.
services: cognitive-services
author: nitinme
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: nitinme
ms.custom: devx-track-javascript
ms.openlocfilehash: 44de79afd6e2fb1f1d4fff1323484364a17da8d0
ms.sourcegitcommit: 59ea8436d7f23bee75e04a84ee6ec24702fb2e61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2020
ms.locfileid: "89505310"
---
# <a name="how-to-set-the-cookie-policy-for-the-immersive-reader"></a>Derinlikli okuyucu için tanımlama bilgisi ilkesini ayarlama

Tam ekran okuyucu, varsayılan olarak tanımlama bilgisi kullanımını devre dışı bırakır. Tanımlama bilgisi kullanımını etkinleştirirseniz, tam ekran okuyucu Kullanıcı tercihlerini korumak ve özellik kullanımını izlemek için tanımlama bilgilerini kullanabilir. Derinlikli okuyucuda tanımlama bilgisi kullanımını etkinleştirirseniz, lütfen AB tanımlama bilgisi uyumluluk Ilkesinin gereksinimlerini göz önünde bulundurun. Bu, ana bilgisayar uygulamasının, AB tanımlama bilgisi uyumluluk Ilkesiyle uyumlu olarak gerekli Kullanıcı onayını elde etmek için sorumluluğudur.

Tanımlama bilgisi İlkesi, derinlikli okuyucu [seçenekleri](../reference.md#options)aracılığıyla ayarlanabilir.

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

* Node.js kullanarak modern Okuyucu SDK 'Sı ile neler yapabileceğinizi görmek için [Node.js hızlı](../quickstarts/client-libraries.md?pivots=programming-language-nodejs) başlangıcı 'nı görüntüleyin.
* Java ya da Android için Kotlin kullanarak modern Okuyucu SDK 'Sı ile neler yapabileceğinizi öğrenmek için [Android öğreticisini](../tutorial-android.md) görüntüleyin
* İOS için Swift kullanarak modern Okuyucu SDK 'Sı ile neler yapabileceğinizi öğrenmek için [iOS öğreticisini](../tutorial-ios.md) görüntüleyin
* Python kullanarak modern Okuyucu SDK 'Sı ile neler yapabileceğinizi öğrenmek için [Python öğreticisini](../tutorial-python.md) görüntüleyin
* [Modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) ve [tam ekran okuyucu SDK başvurusunu](../reference.md) keşfet