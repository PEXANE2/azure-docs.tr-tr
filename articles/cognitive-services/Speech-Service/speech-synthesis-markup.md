---
title: Konuşma birleştirme biçimlendirme dili (SSML)-konuşma Hizmetleri
titleSuffix: Azure Cognitive Services
description: Söyleniş ve metin okuma, prosody denetlemek için konuşma sentezi biçimlendirme dili kullanma.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: dd535f96c60a3f9259a108f3e8aff643eed1870d
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414719"
---
# <a name="speech-synthesis-markup-language-ssml"></a>Konuşma Sentezi Biçimlendirme Dili (SSML)

Konuşma sentezi biçimlendirme dili (SSML), geliştiricilerin metin okuma hizmeti kullanılarak, giriş metninin birleştirilmiş konuşmaya nasıl dönüştürüldüğünü belirtmesini sağlayan XML tabanlı bir biçimlendirme dilidir. SSML, düz metin ile karşılaştırıldığında, geliştiricilerin sıklık, telaffuz, konuşma oranı, hacim ve metin okuma çıktısından daha fazlasını yapma olanağı sağlar. Bir dönemden sonra duraklatma gibi normal noktalama işaretleri veya bir tümce bir soru işaretiyle sona erdiğinde doğru innnation 'ın kullanılması otomatik olarak işlenir.

SSML 'nin konuşma hizmetleri uygulamasının, World Wide Web Konsorsiyumu [konuşma sen, biçimlendirme dili sürüm 1,0](https://www.w3.org/TR/speech-synthesis)' i temel alır.

> [!IMPORTANT]
> Çince, Japonca ve Korece karakterler, faturalandırma için iki karakter olarak sayılır. Daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="standard-neural-and-custom-voices"></a>Standart, sinir ve özel sesler

Standart ve sinir sesler arasından seçim yapın ya da ürün veya marka için özel sesinizi benzersiz bir şekilde oluşturun. 75 + standart sesler 45 ' den fazla dilde ve yerel ayarlarda kullanılabilir ve 4 dilde ve yerel ayarlarda 5 sinir seste erişilebilir. Desteklenen dillerin, yerel ayarların ve seslerin (sinir ve standart) tüm listesi için bkz. [dil desteği](language-support.md).

Standart, sinir ve özel sesler hakkında daha fazla bilgi edinmek için bkz. [metinden konuşmaya genel bakış](text-to-speech.md).

## <a name="supported-ssml-elements"></a>Desteklenen SSML öğeleri

Her SSML belgesi SSML öğeleri (veya etiketleri) ile oluşturulur. Bu öğeler, sıklık, Prosody, hacim ve daha fazlasını ayarlamak için kullanılır. Aşağıdaki bölümler her bir öğenin nasıl kullanıldığını ve bir öğe gerekli veya isteğe bağlı olduğunu ayrıntılandırır.  

> [!IMPORTANT]
> Öznitelik değerleri etrafında çift tırnak kullanmayı unutmayın. Doğru biçimlendirilmiş, geçerli XML standartları, öznitelik değerlerinin çift tırnak işareti içine alınması gerekir. Örneğin, `<prosody volume="90">` iyi biçimlendirilmiş, geçerli bir öğedir, ancak `<prosody volume=90>` değildir. SSML tırnak içinde olmayan öznitelik değerlerini tanıyamayabilir.

## <a name="create-an-ssml-document"></a>SSML belgesi oluşturma

`speak`, kök öğesidir ve tüm SSML belgeleri için **gereklidir** . `speak` Öğesi sürüm, dil ve biçimlendirme sözlük tanımı gibi önemli bilgiler içerir.

**Söz dizimi**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="string"></speak>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| version | Belge işaretlemesini yorumlamak için kullanılan SSML belirtiminin sürümünü gösterir. Geçerli sürüm 1,0 ' dir. | Gerekli |
| XML: lang | Kök belgenin dilini belirtir. Değer küçük harf, iki harfli dil kodu (örneğin, **en**) veya dil kodu ve büyük harfli ülke/bölge (örneğin, **en-US**) içerebilir. | Gerekli |
| özniteliði | SSML belgesinin biçimlendirme sözlüğünü (öğe türleri ve öznitelik adları) tanımlayan belgenin URI 'sini belirtir. Geçerli URI https://www.w3.org/2001/10/synthesis. | Gerekli |

## <a name="choose-a-voice-for-text-to-speech"></a>Metinden konuşmaya için bir ses seçin

`voice` Öğe gereklidir. Metin okuma için kullanılan sesi belirtmek için kullanılır.

**Söz dizimi**

```xml
<voice name="string">
    This text will get converted into synthesized speech.
</voice>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| name | Metinden konuşmaya çıkış için kullanılan sesi tanımlar. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech). | Gerekli |

**Örnek**

> [!NOTE]
> Bu örnek, `en-US-Jessa24kRUS` sesini kullanır. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech).

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        This is the text that is spoken.
    </voice>
</speak>
```

## <a name="use-multiple-voices"></a>Birden çok kişilerden daha fazlasını kullanın

`speak` Öğesi içinde, metinden konuşmaya çıkış için birden çok ses belirtebilirsiniz. Bu sesler farklı dillerde olabilir. Her ses için, metnin bir `voice` öğe içinde sarmalanması gerekir.

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| name | Metinden konuşmaya çıkış için kullanılan sesi tanımlar. Desteklenen seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech). | Gerekli |

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Good morning!
    </voice>
    <voice  name="en-US-Guy24kRUS">
        Good morning to you too Jessa!
    </voice>
</speak>
```

## <a name="adjust-speaking-styles"></a>Konuşma stillerini ayarla

> [!IMPORTANT]
> Bu özellik yalnızca sinir seslerle çalışır.

Varsayılan olarak, metinden konuşmaya hizmeti, standart ve sinir sesler için bağımsız bir konuşma stili kullanarak metni birleştirir. Sinir seslerle, konuşma stilini, `<mstts:express-as>` öğesiyle birlikte hızlı bir şekilde, empabillik veya yaklaşım için ayarlayabilirsiniz. Bu, Azure konuşma Hizmetleri için benzersiz olan isteğe bağlı bir öğedir.

Şu anda, bu sinir sesleri için konuşma stil ayarlamaları desteklenir:
* `en-US-JessaNeural`
* `zh-CN-XiaoxiaoNeural`

Değişiklikler tümce düzeyinde uygulanır ve stil sese göre farklılık gösterir. Bir stil desteklenmiyorsa, hizmet konuşmayı varsayılan nötr konuşma stilinde döndürür.

**Söz dizimi**

```xml
<mstts:express-as type="string"></mstts:express-as>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| türü | Konuşma stilini belirtir. Şu anda, konuşma stilleri sese özeldir. | Sinir Voice için konuşma stilini ayarlarken gereklidir. Kullanıyorsanız `mstts:express-as`, Type belirtilmelidir. Geçersiz bir değer sağlanmışsa, bu öğe yok sayılır. |

Her sinir sesi için hangi konuşma stillerinin desteklendiğini öğrenmek için bu tabloyu kullanın.

| Ses | Type | Açıklama |
|-------|------|-------------|
| `en-US-JessaNeural` | tür =`cheerful` | Olumlu ve mutlu bir ehareketini ifade eder |
| | tür =`empathy` | Bir fikir ve anlalama hakkında ifade eder |
| `zh-CN-XiaoxiaoNeural` | tür =`newscast` | Haber yayınlarına benzer şekilde resmi bir tonu ifade eder |
| | tür =`sentiment` | Dokunmadan bir ileti veya hikaye ileten |

**Örnek**

Bu SSML kod parçacığı, `<mstts:express-as>` konuşma stilini olarak `cheerful`değiştirmek için öğesinin nasıl kullanıldığını gösterir.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-JessaNeural">
        <mstts:express-as type="cheerful">
            That'd be just amazing!
        </mstts:express-as>
    </voice>
</speak>
```

## <a name="add-or-remove-a-breakpause"></a>Kesme/duraklatma ekleme veya kaldırma

Sözcükler arasına duraklamalar (veya kesintiler) eklemek için öğesinikullanınveyaotomatikolarakmetinokumahizmetitarafındanduraklamalarıönleyin.`break`

> [!NOTE]
> Bu sözcük veya tümcecik için birleştirilmiş konuşma doğal olmayan bir sözcük veya tümcecik için metin okuma (TTS) varsayılan davranışını geçersiz kılmak için bu öğeyi kullanın. Metin `strength` okuma `none` hizmeti tarafından otomatik olarak yerleştirilen bir bürünsel kesmesini engellemek için olarak ayarlayın.

**Söz dizimi**

```xml
<break strength="string" />
<break time="string" />
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| gücüyle | Aşağıdaki değerlerden birini kullanarak duraklamanın göreli süresini belirtir:<ul><li>yok</li><li>x-zayıf</li><li>zayıf</li><li>Orta (varsayılan)</li><li>güçlü</li><li>x-Strong</li></ul> | İsteğe Bağlı |
| time | Saniye veya milisaniye cinsinden bir duraklama 'nin mutlak süresini belirtir. Geçerli değer örnekleri 2s ve 500 ' dir | İsteğe Bağlı |

