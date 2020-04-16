---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/15/2020
ms.author: trbye
ms.openlocfilehash: b11194640c4d049c90f85974022908dce6b4fd79
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399761"
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
import {
    AudioConfig,
    CancellationDetails,
    CancellationReason,
    PhraseListGrammar,
    ResultReason,
    SpeechConfig,
    SpeechRecognizer
} from "microsoft-cognitiveservices-speech-sdk";
```

Daha fazla `import`bilgi için, <a href="https://javascript.info/import-export" target="_blank">bkz. <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>

# <a name="require"></a>[Gerektirir](#tab/require)

```javascript
const sdk = require("microsoft-cognitiveservices-speech-sdk");
```

Daha fazla `require`bilgi için, <a href="https://nodejs.org/en/knowledge/getting-started/what-is-require/" target="_blank">ne gerektirdiğini görmek? <span class="docon docon-navigate-external x-hidden-focus"></span></a>.


# <a name="script"></a>[Komut dosyası](#tab/script)

<a href="https://aka.ms/csspeech/jsbrowserpackage" target="_blank">JavaScript Speech <span class="docon docon-navigate-external x-hidden-focus"></span> SDK</a> *microsoft.cognitiveservices.speech.bundle.js* dosyasını indirin ve ayıklayın ve HTML dosyanıza erişilebilen bir klasöre yerleştirin.

```html
<script src="microsoft.cognitiveservices.speech.bundle.js"></script>;
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

Bir anahtar ve bölge kullanılarak [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest) nasıl oluşturulduğuna bir göz atalım. Bölge tanımlayıcınızı bulmak için [bölge destek](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```javascript
const speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Tanıyıcıyı başlatma

Bir , bir [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)sonraki adım oluşturduktan sonra [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest)bir . Bir [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest)harfini başharflediğinizde, bunu geçirmeniz `speechConfig`gerekir. Bu, konuşma hizmetinin isteğinizi doğrulamak için gerektirdiği kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu kullanarak konuşmayı fark ediyorsanız, aşağıdaki [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest) gibi görünmelidir:

```javascript
const recognizer = new SpeechRecognizer(speechConfig);
```

Ses giriş aygıtını belirtmek istiyorsanız, ''ınızı ilke [`AudioConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig?view=azure-node-latest) olarak alırken `audioConfig` bir parametre [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest)oluşturmanız ve parametreyi sağlamanız gerekir.

> [!TIP]
> [Ses giriş aygıtınız için aygıt kimliğini nasıl alacağınızı öğrenin.](../../../how-to-select-audio-input-devices.md)

Nesneyi `AudioConfig` aşağıdaki gibi başvurun:

```javascript
const audioConfig = AudioConfig.fromDefaultMicrophoneInput();
const speechConfig = SpeechConfig.fromSubscription(speechConfig, audioConfig);
```

Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de `audioConfig`bir . Ancak, bu yalnızca **Düğüm.js'yi** hedef alırken ve [`AudioConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig?view=azure-node-latest)aramak `fromDefaultMicrophoneInput`yerine parametreyi arar `fromWavFileOutput` ve `filename` geçersiniz.

```javascript
const audioConfig = AudioConfig.fromWavFileInput("YourAudioFile.wav");
const speechConfig = SpeechConfig.fromSubscription(speechConfig, audioConfig);
```

## <a name="recognize-speech"></a>Konuşma tanıma

C# için Konuşma SDK'sının [Recognizer sınıfı,](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest) konuşma tanıma için kullanabileceğiniz birkaç yöntemi ortaya çıkarır.

* Tek çekim tanıma (async) - Engellemesiz (asynchronous) modda tanıma gerçekleştirir. Bu tek bir söyleyiş tanıyacaktır. Tek bir söyleyninin sonu, sonunda sessizlik dinleyerek veya en fazla 15 saniyelik ses işlenene kadar belirlenir.
* Sürekli tanıma (async) - Asynchronously sürekli tanıma işlemi başlatır. Kullanıcı olaylara kaydolur ve çeşitli uygulama durumunu işler. Eşzamanlı sürekli tanımayı durdurmak için. [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#stopcontinuousrecognitionasync)

> [!NOTE]
> [Konuşma tanıma modunu](../../../how-to-choose-recognition-mode.md)nasıl seçeceğiniz hakkında daha fazla bilgi edinin.

### <a name="single-shot-recognition"></a>Tek çekim tanıma

Burada kullanarak [`recognizeOnceAsync`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#recognizeonceasync)eşzamanlı tek çekim tanıma bir örnek:

```javascript
recognizer.recognizeOnceAsync(result => {
    // Interact with result
});
```

Sonucu işlemek için bazı kod yazmanız gerekir. Bu örnek şu [`result.reason`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognitionresult?view=azure-node-latest#reason)şekilde değerlendirilir:

* Tanıma sonucunu yazdırır:`ResultReason.RecognizedSpeech`
* Tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin:`ResultReason.NoMatch`
* Bir hatayla karşılaşılırsa, hata iletisini yazdırın:`ResultReason.Canceled`

```javascript
switch (result.reason) {
    case ResultReason.RecognizedSpeech:
        console.log(`RECOGNIZED: Text=${result.text}`);
        console.log("    Intent not recognized.");
        break;
    case ResultReason.NoMatch:
        console.log("NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled:
        const cancellation = CancellationDetails.fromResult(result);
        console.log(`CANCELED: Reason=${cancellation.reason}`);

        if (cancellation.reason == CancellationReason.Error) {
            console.log(`CANCELED: ErrorCode=${cancellation.ErrorCode}`);
            console.log(`CANCELED: ErrorDetails=${cancellation.errorDetails}`);
            console.log("CANCELED: Did you update the subscription info?");
        }
        break;
    }
}
```

### <a name="continuous-recognition"></a>Sürekli tanıma

Sürekli tanıma, tek çekimtanımadan biraz daha fazla ilgilidir. Tanıma sonuçlarını almak için `Recognizing` `Recognized`, `Canceled` ve olaylara abone olmak için gerekli. Tanınmayı durdurmak [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#stopcontinuousrecognitionasync)için. Aşağıda, ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirildiğine ilişkin bir örnek verilmiştir.

Girişi tanımlayarak ve bir [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest)baş harfe başlayalım:

```javascript
const recognizer = new SpeechRecognizer(speechConfig);
```

Biz gönderilen olaylara abone olacağız [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest).

* [`recognizing`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#recognizing): Ara tanıma sonuçlarını içeren olaylar için sinyal.
* [`recognized`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#recognized): Nihai tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi gösteren).
* [`sessionStopped`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#sessionstopped): Tanıma oturumunun (operasyonun) sona erini gösteren olaylar için sinyal.
* [`canceled`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#canceled): İptal edilmiş tanıma sonuçlarını içeren olaylar için sinyal (sonuç olarak iptal edilen bir tanıma girişimi veya doğrudan iptal isteği veya alternatif olarak bir aktarım veya protokol hatası) gösterir.

```javascript
recognizer.recognizing = (s, e) => {
    console.log(`RECOGNIZING: Text=${e.result.text}`);
};

recognizer.recognized = (s, e) => {
    if (e.result.reason == ResultReason.RecognizedSpeech) {
        console.log(`RECOGNIZED: Text=${e.result.text}`);
    }
    else if (e.result.reason == ResultReason.NoMatch) {
        console.log("NOMATCH: Speech could not be recognized.");
    }
};

recognizer.canceled = (s, e) => {
    console.log(`CANCELED: Reason=${e.reason}`);

    if (e.reason == CancellationReason.Error) {
        console.log(`"CANCELED: ErrorCode=${e.errorCode}`);
        console.log(`"CANCELED: ErrorDetails=${e.errorDetails}`);
        console.log("CANCELED: Did you update the subscription info?");
    }

    recognizer.stopContinuousRecognitionAsync();
};

recognizer.sessionStopped = (s, e) => {
    console.log("\n    Session stopped event.");
    recognizer.stopContinuousRecognitionAsync();
};
```

Her şey ayarlandırıtamam, arayabiliyoruz. [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest#stopcontinuousrecognitionasync)

```javascript
// Starts continuous recognition. Uses stopContinuousRecognitionAsync() to stop recognition.
recognizer.startContinuousRecognitionAsync();

// Something later can call, stops recognition.
// recognizer.StopContinuousRecognitionAsync();
```

### <a name="dictation-mode"></a>Dikte modu

Sürekli tanıma kullanırken, ilgili "dikte işlemini etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, noktalama işaretleri gibi tümce yapılarının sözcük açıklamalarını yorumlamak için konuşma config örneğine neden olur. Örneğin, "Şehirde mi yaşıyorsunuz soru işareti" sözü "Şehirde mi yaşıyorsunuz?" şeklinde yorumlanır.

Dikte modunu etkinleştirmek için, [`enableDictation`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#enabledictation--) yöntemi [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)nikullanarak.

```javascript
speechConfig.enableDictation();
```

## <a name="change-source-language"></a>Kaynak dilini değiştirme

Konuşma tanıma için ortak bir görev giriş (veya kaynak) dilini belirtmektir. Giriş dilini İtalyanca'ya nasıl değiştireceğinize bir göz atalım. Kodunuzda, [`SpeechConfig`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest)bu satırı doğrudan altına ekleyin.

```javascript
speechConfig.speechRecognitionLanguage = "it-IT";
```

Özellik [`speechRecognitionLanguage`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig?view=azure-node-latest#speechrecognitionlanguage) bir dil-yerel biçim dizesi bekliyor. Desteklenen [yerel halk/dil](../../../language-support.md)listesindeki **Yerel sütundaki** herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu artırın

Konuşma ile tanıma doğruluğunu artırmanın birkaç yolu vardır İfade Listeleri'ne bir göz atalım. Tümcecik Listeleri, ses verilerindeki bilinen ifadeleri tanımlamak için kullanılır, örneğin bir kişinin adı veya belirli bir konum. Tümcecik Listesine tek sözcük veya tam tümcecik eklenebilir. Tanıma sırasında, sese tüm tümcecik için tam bir eşleşme varsa, bir ifade listesindeki bir giriş kullanılır. İfadeyle tam eşleşme bulunamazsa, tanıma yardımcı olmaz.

> [!IMPORTANT]
> İfade Listesi özelliği yalnızca İngilizce olarak kullanılabilir.

Bir tümcecik listesini kullanmak [`PhraseListGrammar`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar?view=azure-node-latest) için önce bir nesne oluşturun, sonra [`addPhrase`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar?view=azure-node-latest#addphrase-string-)belirli sözcükler ve tümcecikler ekleyin.

Bir sonraki [`PhraseListGrammar`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar?view=azure-node-latest) tanıma da veya Konuşma hizmetine yeniden bağlantı dan sonra etkili olacak değişiklikler.

```javascript
const phraseList = PhraseListGrammar.fromRecognizer(recognizer);
phraseList.addPhrase("Supercalifragilisticexpialidocious");
```

İfade listenizi temizlemeniz gerekiyorsa:

```javascript
phraseList.clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../how-to-custom-speech.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)