---
title: Tam Ekran Okuyucu’yu HTML içeriğiyle başlatma
titleSuffix: Azure Cognitive Services
description: Bu makalede, HTML içeriği ile Immersive Reader başlatmak için nasıl gösterecektir.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: bc7ab46113e1b819fc71a9f6e8a18400f8acfbef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946248"
---
# <a name="how-to-launch-the-immersive-reader-with-html-content"></a>HTML içeriği ile Immersive Reader nasıl başlatılır?

Bu makalede, HTML içeriği ile Immersive Reader başlatmak için nasıl gösterir.

## <a name="prepare-the-html-content"></a>HTML içeriğini hazırlama

İşlemek istediğiniz içeriği Bir kapsayıcı öğesinin içine Sürükleyici Okuyucu'ya yerleştirin. Kapsayıcı öğesinin benzersiz `id`bir . Immersive Reader temel HTML öğeleri için destek sağlar, daha fazla bilgi için [başvurubakın.](./reference.md#html-support)

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

## <a name="get-the-html-content-in-javascript"></a>JavaScript'te HTML içeriğini alın

`id` JavaScript kodunuzda HTML içeriğini almak için kapsayıcı öğesini kullanın.

```javascript
const htmlContent = document.getElementById('immersive-reader-content').innerHTML;
```

## <a name="launch-the-immersive-reader-with-your-html-content"></a>HTML içeriğinizle Sürükleyici Okuyucuyu başlatın

Ararken, `ImmersiveReader.launchAsync`HTML işlemeyi `mimeType` etkinleştirmek için `text/html` öbek özelliğini ayarlayın.

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

* Sürükleyici [Okuyucu SDK Referans](./reference.md) keşfedin