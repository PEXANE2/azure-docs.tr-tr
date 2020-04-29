---
title: Tam ekran okuyucu düğmesini özelleştirme
titleSuffix: Azure Cognitive Services
description: Bu makalede, tam ekran okuyucuyu Başlatan düğmeyi nasıl özelleştireceğiniz gösterilmektedir.
services: cognitive-services
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: 6461cc60a237cf834226b07db5e1ea6fbbea7a92
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75946215"
---
# <a name="how-to-customize-the-immersive-reader-button"></a>Tam ekran okuyucu düğmesini özelleştirme

Bu makalede, tam ekran okuyucuyu uygulamanızın ihtiyaçlarına uyacak şekilde Başlatan düğmenin nasıl özelleştirileceği gösterilmektedir.

## <a name="add-the-immersive-reader-button"></a>Derinlikli okuyucu düğmesini ekleme

Modern Okuyucu SDK 'Sı, tam ekran okuyucuyu başlatan düğme için varsayılan stil sağlar. Bu stillendirme özelliğini etkinleştirmek için `immersive-reader-button` Class özniteliğini kullanın.

```html
<div class='immersive-reader-button'></div>
```

## <a name="customize-the-button-style"></a>Düğme stilini özelleştirme

Düğmenin stilini `data-button-style` ayarlamak için özniteliğini kullanın. İzin verilen değerler `icon` `text`,, ve `iconAndText`. Varsayılan değer: `icon`.

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

`data-locale` Özniteliğini ve düğmesini kullanarak düğme için alternatif metni yapılandırın. İngilizce varsayılan dildir.

```html
<div class='immersive-reader-button' data-locale='fr-FR'></div>
```

## <a name="customize-the-size-of-the-icon"></a>Simgenin boyutunu özelleştirme

Derinlikli okuyucu simgesinin boyutu `data-icon-px-size` özniteliği kullanılarak yapılandırılabilir. Bu, simgenin boyutunu piksel cinsinden ayarlar. Varsayılan boyut 20 px ' dir.

```html
<div class='immersive-reader-button' data-icon-px-size='50'></div>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet