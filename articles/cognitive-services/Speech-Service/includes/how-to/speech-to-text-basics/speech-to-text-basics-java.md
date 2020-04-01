---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: dapine
ms.openlocfilehash: c2b474c9d6485bdba31412435f4edbdd2383b3c0
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80501634"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir Azure hesabınız ve Konuşma hizmeti aboneliğiniz olduğu varsayar. Hesabınız ve aboneliğiniz yoksa, [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Herhangi bir şey yapmadan önce Konuşma SDK'sını yüklemeniz gerekir. Platformunuza bağlı olarak aşağıdaki yönergeleri kullanın:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=jre&pivots=programming-language-java" target="_blank">Java Çalışma Süresi<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=android&pivots=programming-language-java" target="_blank">Android<span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK'sını kullanarak Konuşma hizmetini aramak için [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)bir . Bu sınıf, anahtarınız ve ilişkili bölgeniz, bitiş noktanız, ana bilgisayar veya yetkilendirme belirteciniz gibi aboneliğiniz hakkında bilgiler içerir.

> [!NOTE]
> Konuşma tanıma, konuşma sentezi, çeviri veya niyet tanıma gerçekleştirin, her zaman bir yapılandırma oluşturursunuz.

Bir baş harfe başlatmanın birkaç [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)yolu vardır:

* Abonelikle: bir anahtar ve ilişkili bölgede geçirin.
* Bir bitiş noktası ile: Konuşma hizmeti bitiş noktası geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir ana bilgisayar adresi geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci yle: yetkilendirme jetonundan ve ilişkili bölgeye geçin.

Bir anahtar ve bölge kullanılarak [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable) nasıl oluşturulduğuna bir göz atalım. Bölge tanımlayıcınızı bulmak için [bölge destek](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```java
SpeechConfig config = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Tanıyıcıyı başlatma

Bir , bir [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)sonraki adım oluşturduktan sonra [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)bir . Bir [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)harfini başharflediğinizde, bunu geçirmeniz `config`gerekir. Bu, konuşma hizmetinin isteğinizi doğrulamak için gerektirdiği kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu kullanarak konuşmayı fark ediyorsanız, aşağıdaki [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable) gibi görünmelidir:

```java
SpeechRecognizer recognizer = new SpeechRecognizer(config);
```

Ses giriş aygıtını belirtmek istiyorsanız, ''ınızı ilke [`AudioConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-java-stable) olarak alırken `audioConfig` bir parametre [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)oluşturmanız ve parametreyi sağlamanız gerekir.

> [!TIP]
> [Ses giriş aygıtınız için aygıt kimliğini nasıl alacağınızı öğrenin.](../../../how-to-select-audio-input-devices.md)

İlk olarak, `import` aşağıdaki ifadeleri ekleyin.

```java
import java.util.concurrent.Future;
import com.microsoft.cognitiveservices.speech.*;
```

Ardından, `AudioConfig` nesneye aşağıdaki gibi başvuruda bulunabilirsiniz:

```java
AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de `audioConfig`bir . Ancak, `fromDefaultMicrophoneInput`aramak yerine [`AudioConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-java-stable)bir , parametre oluşturduğunuzda, `filename` arar `fromWavFileOutput` ve parametregeçersiniz.

```java
AudioConfig audioConfig = AudioConfig.fromWavFileInput("YourAudioFile.wav");
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

## <a name="recognize-speech"></a>Konuşma tanıma

Java için Konuşma SDK'sının [Recognizer sınıfı,](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable) konuşma tanıma için kullanabileceğiniz birkaç yöntemi ortaya çıkarır.

* Tek çekim tanıma (async) - Engellemesiz (asynchronous) modda tanıma gerçekleştirir. Bu tek bir söyleyiş tanıyacaktır. Tek bir söyleyninin sonu, sonunda sessizlik dinleyerek veya en fazla 15 saniyelik ses işlenene kadar belirlenir.
* Sürekli tanıma (async) - Asynchronously sürekli tanıma işlemi başlatır. Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de bir ses dosyası sağlamanız gerekir. Asynchronous sürekli tanımadurdurmak için [stopContinuousRecognitionAsync](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync)arayın.

> [!NOTE]
> [Konuşma tanıma modunu](../../../how-to-choose-recognition-mode.md)nasıl seçeceğiniz hakkında daha fazla bilgi edinin.

### <a name="single-shot-recognition"></a>Tek çekim tanıma

Burada kullanarak [`recognizeOnceAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-java-stable)eşzamanlı tek çekim tanıma bir örnek:

