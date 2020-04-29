---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: trbye
ms.openlocfilehash: e39486f0daf1c865de3e53372d32a44fdcb8eb99
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81399730"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir. Platformunuza bağlı olarak, aşağıdaki yönergeleri kullanın:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=linux&pivots=programming-language-cpp" target="_blank">'Un<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=macos&pivots=programming-language-cpp" target="_blank">macOS<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=windows&pivots=programming-language-cpp" target="_blank">Pencerelerin<span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)oluşturmanız gerekir. Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir.

> [!NOTE]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

[`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)Şunları başlatabilmeniz için birkaç yol vardır:

* Abonelik ile: bir anahtarı ve ilişkili bölgeyi geçirin.
* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

Bir [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig) anahtar ve bölge kullanarak nasıl oluşturulduğuna göz atalım. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```cpp
auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Tanıyıcı başlatma

Bir [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)oluşturduktan sonra, bir sonraki adım bir [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer)' ı başlatmaktır. Bir [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer)başlattığınızda, bunu geçirmeniz gerekir `speech_config`. Bu, konuşma hizmetinin isteğinizi doğrulamak için ihtiyaç duyduğu kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer) kullanarak konuşmayı tanıyor olmanız halinde şöyle görünmelidir:

```cpp
auto recognizer = SpeechRecognizer::FromConfig(config);
```

Ses giriş cihazını belirtmek isterseniz, oluşturmanız [`AudioConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/audio-audioconfig) ve ' ı başlatırken `audioConfig` parametresini sağlamanız gerekir. [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer)

> [!TIP]
> [Ses giriş cihazınız için CIHAZ kimliğini nasıl alabileceğinizi öğrenin](../../../how-to-select-audio-input-devices.md).

İlk olarak, tanımlarınızın `using namespace` `#include` ardından aşağıdaki ifadeyi ekleyin.

```cpp
using namespace Microsoft::CognitiveServices::Speech::Audio;
```

Daha sonra, `AudioConfig` nesnesine aşağıdaki gibi başvurabilirsiniz:

```cpp
auto audioConfig = AudioConfig::FromDefaultMicrophoneInput();
auto recognizer = SpeechRecognizer::FromConfig(config, audioConfig);
```

Mikrofon kullanmak yerine bir ses dosyası sağlamak istiyorsanız, yine de sağlamanız gerekir `audioConfig`. Ancak [`AudioConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/audio-audioconfig), öğesini çağırmak `FromDefaultMicrophoneInput`yerine bir oluşturduğunuzda, `FromWavFileOutput` `filename` parametresini çağırır ve geçireceğiz.

```cpp
auto audioInput = AudioConfig::FromWavFileInput("YourAudioFile.wav");
auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);
```

## <a name="recognize-speech"></a>Konuşma tanıma

C++ için konuşma SDK 'Sı için [tanıyıcı sınıfı](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer) , konuşma tanıma için kullanabileceğiniz birkaç yöntem sunar.

* Tek atışı tanıma (Async)-engelleyici olmayan (zaman uyumsuz) modda tanıma gerçekleştirir. Bu, tek bir utterance algılar. Tek bir utterüance 'in sonunda, sonda sessizlik dinlemesi veya en fazla 15 saniyelik ses işlenene kadar belirlenir.
* Sürekli tanıma (Async)-zaman uyumsuz olarak sürekli tanıma işlemini başlatır. Kullanıcı, tanıma sonuçlarını almak için olayı işlemek üzere bağlanmalıdır. Zaman uyumsuz sürekli tanımayı durdurmak için çağrısı [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#stopcontinuousrecognitionasync)yapın.

> [!NOTE]
> [Konuşma tanıma modunu seçme](../../../how-to-choose-recognition-mode.md)hakkında daha fazla bilgi edinin.

### <a name="single-shot-recognition"></a>Tek atışı tanıma

Şu kullanılarak [`RecognizeOnceAsync`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#recognizeonceasync)zaman uyumsuz tek kararlı tanıma örneği aşağıda verilmiştir:

```cpp
auto result = recognizer->RecognizeOnceAsync().get();
```

Sonucu işlemek için bazı kodlar yazmanız gerekir. Bu örnek [`result->Reason`](https://docs.microsoft.com/cpp/cognitive-services/speech/recognitionresult#reason)şunları değerlendirir:

* Tanınma sonucunu yazdırır:`ResultReason::RecognizedSpeech`
* Bir tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin:`ResultReason::NoMatch`
* Bir hata ile karşılaşırsanız, hata iletisini yazdırın:`ResultReason::Canceled`

```cpp
switch (result->Reason)
{
    case ResultReason::RecognizedSpeech:
        cout << "We recognized: " << result->Text << std::endl;
        break;
    case ResultReason::NoMatch:
        cout << "NOMATCH: Speech could not be recognized." << std::endl;
        break;
    case ResultReason::Canceled:
        {
            auto cancellation = CancellationDetails::FromResult(result);
            cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;
    
            if (cancellation->Reason == CancellationReason::Error) {
                cout << "CANCELED: ErrorCode= " << (int)cancellation->ErrorCode << std::endl;
                cout << "CANCELED: ErrorDetails=" << cancellation->ErrorDetails << std::endl;
                cout << "CANCELED: Did you update the subscription info?" << std::endl;
            }
        }
        break;
    default:
        break;
}
```

### <a name="continuous-recognition"></a>Sürekli tanıma

Sürekli tanıma, tek kararlı tanıma göre biraz daha karmaşıktır. Tanıma sonuçlarını almak için `Recognizing`, `Recognized`, ve `Canceled` olaylarına abone olmanızı gerektirir. Tanımayı durdurmak için [StopContinuousRecognitionAsync](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#stopcontinuousrecognitionasync)öğesini çağırmanız gerekir. İşte, bir ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirilebileceğini gösteren bir örnek.

Girişi tanımlayarak ve şunu başlatarak başlayalım [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer):

```cpp
auto audioInput = AudioConfig::FromWavFileInput("YourAudioFile.wav");
auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);
```

Sonra, konuşma tanımanın durumunu yönetmek için bir değişken oluşturalım. Başlamak için, tanımanın başlangıcında bitmediğini `promise<void>`güvenli bir şekilde varsaydığımız için bir bildirir.

```cpp
promise<void> recognitionEnd;
```

Kaynağından gönderilen olaylara abone edeceğiz [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer).

* [`Recognizing`](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#recognizing): Ara tanıma sonuçları içeren olaylar için sinyal.
* [`Recognized`](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#recognized): Son tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi olduğunu gösterir).
* [`SessionStopped`](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#sessionstopped): Bir tanıma oturumunun (işlem) sonunu gösteren olaylar için sinyal.
* [`Canceled`](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#canceled): İptal edilen tanıma sonuçlarını içeren olaylar için sinyal (sonuç veya doğrudan iptal isteği olarak iptal edilen bir tanıma girişimi veya ya da bir aktarım ya da protokol arızası).

```cpp
recognizer->Recognizing.Connect([](const SpeechRecognitionEventArgs& e)
    {
        cout << "Recognizing:" << e.Result->Text << std::endl;
    });

recognizer->Recognized.Connect([](const SpeechRecognitionEventArgs& e)
    {
        if (e.Result->Reason == ResultReason::RecognizedSpeech)
        {
            cout << "RECOGNIZED: Text=" << e.Result->Text 
                 << " (text could not be translated)" << std::endl;
        }
        else if (e.Result->Reason == ResultReason::NoMatch)
        {
            cout << "NOMATCH: Speech could not be recognized." << std::endl;
        }
    });

recognizer->Canceled.Connect([&recognitionEnd](const SpeechRecognitionCanceledEventArgs& e)
    {
        cout << "CANCELED: Reason=" << (int)e.Reason << std::endl;
        if (e.Reason == CancellationReason::Error)
        {
            cout << "CANCELED: ErrorCode=" << (int)e.ErrorCode << "\n"
                 << "CANCELED: ErrorDetails=" << e.ErrorDetails << "\n"
                 << "CANCELED: Did you update the subscription info?" << std::endl;

            recognitionEnd.set_value(); // Notify to stop recognition.
        }
    });

recognizer->SessionStopped.Connect([&recognitionEnd](const SessionEventArgs& e)
    {
        cout << "Session stopped.";
        recognitionEnd.set_value(); // Notify to stop recognition.
    });
```

Her şey ayarlandığında, çağırabiliriz [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#startcontinuousrecognitionasync).

```cpp
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
recognizer->StartContinuousRecognitionAsync().get();

// Waits for recognition end.
recognitionEnd.get_future().get();

// Stops recognition.
recognizer->StopContinuousRecognitionAsync().get();
```

### <a name="dictation-mode"></a>Dikte etme modu

Sürekli tanıma kullanırken, ilgili "dikte etmeyi etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, konuşma yapılandırma örneğinin noktalama gibi tümce yapılarının sözcük açıklamalarını yorumlamasını sağlar. Örneğin, "kasadaki gerçek zamanlı olarak", "kasadaki canlı mısınız?" metni olarak yorumlanabilir.

Dikte modunu etkinleştirmek için, içindeki [`EnableDictation`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#enabledictation) yöntemi kullanın. [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)

```cpp
config->EnableDictation();
```

## <a name="change-source-language"></a>Kaynak dilini değiştir

Konuşma tanıma için ortak bir görev, giriş (veya kaynak) dilini belirtmektir. Giriş dilini Almanca olarak nasıl değiştirebileceğinizi göz atalım. Kodunuzda [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig), daha sonra bu satırı hemen altına ekleyin.

```cpp
config->SetSpeechRecognitionLanguage("fr-FR");
```

[`SetSpeechRecognitionLanguage`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setspeechrecognitionlanguage)bağımsız değişken olarak bir dize alan parametredir. Desteklenen [yerel ayarlar/diller](../../../language-support.md)listesinde herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu geliştirme

Konuşma SDK 'Sı ile tanıma doğruluğunu geliştirmenin birkaç yolu vardır. Tümcecik listelerine göz atalım. Tümcecik listeleri, bir kişinin adı veya belirli bir konum gibi, ses verilerinde bilinen tümcecikleri belirlemek için kullanılır. Tek sözcükler veya bütün ifadeler, bir tümcecik listesine eklenebilir. Tanıma sırasında, tüm tümcecik için tam eşleşme sese dahil olursa tümcecik listesindeki bir giriş kullanılır. Tümcecikle tam eşleşme bulunamazsa, tanıma yardımlı değildir.

> [!IMPORTANT]
> Tümcecik listesi özelliği yalnızca Ingilizce olarak kullanılabilir.

Bir tümcecik listesi kullanmak için, önce bir [`PhraseListGrammar`](https://docs.microsoft.com/cpp/cognitive-services/speech/phraselistgrammar) nesne oluşturun, ardından ile [`AddPhrase`](https://docs.microsoft.com/cpp/cognitive-services/speech/phraselistgrammar#addphrase)belirli sözcükler ve deyimler ekleyin.

Herhangi bir değişiklik [`PhraseListGrammar`](https://docs.microsoft.com/cpp/cognitive-services/speech/phraselistgrammar) , bir sonraki tanıma göre veya konuşma hizmetine yeniden bağlanmaya sonra devreye girer.

```cpp
auto phraseListGrammar = PhraseListGrammar::FromRecognizer(recognizer);
phraseListGrammar->AddPhrase("Supercalifragilisticexpialidocious");
```

Tümcecik listenizi temizlemeniz gerekirse: 

```cpp
phraseListGrammar->Clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../how-to-custom-speech.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)