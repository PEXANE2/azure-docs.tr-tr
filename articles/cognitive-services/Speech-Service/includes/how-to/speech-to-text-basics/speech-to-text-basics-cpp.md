---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: dapine
ms.openlocfilehash: 8392e03f272c6db2bb384c8031cb975bca53d530
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79372805"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir Azure hesabınız ve Konuşma hizmeti aboneliğiniz olduğu varsayar. Hesabınız ve aboneliğiniz yoksa, [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Herhangi bir şey yapmadan önce Konuşma SDK'sını yüklemeniz gerekir. Platformunuza bağlı olarak aşağıdaki yönergeleri kullanın:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=linux&pivots=programming-language-cpp" target="_blank">Linux<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=macos&pivots=programming-language-cpp" target="_blank">Macos<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=windows&pivots=programming-language-cpp" target="_blank">Windows<span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK'sını kullanarak Konuşma hizmetini aramak için [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)bir . Bu sınıf, anahtarınız ve ilişkili bölgeniz, bitiş noktanız, ana bilgisayar veya yetkilendirme belirteciniz gibi aboneliğiniz hakkında bilgiler içerir.

> [!NOTE]
> Konuşma tanıma, konuşma sentezi, çeviri veya niyet tanıma gerçekleştirin, her zaman bir yapılandırma oluşturursunuz.

Bir baş harfe başlatmanın birkaç [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)yolu vardır:

* Abonelikle: bir anahtar ve ilişkili bölgede geçirin.
* Bir bitiş noktası ile: Konuşma hizmeti bitiş noktası geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir ana bilgisayar adresi geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci yle: yetkilendirme jetonundan ve ilişkili bölgeye geçin.

Bir anahtar ve bölge kullanılarak [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig) nasıl oluşturulduğuna bir göz atalım.

```cpp
auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Tanıyıcıyı başlatma

Bir , bir [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)sonraki adım oluşturduktan sonra [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer)bir . Bir [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer)harfini başharflediğinizde, bunu geçirmeniz `speech_config`gerekir. Bu, konuşma hizmetinin isteğinizi doğrulamak için gerektirdiği kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu kullanarak konuşmayı fark ediyorsanız, aşağıdaki [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer) gibi görünmelidir:

```cpp
auto recognizer = SpeechRecognizer::FromConfig(config);
```

Ses giriş aygıtını belirtmek istiyorsanız, ''ınızı ilke [`AudioConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/audio-audioconfig) olarak alırken `audioConfig` bir parametre [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer)oluşturmanız ve parametreyi sağlamanız gerekir.

> [!TIP]
> [Ses giriş aygıtınız için aygıt kimliğini nasıl alacağınızı öğrenin.](../../../how-to-select-audio-input-devices.md)

İlk olarak, `using namespace` tanımlarınızdan `#include` sonra aşağıdaki ifadeyi ekleyin.

```cpp
using namespace Microsoft::CognitiveServices::Speech::Audio;
```

Ardından, `AudioConfig` nesneye aşağıdaki gibi başvuruda bulunabilirsiniz:

```cpp
auto audioConfig = AudioConfig::FromDefaultMicrophoneInput();
auto recognizer = SpeechRecognizer::FromConfig(config, audioConfig);
```

Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de `audioConfig`bir . Ancak, `FromDefaultMicrophoneInput`aramak yerine [`AudioConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/audio-audioconfig)bir , parametre oluşturduğunuzda, `filename` arar `FromWavFileOutput` ve parametregeçersiniz.

```cpp
auto audioInput = AudioConfig::FromWavFileInput("YourAudioFile.wav");
auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);
```

## <a name="recognize-speech"></a>Konuşma tanıma

C++ için Konuşma SDK'sının [Recognizer sınıfı,](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer) konuşma tanıma için kullanabileceğiniz birkaç yöntemi ortaya çıkarır.

* Tek çekim tanıma (async) - Engellemesiz (asynchronous) modda tanıma gerçekleştirir. Bu tek bir söyleyiş tanıyacaktır. Tek bir söyleyninin sonu, sonunda sessizlik dinleyerek veya en fazla 15 saniyelik ses işlenene kadar belirlenir.
* Sürekli tanıma (async) - Asynchronously sürekli tanıma işlemi başlatır. Kullanıcı tanıma sonuçları almak için olayı işlemek için bağlanmak zorundadır. Eşzamanlı sürekli tanımayı durdurmak için. [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#stopcontinuousrecognitionasync)

> [!NOTE]
> [Konuşma tanıma modunu](../../../how-to-choose-recognition-mode.md)nasıl seçeceğiniz hakkında daha fazla bilgi edinin.

### <a name="single-shot-recognition"></a>Tek çekim tanıma

Burada kullanarak [`RecognizeOnceAsync`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#recognizeonceasync)eşzamanlı tek çekim tanıma bir örnek:

```cpp
auto result = recognizer->RecognizeOnceAsync().get();
```

Sonucu işlemek için bazı kod yazmanız gerekir. Bu örnek şu [`result->Reason`](https://docs.microsoft.com/cpp/cognitive-services/speech/recognitionresult#reason)şekilde değerlendirilir:

* Tanıma sonucunu yazdırır:`ResultReason::RecognizedSpeech`
* Tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin:`ResultReason::NoMatch`
* Bir hatayla karşılaşılırsa, hata iletisini yazdırın:`ResultReason::Canceled`

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

Sürekli tanıma, tek çekimtanımadan biraz daha fazla ilgilidir. Tanıma sonuçlarını almak için `Recognizing` `Recognized`, `Canceled` ve olaylara abone olmak için gerekli. Tanımayı durdurmak için [StopContinuousRecognitionAsync'i](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#stopcontinuousrecognitionasync)aramanız gerekir. Aşağıda, ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirildiğine ilişkin bir örnek verilmiştir.

Girişi tanımlayarak ve bir [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer)baş harfe başlayalım:

```cpp
auto audioInput = AudioConfig::FromWavFileInput("YourAudioFile.wav");
auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);
```

Sonra, konuşma tanıma durumunu yönetmek için bir değişken oluşturalım. Başlangıç olarak, tanımanın `promise<void>`başlangıcından beri güvenli bir şekilde bitmediğini varsayabiliriz.

```cpp
promise<void> recognitionEnd;
```

Biz gönderilen olaylara abone olacağız [`SpeechRecognizer`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer).

* [`Recognizing`](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#recognizing): Ara tanıma sonuçlarını içeren olaylar için sinyal.
* [`Recognized`](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#recognized): Nihai tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi gösteren).
* [`SessionStopped`](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#sessionstopped): Tanıma oturumunun (operasyonun) sona erini gösteren olaylar için sinyal.
* [`Canceled`](https://docs.microsoft.com/cpp/cognitive-services/speech/asyncrecognizer#canceled): İptal edilmiş tanıma sonuçlarını içeren olaylar için sinyal (sonuç olarak iptal edilen bir tanıma girişimi veya doğrudan iptal isteği veya alternatif olarak bir aktarım veya protokol hatası) gösterir.

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

Her şey ayarlandırıtamam, arayabiliyoruz. [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechrecognizer#startcontinuousrecognitionasync)

```cpp
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
recognizer->StartContinuousRecognitionAsync().get();