```java
Future<SpeechRecognitionResult> task = recognizer.recognizeOnceAsync();
SpeechRecognitionResult result = task.get();
```

Sonucu işlemek için bazı kod yazmanız gerekir. Bu örnek şu [`result.getReason()`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.resultreason?view=azure-java-stable)şekilde değerlendirilir:

* Tanıma sonucunu yazdırır:`ResultReason.RecognizedSpeech`
* Tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin:`ResultReason.NoMatch`
* Bir hatayla karşılaşılırsa, hata iletisini yazdırın:`ResultReason.Canceled`

```java
switch (result.getReason()) {
    case ResultReason.RecognizedSpeech:
        System.out.println("We recognized: " + result.getText());
        exitCode = 0;
        break;
    case ResultReason.NoMatch:
        System.out.println("NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled: {
            CancellationDetails cancellation = CancellationDetails.fromResult(result);
            System.out.println("CANCELED: Reason=" + cancellation.getReason());

            if (cancellation.getReason() == CancellationReason.Error) {
                System.out.println("CANCELED: ErrorCode=" + cancellation.getErrorCode());
                System.out.println("CANCELED: ErrorDetails=" + cancellation.getErrorDetails());
                System.out.println("CANCELED: Did you update the subscription info?");
            }
        }
        break;
}
```

### <a name="continuous-recognition"></a>Sürekli tanıma

Sürekli tanıma, tek çekimtanımadan biraz daha fazla ilgilidir. Tanıma sonuçlarını almak için `recognizing` `recognized`, `canceled` ve olaylara abone olmak için gerekli. Tanınmayı durdurmak [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync)için. Aşağıda, ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirildiğine ilişkin bir örnek verilmiştir.

Girişi tanımlayarak ve bir [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable)baş harfe başlayalım:

```java
AudioConfig audioConfig = AudioConfig.fromWavFileInput("YourAudioFile.wav");
SpeechRecognizer recognizer = new SpeechRecognizer(config, audioConfig);
```

Sonra, konuşma tanıma durumunu yönetmek için bir değişken oluşturalım. Başlangıç olarak, sınıf kapsamı `Semaphore` bir bildiririz.

```java
private static Semaphore stopTranslationWithFileSemaphore;
```

Biz gönderilen olaylara abone olacağız [`SpeechRecognizer`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer?view=azure-java-stable).

* [`recognizing`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognizing?view=azure-java-stable): Ara tanıma sonuçlarını içeren olaylar için sinyal.
* [`recognized`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.recognized?view=azure-java-stable): Nihai tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi gösteren).
* [`sessionStopped`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.recognizer.sessionstopped?view=azure-java-stable): Tanıma oturumunun (operasyonun) sona erini gösteren olaylar için sinyal.
* [`canceled`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.canceled?view=azure-java-stable): İptal edilmiş tanıma sonuçlarını içeren olaylar için sinyal (sonuç olarak iptal edilen bir tanıma girişimi veya doğrudan iptal isteği veya alternatif olarak bir aktarım veya protokol hatası) gösterir.

