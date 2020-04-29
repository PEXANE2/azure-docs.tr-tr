---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/15/2020
ms.author: trbye
ms.openlocfilehash: d91082ae3c0ae5e501675a06b02e2f55f20ce236
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81399552"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz için önce <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">JavaScript konuşma SDK 'sını <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>yüklemeniz gerekir. Platformunuza bağlı olarak, aşağıdaki yönergeleri kullanın:
- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs#get-the-speech-sdk" target="_blank">Node. js<span 
class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=browser#get-the-speech-sdk" target="_blank">Web tarayıcısı<span class="docon docon-navigate-external x-hidden-focus"></span></a>

Ayrıca, hedef ortama bağlı olarak, aşağıdakilerden birini kullanın:

# <a name="import"></a>[aktarmaya](#tab/import)

```javascript
import { readFileSync } from "fs";
import {
    AudioConfig,
    SpeechConfig,
    SpeechSynthesisOutputFormat,
    SpeechSynthesizer 
} from "microsoft-cognitiveservices-speech-sdk";
```

Hakkında `import`daha fazla bilgi için bkz. <a href="https://javascript.info/import-export" target="_blank">dışa ve <span class="docon docon-navigate-external x-hidden-focus"> </span>içeri aktarma </a>.

# <a name="require"></a>[gerektirir](#tab/require)

```javascript
const readFileSync = require("fs").readFileSync;
const sdk = require("microsoft-cognitiveservices-speech-sdk");
```

Hakkında `require`daha fazla bilgi için bkz. <a href="https://nodejs.org/en/knowledge/getting-started/what-is-require/" target="_blank">ne gerekir? <span class="docon docon-navigate-external x-hidden-focus"></span></a>.


# <a name="script"></a>[SCRIPT](#tab/script)

<a href="https://aka.ms/csspeech/jsbrowserpackage" target="_blank">JavaScript konuşma SDK 'sı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> *Microsoft. biliveservices. Speech. SDK. demeti. js* dosyasını indirip ayıklayın ve HTML dosyanıza erişilebilen bir klasöre yerleştirin.

```html
<script src="microsoft.cognitiveservices.speech.sdk.bundle.js"></script>;
```

> [!TIP]
> Bir Web tarayıcısını hedefliyorsanız ve `<script>` etiketini kullanıyorsanız; `sdk` ön ek gerekli değildir. `sdk` Ön ek, `require` modülü adlandırmak için kullanılan bir diğer addır.

---

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)oluşturmanız gerekir. Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir.

> [!NOTE]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

[`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)Şunları başlatabilmeniz için birkaç yol vardır:

* Abonelik ile: bir anahtarı ve ilişkili bölgeyi geçirin.
* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

Bu örnekte, bir abonelik anahtarı ve [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest) bölgesi kullanarak bir oluşturun. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın. Ayrıca, bu makalenin geri kalanı için kullanabileceğiniz, farklı özelleştirmeler için değiştirdiğiniz bazı temel ortak kod oluşturabilirsiniz.

```javascript
function synthesizeSpeech() {
    const speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
}
```

## <a name="synthesize-speech-to-a-file"></a>Konuşmayı bir dosyaya sentezleştirme

Sonra, metin okuma dönüştürmeleri [`SpeechSynthesizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesizer?view=azure-node-latest) yürüten ve hoparlörlerde, dosyalarda veya diğer çıkış akışlarına giden çıktıları yürüten bir nesne oluşturun. , [`SpeechSynthesizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesizer?view=azure-node-latest) Önceki adımda oluşturulan [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest) nesneyi params olarak kabul eder ve çıkış sonuçlarının nasıl işleneceğini [`AudioConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig?view=azure-node-latest) belirten bir nesnedir.

Başlamak için, `AudioConfig` `.wav` `fromAudioFileOutput()` statik işlevi kullanarak çıktıyı bir dosyaya otomatik olarak yazmak üzere bir oluşturun.

```javascript
function synthesizeSpeech() {
    const speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const audioConfig = AudioConfig.fromAudioFileOutput("path/to/file.wav");
}
```

Ardından, `SpeechSynthesizer` `speechConfig` nesne ve `audioConfig` nesneyi params olarak geçirerek bir örnek oluşturun. Daha sonra, konuşma birleştirmenin yürütülmesi ve bir dosyaya yazılması, bir metin dizesiyle çalışırken `speakTextAsync()` basittir. Sonucun doğru bir şekilde çalışması için sonuç geri çağırması `synthesizer.close()`, aslında çağırmak için gereken harika bir yerdir.

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

Programı çalıştırın ve belirttiğiniz konuma bir sentezlenmiş `.wav` dosya yazılır. Bu, en temel kullanımın iyi bir örneğidir, ancak sonraki bir deyişle, özel senaryolarla çalışmaya yönelik bellek içi akış olarak çıktıyı özelleştirmeye ve çıkış yanıtını işlemeye baktığınızın ardından.

## <a name="synthesize-to-speaker-output"></a>Konuşmacı çıktısına sentezleştirme

Bazı durumlarda, doğrudan sentezlenmiş konuşmayı doğrudan bir konuşmacının çıktısını almak isteyebilirsiniz. Bunu yapmak için `AudioConfig` `fromDefaultSpeakerOutput()` static işlevini kullanarak örneğini oluşturun. Bu, geçerli etkin çıkış cihazına çıkış verir.

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

## <a name="get-result-as-an-in-memory-stream"></a>Bellek içi akış olarak sonuç alın

Konuşma uygulaması geliştirmede birçok senaryo için, büyük olasılıkla bir dosyaya doğrudan yazmak yerine, sonuçta elde edilen ses verilerinin bellek içi akış olarak olması gerekir. Bu, aşağıdakiler dahil olmak üzere özel davranış oluşturmanıza izin verir:

* Özel aşağı akış Hizmetleri için arama yapabilecek bir akış olarak ortaya çıkan bayt dizisinin soyut olması.
* Sonucu diğer API 'leri veya hizmetleriyle tümleştirin.
* Ses verilerini değiştirin, özel `.wav` üstbilgiler yazın vb.

Önceki örnekte bu değişikliği yapmak basittir. İlk olarak, daha `AudioConfig` fazla denetim için çıkış davranışını bu noktadan el ile yöneteceği için bloğu kaldırın. Sonra `SpeechSynthesizer` Oluşturucu `undefined` `AudioConfig` içinde öğesine geçirin. 

> [!NOTE]
> Yukarıdaki `undefined` konuşmacı çıktısı `AudioConfig`örneğinde olduğu gibi değil, için geçirme, geçerli etkin çıkış cihazında varsayılan olarak sesi oynamaz.

Bu kez, sonucu bir [`SpeechSynthesisResult`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisresult?view=azure-node-latest) değişkene kaydedersiniz. `SpeechSynthesisResult.audioData` Özelliği, çıkış verilerinden `ArrayBuffer` birini döndürür. Bu `ArrayBuffer` , el ile çalışabilirsiniz.

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

Buradan, sonuçta elde edilen `ArrayBuffer` nesneyi kullanarak özel davranışları uygulayabilirsiniz.

## <a name="customize-audio-format"></a>Ses biçimini Özelleştir

Aşağıdaki bölümde aşağıdakiler dahil olmak üzere ses çıkış özniteliklerinin nasıl özelleştirileceği gösterilmektedir:

* Ses dosyası türü
* Örnek hız
* Bit derinliği

Ses biçimini değiştirmek için `speechSynthesisOutputFormat` `SpeechConfig` nesnesi üzerinde özelliğini kullanın. Bu özellik, çıkış `enum` biçimini seçmek [`SpeechSynthesisOutputFormat`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisoutputformat?view=azure-node-latest)için kullandığınız türünde bir tür bekliyor. Kullanılabilir [Ses biçimlerinin listesi](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisoutputformat?view=azure-node-latest) için başvuru belgelerine bakın.

Gereksinimlerinize bağlı olarak farklı dosya türleri için çeşitli seçenekler vardır. Tanımına göre, gibi `Raw24Khz16BitMonoPcm` ham biçimlerin ses üst bilgilerini içermediğini unutmayın. Ham biçimleri yalnızca aşağı akış uygulamanızın bir ham Bitstream kodunu çözemediğini veya bit derinliğine, örnek hızına, kanal sayısına ve vb. göre el ile üstbilgileri oluşturmayı planlıyorsanız kullanın.

Bu örnekte, `Riff24Khz16BitMonoPcm` `speechSynthesisOutputFormat` `SpeechConfig` nesnesi üzerinde ayarını yaparak Yüksek uygunluğa sahip bir biçim belirtirsiniz. Önceki bölümdeki örneğe benzer şekilde, ses `ArrayBuffer` verilerini alın ve onunla etkileşime geçin.

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

