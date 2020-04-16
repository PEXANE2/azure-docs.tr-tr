---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: 982c3c6011936c184c55dd92a76d4aec023baaf6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399788"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir Azure hesabınız ve Konuşma hizmeti aboneliğiniz olduğu varsayar. Hesabınız ve aboneliğiniz yoksa, [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Herhangi bir şey yapmadan önce Konuşma SDK'sını yüklemeniz gerekir. Platformunuza bağlı olarak aşağıdaki yönergeleri kullanın:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=dotnet&pivots=programming-language-csharp" target="_blank">.NET Çerçevesi<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=dotnetcore&pivots=programming-language-csharp" target="_blank">.NET Çekirdek<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=unity&pivots=programming-language-csharp" target="_blank">Birlik<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=uwps&pivots=programming-language-csharp" target="_blank">UWP<span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=xaml&pivots=programming-language-csharp" target="_blank">Xamarin<span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK'sını kullanarak Konuşma hizmetini aramak için [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)bir . Bu sınıf, anahtarınız ve ilişkili bölgeniz, bitiş noktanız, ana bilgisayar veya yetkilendirme belirteciniz gibi aboneliğiniz hakkında bilgiler içerir.

> [!NOTE]
> Konuşma tanıma, konuşma sentezi, çeviri veya niyet tanıma gerçekleştirin, her zaman bir yapılandırma oluşturursunuz.

Bir baş harfe başlatmanın birkaç [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)yolu vardır:

* Abonelikle: bir anahtar ve ilişkili bölgede geçirin.
* Bir bitiş noktası ile: Konuşma hizmeti bitiş noktası geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir ana bilgisayar adresi geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci yle: yetkilendirme jetonundan ve ilişkili bölgeye geçin.

Bir anahtar ve bölge kullanılarak [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet) nasıl oluşturulduğuna bir göz atalım. Bölge tanımlayıcınızı bulmak için [bölge destek](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```csharp
var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

## <a name="initialize-a-recognizer"></a>Tanıyıcıyı başlatma

Bir , bir [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)sonraki adım oluşturduktan sonra [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)bir . Bir [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)harfini başharflediğinizde, bunu geçirmeniz `speechConfig`gerekir. Bu, konuşma hizmetinin isteğinizi doğrulamak için gerektirdiği kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu kullanarak konuşmayı fark ediyorsanız, aşağıdaki [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet) gibi görünmelidir:

```csharp
using var recognizer = new SpeechRecognizer(speechConfig);
```

Ses giriş aygıtını belirtmek istiyorsanız, ''ınızı ilke [`AudioConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-dotnet) olarak alırken `audioConfig` bir parametre [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)oluşturmanız ve parametreyi sağlamanız gerekir.

> [!TIP]
> [Ses giriş aygıtınız için aygıt kimliğini nasıl alacağınızı öğrenin.](../../../how-to-select-audio-input-devices.md)

İlk olarak, `using` aşağıdaki ifadeyi ekleyin.

```csharp
using Microsoft.CognitiveServices.Speech.Audio;
```

Ardından, `AudioConfig` nesneye aşağıdaki gibi başvuruda bulunabilirsiniz:

```csharp
using var audioConfig = AudioConfig.FromDefaultMicrophoneInput();
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```

Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de `audioConfig`bir . Ancak, `FromDefaultMicrophoneInput`aramak yerine [`AudioConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-dotnet)bir , parametre oluşturduğunuzda, `filename` arar `FromWavFileOutput` ve parametregeçersiniz.


```csharp
using var audioConfig = AudioConfig.FromWavFileInput("YourAudioFile.wav");
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```

## <a name="recognize-speech"></a>Konuşma tanıma

C# için Konuşma SDK'sının [Recognizer sınıfı,](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet) konuşma tanıma için kullanabileceğiniz birkaç yöntemi ortaya çıkarır.

* Tek çekim tanıma (async) - Engellemesiz (asynchronous) modda tanıma gerçekleştirir. Bu tek bir söyleyiş tanıyacaktır. Tek bir söyleyninin sonu, sonunda sessizlik dinleyerek veya en fazla 15 saniyelik ses işlenene kadar belirlenir.
* Sürekli tanıma (async) - Asynchronously sürekli tanıma işlemi başlatır. Kullanıcı olaylara kaydolur ve çeşitli uygulama durumunu işler. Eşzamanlı sürekli tanımayı durdurmak için. [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet)

> [!NOTE]
> [Konuşma tanıma modunu](../../../how-to-choose-recognition-mode.md)nasıl seçeceğiniz hakkında daha fazla bilgi edinin.

### <a name="single-shot-recognition"></a>Tek çekim tanıma

Burada kullanarak [`RecognizeOnceAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizeonceasync?view=azure-dotnet)eşzamanlı tek çekim tanıma bir örnek:

```csharp
var result = await recognizer.RecognizeOnceAsync();
```

Sonucu işlemek için bazı kod yazmanız gerekir. Bu örnek şu [`result.Reason`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.recognitionresult.reason?view=azure-dotnet)şekilde değerlendirilir:

