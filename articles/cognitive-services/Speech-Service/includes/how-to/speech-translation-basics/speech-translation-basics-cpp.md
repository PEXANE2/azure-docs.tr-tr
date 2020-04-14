---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/13/2020
ms.author: dapine
ms.openlocfilehash: 7c57952d0b78271bbcc45bd282385524772f0f9e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266652"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir Azure hesabınız ve Konuşma hizmeti aboneliğiniz olduğu varsayar. Hesabınız ve aboneliğiniz yoksa, [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Herhangi bir şey yapmadan önce Konuşma SDK'sını yüklemeniz gerekir. Platformunuza bağlı olarak, Konuşma <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-sdk#get-the-speech-sdk" target="_blank">SDK makalesinin <span class="docon docon-navigate-external x-hidden-focus"></span> Konuşma SDK bölümünün</a> altındaki talimatları izleyin.

## <a name="import-dependencies"></a>İthalat bağımlılıkları

Bu makaledeki örnekleri çalıştırmak için, `#include` `using` C++ kodu dosyasının üst kısmında aşağıdaki leri ve ifadeleri ekleyin.

```cpp
#include <iostream> // cin, cout
#include <fstream>
#include <string>
#include <stdio.h>
#include <stdlib.h>
#include <speechapi_cxx.h>

using namespace std;
using namespace Microsoft::CognitiveServices::Speech;
using namespace Microsoft::CognitiveServices::Speech::Audio;
using namespace Microsoft::CognitiveServices::Speech::Translation;
```

## <a name="sensitive-data-and-environment-variables"></a>Hassas veriler ve çevre değişkenleri

