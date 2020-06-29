---
title: Mağaza Kullanıcı tercihleri
titleSuffix: Azure Cognitive Services
description: Bu makalede, kullanıcının tercihlerini nasıl depolayacaksınız.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: metan
ms.openlocfilehash: beb053551dc1fa28672c488b31dfb29ca3b53651
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85486927"
---
# <a name="how-to-store-user-preferences"></a>Kullanıcı tercihlerini depolama

Bu makalede, kullanıcının tercihlerinin nasıl depolandığı gösterilmektedir. Bu işlevsellik, tanımlama bilgilerinin kullanılması istenmediği veya uygun olmadığı durumlarda kullanıcının tercihlerini depolamanın alternatif bir yolu olarak tasarlanmıştır.

## <a name="how-to-enable-storing-user-preferences"></a>Kullanıcı tercihlerini depolamayı etkinleştirme

`options`Parametresi `onPreferencesChanged` geri çağırma işlemini içerir. Bu işlev, Kullanıcı tercihlerini her değiştirdiklerinde çağrılır (örneğin, metin boyutunu, tema rengini, yazı tipini ve benzerlerini değiştirir). `value`Parametresi, kullanıcının geçerli tercihlerini temsil eden bir dize içerir. Bu dize, tercih ettiğiniz herhangi bir mekanizma kullanılarak depolanabilir.

```typescript
const options = {
    onPreferencesChanged: (value: string) => {
        // Store user preferences here
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="how-to-load-user-preferences-into-the-immersive-reader"></a>Kullanıcı tercihlerini tam ekran okuyucusuna yükleme

Parametresini kullanarak kullanıcının tercihlerini tam ekran okuyucusuna geçirin `preferences` . Kullanıcı tercihlerini depolamak ve yüklemek için önemsiz bir örnek aşağıdaki gibidir:

```typescript
let userPreferences = null;
const options = {
    preferences: userPreferences,
    onPreferencesChanged: (value: string) => {
        userPreferences = value;
    }
};
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet