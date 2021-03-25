---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/04/2021
ms.author: trbye
ms.custom: devx-track-js
ms.openlocfilehash: 7649f9c13db1a10bdb48d19f1019ec3b31244f45
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105105099"
---
Konuşma hizmetinin temel özelliklerinden biri de insan konuşmanızı tanıyabilme ve (genellikle konuşma-metin olarak adlandırılır). Bu hızlı başlangıçta, uygulama ve ürünlerinize yönelik konuşma SDK 'sını kullanarak yüksek kaliteli bir konuşmayı metne dönüştürme işlemini nasıl gerçekleştireceğinizi öğreneceksiniz.

## <a name="skip-to-samples-on-github"></a>GitHub 'da örneklere atlayın

Örnek koda doğrudan atlamak istiyorsanız GitHub 'da [JavaScript hızlı başlangıç örneklerine](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node) bakın.

Alternatif olarak, bir tarayıcı tabanlı ortamda konuşma SDK 'sını nasıl kullanacağınızı öğrenmek için [tepki verme örneğine](https://github.com/Azure-Samples/AzureSpeechReactSample) bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapmadan önce, Node.js için konuşma SDK 'sını yüklemeniz gerekir. Yalnızca paket adının yüklenmesini istiyorsanız, öğesini çalıştırın `npm install microsoft-cognitiveservices-speech-sdk` . Kılavuzlu yükleme yönergeleri için [Başlarken](../../../quickstarts/setup-platform.md?pivots=programming-language-javascript&tabs=dotnet%2clinux%2cjre%2cnodejs) makalesine bakın.

`require`SDK 'yı içeri aktarmak için aşağıdaki ifadeyi kullanın.

```javascript
const sdk = require("microsoft-cognitiveservices-speech-sdk");
```