// Waits for recognition end.
recognitionEnd.get_future().get();

// Stops recognition.
recognizer->StopContinuousRecognitionAsync().get();
```

### <a name="dictation-mode"></a>Dikte modu

Sürekli tanıma kullanırken, ilgili "dikte işlemini etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, noktalama işaretleri gibi tümce yapılarının sözcük açıklamalarını yorumlamak için konuşma config örneğine neden olur. Örneğin, "Şehirde mi yaşıyorsunuz soru işareti" sözü "Şehirde mi yaşıyorsunuz?" şeklinde yorumlanır.

Dikte modunu etkinleştirmek için, [`EnableDictation`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#enabledictation) yöntemi [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)nikullanarak.

```cpp
config->EnableDictation();
```

## <a name="change-source-language"></a>Kaynak dilini değiştirme

Konuşma tanıma için ortak bir görev giriş (veya kaynak) dilini belirtmektir. Giriş dilini Almanca'ya nasıl değiştireceğinize bir göz atalım. Kodunuzda, [`SpeechConfig`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)bu satırı doğrudan altına ekleyin.

```cpp
config->SetSpeechRecognitionLanguage("fr-FR");
```

[`SetSpeechRecognitionLanguage`](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setspeechrecognitionlanguage)bir bağımsız değişken olarak bir dize alan bir parametredir. Desteklenen [yerel halk/dil](../../../language-support.md)listesinde herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu artırın

Konuşma SDK ile tanıma doğruluğunu artırmanın birkaç yolu vardır. İfade Listeleri'ne bir göz atalım. Tümcecik Listeleri, ses verilerindeki bilinen ifadeleri tanımlamak için kullanılır, örneğin bir kişinin adı veya belirli bir konum. Tümcecik Listesine tek sözcük veya tam tümcecik eklenebilir. Tanıma sırasında, sese tüm tümcecik için tam bir eşleşme varsa, bir ifade listesindeki bir giriş kullanılır. İfadeyle tam eşleşme bulunamazsa, tanıma yardımcı olmaz.

> [!IMPORTANT]
> İfade Listesi özelliği yalnızca İngilizce olarak kullanılabilir.

Bir tümcecik listesini kullanmak [`PhraseListGrammar`](https://docs.microsoft.com/cpp/cognitive-services/speech/phraselistgrammar) için önce bir nesne oluşturun, sonra [`AddPhrase`](https://docs.microsoft.com/cpp/cognitive-services/speech/phraselistgrammar#addphrase)belirli sözcükler ve tümcecikler ekleyin.

Bir sonraki [`PhraseListGrammar`](https://docs.microsoft.com/cpp/cognitive-services/speech/phraselistgrammar) tanıma da veya Konuşma hizmetine yeniden bağlantı dan sonra etkili olacak değişiklikler.

```cpp
auto phraseListGrammar = PhraseListGrammar::FromRecognizer(recognizer);
phraseListGrammar->AddPhrase("Supercalifragilisticexpialidocious");
```

İfade listenizi temizlemeniz gerekiyorsa: 

```cpp
phraseListGrammar->Clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../how-to-custom-speech.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)