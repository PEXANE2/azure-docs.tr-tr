---
title: Sürükleyici Okuyucu SDK Referans
titleSuffix: Azure Cognitive Services
description: Immersive Reader SDK, Sürükleyici Okuyucu'yu uygulamanız için entegre etmenizi sağlayan bir JavaScript kitaplığı içerir.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: b20a3e6dd3b32b183bbf34dbefd76f0e4cd56b99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76156412"
---
# <a name="immersive-reader-sdk-reference-guide"></a>Sürükleyici Okuyucu SDK Başvuru Kılavuzu

Immersive Reader SDK, Sürükleyici Okuyucu'yu uygulamanız için entegre etmenizi sağlayan bir JavaScript kitaplığı içerir.

## <a name="functions"></a>İşlevler

SDK işlevleri ortaya çıkarır:

- [`ImmersiveReader.launchAsync(token, subdomain, content, options)`](#launchasync)

- [`ImmersiveReader.close()`](#close)

- [`ImmersiveReader.renderButtons(options)`](#renderbuttons)

## <a name="launchasync"></a>fırlatmaAsync

Bir web uygulama içinde `iframe` Immersive Reader başlattı.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<LaunchResponse>;
```

### <a name="parameters"></a>Parametreler

| Adı | Tür | Açıklama |
| ---- | ---- |------------ |
| `token` | string | Azure AD kimlik doğrulama belirteci. |
| `subdomain` | string | Azure'daki Sürükleyici Okuyucu kaynağınızın özel alt etki alanı. |
| `content` | [İçerik](#content) | Sürükleyici Okuyucu'da gösterilecek içeriği içeren bir nesne. |
| `options` | [Seçenekler](#options) | Sürükleyici Reader'ın belirli davranışlarını yapılandırma seçenekleri. İsteğe bağlı. |

### <a name="returns"></a>Döndürür

Sürükleyici `Promise<LaunchResponse>`Okuyucu yüklendiğinde çözen bir , döndürür. Bir `Promise` [`LaunchResponse`](#launchresponse) nesneye giderir.

### <a name="exceptions"></a>Özel durumlar

Döndürülen `Promise` Okuyucu yüklenmezse bir [`Error`](#error) nesneyle birlikte reddedilir. Daha fazla bilgi için [hata kodlarına](#error-codes)bakın.

## <a name="close"></a>close

Sürükleyici Okuyucuyu kapatır.

Bu işlev için bir örnek kullanım örneği, çıkış ```hideExitButton: true``` düğmesinin [seçenekleri](#options)ayarlayarak gizli olmasıdır. Ardından, farklı bir düğme (örneğin mobil üstbilginin arka ```close``` oku) tıklatıldığında bu işlevi arayabilir.

```typescript
close(): void;
```

## <a name="renderbuttons"></a>renderDüğmeler

Bu işlev, belgenin Sürükleyici Reader düğmesi öğelerini stilleri ve güncelleştirir. Sağlanırsa, ```options.elements``` bu işlev içindeki ```options.elements```düğmeleri işleyecek. Aksi takdirde, düğmeler sınıfına ```immersive-reader-button```sahip belgenin öğeleri içinde işlenir.

Bu işlev, pencere yüklendiğinde SDK tarafından otomatik olarak çağrılır.

Daha fazla görüntüleme seçeneği için [İsteğe Bağlı Özniteliklere](#optional-attributes) bakın.

```typescript
renderButtons(options?: RenderButtonsOptions): void;
```

### <a name="parameters"></a>Parametreler

| Adı | Tür | Açıklama |
| ---- | ---- |------------ |
| `options` | [RenderButtonsOptions](#renderbuttonsoptions) | RenderButtons işlevinin belirli davranışlarını yapılandırma seçenekleri. İsteğe bağlı. |

## <a name="types"></a>Türler

### <a name="content"></a>İçerik

Sürükleyici Okuyucu'da gösterilecek içeriği içerir.

```typescript
{
    title?: string;    // Title text shown at the top of the Immersive Reader (optional)
    chunks: Chunk[];   // Array of chunks
}
```

### <a name="chunk"></a>Öbek

Sürükleyici Okuyucu'nun İçeriğine aktalacaktır veri tek bir yığın.

```typescript
{
    content: string;        // Plain text string
    lang?: string;          // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
    mimeType?: string;      // MIME type of the content (optional). Currently 'text/plain', 'application/mathml+xml', and 'text/html' are supported. Defaults to 'text/plain' if not specified.
}
```

### <a name="launchresponse"></a>LaunchResponse

Aramadan gelen yanıtı `ImmersiveReader.launchAsync`içerir.

```typescript
{
    container: HTMLDivElement;    // HTML element which contains the Immersive Reader iframe
    sessionId: string;            // Globally unique identifier for this session, used for debugging
}
```

### <a name="cookiepolicy-enum"></a>CookiePoliçe enum

Sürükleyici Okuyucu'nun çerez kullanımı için ilkeyi ayarlamak için kullanılan bir enum. [Seçeneklere](#options)bakın.

```typescript
enum CookiePolicy { Disable, Enable }
```

#### <a name="supported-mime-types"></a>Desteklenen MIME türleri

| MIME Türü | Açıklama |
| --------- | ----------- |
| metin/düz | Düz metin. |
| text/html | HTML içeriği. [Daha fazlasını öğrenin](#html-support)|
| uygulama/mathml+xml | Matematiksel Biçimlendirme Dili (MathML). [Daha fazla bilgi edinin](./how-to/display-math.md).
| uygulama/vnd.openxmlformats-officedocument.wordprocessingml.document | Microsoft Word .docx biçimli belge.

### <a name="html-support"></a>HTML Desteği

| HTML | Desteklenen İçerik |
| --------- | ----------- |
| Yazı Tipi Stilleri | Kalın, Italik, Altı Çizili, Kod, Strikethrough, Superscript, Subscript |
| Sırasız listeler | Disk, Daire, Kare |
| Sipariş Listeleri | Ondalık, Üst-Alfa, Alt-Alfa, Üst-Roma, Alt-Roma |
| Köprüler | Çok Yakında |

Desteklenmeyen etiketler karşılaştırılarak işlenir. Görüntüler ve tablolar şu anda desteklenmiyor.

### <a name="options"></a>Seçenekler

Sürükleyici Reader'ın belirli davranışlarını yapılandıran özellikler içerir.

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

Sürükleyici Reader düğmelerini oluşturma seçenekleri.

```typescript
{
    elements: HTMLDivElement[];    // Elements to render the Immersive Reader buttons in
}
```

### <a name="error"></a>Hata

Hata hakkında bilgi içerir.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Hata kodları

| Kod | Açıklama |
| ---- | ----------- |
| BadArgument | Verilen bağımsız değişken geçersizdir, ayrıntılar için bkz. `message` |
| Zaman aşımı | Immersive Reader belirtilen zaman aşımı içinde yüklemek için başarısız oldu. |
| TokenExpired | Verilen belirteç süresi doldu. |
| Bastırma | Arama oranı sınırı aşıldı. |

## <a name="launching-the-immersive-reader"></a>Sürükleyici Okuyucu başlatma

SDK, Sürükleyici Okuyucu'yu başlatmak için düğme için varsayılan stil sağlar. Bu `immersive-reader-button` stili etkinleştirmek için sınıf özniteliğini kullanın. Daha fazla bilgi için [bu makaleye](./how-to-customize-launch-button.md) bakın.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>İsteğe bağlı öznitelikler

Düğmenin görünümünü ve hissini yapılandırmak için aşağıdaki öznitelikleri kullanın.

| Öznitelik | Açıklama |
| --------- | ----------- |
| `data-button-style` | Düğmenin stilini ayarlar. Olabilir `icon`, `text`veya `iconAndText`. Varsayılan `icon`değer. |
| `data-locale` | Yerel alanı ayarlar. Örneğin `en-US` veya `fr-FR` olabilir. Varsayılan İngilizce. `en` |
| `data-icon-px-size` | Simgenin boyutunu pikselolarak ayarlar. Varsayılan olarak 20px'e kadar. |

## <a name="browser-support"></a>Tarayıcı desteği

Immersive Reader ile en iyi deneyimi yaşamak için aşağıdaki tarayıcıların en son sürümlerini kullanın.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub'da Sürükleyici Reader SDK'yı](https://github.com/microsoft/immersive-reader-sdk) keşfedin
* [Quickstart: Sürükleyici Okuyucu (C#) başlatan bir web uygulaması oluşturun](./quickstart.md)