Programınızı yeniden çalıştırmak, belirtilen yola bir `.wav` dosya yazacak.

## <a name="use-ssml-to-customize-speech-characteristics"></a>SSML 'yi kullanarak konuşma özelliklerini özelleştirme

Konuşma sen, biçimlendirme dili (SSML), isteklerinizi bir XML şemasından göndererek, metin okuma çıktılarından oluşan sıklık, Söyleniş, konuşma oranı, hacim ve daha fazlasını ayarlamanıza olanak sağlar. Bu bölümde, birkaç pratik kullanım örneği gösterilmektedir, ancak daha ayrıntılı bir kılavuz için bkz. [SSML nasıl yapılır makalesi](../../../speech-synthesis-markup.md).

Özelleştirme için SSML 'yi kullanmaya başlamak için, sese geçiş yapan basit bir değişiklik yaparsınız.
İlk olarak, bu örnekte `ssml.xml`, kök proje dizininizde SSML yapılandırması için yenı bir XML dosyası oluşturun. Kök öğesi her zaman `<speak>`ve metni bir `<voice>` öğe içinde sarmalama, `name` param kullanarak sesi değiştirmenize olanak sağlar. Bu örnek, sesi erkek Ingilizce (UK) sesine dönüştürür. Bu sesin, **sinir** sesinden farklı fiyatlandırmaya ve kullanılabilirliğine sahip **Standart** bir ses olduğunu unutmayın. Desteklenen **Standart** seslerin [tam listesine](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#standard-voices) bakın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    When you're on the motorway, it's a good idea to use a sat-nav.
  </voice>
</speak>
```

Ardından, XML dosyanıza başvurmak için konuşma sensıs isteğini değiştirmeniz gerekir. İstek genellikle aynıdır, ancak `speakTextAsync()` işlevini kullanmak yerine kullanın. `speakSsmlAsync()` Bu işlev bir XML dizesi bekliyor, bu nedenle ilk olarak bir XML dosyası yüklemek için bir işlev oluşturun ve bunu bir dize olarak döndürün.

```javascript
function xmlToString(filePath) {
    const xml = readFileSync(filePath, "utf8");
    return xml;
}
```

Hakkında `readFileSync`daha fazla bilgi için bkz. <a href="https://nodejs.org/api/fs.html#fs_fs_readlinksync_path_options" target="_blank">Node. js dosya<span class="docon docon-navigate-external x-hidden-focus"></span>sistemi</a>. Buradan, sonuç nesnesi önceki örneklerle tamamen aynıdır.

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

Çıktı işe yarar, ancak daha doğal bir şekilde ses duymayı sağlayacak birkaç basit ek değişiklik vardır. Genel konuşma hızı çok hızlıdır, bu nedenle bir etiket ekleyeceğiz ve varsayılan orandaki `<prosody>` **%90** ' a kadar olan hızı azaltabilirsiniz. Ayrıca, tümcede virgülden sonraki duraklama, çok kısa ve doğal olmayan bir soun. Bu sorunu onarmak için konuşmayı geciktirmek üzere `<break>` bir etiket ekleyin ve zaman param ' ı **200ms**olarak ayarlayın. Bu özelleştirmelerin çıktıyı nasıl etkilediğine bakmak için senmu yeniden çalıştırın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    <prosody rate="0.9">
      When you're on the motorway,<break time="200ms"/> it's a good idea to use a sat-nav.
    </prosody>
  </voice>
</speak>
```

## <a name="neural-voices"></a>Sinir sesleri

Sinir sesleri, derin sinir Networks tarafından desteklenen konuşma senime algoritmalardır. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

Bir sinir sesinize geçiş yapmak için, `name` [sinir Voice seçeneklerinden](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices)birini değiştirin. Ardından, için `mstts`bir XML ad alanı ekleyin ve metninizi `<mstts:express-as>` etikete sarın. Konuşma stilini `style` özelleştirmek için param 'ı kullanın. Bu örnek kullanır `cheerful`, ancak konuşma stilindeki farkı görmek `customerservice` için `chat` veya olarak ayarlamayı deneyin.

> [!IMPORTANT]
> Sinir sesleri **yalnızca** *Doğu ABD*, *Güney Doğu Asya*ve *Batı Avrupa* bölgelerinde oluşturulan konuşma kaynakları için desteklenir.

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
