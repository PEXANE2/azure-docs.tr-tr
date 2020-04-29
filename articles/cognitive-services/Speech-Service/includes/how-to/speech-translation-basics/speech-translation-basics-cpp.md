---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/13/2020
ms.author: trbye
ms.openlocfilehash: 09f08e314a634de13a683440ad9fead97ad8a260
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81399657"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir. Platformunuza bağlı olarak, konuşma SDK makalesinin <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-sdk#get-the-speech-sdk" target="_blank">konuşma SDK 'sını <span class="docon docon-navigate-external x-hidden-focus"></span> al</a> bölümünde yer alan yönergeleri izleyin.

## <a name="import-dependencies"></a>Bağımlılıkları içeri aktar

Bu makaledeki örnekleri çalıştırmak için, C++ kod dosyasının en üstüne `#include` aşağıdaki `using` ve deyimlerini ekleyin.

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

## <a name="sensitive-data-and-environment-variables"></a>Hassas veriler ve ortam değişkenleri

Bu makaledeki örnek kaynak kodu, konuşma kaynağı abonelik anahtarı ve bölgesi gibi hassas verileri depolamak için ortam değişkenlerine bağımlıdır. C++ kod dosyası, `SPEECH__SUBSCRIPTION__KEY` ve `SPEECH__SERVICE__REGION`gibi konak makineler ortam değişkenlerinden atanan iki dize değeri içerir. Bu alanların her ikisi de sınıf kapsamıdır ve sınıfın yöntem gövdelerinde erişilebilir hale getirir. Ortam değişkenleri hakkında daha fazla bilgi için bkz. [ortam değişkenleri ve uygulama yapılandırması](../../../../cognitive-services-security.md#environment-variables-and-application-configuration).

```cpp
auto SPEECH__SUBSCRIPTION__KEY = getenv("SPEECH__SUBSCRIPTION__KEY");
auto SPEECH__SERVICE__REGION = getenv("SPEECH__SERVICE__REGION");
```

## <a name="create-a-speech-translation-configuration"></a>Konuşma çevirisi yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir [`SpeechTranslationConfig`][config]oluşturmanız gerekir. Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir.

> [!TIP]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

[`SpeechTranslationConfig`][config]Şunları başlatabilmeniz için birkaç yol vardır:

* Abonelik ile: bir anahtarı ve ilişkili bölgeyi geçirin.
* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

Bir [`SpeechTranslationConfig`][config] anahtar ve bölge kullanarak nasıl oluşturulduğuna göz atalım. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

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

## <a name="change-source-language"></a>Kaynak dilini değiştir

Konuşma çevirisi 'nin yaygın bir görevi, giriş (veya kaynak) dilini belirtmektir. Giriş dilini Italyanca olarak nasıl değiştirebileceğinizi göz atalım. Kodunuzda, [`SpeechTranslationConfig`][config] `SetSpeechRecognitionLanguage` yöntemini çağırarak örnekle etkileşime geçin.

```cpp
void translateSpeech() {
    auto translationConfig =
        SpeechTranslationConfig::FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    // Source (input) language
    translationConfig->SetSpeechRecognitionLanguage("it-IT");
}
```

Özelliği [`SpeechRecognitionLanguage`][recognitionlang] , bir dil yerel ayar dizesi bekliyor. Desteklenen [yerel ayarlar/diller](../../../language-support.md)listesindeki **yerel ayar** sütununda herhangi bir değer sağlayabilirsiniz.

## <a name="add-translation-language"></a>Çeviri dili Ekle

Konuşma çevirisi 'nin başka bir ortak görevi, hedef çeviri dillerini belirtmektir, ancak en az bir tane gereklidir ancak katları desteklenir. Aşağıdaki kod parçacığında, hem Fransızca hem de Almanca, Çeviri dili olarak hedefler.

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

Her çağrısıyla [`AddTargetLanguage`][addlang], yeni bir hedef çeviri dili belirtilir. Diğer bir deyişle, konuşma kaynak dilden tanındığında, her hedef çeviri, elde edilen çeviri işleminin bir parçası olarak kullanılabilir.

## <a name="initialize-a-translation-recognizer"></a>Çeviri tanıyıcısı başlatma

Bir [`SpeechTranslationConfig`][config]oluşturduktan sonra, bir sonraki adım bir [`TranslationRecognizer`][recognizer]' ı başlatmaktır. Bir [`TranslationRecognizer`][recognizer]başlattığınızda, bunu geçirmeniz gerekir `translationConfig`. Yapılandırma nesnesi, konuşma hizmeti 'nin isteğinizi doğrulamak için ihtiyaç duyduğu kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu [`TranslationRecognizer`][recognizer] kullanarak konuşmayı tanıyor olmanız halinde şöyle görünmelidir:

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

Ses giriş cihazını belirtmek isterseniz, oluşturmanız [`AudioConfig`][audioconfig] ve ' ı başlatırken `audioConfig` parametresini sağlamanız gerekir. [`TranslationRecognizer`][recognizer]

> [!TIP]
> [Ses giriş cihazınız için CIHAZ kimliğini nasıl alabileceğinizi öğrenin](../../../how-to-select-audio-input-devices.md).

İlk olarak, `AudioConfig` nesnesine aşağıdaki gibi başvuracağız:

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

Mikrofon kullanmak yerine bir ses dosyası sağlamak istiyorsanız, yine de sağlamanız gerekir `audioConfig`. Ancak [`AudioConfig`][audioconfig], öğesini çağırmak `FromDefaultMicrophoneInput`yerine bir oluşturduğunuzda, `FromWavFileInput` `filename` parametresini çağırır ve geçireceğiz.

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

Konuşmayı çevirmek için, konuşma SDK 'Sı bir mikrofona veya ses dosyası girişine bağımlıdır. Konuşma tanıma, konuşma çevirisi öncesinde oluşur. Tüm nesneler başlatıldıktan sonra, Recognize-bir kez işlevini çağırın ve sonucu alın.

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

Konuşmayı metne dönüştürme hakkında daha fazla bilgi için bkz. [konuşma tanımanın temelleri](../../../speech-to-text-basics.md).

## <a name="synthesize-translations"></a>Çevirileri sentezleştir

Başarılı bir konuşma tanıma ve çeviri işleminden sonra, sonuç bir Sözlükteki tüm çevirileri içerir. [`Translations`][translations] Sözlük anahtarı hedef çeviri dilidir ve değer, çevrilmiş metindir. Tanınan konuşma çevrilebilir ve farklı bir dilde (konuşma konuşmaya) alınabilir.

### <a name="event-based-synthesis"></a>Olay tabanlı sensıs

`TranslationRecognizer` Nesne bir `Synthesizing` olay gösterir. Olay birkaç kez harekete geçirilir ve çeviri tanıma sonucundan birleştirilmiş sesi almak için bir mekanizma sağlar. Birden çok dile arıyorsanız bkz. [el ile birleştirme](#manual-synthesis). Bir oluşturma [`SetVoiceName`][voicename] ve `Synthesizing` olay için olay işleyicisi sağlama yoluyla sensıs sesini belirtin, sesi alın. Aşağıdaki örnek, çevrilmiş sesi bir *. wav* dosyası olarak kaydeder.

> [!IMPORTANT]
> Olay tabanlı birleştirme yalnızca tek bir çeviri ile birlikte çalışarak birden çok hedef çeviri **dili eklemeyin.** [`SetVoiceName`][voicename] Ayrıca, hedef çeviri diliyle aynı dil olmalıdır, örneğin; `"de"` ile `"de-DE-Hedda"`eşlenecek.

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

### <a name="manual-synthesis"></a>El ile birleştirme

[`Translations`][translations] Sözlük, çeviri metninin sesini sentezleştirmek için kullanılabilir. Her bir çeviride yineleme yapın ve çeviriyi sentezleştirme. Bir `SpeechSynthesizer` örnek oluştururken `SpeechConfig` nesnenin [`SetSpeechSynthesisVoiceName`][speechsynthesisvoicename] özelliği istenen sesle ayarlanmış olması gerekir. Aşağıdaki örnek, beş dili dönüştürür ve her bir çeviri, karşılık gelen sinir dilinde bir ses dosyası ile birleştirilmiştir.

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

Konuşma birleştirme hakkında daha fazla bilgi için bkz. [konuşma birleştirme temelleri](../../../text-to-speech-basics.md).

[config]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-speechtranslationconfig
[audioconfig]: https://docs.microsoft.com/cpp/cognitive-services/speech/audio-audioconfig
[recognizer]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-translationrecognizer
[recognitionlang]: https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setspeechrecognitionlanguage
[addlang]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-speechtranslationconfig#addtargetlanguage
[translations]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-translationrecognitionresult#translations
[voicename]: https://docs.microsoft.com/cpp/cognitive-services/speech/translation-speechtranslationconfig#setvoicename
[speechsynthesisvoicename]: https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig#setspeechsynthesisvoicename
