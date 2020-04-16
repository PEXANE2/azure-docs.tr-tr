---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/13/2020
ms.author: trbye
ms.openlocfilehash: 17d8c0157fcd478d01452167d240fb67daeeda5b
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399663"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir Azure hesabınız ve Konuşma hizmeti aboneliğiniz olduğu varsayar. Hesabınız ve aboneliğiniz yoksa, [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Herhangi bir şey yapmadan önce Konuşma SDK'sını yüklemeniz gerekir. Platformunuza bağlı olarak, Konuşma <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-sdk#get-the-speech-sdk" target="_blank">SDK makalesinin <span class="docon docon-navigate-external x-hidden-focus"></span> Konuşma SDK bölümünün</a> altındaki talimatları izleyin.

## <a name="import-dependencies"></a>İthalat bağımlılıkları

Bu makaledeki örnekleri çalıştırmak için `import` python kodu dosyasının en üstünde aşağıdaki ifadeleri ekleyin.

```python
import os
import azure.cognitiveservices.speech as speechsdk
```

## <a name="sensitive-data-and-environment-variables"></a>Hassas veriler ve çevre değişkenleri

Bu makaledeki örnek kaynak kodu, Konuşma kaynağı abonelik anahtarı ve bölge gibi hassas verileri depolamak için ortam değişkenlerine bağlıdır. Python kodu dosyası, ana bilgisayar ortam değişkenlerinden atanan iki `SPEECH__SUBSCRIPTION__KEY` değer `SPEECH__SERVICE__REGION`içerir, yani. Bu değişkenlerin her ikisi de genel kapsamdadır ve kod dosyasının işlev tanımı içinde erişilebilir olmalarını sağlar. Çevre değişkenleri hakkında daha fazla bilgi için [çevre değişkenlerine ve uygulama yapılandırmalarına](../../../../cognitive-services-security.md#environment-variables-and-application-configuration)bakın.

```python
speech_key, service_region = os.environ['SPEECH__SUBSCRIPTION__KEY'], os.environ['SPEECH__SERVICE__REGION']
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

```python
from_language, to_language = 'en-US', 'de'

def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)
```

## <a name="change-source-language"></a>Kaynak dilini değiştirme

Konuşma çevirisinin yaygın bir görevi giriş (veya kaynak) dilini belirtmektir. Giriş dilini İtalyanca'ya nasıl değiştireceğinize bir göz atalım. Kodunuzda, [`SpeechTranslationConfig`][config] özelliğe atama, örnekle `speech_recognition_language` etkileşimde bulundu.

```python
def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)

    # Source (input) language
    translation_config.speech_recognition_language = from_language
```

Özellik [`speech_recognition_language`][recognitionlang] bir dil-yerel biçim dizesi bekliyor. Desteklenen [yerel halk/dil](../../../language-support.md)listesindeki **Yerel sütundaki** herhangi bir değer sağlayabilirsiniz.

## <a name="add-translation-language"></a>Çeviri dili ekleme

Konuşma çevirisinin bir diğer yaygın görevi hedef çeviri dillerini belirtmektir, en az bir tane gereklidir, ancak katları desteklenir. Aşağıdaki kod snippet' inde, çeviri dili olarak hem Fransızca hem de Almanca hedefolarak.

```python
def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)

    translation_config.speech_recognition_language = "it-IT"

    # Translate to languages. See, https://aka.ms/speech/sttt-languages
    translation_config.add_target_language("fr")
    translation_config.add_target_language("de")
```

Her aramada [`add_target_language`][addlang]yeni bir hedef çeviri dili belirtilir. Başka bir deyişle, konuşma kaynak dilden tanındığı zaman, her hedef çeviri, ortaya çıkan çeviri işleminin bir parçası olarak kullanılabilir.

## <a name="initialize-a-translation-recognizer"></a>Çeviri tanıyıcısını başlatma

Bir , bir [`SpeechTranslationConfig`][config]sonraki adım oluşturduktan sonra [`TranslationRecognizer`][recognizer]bir . Bir [`TranslationRecognizer`][recognizer]harfini başharflediğinizde, bunu geçirmeniz `translation_config`gerekir. Yapılandırma nesnesi, konuşma hizmetinin isteğinizi doğrulamak için gerektirdiği kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu kullanarak konuşmayı fark ediyorsanız, aşağıdaki [`TranslationRecognizer`][recognizer] gibi görünmelidir:

```python
def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)

    translation_config.speech_recognition_language = from_language
    translation_config.add_target_language(to_language)

    recognizer = speechsdk.translation.TranslationRecognizer(
            translation_config=translation_config)
