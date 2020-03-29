---
title: Sürükleyici Okuyucuda matematiği görüntüleyin
titleSuffix: Azure Cognitive Services
description: Bu makalede, Nasıl Sürükleyici Reader matematik görüntülemek için gösterecektir.
author: pasta
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: pasta
ms.openlocfilehash: e01cc9e8cedb6c38da0b56e04419c706d5d0566e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75946127"
---
# <a name="how-to-display-math-in-the-immersive-reader"></a>Sürükleyici Okuyucu'da matematik nasıl görüntülenir?

Sürükleyici Okuyucu Matematik Biçimlendirme Dili[(MathML)](https://developer.mozilla.org/docs/Web/MathML)şeklinde sağlandığında matematik görüntüleyebilirsiniz.
MIME türü Immersive Reader [yığın](../reference.md#chunk)üzerinden ayarlanabilir. Daha fazla bilgi için [desteklenen MIME türlerine](../reference.md#supported-mime-types) bakın.

## <a name="send-math-to-the-immersive-reader"></a>Matematiği Sürükleyici Okuyucuya Gönder
Sürükleyici Okuyucu'ya matematik göndermek için MathML içeren bir yığın verin ve MIME ```application/mathml+xml```türünü ayarlayın;

Örneğin, içeriğiniz aşağıdakiler olsaydı:

```html
<div id='ir-content'>
    <math xmlns='http://www.w3.org/1998/Math/MathML'>
        <mfrac>
            <mrow>
                <msup>
                    <mi>x</mi>
                    <mn>2</mn>
                </msup>
                <mo>+</mo>
                <mn>3</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mn>2</mn>
            </mrow>
            <mrow>
                <mi>x</mi>
                <mo>−</mo>
                <mn>3</mn>
            </mrow>
        </mfrac>
        <mo>=</mo>
        <mn>4</mn>
    </math>
</div>
```

Ardından aşağıdaki JavaScript'i kullanarak içeriğinizi görüntüleyebilirsiniz.

```javascript
const data = {
    title: 'My Math',
    chunks: [{
        content: document.getElementById('ir-content').innerHTML.trim(),
        mimeType: 'application/mathml+xml'
    }]
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, data, YOUR_OPTIONS);
```

Sürükleyici Okuyucuyu başlattığınızda şunları görmeniz gerekir:

![Sürükleyici Okuyucuda Matematik](../media/how-tos/1-math.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve [Immersive Reader SDK Referans](../reference.md) keşfedin