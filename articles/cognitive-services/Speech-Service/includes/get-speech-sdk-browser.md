---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5545b10a4228448d49849e7cd52728febf14ce2d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399852"
---
:::row:::
    :::column span="3":::
        JavaScript Konuşma SDK bir npm paketi olarak kullanılabilir, <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">microsoft-cognitiveservices-speech-sdk <span class="docon docon-navigate-external x-hidden-focus"></span> </a> görmek ve arkadaşı GitHub deposu <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">bilişsel <span class="docon docon-navigate-external x-hidden-focus"> </span>hizmetler-konuşma-sdk-js </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="JavaScript" src="https://docs.microsoft.com/media/logos/logo_js.svg"  width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> JavaScript Konuşma SDK bir npm paketi olarak kullanılabilir olmasına rağmen, böylece hem istemci web tarayıcıları ve Node.js tüketebilir - her ortamın çeşitli mimari etkileri düşünün. Örneğin, <a href="https://nodejs.org/api/fs.html" target="_blank">dosya sistemi <span class="docon docon-navigate-external x-hidden-focus"></span> </a> istemci tarafındaki uygulamalariçin kullanılamadığı gibi belge <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">nesnesi modeli (DOM) <span class="docon docon-navigate-external x-hidden-focus"></span> </a> sunucu tarafındaki uygulamalar için kullanılamaz.

### <a name="nodejs-package-manager-npm"></a>Düğüm.js Paket Yöneticisi (NPM)

JavaScript Konuşma SDK'yı yüklemek `npm install` için aşağıdaki komutu çalıştırın.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

### <a name="html-script-tag"></a>HTML komut dosyası etiketi

Alternatif olarak, <a href="https://www.jsdelivr.com/package/npm/microsoft-cognitiveservices-speech-sdk" target="_blank"> **doğrudan JSDelivr** NPM sendikası <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>güvenerek, HTMLs `<script>` `<head>` öğesine bir etiket ekleyebilir.

```html
<script src="https://cdn.jsdelivr.net/npm/microsoft-cognitiveservices-speech-sdk@1.10.1/distrib/lib/microsoft.cognitiveservices.speech.sdk.min.js">
</script>
```

Daha fazla bilgi için <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser" target="_blank">Web Tarayıcısı <span class="docon docon-navigate-external x-hidden-focus"> </span>Konuşma SDK quickstart </a>bakın.
