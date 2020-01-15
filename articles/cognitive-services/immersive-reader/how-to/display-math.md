---
title: Modern okuyucuda matematik görüntüleme
titleSuffix: Azure Cognitive Services
description: Bu makalede, tam ekran okuyucu 'da matematik görüntüleme yöntemi gösterilmektedir.
author: pasta
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: pasta
ms.openlocfilehash: e01cc9e8cedb6c38da0b56e04419c706d5d0566e
ms.sourcegitcommit: 49e14e0d19a18b75fd83de6c16ccee2594592355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946127"
---
# <a name="how-to-display-math-in-the-immersive-reader"></a>Modern okuyucuda matematik görüntüleme

Tam ekran okuyucu matematiksel biçimlendirme dili ([MathML](https://developer.mozilla.org/docs/Web/MathML)) biçiminde sağlandığında matematiği gösterebilir.
MIME türü, derinlikli okuyucu [öbeği](../reference.md#chunk)aracılığıyla ayarlanabilir. Daha fazla bilgi için bkz. [desteklenen MIME türleri](../reference.md#supported-mime-types) .

## <a name="send-math-to-the-immersive-reader"></a>Matematik okuyucuyu tam ekran okuyucuya gönderin
Bir ekran derinlikli okuyucuya matematik göndermek için, MathML içeren bir öbek sağlayın ve MIME türünü ```application/mathml+xml```; olarak ayarlayın.

Örneğin, içeriğiniz aşağıdaki olsaydı:

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

Ardından, aşağıdaki JavaScript 'ı kullanarak içeriğinizi görüntüleyebilirsiniz.

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

Tam ekran okuyucuyu başlattığınızda şunları görmeniz gerekir:

![Modern okuyucudaki matematik](../media/how-tos/1-math.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) ve [tam ekran okuyucu SDK başvurusunu](../reference.md) keşfet