---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/13/2020
ms.author: trbye
ms.openlocfilehash: bac2ed447c9055f095e604725591c487378f5091
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399617"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir Azure hesabınız ve Konuşma hizmeti aboneliğiniz olduğu varsayar. Hesabınız ve aboneliğiniz yoksa, [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Herhangi bir şey yapmadan önce Konuşma SDK'sını yüklemeniz gerekir. Platformunuza bağlı olarak, Konuşma <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-sdk#get-the-speech-sdk" target="_blank">SDK makalesinin <span class="docon docon-navigate-external x-hidden-focus"></span> Konuşma SDK bölümünün</a> altındaki talimatları izleyin.

## <a name="import-dependencies"></a>İthalat bağımlılıkları

Bu makaledeki örnekleri çalıştırmak `using` *için, Program.cs* dosyasının üst kısmında aşağıdaki ifadeleri ekleyin.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Translation;
```

## <a name="sensitive-data-and-environment-variables"></a>Hassas veriler ve çevre değişkenleri

Bu makaledeki örnek kaynak kodu, Konuşma kaynağı abonelik anahtarı ve bölge gibi hassas verileri depolamak için ortam değişkenlerine bağlıdır. Sınıf, `Program` ana `static readonly string` bilgisayar ortam değişkenlerinden atanan iki değer `SPEECH__SUBSCRIPTION__KEY` içerir, `SPEECH__SERVICE__REGION`yani. Bu alanların her ikisi de sınıf kapsamı içinde dir ve bu da onları sınıfın yöntem gövdeleri içinde erişilebilir hale getirir. Çevre değişkenleri hakkında daha fazla bilgi için [çevre değişkenlerine ve uygulama yapılandırmalarına](../../../../cognitive-services-security.md#environment-variables-and-application-configuration)bakın.

```csharp
public class Program
{
    static readonly string SPEECH__SUBSCRIPTION__KEY =
        Environment.GetEnvironmentVariable(nameof(SPEECH__SUBSCRIPTION__KEY));
    
    static readonly string SPEECH__SERVICE__REGION =
        Environment.GetEnvironmentVariable(nameof(SPEECH__SERVICE__REGION));

    static Task Main() => Task.CompletedTask;
}
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

```csharp
public class Program
{
    static readonly string SPEECH__SUBSCRIPTION__KEY =
        Environment.GetEnvironmentVariable(nameof(SPEECH__SUBSCRIPTION__KEY));
    
    static readonly string SPEECH__SERVICE__REGION =
        Environment.GetEnvironmentVariable(nameof(SPEECH__SERVICE__REGION));

    static Task Main() => TranslateSpeechAsync();

    static async Task TranslateSpeechAsync()
    {
        var translationConfig =
            SpeechTranslationConfig.FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);
    }
}
```

## <a name="change-source-language"></a>Kaynak dilini değiştirme

Konuşma çevirisinin yaygın bir görevi giriş (veya kaynak) dilini belirtmektir. Giriş dilini İtalyanca'ya nasıl değiştireceğinize bir göz atalım. Kodunuzda, [`SpeechTranslationConfig`][config] özelliğe atama, örnekle `SpeechRecognitionLanguage` etkileşimde bulundu.

```csharp
static async Task TranslateSpeechAsync()
{
    var translationConfig =
        SpeechTranslationConfig.FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    // Source (input) language
    translationConfig.SpeechRecognitionLanguage = "it-IT";
}
```

Özellik [`SpeechRecognitionLanguage`][recognitionlang] bir dil-yerel biçim dizesi bekliyor. Desteklenen [yerel halk/dil](../../../language-support.md)listesindeki **Yerel sütundaki** herhangi bir değer sağlayabilirsiniz.

## <a name="add-translation-language"></a>Çeviri dili ekleme

Konuşma çevirisinin bir diğer yaygın görevi hedef çeviri dillerini belirtmektir, en az bir tane gereklidir, ancak katları desteklenir. Aşağıdaki kod snippet' inde, çeviri dili olarak hem Fransızca hem de Almanca hedefolarak.

```csharp
static async Task TranslateSpeechAsync()
{
    var translationConfig =
        SpeechTranslationConfig.FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    translationConfig.SpeechRecognitionLanguage = "it-IT";
    
    // Translate to languages. See, https://aka.ms/speech/sttt-languages
    translationConfig.AddTargetLanguage("fr");
    translationConfig.AddTargetLanguage("de");
}
```

Her aramada [`AddTargetLanguage`][addlang]yeni bir hedef çeviri dili belirtilir. Başka bir deyişle, konuşma kaynak dilden tanındığı zaman, her hedef çeviri, ortaya çıkan çeviri işleminin bir parçası olarak kullanılabilir.

## <a name="initialize-a-translation-recognizer"></a>Çeviri tanıyıcısını başlatma

Bir , bir [`SpeechTranslationConfig`][config]sonraki adım oluşturduktan sonra [`TranslationRecognizer`][recognizer]bir . Bir [`TranslationRecognizer`][recognizer]harfini başharflediğinizde, bunu geçirmeniz `translationConfig`gerekir. Yapılandırma nesnesi, konuşma hizmetinin isteğinizi doğrulamak için gerektirdiği kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu kullanarak konuşmayı fark ediyorsanız, aşağıdaki [`TranslationRecognizer`][recognizer] gibi görünmelidir:

