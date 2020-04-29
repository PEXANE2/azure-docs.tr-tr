---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: 982c3c6011936c184c55dd92a76d4aec023baaf6
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81399788"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir. Platformunuza bağlı olarak, aşağıdaki yönergeleri kullanın:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=dotnet&pivots=programming-language-csharp" target="_blank">.NET Framework<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=dotnetcore&pivots=programming-language-csharp" target="_blank">.NET Core<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=unity&pivots=programming-language-csharp" target="_blank">'Yi<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=uwps&pivots=programming-language-csharp" target="_blank">UWP<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=xaml&pivots=programming-language-csharp" target="_blank">Xamarin<span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)oluşturmanız gerekir. Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir.

> [!NOTE]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

[`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)Şunları başlatabilmeniz için birkaç yol vardır:

* Abonelik ile: bir anahtarı ve ilişkili bölgeyi geçirin.
* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

Bir [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet) anahtar ve bölge kullanarak nasıl oluşturulduğuna göz atalım. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```csharp
var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Tanıyıcı başlatma

Bir [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)oluşturduktan sonra, bir sonraki adım bir [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)' ı başlatmaktır. Bir [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)başlattığınızda, bunu geçirmeniz gerekir `speechConfig`. Bu, konuşma hizmetinin isteğinizi doğrulamak için ihtiyaç duyduğu kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet) kullanarak konuşmayı tanıyor olmanız halinde şöyle görünmelidir:

```csharp
using var recognizer = new SpeechRecognizer(speechConfig);
```

Ses giriş cihazını belirtmek isterseniz, oluşturmanız [`AudioConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-dotnet) ve ' ı başlatırken `audioConfig` parametresini sağlamanız gerekir. [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)

> [!TIP]
> [Ses giriş cihazınız için CIHAZ kimliğini nasıl alabileceğinizi öğrenin](../../../how-to-select-audio-input-devices.md).

İlk olarak, aşağıdaki `using` ifadeyi ekleyin.

```csharp
using Microsoft.CognitiveServices.Speech.Audio;
```

Daha sonra, `AudioConfig` nesnesine aşağıdaki gibi başvurabilirsiniz:

```csharp
using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```

Mikrofon kullanmak yerine bir ses dosyası sağlamak istiyorsanız, yine de sağlamanız gerekir `audioConfig`. Ancak [`AudioConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-dotnet), öğesini çağırmak `FromDefaultMicrophoneInput`yerine bir oluşturduğunuzda, `FromWavFileOutput` `filename` parametresini çağırır ve geçireceğiz.


```csharp
using var audioConfig = AudioConfig.FromWavFileInput("YourAudioFile.wav");
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```

## <a name="recognize-speech"></a>Konuşma tanıma

C# için konuşma SDK 'Sı için [tanıyıcı sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet) , konuşma tanıma için kullanabileceğiniz birkaç yöntem sunar.

* Tek atışı tanıma (Async)-engelleyici olmayan (zaman uyumsuz) modda tanıma gerçekleştirir. Bu, tek bir utterance algılar. Tek bir utterüance 'in sonunda, sonda sessizlik dinlemesi veya en fazla 15 saniyelik ses işlenene kadar belirlenir.
* Sürekli tanıma (Async)-zaman uyumsuz olarak sürekli tanıma işlemini başlatır. Kullanıcı olaylara kaydolur ve çeşitli uygulama durumlarını işler. Zaman uyumsuz sürekli tanımayı durdurmak için çağrısı [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet)yapın.

> [!NOTE]
> [Konuşma tanıma modunu seçme](../../../how-to-choose-recognition-mode.md)hakkında daha fazla bilgi edinin.

### <a name="single-shot-recognition"></a>Tek atışı tanıma

Şu kullanılarak [`RecognizeOnceAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-dotnet)zaman uyumsuz tek kararlı tanıma örneği aşağıda verilmiştir:

```csharp
var result = await recognizer.RecognizeOnceAsync();
```

Sonucu işlemek için bazı kodlar yazmanız gerekir. Bu örnek [`result.Reason`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.recognitionresult.reason?view=azure-dotnet)şunları değerlendirir:

* Tanınma sonucunu yazdırır:`ResultReason.RecognizedSpeech`
* Bir tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin:`ResultReason.NoMatch`
* Bir hata ile karşılaşırsanız, hata iletisini yazdırın:`ResultReason.Canceled`

```csharp
switch (result.Reason)
{
    case ResultReason.RecognizedSpeech:
        Console.WriteLine($"RECOGNIZED: Text={result.Text}");
        Console.WriteLine($"    Intent not recognized.");
        break;
    case ResultReason.NoMatch:
        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
        break;
    case ResultReason.Canceled:
        var cancellation = CancellationDetails.FromResult(result);
        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

        if (cancellation.Reason == CancellationReason.Error)
        {
            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
            Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
            Console.WriteLine($"CANCELED: Did you update the subscription info?");
        }
        break;
}
```

### <a name="continuous-recognition"></a>Sürekli tanıma

Sürekli tanıma, tek kararlı tanıma göre biraz daha karmaşıktır. Tanıma sonuçlarını almak için `Recognizing`, `Recognized`, ve `Canceled` olaylarına abone olmanızı gerektirir. Tanımayı durdurmak için çağrısı [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet)yapmanız gerekir. İşte, bir ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirilebileceğini gösteren bir örnek.

Girişi tanımlayarak ve şunu başlatarak başlayalım [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet):

```csharp
using var audioConfig = AudioConfig.FromWavFileInput("YourAudioFile.wav");
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```
Sonra, konuşma tanımanın durumunu yönetmek için bir değişken oluşturalım. Başlamak için, önceki bildirimlerden sonra `TaskCompletionSource<int>` bir bildirir.

```csharp
var stopRecognition = new TaskCompletionSource<int>();
```

Kaynağından gönderilen olaylara abone edeceğiz [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet).

* [`Recognizing`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizing?view=azure-dotnet): Ara tanıma sonuçları içeren olaylar için sinyal.
* [`Recognized`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognized?view=azure-dotnet): Son tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi olduğunu gösterir).
* [`SessionStopped`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.recognizer.sessionstopped?view=azure-dotnet): Bir tanıma oturumunun (işlem) sonunu gösteren olaylar için sinyal.
* [`Canceled`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.canceled?view=azure-dotnet): İptal edilen tanıma sonuçlarını içeren olaylar için sinyal (sonuç veya doğrudan iptal isteği olarak iptal edilen bir tanıma girişimi veya ya da bir aktarım ya da protokol arızası).

```csharp
recognizer.Recognizing += (s, e) =>
{
    Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
};

recognizer.Recognized += (s, e) =>
{
    if (e.Result.Reason == ResultReason.RecognizedSpeech)
    {
        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}");
    }
    else if (e.Result.Reason == ResultReason.NoMatch)
    {
        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
    }
};

recognizer.Canceled += (s, e) =>
{
    Console.WriteLine($"CANCELED: Reason={e.Reason}");

    if (e.Reason == CancellationReason.Error)
    {
        Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
        Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
        Console.WriteLine($"CANCELED: Did you update the subscription info?");
    }

    stopRecognition.TrySetResult(0);
};

recognizer.SessionStopped += (s, e) =>
{
    Console.WriteLine("\n    Session stopped event.");
    stopRecognition.TrySetResult(0);
};
```

Her şey ayarlandığında, çağırabiliriz [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet).

```csharp
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
await recognizer.StartContinuousRecognitionAsync();

// Waits for completion. Use Task.WaitAny to keep the task rooted.
Task.WaitAny(new[] { stopRecognition.Task });

// Stops recognition.
await recognizer.StopContinuousRecognitionAsync();
```

### <a name="dictation-mode"></a>Dikte etme modu

Sürekli tanıma kullanırken, ilgili "dikte etmeyi etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, konuşma yapılandırma örneğinin noktalama gibi tümce yapılarının sözcük açıklamalarını yorumlamasını sağlar. Örneğin, "kasadaki gerçek zamanlı olarak", "kasadaki canlı mısınız?" metni olarak yorumlanabilir.

Dikte modunu etkinleştirmek için, içindeki [`EnableDictation`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-dotnet) yöntemi kullanın. [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)

```csharp
speechConfig.EnableDictation();
```

## <a name="change-source-language"></a>Kaynak dilini değiştir

Konuşma tanıma için ortak bir görev, giriş (veya kaynak) dilini belirtmektir. Giriş dilini Italyanca olarak nasıl değiştirebileceğinizi göz atalım. Kodunuzda [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet), daha sonra bu satırı hemen altına ekleyin.

```csharp
speechConfig.SpeechRecognitionLanguage = "it-IT";
```

Özelliği [`SpeechRecognitionLanguage`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.speechrecognitionlanguage?view=azure-dotnet) , bir dil yerel ayar dizesi bekliyor. Desteklenen [yerel ayarlar/diller](../../../language-support.md)listesindeki **yerel ayar** sütununda herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu geliştirme

Konuşma SDK 'Sı ile tanıma doğruluğunu geliştirmenin birkaç yolu vardır. Tümcecik listelerine göz atalım. Tümcecik listeleri, bir kişinin adı veya belirli bir konum gibi, ses verilerinde bilinen tümcecikleri belirlemek için kullanılır. Tek sözcükler veya bütün ifadeler, bir tümcecik listesine eklenebilir. Tanıma sırasında, tüm tümcecik için tam eşleşme sese dahil olursa tümcecik listesindeki bir giriş kullanılır. Tümcecikle tam eşleşme bulunamazsa, tanıma yardımlı değildir.

> [!IMPORTANT]
> Tümcecik listesi özelliği yalnızca Ingilizce olarak kullanılabilir.

Bir tümcecik listesi kullanmak için, önce bir [`PhraseListGrammar`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-dotnet) nesne oluşturun, ardından ile [`AddPhrase`](https://docs.microsoft.com//dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar.addphrase?view=azure-dotnet)belirli sözcükler ve deyimler ekleyin.

Herhangi bir değişiklik [`PhraseListGrammar`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-dotnet) , bir sonraki tanıma göre veya konuşma hizmetine yeniden bağlanmaya sonra devreye girer.

```csharp
var phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Supercalifragilisticexpialidocious");
```

Tümcecik listenizi temizlemeniz gerekirse: 

```csharp
phraseList.Clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../how-to-custom-speech.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)