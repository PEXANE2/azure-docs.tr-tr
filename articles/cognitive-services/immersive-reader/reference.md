---
title: Modern Okuyucu SDK başvurusu
titleSuffix: Azure Cognitive Services
description: Modern Okuyucu SDK 'Sı için başvuru
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 22860e0798ec852b6d25da27e108befe8a3089a2
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68488825"
---
# <a name="immersive-reader-sdk-reference"></a>Modern Okuyucu SDK başvurusu

Modern Okuyucu SDK 'Sı, tam ekran okuyucuyu Web uygulamanızla tümleştirmenize olanak tanıyan bir JavaScript kitaplığıdır.

## <a name="functions"></a>İşlevler

SDK tek bir işlevi `ImmersiveReader.launchAsync(token, subdomain, content, options)`sunar.

### <a name="launchasync"></a>launchAsync

Web uygulamanızda bir `iframe` içinde tam ekran okuyucu başlatır.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<HTMLDivElement>;
```

#### <a name="parameters"></a>Parametreler

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| `token` | dize | Azure AD kimlik doğrulama belirteci. Bkz. [Azure AD kimlik doğrulaması nasıl yapılır](./azure-active-directory-authentication.md). |
| `subdomain` | dize | Azure 'daki tam ekran okuyucu kaynağınızın özel alt etki alanı. Bkz. [Azure AD kimlik doğrulaması nasıl yapılır](./azure-active-directory-authentication.md). |
| `content` | [İçeriği](#content) | Tam ekran okuyucu 'da gösterilecek içeriği içeren nesne. |
| `options` | [Seçenekler](#options) | Modern okuyucunun belirli davranışlarını yapılandırmaya yönelik seçenekler. İsteğe bağlı. |

#### <a name="returns"></a>Döndürür

Modern okuyucu `Promise<HTMLDivElement>` yüklendiğinde çözümlenen bir döndürür. Yalnızca alt öğesi, `div` tam ekran okuyucu sayfasını içeren bir `iframe` öğe olan bir öğe olarak çözümlenir.`Promise`

#### <a name="exceptions"></a>Özel durumlar

Tam ekran `Promise` okuyucu yüklenemezse, döndürülen bir [`Error`](#error) nesne ile reddedilir. Daha fazla bilgi için bkz. [hata kodları](#error-codes).

## <a name="types"></a>Türler

### <a name="content"></a>İçerik

Tam ekran okuyucu 'da gösterilecek içeriği içerir.

```typescript
{
    title?: string;      // Title text shown at the top of the Immersive Reader (optional)
    chunks: [ {          // Array of chunks
        content: string; // Plain text string
        lang?: string;   // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
        mimeType?: string; // MIME type of the content (optional). Defaults to 'text/plain' if not specified.
    } ];
}
```

#### <a name="supported-mime-types"></a>Desteklenen MIME türleri

| MIME Türü | Açıklama |
| --------- | ----------- |
| metin/düz | Düz metin. |
| Application/MathML + XML | Matematik biçimlendirme dili (MathML). [Daha fazla bilgi edinin](https://developer.mozilla.org/en-US/docs/Web/MathML).

### <a name="options"></a>Seçenekler

Modern okuyucunun belirli davranışlarını yapılandıran özellikler içerir.

```typescript
{
    uiLang?: string;   // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;  // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number; // Z-index of the iframe that will be created (default is 1000)
    useWebview?: boolean; // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
}
```

### <a name="error"></a>Hata

Hata hakkındaki bilgileri içerir.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Hata kodları

| Kod | Açıklama |
| ---- | ----------- |
| BadArgument | Sağlanan bağımsız değişken geçersiz, Ayrıntılar `message` için bkz. |
| zaman aşımı | Tam ekran okuyucusu belirtilen zaman aşımı süresi içinde yüklenemedi. |
| Tokenaşımına uğradı| Sağlanan belirtecin geçerliliği zaman aşımına uğradı. |

## <a name="launching-the-immersive-reader"></a>Modern okuyucu başlatılıyor

SDK, tam ekran okuyucuyu başlatmaya yönelik düğme için varsayılan stil sağlar. Bu stillendirme özelliğini etkinleştirmek için Classözniteliğinikullanın.`immersive-reader-button`

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>İsteğe bağlı öznitelikler

Düğmenin genel görünümünü yapılandırmak için aşağıdaki öznitelikleri kullanın.

| Öznitelik | Açıklama |
| --------- | ----------- |
| `data-button-style` | Düğmenin stilini ayarlar. `icon` ,`text`Veya olabilir`iconAndText`. Varsayılan olarak `icon`. |
| `data-locale` | Yerel ayarı ayarlar, ör `en-US`. `fr-FR` Varsayılan olarak Ingilizce 'Dir. |
| `data-icon-px-size` | Simgenin boyutunu piksel cinsinden ayarlar. Varsayılan değer 20 px olur. |

## <a name="browser-support"></a>Tarayıcı desteği

Tam ekran okuyucu ile en iyi deneyim için aşağıdaki tarayıcıların en son sürümlerini kullanın.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub 'Da modern Okuyucu SDK 'sını](https://github.com/Microsoft/immersive-reader-sdk) keşfet
* [Hızlı Başlangıç: Tam ekran okuyucu (C#) başlatan bir Web uygulaması oluşturma](./quickstart.md)