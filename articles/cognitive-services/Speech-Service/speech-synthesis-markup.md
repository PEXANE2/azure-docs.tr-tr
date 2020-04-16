---
title: Konuşma Sentezi Biçimlendirme Dili (SSML) - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Metin-konuşma dilinde telaffuz ve prosody kontrol etmek için Konuşma Sentezi Biçimlendirme Dili kullanma.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: trbye
ms.openlocfilehash: dc11d26c73c52b5e6c4d8e05cc27dd6ebce0c5d8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399831"
---
# <a name="improve-synthesis-with-speech-synthesis-markup-language-ssml"></a>Konuşma Sentezi Biçimlendirme Dili (SSML) ile sentezi geliştirin

Konuşma Sentezi Biçimlendirme Dili (SSML), geliştiricilerin metinden konuşmaya hizmeti kullanarak giriş metninin nasıl sentezlenmiş konuşmaya dönüştürüldüğünü belirtmelerini sağlayan XML tabanlı bir biçimlendirme dilidir. Düz metinle karşılaştırıldığında, SSML geliştiricilerin adım, telaffuz, konuşma hızı, ses düzeyi ve metinden konuşmaya çıktının daha fazlasına ince ayar yapmasına olanak tanır. Bir süre sonra duraklatma veya bir cümle soru işaretiyle sona erdiğinde doğru tonlamayı kullanmak gibi normal noktalama işareti otomatik olarak işlenir.