Hakkında daha fazla bilgi için `require` bkz. [gerekli belgeler](https://nodejs.org/en/knowledge/getting-started/what-is-require/).

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir oluşturmanız gerekir [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) . Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir. [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig)Anahtarınızı ve bölgenizi kullanarak bir oluşturun. Anahtar-bölge çiftini bulmak için [anahtarlar ve bölge bulma](../../../overview.md#find-keys-and-region) sayfasına bakın.

```javascript
const speechConfig = sdk.SpeechConfig.fromSubscription("<paste-your-subscription-key>", "<paste-your-region>");
```

Şunları başlatabilmeniz için birkaç farklı yol vardır [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) :

* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

> [!NOTE]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

## <a name="recognize-from-microphone-browser-only"></a>Mikrofondan tanıma (yalnızca tarayıcı)

Konuşmayı bir mikrofondan tanıma **Node.jsdesteklenmez** ve yalnızca tarayıcı tabanlı bir JavaScript ortamında desteklenir. [Mikrofon uygulamasının konuşmadan metne](https://github.com/Azure-Samples/AzureSpeechReactSample/blob/main/src/App.js#L29)bakmak için GitHub 'daki [tepki verme örneğine](https://github.com/Azure-Samples/AzureSpeechReactSample) bakın.

> [!NOTE]
> *Belirli* bir ses giriş cihazını kullanmak istiyorsanız, IÇINDE cihaz kimliği belirtmeniz gerekir `AudioConfig` . Ses giriş cihazınız için [CIHAZ kimliğini nasıl alabileceğinizi](../../../how-to-select-audio-input-devices.md) öğrenin.

## <a name="recognize-from-file"></a>Dosyadan tanı 

Bir ses dosyasından konuşmayı tanımak için, `AudioConfig` `fromWavFileInput()` bir nesnesi kabul eden bir using oluşturun `Buffer` . Ardından, ve ' yi geçirerek bir başlatın [`SpeechRecognizer`](https://docs.microsoft.com/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer?view=azure-node-latest) `audioConfig` `speechConfig` .

```javascript
const fs = require('fs');
const sdk = require("microsoft-cognitiveservices-speech-sdk");
const speechConfig = sdk.SpeechConfig.fromSubscription("<paste-your-subscription-key>", "<paste-your-region>");

function fromFile() {
    let audioConfig = sdk.AudioConfig.fromWavFileInput(fs.readFileSync("YourAudioFile.wav"));
    let recognizer = new sdk.SpeechRecognizer(speechConfig, audioConfig);

    recognizer.recognizeOnceAsync(result => {
        console.log(`RECOGNIZED: Text=${result.text}`);
        recognizer.close();
    });
}
fromFile();
```

## <a name="recognize-from-in-memory-stream"></a>Bellek içi akıştan tanıma

Birçok kullanım durumu için, ses veriniz blob depolamadan geliyor olabilir, aksi takdirde [`ArrayBuffer`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) veya benzer bir ham veri yapısı olarak zaten bellek içi olabilir. Aşağıdaki kod:

* Kullanarak bir anında iletme akışı oluşturur `createPushStream()` .
* `.wav`Tanıtım amacıyla kullanarak bir dosya okur `fs.createReadStream` , ancak zaten bir içinde ses veriniz varsa `ArrayBuffer` , içeriği giriş akışına yazmak için doğrudan atlayabilirsiniz.
* Gönderim akışını kullanarak bir ses yapılandırması oluşturur.

```javascript
const fs = require('fs');
const sdk = require("microsoft-cognitiveservices-speech-sdk");
const speechConfig = sdk.SpeechConfig.fromSubscription("<paste-your-subscription-key>", "<paste-your-region>");

function fromStream() {
    let pushStream = sdk.AudioInputStream.createPushStream();

    fs.createReadStream("YourAudioFile.wav").on('data', function(arrayBuffer) {
        pushStream.write(arrayBuffer.slice());
    }).on('end', function() {
        pushStream.close();
    });
 
    let audioConfig = sdk.AudioConfig.fromStreamInput(pushStream);
    let recognizer = new sdk.SpeechRecognizer(speechConfig, audioConfig);
    recognizer.recognizeOnceAsync(result => {
        console.log(`RECOGNIZED: Text=${result.text}`);
        recognizer.close();
    });
}
fromStream();
```

Giriş olarak bir gönderim akışı kullanmak, ses verilerinin ham PCM olduğunu varsayar, ör. herhangi bir üst bilgi atlanıyor.
Üst bilgi atlanmadığında, API bazı durumlarda çalışmaya devam eder, ancak en iyi sonuçlar için, `fs` *ses verilerinin başlangıcında* başlayacak şekilde üstbilgileri okumak için mantığı uygulamayı düşünün.

## <a name="error-handling"></a>Hata işleme

Önceki örneklerde, ' den tanınan metin alınır `result.text` , ancak hataları ve diğer yanıtları işlemek için, sonucu işlemek üzere bazı kodlar yazmanız gerekir. Aşağıdaki kod, özelliğini değerlendirir [`result.reason`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognitionresult#reason) ve:

* Tanınma sonucunu yazdırır: `ResultReason.RecognizedSpeech`
* Bir tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin: `ResultReason.NoMatch`
* Bir hata ile karşılaşırsanız, hata iletisini yazdırın: `ResultReason.Canceled`

```javascript
switch (result.reason) {
    case ResultReason.RecognizedSpeech:
        console.log(`RECOGNIZED: Text=${result.text}`);
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
```

## <a name="continuous-recognition"></a>Sürekli tanıma

Önceki örneklerde tek bir söylik tanınabilmesi için tek kararlı tanıma kullanılır. Tek bir utterüance 'in sonunda, sonda sessizlik dinlemesi veya en fazla 15 saniyelik ses işlenene kadar belirlenir.

Bunun aksine, sürekli tanıma, tanımanın ne zaman durdurulacağını **denetlemek** istediğinizde kullanılır. `Recognizing` `Recognized` Tanıma sonuçlarını almak için,, ve olaylarına abone olmanızı gerektirir `Canceled` . Tanımayı durdurmak için çağrısı yapmanız gerekir [`stopContinuousRecognitionAsync`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#stopcontinuousrecognitionasync) . İşte, bir ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirilebileceğini gösteren bir örnek.

Girişi tanımlayarak ve şunu başlatarak başlayın [`SpeechRecognizer`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer) :

```javascript
const recognizer = new sdk.SpeechRecognizer(speechConfig, audioConfig);
```

Sonra, öğesinden gönderilen olaylara abone olun [`SpeechRecognizer`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer) .

* [`recognizing`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#recognizing): Ara tanıma sonuçları içeren olaylar için sinyal.
* [`recognized`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#recognized): Son tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi olduğunu gösterir).
* [`sessionStopped`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#sessionstopped): Bir tanıma oturumunun (işlem) sonunu gösteren olaylar için sinyal.
* [`canceled`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#canceled): İptal edilen tanıma sonuçlarını içeren olaylar için sinyal (sonuç veya doğrudan iptal isteği olarak iptal edilen bir tanıma girişimi veya ya da bir aktarım ya da protokol arızası).

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

Her şey ayarlandığında, tanımayı Başlat ' ı çağırın [`startContinuousRecognitionAsync`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#startcontinuousrecognitionasync) .

```javascript
recognizer.startContinuousRecognitionAsync();

// make the following call at some point to stop recognition.
// recognizer.StopContinuousRecognitionAsync();
```

### <a name="dictation-mode"></a>Dikte etme modu

Sürekli tanıma kullanırken, ilgili "dikte etmeyi etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, konuşma yapılandırma örneğinin noktalama gibi tümce yapılarının sözcük açıklamalarını yorumlamasını sağlar. Örneğin, "kasadaki gerçek zamanlı olarak", "kasadaki canlı mısınız?" metni olarak yorumlanabilir.

Dikte modunu etkinleştirmek için, içindeki [`enableDictation`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig#enabledictation--) yöntemi kullanın [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) .

```javascript
speechConfig.enableDictation();
```

## <a name="change-source-language"></a>Kaynak dilini değiştir

Konuşma tanıma için ortak bir görev, giriş (veya kaynak) dilini belirtmektir. Giriş dilini Italyanca olarak nasıl değiştirebileceğinizi göz atalım. Kodunuzda, [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) daha sonra bu satırı hemen altına ekleyin.

```javascript
speechConfig.speechRecognitionLanguage = "it-IT";
```

[`speechRecognitionLanguage`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig#speechrecognitionlanguage)Özelliği, bir dil yerel ayar dizesi bekliyor. Desteklenen [yerel ayarlar/diller](../../../language-support.md)listesindeki **yerel ayar** sütununda herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu geliştirme

Tümcecik listeleri, bir kişinin adı veya belirli bir konum gibi, ses verilerinde bilinen tümcecikleri belirlemek için kullanılır. Deyimlerin bir listesini sağlayarak konuşma tanımanın doğruluğunu geliştirerek.

Örnek olarak, "taşı" komutuna ve söylenen "Ward" bir hedefe sahipseniz, "Ward 'e taşı" girişini ekleyebilirsiniz. Bir tümcecik eklemek, ses "ilerlemek için taşı" yerine "ilerlemek" yerine tanınabilmesi olasılığını artırır.

Tek sözcükler veya bütün ifadeler, bir tümcecik listesine eklenebilir. Tanıma sırasında, bir ifade listesindeki bir giriş, girdiler utterance 'in ortasında görünse bile, listedeki sözcüklerin ve deyimlerin tanınmasını artırmak için kullanılır. 

> [!IMPORTANT]
> Tümcecik listesi özelliği şu dillerde kullanılabilir: en-US, de-DE, en-AU, en-CA, en-GB, ES-ES, es-MX, fr-CA, fr-FR, It-IT, ja-JP, Ko

Bir tümcecik listesi kullanmak için, önce bir [`PhraseListGrammar`](/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar) nesne oluşturun, ardından ile belirli sözcükler ve deyimler ekleyin [`addPhrase`](/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar#addphrase-string-) .

Herhangi bir değişiklik [`PhraseListGrammar`](/javascript/api/microsoft-cognitiveservices-speech-sdk/phraselistgrammar) , bir sonraki tanıma göre veya konuşma hizmetine yeniden bağlanmaya sonra devreye girer.

```javascript
const phraseList = sdk.PhraseListGrammar.fromRecognizer(recognizer);
phraseList.addPhrase("Supercalifragilisticexpialidocious");
```

Tümcecik listenizi temizlemeniz gerekirse:

```javascript
phraseList.clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../custom-speech-overview.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)