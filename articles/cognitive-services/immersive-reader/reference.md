---
title: Modern Okuyucu SDK başvurusu
titleSuffix: Azure Cognitive Services
description: Modern Okuyucu SDK 'Sı, tam ekran okuyucuyu uygulamanızla tümleştirmenize olanak tanıyan bir JavaScript kitaplığı içerir.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: cb88fb24ceed943d4104da6914959e4b79c35571
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231926"
---
# <a name="immersive-reader-sdk-reference-guide"></a>Modern Okuyucu SDK 'Sı başvuru kılavuzu

Modern Okuyucu SDK 'Sı, tam ekran okuyucuyu uygulamanızla tümleştirmenize olanak tanıyan bir JavaScript kitaplığı içerir.

## <a name="functions"></a>İşlevler

SDK işlevleri kullanıma sunar:

- [`ImmersiveReader.launchAsync(token, subdomain, content, options)`](#launchasync)

- [`ImmersiveReader.close()`](#close)

- [`ImmersiveReader.renderButtons(options)`](#renderbuttons)

## <a name="launchasync"></a>launchAsync

Web uygulamanızda bir `iframe` Içinde tam ekran okuyucu başlatır.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<LaunchResponse>;
```

### <a name="parameters"></a>Parametreler

| Adı | Tür | Açıklama |
| ---- | ---- |------------ |
| `token` | string | Azure AD kimlik doğrulama belirteci. |
| `subdomain` | string | Azure 'daki tam ekran okuyucu kaynağınızın özel alt etki alanı. |
| `content` | [İçerik](#content) | Tam ekran okuyucu 'da gösterilecek içeriği içeren nesne. |
| `options` | [Seçenekler](#options) | Modern okuyucunun belirli davranışlarını yapılandırmaya yönelik seçenekler. İsteğe bağlı. |

### <a name="returns"></a>Döndürür

Derinlikli `Promise<LaunchResponse>`okuyucu yüklendiğinde çözümlenen bir döndürür. Bir `Promise` [`LaunchResponse`](#launchresponse) nesne olarak çözümlenir.

### <a name="exceptions"></a>Özel Durumlar

Tam ekran `Promise` okuyucu yüklenemezse, döndürülen bir [`Error`](#error) nesne ile reddedilir. Daha fazla bilgi için bkz. [hata kodları](#error-codes).

## <a name="close"></a>close

Tam ekran okuyucuyu kapatır.

Bu işlev için bir örnek kullanım örneği, ```hideExitButton: true``` [Seçenekler](#options)' de ayar yaparak çıkış düğmesidir. Ardından, farklı bir düğme (örneğin, bir mobil üstbilginin geri oku) tıklandığında bu ```close``` işlevi çağırabilir.

```typescript
close(): void;
```

## <a name="renderbuttons"></a>renderButtons

Bu işlev, belgenin tam ekran okuyucusu düğme öğelerini stiller ve güncelleştirir. ```options.elements``` Sağlanmışsa, bu işlev içindeki ```options.elements```düğmeleri işleyebilir. Aksi halde düğmeler, belgenin sınıfı ```immersive-reader-button```olan öğeleri içinde işlenir.

Bu işlev, pencere yüklendiğinde SDK tarafından otomatik olarak çağırılır.

Daha fazla işleme seçeneği için bkz. [Isteğe bağlı öznitelikler](#optional-attributes) .

```typescript
renderButtons(options?: RenderButtonsOptions): void;
```

### <a name="parameters"></a>Parametreler

| Adı | Tür | Açıklama |
| ---- | ---- |------------ |
| `options` | [RenderButtonsOptions](#renderbuttonsoptions) | RenderButtons işlevinin belirli davranışlarını yapılandırmaya yönelik seçenekler. İsteğe bağlı. |

## <a name="types"></a>Türler

### <a name="content"></a>İçerik

Tam ekran okuyucu 'da gösterilecek içeriği içerir.

```typescript
{
    title?: string;    // Title text shown at the top of the Immersive Reader (optional)
    chunks: Chunk[];   // Array of chunks
}
```

### <a name="chunk"></a>Öbek

Tam ekran okuyucu Içeriğine geçirilecek tek bir veri öbeği.

```typescript
{
    content: string;        // Plain text string
    lang?: string;          // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
    mimeType?: string;      // MIME type of the content (optional). Currently 'text/plain', 'application/mathml+xml', and 'text/html' are supported. Defaults to 'text/plain' if not specified.
}
```

### <a name="launchresponse"></a>LaunchResponse

Çağrısının yanıtını içerir `ImmersiveReader.launchAsync`.

```typescript
{
    container: HTMLDivElement;    // HTML element which contains the Immersive Reader iframe
    sessionId: string;            // Globally unique identifier for this session, used for debugging
}
```

### <a name="cookiepolicy-enum"></a>Tanımlama listesi ıepolicy Enum

Derinlikli okuyucunun tanımlama bilgisi kullanımı için ilkeyi ayarlamak üzere kullanılan bir sabit listesi. Bkz. [Seçenekler](#options).

```typescript
enum CookiePolicy { Disable, Enable }
```

#### <a name="supported-mime-types"></a>Desteklenen MIME türleri

| MIME Türü | Açıklama |
| --------- | ----------- |
| metin/düz | Düz metin. |
| text/html | HTML içeriği. [Daha fazlasını öğrenin](#html-support)|
| Application/MathML + XML | Matematik biçimlendirme dili (MathML). [Daha fazla bilgi edinin](./how-to/display-math.md).
| application/vnd. openxmlformats-officedocument. WordprocessingML. Document | Microsoft Word. docx biçim belgesi.

### <a name="html-support"></a>HTML desteği

| HTML | Desteklenen Içerik |
| --------- | ----------- |
| Yazı tipi stilleri | Kalın, Italik, altı çizili, kod, üstü çizili, üst simge, alt simge |
| Sırasız listeler | Disk, daire, kare |
| Sıralı listeler | Ondalık, büyük Alfa, alt Alfa, büyük Latin, alt roman |

Desteklenmeyen Etiketler comparably işlenecek. Görüntüler ve tablolar şu anda desteklenmiyor.

### <a name="options"></a>Seçenekler

Modern okuyucunun belirli davranışlarını yapılandıran özellikler içerir.

```typescript
{
    uiLang?: string;           // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;          // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number;         // Z-index of the iframe that will be created (default is 1000)
    useWebview?: boolean;      // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
    onExit?: () => any;        // Executes when the Immersive Reader exits
    customDomain?: string;     // Reserved for internal use. Custom domain where the Immersive Reader webapp is hosted (default is null).
    allowFullscreen?: boolean; // The ability to toggle fullscreen (default is true).
    hideExitButton?: boolean;  // Whether or not to hide the Immersive Reader's exit button arrow (default is false). This should only be true if there is an alternative mechanism provided to exit the Immersive Reader (e.g a mobile toolbar's back arrow).
    cookiePolicy?: CookiePolicy; // Setting for the Immersive Reader's cookie usage (default is CookiePolicy.Disable). It's the responsibility of the host application to obtain any necessary user consent in accordance with EU Cookie Compliance Policy.
}
```

### <a name="renderbuttonsoptions"></a>RenderButtonsOptions

Tam ekran okuyucu düğmelerini işleme seçenekleri.

```typescript
{
    elements: HTMLDivElement[];    // Elements to render the Immersive Reader buttons in
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
| BadArgument | Sağlanan bağımsız değişken geçersiz, Ayrıntılar `message` için bkz.. |
| Zaman aşımı | Tam ekran okuyucusu belirtilen zaman aşımı süresi içinde yüklenemedi. |
| TokenExpired | Sağlanan belirtecin geçerliliği zaman aşımına uğradı. |
| Sürecek | Çağrı hızı sınırı aşıldı. |

## <a name="launching-the-immersive-reader"></a>Modern okuyucu başlatılıyor

SDK, tam ekran okuyucuyu başlatmaya yönelik düğme için varsayılan stil sağlar. Bu stillendirme özelliğini etkinleştirmek için `immersive-reader-button` Class özniteliğini kullanın. Daha fazla bilgi için [Bu makaleye](./how-to-customize-launch-button.md) bakın.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>İsteğe bağlı öznitelikler

Düğmenin genel görünümünü yapılandırmak için aşağıdaki öznitelikleri kullanın.

| Öznitelik | Açıklama |
| --------- | ----------- |
| `data-button-style` | Düğmenin stilini ayarlar. `icon`, `text`Veya `iconAndText`olabilir. Varsayılan olarak `icon`olur. |
| `data-locale` | Yerel ayarı ayarlar. Örneğin `en-US` veya `fr-FR` olabilir. Varsayılan olarak Ingilizce `en`'dir. |
| `data-icon-px-size` | Simgenin boyutunu piksel cinsinden ayarlar. Varsayılan değer 20 px olur. |

## <a name="browser-support"></a>Tarayıcı desteği

Tam ekran okuyucu ile en iyi deneyim için aşağıdaki tarayıcıların en son sürümlerini kullanın.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub 'Da modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) keşfet
* [Hızlı başlangıç: tam ekran okuyucuyu Başlatan bir Web uygulaması oluşturma (C#)](./quickstart.md)