SSML Konuşma hizmeti uygulaması World Wide Web [Konsorsiyumu'nun Konuşma Sentezi Biçimlendirme Dil Sürüm 1.0](https://www.w3.org/TR/speech-synthesis)dayanmaktadır.

> [!IMPORTANT]
> Çince, Japonca ve Korece karakterler faturalandırma için iki karakter olarak sayılır. Daha fazla bilgi için [Fiyatlandırma'ya](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)bakın.

## <a name="standard-neural-and-custom-voices"></a>Standart, nöral ve özel sesler

Standart ve sinirsel sesler arasından seçim yapın veya ürününize veya markanıza özgü kendi özel sesinizi oluşturun. 75+ standart ses 45'ten fazla dilde ve yerel olarak, 5 sinirsel ses ise dört dilde ve yerel olarak mevcuttur. Desteklenen dillerin, yerel dillerin ve seslerin (sinirsel ve standart) tam listesi için [bkz.](language-support.md)

Standart, sinirsel ve özel sesler hakkında daha fazla bilgi edinmek [için Metinden konuşmaya genel bakış'a](text-to-speech.md)bakın.

## <a name="special-characters"></a>Özel karakterler

SSML kullanırken, tırnak işaretleri, kesit işaretleri ve köşeli ayraçlar gibi özel karakterlerin kaçması gerektiğini unutmayın. Daha fazla bilgi için bkz: [Genişletilebilir Biçimlendirme Dili (XML) 1.0: Ek D](https://www.w3.org/TR/xml/#sec-entexpand).

## <a name="supported-ssml-elements"></a>Desteklenen SSML elemanları

Her SSML belgesi SSML öğeleri (veya etiketleri) ile oluşturulur. Bu öğeler perdeyi, prosody'yi, hacmi ve daha fazlasını ayarlamak için kullanılır. Aşağıdaki bölümlerde her öğenin nasıl kullanıldığı ve bir öğenin ne zaman gerekli olduğu veya isteğe bağlı olduğu ayrıntılı olarak açıklanmıştır.  

> [!IMPORTANT]
> Öznitelik değerleri etrafında çift tırnak kullanmayı unutmayın. İyi biçimlendirilmiş, geçerli XML standartları, öznitelik değerlerinin çift tırnak işaretleriyle çevreleşdirilmesini gerektirir. Örneğin, `<prosody volume="90">` iyi biçimlendirilmiş, geçerli bir `<prosody volume=90>` öğedir, ancak değildir. SSML, tırnak içinde olmayan öznitelik değerlerini tanımayabilir.

## <a name="create-an-ssml-document"></a>SSML belgesi oluşturma

`speak`kök öğedir ve tüm SSML belgeleri için **gereklidir.** Öğe `speak` sürüm, dil ve biçimlendirme kelime tanımı gibi önemli bilgiler içerir.

**Sözdizimi**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `version` | Belge biçimlendirmesini yorumlamak için kullanılan SSML belirtiminin sürümünü gösterir. Geçerli sürüm 1.0'dır. | Gerekli |
| `xml:lang` | Kök belgenin dilini belirtir. Değer küçük harfli, iki harfli bir dil `en`kodu (örneğin, ) veya dil kodu ve `en-US`büyük harf ülke/bölge (örneğin,) içerebilir. | Gerekli |
| `xmlns` | URI'yi, SSML belgesinin biçimlendirme sözcük dağarcığını (öğe türleri ve öznitelik adları) tanımlayan belgeye belirtir. Geçerli URI http://www.w3.org/2001/10/synthesis. | Gerekli |

## <a name="choose-a-voice-for-text-to-speech"></a>Metinden konuşmaya bir ses seçin

Öğe `voice` gereklidir. Metinden konuşmaya için kullanılan sesi belirtmek için kullanılır.

**Sözdizimi**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `name` | Metinden konuşmaya çıktıiçin kullanılan sesi tanımlar. Desteklenen seslerin tam listesi için [Dil desteğine](language-support.md#text-to-speech)bakın. | Gerekli |

**Örnek**

> [!NOTE]
> Bu örnekte `en-US-AriaRUS` ses kullanır. Desteklenen seslerin tam listesi için [Dil desteğine](language-support.md#text-to-speech)bakın.

```XML
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Birden çok ses kullanma

Öğe `speak` içinde, metinden konuşmaya çıktı için birden çok ses belirtebilirsiniz. Bu sesler farklı dillerde olabilir. Her ses için metnin bir `voice` öğeye sarılması gerekir. 

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `name` | Metinden konuşmaya çıktıiçin kullanılan sesi tanımlar. Desteklenen seslerin tam listesi için [Dil desteğine](language-support.md#text-to-speech)bakın. | Gerekli |

> [!IMPORTANT]
> Birden çok ses sözcük sınır özelliğiile uyumsuzdur. Birden çok ses kullanabilmek için sözcük sınır özelliğinin devre dışı edilmesi gerekir.

### <a name="disable-word-boundary"></a>Sözcük sınırını devre dışı

Konuşma SDK diline bağlı olarak, `"SpeechServiceResponse_Synthesis_WordBoundaryEnabled"` özelliği `false` `SpeechConfig` nesnenin bir örneğine ayarlarsınız.

# <a name="c"></a>[C #](#tab/csharp)

Daha fazla bilgi <a href="https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.setproperty?view=azure-dotnet" target="_blank"> `SetProperty` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```csharp
speechConfig.SetProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="c"></a>[C++](#tab/cpp)

Daha fazla bilgi <a href="https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setproperty" target="_blank"> `SetProperty` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```cpp
speechConfig->SetProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="java"></a>[Java](#tab/java)

Daha fazla bilgi <a href="https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.setproperty?view=azure-java-stable#com_microsoft_cognitiveservices_speech_SpeechConfig_setProperty_String_String_" target="_blank"> `setProperty` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```java
speechConfig.setProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="python"></a>[Python](#tab/python)

Daha fazla bilgi <a href="https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#set-property-by-name-property-name--str--value--str-" target="_blank"> `set_property_by_name` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```python
speech_config.set_property_by_name(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Daha fazla bilgi <a href="https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#setproperty-string--string-" target="_blank"> `setProperty` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```javascript
speechConfig.setProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="objective-c"></a>[Objective-C](#tab/objectivec)

Daha fazla bilgi <a href="https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#setpropertytobyname" target="_blank"> `setPropertyTo` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```objectivec
[speechConfig setPropertyTo:@"false" byName:@"SpeechServiceResponse_Synthesis_WordBoundaryEnabled"];
```

# <a name="swift"></a>[Swift](#tab/swift)

Daha fazla bilgi <a href="https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#setpropertytobyname" target="_blank"> `setPropertyTo` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```swift
speechConfig!.setPropertyTo(
    "false", byName: "SpeechServiceResponse_Synthesis_WordBoundaryEnabled")
```

---

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        Good morning!
    </voice>
    <voice name="en-US-Guy24kRUS">
        Good morning to you too Aria!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Konuşma stillerini ayarlama

> [!IMPORTANT]
> Konuşma stillerinin ayarını sadece nöral sesler ile çalışacaktır.

Varsayılan olarak, metinden konuşmaya hizmet, hem standart hem de nöral sesler için nötr bir konuşma stili kullanarak metni sentezler. Nöral sesler ile, konuşma stilini neşeyle, empatiyi veya `<mstts:express-as>` duyguyu elementle ifade etmek için ayarlayabilirsiniz. Bu, Konuşma hizmetine özgü isteğe bağlı bir öğedir.

Şu anda, konuşma stili ayarlamaları bu sinirsel sesler için desteklenir:
* `en-US-AriaNeural`
* `zh-CN-XiaoxiaoNeural`

Değişiklikler cümle düzeyinde uygulanır ve stil sese göre değişir. Bir stil desteklenmezse, hizmet konuşmayı varsayılan nötr konuşma stilinde döndürer.

**Sözdizimi**

```xml
<mstts:express-as style="string"></mstts:express-as>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `style` | Konuşma stilini belirtir. Şu anda, konuşma stilleri sese özgü. | Nöral bir ses için konuşma stilini ayarlıyorsanız gereklidir. Kullanıyorsanız, `mstts:express-as`o zaman stil sağlanmalıdır. Geçersiz bir değer sağlanırsa, bu öğe yoksayılır. |

Her nöral ses için hangi konuşma stillerinin destekleniyi belirlemek için bu tabloyu kullanın.

| Ses                   | Stil                     | Açıklama                                                 |
|-------------------------|---------------------------|-------------------------------------------------------------|
| `en-US-AriaNeural`      | `style="newscast"`        | Haber anlatımı için resmi ve profesyonel bir ton ifade eder |
|                         | `style="customerservice"` | Müşteri desteği için samimi ve yararlı bir ton ifade eder  |
|                         | `style="chat"`            | Rahat ve rahat bir ton ifade eder                         |
|                         | `style="cheerful"`        | Olumlu ve mutlu bir ton ifade eder                         |
|                         | `style="empathetic"`      | Bir bakım ve anlayış duygusunu ifade eder               |
| `zh-CN-XiaoxiaoNeural`  | `style="newscast"`        | Haber anlatımı için resmi ve profesyonel bir ton ifade eder |
|                         | `style="customerservice"` | Müşteri desteği için samimi ve yararlı bir ton ifade eder  |
|                         | `style="assistant"`       | Dijital asistanlar için sıcak ve rahat bir ton ifade eder    |
|                         | `style="lyrical"`         | Duyguları melodik ve duygusal bir şekilde ifade eder         |

**Örnek**

Bu SSML snippet, `<mstts:express-as>` öğenin konuşma stilini `cheerful`nasıl değiştireceğini gösterir.

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        <mstts:express-as style="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Kesme/duraklatma ekleme veya kaldırma

Sözcükler `break` arasında duraklamalar (veya molalar) eklemek veya metinden konuşmaya hizmet tarafından otomatik olarak eklenen duraklamaları önlemek için öğeyi kullanın.

> [!NOTE]
> Söz veya tümcecik için sentezlenen konuşma doğal değilse, bir sözcük veya tümcecik için metinden konuşmaya (TTS) varsayılan davranışını geçersiz kılmak için bu öğeyi kullanın. Metinden konuşmaya hizmeti tarafından otomatik olarak eklenen prozodik bir sonu önlemek için ayarlayın. `strength` `none`

**Sözdizimi**

```xml
<break strength="string" />
<break time="string" />
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `strength` | Aşağıdaki değerlerden birini kullanarak duraklatmanın göreli süresini belirtir:<ul><li>yok</li><li>x-zayıf</li><li>Zayıf</li><li>orta (varsayılan)</li><li>Güçlü</li><li>x-güçlü</li></ul> | İsteğe bağlı |
| `time` | Saniye veya milisaniye cinsinden bir duraklamanın mutlak süresini belirtir. Geçerli değerlere `2s` örnek olarak`500` | İsteğe bağlı |

| Gücü                      | Açıklama |
|-------------------------------|-------------|
| Yok veya değer sağlanmadıysa | 0 ms        |
| x-zayıf                        | 250 ms      |
| Zayıf                          | 500 ms      |
| orta                        | 750 ms      |
| Güçlü                        | 1000 ms     |
| x-güçlü                      | 1250 ms     |

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Paragrafları ve cümleleri belirtin

`p`ve `s` öğeler sırasıyla paragrafları ve cümleleri belirtmek için kullanılır. Bu öğelerin yokluğunda, metinden konuşmaya hizmet otomatik olarak SSML belgenin yapısını belirler.

Öğe `p` metin ve aşağıdaki öğeleri `audio`içerebilir: , `say-as` `sub`, `mstts:express-as` `break` `phoneme` `prosody`, `s`, , , ve .

Öğe `s` metin ve aşağıdaki öğeleri `audio`içerebilir: `prosody`, `say-as` `mstts:express-as`, `break` `phoneme` `sub`, , , ve .

**Sözdizimi**

```XML
<p></p>
<s></s>
```

**Örnek**

```XML
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <p>
            <s>Introducing the sentence element.</s>
            <s>Used to mark individual sentences.</s>
        </p>
        <p>
            Another simple paragraph.
            Sentence structure in this paragraph is not explicitly marked.
        </p>
    </voice>
</speak>
```

## <a name="use-phonemes-to-improve-pronunciation"></a>Telaffuzunu artırmak için phonemes kullanın

Öğe, `ph` SSML belgelerinde fonetik telaffuz için kullanılır. Öğe `ph` yalnızca metin içerebilir, başka öğe içermez. Her zaman bir geri dönüş olarak insan tarafından okunabilir konuşma sağlar.

Fonetik alfabeler, bazen birlikte harfler, sayılar veya karakterlerden oluşan telefonlardan oluşur. Her telefon benzersiz bir konuşma sesi tanımlar. Bu, herhangi bir harfin birden çok konuşulan sesi temsil edebileceği Latin alfabesinin aksinedir. "c" harfinin "şeker" ve "dur" sözcüklerindeki farklı telaffuzlarını veya "th" harfinin "şey" ve "bunlar" sözcüklerinde ki farklı telaffuzlarını göz önünde bulundurun.

**Sözdizimi**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `alphabet` | Öznitelikteki dizenin telaffuzunu sentezlerken kullanılacak fonetik alfabeyi `ph` belirtir. Alfabeyi belirten dize küçük harflerle belirtilmelidir. Aşağıda belirtebilirsiniz olası alfabeler vardır.<ul><li>`ipa`&ndash; <a href="https://en.wikipedia.org/wiki/International_Phonetic_Alphabet" target="_blank">Uluslararası Fonetik <span class="docon docon-navigate-external x-hidden-focus"></span> Alfabe</a></li><li>`sapi`&ndash; [Konuşma hizmeti fonetik alfabe](speech-ssml-phonetic-sets.md)</li><li>`ups`&ndash; Evrensel Telefon Seti</li></ul><br>Alfabe yalnızca `phoneme` elemaniçin geçerlidir... | İsteğe bağlı |
| `ph` | `phoneme` Öğedeki sözcüğün telaffuzunu belirten telefonlar içeren bir dize. Belirtilen dize tanınmayan telefonlar içeriyorsa, metinden konuşmaya (TTS) hizmeti SSML belgesinin tamamını reddeder ve belgede belirtilen konuşma çıktılarının hiçbirini üretmez. | Phonemes kullanıyorsanız gereklidir. |

**Örnekler**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <phoneme alphabet="sapi" ph="iy eh n y uw eh s"> en-US </phoneme>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

## <a name="use-custom-lexicon-to-improve-pronunciation"></a>Telaffuzunu geliştirmek için özel sözlüğü kullanma

Bazen TTS bir sözcüğü, örneğin bir şirketi veya yabancı adı doğru bir şekilde telaffuz edemez. Geliştiriciler, ssml'deki bu varlıkların okumasını kullanarak `phoneme` ve `sub` etiketleyerek tanımlayabilir veya etiketi kullanarak `lexicon` özel bir sözlük dosyasına atıfta bulunarak birden çok varlığın okunmasını tanımlayabilir.

**Sözdizimi**

```XML
<lexicon uri="string"/>
```

**Öznitelikler**

| Öznitelik | Açıklama                               | Gerekli / İsteğe Bağlı |
|-----------|-------------------------------------------|---------------------|
| `uri`     | Harici PLS belgesinin adresi. | Gereklidir.           |

**Kullanım**

Adım 1: Özel sözlüğü tanımla 

Varlıkların okumasını ,xml veya .pls dosyası olarak depolanan özel sözlük öğeleri listesiyle tanımlayabilirsiniz.

**Örnek**

```xml
<?xml version="1.0" encoding="UTF-16"?>
<lexicon version="1.0" 
      xmlns="http://www.w3.org/2005/01/pronunciation-lexicon"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
      xsi:schemaLocation="http://www.w3.org/2005/01/pronunciation-lexicon 
        http://www.w3.org/TR/2007/CR-pronunciation-lexicon-20071212/pls.xsd"
      alphabet="ipa" xml:lang="en-US">
  <lexeme>
    <grapheme>BTW</grapheme> 
    <alias>By the way</alias> 
  </lexeme>
  <lexeme>
    <grapheme> Benigni </grapheme> 
    <phoneme> bɛˈniːnji</phoneme>
  </lexeme>
</lexicon>
```

Her `lexeme` öğe bir sözlük öğesidir. `grapheme``lexeme`ortografını açıklayan bir metin içerir. Okuma formu olarak `alias`sağlanabilir. Telefon dizesi `phoneme` öğe olarak sağlanabilir.

Öğe `lexicon` en az `lexeme` bir öğe içerir. Her `lexeme` öğe en `grapheme` az bir öğe `grapheme` `alais`ve `phoneme` bir veya daha fazla , ve öğeleri içerir. `grapheme` Öğe, <a href="https://www.w3.org/TR/pronunciation-lexicon/#term-Orthography" target="_blank">ortografiyi <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>açıklayan metin içerir. Öğeler, `alias` kısaltmanın veya kısaltılmış bir terimin telaffuzunu belirtmek için kullanılır. Öğe, `phoneme` nasıl telaffuz `lexeme` edildiğini açıklayan metin sağlar.

Özel sözlük dosyası hakkında daha fazla bilgi için W3C web sitesinde [telaffuz sözlüğü belirtimi (PLS) Sürüm 1.0'a](https://www.w3.org/TR/pronunciation-lexicon/) bakın.

Adım 2: Adım 1'de oluşturulan özel sözlük dosyasını çevrimiçi olarak yükleyin, istediğiniz yerde saklayabilirsiniz ve Azure [Blob Depolama](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)gibi Microsoft Azure'da saklamanızı öneririz.

Adım 3: SSML'deki özel sözlük dosyasına bakın

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" 
          xmlns:mstts="http://www.w3.org/2001/mstts" 
          xml:lang="en-US">
<lexicon uri="http://www.example.com/customlexicon.xml"/>
BTW, we will be there probably 8:00 tomorrow morning.
Could you help leave a message to Robert Benigni for me?
</speak>
```
"BTW" olarak okunacak "Bu arada". "Benigni" sağlanan IPA "bˈniˈnji" ile okunacaktır.  

**Sınırlama**
- Dosya boyutu: özel sözlük dosya boyutu maksimum limiti 100KB' dir, bu boyutun ötesindeyse, sentez isteği başarısız olur.
- Sözlük önbelleği yenileme: Özel sözlük, ilk yüklendiğinde TTS Hizmeti'nde anahtar olarak URI ile önbelleğe alınacaktır. Aynı URI ile Sözlük 15 dakika içinde yeniden olmayacak, bu nedenle özel sözlük değişikliği etkili olmak için en fazla 15 dakika beklemek gerekiyor.

**Konuşma hizmeti fonetik setleri**

Yukarıdaki örnekte, IPA telefon seti olarak da bilinen Uluslararası Fonetik Alfabesi'ni kullanıyoruz. Uluslararası standart olduğu için geliştiricilerin IPA kullanmalarını öneriyoruz. IPA'nın hatırlanması kolay olmadığı düşünülürse, Konuşma hizmeti yedi dil`en-US` `fr-FR`için `de-DE` `es-ES`fonetik bir küme tanımlar ( , , , `ja-JP`, `zh-CN`, , ve `zh-TW`).

Aşağıda gösterildiği `sapi` gibi özel sözlükler ile `alphabet` öznitelik için vale olarak kullanabilirsiniz:

```xml
<?xml version="1.0" encoding="UTF-16"?>
<lexicon version="1.0" 
      xmlns="http://www.w3.org/2005/01/pronunciation-lexicon"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.w3.org/2005/01/pronunciation-lexicon
        http://www.w3.org/TR/2007/CR-pronunciation-lexicon-20071212/pls.xsd"
      alphabet="sapi" xml:lang="en-US">
  <lexeme>
    <grapheme>BTW</grapheme>
    <alias> By the way </alias>
  </lexeme>
  <lexeme>
    <grapheme> Benigni </grapheme>
    <phoneme> b eh 1 - n iy - n y iy </phoneme>
  </lexeme>
</lexicon>
```

Ayrıntılı Konuşma hizmeti fonetik alfabesi hakkında daha fazla bilgi için Konuşma [hizmeti fonetik kümeleri'ne](speech-ssml-phonetic-sets.md)bakın.

## <a name="adjust-prosody"></a>Prosody'yi ayarlayın

Öğe, `prosody` metinden konuşmaya çıktıiçin adım, kontur, aralık, hız, süre ve hacim deki değişiklikleri belirtmek için kullanılır. Öğe `prosody` metin ve aşağıdaki öğeleri `audio`içerebilir: , `prosody` `say-as`, `sub` `break` `p` `phoneme`, `s`, , , ve .

Prozodik öznitelik değerleri geniş bir aralıkta değişebildiği için, konuşma tanıyıcısı atanan değerleri seçili sesin gerçek prozodik değerlerinin ne olması gerektiğine dair bir öneri olarak yorumlar. Metinden konuşmaya hizmet, desteklenmeyen değerleri sınırlar veya yerine geçer. Desteklenmeyen değerlere örnek olarak 1 MHz'lik bir aralık veya 120 hacim verilebilir.

**Sözdizimi**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `pitch` | Metnin temel adımını gösterir. Perdeyi şu şekilde ifade edebilirsiniz:<ul><li>Mutlak bir değer, bir sayı olarak ifade "Hz" (Hertz) takip. Örneğin, 600 Hz.</li><li>"+" veya "-" ve ardından "Hz" veya "st" tarafından önce bir sayı olarak ifade edilen ve perdeyi değiştirmek için bir miktar belirten göreli bir değer. Örneğin: +80 Hz veya -2. "St", değişim ünitesinin standart diyatonik ölçekte bir tonun (yarım adım) yarısı olan yarı ton olduğunu gösterir.</li><li>Sabit bir değer:<ul><li>x-düşük</li><li>Düşük</li><li>orta</li><li>yüksek</li><li>x-yüksek</li><li>default</li></ul></li></ul>. | İsteğe bağlı |
| `contour` | Kontur sinirsel sesler için desteklenmez. Kontur, perdedeki değişiklikleri temsil eder. Bu değişiklikler, konuşma çıkışında belirtilen zaman konumlarında bir dizi hedef olarak temsil edilir. Her hedef parametre çiftleri kümeleri ile tanımlanır. Örneğin: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>Her parametre kümesindeki ilk değer, metin süresinin yüzdesi olarak adım değişikliğinin konumunu belirtir. İkinci değer, göreli bir değer veya pitch için numaralandırma değeri kullanarak perdeyi yükseltecek `pitch`veya düşürecek tutarı belirtir (bkz. | İsteğe bağlı |
| `range` | Metnin pitch aralığını temsil eden bir değer. Tanımlamak için `range` kullanılan aynı mutlak değerleri, göreli değerleri veya numaralandırma değerlerini kullanarak ifade `pitch`edebilirsiniz. | İsteğe bağlı |
| `rate` | Metnin konuşma hızını gösterir. Şunları ifade `rate` edebilirsiniz:<ul><li>Varsayılan çarpan olarak hareket eden bir sayı olarak ifade edilen göreli bir değer. Örneğin, *1* değeri, oranhiçbir değişiklik sonuçlanır. *0,5* değeri, oranın yarıya inilmesiyle sonuçlanır. *3* değeri, oranın üçe kaynırla sonuçlanmaktadır.</li><li>Sabit bir değer:<ul><li>x-yavaş</li><li>Yavaş</li><li>orta</li><li>Hızlı</li><li>x-hızlı</li><li>default</li></ul></li></ul> | İsteğe bağlı |
| `duration` | Konuşma sentezi (TTS) hizmeti metni saniye veya milisaniye cinsinden okurken, süresinin dolması gereken süre. Örneğin, *2s* veya *1800ms*. | İsteğe bağlı |
| `volume` | Konuşan sesin ses düzeyini gösterir. Hacmi aşağıdaki gibi ifade edebilirsiniz:<ul><li>0,0 ile 100,0 aralığında, *en sessizden* *en yüksek sese*doğru bir sayı olarak ifade edilen mutlak bir değer. Örneğin, 75. Varsayılan değer 100.0'dır.</li><li>Hacmi değiştirmek için bir miktar belirten "+" veya "-" aralığından önce bir sayı olarak ifade edilen göreli değer. Örneğin, +10 veya -5,5.</li><li>Sabit bir değer:<ul><li>Sessiz</li><li>x-yumuşak</li><li>Yumuşak</li><li>orta</li><li>Yüksek sesle</li><li>x-yüksek sesle</li><li>default</li></ul></li></ul> | İsteğe bağlı |

### <a name="change-speaking-rate"></a>Konuşma oranını değiştir

Konuşma oranı, sözcük veya cümle düzeyindeki standart seslere uygulanabilir. Oysa konuşma hızı sadece cümle düzeyinde nöral seslere uygulanabilir.

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Guy24kRUS">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Ses düzeyini değiştirme

Birim değişiklikleri sözcük veya cümle düzeyindestandart seslere uygulanabilir. Halbuki ses değişiklikleri sadece cümle düzeyinden nöral seslere uygulanabilir.

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Perdeyi değiştir

Adım değişiklikleri sözcük veya cümle düzeyindestandart seslere uygulanabilir. Halbuki perde değişiklikleri sadece cümle düzeyinden nöral seslere uygulanabilir.

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Pitch konturunu değiştir

> [!IMPORTANT]
> Pitch kontur değişiklikleri nöral seslerle desteklenmez.

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```
## <a name="say-as-element"></a>say-as öğesi

`say-as`öğenin metninin içerik türünü (sayı veya tarih gibi) gösteren isteğe bağlı bir öğedir. Bu, metnin nasıl okunuşu hakkında konuşma sentezi altyapısına rehberlik sağlar.

**Sözdizimi**

```XML
<say-as interpret-as="string" format="digit string" detail="string"> <say-as>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `interpret-as` | Öğenin metninin içerik türünü gösterir. Türlerin listesi için aşağıdaki tabloya bakın. | Gerekli |
| `format` | Belirsiz biçimlere sahip içerik türleri için öğenin metninin kesin biçimlendirmesi hakkında ek bilgiler sağlar. SSML, bunları kullanan içerik türleri için biçimleri tanımlar (aşağıdaki tabloya bakın). | İsteğe bağlı |
| `detail` | Konuşulacak ayrıntı düzeyini gösterir. Örneğin, bu öznitelik konuşma sentezi altyapısı noktalama işaretleri telaffuz isteyebilir. `detail`' için tanımlanan standart değer yoktur. | İsteğe bağlı |

<!-- I don't understand the last sentence. Don't we know which one Cortana uses? -->

Aşağıda, öznitelikler `interpret-as` ve `format` öznitelikler için desteklenen içerik türleri verilmiştir. Yalnızca `format` tarih ve `interpret-as` saat olarak ayarlanmışsa özniteliği ekleyin.

| olarak yorumlamak | biçim | Yorum |
|--------------|--------|----------------|
| `address` | | Metin bir adres olarak konuşulur. Konuşma sentezi motoru bildirir:<br /><br />`I'm at <say-as interpret-as="address">150th CT NE, Redmond, WA</say-as>`<br /><br />"Kuzey Doğu Redmond Washington'daki 150. |
| `cardinal`, `number` | | Metin bir kardinal numarası olarak konuşulur. Konuşma sentezi motoru bildirir:<br /><br />`There are <say-as interpret-as="cardinal">3</say-as> alternatives`<br /><br />"Üç alternatif var" gibi. |
| `characters`, `spell-out` | | Metin tek tek harfler (hecelenmiş) olarak konuşulur. Konuşma sentezi motoru bildirir:<br /><br />`<say-as interpret-as="characters">test</say-as>`<br /><br />"T E S T" olarak. |
| `date` | dmy, mdy, ymd, ydm, ym, my, md, dm, d, m, y | Metin tarih olarak konuşulur. Öznitelik, `format` tarihin biçimini *(d=gün, m=ay ve y=yıl)* belirtir. Konuşma sentezi motoru bildirir:<br /><br />`Today is <say-as interpret-as="date" format="mdy">10-19-2016</say-as>`<br /><br />"Bugün on dokuzuncu Ekim iki bin on altı." |
| `digits`, `number_digit` | | Metin tek tek basamak lar dizisi olarak konuşulur. Konuşma sentezi motoru bildirir:<br /><br />`<say-as interpret-as="number_digit">123456789</say-as>`<br /><br />"1 2 3 4 5 6 7 8 9" olarak. |
| `fraction` | | Metin kesirli bir sayı olarak konuşulur. Konuşma sentezi motoru bildirir:<br /><br /> `<say-as interpret-as="fraction">3/8</say-as> of an inch`<br /><br />"Bir santimin sekizde üçü" olarak. |
| `ordinal` | | Metin bir ordinal sayı olarak konuşulur. Konuşma sentezi motoru bildirir:<br /><br />`Select the <say-as interpret-as="ordinal">3rd</say-as> option`<br /><br />"Üçüncü seçeneği seçin" gibi. |
| `telephone` | | Metin bir telefon numarası olarak konuşulur. Öznitelik, `format` bir ülke kodunu temsil eden basamaklar içerebilir. Örneğin, Amerika Birleşik Devletleri için "1", İtalya için "39". Konuşma sentezi motoru, bir telefon numarasının telaffuzuna rehberlik etmek için bu bilgileri kullanabilir. Telefon numarası ülke kodunu da içerebilir ve eğer öyleyse, ülke kodundan `format`önce gelir. Konuşma sentezi motoru bildirir:<br /><br />`The number is <say-as interpret-as="telephone" format="1">(888) 555-1212</say-as>`<br /><br />"Benim numaram alan kodu sekiz sekiz beş beş beş bir iki iki." |
| `time` | hms12, hms24 | Metin bir zaman olarak konuşulur. Öznitelik, `format` saatin 12 saatlik saat (hms12) veya 24 saatlik bir saat (hms24) kullanılarak belirtilip belirtilmediğini belirtir. Saatleri, dakikaları ve saniyeleri temsil eden sayıları ayırmak için bir üst üste kullanın. Geçerli zaman örnekleri aşağıdadır: 12:35, 1:14:32, 08:15 ve 02:50:45. Konuşma sentezi motoru bildirir:<br /><br />`The train departs at <say-as interpret-as="time" format="hms12">4:00am</say-as>`<br /><br />"Tren dört A M'de kalkıyor." |

**Kullanım**

Öğe `say-as` yalnızca metin içerebilir.

**Örnek**

Konuşma sentezi motoru aşağıdaki örnek olarak konuşuyor "İlk isteğiniz on dokuz Ekim yirmi on iki on iki otuz beş PM erken varış ile bir oda için oldu."
 
```XML
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <p>
        Your <say-as interpret-as="ordinal"> 1st </say-as> request was for <say-as interpret-as="cardinal"> 1 </say-as> room
        on <say-as interpret-as="date" format="mdy"> 10/19/2010 </say-as>, with early arrival at <say-as interpret-as="time" format="hms12"> 12:35pm </say-as>.
        </p>
    </voice>
</speak>
```

## <a name="add-recorded-audio"></a>Kaydedilen ses ekleme

`audio`bir SSML belgesine MP3 ses eklemenize olanak tanıyan isteğe bağlı bir öğedir. Ses öğesinin gövdesi, ses dosyası kullanılamıyorsa veya çalınamayacaksa konuşulan düz metin veya SSML biçimlendirmesi içerebilir. Ayrıca, `audio` öğe metin ve aşağıdaki öğeleri `audio`içerebilir: `p` `s`, `phoneme` `prosody`, `say-as` `break`, `sub`, , , ve .

SSML belgesinde yer alan tüm ses şu gereksinimleri karşılamalıdır:

* MP3, Internet'e erişilebilen bir HTTPS bitiş noktasında barındırılmalıdır. HTTPS gereklidir ve MP3 dosyasını barındıran etki alanı nın geçerli, güvenilir bir TLS/SSL sertifikası sunması gerekir.
* MP3 geçerli bir MP3 dosyası (MPEG v2) olmalıdır.
* Bit hızı 48 kbps olmalıdır.
* Örneklem oranı 16.000 Hz olmalıdır.
* Tek bir yanıttaki tüm metin ve ses dosyalarının toplam toplam süresi doksan (90) saniyeyi geçemez.
* MP3 müşteriye özgü veya diğer hassas bilgileri içermemelidir.

**Sözdizimi**

```xml
<audio src="string"/></audio>
```

**Öznitelikler**

| Öznitelik | Açıklama                                   | Gerekli / İsteğe Bağlı                                        |
|-----------|-----------------------------------------------|------------------------------------------------------------|
| `src`     | Ses dosyasının konumunu/URL'sini belirtir. | SSML belgenizdeki ses öğesini kullanıyorsanız gereklidir. |

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <p>
            <audio src="https://contoso.com/opinionprompt.wav"/>
            Thanks for offering your opinion. Please begin speaking after the beep.
            <audio src="https://contoso.com/beep.wav">
                Could not play the beep, please voice your opinion now.
            </audio>
        </p>
    </voice>
</speak>
```

## <a name="add-background-audio"></a>Arka plan sesi ekleme

Bu `mstts:backgroundaudio` öğe, SSML belgelerinize arka plan sesi eklemenize (veya ses dosyasını metinden konuşmaya karıştırmanıza) olanak tanır. Arka `mstts:backgroundaudio` planda bir ses dosyası döngü, metin-to-konuşma başında solmaya ve metin-to-konuşma sonunda solmaya.

Sağlanan arka plan sesi metinden konuşmaya veya soluklulundan daha kısaysa, döngü yever. Metinden konuşmaya daha uzunsa, solma sona erdiğinde durur.

SSML belgesi başına yalnızca bir arka plan ses dosyasına izin verilir. Ancak, SSML belgenize `audio` ek `voice` ses eklemek için etiketleri öğenin içine serpebilirsiniz.

**Sözdizimi**

```XML
<mstts:backgroundaudio src="string" volume="string" fadein="string" fadeout="string"/>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `src` | Arka plan ses dosyasının konumunu/URL'sini belirtir. | SSML belgenizde arka plan sesi kullanıyorsanız gereklidir. |
| `volume` | Arka plan ses dosyasının hacmini belirtir. **Kabul edilen** `0` değerler `100` : kapsayıcıya. Varsayılan değer: `1`. | İsteğe bağlı |
| `fadein` | Arka plan sesinin süresini milisaniye olarak "soluk" olarak belirtir. Varsayılan değer, `0`hiçbir solmaya eşdeğerdir. **Kabul edilen** `0` değerler `10000` : kapsayıcıya.  | İsteğe bağlı |
| `fadeout` | Arka plan sesinin milisaniye cinsinden solma süresini belirtir. Varsayılan `0`değer, solmaya eşdeğerdir. **Kabul edilen** `0` değerler `10000` : kapsayıcıya.  | İsteğe bağlı |

**Örnek**

```xml
<speak version="1.0" xml:lang="en-US" xmlns:mstts="http://www.w3.org/2001/mstts">
    <mstts:backgroundaudio src="https://contoso.com/sample.wav" volume="0.7" fadein="3000" fadeout="4000"/>
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)">
        The text provided in this document will be spoken over the background audio.
    </voice>
</speak>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Dil desteği: sesler, yerel yönetimler, diller](language-support.md)