Bu makaledeki örnek kaynak kodu, Konuşma kaynağı abonelik anahtarı ve bölge gibi hassas verileri depolamak için ortam değişkenlerine bağlıdır. C++ kodu dosyası, ana bilgisayar ortam değişkenlerinden atanan iki dize değeri içerir, yani `SPEECH__SUBSCRIPTION__KEY` `SPEECH__SERVICE__REGION`. Bu alanların her ikisi de sınıf kapsamı içinde dir ve bu da onları sınıfın yöntem gövdeleri içinde erişilebilir hale getirir. Çevre değişkenleri hakkında daha fazla bilgi için [çevre değişkenlerine ve uygulama yapılandırmalarına](../../../../cognitive-services-security.md#environment-variables-and-application-configuration)bakın.

```cpp
auto SPEECH__SUBSCRIPTION__KEY = getenv("SPEECH__SUBSCRIPTION__KEY");
auto SPEECH__SERVICE__REGION = getenv("SPEECH__SERVICE__REGION");
```

## <a name="create-a-speech-translation-configuration"></a>Konuşma çevirisi yapılandırması oluşturma

Konuşma SDK'sını kullanarak Konuşma hizmetini aramak için [`SpeechTranslationConfig`][config]bir . Bu sınıf, anahtarınız ve ilişkili bölgeniz, bitiş noktanız, ana bilgisayar veya yetkilendirme belirteciniz gibi aboneliğiniz hakkında bilgiler içerir.

> [!TIP]
> Konuşma tanıma, konuşma sentezi, çeviri veya niyet tanıma gerçekleştirin, her zaman bir yapılandırma oluşturursunuz.

Bir baş harfe başlatmanın birkaç [`SpeechTranslationConfig`][config]yolu vardır:

* Abonelikle: bir anahtar ve ilişkili bölgede geçirin.
* Bir bitiş noktası ile: Konuşma hizmeti bitiş noktası geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir ana bilgisayar adresi geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci yle: yetkilendirme jetonundan ve ilişkili bölgeye geçin.

Bir anahtar ve bölge kullanılarak [`SpeechTranslationConfig`][config] nasıl oluşturulduğuna bir göz atalım. Bölge tanımlayıcınızı bulmak için [bölge destek](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```cpp
auto SPEECH__SUBSCRIPTION__KEY = getenv("SPEECH__SUBSCRIPTION__KEY");
auto SPEECH__SERVICE__REGION = getenv("SPEECH__SERVICE__REGION");

void translateSpeech() {
    auto config =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);
}

int main(int argc, char** argv) {
    setlocale(LC_ALL, "");
    translateSpeech();
    return 0;
}
```

## <a name="change-source-language"></a>Kaynak dilini değiştirme

Konuşma çevirisinin yaygın bir görevi giriş (veya kaynak) dilini belirtmektir. Giriş dilini İtalyanca'ya nasıl değiştireceğinize bir göz atalım. Kodunuzda, `SetSpeechRecognitionLanguage` yöntemi [`SpeechTranslationConfig`][config] çağırarak örnekle etkileşime geçin.

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    // Source (input) language
    translationConfig->SetSpeechRecognitionLanguage("it-IT");
}
```

Özellik [`SpeechRecognitionLanguage`][recognitionlang] bir dil-yerel biçim dizesi bekliyor. Desteklenen [yerel halk/dil](../../../language-support.md)listesindeki **Yerel sütundaki** herhangi bir değer sağlayabilirsiniz.

## <a name="add-translation-language"></a>Çeviri dili ekleme

Konuşma çevirisinin bir diğer yaygın görevi hedef çeviri dillerini belirtmektir, en az bir tane gereklidir, ancak katları desteklenir. Aşağıdaki kod snippet' inde, çeviri dili olarak hem Fransızca hem de Almanca hedefolarak.

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    translationConfig->SetSpeechRecognitionLanguage("it-IT");

    // Translate to languages. See, https://aka.ms/speech/sttt-languages
    translationConfig->AddTargetLanguage("fr");
    translationConfig->AddTargetLanguage("de");
}
```

Her aramada [`AddTargetLanguage`][addlang]yeni bir hedef çeviri dili belirtilir. Başka bir deyişle, konuşma kaynak dilden tanındığı zaman, her hedef çeviri, ortaya çıkan çeviri işleminin bir parçası olarak kullanılabilir.

## <a name="initialize-a-translation-recognizer"></a>Çeviri tanıyıcısını başlatma

Bir , bir [`SpeechTranslationConfig`][config]sonraki adım oluşturduktan sonra [`TranslationRecognizer`][recognizer]bir . Bir [`TranslationRecognizer`][recognizer]harfini başharflediğinizde, bunu geçirmeniz `translationConfig`gerekir. Yapılandırma nesnesi, konuşma hizmetinin isteğinizi doğrulamak için gerektirdiği kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu kullanarak konuşmayı fark ediyorsanız, aşağıdaki [`TranslationRecognizer`][recognizer] gibi görünmelidir:

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    auto fromLanguage = "en-US";
    auto toLanguages = { "it", "fr", "de" };
    translationConfig->SetSpeechRecognitionLanguage(fromLanguage);
    for (auto language : toLanguages) {
        translationConfig->AddTargetLanguage(language);
    }

    auto recognizer = TranslationRecognizer::FromConfig(translationConfig);
}
```

Ses giriş aygıtını belirtmek istiyorsanız, ''ınızı ilke [`AudioConfig`][audioconfig] olarak alırken `audioConfig` bir parametre [`TranslationRecognizer`][recognizer]oluşturmanız ve parametreyi sağlamanız gerekir.

> [!TIP]
> [Ses giriş aygıtınız için aygıt kimliğini nasıl alacağınızı öğrenin.](../../../how-to-select-audio-input-devices.md)

İlk olarak, nesneye `AudioConfig` aşağıdaki gibi başvurursunuz:

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    auto fromLanguage = "en-US";
    auto toLanguages = { "it", "fr", "de" };
    translationConfig->SetSpeechRecognitionLanguage(fromLanguage);
    for (auto language : toLanguages) {
        translationConfig->AddTargetLanguage(language);
    }

    auto audioConfig = AudioConfig::FromDefaultMicrophoneInput();
    auto recognizer = TranslationRecognizer::FromConfig(translationConfig, audioConfig);
}
```

Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de `audioConfig`bir . Ancak, `FromDefaultMicrophoneInput`aramak yerine [`AudioConfig`][audioconfig]bir , parametre oluşturduğunuzda, `filename` arar `FromWavFileInput` ve parametregeçersiniz.

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    auto fromLanguage = "en-US";
    auto toLanguages = { "it", "fr", "de" };
    translationConfig->SetSpeechRecognitionLanguage(fromLanguage);
    for (auto language : toLanguages) {
        translationConfig->AddTargetLanguage(language);
    }

    auto audioConfig = AudioConfig::FromWavFileInput("YourAudioFile.wav");
    auto recognizer = TranslationRecognizer::FromConfig(translationConfig, audioConfig);
}
```

## <a name="translate-speech"></a>Konuşmayı çevirme

Konuşma çevirmek için, Konuşma SDK bir mikrofon veya ses dosyası girişi dayanır. Konuşma tanıma, konuşma çevirisiden önce gerçekleşir. Tüm nesneler baş harfe böldükten sonra tanı-bir işlevi arayın ve sonucu alın.

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    string fromLanguage = "en-US";
    string toLanguages[3] = { "it", "fr", "de" };
    translationConfig->SetSpeechRecognitionLanguage(fromLanguage);
    for (auto language : toLanguages) {
        translationConfig->AddTargetLanguage(language);
    }

    auto recognizer = TranslationRecognizer::FromConfig(translationConfig);
    cout << "Say something in '" << fromLanguage << "' and we'll translate...\n";

    auto result = recognizer->RecognizeOnceAsync().get();
    if (result->Reason == ResultReason::TranslatedSpeech)
    {
        cout << "Recognized: \"" << result->Text << "\"" << std::endl;
        for (auto pair : result->Translations)
        {
            auto language = pair.first;
            auto translation = pair.second;
            cout << "Translated into '" << language << "': " << translation << std::endl;
        }
    }
}
```

