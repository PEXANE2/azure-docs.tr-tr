---
title: Tam Ekran Okuyucu’yu HTML içeriğiyle başlatma
titleSuffix: Azure Cognitive Services
description: Bu makalede, HTML içeriğiyle tam ekran okuyucuyu nasıl başlatacaksınız gösterilmektedir.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.custom: devx-track-js
ms.openlocfilehash: f7f55d1d349fc7d02c2ac53f7a50f9d649688cbd
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326771"
---
# <a name="how-to-launch-the-immersive-reader-with-html-content"></a>HTML içeriğiyle modern okuyucu başlatma

Bu makalede, HTML içeriğiyle modern okuyucu başlatma gösterilmektedir.

## <a name="prepare-the-html-content"></a>HTML içeriğini hazırlama

İşlemek istediğiniz içeriği bir kapsayıcı öğesinin içindeki tam ekran okuyucusuna yerleştirin. Kapsayıcı öğesinin benzersiz olduğundan emin olun `id` . Modern okuyucu, temel HTML öğeleri için destek sağlar, daha fazla bilgi için [başvuruya](./reference.md#html-support) bakın.

```html
<div id='immersive-reader-content'>
    <b>Bold</b>
    <i>Italic</i>
    <u>Underline</u>
    <strike>Strikethrough</strike>
    <code>Code</code>
    <sup>Superscript</sup>
    <sub>Subscript</sub>
    <ul><li>Unordered lists</li></ul>
    <ol><li>Ordered lists</li></ol>
</div>
```

## <a name="get-the-html-content-in-javascript"></a>JavaScript 'te HTML içeriğini al

`id`JavaScript KODUNUZDA HTML içeriğini almak için kapsayıcı öğesinin öğesini kullanın.

```javascript
const htmlContent = document.getElementById('immersive-reader-content').innerHTML;
```

## <a name="launch-the-immersive-reader-with-your-html-content"></a>HTML içeriğinizdeki tam ekran okuyucuyu başlatın

`ImmersiveReader.launchAsync`' I çağırırken, `mimeType` `text/html` HTML oluşturmayı etkinleştirmek için öbek özelliğini olarak ayarlayın.

```javascript
const data = {
    chunks: [{
        content: htmlContent,
        mimeType: 'text/html'
    }]
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, data, YOUR_OPTIONS);
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tam ekran okuyucu SDK başvurusunu](./reference.md) keşfet