---
title: Immersive Reader düğmesini özelleştirin
titleSuffix: Azure Cognitive Services
description: Bu makalede, Immersive Reader başlattı düğmesini özelleştirmek için nasıl gösterecektir.
services: cognitive-services
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: 6461cc60a237cf834226b07db5e1ea6fbbea7a92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946215"
---
# <a name="how-to-customize-the-immersive-reader-button"></a>Immersive Reader düğmesini özelleştirme

Bu makalede, uygulamanızın gereksinimlerine uyacak şekilde Immersive Reader'ı başlatan düğmenin nasıl özelleştirilenbir şekilde özelleştirilen gösterilmiştir.

## <a name="add-the-immersive-reader-button"></a>Sürükleyici Okuyucu düğmesini ekle

Immersive Reader SDK, Sürükleyici Okuyucuyu başlatan düğme için varsayılan stil sağlar. Bu `immersive-reader-button` stili etkinleştirmek için sınıf özniteliğini kullanın.

```html
<div class='immersive-reader-button'></div>
```

## <a name="customize-the-button-style"></a>Düğme stilini özelleştirme

Düğmenin `data-button-style` stilini ayarlamak için özniteliği kullanın. İzin verilen `icon`değerler `text`, `iconAndText`ve . Varsayılan değer: `icon`.

### <a name="icon-button"></a>Simge düğmesi

```html
<div class='immersive-reader-button' data-button-style='icon'></div>
```

Bu, aşağıdakileri işler:

![Simge düğmesi](./media/button-icon.png)

### <a name="text-button"></a>Metin düğmesi

```html
<div class='immersive-reader-button' data-button-style='text'></div>
```

Bu, aşağıdakileri işler:

![Simge düğmesi](./media/button-text.png)

### <a name="icon-and-text-button"></a>Simge ve metin düğmesi

```html
<div class='immersive-reader-button' data-button-style='iconAndText'></div>
```

Bu, aşağıdakileri işler:

![Simge düğmesi](./media/button-icon-and-text.png)

## <a name="customize-the-button-text"></a>Düğme metnini özelleştirme

Özniteliği kullanarak düğmenin dilini ve alt `data-locale` metnini yapılandırın. İngilizce varsayılan dildir.

```html
<div class='immersive-reader-button' data-locale='fr-FR'></div>
```

## <a name="customize-the-size-of-the-icon"></a>Simgenin boyutunu özelleştirme

Immersive Reader simgesinin boyutu öznitelik kullanılarak `data-icon-px-size` yapılandırılabilir. Bu, simgenin piksel boyutunu ayarlar. Varsayılan boyutu 20px olduğunu.

```html
<div class='immersive-reader-button' data-icon-px-size='50'></div>
```

## <a name="next-steps"></a>Sonraki adımlar

* Sürükleyici [Okuyucu SDK Referans](./reference.md) keşfedin