```csharp
static async Task TranslateSpeechAsync()
{
    var translationConfig =
        SpeechTranslationConfig.FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);

    var fromLanguage = "en-US";
    var toLanguages = new List<string> { "it", "fr", "de" };
    translationConfig.SpeechRecognitionLanguage = fromLanguage;
    toLanguages.ForEach(translationConfig.AddTargetLanguage);

    using var recognizer = new TranslationRecognizer(translationConfig);
}
```

Ses giriş aygıtını belirtmek istiyorsanız, ''ınızı ilke [`AudioConfig`][audioconfig] olarak alırken `audioConfig` bir parametre [`TranslationRecognizer`][recognizer]oluşturmanız ve parametreyi sağlamanız gerekir.

> [!TIP]
> [Ses giriş aygıtınız için aygıt kimliğini nasıl alacağınızı öğrenin.](../../../how-to-select-audio-input-devices.md)

İlk olarak, nesneye `AudioConfig` aşağıdaki gibi başvurursunuz:

```csharp
static async Task TranslateSpeechAsync()
{
    var translationConfig =
        SpeechTranslationConfig.FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);
    
    var fromLanguage = "en-US";
    var toLanguages = new List<string> { "it", "fr", "de" };
    translationConfig.SpeechRecognitionLanguage = fromLanguage;
    toLanguages.ForEach(translationConfig.AddTargetLanguage);

    using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
    using var recognizer = new TranslationRecognizer(translationConfig, audioConfig);
}
```

Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de `audioConfig`bir . Ancak, `FromDefaultMicrophoneInput`aramak yerine [`AudioConfig`][audioconfig]bir , parametre oluşturduğunuzda, `filename` arar `FromWavFileInput` ve parametregeçersiniz.

```csharp
static async Task TranslateSpeechAsync()
{
    var translationConfig =
        SpeechTranslationConfig.FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);
    
    var fromLanguage = "en-US";
    var toLanguages = new List<string> { "it", "fr", "de" };
    translationConfig.SpeechRecognitionLanguage = fromLanguage;
    toLanguages.ForEach(translationConfig.AddTargetLanguage);

    using var audioConfig = AudioConfig.FromWavFileInput("YourAudioFile.wav");
    using var recognizer = new TranslationRecognizer(translationConfig, audioConfig);
}
```

## <a name="translate-speech"></a>Konuşmayı çevirme

Konuşma çevirmek için, Konuşma SDK bir mikrofon veya ses dosyası girişi dayanır. Konuşma tanıma, konuşma çevirisiden önce gerçekleşir. Tüm nesneler baş harfe böldükten sonra tanı-bir işlevi arayın ve sonucu alın.

```csharp
static async Task TranslateSpeechAsync()
{
    var translationConfig =
        SpeechTranslationConfig.FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);
    
    var fromLanguage = "en-US";
    var toLanguages = new List<string> { "it", "fr", "de" };
    translationConfig.SpeechRecognitionLanguage = fromLanguage;
    toLanguages.ForEach(translationConfig.AddTargetLanguage);

    using var recognizer = new TranslationRecognizer(translationConfig);

    Console.Write($"Say something in '{fromLanguage}' and ");
    Console.WriteLine($"we'll translate into '{string.Join("', '", toLanguages)}'.\n");
    
    var result = await recognizer.RecognizeOnceAsync();
    if (result.Reason == ResultReason.TranslatedSpeech)
    {
        Console.WriteLine($"Recognized: \"{result.Text}\":");
        foreach (var (language, translation) in result.Translations)
        {
            Console.WriteLine($"Translated into '{language}': {translation}");
        }
    }
}
```

Konuşmadan metne ilişkin daha fazla bilgi için [konuşma tanımanın temellerini](../../../speech-to-text-basics.md)görün.

## <a name="synthesize-translations"></a>Synthesize çevirileri

Başarılı bir konuşma tanıma ve çeviriden sonra, sonuç sözlükteki tüm çevirileri içerir. Sözlük [`Translations`][translations] anahtarı hedef çeviri dilidir ve değeri çevrilmiş metindir. Tanınan konuşma tercüme edilebilir, sonra farklı bir dilde sentezlenebilir (konuşma-konuşma).

### <a name="event-based-synthesis"></a>Olay tabanlı sentez

