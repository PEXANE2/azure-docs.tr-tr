---
title: Konuşma birleştirme biçimlendirme dili (SSML)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Söyleniş ve metin okuma, prosody denetlemek için konuşma sentezi biçimlendirme dili kullanma.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/05/2020
ms.author: dapine
ms.openlocfilehash: 68691ad60542c55db4d381e2923a9f928a22995a
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79220506"
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

`speak` kök öğesidir ve tüm SSML belgeleri için **gereklidir** . `speak` öğesi sürüm, dil ve biçimlendirme sözlük tanımı gibi önemli bilgiler içerir.

**Sözdizimi**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `version` | Belge işaretlemesini yorumlamak için kullanılan SSML belirtiminin sürümünü gösterir. Geçerli sürüm 1,0 ' dir. | Gerekli |
| `xml:lang` | Kök belgenin dilini belirtir. Değer küçük harf, iki harfli dil kodu (örneğin, `en`) veya dil kodu ve büyük harfli ülke/bölge (örneğin, `en-US`) içerebilir. | Gerekli |
| `xmlns` | SSML belgesinin biçimlendirme sözlüğünü (öğe türleri ve öznitelik adları) tanımlayan belgenin URI 'sini belirtir. Geçerli URI https://www.w3.org/2001/10/synthesis. | Gerekli |

## <a name="choose-a-voice-for-text-to-speech"></a>Metinden konuşmaya için bir ses seçin

`voice` öğesi gereklidir. Metin okuma için kullanılan sesi belirtmek için kullanılır.

**Sözdizimi**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `name` | Metinden konuşmaya çıkış için kullanılan sesi tanımlar. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech). | Gerekli |

**Örnek**

> [!NOTE]
> Bu örnek `en-US-Jessa24kRUS` sesini kullanır. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Birden çok kişilerden daha fazlasını kullanın

`speak` öğesi içinde, metinden konuşmaya çıkış için birden çok ses belirtebilirsiniz. Bu sesler farklı dillerde olabilir. Her ses için, metnin bir `voice` öğesinde sarmalanması gerekir. 

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `name` | Metinden konuşmaya çıkış için kullanılan sesi tanımlar. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech). | Gerekli |

> [!IMPORTANT]
> Birden çok ses sözcük sınırı özelliğiyle uyumlu değil. Birden çok ses kullanabilmek için sözcük sınırı özelliğinin devre dışı bırakılması gerekir.

### <a name="disable-word-boundary"></a>Sözcük sınırını devre dışı bırak

Konuşma SDK diline bağlı olarak, `"SpeechServiceResponse_Synthesis_WordBoundaryEnabled"` özelliğini `SpeechConfig` nesnesinin bir örneğinde `false` olarak ayarlarsınız.

# <a name="c"></a>[C#](#tab/csharp)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.setproperty?view=azure-dotnet" target="_blank">. <span class="docon docon-navigate-external x-hidden-focus"> </span>`SetProperty` </a>.

