---
title: Konuşma birleştirme biçimlendirme dili (SSML)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma senis, metinden konuşmaya konuşmayla telaffuz ve Prosody 'yi denetlemek için biçimlendirme dilini kullanma.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: trbye
ms.openlocfilehash: 8ec4c87dc4f19c140c5ac02779c5551016dfb0b3
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714330"
---
# <a name="improve-synthesis-with-speech-synthesis-markup-language-ssml"></a>Konuşma birleştirme biçimlendirme dili (SSML) ile senssıs 'yi geliştirme

Konuşma sentezi biçimlendirme dili (SSML), geliştiricilerin metin okuma hizmeti kullanılarak, giriş metninin birleştirilmiş konuşmaya nasıl dönüştürüldüğünü belirtmesini sağlayan XML tabanlı bir biçimlendirme dilidir. SSML, düz metin ile karşılaştırıldığında, geliştiricilerin sıklık, telaffuz, konuşma oranı, hacim ve metin okuma çıktısından daha fazlasını yapma olanağı sağlar. Bir dönemden sonra duraklatma gibi normal noktalama işaretleri veya bir tümce bir soru işaretiyle sona erdiğinde doğru innnation 'ın kullanılması otomatik olarak işlenir.

SSML konuşma hizmeti uygulamasının, World Wide Web Konsorsiyumu [konuşma Senssıs biçimlendirme dili sürüm 1,0](https://www.w3.org/TR/speech-synthesis)' i temel alır.

> [!IMPORTANT]
> Çince, Japonca ve Korece karakterler, faturalandırma için iki karakter olarak sayılır. Daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Standart, sinir ve özel sesler

Standart ve sinir sesler arasından seçim yapın ya da ürün veya marka için özel sesinizi benzersiz bir şekilde oluşturun. 45 ' den fazla dilde ve yerel ayarlarda 75 + standart sesler mevcuttur ve dört dilde ve yerel ayarlarda 5 sinir seste erişilebilir. Desteklenen dillerin, yerel ayarların ve seslerin (sinir ve standart) tüm listesi için bkz. [dil desteği](language-support.md).

Standart, sinir ve özel sesler hakkında daha fazla bilgi edinmek için bkz. [metinden konuşmaya genel bakış](text-to-speech.md).

## <a name="special-characters"></a>Özel karakterler

SSML kullanırken, tırnak işaretleri, kesme işareti ve köşeli ayraç gibi özel karakterlerin kaçılması gerektiğini aklınızda bulundurun. Daha fazla bilgi için bkz. [Genişletilebilir Biçimlendirme Dili (XML) 1,0: Ek D](https://www.w3.org/TR/xml/#sec-entexpand).

## <a name="supported-ssml-elements"></a>Desteklenen SSML öğeleri

Her SSML belgesi SSML öğeleri (veya etiketleri) ile oluşturulur. Bu öğeler, sıklık, Prosody, hacim ve daha fazlasını ayarlamak için kullanılır. Aşağıdaki bölümler her bir öğenin nasıl kullanıldığını ve bir öğe gerekli veya isteğe bağlı olduğunu ayrıntılandırır.  

> [!IMPORTANT]
> Öznitelik değerleri etrafında çift tırnak kullanmayı unutmayın. Doğru biçimlendirilmiş, geçerli XML standartları, öznitelik değerlerinin çift tırnak işareti içine alınması gerekir. Örneğin, `<prosody volume="90">` iyi biçimlendirilmiş, geçerli bir öğedir, ancak `<prosody volume=90>` değildir. SSML tırnak içinde olmayan öznitelik değerlerini tanıyamayabilir.

## <a name="create-an-ssml-document"></a>SSML belgesi oluşturma

`speak`, kök öğesidir ve tüm SSML belgeleri için **gereklidir** . `speak`Öğesi sürüm, dil ve biçimlendirme sözlük tanımı gibi önemli bilgiler içerir.

**Söz dizimi**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `version` | Belge işaretlemesini yorumlamak için kullanılan SSML belirtiminin sürümünü gösterir. Geçerli sürüm 1,0 ' dir. | Gerekli |
| `xml:lang` | Kök belgenin dilini belirtir. Değer küçük harf, iki harfli dil kodu (örneğin, `en` ) veya dil kodu ve büyük harf ülke/bölge (örneğin, `en-US` ) içerebilir. | Gerekli |
| `xmlns` | SSML belgesinin biçimlendirme sözlüğünü (öğe türleri ve öznitelik adları) tanımlayan belgenin URI 'sini belirtir. Geçerli URI http://www.w3.org/2001/10/synthesis . | Gerekli |

## <a name="choose-a-voice-for-text-to-speech"></a>Metinden konuşmaya için bir ses seçin

`voice`Öğe gereklidir. Metin okuma için kullanılan sesi belirtmek için kullanılır.

**Söz dizimi**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `name` | Metinden konuşmaya çıkış için kullanılan sesi tanımlar. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech). | Gerekli |

**Örnek**

> [!NOTE]
> Bu örnek, `en-US-AriaRUS` sesini kullanır. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Birden çok ses kullan

Öğesi içinde `speak` , metinden konuşmaya çıkış için birden çok ses belirtebilirsiniz. Bu sesler farklı dillerde olabilir. Her ses için, metnin bir öğe içinde sarmalanması gerekir `voice` . 

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `name` | Metinden konuşmaya çıkış için kullanılan sesi tanımlar. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech). | Gerekli |

