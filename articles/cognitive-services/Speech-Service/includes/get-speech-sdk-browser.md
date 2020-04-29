---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5545b10a4228448d49849e7cd52728febf14ce2d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81399852"
---
:::row:::
    :::column span="3":::
        JavaScript konuşma SDK 'sı, NPM paketi olarak kullanılabilir, bkz. <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">Microsoft-biliveservices-Speech-SDK <span class="docon docon-navigate-external x-hidden-focus"></span> </a> ve yardımcı GitHub deposu bilişsel <a href="https://github.com/Microsoft/cognitive-services-speech-sdk-js" target="_blank">-Hizmetler-konuşma-SDK-js <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="JavaScript" src="https://docs.microsoft.com/media/logos/logo_js.svg"  width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> JavaScript konuşma SDK 'Sı NPM paketi olarak kullanılabilir olsa da, bu nedenle hem istemci Web tarayıcıları hem de Node. js bunu tüketebilir-her bir ortamın çeşitli mimari etkilerini göz önünde bulundurun. Örneğin, <a href="https://en.wikipedia.org/wiki/Document_Object_Model" target="_blank">belge nesne modeli (DOM) <span class="docon docon-navigate-external x-hidden-focus"></span> </a> , <a href="https://nodejs.org/api/fs.html" target="_blank">dosya sisteminin <span class="docon docon-navigate-external x-hidden-focus"></span> </a> istemci tarafı uygulamalar tarafından kullanılamadığı gibi sunucu tarafı uygulamalar için kullanılamaz.

### <a name="nodejs-package-manager-npm"></a>Node. js Paket Yöneticisi (NPM)

JavaScript konuşma SDK 'sını yüklemek için aşağıdaki `npm install` komutu çalıştırın.

```nodejs
npm install microsoft-cognitiveservices-speech-sdk
```

### <a name="html-script-tag"></a>HTML betiği etiketi

Alternatif olarak, <a href="https://www.jsdelivr.com/package/npm/microsoft-cognitiveservices-speech-sdk" target="_blank"> **Jsdelivr** NPM <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>'e bağlı olarak `<head>` HTMLS öğesine doğrudan bir `<script>` etiket ekleyebilirsiniz.

```html
<script src="https://cdn.jsdelivr.net/npm/microsoft-cognitiveservices-speech-sdk@1.10.1/distrib/lib/microsoft.cognitiveservices.speech.sdk.min.js">
</script>
```

Daha fazla bilgi için bkz. <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/browser" target="_blank">Web tarayıcısı konuşma hızlı başlangıç <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>.
