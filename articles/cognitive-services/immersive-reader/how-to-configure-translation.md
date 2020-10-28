---
title: Çeviriyi yapılandırma
titleSuffix: Azure Cognitive Services
description: Bu makalede, çeviri için çeşitli seçeneklerin nasıl yapılandırılacağı gösterilir.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 0a6ed6ecea216a36cfc9da4ef36a2bddc69cc6a8
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92633153"
---
# <a name="how-to-configure-translation"></a>Çeviriyi yapılandırma

Bu makalede, tam ekran okuyucu 'da çeviri için çeşitli seçeneklerin nasıl yapılandırılacağı gösterilir.

## <a name="configure-translation-language"></a>Çeviri dilini yapılandırma

`options`Parametresi, çeviriyi yapılandırmak için kullanılabilen tüm bayrakları içerir. Parametresini, `language` çevirmek istediğiniz dile ayarlayın. Desteklenen dillerin tam listesi için bkz. [dil desteği](./language-support.md) .

```typescript
const options = {
    translationOptions: {
        language: 'fr-FR'
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="automatically-translate-the-document-on-load"></a>Belgeyi yükleme sırasında otomatik olarak çevir

`autoEnableDocumentTranslation` `true` Tam ekran okuyucusu yüklendiğinde tüm belgeyi otomatik olarak çevirmeyi etkinleştirmek için olarak ayarlayın.

```typescript
const options = {
    translationOptions: {
        autoEnableDocumentTranslation: true
    }
};
```

## <a name="automatically-enable-word-translation"></a>Sözcük çevirisini otomatik olarak etkinleştir

`autoEnableWordTranslation` `true` Tek sözcük çevirisini etkinleştirmek için olarak ayarlayın.

```typescript
const options = {
    translationOptions: {
        autoEnableWordTranslation: true
    }
};
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet