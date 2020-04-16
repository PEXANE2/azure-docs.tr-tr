---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/15/2020
ms.author: trbye
ms.openlocfilehash: d91082ae3c0ae5e501675a06b02e2f55f20ce236
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399552"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir Azure hesabınız ve Konuşma hizmeti aboneliğiniz olduğu varsayar. Hesabınız ve aboneliğiniz yoksa, [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Bir şey yapmadan önce <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">JavaScript Konuşma <span class="docon docon-navigate-external x-hidden-focus"> </span>SDK'sını </a>yüklemeniz gerekir. Platformunuza bağlı olarak aşağıdaki yönergeleri kullanın:
- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs#get-the-speech-sdk" target="_blank">Node.js<span 
class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=browser#get-the-speech-sdk" target="_blank">Web Tarayıcısı<span class="docon docon-navigate-external x-hidden-focus"></span></a>

Ayrıca, hedef ortama bağlı olarak aşağıdakilerden birini kullanın:

# <a name="import"></a>[Ithalat](#tab/import)

```javascript
import { readFileSync } from "fs";
import {
    AudioConfig,
    SpeechConfig,
    SpeechSynthesisOutputFormat,
    SpeechSynthesizer 
} from "microsoft-cognitiveservices-speech-sdk";
```

Daha fazla `import`bilgi için, <a href="https://javascript.info/import-export" target="_blank">bkz. <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>

# <a name="require"></a>[Gerektirir](#tab/require)

```javascript
const readFileSync = require("fs").readFileSync;
const sdk = require("microsoft-cognitiveservices-speech-sdk");
```

Daha fazla `require`bilgi için, <a href="https://nodejs.org/en/knowledge/getting-started/what-is-require/" target="_blank">ne gerektirdiğini görmek? <span class="docon docon-navigate-external x-hidden-focus"></span></a>.


# <a name="script"></a>[Komut dosyası](#tab/script)

<a href="https://aka.ms/csspeech/jsbrowserpackage" target="_blank">JavaScript Speech <span class="docon docon-navigate-external x-hidden-focus"></span> SDK</a> *microsoft.cognitiveservices.speech.sdk.bundle.js* dosyasını indirin ve ayıklayın ve HTML dosyanıza erişilebilen bir klasöre yerleştirin.

```html
<script src="microsoft.cognitiveservices.speech.sdk.bundle.js"></script>;
```

> [!TIP]
> Bir web tarayıcısını hedefliyorsanız ve etiketi `<script>` kullanıyorsanız; `sdk` önek gerekli değildir. Önek, `sdk` modülü adlandırmak için `require` kullanılan bir diğer addır.

---

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK'sını kullanarak Konuşma hizmetini aramak için [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)bir . Bu sınıf, anahtarınız ve ilişkili bölgeniz, bitiş noktanız, ana bilgisayar veya yetkilendirme belirteciniz gibi aboneliğiniz hakkında bilgiler içerir.

> [!NOTE]
> Konuşma tanıma, konuşma sentezi, çeviri veya niyet tanıma gerçekleştirin, her zaman bir yapılandırma oluşturursunuz.

Bir baş harfe başlatmanın birkaç [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)yolu vardır:

* Abonelikle: bir anahtar ve ilişkili bölgede geçirin.
* Bir bitiş noktası ile: Konuşma hizmeti bitiş noktası geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir ana bilgisayar adresi geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci yle: yetkilendirme jetonundan ve ilişkili bölgeye geçin.

Bu örnekte, bir [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest) abonelik anahtarı ve bölge kullanarak oluşturun. Bölge tanımlayıcınızı bulmak için [bölge destek](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın. Ayrıca, farklı özelleştirmeler için değiştirdiğiniz bu makalenin geri kalanı için kullanmak üzere bazı temel ortak kodlar da oluşturursunuz.

```javascript
function synthesizeSpeech() {
    const speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
}
```

## <a name="synthesize-speech-to-a-file"></a>Konuşmayı bir dosyaya sentezleme

Ardından, konuşmacılara, dosyalara veya diğer çıktı akışlarına metin den konuşmaya dönüşümleri ve çıktıları yürüten bir [`SpeechSynthesizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesizer?view=azure-node-latest) nesne oluşturursunuz. Önceki [`SpeechSynthesizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesizer?view=azure-node-latest) adımda oluşturulan [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest) nesneyi ve çıktı sonuçlarının [`AudioConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig?view=azure-node-latest) nasıl işleneceğini belirten bir nesneyi param olarak kabul eder.

Başlatmak için, `AudioConfig` `fromAudioFileOutput()` statik işlevi kullanarak bir `.wav` dosyaya çıktıyı otomatik olarak yazmak için bir oluştur.

```javascript
function synthesizeSpeech() {
    const speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const audioConfig = AudioConfig.fromAudioFileOutput("path/to/file.wav");
}
```

Ardından, geçen bir `SpeechSynthesizer` `speechConfig` nesneyi ve `audioConfig` nesneyi param olarak anında anons edin. Daha sonra, konuşma sentezini yürütmek ve bir `speakTextAsync()` dosyaya yazma, bir metin dizisiyle çalışmak kadar basittir. Sonuç geri arama aramak `synthesizer.close()`için harika bir yerdir , aslında - bu çağrı sentezi doğru çalışması için gereklidir.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const audioConfig = AudioConfig.fromAudioFileOutput("path-to-file.wav");

    const synthesizer = new SpeechSynthesizer(speechConfig, audioConfig);
    synthesizer.speakTextAsync(
        "A simple test to write to a file.",
        result => {
            if (result) {
                console.log(JSON.stringify(result));
            }
            synthesizer.close();
        }
    },
    error => {
        console.log(error);
        synthesizer.close();
    });
}
```

Programı çalıştırın ve belirttiğiniz `.wav` konuma sentezlenmiş bir dosya yazılır. Bu, en temel kullanıma iyi bir örnektir, ancak daha sonra çıktıyı özelleştirmeye ve çıktı yanıtını özel senaryolarla çalışmak için bellek içi bir akış olarak işlemeye bakabilirsiniz.

## <a name="synthesize-to-speaker-output"></a>Hoparlör çıkışına sentezleme

Bazı durumlarda, sentezlenmiş konuşmayı doğrudan bir konuşmacıya doğrudan çıktınız isteyebilirsiniz. Bunu yapmak için, `AudioConfig` `fromDefaultSpeakerOutput()` statik işlevi kullanarak anlık. Bu, geçerli etkin çıktı aygıtına çıkar.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const audioConfig = AudioConfig.fromDefaultSpeakerOutput();

    const synthesizer = new SpeechSynthesizer(speechConfig, audioConfig);
    synthesizer.speakTextAsync(
        "Synthesizing directly to speaker output.",
        result => {
            if (result) {
                console.log(JSON.stringify(result));
            }
            synthesizer.close();
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

## <a name="get-result-as-an-in-memory-stream"></a>Bellek içi akış olarak sonuç alma

Konuşma uygulaması geliştirmedeki birçok senaryo için, büyük olasılıkla bir dosyaya doğrudan yazmak yerine, ortaya çıkan ses verilerine bellek içi akış olarak gereksinim duymanız gerekir. Bu, şunları içeren özel davranış oluşturmanıza olanak sağlar:

* Ortaya çıkan bayt dizisini özel downstream hizmetleri için aranabilir bir akış olarak özetleyin.
* Sonucu diğer API'ler veya hizmetlerle tümleştirin.
* Ses verilerini değiştirin, `.wav` özel üstbilgiler yazın, vb.

Önceki örnekten bu değişikliği yapmak kolaydır. İlk olarak, `AudioConfig` daha fazla denetim için çıktı davranışını bu noktadan itibaren el ile yöneteceğiniz için bloğu kaldırın. Sonra `undefined` `SpeechSynthesizer` yapıcı `AudioConfig` için geçmek. 

> [!NOTE]
> `undefined` Yukarıdaki hoparlör `AudioConfig`çıktısı örneğinde olduğu gibi atlayarak yerine, geçerli etkin çıkış aygıtında varsayılan olarak sesi oynatmaz.

Bu kez, sonucu bir [`SpeechSynthesisResult`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisresult?view=azure-node-latest) değişkene kaydedin. Özellik `SpeechSynthesisResult.audioData` çıktı `ArrayBuffer` verilerinden birini döndürür. Bununla `ArrayBuffer` el ile çalışabilirsiniz.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const synthesizer = new sdk.SpeechSynthesizer(speechConfig);

    synthesizer.speakTextAsync(
        "Getting the response as an in-memory stream.",
        result => {
            // Interact with the audio ArrayBuffer data
            const audioData = result.audioData;
            console.log(`Audio data byte size: ${audioData.byteLength}.`)

            synthesizer.close();
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

Buradan, ortaya `ArrayBuffer` çıkan nesneyi kullanarak herhangi bir özel davranış uygulayabilirsiniz.

## <a name="customize-audio-format"></a>Ses biçimini özelleştirme

Aşağıdaki bölümde, aşağıdakiler de dahil olmak üzere ses çıktısı özniteliklerinin nasıl özelleştirilen gösterilmektedir:

* Ses dosyası türü
* Örnek oranı
* Bit derinliği

Ses biçimini değiştirmek için `speechSynthesisOutputFormat` `SpeechConfig` nesneüzerindeki özelliği kullanırsınız. Bu özellik, `enum` çıktı [`SpeechSynthesisOutputFormat`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisoutputformat?view=azure-node-latest)biçimini seçmek için kullandığınız bir tür bekler. Kullanılabilen ses biçimleri listesi için başvuru [dokümanlarına](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisoutputformat?view=azure-node-latest) bakın.

Gereksinimlerinize bağlı olarak farklı dosya türleri için çeşitli seçenekler vardır. Tanım olarak, gibi `Raw24Khz16BitMonoPcm` ham biçimlerin ses üstbilgilerini içermediğini unutmayın. Ham biçimleri yalnızca alt akış uygulamanızın ham bit akışını çözebileceğini bildiğinizde veya bit derinliğine, örnek hızına, kanal sayısına vb. göre el ile üstbilgi oluşturmayı planlıyorsanız kullanın.

Bu örnekte, `Riff24Khz16BitMonoPcm` `speechSynthesisOutputFormat` `SpeechConfig` nesne üzerinde ayarlayarak yüksek doğruluklu BIR RIFF biçimi belirtirsiniz. Önceki bölümdeki örneğe benzer şekilde, `ArrayBuffer` ses verilerini alın ve onunla etkileşime geçin.

```javascript
function synthesizeSpeech() {
    const speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

    // Set the output format
    speechConfig.speechSynthesisOutputFormat = SpeechSynthesisOutputFormat.Riff24Khz16BitMonoPcm;

    const synthesizer = new sdk.SpeechSynthesizer(speechConfig, undefined);
    synthesizer.speakTextAsync(
        "Customizing audio output format.",
        result => {
            // Interact with the audio ArrayBuffer data
            const audioData = result.audioData;
            console.log(`Audio data byte size: ${audioData.byteLength}.`)

            synthesizer.close();
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

Programınızı yeniden çalıştırmak, `.wav` belirtilen yola bir dosya yazar.

## <a name="use-ssml-to-customize-speech-characteristics"></a>Konuşma özelliklerini özelleştirmek için SSML'yi kullanın

Konuşma Sentezi Biçimlendirme Dili (SSML), bir XML şemasından isteklerinizi göndererek adım, telaffuz, konuşma hızı, ses düzeyi ve daha fazla metin-konuşma çıktısı ince ayar yapmanızı sağlar. Bu bölümde birkaç pratik kullanım örneği gösterilmektedir, ancak daha ayrıntılı bir kılavuz için [SSML nasıl yapılacağını makaleye](../../../speech-synthesis-markup.md)bakın.

Özelleştirme için SSML kullanmaya başlamak için, sesi değiştiren basit bir değişiklik yaparsınız.
İlk olarak, bu örnekte, `ssml.xml`kök proje dizininizde SSML config için yeni bir XML dosyası oluşturun. Kök öğe her `<speak>`zaman ve bir `<voice>` öğedeki metni sarma `name` param kullanarak sesi değiştirmenize olanak sağlar. Bu örnek, sesi erkek bir İngilizce (İngiltere) sesine değiştirir. Bu sesin, **nöral** seslere göre farklı fiyatlandırma ve kullanılabilirlik olan **standart** bir ses olduğunu unutmayın. Desteklenen **standart** seslerin [tam listesine](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#standard-voices) bakın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    When you're on the motorway, it's a good idea to use a sat-nav.
  </voice>
</speak>
```

Ardından, XML dosyanıza başvurmak için konuşma sentezi isteğini değiştirmeniz gerekir. İstek çoğunlukla aynıdır, ancak `speakTextAsync()` işlevi kullanmak yerine. `speakSsmlAsync()` Bu işlev bir XML dizesi bekler, bu nedenle önce bir XML dosyasını yüklemek ve dize olarak döndürmek için bir işlev oluşturursunuz.

```javascript
function xmlToString(filePath) {
    const xml = readFileSync(filePath, "utf8");
    return xml;
}
```

Daha fazla `readFileSync`bilgi için, <a href="https://nodejs.org/api/fs.html#fs_fs_readlinksync_path_options" target="_blank">Bkz. Node.js dosya sistemi.<span class="docon docon-navigate-external x-hidden-focus"></span></a> Buradan, sonuç nesnesi önceki örneklerle tam olarak aynıdır.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const synthesizer = new sdk.SpeechSynthesizer(speechConfig, undefined);

    const ssml = xmlToString("ssml.xml");
    synthesizer.speakSsmlAsync(
        ssml,
        result => {
            if (result.errorDetails) {
                console.error(result.errorDetails);
            } else {
                console.log(JSON.stringify(result));
            }

            synthesizer.close();
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

Çıktı çalışır, ancak daha doğal ses yardımcı olmak için yapabileceğiniz birkaç basit ek değişiklikler vardır. Genel konuşma hızı biraz fazla hızlı, bu yüzden `<prosody>` bir etiket ekleyeceğiz ve varsayılan hızın% **90** hızı azaltmak. Ayrıca, cümledeki virgülden sonra duraklama biraz fazla kısa ve doğal olmayan bir sondajdır. Bu sorunu gidermek için, konuşmayı geciktirmek için bir `<break>` etiket ekleyin ve zaman paramini **200ms**olarak ayarlayın. Bu özelleştirmelerin çıktıyı nasıl etkilediğini görmek için sentezi yeniden çalıştırın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    <prosody rate="0.9">
      When you're on the motorway,<break time="200ms"/> it's a good idea to use a sat-nav.
    </prosody>
  </voice>
</speak>
```

## <a name="neural-voices"></a>Sinirsel sesler

Sinirsel sesler derin sinir ağları tarafından desteklenen konuşma sentezi algoritmalarıdır. Sinirsel bir ses kullanırken, sentezlenmiş konuşma insan kayıtlarından neredeyse ayırt edilemez. İnsana benzer doğal prosody ve kelimelerin net eklemleme ile, nöral sesler önemli ölçüde kullanıcıların AI sistemleri ile etkileşim dinleme yorgunluğunu azaltır.

Nöral bir sese `name` geçmek için, [nöral ses seçeneklerinden](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices)birine değiştirin. Ardından, için `mstts`bir XML ad alanı ekleyin `<mstts:express-as>` ve metninizi etikete sarın. Konuşma `style` stilini özelleştirmek için param'ı kullanın. Bu örnek, `cheerful`ancak ayarlamayı `customerservice` `chat` veya konuşma stilindeki farkı görmeyi deneyin.

> [!IMPORTANT]
> Nöral sesler **sadece** *Doğu ABD,* *Güney Doğu Asya*ve Batı *Avrupa* bölgelerinde oluşturulan Konuşma kaynakları için desteklenir.

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
    xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="en-US-AriaNeural">
    <mstts:express-as style="cheerful">
      This is awesome!
    </mstts:express-as>
  </voice>
</speak>
```
