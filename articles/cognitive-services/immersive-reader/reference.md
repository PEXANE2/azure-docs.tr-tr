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
ms.openlocfilehash: 73322cdee151969e6e765690284bbffc1c871f4e
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090202"
---
# <a name="immersive-reader-javascript-sdk-reference-v11"></a>Modern okuyucu JavaScript SDK başvurusu (v 1.1)

Modern Okuyucu SDK 'Sı, tam ekran okuyucuyu uygulamanızla tümleştirmenize olanak tanıyan bir JavaScript kitaplığı içerir.

## <a name="functions"></a>İşlevler

SDK işlevleri kullanıma sunar:

- [`ImmersiveReader.launchAsync(token, subdomain, content, options)`](#launchasync)

- [`ImmersiveReader.close()`](#close)

- [`ImmersiveReader.renderButtons(options)`](#renderbuttons)

<br>

## <a name="launchasync"></a>launchAsync

Web uygulamanızda bir içinde tam ekran okuyucu başlatır `iframe` . İçeriğinizin boyutunun en fazla 50 MB ile sınırlı olduğunu unutmayın.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<LaunchResponse>;
```

#### <a name="launchasync-parameters"></a>launchAsync parametreleri

| Ad | Tür | Description |
| ---- | ---- |------------ |
| `token` | dize | Azure AD kimlik doğrulama belirteci. Daha ayrıntılı bilgi için bkz. [nasıl yapılır-derinlikli okuyucu kaynağı oluşturma](./how-to-create-immersive-reader.md) . |
| `subdomain` | string | Azure 'daki tam ekran okuyucu kaynağınızın özel alt etki alanı. Daha ayrıntılı bilgi için bkz. [nasıl yapılır-derinlikli okuyucu kaynağı oluşturma](./how-to-create-immersive-reader.md) . |
| `content` | [İçerik](#content) | Tam ekran okuyucu 'da gösterilecek içeriği içeren nesne. |
| `options` | [Seçenekler](#options) | Modern okuyucunun belirli davranışlarını yapılandırmaya yönelik seçenekler. İsteğe bağlı. |

#### <a name="returns"></a>Döndürülenler

`Promise<LaunchResponse>`Derinlikli okuyucu yüklendiğinde çözümlenen bir döndürür. `Promise`Bir nesne olarak çözümlenir [`LaunchResponse`](#launchresponse) .

#### <a name="exceptions"></a>Özel Durumlar

`Promise` [`Error`](#error) Tam ekran okuyucu yüklenemezse, döndürülen bir nesne ile reddedilir. Daha fazla bilgi için bkz. [hata kodları](#error-codes).

<br>

## <a name="close"></a>close

Tam ekran okuyucuyu kapatır.

Bu işlev için bir örnek kullanım örneği, ```hideExitButton: true``` [Seçenekler](#options)' de ayar yaparak çıkış düğmesidir. Ardından, farklı bir düğme (örneğin, bir mobil üstbilginin geri oku) tıklandığında bu işlevi çağırabilir ```close``` .

```typescript
close(): void;
```

<br>

## <a name="immersive-reader-launch-button"></a>Modern okuyucu başlatma düğmesi

SDK, tam ekran okuyucuyu başlatmaya yönelik düğme için varsayılan stil sağlar. `immersive-reader-button`Bu stillendirme özelliğini etkinleştirmek için Class özniteliğini kullanın. Daha fazla ayrıntı için bkz. [nasıl yapılır okuyucu düğmesini özelleştirme düğmesi](./how-to-customize-launch-button.md) .

```html
<div class='immersive-reader-button'></div>
```

#### <a name="optional-attributes"></a>İsteğe bağlı öznitelikler

Düğmenin genel görünümünü yapılandırmak için aşağıdaki öznitelikleri kullanın.

| Öznitelik | Açıklama |
| --------- | ----------- |
| `data-button-style` | Düğmenin stilini ayarlar. `icon`, Veya olabilir `text` `iconAndText` . Varsayılan olarak olur `icon` . |
| `data-locale` | Yerel ayarı ayarlar. Örneğin `en-US` veya `fr-FR` olabilir. Varsayılan olarak Ingilizce 'Dir `en` . |
| `data-icon-px-size` | Simgenin boyutunu piksel cinsinden ayarlar. Varsayılan değer 20 px olur. |

<br>

## <a name="renderbuttons"></a>renderButtons

```renderButtons``` [Nasıl yapılır, derinlikli okuyucu düğme kılavuzunu özelleştirmek için](./how-to-customize-launch-button.md) işlevi gerekli değildir.

Bu işlev, belgenin tam ekran okuyucusu düğme öğelerini stiller ve güncelleştirir. ```options.elements```Sağlanırsa, düğmeleri ' de verilen her öğe içinde işlenir ```options.elements``` . , ```options.elements``` Belgenizde tam ekran okuyucuyu başlatmak için kullanabileceğiniz birden fazla bölüm varsa ve aynı stille birden çok düğme işlemek ya da düğmeleri basit ve tutarlı bir tasarım düzeniyle işlemek istiyorsanız, parametresini kullanmak faydalıdır. Bu işlevi [RenderButtons seçenekleri](#renderbuttons-options) parametresiyle birlikte kullanmak için, ```ImmersiveReader.renderButtons(options: RenderButtonsOptions);``` Aşağıdaki kod parçacığında gösterildiği gibi sayfa yüküne çağrı yapın. Aksi takdirde, düğmeler, ```immersive-reader-button``` [nasıl yapılır okuyucu düğmesini özelleştirmek için](./how-to-customize-launch-button.md) gösterildiği gibi, sınıfı bulunan belge öğeleri içinde işlenir.

```typescript
// This snippet assumes there are two empty div elements in
// the page HTML, button1 and button2.
const btn1: HTMLDivElement = document.getElementById('button1');
const btn2: HTMLDivElement = document.getElementById('button2');
const btns: HTMLDivElement[] = [btn1, btn2];
ImmersiveReader.renderButtons({elements: btns});
```

Daha fazla işleme seçeneği için yukarıdaki [Isteğe bağlı öznitelikleri](#optional-attributes) inceleyin. Bu seçenekleri kullanmak için, ```HTMLDivElement``` sayfa HTML 'inizdeki her bir seçenek özniteliğini ekleyin.

```typescript
renderButtons(options?: RenderButtonsOptions): void;
```

#### <a name="renderbuttons-parameters"></a>renderButtons parametreleri

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| `options` | [renderButtons seçenekleri](#renderbuttons-options) | RenderButtons işlevinin belirli davranışlarını yapılandırmaya yönelik seçenekler. İsteğe bağlı. |

### <a name="renderbuttons-options"></a>renderButtons seçenekleri

Tam ekran okuyucu düğmelerini işleme seçenekleri.

```typescript
{
    elements: HTMLDivElement[];
}
```

#### <a name="renderbuttons-options-parameters"></a>renderButtons seçenek parametreleri

| Ayar | Tür | Açıklama |
| ------- | ---- | ----------- |
| öğeler | HTMLDivElement [] | İçindeki derinlikli okuyucu düğmelerini işlemek için öğeleri. |

##### `-elements`
```Parameters
Type: HTMLDivElement[]
Required: false
```

<br>

## <a name="launchresponse"></a>LaunchResponse

Çağrısının yanıtını içerir `ImmersiveReader.launchAsync` . `iframe`Ile modern okuyucuyu içeren bir başvuruya aracılığıyla erişilebileceğini unutmayın `container.firstChild` .

```typescript
{
    container: HTMLDivElement;
    sessionId: string;
}
```

#### <a name="launchresponse-parameters"></a>LaunchResponse parametreleri

| Ayar | Tür | Açıklama |
| ------- | ---- | ----------- |
| kapsayıcı | HTMLDivElement | Tam ekran okuyucu iframe 'i içeren HTML öğesi. |
| Kimliği | Dize | Bu oturum için, hata ayıklama için kullanılan genel benzersiz tanımlayıcı. |
 
## <a name="error"></a>Hata

Bir hata hakkındaki bilgileri içerir.

```typescript
{
    code: string;
    message: string;
}
```

#### <a name="error-parameters"></a>Hata parametreleri

| Ayar | Tür | Açıklama |
| ------- | ---- | ----------- |
| kod | Dize | Bir hata kodları kümesinden biri. Bkz. [hata kodları](#error-codes). |
| message | Dize | Hatanın insan tarafından okunabilir gösterimi. |

#### <a name="error-codes"></a>Hata kodları

| Kod | Açıklama |
| ---- | ----------- |
| BadArgument | Sağlanan bağımsız değişken geçersiz, `message` [hata](#error)parametresine bakın. |
| Zaman aşımı | Tam ekran okuyucusu belirtilen zaman aşımı süresi içinde yüklenemedi. |
| TokenExpired | Sağlanan belirtecin geçerliliği zaman aşımına uğradı. |
| Sürecek | Çağrı hızı sınırı aşıldı. |

<br>

## <a name="types"></a>Türler

### <a name="content"></a>Content

Tam ekran okuyucu 'da gösterilecek içeriği içerir.

```typescript
{
    title?: string;
    chunks: Chunk[];
}
```

#### <a name="content-parameters"></a>İçerik parametreleri

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| başlık | Dize | Tam ekran okuyucunun en üstünde gösterilen başlık metni (isteğe bağlı) |
| Öbek | [Öbek []](#chunk) | Öbeklerin dizisi |

##### `-title`
```Parameters
Type: String
Required: false
Default value: "Immersive Reader" 
```

##### `-chunks`
```Parameters
Type: Chunk[]
Required: true
Default value: null 
```

<br>

### <a name="chunk"></a>Öbek

Tam ekran okuyucu Içeriğine geçirilecek tek bir veri öbeği.

```typescript
{
    content: string;
    lang?: string;
    mimeType?: string;
}
```

#### <a name="chunk-parameters"></a>Öbek parametreleri

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| içerik | Dize | Tam ekran okuyucusuna gönderilen içeriği içeren dize. |
| lang | Dize | Metnin dili, bu değer IETF BCP 47 dil etiketi biçimindedir, örn., ES-ES. Belirtilmezse, dil otomatik olarak algılanır. Bkz. [Desteklenen Diller](#supported-languages). |
| mimeType | string | Düz metin, MathML, HTML & Microsoft Word DOCX biçimleri desteklenir. Daha fazla ayrıntı için bkz. [desteklenen MIME türleri](#supported-mime-types) . |

##### `-content`
```Parameters
Type: String
Required: true
Default value: null 
```

##### `-lang`
```Parameters
Type: String
Required: false
Default value: Automatically detected 
```

##### `-mimeType`
```Parameters
Type: String
Required: false
Default value: "text/plain"
```

#### <a name="supported-mime-types"></a>Desteklenen MIME türleri

| MIME Türü | Açıklama |
| --------- | ----------- |
| metin/düz | Düz metin. |
| text/html | HTML içeriği. [Daha fazla bilgi edinin](#html-support)|
| Application/MathML + XML | Matematik biçimlendirme dili (MathML). [Daha fazla bilgi edinin](./how-to/display-math.md).
| Uygulama/vnd.openxmlformats-officedocument.wordprocessingml.document | Microsoft Word. docx biçim belgesi.


<br>

## <a name="options"></a>Seçenekler

Modern okuyucunun belirli davranışlarını yapılandıran özellikler içerir.

```typescript
{
    uiLang?: string;
    timeout?: number;
    uiZIndex?: number;
    useWebview?: boolean;
    onExit?: () => any;
    allowFullscreen?: boolean;
    hideExitButton?: boolean;
    cookiePolicy?: CookiePolicy;
    disableFirstRun?: boolean;
    readAloudOptions?: ReadAloudOptions;
    translationOptions?: TranslationOptions;
    displayOptions?: DisplayOptions;
    preferences?: string;
    onPreferencesChanged?: (value: string) => any;
    customDomain?: string;
}
```

#### <a name="options-parameters"></a>Seçenek parametreleri

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| UILang | Dize | Kullanıcı arabiriminin dili, bu değer IETF BCP 47 dil etiketi biçimindedir, örn., ES-ES. Belirtilmemişse, varsayılan olarak tarayıcı dili olur. |
| timeout | Sayı | [LaunchAsync](#launchasync) Before zaman aşımı hatasıyla başarısız olan süre (milisaniye cinsinden) (varsayılan değer 15000 MS 'dir). Bu zaman aşımı yalnızca okuyucu sayfası açıldığında ve değer değiştirici başladığında başarıyı gözlemlenen okuyucu sayfasının ilk başlatması için geçerlidir. Zaman aşımı ayarlaması gerekli olmamalıdır. |
| Uizındex | Sayı | Oluşturulacak iframe 'nin Z dizini (varsayılan değer 1000 ' dir). |
| useWebview | Boole| Chrome uygulamalarıyla uyumluluk için bir iframe yerine WebView etiketi kullanın (varsayılan değer false 'dur). |
| onExit | İşlev | Modern okuyucu çıktığında yürütülür. |
| allowFullscreen | Boole | Tam ekran geçiş özelliği (varsayılan değer true 'dur). |
| hideExitButton | Boole | Tam ekran okuyucunun çıkış düğmesine okunun gizlenmeyeceğini belirtir (varsayılan değer false). Bu, yalnızca tam ekran okuyucudan çıkmak için bir alternatif mekanizma (örn. bir mobil araç çubuğunun geri oku) varsa doğru olmalıdır. |
| Tanımlama, ıepolicy | [Tanımlama, ıepolicy](#cookiepolicy-options) | Derinlikli okuyucunun tanımlama bilgisi kullanımı için ayarlama (varsayılan, tanımlama bilgileri \ *ilke. Disable*). Bu, ana bilgisayar uygulamasının, AB tanımlama bilgisi uyumluluk Ilkesine uygun olan tüm gerekli Kullanıcı onayını elde etmek için sorumluluğudur. Bkz. [tanımlama bilgisi Ilkesi seçenekleri](#cookiepolicy-options). |
| disableFirstRun | Boole | İlk çalıştırma deneyimini devre dışı bırakın. |
| Readaloudoçen'lar | [Readaloudoçen'lar](#readaloudoptions) | Okumayı yüksek sesle yapılandırma seçenekleri. |
| translationOptions | [TranslationOptions](#translationoptions) | Çeviri yapılandırma seçenekleri. |
| displayOptions | [DisplayOptions](#displayoptions) | Metin boyutunu, yazı tipini, vb. yapılandırma seçenekleri |
| tercihlerindeki | Dize | Onpreferences'ten döndürülen dize, kullanıcının tam ekran okuyucudaki tercihlerini temsil eden değişti. Daha fazla bilgi için bkz. [Ayarlar parametreleri](#settings-parameters) ve [nasıl yapılır? Kullanıcı tercihlerini depolama](./how-to-store-user-preferences.md) . |
| onPreferencesChanged | İşlev | Kullanıcı tercihleri değiştiğinde yürütülür. Daha fazla bilgi için bkz. [Kullanıcı tercihlerini depolama](./how-to-store-user-preferences.md) . |
| customDomain | Dize | Dahili kullanım için ayrılmıştır. Tam ekran okuyucu WebApp 'nin barındırıldığı özel etki alanı (varsayılan değer null). |

##### `-uiLang`
```Parameters
Type: String
Required: false
Default value: User's browser language 
```

##### `-timeout`
```Parameters
Type: Number
Required: false
Default value: 15000
```

##### `-uiZIndex`
```Parameters
Type: Number
Required: false
Default value: 1000
```

##### `-onExit`
```Parameters
Type: Function
Required: false
Default value: null
```

##### `-preferences`

> [!CAUTION]
> **Önemli** Derinlikli okuyucu uygulamasına gönderilen dize değerlerini programlı olarak değiştirmeyi denemeyin, `-preferences` çünkü bu durum, müşterileriniz için daha fazla kullanıcı deneyimi oluşmasına neden olabilir.

```Parameters
Type: String
Required: false
Default value: null
```

##### `-onPreferencesChanged`
```Parameters
Type: Function
Required: false
Default value: null
```

##### `-customDomain`
```Parameters
Type: String
Required: false
Default value: null
```

<br>

## <a name="readaloudoptions"></a>Readaloudoçen'lar

```typescript
type ReadAloudOptions = {
    voice?: string;
    speed?: number;
    autoplay?: boolean;
};
```

#### <a name="readaloudoptions-parameters"></a>Readaloudoçenparametreleri

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| ses | Dize | Ses, "kadın" veya "erkek" olarak da. Tüm dillerin her iki genders de desteklemediğini unutmayın. |
| hız | Sayı | Kayıttan yürütme hızı, 0,5 ve 2,5 (dahil) arasında olmalıdır. |
| Yürüt | Boole | Tam ekran okuyucu yüklenirken otomatik olarak okumaya başlayın. |

##### `-voice`
```Parameters
Type: String
Required: false
Default value: "Female" or "Male" (determined by language) 
Values available: "Female", "Male"
```

##### `-speed`
```Parameters
Type: Number
Required: false
Default value: 1
Values available: 0.5, 0.75, 1, 1.25, 1.5, 1.75, 2, 2.25, 2.5
```

> [!NOTE]
> Tarayıcı sınırlamaları nedeniyle, Otomatik Kullan Safari 'de desteklenmez.

<br>

## <a name="translationoptions"></a>TranslationOptions

```typescript
type TranslationOptions = {
    language: string;
    autoEnableDocumentTranslation?: boolean;
    autoEnableWordTranslation?: boolean;
};
```

#### <a name="translationoptions-parameters"></a>TranslationOptions parametreleri

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| language | Dize | Çeviri dilini ayarlar, bu değer IETF BCP 47 dil etiketi biçimindedir, ör. fr-FR, es-MX, zh-Hans-CN. Sözcük veya belge çevirisini otomatik olarak etkinleştirmek için gereklidir. |
| Oto Enabledocumenttranslation | Boole | Tüm belgeyi otomatik olarak çevir. |
| Oto Enablewordtranslation | Boole | Sözcük çevirisini otomatik olarak etkinleştir. |

##### `-language`
```Parameters
Type: String
Required: true
Default value: null 
Values available: See the Supported Languages section
```

<br>

## <a name="displayoptions"></a>DisplayOptions

```typescript
type DisplayOptions = {
    textSize?: number;
    increaseSpacing?: boolean;
    fontFamily?: string;
};
```

#### <a name="displayoptions-parameters"></a>DisplayOptions parametreleri

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| TEXTSIZE | Sayı | Seçilen metin boyutunu ayarlar. |
| ıncreasespacing | Boole | Metin aralıklarının açılıp kapanmadığını ayarlar. |
| fontFamily | Dize | Seçilen yazı tipini ayarlar ("Calibri", "ComicSans" veya "Sıtka"). |

##### `-textSize`
```Parameters
Type: Number
Required: false
Default value: 20, 36 or 42 (Determined by screen size)
Values available: 14, 20, 28, 36, 42, 48, 56, 64, 72, 84, 96
```

##### `-fontFamily`
```Parameters
Type: String
Required: false
Default value: "Calibri"
Values available: "Calibri", "Sitka", "ComicSans"
```

<br>

## <a name="cookiepolicy-options"></a>Tanımlama, ıepolicy seçenekleri

```typescript
enum CookiePolicy { Disable, Enable }
```

**Aşağıda listelenen ayarlar yalnızca bilgilendirme amaçlıdır**. Tam ekran okuyucu ayarları veya Kullanıcı tercihlerini tanımlama bilgilerinde depolar. Bu *tanımlama* BILGILERI, AB tanımlama bilgisi uyumluluk yasaları ile uyumlu olması için varsayılan olarak tanımlama bilgilerinin kullanılmasını **devre dışı bırakır** . Tanımlama bilgilerini yeniden etkinleştirmek ve tam ekran okuyucusu Kullanıcı tercihleri için varsayılan işlevselliği geri yüklemek istediğinizde, Web sitenizin veya uygulamanızın tanımlama bilgilerini etkinleştirmek üzere kullanıcıdan uygun izni aldığından emin olmanız gerekir. Daha sonra, derinlikli okuyucudaki tanımlama bilgilerini yeniden etkinleştirmek için, kapsamlı bir okuyucu başlatırken *ıepolicy* *. Enable* seçeneğini açıkça ayarlamanız gerekir. Aşağıdaki tabloda, tanımlama bilgisi *ıepolicy* seçeneği etkinken tam ekran derinlikli okuyucu hangi ayarları depoladığını anlatmaktadır.

#### <a name="settings-parameters"></a>Ayarlar parametreleri

| Ayar | Tür | Açıklama |
| ------- | ---- | ----------- |
| TEXTSIZE | Sayı | Seçilen metin boyutunu ayarlar. |
| fontFamily | Dize | Seçilen yazı tipini ayarlar ("Calibri", "ComicSans" veya "Sıtka"). |
| Metin aralığı | Sayı | Metin aralıklarının açılıp kapanmadığını ayarlar. |
| Formatıngenabled | Boole | HTML biçimlendirmesinin açılıp kapanmadığını ayarlar. |
| Tema | Dize | Seçilen temayı ayarlar (ör. "hafif", "koyu"...). |
| Heceificationenabled | Boole | Hecelenmiş veya kapalı olup olmadığını ayarlar. |
| nounHighlightingEnabled | Boole | Bu, ad vurgulamanın açılıp kapanmadığını belirler. |
| nounHighlightingColor | Dize | Seçilen ad vurgulama rengini ayarlar. |
| verbHighlightingEnabled | Boole | Fiil vurgulamanın açılıp kapanmadığını ayarlar. |
| verbHighlightingColor | Dize | Seçilen fiil vurgulama rengini ayarlar. |
| adjectiveHighlightingEnabled | Boole | Sıfatıcı vurgulamanın açılıp kapanmadığını ayarlar. |
| Sıftivehighlightingcolor | Dize | Seçilen sıfatıcı vurgulama rengini ayarlar. |
| adverbHighlightingEnabled | Boole | M b vurgulamanın açılıp kapanmadığını ayarlar. |
| Duyurusu Bhighlightingcolor | Dize | Seçilen duyurusu b vurgulama rengini ayarlar. |
| pictureDictionaryEnabled | Boole | Resim sözlüğünün açılıp kapanmadığını ayarlar. |
| posLabelsEnabled | Boole | Her bir konuşma bölümünün üst simge metin etiketinin açılıp kapanmadığını ayarlar.  |

<br>

## <a name="supported-languages"></a>Desteklenen Diller

Modern okuyucunun çeviri özelliği birçok dili destekler. Daha fazla bilgi için [Bu makaleye](https://www.onenote.com/learningtools/languagesupport) bakın.

<br>

## <a name="html-support"></a>HTML desteği

Biçimlendirme etkinleştirildiğinde, aşağıdaki içerik tam ekran okuyucu 'da HTML olarak işlenir.

| HTML | Desteklenen Içerik |
| --------- | ----------- |
| Yazı tipi stilleri | Kalın, Italik, altı çizili, kod, üstü çizili, üst simge, alt simge |
| Sırasız listeler | Disk, daire, kare |
| Sıralı listeler | Ondalık, büyük Alfa, alt Alfa, büyük Latin, alt roman |

Desteklenmeyen Etiketler comparably işlenecek. Görüntüler ve tablolar şu anda desteklenmiyor.

<br>

## <a name="browser-support"></a>Tarayıcı desteği

Tam ekran okuyucu ile en iyi deneyim için aşağıdaki tarayıcıların en son sürümlerini kullanın.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

<br>

## <a name="next-steps"></a>Sonraki adımlar

* [GitHub 'Da modern Okuyucu SDK 'sını](https://github.com/microsoft/immersive-reader-sdk) keşfet
* [Hızlı başlangıç: tam ekran okuyucuyu Başlatan bir Web uygulaması oluşturma (C#)](./quickstarts/client-libraries.md?pivots=programming-language-csharp)