> [!IMPORTANT]
> Birden çok ses sözcük sınırı özelliğiyle uyumlu değil. Birden çok ses kullanabilmek için sözcük sınırı özelliğinin devre dışı bırakılması gerekir.

### <a name="disable-word-boundary"></a>Sözcük sınırını devre dışı bırak

Konuşma SDK diline bağlı olarak, `"SpeechServiceResponse_Synthesis_WordBoundaryEnabled"` özelliğini `false` nesnesinin bir örneği üzerinde olarak ayarlarsınız `SpeechConfig` .

# <a name="c"></a>[C#](#tab/csharp)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.setproperty?view=azure-dotnet" target="_blank"> `SetProperty` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```csharp
speechConfig.SetProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="c"></a>[C++](#tab/cpp)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setproperty" target="_blank"> `SetProperty` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```cpp
speechConfig->SetProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="java"></a>[Java](#tab/java)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.setproperty?view=azure-java-stable#com_microsoft_cognitiveservices_speech_SpeechConfig_setProperty_String_String_" target="_blank"> `setProperty` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```java
speechConfig.setProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="python"></a>[Python](#tab/python)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#set-property-by-name-property-name--str--value--str-" target="_blank"> `set_property_by_name` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```python
speech_config.set_property_by_name(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#setproperty-string--string-" target="_blank"> `setProperty` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```javascript
speechConfig.setProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="objective-c"></a>[Objective-C](#tab/objectivec)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#setpropertytobyname" target="_blank"> `setPropertyTo` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```objectivec
[speechConfig setPropertyTo:@"false" byName:@"SpeechServiceResponse_Synthesis_WordBoundaryEnabled"];
```

# <a name="swift"></a>[Swift](#tab/swift)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#setpropertytobyname" target="_blank"> `setPropertyTo` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

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

## <a name="adjust-speaking-styles"></a>Konuşma stillerini ayarla

> [!IMPORTANT]
> Konuşma stillerinin ayarlanması yalnızca sinir seslerle çalışır.

Varsayılan olarak, metinden konuşmaya hizmeti, standart ve sinir sesler için bağımsız bir konuşma stili kullanarak metni birleştirir. Sinir seslerde, konuşma stilini, peyerfullik, empabili ve Calm gibi farklı tarzları ifade etmek veya <mstts: Express-as> öğesini kullanarak özel hizmet, newscadıya ve Voice Yardımcısı gibi farklı senaryolar için en iyi duruma getirebilirsiniz. Bu, konuşma hizmetine özgü olan isteğe bağlı bir öğedir.

Şu anda, bu sinir sesleri için konuşma stil ayarlamaları desteklenir:
* `en-US-AriaNeural`
* `zh-CN-XiaoxiaoNeural`
* `zh-CN-YunyangNeural`

Değişiklikler tümce düzeyinde uygulanır ve stil sese göre farklılık gösterir. Bir stil desteklenmiyorsa, hizmet konuşmayı varsayılan nötr konuşma stilinde döndürür.

**Söz dizimi**

```xml
<mstts:express-as style="string"></mstts:express-as>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `style` | Konuşma stilini belirtir. Şu anda konuşma stilleri sese özgüdür. | Sinir Voice için konuşma stilini ayarlarken gereklidir. Kullanıyorsanız `mstts:express-as` stilin sağlanması gerekir. Geçersiz bir değer sağlanmışsa, bu öğe yok sayılır. |

Her sinir sesi için hangi konuşma stillerinin desteklendiğini öğrenmek için bu tabloyu kullanın.

| Ses                   | Stil                     | Açıklama                                                 |
|-------------------------|---------------------------|-------------------------------------------------------------|
| `en-US-AriaNeural`      | `style="newscast"`        | Anlatım haberleri için resmi ve profesyonel tonu ifade eder |
|                         | `style="customerservice"` | Müşteri desteği için kolay ve yararlı bir tonu ifade eder  |
|                         | `style="chat"`            | Rastgele ve gevşek bir tonu ifade eder                         |
|                         | `style="cheerful"`        | Pozitif ve kutlu bir tonu ifade eder                         |
|                         | `style="empathetic"`      | Bir fikir ve anlalama hakkında ifade eder               |
| `zh-CN-XiaoxiaoNeural`  | `style="newscast"`        | Anlatım haberleri için resmi ve profesyonel tonu ifade eder |
|                         | `style="customerservice"` | Müşteri desteği için kolay ve yararlı bir tonu ifade eder  |
|                         | `style="assistant"`       | Dijital yardımcılar için bir sıcak ve gevşek tonu ifade eder    |
|                         | `style="lyrical"`         | Bir Melodic ve sentisel şekilde ifade eder         |   
| `zh-CN-YunyangNeural`   | `style="customerservice"` | Müşteri desteği için kolay ve yararlı bir tonu ifade eder  | 

**Örnek**

Bu SSML kod parçacığı, `<mstts:express-as>` konuşma stilini olarak değiştirmek için öğesinin nasıl kullanıldığını gösterir `cheerful` .

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

`break`Sözcükler arasına duraklamalar (veya kesintiler) eklemek için öğesini kullanın veya otomatik olarak metin okuma hizmeti tarafından duraklamaları önleyin.

> [!NOTE]
> Bu sözcük veya tümcecik için birleştirilmiş konuşma doğal olmayan bir sözcük veya tümcecik için metin okuma (TTS) varsayılan davranışını geçersiz kılmak için bu öğeyi kullanın. `strength` `none` Metin okuma hizmeti tarafından otomatik olarak yerleştirilen bir bürünsel kesmesini engellemek için olarak ayarlayın.

**Söz dizimi**

```xml
<break strength="string" />
<break time="string" />
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `strength` | Aşağıdaki değerlerden birini kullanarak duraklamanın göreli süresini belirtir:<ul><li>yok</li><li>x-zayıf</li><li>zayıf</li><li>Orta (varsayılan)</li><li>lemenize</li><li>x-Strong</li></ul> | İsteğe Bağlı |
| `time` | Saniye veya milisaniye cinsinden bir duraklama 'nin mutlak süresini belirtir. Geçerli değerler örnekleri `2s` ve`500` | İsteğe Bağlı |

| Gücüyle                      | Açıklama |
|-------------------------------|-------------|
| Hiçbiri veya hiçbir değer sağlanmazsa | 0 MS        |
| x-zayıf                        | 250 MS      |
| zayıf                          | 500 ms      |
| orta                        | 750 ms      |
| lemenize                        | 1000 ms     |
| x-Strong                      | 1250 MS     |

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Paragrafları ve tümceleri belirtme

`p`ve `s` öğeleri, sırasıyla paragrafları ve tümceleri göstermek için kullanılır. Bu öğelerin yokluğunda, metinden konuşmaya hizmeti, SSML belgesinin yapısını otomatik olarak belirler.

`p`Öğesi metin ve şu öğeleri içerebilir: `audio` , `break` ,, `phoneme` `prosody` , `say-as` , `sub` , `mstts:express-as` , ve `s` .

`s`Öğesi metin ve şu öğeleri içerebilir: `audio` , `break` ,, `phoneme` `prosody` , `say-as` , `mstts:express-as` , ve `sub` .

**Söz dizimi**

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

## <a name="use-phonemes-to-improve-pronunciation"></a>Söyleniş 'yi geliştirmek için alfabesine kullanın

`ph`Öğesi SSML belgelerindeki fonetik telaffuz için kullanılır. `ph`Öğesi yalnızca metin içerebilir, başka öğe içeremez. Her zaman bir geri dönüş olarak insan tarafından okunabilen konuşmayı sağlayın.

Fonetik alfabeller, bazen birlikte harflerin, sayıların veya karakterlerin üzerinde oluşan telefonlardan oluşur. Her telefonda benzersiz bir konuşma sesi açıklanır. Bu Latin alfabesinden farklıdır, burada herhangi bir harf birden çok konuşulan sesi temsil edebilir. "Candy" ve "işten" kelimelerdeki "c" harfinin farklı söylenmelerini veya "şeyler" ve "Bunlar" kelimelerinde "TH" harf birleşiminin farklı söylenmelerini göz önünde bulundurun.

**Söz dizimi**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `alphabet` | Öznitelikteki dizenin okunuşunu birleştirirken kullanılacak fonetik alfabesini belirtir `ph` . Alfabeyi belirten dize küçük harfle belirtilmelidir. Aşağıdakiler, belirtebileceğiniz olası harfler sonuçlardır.<ul><li>`ipa`&ndash; <a href="https://en.wikipedia.org/wiki/International_Phonetic_Alphabet" target="_blank">Uluslararası fonetik alfabesi <span class="docon docon-navigate-external x-hidden-focus"></span> </a></li><li>`sapi`&ndash; [Konuşma hizmeti fonetik alfabesi](speech-ssml-phonetic-sets.md)</li><li>`ups`&ndash;Evrensel telefon kümesi</li></ul><br>Alfabe yalnızca öğesi içindeki öğesine uygulanır `phoneme` . | İsteğe Bağlı |
| `ph` | Öğe içinde sözcüğün söylenişini belirten telefonları içeren bir dize `phoneme` . Belirtilen dize tanınmayan telefonlar içeriyorsa, metinden konuşmaya (TTS) hizmeti SSML belgesinin tamamını reddeder ve belgede belirtilen konuşma çıktısından hiçbiri üretir. | Phonemes kullanılıyorsa gereklidir. |

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

## <a name="use-custom-lexicon-to-improve-pronunciation"></a>Söylenişi geliştirmek için özel sözlüğü kullanma

Bazen metinden konuşmaya hizmeti bir sözcüğe doğru pronounce. Örneğin, bir şirketin adı veya bir tıbbi dönem. Geliştiriciler, ve etiketlerini kullanarak SSML 'de tek varlıkların nasıl okunacağını tanımlayabilir `phoneme` `sub` . Ancak, birden çok varlığın nasıl okunduğunu tanımlamanız gerekiyorsa etiketini kullanarak özel bir sözlük oluşturabilirsiniz `lexicon` .

> [!NOTE]
> Özel sözlük Şu anda UTF-8 kodlamasını desteklemektedir. 

**Söz dizimi**

```XML
<lexicon uri="string"/>
```

**Öznitelikler**

| Öznitelik | Açıklama                               | Gerekli/Isteğe bağlı |
|-----------|-------------------------------------------|---------------------|
| `uri`     | Dış PLS belgesinin adresi. | Gereklidir.           |

**Kullanım**

Birden çok varlığın nasıl okunduğunu tanımlamak için, bir. xml veya. pls dosyası olarak depolanan özel bir sözlük oluşturabilirsiniz. Aşağıda örnek bir. xml dosyası verilmiştir.

```xml
<?xml version="1.0" encoding="UTF-8"?>
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

`lexicon`Öğesi en az bir öğe içeriyor `lexeme` . Her `lexeme` öğe en az bir `grapheme` öğe ve bir veya daha fazla `grapheme` , `alias` , ve `phoneme` öğesi içerir. `grapheme`Öğesi, <a href="https://www.w3.org/TR/pronunciation-lexicon/#term-Orthography" target="_blank">dikgrafi <span class="docon docon-navigate-external x-hidden-focus"></span> </a>tanımlayan metni içerir. `alias`Öğeler, bir kısaltın veya kısaltılmış bir terimin telaffuz olduğunu göstermek için kullanılır. `phoneme`Öğesi, nasıl bir açıklama ekleneceğini açıklayan metin sağlar `lexeme` .

Özel sözlüğü kullanarak bir sözcüğün söylenişini doğrudan ayarlayamayacağınızı aklınızda olmak önemlidir. İçin telaffuz ayarlamanız gerekiyorsa, önce bir belirtin ve `alias` ardından `phoneme` ile ilişkilendirin `alias` . Örnek:

```xml
  <lexeme>
    <grapheme>Scotland MV</grapheme> 
    <alias>ScotlandMV</alias> 
  </lexeme>
  <lexeme>
    <grapheme>ScotlandMV</grapheme> 
    <phoneme>ˈskɒtlənd.ˈmiːdiəm.weɪv</phoneme>
  </lexeme>
```

> [!IMPORTANT]
> `phoneme`IPA kullanılırken öğe boşluk içeremez.

Özel sözlük dosyası hakkında daha fazla bilgi için bkz. [telaffuz sözlüğü belirtim (PLS) sürüm 1,0](https://www.w3.org/TR/pronunciation-lexicon/).

Sonra, özel sözlük dosyanızı yayımlayın. Bu dosyanın nerede depolanabileceği konusunda kısıtlamalar olmadığı sürece [Azure Blob depolamayı](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)kullanmanızı öneririz.

Özel sözlüğü yayımladıktan sonra SSML 'nizden buna başvurabilirsiniz.

> [!NOTE]
> `lexicon`Öğe, öğesinin içinde olmalıdır `voice` .

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" 
          xmlns:mstts="http://www.w3.org/2001/mstts" 
          xml:lang="en-US">
    <voice name="en-US-AriaRUS">
        <lexicon uri="http://www.example.com/customlexicon.xml"/>
        BTW, we will be there probably at 8:00 tomorrow morning.
        Could you help leave a message to Robert Benigni for me?
    </voice>
</speak>
```

Bu özel sözlük kullanılırken "BTW", "sizin" olarak okunacak. "Benignı", belirtilen IPA "bɛ ˈ nı ː nji" ile okunacaktır.  

**Sınırlamalar**
- Dosya boyutu: özel sözlük dosyası boyutu üst sınırı 100KB, bu boyuttan daha fazla olursa sensıs isteği başarısız olur.
- Sözlük önbelleği yenilemesi: özel sözlük, ilk yüklendiğinde TTS hizmetinde anahtar olarak URI ile önbelleğe alınır. Aynı URI 'ye sahip bir sözlük 15 dakika içinde yeniden yüklenmez, bu nedenle özel sözlük değişikliğinin en fazla 15 dakikalık bir geçerlilik yapması gerekir.

**Konuşma hizmeti fonetik kümeleri**

Yukarıdaki örnekte, IPA telefon kümesi olarak da bilinen International fonetik alfabesini kullanıyoruz. Uluslararası standart olduğundan, geliştiricilerin IPA kullanmasını öneririz. Bazı IPA karakterler için Unicode ile temsil edildiğinde ' önceden oluşturulmuş ' ve ' ayrıştırılmış ' sürümü vardır. Özel sözlükte yalnızca ayrıştırılmış unicodes desteklenir.

IPA 'in anımsanması kolay olmadığından, konuşma hizmeti yedi dil (,,,,,, `en-US` `fr-FR` `de-DE` `es-ES` `ja-JP` `zh-CN` ve `zh-TW` ) için bir fonetik kümesi tanımlar.

`sapi`Özniteliği için değerini olarak, `alphabet` aşağıda gösterildiği gibi özel lexsimgeleri ile kullanabilirsiniz:

```xml
<?xml version="1.0" encoding="UTF-8"?>
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

Ayrıntılı konuşma hizmeti fonetik alfabe hakkında daha fazla bilgi için bkz. [konuşma hizmeti fonetik kümeleri](speech-ssml-phonetic-sets.md).

## <a name="adjust-prosody"></a>Prosody 'ı ayarla

`prosody`Öğesi, metinden konuşmaya çıkışı için sıklık, dağılım, Aralık, oran, süre ve birim değişikliklerini belirtmek için kullanılır. `prosody`Öğesi metin ve şu öğeleri içerebilir: `audio` , `break` ,, `p` `phoneme` , `prosody` , `say-as` , `sub` , ve `s` .

Bürünsel öznitelik değerleri geniş bir aralığa göre değişebildiğinden, konuşma tanıyıcı atanan değerleri, seçili sesin gerçek bürünsel değerlerinin ne olduğuna ilişkin bir öneri olarak yorumlar. Metinden konuşmaya hizmeti, desteklenmeyen değerleri sınırlandırır veya yerini alır. Desteklenmeyen değerlere örnek olarak 1 MHz veya 120 birimi gösterilebilir.

**Söz dizimi**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `pitch` | Metnin taban çizgisi aralığını gösterir. Bu aralığı şöyle ifade edebilirsiniz:<ul><li>Sayı olarak ifade edilen ve ardından "Hz" (Hertz) gelen mutlak bir değer. Örneğin, 600 Hz.</li><li>"+" Veya "-" işaretinden sonra gelen ve ardından "Hz" veya "St" gelen bir sayı olarak ifade edilen, sıklığı değiştirecek bir miktar belirten göreli bir değer. Örneğin: + 80 Hz veya-2ST. "St", değişim biriminin standart Diatonic ölçeğinde bir ton (yarım bir adım) yarısı olan semitone olduğunu gösterir.</li><li>Sabit değer:<ul><li>x-düşük</li><li>zayıf</li><li>orta</li><li>yüksek</li><li>x-yüksek</li><li>default</li></ul></li></ul>. | İsteğe Bağlı |
| `contour` |Artık kontur hem sinir hem de standart sesleri desteklemektedir. Dağılım, sıklık içindeki değişiklikleri temsil eder. Bu değişiklikler, konuşma çıkışında belirlenen zaman konumlarında bir hedef dizisi olarak gösterilir. Her hedef, parametre çiftleri kümesi tarafından tanımlanır. Örnek: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>Her bir parametre kümesindeki ilk değer, metnin süresinin yüzdesi olarak sıklık değişikliğinin konumunu belirtir. İkinci değer, bir göreli değer veya sıklık için bir numaralandırma değeri kullanarak, sıklığı yükseltmek veya azaltmak için miktarı belirtir (bkz `pitch` .). | İsteğe Bağlı |
| `range` | Metin için sıklık aralığını temsil eden bir değer. `range`' I betimleyen mutlak değerleri, göreli değerleri veya numaralandırma değerlerini kullanarak ifade edebilirsiniz `pitch` . | İsteğe Bağlı |
| `rate` | Metnin konuşma oranını gösterir. Şöyle ifade edebilirsiniz `rate` :<ul><li>Varsayılan değer çarpanı olarak davranan sayı olarak ifade edilen göreli bir değer. Örneğin, *1* değeri, fiyata hiçbir değişikliğe neden olmaz. *0,5* değeri, oranın bir haline neden olur. *3* değeri, ücretle sonuçlanmasına neden olur.</li><li>Sabit değer:<ul><li>x-yavaş</li><li>dığını</li><li>orta</li><li>Hızlı</li><li>x-Fast</li><li>default</li></ul></li></ul> | İsteğe Bağlı |
| `duration` | Konuşma birleştirme (TTS) hizmeti, metni saniye veya milisaniye olarak okurken geçmesi gereken süre. Örneğin, *2s* veya *1800ms*. | İsteğe Bağlı |
| `volume` | Konuşma sesinizin birim düzeyini gösterir. Birimi şu şekilde ifade edebilirsiniz:<ul><li>*Deetest* 'den *loudest*'e kadar 0,0 ile 100,0 arasında bir sayı olarak ifade edilen mutlak bir değer. Örneğin, 75. Varsayılan değer 100,0 ' dir.</li><li>Birimin değiştirileceği miktarı belirten, "+" veya "-" işaretinden önce gelen sayı olarak ifade edilen göreli bir değer. Örneğin, + 10 veya-5,5.</li><li>Sabit değer:<ul><li>katılımı</li><li>x-Soft</li><li>yumuşatılmış</li><li>orta</li><li>DIN</li><li>x-yüksek</li><li>default</li></ul></li></ul> | İsteğe Bağlı |

### <a name="change-speaking-rate"></a>Konuşma hızını değiştir

Konuşma ücreti, sinir seslere ve standart seslere, sözcük veya tümce düzeyinde uygulanabilir. 

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-GuyNeural">
        <prosody rate="+30.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-volume"></a>Birimi Değiştir

Birim değişiklikleri, standart seslere veya tümce düzeyinde uygulanabilir. Birim değişiklikleri, tümce düzeyinde yalnızca sinir seslere uygulanabilir.

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

### <a name="change-pitch"></a>Değiştirme aralığı

Sıklık değişiklikleri, standart seslere veya tümce düzeyinde uygulanabilir. Sıklık değişiklikleri yalnızca tümce düzeyinde sinir seslere uygulanabilir.

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Aralık dağılımı değiştirme

> [!IMPORTANT]
> Aralık dağılımı değişiklikleri artık sinir seslerle desteklenmektedir.

**Örnek**

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AriaNeural">
        <prosody contour="(60%,-60%) (100%,+80%)" >
            Were you the only person in the room? 
        </prosody>
    </voice>
</speak>
```
## <a name="say-as-element"></a>as öğesi

`say-as`, öğe metninin (sayı veya tarih gibi) içerik türünü belirten isteğe bağlı bir öğedir. Bu, metnin nasıl pronounce hakkında konuşma birleştirme altyapısına kılavuzluk sağlar.

**Söz dizimi**

```XML
<say-as interpret-as="string" format="digit string" detail="string"> <say-as>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `interpret-as` | Öğe metninin içerik türünü gösterir. Türlerin listesi için aşağıdaki tabloya bakın. | Gerekli |
| `format` | Belirsiz biçimleri olabilecek içerik türleri için öğenin metninin kesin biçimlendirmesi hakkında ek bilgiler sağlar. SSML bunları kullanan içerik türleri için biçimleri tanımlar (aşağıdaki tabloya bakın). | İsteğe Bağlı |
| `detail` | Söylenen ayrıntı düzeyini gösterir. Örneğin, bu öznitelik konuşma senkiyle motor pronounce noktalama işaretlerinin olmasını isteyebilir. İçin tanımlanmış standart değer yok `detail` . | İsteğe Bağlı |

<!-- I don't understand the last sentence. Don't we know which one Cortana uses? -->

Ve öznitelikleri için desteklenen içerik türleri aşağıda verilmiştir `interpret-as` `format` . `format`Yalnızca `interpret-as` Tarih ve saat olarak ayarlandıysa özniteliği ekleyin.

| farklı yorumlama | biçim | Yorum |
|--------------|--------|----------------|
| `address` | | Metin bir adres olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`I'm at <say-as interpret-as="address">150th CT NE, Redmond, WA</say-as>`<br /><br />Yani "150th mahkeme, Kuzey Doğu Redmond Washington." |
| `cardinal`, `number` | | Metin bir Kardinal sayı olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`There are <say-as interpret-as="cardinal">3</say-as> alternatives`<br /><br />"Üç alternatif vardır." |
| `characters`, `spell-out` | | Metin, tek tek harfler (yazılmış) olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`<say-as interpret-as="characters">test</say-as>`<br /><br />"T E S T" olarak. |
| `date` | DMY, mdy, yımd, yıdm, YM, My, MD, DM, d, m, y | Metin bir tarih olarak konuşulur. `format`Öznitelik, tarihin biçimini (*d = Day, m = month ve y = Year*) belirtir. Konuşma birleştirme motoru pronounces:<br /><br />`Today is <say-as interpret-as="date" format="mdy">10-19-2016</say-as>`<br /><br />"Bugün Ekim, 2016. |
| `digits`, `number_digit` | | Metin, tek basamaklı bir dizi olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`<say-as interpret-as="number_digit">123456789</say-as>`<br /><br />"1 2 3 4 5 6 7 8 9" olarak |
| `fraction` | | Metin kesirli bir sayı olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br /> `<say-as interpret-as="fraction">3/8</say-as> of an inch`<br /><br />"Bir inç üç sekizde biri." |
| `ordinal` | | Metin bir sıra numarası olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`Select the <say-as interpret-as="ordinal">3rd</say-as> option`<br /><br />As "üçüncü seçeneği belirleyin". |
| `telephone` | | Metin telefon numarası olarak konuşulur. `format`Öznitelik, bir ülke kodunu temsil eden rakamlar içerebilir. Örneğin, Birleşik Devletler için "1" veya Italya için "39". Konuşma birleştirme altyapısı, bu bilgileri bir telefon numarası söylenişini yönlendirecek şekilde kullanabilir. Telefon numarası da ülke kodunu içerebilir ve bu durumda, içindeki ülke kodundan önceliklidir `format` . Konuşma birleştirme motoru pronounces:<br /><br />`The number is <say-as interpret-as="telephone" format="1">(888) 555-1212</say-as>`<br /><br />As "My No Area Code 8 8 8 5 5 5 1 2 1 2." |
| `time` | hms12, hms24 | Metin bir zaman olarak konuşulur. `format`Öznitelik, saatin 12 saatlik bir saat (hms12) veya 24 saatlik saat (hms24) kullanılarak mı belirtilmediğini belirtir. Saatleri, dakikaları ve saniyeleri temsil eden sayıları ayırmak için iki nokta üst üste kullanın. Şu geçerli zaman örnekleri şunlardır: 12:35, 1:14:32, 08:15 ve 02:50:45. Konuşma birleştirme motoru pronounces:<br /><br />`The train departs at <say-as interpret-as="time" format="hms12">4:00am</say-as>`<br /><br />"," Parçaları dört bir A 'da eğitme. " |

**Kullanım**

`say-as`Öğe yalnızca metin içerebilir.

**Örnek**

Konuşma sennet12 35 Me motoru, "ilk isteğiniz," Ekim 20 10 ' de erken gelişle birlikte, en az bir yere kadar bir yere vardı.
 
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

## <a name="add-recorded-audio"></a>Kayıtlı ses ekleme

`audio`, bir SSML belgesine MP3 sesi eklemenize olanak sağlayan isteğe bağlı bir öğedir. Ses öğesinin gövdesi, ses dosyasının kullanılamadığı veya yürütülemeyebilir olmadığı durumlarda konuşulan düz metin veya SSML biçimlendirmesi içerebilir. Ayrıca, `audio` öğesi metin ve şu öğeleri içerebilir: `audio` , `break` ,, `p` `s` , `phoneme` , `prosody` , `say-as` ve `sub` .

SSML belgesine dahil edilen tüm seslerin bu gereksinimleri karşılaması gerekir:

* MP3, Internet erişimli bir HTTPS uç noktasında barındırılmalıdır. HTTPS gereklidir ve MP3 dosyasını barındıran etki alanı geçerli, güvenilen bir TLS/SSL sertifikası sunmalıdır.
* MP3 geçerli bir MP3 dosyası (MPEG v2) olmalıdır.
* Bit hızının 48 kbps olması gerekir.
* Örnek hız 16.000 Hz olmalıdır.
* Tek bir yanıttaki tüm metin ve ses dosyaları için Birleşik Toplam süre 90 (90) saniyeyi aşamaz.
* MP3, müşteriye özgü veya diğer gizli bilgileri içermemelidir.

**Söz dizimi**

```xml
<audio src="string"/></audio>
```

**Öznitelikler**

| Öznitelik | Açıklama                                   | Gerekli/Isteğe bağlı                                        |
|-----------|-----------------------------------------------|------------------------------------------------------------|
| `src`     | Ses dosyasının konumunu/URL 'sini belirtir. | SSML belgenizde ses öğesi kullanılıyorsa gereklidir. |

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

## <a name="add-background-audio"></a>Arka plan sesi Ekle

`mstts:backgroundaudio`Öğesi SSML belgelerinize arka plan sesi eklemenize (veya metinden konuşmaya sahip bir ses dosyası karıştırabilmeniz) olanak tanır. İle `mstts:backgroundaudio` , metin okuma başlangıcında durarak bir ses dosyasını arka planda döngüye alabilir ve metin okuma sonunda soluklaştırın.

Belirtilen arka plan sesi metinden konuşmaya veya soluklaştırmadan kısaysa, döngü gerçekleştirilir. Metinden konuşmaya daha uzunsa, bu, silinme bittiğinde durur.

SSML belgesi başına yalnızca bir arka plan ses dosyasına izin verilir. Ancak, `audio` `voice` SSML belgenize Ek ses eklemek için öğesi içindeki etiketleri birbirine bağlayabilirsiniz.

**Söz dizimi**

```XML
<mstts:backgroundaudio src="string" volume="string" fadein="string" fadeout="string"/>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `src` | Arka plan ses dosyasının konumunu/URL 'sini belirtir. | SSML belgenizde arka plan sesi kullanılıyorsa gereklidir. |
| `volume` | Arka plan ses dosyasının hacmini belirtir. **Kabul edilen değerler**: `0` `100` dahil. Varsayılan değer: `1`. | İsteğe Bağlı |
| `fadein` | Arka plan sesinin "belirme süresi" süresini milisaniye olarak belirtir. Varsayılan değer `0` , belirme olmaması ile eşdeğerdir. **Kabul edilen değerler**: `0` `10000` dahil.  | İsteğe Bağlı |
| `fadeout` | Arka plan sesinin milisaniye cinsinden silinme süresini belirtir. Varsayılan değer `0` , soluklaştırma eşdeğeri olan ' tir. **Kabul edilen değerler**: `0` `10000` dahil.  | İsteğe Bağlı |

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

* [Dil desteği: sesler, yerel ayarlar, diller](language-support.md)