```

Ses giriş aygıtını belirtmek istiyorsanız, ''ınızı ilke [`AudioConfig`][audioconfig] olarak alırken `audio_config` bir parametre [`TranslationRecognizer`][recognizer]oluşturmanız ve parametreyi sağlamanız gerekir.

> [!TIP]
> [Ses giriş aygıtınız için aygıt kimliğini nasıl alacağınızı öğrenin.](../../../how-to-select-audio-input-devices.md)

İlk olarak, nesneye `AudioConfig` aşağıdaki gibi başvurursunuz:

```python
def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)

    translation_config.speech_recognition_language = from_language
    for lang in to_languages:
        translation_config.add_target_language(lang)

    audio_config = speechsdk.audio.AudioConfig(use_default_microphone=True)
    recognizer = speechsdk.translation.TranslationRecognizer(
            translation_config=translation_config, audio_config=audio_config)
```

Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de `audioConfig`bir . Ancak, aramak yerine [`AudioConfig`][audioconfig] `use_default_microphone=True`bir parametre oluşturduğunuzda, `filename="path-to-file.wav"` parametreyi `filename` arar ve sağlarsınız.

```python
def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)

    translation_config.speech_recognition_language = from_language
    for lang in to_languages:
        translation_config.add_target_language(lang)

    audio_config = speechsdk.audio.AudioConfig(filename="path-to-file.wav")
    recognizer = speechsdk.translation.TranslationRecognizer(
            translation_config=translation_config, audio_config=audio_config)
```

## <a name="translate-speech"></a>Konuşmayı çevirme

Konuşma çevirmek için, Konuşma SDK bir mikrofon veya ses dosyası girişi dayanır. Konuşma tanıma, konuşma çevirisiden önce gerçekleşir. Tüm nesneler baş harfe böldükten sonra tanı-bir işlevi arayın ve sonucu alın.

```python
import os
import azure.cognitiveservices.speech as speechsdk

speech_key, service_region = os.environ['SPEECH__SERVICE__KEY'], os.environ['SPEECH__SERVICE__REGION']
from_language, to_languages = 'en-US', 'de'

def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)

    translation_config.speech_recognition_language = from_language
    translation_config.add_target_language(to_language)

    recognizer = speechsdk.translation.TranslationRecognizer(
            translation_config=translation_config)
    
    print('Say something...')
    result = recognizer.recognize_once()
    print(get_result_text(reason=result.reason, result=result))

def get_result_text(reason, result):
    reason_format = {
        speechsdk.ResultReason.TranslatedSpeech:
            f'RECOGNIZED "{from_language}": {result.text}\n' +
            f'TRANSLATED into "{to_language}"": {result.translations[to_language]}',
        speechsdk.ResultReason.RecognizedSpeech: f'Recognized: "{result.text}"',
        speechsdk.ResultReason.NoMatch: f'No speech could be recognized: {result.no_match_details}',
        speechsdk.ResultReason.Canceled: f'Speech Recognition canceled: {result.cancellation_details}'
    }
    return reason_format.get(reason, 'Unable to recognize speech')

translate_speech_to_text()
```

Konuşmadan metne ilişkin daha fazla bilgi için [konuşma tanımanın temellerini](../../../speech-to-text-basics.md)görün.

## <a name="synthesize-translations"></a>Synthesize çevirileri

Başarılı bir konuşma tanıma ve çeviriden sonra, sonuç sözlükteki tüm çevirileri içerir. Sözlük [`translations`][translations] anahtarı hedef çeviri dilidir ve değeri çevrilmiş metindir. Tanınan konuşma tercüme edilebilir, sonra farklı bir dilde sentezlenebilir (konuşma-konuşma).

### <a name="event-based-synthesis"></a>Olay tabanlı sentez

Nesne `TranslationRecognizer` bir `Synthesizing` olayı ortaya çıkarır. Olay birkaç kez ateşler ve sentezlenen sesi çeviri tanıma sonucundan almak için bir mekanizma sağlar. Birden çok dile çeviri yorsanız, [bkz.](#manual-synthesis) Bir [`voice_name`][voicename] atama yaparak sentez sesi belirtin ve olay `Synthesizing` için bir olay işleyicisi sağlayın, ses olsun. Aşağıdaki örnek, çevrilen sesi *.wav* dosyası olarak kaydeder.

> [!IMPORTANT]
> Olay tabanlı sentez yalnızca tek bir çeviri ile çalışır, birden çok hedef çeviri dili **eklemeyin.** Ayrıca, hedef [`voice_name`][voicename] çeviri dili ile aynı dil olmalıdır, örneğin; `"de"` için `"de-DE-Hedda"`harita olabilir.

```python
import os
import azure.cognitiveservices.speech as speechsdk