Konuşmadan metne ilişkin daha fazla bilgi için [konuşma tanımanın temellerini](../../../speech-to-text-basics.md)görün.

## <a name="synthesize-translations"></a>Synthesize çevirileri

Başarılı bir konuşma tanıma ve çeviriden sonra, sonuç sözlükteki tüm çevirileri içerir. Sözlük [`Translations`][translations] anahtarı hedef çeviri dilidir ve değeri çevrilmiş metindir. Tanınan konuşma tercüme edilebilir, sonra farklı bir dilde sentezlenebilir (konuşma-konuşma).

### <a name="event-based-synthesis"></a>Olay tabanlı sentez

Nesne `TranslationRecognizer` bir `Synthesizing` olayı ortaya çıkarır. Olay birkaç kez ateşler ve sentezlenen sesi çeviri tanıma sonucundan almak için bir mekanizma sağlar. Birden çok dile çeviri yorsanız, [bkz.](#manual-synthesis) Bir [`SetVoiceName`][voicename] atama yaparak sentez sesi belirtin ve olay `Synthesizing` için bir olay işleyicisi sağlayın, ses olsun. Aşağıdaki örnek, çevrilen sesi *.wav* dosyası olarak kaydeder.

> [!IMPORTANT]
> Olay tabanlı sentez yalnızca tek bir çeviri ile çalışır, birden çok hedef çeviri dili **eklemeyin.** Ayrıca, hedef [`SetVoiceName`][voicename] çeviri dili ile aynı dil olmalıdır, örneğin; `"de"` için `"de-DE-Hedda"`harita olabilir.

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    auto fromLanguage = "en-US";
    auto toLanguage = "de";
    translationConfig->SetSpeechRecognitionLanguage(fromLanguage);
    translationConfig->AddTargetLanguage(toLanguage);

    // See: https://aka.ms/speech/sdkregion#standard-and-neural-voices
    translationConfig->SetVoiceName("de-DE-Hedda");

    auto recognizer = TranslationRecognizer::FromConfig(translationConfig);
    recognizer->Synthesizing.Connect([](const TranslationSynthesisEventArgs& e)
        {
            auto audio = e.Result->Audio;
            auto size = audio.size();
            cout << "Audio synthesized: " << size << " byte(s)" << (size == 0 ? "(COMPLETE)" : "") << std::endl;

            if (size > 0) {
                ofstream file("translation.wav", ios::out | ios::binary);
                auto audioData = audio.data();
                file.write((const char*)audioData, sizeof(audio[0]) * size);
                file.close();
            }
        });

    cout << "Say something in '" << fromLanguage << "' and we'll translate...\n";

    auto result = recognizer->RecognizeOnceAsync().get();
    if (result->Reason == ResultReason::TranslatedSpeech)
    {
        cout << "Recognized: \"" << result->Text << "\"" << std::endl;
        for (auto pair : result->Translations)
        {
            auto language = pair.first;
            auto translation = pair.second;
            cout << "Translated into '" << language << "': " << translation << std::endl;
        }
    }
}
```

### <a name="manual-synthesis"></a>Manuel sentez

Sözlük, [`Translations`][translations] çeviri metninden ses sentezlemek için kullanılabilir. Her çeviride yineleyin ve çeviriyi sentezleyin. Bir `SpeechSynthesizer` örnek oluştururken, nesnenin `SpeechConfig` [`SetSpeechSynthesisVoiceName`][speechsynthesisvoicename] özelliğinin istenilen sese ayarlanması gerekir. Aşağıdaki örnek beş dile çevirir ve her çeviri daha sonra ilgili nöral dilde bir ses dosyasına sentezlenir.

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    auto fromLanguage = "en-US";
    auto toLanguages = { "de", "en", "it", "pt", "zh-Hans" };
    translationConfig->SetSpeechRecognitionLanguage(fromLanguage);
    for (auto language : toLanguages) {
        translationConfig->AddTargetLanguage(language);
    }

    auto recognizer = TranslationRecognizer::FromConfig(translationConfig);

    cout << "Say something in '" << fromLanguage << "' and we'll translate...\n";

    auto result = recognizer->RecognizeOnceAsync().get();
    if (result->Reason == ResultReason::TranslatedSpeech)
    {
        // See: https://aka.ms/speech/sdkregion#standard-and-neural-voices
        map<string, string> languageToVoiceMap;
        languageToVoiceMap["de"] = "de-DE-KatjaNeural";
        languageToVoiceMap["en"] = "en-US-AriaNeural";
        languageToVoiceMap["it"] = "it-IT-ElsaNeural";
        languageToVoiceMap["pt"] = "pt-BR-FranciscaNeural";
        languageToVoiceMap["zh-Hans"] = "zh-CN-XiaoxiaoNeural";

        cout << "Recognized: \"" << result->Text << "\"" << std::endl;
        for (auto pair : result->Translations)
        {
            auto language = pair.first;
            auto translation = pair.second;
            cout << "Translated into '" << language << "': " << translation << std::endl;

            auto speech_config =
                SpeechConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);
            speech_config->SetSpeechSynthesisVoiceName(languageToVoiceMap[language]);

            auto audio_config = AudioConfig::FromWavFileOutput(language + "-translation.wav");
            auto synthesizer = SpeechSynthesizer::FromConfig(speech_config, audio_config);

            synthesizer->SpeakTextAsync(translation).get();
        }
    }
}
```

Konuşma sentezi hakkında daha fazla bilgi [için, konuşma sentezinin temellerini](../../../text-to-speech-basics.md)görün.

[config]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-speechtranslationconfig
[audioconfig]: https://docs.microsoft.com/cpp/cognitive-services/speech/audio-audioconfig
[recognizer]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-translationrecognizer
[recognitionlang]: https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setspeechrecognitionlanguage
[addlang]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-speechtranslationconfig#addtargetlanguage
[translations]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-translationrecognitionresult#translations
[voicename]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-speechtranslationconfig#setvoicename
[speechsynthesisvoicename]: https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setspeechsynthesisvoicename