| Gücüyle | Açıklama |
|----------|-------------|
| Hiçbiri veya hiçbir değer sağlanmazsa | 0 MS |
| x-zayıf | 250 MS |
| zayıf | 500 ms |
| orta | 750 ms |
| güçlü | 1000 ms |
| x-Strong | 1250 MS |


**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        Welcome to Microsoft Cognitive Services <break time="100ms" /> Text-to-Speech API.
    </voice>
</speak>
```

## <a name="specify-paragraphs-and-sentences"></a>Paragrafları ve tümceleri belirtme

`p`ve `s` öğeleri, sırasıyla paragrafları ve tümceleri göstermek için kullanılır. Bu öğelerin yokluğunda, metinden konuşmaya hizmeti, SSML belgesinin yapısını otomatik olarak belirler.

`phoneme` `prosody` Öğesimetin`say-as`ve şu öğeleri içerebilir: `audio`, `break`,,,, `sub`, ,`mstts:express-as`ve .`s` `p`

`phoneme` `break` `audio` `prosody` `sub`Öğesi metin ve şu öğeleri içerebilir:,,,, `say-as`, `mstts:express-as`, ve. `s`

**Söz dizimi**

```XML
<p></p>
<s></s>
```

**Örnek**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
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

`ph` Öğesi SSML belgelerindeki fonetik telaffuz için kullanılır. `ph` Öğesi yalnızca metin içerebilir, başka öğe içeremez. Her zaman bir geri dönüş olarak insan tarafından okunabilen konuşmayı sağlayın.

Fonetik alfabeller, bazen birlikte harflerin, sayıların veya karakterlerin üzerinde oluşan telefonlardan oluşur. Her telefonda benzersiz bir konuşma sesi açıklanır. Bu Latin alfabesinden farklıdır, burada herhangi bir harf birden çok konuşulan sesi temsil edebilir. "Candy" ve "işten" kelimelerdeki "c" harfinin farklı söylenmelerini veya "şeyler" ve "Bunlar" kelimelerinde "TH" harf birleşiminin farklı söylenmelerini göz önünde bulundurun.

**Söz dizimi**

```XML
<phoneme alphabet="string" ph="string"></phoneme>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| yi | `ph` Öznitelikteki dizenin okunuşunu birleştirirken kullanılacak fonetik alfabesini belirtir. Alfabeyi belirten dize küçük harfle belirtilmelidir. Aşağıdakiler, belirtebileceğiniz olası harfler sonuçlardır.<ul><li>ipa &ndash; uluslararası fonetik alfabesi</li><li>sapı &ndash; konuşma API telefon kümesi</li><li>UPS &ndash; Universal telefon kümesi</li></ul>Alfabe yalnızca öğedeki Fonem için geçerlidir. Daha fazla bilgi için bkz. [Fonetik alfabe başvurusu](https://msdn.microsoft.com/library/hh362879(v=office.14).aspx). | İsteğe Bağlı |
| f | `phoneme` Öğe içinde sözcüğün söylenişini belirten telefonları içeren bir dize. Belirtilen dize tanınmayan telefonlar içeriyorsa, metinden konuşmaya (TTS) hizmeti SSML belgesinin tamamını reddeder ve belgede belirtilen konuşma çıktısından hiçbiri üretir. | Phonemes kullanılıyorsa gereklidir. |

**Örnekler**

```XML
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <s>His name is Mike <phoneme alphabet="ups" ph="JH AU"> Zhou </phoneme></s>
    </voice>
</speak>
```

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Jessa24kRUS">
        <phoneme alphabet="ipa" ph="t&#x259;mei&#x325;&#x27E;ou&#x325;"> tomato </phoneme>
    </voice>
</speak>
```

## <a name="adjust-prosody"></a>Prosody 'ı ayarla

`prosody` Öğesi, metinden konuşmaya çıkışı için sıklık, countur, Aralık, oran, süre ve birim değişikliklerini belirtmek için kullanılır. `p` `phoneme` Öğesimetin`prosody`ve şu öğeleri içerebilir: `audio`, `break`,,,, `say-as`, ,`sub`ve .`s` `prosody`

Bürünsel öznitelik değerleri geniş bir aralığa göre değişebildiğinden, konuşma tanıyıcı atanan değerleri, seçili sesin gerçek bürünsel değerlerinin ne olduğuna ilişkin bir öneri olarak yorumlar. Metinden konuşmaya hizmeti, desteklenmeyen değerleri sınırlandırır veya yerini alır. Desteklenmeyen değerlere örnek olarak 1 MHz veya 120 birimi gösterilebilir.

**Söz dizimi**

```XML
<prosody pitch="value" contour="value" range="value" rate="value" duration="value" volume="value"></prosody>
```

**Öznitelikler**

| Öznitelik | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| perde | Metnin taban çizgisi aralığını gösterir. Bu aralığı şöyle ifade edebilirsiniz:<ul><li>Sayı olarak ifade edilen ve ardından "Hz" (Hertz) gelen mutlak bir değer. Örneğin, 600Hz.</li><li>"+" Veya "-" işaretinden sonra gelen ve ardından "Hz" veya "St" gelen bir sayı olarak ifade edilen, sıklığı değiştirecek bir miktar belirten göreli bir değer. Örneğin: + 80Hz veya-2ST. "St", değişim biriminin standart Diatonic ölçeğinde bir ton (yarım bir adım) yarısı olan semitone olduğunu gösterir.</li><li>Sabit değer:<ul><li>x-düşük</li><li>düşük</li><li>orta</li><li>Geniş</li><li>x-yüksek</li><li>default</li></ul></li></ul>. | İsteğe Bağlı |
| kapalı | Sinir sesleriniz için kontur desteklenmez. Dağılım, konuşma çıkışında belirtilen zaman konumlarında bir hedef dizisi olarak konuşma içeriği için değişen değişiklik değişikliklerini temsil eder. Her hedef, parametre çiftleri kümesi tarafından tanımlanır. Örneğin: <br/><br/>`<prosody contour="(0%,+20Hz) (10%,-2st) (40%,+10Hz)">`<br/><br/>Her bir parametre kümesindeki ilk değer, metnin süresinin yüzdesi olarak sıklık değişikliğinin konumunu belirtir. İkinci değer, bir göreli değer veya sıklık için bir numaralandırma değeri kullanarak, sıklığı yükseltmek veya azaltmak için miktarı belirtir (bkz `pitch`.). | İsteğe Bağlı |
| aralığı  | Metin için sıklık aralığını temsil eden bir değer. ' I betimleyen `range` `pitch`mutlak değerleri, göreli değerleri veya numaralandırma değerlerini kullanarak ifade edebilirsiniz. | İsteğe Bağlı |
| derecelendir  | Metnin konuşma oranını gösterir. Şöyle ifade `rate` edebilirsiniz:<ul><li>Varsayılan değer çarpanı olarak davranan sayı olarak ifade edilen göreli bir değer. Örneğin, *1* değeri, fiyata hiçbir değişikliğe neden olmaz. *5* değeri, oranın bir haline neden olur. *3* değeri, ücretle sonuçlanmasına neden olur.</li><li>Sabit değer:<ul><li>x-yavaş</li><li>dığını</li><li>orta</li><li>hızlı</li><li>x-Fast</li><li>default</li></ul></li></ul> | İsteğe Bağlı |
| duration  | Konuşma birleştirme (TTS) hizmeti, metni saniye veya milisaniye olarak okurken geçmesi gereken süre. Örneğin, *2s* veya *1800ms*. | İsteğe Bağlı |
| birim  | Konuşma sesinizin birim düzeyini gösterir. Birimi şu şekilde ifade edebilirsiniz:<ul><li>*Deetest* 'den *loudest*'e kadar 0,0 ile 100,0 arasında bir sayı olarak ifade edilen mutlak bir değer. Örneğin, 75. Varsayılan değer 100,0 ' dir.</li><li>Birimin değiştirileceği miktarı belirten, "+" veya "-" işaretinden önce gelen sayı olarak ifade edilen göreli bir değer. Örneğin + 10 veya-5,5.</li><li>Sabit değer:<ul><li>katılımı</li><li>x-Soft</li><li>yumuşatılmış</li><li>orta</li><li>DIN</li><li>x-yüksek</li><li>default</li></ul></li></ul> | İsteğe Bağlı |

### <a name="change-speaking-rate"></a>Konuşma hızını değiştirme

Konuşma ücreti, standart seslere veya tümce düzeyinde uygulanabilir. Konuşma oranı yalnızca tümce düzeyinde sinir seslere uygulanabilir.

**Örnek**

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice  name="en-US-Guy24kRUS">
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
    <voice  name="en-US-Jessa24kRUS">
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
    <voice  name="en-US-Guy24kRUS">
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
    <voice  name="en-US-Jessa24kRUS">
        <prosody contour="(80%,+20%) (90%,+30%)" >
            Good morning.
        </prosody>
    </voice>
</speak>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Dil desteği: sesler, yerel ayarlar, diller](language-support.md)