speech_key, service_region = os.environ['SPEECH__SERVICE__KEY'], os.environ['SPEECH__SERVICE__REGION']
from_language, to_language = 'en-US', 'de'

def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)

    translation_config.speech_recognition_language = from_language
    translation_config.add_target_language(to_language)

    # See: https://aka.ms/speech/sdkregion#standard-and-neural-voices
    translation_config.voice_name = "de-DE-Hedda"

    recognizer = speechsdk.translation.TranslationRecognizer(
            translation_config=translation_config)

    def synthesis_callback(evt):
        size = len(evt.result.audio)
        print(f'Audio synthesized: {size} byte(s) {"(COMPLETED)" if size == 0 else ""}')

        if size > 0:
            file = open('translation.wav', 'wb+')
            file.write(evt.result.audio)
            file.close()

    recognizer.synthesizing.connect(synthesis_callback)

    print(f'Say something in "{from_language}" and we\'ll translate into "{to_language}".')

    result = recognizer.recognize_once()
    print(get_result_text(reason=result.reason, result=result))

def get_result_text(reason, result):
    reason_format = {
        speechsdk.ResultReason.TranslatedSpeech:
            f'Recognized "{from_language}": {result.text}\n' +
            f'Translated into "{to_language}"": {result.translations[to_language]}',
        speechsdk.ResultReason.RecognizedSpeech: f'Recognized: "{result.text}"',
        speechsdk.ResultReason.NoMatch: f'No speech could be recognized: {result.no_match_details}',
        speechsdk.ResultReason.Canceled: f'Speech Recognition canceled: {result.cancellation_details}'
    }
    return reason_format.get(reason, 'Unable to recognize speech')

translate_speech_to_text()
```

### <a name="manual-synthesis"></a>Manuel sentez

Sözlük, [`translations`][translations] çeviri metninden ses sentezlemek için kullanılabilir. Her çeviride yineleyin ve çeviriyi sentezleyin. Bir `SpeechSynthesizer` örnek oluştururken, nesnenin `SpeechConfig` [`speech_synthesis_voice_name`][speechsynthesisvoicename] özelliğinin istenilen sese ayarlanması gerekir. Aşağıdaki örnek beş dile çevirir ve her çeviri daha sonra ilgili nöral dilde bir ses dosyasına sentezlenir.

```python
import os
import azure.cognitiveservices.speech as speechsdk

speech_key, service_region = os.environ['SPEECH__SERVICE__KEY'], os.environ['SPEECH__SERVICE__REGION']
from_language, to_languages = 'en-US', [ 'de', 'en', 'it', 'pt', 'zh-Hans' ]

def translate_speech_to_text():
    translation_config = speechsdk.translation.SpeechTranslationConfig(
            subscription=speech_key, region=service_region)

    translation_config.speech_recognition_language = from_language
    for lang in to_languages:
        translation_config.add_target_language(lang)

    recognizer = speechsdk.translation.TranslationRecognizer(
            translation_config=translation_config)
    
    print('Say something...')
    result = recognizer.recognize_once()
    synthesize_translations(result=result)

def synthesize_translations(result):
    language_to_voice_map = {
        "de": "de-DE-KatjaNeural",
        "en": "en-US-AriaNeural",
        "it": "it-IT-ElsaNeural",
        "pt": "pt-BR-FranciscaNeural",
        "zh-Hans": "zh-CN-XiaoxiaoNeural"
    }
    print(f'Recognized: "{result.text}"')

    for language in result.translations:
        translation = result.translations[language]
        print(f'Translated into "{language}": {translation}')

        speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
        speech_config.speech_synthesis_voice_name = language_to_voice_map.get(language)
        
        audio_config = speechsdk.audio.AudioOutputConfig(filename=f'{language}-translation.wav')
        speech_synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config, audio_config=audio_config)
        speech_synthesizer.speak_text_async(translation).get()

translate_speech_to_text()
```

Konuşma sentezi hakkında daha fazla bilgi [için, konuşma sentezinin temellerini](../../../text-to-speech-basics.md)görün.

[config]: https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.translation.speechtranslationconfig?view=azure-python
[audioconfig]: https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audioconfig?view=azure-python
[recognizer]: https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.translation.translationrecognizer?view=azure-python
[recognitionlang]: https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python
[addlang]: https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.translation.speechtranslationconfig?view=azure-python#add-target-language-language--str-
[translations]: https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.translation.translationrecognitionresult?view=azure-python#translations
[voicename]: https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.translation.speechtranslationconfig?view=azure-python#voice-name
[speechsynthesisvoicename]: https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#speech-synthesis-voice-name