```csharp
speechConfig.SetProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="c"></a>[C++](#tab/cpp)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setproperty" target="_blank">. <span class="docon docon-navigate-external x-hidden-focus"> </span>`SetProperty` </a>.

```cpp
speechConfig->SetProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="java"></a>[Java](#tab/java)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.setproperty?view=azure-java-stable#com_microsoft_cognitiveservices_speech_SpeechConfig_setProperty_String_String_" target="_blank">. <span class="docon docon-navigate-external x-hidden-focus"> </span>`setProperty` </a>.

```java
speechConfig.setProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="python"></a>[Python](#tab/python)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#set-property-by-name-property-name--str--value--str-" target="_blank">. <span class="docon docon-navigate-external x-hidden-focus"> </span>`set_property_by_name` </a>.

```python
speech_config.set_property_by_name(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#setproperty-string--string-" target="_blank">. <span class="docon docon-navigate-external x-hidden-focus"> </span>`setProperty` </a>.

```javascript
speechConfig.setProperty(
    "SpeechServiceResponse_Synthesis_WordBoundaryEnabled", "false");
```

# <a name="objective-c"></a>[Objective-C](#tab/objectivec)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#setpropertytobyname" target="_blank">. <span class="docon docon-navigate-external x-hidden-focus"> </span>`setPropertyTo` </a>.

```objectivec
[speechConfig setPropertyTo:@"false" byName:@"SpeechServiceResponse_Synthesis_WordBoundaryEnabled"];
```

# <a name="swift"></a>[Swift](#tab/swift)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/objectivec/cognitive-services/speech/spxspeechconfiguration#setpropertytobyname" target="_blank">. <span class="docon docon-navigate-external x-hidden-focus"> </span>`setPropertyTo` </a>.

```swift
speechConfig!.setPropertyTo(
    "false", byName: "SpeechServiceResponse_Synthesis_WordBoundaryEnabled")
```

---

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
        Good morning!
    </voice>
    <voice name="en-US-Guy24kRUS">
        Good morning to you too Jessa!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Konuşma stillerini ayarla

> [!IMPORTANT]
> Konuşma stillerinin ayarlanması yalnızca sinir seslerle çalışır.

Varsayılan olarak, metinden konuşmaya hizmeti, standart ve sinir sesler için bağımsız bir konuşma stili kullanarak metni birleştirir. Sinir seslerle konuşma stilini, `<mstts:express-as>` öğesiyle birlikte hızlı bir şekilde ifade etmek, empabilmek veya duygu yapmak için ayarlayabilirsiniz. Bu, konuşma hizmetine özgü olan isteğe bağlı bir öğedir.

Şu anda, bu sinir sesleri için konuşma stil ayarlamaları desteklenir:
* `en-US-JessaNeural`
* `pt-BR-FranciscaNeural`
* `zh-CN-XiaoxiaoNeural`

Değişiklikler tümce düzeyinde uygulanır ve stil sese göre farklılık gösterir. Bir stil desteklenmiyorsa, hizmet konuşmayı varsayılan nötr konuşma stilinde döndürür.

**Sözdizimi**

```xml
<mstts:express-as type="string"></mstts:express-as>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `type` | Konuşma stilini belirtir. Şu anda konuşma stilleri sese özgüdür. | Sinir Voice için konuşma stilini ayarlarken gereklidir. `mstts:express-as`kullanılıyorsa, tür sağlanmalıdır. Geçersiz bir değer sağlanmışsa, bu öğe yok sayılır. |

Her sinir sesi için hangi konuşma stillerinin desteklendiğini öğrenmek için bu tabloyu kullanın.

| Ses | Tür | Açıklama |
|-------|------|-------------|
| `en-US-JessaNeural` | `type="cheerful"` | Olumlu ve mutlu bir ehareketini ifade eder |
| | `type="empathy"` | Bir fikir ve anlalama hakkında ifade eder |
| | `type="chat"` | Rastgele, gevşek bir tonda konuşabilirsiniz |
| | `type="newscast"` | Haber yayınlarına benzer şekilde resmi bir tonu ifade eder |
| | `type="customerservice"` | Müşteri Hizmetleri olarak kolay ve hasta bir şekilde konuşun |
| `pt-BR-FranciscaNeural` | `type="cheerful"` | Olumlu ve mutlu bir ehareketini ifade eder |
| `zh-CN-XiaoxiaoNeural` | `type="newscast"` | Haber yayınlarına benzer şekilde resmi bir tonu ifade eder |
| | `type="sentiment"` | Dokunmadan bir ileti veya hikaye ileten |

**Örnek**

Bu SSML kod parçacığı, konuşma stilini `cheerful`olarak değiştirmek için `<mstts:express-as>` öğesinin nasıl kullanıldığını gösterir.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-JessaNeural">
        <mstts:express-as type="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Kesme/duraklatma ekleme veya kaldırma

Sözcükler arasına duraklamalar (veya molalar) eklemek için `break` öğesini kullanın veya metin okuma hizmeti tarafından otomatik olarak duraklamaları önleyin.

> [!NOTE]
> Bu sözcük veya tümcecik için birleştirilmiş konuşma doğal olmayan bir sözcük veya tümcecik için metin okuma (TTS) varsayılan davranışını geçersiz kılmak için bu öğeyi kullanın. Metin okuma hizmeti tarafından otomatik olarak yerleştirilen bir bürünsel kesmesini engellemek için `strength` `none` olarak ayarlayın.

**Sözdizimi**

```xml
<break strength="string" />
<break time="string" />
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `strength` | Aşağıdaki değerlerden birini kullanarak duraklamanın göreli süresini belirtir:<ul><li>yok</li><li>x-zayıf</li><li>zayıf</li><li>Orta (varsayılan)</li><li>tanımlayıcı</li><li>x-Strong</li></ul> | İsteğe bağlı |
| `time` | Saniye veya milisaniye cinsinden bir duraklama 'nin mutlak süresini belirtir. Geçerli değer örnekleri `2s` ve `500` | İsteğe bağlı |

| Gücüyle | Açıklama |
|----------|-------------|
| Hiçbiri veya hiçbir değer sağlanmazsa | 0 MS |
| x-zayıf | 250 MS |
| zayıf | 500 ms |
| uzunlukta | 750 ms |
| tanımlayıcı | 1000 ms |
| x-Strong | 1250 MS |


**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Paragrafları ve tümceleri belirtme

`p` ve `s` öğeleri, sırasıyla paragrafları ve tümceleri göstermek için kullanılır. Bu öğelerin yokluğunda, metinden konuşmaya hizmeti, SSML belgesinin yapısını otomatik olarak belirler.

`p` öğesi metin ve şu öğeleri içerebilir: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `sub`, `mstts:express-as`ve `s`.

`s` öğesi metin ve şu öğeleri içerebilir: `audio`, `break`, `phoneme`, `prosody`, `say-as`, `mstts:express-as`ve `sub`.

**Sözdizimi**

```XML
<p></p>
<s></s>
```

**Örnek**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
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

`ph` öğesi SSML belgelerindeki fonetik telaffuz için kullanılır. `ph` öğesi yalnızca metin içerebilir, başka öğe içeremez. Her zaman bir geri dönüş olarak insan tarafından okunabilen konuşmayı sağlayın.

Fonetik alfabeller, bazen birlikte harflerin, sayıların veya karakterlerin üzerinde oluşan telefonlardan oluşur. Her telefonda benzersiz bir konuşma sesi açıklanır. Bu Latin alfabesinden farklıdır, burada herhangi bir harf birden çok konuşulan sesi temsil edebilir. "Candy" ve "işten" kelimelerdeki "c" harfinin farklı söylenmelerini veya "şeyler" ve "Bunlar" kelimelerinde "TH" harf birleşiminin farklı söylenmelerini göz önünde bulundurun.

**Sözdizimi**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `alphabet` | `ph` özniteliğinde dizenin okunuşunu senilirken kullanılacak fonetik alfabesini belirtir. Alfabeyi belirten dize küçük harfle belirtilmelidir. Aşağıdakiler, belirtebileceğiniz olası harfler sonuçlardır.<ul><li>`ipa` &ndash; uluslararası fonetik alfabesi</li><li>`sapi` &ndash; konuşma hizmeti fonetik alfabesi</li><li>`ups` &ndash; Evrensel telefon kümesi</li></ul><br>Alfabe yalnızca öğesindeki `phoneme` için geçerlidir. Daha fazla bilgi için bkz. [Fonetik alfabe başvurusu](https://en.wikipedia.org/wiki/International_Phonetic_Alphabet). | İsteğe bağlı |
| `ph` | `phoneme` öğesinde sözcüğün söylenişini belirten telefonları içeren bir dize. Belirtilen dize tanınmayan telefonlar içeriyorsa, metinden konuşmaya (TTS) hizmeti SSML belgesinin tamamını reddeder ve belgede belirtilen konuşma çıktısından hiçbiri üretir. | Phonemes kullanılıyorsa gereklidir. |

**Örnekler**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

## <a name="use-custom-lexicon-to-improve-pronunciation"></a>Söylenişi geliştirmek için özel sözlüğü kullanma

Bazen TTS, örneğin bir şirket veya yabancı ad gibi bir kelimeyi doğru bir şekilde pronounce. Geliştiriciler, `phoneme` ve `sub` etiketi kullanarak SSML 'de bu varlıkların okunmasını tanımlayabilir veya `lexicon` etiketi kullanarak özel bir sözlük dosyasına başvurarak birden çok varlığın okunmasını tanımlayabilir.

**Sözdizimi**

```XML
<lexicon uri="string"/>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `uri` | Dış PLS belgesinin adresi. | Gereklidir. |

**Kullanım**

1\. Adım: özel sözlüğü tanımlama 

Varlıkların, bir. xml veya. pls dosyası olarak depolanan özel bir sözlük öğeleri listesiyle okunmasını tanımlayabilirsiniz.

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

Her `lexeme` öğesi bir sözlük öğesidir. `grapheme`, `lexeme`dikgraf tanımlayan metni içerir. Reasyon formu, `alias`olarak sağlanıyor. Telefon dizesi `phoneme` öğesinde sağlanıyor.

`lexicon` öğesi en az bir `lexeme` öğesi içeriyor. Her `lexeme` öğesi en az bir `grapheme` öğesi ve bir veya daha fazla `grapheme`, `alais`ve `phoneme` öğesi içerir. `grapheme` öğesi, <a href="https://www.w3.org/TR/pronunciation-lexicon/#term-Orthography" target="_blank">dikgrafın <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>açıklandığı metni içerir. `alias` öğeleri, bir kısaltın veya kısaltılmış bir terimin telaffuz olduğunu göstermek için kullanılır. `phoneme` öğesi `lexeme` nasıl bir açıklama ekleneceğini açıklayan metin sağlar.

Özel sözlük dosyası hakkında daha fazla bilgi için bkz. W3C Web sitesinde [telaffuz sözlüğü belirtimi (PLS) sürüm 1,0](https://www.w3.org/TR/pronunciation-lexicon/) .

2\. Adım: adım 1 ' de oluşturulan özel sözlük dosyasını karşıya yükleyin, her yerde saklayabilirsiniz ve bu dosyayı [Azure Blob depolama](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)gibi Microsoft Azure depolamanızı öneririz.

Adım 3: SSML 'de özel sözlük dosyasına bakın

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" 
          xmlns:mstts="http://www.w3.org/2001/mstts" 
          xml:lang="en-US">
<lexicon uri="http://www.example.com/customlexicon.xml"/>
BTW, we will be there probably 8:00 tomorrow morning.
Could you help leave a message to Robert Benigni for me?
</speak>
```
"BTW", "yönteme göre" okunacak. "Benignı", IPA "bɛ ˈ nı ː nji" ile okunacak.  

**Sınırlama**
- Dosya boyutu: özel sözlük dosyası boyutu üst sınırı 100KB, bu boyuttan daha fazla olursa sensıs isteği başarısız olur.
- Sözlük önbelleği yenilemesi: özel sözlük, ilk yüklendiğinde TTS hizmetinde anahtar olarak URI ile önbelleğe alınır. Aynı URI 'ye sahip bir sözlük 15 dakika içinde yeniden yüklenmez, bu nedenle özel sözlük değişikliğinin en fazla 15 dakikalık bir geçerlilik yapması gerekir.

**Konuşma hizmeti fonetik kümeleri**

Yukarıdaki örnekte, IPA telefon kümesi olarak da bilinen International fonetik alfabesini kullanıyoruz. Uluslararası standart olduğundan, geliştiricilerin IPA kullanmasını öneririz. IPA 'in anımsanması kolay olmadığı düşünüldüğünde, konuşma hizmeti yedi dil (`en-US`, `fr-FR`, `de-DE`, `es-ES`, `ja-JP`, `zh-CN`ve `zh-TW`) için bir fonetik kümesi tanımlar.

`alphabet` özniteliği için `sapi`, aşağıda gösterildiği gibi özel lexsimgelerle değerini özniteliği olarak kullanabilirsiniz:

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

Ayrıntılı konuşma hizmeti fonetik alfabe hakkında daha fazla bilgi için bkz. [konuşma hizmeti fonetik kümeleri](speech-ssml-phonetic-sets.md).

## <a name="adjust-prosody"></a>Prosody 'ı ayarla

`prosody` öğesi, metinden konuşmaya çıkışı için sıklık, countur, Aralık, oran, süre ve birim değişikliklerini belirtmek için kullanılır. `prosody` öğesi metin ve şu öğeleri içerebilir: `audio`, `break`, `p`, `phoneme`, `prosody`, `say-as`, `sub`ve `s`.

Bürünsel öznitelik değerleri geniş bir aralığa göre değişebildiğinden, konuşma tanıyıcı atanan değerleri, seçili sesin gerçek bürünsel değerlerinin ne olduğuna ilişkin bir öneri olarak yorumlar. Metinden konuşmaya hizmeti, desteklenmeyen değerleri sınırlandırır veya yerini alır. Desteklenmeyen değerlere örnek olarak 1 MHz veya 120 birimi gösterilebilir.

**Sözdizimi**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `pitch` | Metnin taban çizgisi aralığını gösterir. Bu aralığı şöyle ifade edebilirsiniz:<ul><li>Sayı olarak ifade edilen ve ardından "Hz" (Hertz) gelen mutlak bir değer. Örneğin, 600 Hz.</li><li>"+" Veya "-" işaretinden sonra gelen ve ardından "Hz" veya "St" gelen bir sayı olarak ifade edilen, sıklığı değiştirecek bir miktar belirten göreli bir değer. Örneğin: + 80 Hz veya-2ST. "St", değişim biriminin standart Diatonic ölçeğinde bir ton (yarım bir adım) yarısı olan semitone olduğunu gösterir.</li><li>Sabit değer:<ul><li>x-düşük</li><li>zayıf</li><li>uzunlukta</li><li>geniş</li><li>x-yüksek</li><li>default</li></ul></li></ul>arasında yetersiz alanla karşılaştı. | İsteğe bağlı |
| `contour` | Sinir sesleriniz için kontur desteklenmez. Dağılım, sıklık içindeki değişiklikleri temsil eder. Bu değişiklikler, konuşma çıkışında belirlenen zaman konumlarında bir hedef dizisi olarak gösterilir. Her hedef, parametre çiftleri kümesi tarafından tanımlanır. Örnek: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>Her bir parametre kümesindeki ilk değer, metnin süresinin yüzdesi olarak sıklık değişikliğinin konumunu belirtir. İkinci değer, bir göreli değer veya sıklık için bir numaralandırma değeri kullanarak, aralığı yükseltmek veya azaltmak için miktarı belirtir (bkz. `pitch`). | İsteğe bağlı |
| `range` | Metin için sıklık aralığını temsil eden bir değer. `pitch`anlatmak için kullanılan mutlak değerleri, göreli değerleri veya numaralandırma değerlerini kullanarak `range` ifade edebilirsiniz. | İsteğe bağlı |
| `rate` | Metnin konuşma oranını gösterir. `rate` şu şekilde ifade edebilirsiniz:<ul><li>Varsayılan değer çarpanı olarak davranan sayı olarak ifade edilen göreli bir değer. Örneğin, *1* değeri, fiyata hiçbir değişikliğe neden olmaz. *0,5* değeri, oranın bir haline neden olur. *3* değeri, ücretle sonuçlanmasına neden olur.</li><li>Sabit değer:<ul><li>x-yavaş</li><li>dığını</li><li>uzunlukta</li><li>hızlı</li><li>x-Fast</li><li>default</li></ul></li></ul> | İsteğe bağlı |
| `duration` | Konuşma birleştirme (TTS) hizmeti, metni saniye veya milisaniye olarak okurken geçmesi gereken süre. Örneğin, *2s* veya *1800ms*. | İsteğe bağlı |
| `volume` | Konuşma sesinizin birim düzeyini gösterir. Birimi şu şekilde ifade edebilirsiniz:<ul><li>*Deetest* 'den *loudest*'e kadar 0,0 ile 100,0 arasında bir sayı olarak ifade edilen mutlak bir değer. Örneğin, 75. Varsayılan değer 100,0 ' dir.</li><li>Birimin değiştirileceği miktarı belirten, "+" veya "-" işaretinden önce gelen sayı olarak ifade edilen göreli bir değer. Örneğin, + 10 veya-5,5.</li><li>Sabit değer:<ul><li>katılımı</li><li>x-Soft</li><li>yumuşatılmış</li><li>uzunlukta</li><li>DIN</li><li>x-yüksek</li><li>default</li></ul></li></ul> | İsteğe bağlı |

### <a name="change-speaking-rate"></a>Konuşma hızını değiştirme

Konuşma ücreti, standart seslere veya tümce düzeyinde uygulanabilir. Konuşma oranı yalnızca tümce düzeyinde sinir seslere uygulanabilir.

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Guy24kRUS">
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
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
        <prosody volume="+20.00%">
            Welcome to Microsoft Cognitive Services Text-to-Speech API.
        </prosody>
    </voice>
</speak>
```

### <a name="change-pitch"></a>Aralık değiştirme

Sıklık değişiklikleri, standart seslere veya tümce düzeyinde uygulanabilir. Sıklık değişiklikleri yalnızca tümce düzeyinde sinir seslere uygulanabilir.

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Guy24kRUS">
        Welcome to <prosody pitch="high">Microsoft Cognitive Services Text-to-Speech API.</prosody>
    </voice>
</speak>
```

### <a name="change-pitch-contour"></a>Değişiklik aralık dağılımı

> [!IMPORTANT]
> Aralık dağılımı değişiklikleri sinir seslerle desteklenmez.

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```
## <a name="say-as-element"></a>as öğesi

`say-as`, öğe metninin (sayı veya tarih gibi) içerik türünü belirten isteğe bağlı bir öğedir. Bu, metnin nasıl pronounce hakkında konuşma birleştirme altyapısına kılavuzluk sağlar.

**Sözdizimi**

```XML
<say-as interpret-as="string" format="digit string" detail="string"> <say-as>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `interpret-as` | Öğe metninin içerik türünü gösterir. Türlerin listesi için aşağıdaki tabloya bakın. | Gerekli |
| `format` | Belirsiz biçimleri olabilecek içerik türleri için öğenin metninin kesin biçimlendirmesi hakkında ek bilgiler sağlar. SSML bunları kullanan içerik türleri için biçimleri tanımlar (aşağıdaki tabloya bakın). | İsteğe bağlı |
| `detail` | Söylenen ayrıntı düzeyini gösterir. Örneğin, bu öznitelik konuşma senkiyle motor pronounce noktalama işaretlerinin olmasını isteyebilir. `detail`için tanımlanmış standart değer yok. | İsteğe bağlı |

<!-- I don't understand the last sentence. Don't we know which one Cortana uses? -->

`interpret-as` ve `format` öznitelikleri için desteklenen içerik türleri aşağıda verilmiştir. `format` özniteliğini yalnızca `interpret-as` tarih ve saat olarak ayarlandıysa ekleyin.

| farklı yorumlama | format | Yorum |
|--------------|--------|----------------|
| `address` | | Metin bir adres olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`I'm at <say-as interpret-as="address">150th CT NE, Redmond, WA</say-as>`<br /><br />Yani "150th mahkeme, Kuzey Doğu Redmond Washington." |
| `cardinal`, `number` | | Metin bir Kardinal sayı olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`There are <say-as interpret-as="cardinal">3</say-as> alternatives`<br /><br />"Üç alternatif vardır." |
| `characters`, `spell-out` | | Metin, tek tek harfler (yazılmış) olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`<say-as interpret-as="characters">test</say-as>`<br /><br />"T E S T" olarak. |
| `date` | DMY, mdy, yımd, yıdm, YM, My, MD, DM, d, m, y | Metin bir tarih olarak konuşulur. `format` özniteliği, tarihin biçimini belirtir (*d = Day, m = month ve y = Year*). Konuşma birleştirme motoru pronounces:<br /><br />`Today is <say-as interpret-as="date" format="mdy">10-19-2016</say-as>`<br /><br />"Bugün Ekim, 2016. |
| `digits`, `number_digit` | | Metin, tek basamaklı bir dizi olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`<say-as interpret-as="number_digit">123456789</say-as>`<br /><br />"1 2 3 4 5 6 7 8 9" olarak |
| `fraction` | | Metin kesirli bir sayı olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br /> `<say-as interpret-as="fraction">3/8</say-as> of an inch`<br /><br />"Bir inç üç sekizde biri." |
| `ordinal` | | Metin bir sıra numarası olarak konuşulur. Konuşma birleştirme motoru pronounces:<br /><br />`Select the <say-as interpret-as="ordinal">3rd</say-as> option`<br /><br />As "üçüncü seçeneği belirleyin". |
| `telephone` | | Metin telefon numarası olarak konuşulur. `format` özniteliği bir ülke kodunu temsil eden rakamlar içerebilir. Örneğin, Birleşik Devletler için "1" veya Italya için "39". Konuşma birleştirme altyapısı, bu bilgileri bir telefon numarası söylenişini yönlendirecek şekilde kullanabilir. Telefon numarası da ülke kodunu içerebilir ve bu durumda `format`ülke kodundan önceliklidir. Konuşma birleştirme motoru pronounces:<br /><br />`The number is <say-as interpret-as="telephone" format="1">(888) 555-1212</say-as>`<br /><br />As "My No Area Code 8 8 8 5 5 5 1 2 1 2." |
| `time` | hms12, hms24 | Metin bir zaman olarak konuşulur. `format` özniteliği, saatin 12 saatlik bir saat (hms12) veya 24 saatlik saat (hms24) kullanılarak mı belirtilmediğini belirtir. Saatleri, dakikaları ve saniyeleri temsil eden sayıları ayırmak için iki nokta üst üste kullanın. Şu geçerli zaman örnekleri şunlardır: 12:35, 1:14:32, 08:15 ve 02:50:45. Konuşma birleştirme motoru pronounces:<br /><br />`The train departs at <say-as interpret-as="time" format="hms12">4:00am</say-as>`<br /><br />"," Parçaları dört bir A 'da eğitme. " |

**Kullanım**

`say-as` öğesi yalnızca metin içerebilir.

**Örnek**

Konuşma sennet12 35 Me motoru, "ilk isteğiniz," Ekim 20 10 ' de erken gelişle birlikte, en az bir yere kadar bir yere vardı.
 
```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
        <p>
        Your <say-as interpret-as="ordinal"> 1st </say-as> request was for <say-as interpret-as="cardinal"> 1 </say-as> room
        on <say-as interpret-as="date" format="mdy"> 10/19/2010 </say-as>, with early arrival at <say-as interpret-as="time" format="hms12"> 12:35pm </say-as>.
        </p>
    </voice>
</speak>
```

## <a name="add-recorded-audio"></a>Kayıtlı ses ekleme

`audio`, bir SSML belgesine MP3 sesi eklemenize olanak sağlayan isteğe bağlı bir öğedir. Ses öğesinin gövdesi, ses dosyasının kullanılamadığı veya yürütülemeyebilir olmadığı durumlarda konuşulan düz metin veya SSML biçimlendirmesi içerebilir. Ayrıca, `audio` öğesi metin ve şu öğeleri içerebilir: `audio`, `break`, `p`, `s`, `phoneme`, `prosody`, `say-as`ve `sub`.

SSML belgesine dahil edilen tüm seslerin bu gereksinimleri karşılaması gerekir:

* MP3, Internet erişimli bir HTTPS uç noktasında barındırılmalıdır. HTTPS gereklidir ve MP3 dosyasını barındıran etki alanı geçerli ve güvenilen bir SSL sertifikası sunmalıdır.
* MP3 geçerli bir MP3 dosyası (MPEG v2) olmalıdır.
* Bit hızının 48 kbps olması gerekir.
* Örnek hız 16.000 Hz olmalıdır.
* Tek bir yanıttaki tüm metin ve ses dosyaları için Birleşik Toplam süre 90 (90) saniyeyi aşamaz.
* MP3, müşteriye özgü veya diğer gizli bilgileri içermemelidir.

**Sözdizimi**

```xml
<audio src="string"/></audio>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `src` | Ses dosyasının konumunu/URL 'sini belirtir. | SSML belgenizde ses öğesi kullanılıyorsa gereklidir. |

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Jessa24kRUS">
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

`mstts:backgroundaudio` öğesi SSML belgelerinize arka plan sesi eklemenize (veya metinden konuşmaya sahip bir ses dosyası karıştırabilmeniz) olanak tanır. `mstts:backgroundaudio`, bir ses dosyasını arka planda döngüye sokun ve metin okuma başlangıcında durarak metin okuma sonunda açılır.

Belirtilen arka plan sesi metinden konuşmaya veya soluklaştırmadan kısaysa, döngü gerçekleştirilir. Metinden konuşmaya daha uzunsa, bu, silinme bittiğinde durur.

SSML belgesi başına yalnızca bir arka plan ses dosyasına izin verilir. Ancak, SSML belgenize Ek ses eklemek için `voice` öğesi içinde `audio` Etiketler oluşturabilirsiniz.

**Sözdizimi**

```XML
<mstts:backgroundaudio src="string" volume="string" fadein="string" fadeout="string"/>
```

**Özelliklerine**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `src` | Arka plan ses dosyasının konumunu/URL 'sini belirtir. | SSML belgenizde arka plan sesi kullanılıyorsa gereklidir. |
| `volume` | Arka plan ses dosyasının hacmini belirtir. **Kabul edilen değerler**: `100` dahil `0`. Varsayılan değer: `1`. | İsteğe bağlı |
| `fadein` | Arka plan sesinin "belirme süresi" süresini milisaniye olarak belirtir. Varsayılan değer, belirme olmadan eşdeğer olan `0`. **Kabul edilen değerler**: `10000` dahil `0`.  | İsteğe bağlı |
| `fadeout` | Arka plan sesinin milisaniye cinsinden silinme süresini belirtir. Varsayılan değer `0`, soluklaştırma denk gelen. **Kabul edilen değerler**: `10000` dahil `0`.  | İsteğe bağlı |

**Örnek**

```xml
<speak version="1.0" xml:lang="en-US" xmlns:mstts="http://www.w3.org/2001/mstts">
    <mstts:backgroundaudio src="https://contoso.com/sample.wav" volume="0.7" fadein="3000" fadeout="4000"/>
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)">
        The text provided in this document will be spoken over the background audio.
    </voice>
</speak>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Dil desteği: sesler, yerel ayarlar, diller](language-support.md)