```java
// First initialize the semaphore.
stopTranslationWithFileSemaphore = new Semaphore(0);

recognizer.recognizing.addEventListener((s, e) -> {
    System.out.println("RECOGNIZING: Text=" + e.getResult().getText());
});

recognizer.recognized.addEventListener((s, e) -> {
    if (e.getResult().getReason() == ResultReason.RecognizedSpeech) {
        System.out.println("RECOGNIZED: Text=" + e.getResult().getText());
    }
    else if (e.getResult().getReason() == ResultReason.NoMatch) {
        System.out.println("NOMATCH: Speech could not be recognized.");
    }
});

recognizer.canceled.addEventListener((s, e) -> {
    System.out.println("CANCELED: Reason=" + e.getReason());

    if (e.getReason() == CancellationReason.Error) {
        System.out.println("CANCELED: ErrorCode=" + e.getErrorCode());
        System.out.println("CANCELED: ErrorDetails=" + e.getErrorDetails());
        System.out.println("CANCELED: Did you update the subscription info?");
    }

    stopTranslationWithFileSemaphore.release();
});

recognizer.sessionStopped.addEventListener((s, e) -> {
    System.out.println("\n    Session stopped event.");
    stopTranslationWithFileSemaphore.release();
});
```

Her şey ayarlandırıtamam, arayabiliyoruz. [`stopContinuousRecognitionAsync`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync)

```java
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
recognizer.startContinuousRecognitionAsync().get();

// Waits for completion.
stopTranslationWithFileSemaphore.acquire();

// Stops recognition.
recognizer.stopContinuousRecognitionAsync().get();
```

### <a name="dictation-mode"></a>Dikte modu

Sürekli tanıma kullanırken, ilgili "dikte işlemini etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, noktalama işaretleri gibi tümce yapılarının sözcük açıklamalarını yorumlamak için konuşma config örneğine neden olur. Örneğin, "Şehirde mi yaşıyorsunuz soru işareti" sözü "Şehirde mi yaşıyorsunuz?" şeklinde yorumlanır.

Dikte modunu etkinleştirmek için, [`enableDictation`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-java-stable) yöntemi [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)nikullanarak.

```java
config.enableDictation();
```

## <a name="change-source-language"></a>Kaynak dilini değiştirme

Konuşma tanıma için ortak bir görev giriş (veya kaynak) dilini belirtmektir. Giriş dilini Fransızca'ya nasıl değiştireceğinize bir göz atalım. Kodunuzda, [`SpeechConfig`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig?view=azure-java-stable)bu satırı doğrudan altına ekleyin.

```java
config.setSpeechRecognitionLanguage("fr-FR");
```

[`setSpeechRecognitionLanguage`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.setspeechrecognitionlanguage?view=azure-java-stable)bir bağımsız değişken olarak bir dize alan bir parametredir. Desteklenen [yerel halk/dil](../../../language-support.md)listesinde herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu artırın

Konuşma SDK ile tanıma doğruluğunu artırmanın birkaç yolu vardır. İfade Listeleri'ne bir göz atalım. Tümcecik Listeleri, ses verilerindeki bilinen ifadeleri tanımlamak için kullanılır, örneğin bir kişinin adı veya belirli bir konum. Tümcecik Listesine tek sözcük veya tam tümcecik eklenebilir. Tanıma sırasında, sese tüm tümcecik için tam bir eşleşme varsa, bir ifade listesindeki bir giriş kullanılır. İfadeyle tam eşleşme bulunamazsa, tanıma yardımcı olmaz.

> [!IMPORTANT]
> İfade Listesi özelliği yalnızca İngilizce olarak kullanılabilir.

Bir tümcecik listesini kullanmak [`PhraseListGrammar`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-java-stable) için önce bir nesne oluşturun, sonra [`AddPhrase`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar.addphrase?view=azure-java-stable#com_microsoft_cognitiveservices_speech_PhraseListGrammar_addPhrase_String_)belirli sözcükler ve tümcecikler ekleyin.

Bir sonraki [`PhraseListGrammar`](https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-java-stable) tanıma da veya Konuşma hizmetine yeniden bağlantı dan sonra etkili olacak değişiklikler.

```java
PhraseListGrammar phraseList = PhraseListGrammar.fromRecognizer(recognizer);
phraseList.addPhrase("Supercalifragilisticexpialidocious");
```

İfade listenizi temizlemeniz gerekiyorsa: 

```java
phraseList.clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../how-to-custom-speech.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)