Nesne `TranslationRecognizer` bir `Synthesizing` olayı ortaya çıkarır. Olay birkaç kez ateşler ve sentezlenen sesi çeviri tanıma sonucundan almak için bir mekanizma sağlar. Birden çok dile çeviri yorsanız, [bkz.](#manual-synthesis) Bir [`VoiceName`][voicename] atama yaparak sentez sesi belirtin ve olay `Synthesizing` için bir olay işleyicisi sağlayın, ses olsun. Aşağıdaki örnek, çevrilen sesi *.wav* dosyası olarak kaydeder.

> [!IMPORTANT]
> Olay tabanlı sentez yalnızca tek bir çeviri ile çalışır, birden çok hedef çeviri dili **eklemeyin.** Ayrıca, hedef [`VoiceName`][voicename] çeviri dili ile aynı dil olmalıdır, örneğin; `"de"` için `"de-DE-Hedda"`harita olabilir.

```csharp
static async Task TranslateSpeechAsync()
{
    var translationConfig =
        SpeechTranslationConfig.FromSubscription(SPEECH__SUBSCRIPTION__KEY, SPEECH__SERVICE__REGION);
    
    var fromLanguage = "en-US";
    var toLanguage = "de";
    translationConfig.SpeechRecognitionLanguage = fromLanguage;
    translationConfig.AddTargetLanguage(toLanguage);

    // See: https://aka.ms/speech/sdkregion#standard-and-neural-voices
    translationConfig.VoiceName = "de-DE-Hedda";

    using var recognizer = new TranslationRecognizer(translationConfig);

    recognizer.Synthesizing += (_, e) =>
    {
        var audio = e.Result.GetAudio();
        Console.WriteLine($"Audio synthesized: {audio.Length:#,0} byte(s) {(audio.Length == 0 ? "(Complete)" : "")}");

        if (audio.Length > 0)
        {
            File.WriteAllBytes("YourAudioFile.wav", audio);
        }
    };

    Console.Write($"Say something in '{fromLanguage}' and ");
    Console.WriteLine($"we'll translate into '{toLanguage}'.\n");

    var result = await recognizer.RecognizeOnceAsync();
    if (result.Reason == ResultReason.TranslatedSpeech)
    {
        Console.WriteLine($"Recognized: \"{result.Text}\"");
        Console.WriteLine($"Translated into '{toLanguage}': {result.Translations[toLanguage]}");
    }
}
```

### <a name="manual-synthesis"></a>Manuel sentez

Sözlük, [`Translations`][translations] çeviri metninden ses sentezlemek için kullanılabilir. Her çeviride yineleyin ve çeviriyi sentezleyin. Bir `SpeechSynthesizer` örnek oluştururken, nesnenin `SpeechConfig` [`SpeechSynthesisVoiceName`][speechsynthesisvoicename] özelliğinin istenilen sese ayarlanması gerekir. Aşağıdaki örnek beş dile çevirir ve her çeviri daha sonra ilgili nöral dilde bir ses dosyasına sentezlenir.

```csharp
static async Task TranslateSpeechAsync()
{
    var translationConfig =
        SpeechTranslationConfig.FromSubscription(SPEECH__SERVICE__KEY, SPEECH__SERVICE__REGION);

    var fromLanguage = "en-US";
    var toLanguages = new List<string> { "de", "en", "it", "pt", "zh-Hans" };
    translationConfig.SpeechRecognitionLanguage = fromLanguage;
    toLanguages.ForEach(translationConfig.AddTargetLanguage);

    using var recognizer = new TranslationRecognizer(translationConfig);

    Console.Write($"Say something in '{fromLanguage}' and ");
    Console.WriteLine($"we'll translate into '{string.Join("', '", toLanguages)}'.\n");

    var result = await recognizer.RecognizeOnceAsync();
    if (result.Reason == ResultReason.TranslatedSpeech)
    {
        // See: https://aka.ms/speech/sdkregion#standard-and-neural-voices
        var languageToVoiceMap = new Dictionary<string, string>
        {
            ["de"] = "de-DE-KatjaNeural",
            ["en"] = "en-US-AriaNeural",
            ["it"] = "it-IT-ElsaNeural",
            ["pt"] = "pt-BR-FranciscaNeural",
            ["zh-Hans"] = "zh-CN-XiaoxiaoNeural"
        };

        Console.WriteLine($"Recognized: \"{result.Text}\"");

        foreach (var (language, translation) in result.Translations)
        {
            Console.WriteLine($"Translated into '{language}': {translation}");

            var speechConfig =
                SpeechConfig.FromSubscription(
                    SPEECH__SERVICE__KEY, SPEECH__SERVICE__REGION);
            speechConfig.SpeechSynthesisVoiceName = languageToVoiceMap[language];

            using var audioConfig = AudioConfig.FromWavFileOutput($"{language}-translation.wav");
            using var synthesizer = new SpeechSynthesizer(speechConfig, audioConfig);
            
            await synthesizer.SpeakTextAsync(translation);
        }
    }
}
```

Konuşma sentezi hakkında daha fazla bilgi [için, konuşma sentezinin temellerini](../../../text-to-speech-basics.md)görün.

[config]: https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechtranslationconfig?view=azure-dotnet
[audioconfig]: https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-dotnet
[recognizer]: https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.translation.translationrecognizer?view=azure-dotnet
[recognitionlang]: https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.speechrecognitionlanguage?view=azure-dotnet
[addlang]: https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechtranslationconfig.addtargetlanguage?view=azure-dotnet
[translations]: https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.translation.translationrecognitionresult.translations?view=azure-dotnet
[voicename]: https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechtranslationconfig.voicename?view=azure-dotnet
[speechsynthesisvoicename]: https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.speechsynthesisvoicename?view=azure-dotnet