* Tanıma sonucunu yazdırır:`ResultReason.RecognizedSpeech`
* Tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin:`ResultReason.NoMatch`
* Bir hatayla karşılaşılırsa, hata iletisini yazdırın:`ResultReason.Canceled`

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

Sürekli tanıma, tek çekimtanımadan biraz daha fazla ilgilidir. Tanıma sonuçlarını almak için `Recognizing` `Recognized`, `Canceled` ve olaylara abone olmak için gerekli. Tanınmayı durdurmak [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet)için. Aşağıda, ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirildiğine ilişkin bir örnek verilmiştir.

Girişi tanımlayarak ve bir [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet)baş harfe başlayalım:

```csharp
using var audioConfig = AudioConfig.FromWavFileInput("YourAudioFile.wav");
using var recognizer = new SpeechRecognizer(speechConfig, audioConfig);
```
Sonra, konuşma tanıma durumunu yönetmek için bir değişken oluşturalım. Başlangıç olarak, önceki bildirimlerden sonra bir `TaskCompletionSource<int>` bildirimde olacağız.

```csharp
var stopRecognition = new TaskCompletionSource<int>();
```

Biz gönderilen olaylara abone olacağız [`SpeechRecognizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer?view=azure-dotnet).

* [`Recognizing`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognizing?view=azure-dotnet): Ara tanıma sonuçlarını içeren olaylar için sinyal.
* [`Recognized`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.recognized?view=azure-dotnet): Nihai tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi gösteren).
* [`SessionStopped`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.recognizer.sessionstopped?view=azure-dotnet): Tanıma oturumunun (operasyonun) sona erini gösteren olaylar için sinyal.
* [`Canceled`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.canceled?view=azure-dotnet): İptal edilmiş tanıma sonuçlarını içeren olaylar için sinyal (sonuç olarak iptal edilen bir tanıma girişimi veya doğrudan iptal isteği veya alternatif olarak bir aktarım veya protokol hatası) gösterir.

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

Her şey ayarlandırıtamam, arayabiliyoruz. [`StopContinuousRecognitionAsync`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechrecognizer.stopcontinuousrecognitionasync?view=azure-dotnet)

```csharp
// Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
await recognizer.StartContinuousRecognitionAsync();

// Waits for completion. Use Task.WaitAny to keep the task rooted.
Task.WaitAny(new[] { stopRecognition.Task });

// Stops recognition.
await recognizer.StopContinuousRecognitionAsync();
```

### <a name="dictation-mode"></a>Dikte modu

Sürekli tanıma kullanırken, ilgili "dikte işlemini etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, noktalama işaretleri gibi tümce yapılarının sözcük açıklamalarını yorumlamak için konuşma config örneğine neden olur. Örneğin, "Şehirde mi yaşıyorsunuz soru işareti" sözü "Şehirde mi yaşıyorsunuz?" şeklinde yorumlanır.

Dikte modunu etkinleştirmek için, [`EnableDictation`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.enabledictation?view=azure-dotnet) yöntemi [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)nikullanarak.

```csharp
speechConfig.EnableDictation();
```

## <a name="change-source-language"></a>Kaynak dilini değiştirme

Konuşma tanıma için ortak bir görev giriş (veya kaynak) dilini belirtmektir. Giriş dilini İtalyanca'ya nasıl değiştireceğinize bir göz atalım. Kodunuzda, [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)bu satırı doğrudan altına ekleyin.

```csharp
speechConfig.SpeechRecognitionLanguage = "it-IT";
```

Özellik [`SpeechRecognitionLanguage`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.speechrecognitionlanguage?view=azure-dotnet) bir dil-yerel biçim dizesi bekliyor. Desteklenen [yerel halk/dil](../../../language-support.md)listesindeki **Yerel sütundaki** herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu artırın

Konuşma SDK ile tanıma doğruluğunu artırmanın birkaç yolu vardır. İfade Listeleri'ne bir göz atalım. Tümcecik Listeleri, ses verilerindeki bilinen ifadeleri tanımlamak için kullanılır, örneğin bir kişinin adı veya belirli bir konum. Tümcecik Listesine tek sözcük veya tam tümcecik eklenebilir. Tanıma sırasında, sese tüm tümcecik için tam bir eşleşme varsa, bir ifade listesindeki bir giriş kullanılır. İfadeyle tam eşleşme bulunamazsa, tanıma yardımcı olmaz.

> [!IMPORTANT]
> İfade Listesi özelliği yalnızca İngilizce olarak kullanılabilir.

Bir tümcecik listesini kullanmak [`PhraseListGrammar`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-dotnet) için önce bir nesne oluşturun, sonra [`AddPhrase`](https://docs.microsoft.com//dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar.addphrase?view=azure-dotnet)belirli sözcükler ve tümcecikler ekleyin.

Bir sonraki [`PhraseListGrammar`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.phraselistgrammar?view=azure-dotnet) tanıma da veya Konuşma hizmetine yeniden bağlantı dan sonra etkili olacak değişiklikler.

```csharp
var phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Supercalifragilisticexpialidocious");
```

İfade listenizi temizlemeniz gerekiyorsa: 

```csharp
phraseList.Clear();
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../how-to-custom-speech.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)