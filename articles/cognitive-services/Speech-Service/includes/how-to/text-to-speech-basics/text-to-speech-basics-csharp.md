---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2020
ms.author: trbye
ms.openlocfilehash: 8679d6d4c8ff0a6abdf045187c284ca65f43ee7e
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80986264"
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

## <a name="import-dependencies"></a>İthalat bağımlılıkları

Bu makaledeki örnekleri çalıştırmak için, `using` komut dosyanızın en üstünde aşağıdaki ifadeleri ekleyin.

```csharp
using System;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
```

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK'sını kullanarak Konuşma hizmetini aramak için [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)bir . Bu sınıf, anahtarınız ve ilişkili bölgeniz, bitiş noktanız, ana bilgisayar veya yetkilendirme belirteciniz gibi aboneliğiniz hakkında bilgiler içerir.

> [!NOTE]
> Konuşma tanıma, konuşma sentezi, çeviri veya niyet tanıma gerçekleştirin, her zaman bir yapılandırma oluşturursunuz.

Bir baş harfe başlatmanın birkaç [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)yolu vardır:

* Abonelikle: bir anahtar ve ilişkili bölgede geçirin.
* Bir bitiş noktası ile: Konuşma hizmeti bitiş noktası geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir ana bilgisayar adresi geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci yle: yetkilendirme jetonundan ve ilişkili bölgeye geçin.

Bu örnekte, bir [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet) abonelik anahtarı ve bölge kullanarak oluşturun. Bölge tanımlayıcınızı bulmak için [bölge destek](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın. Ayrıca, farklı özelleştirmeler için değiştirdiğiniz bu makalenin geri kalanı için kullanmak üzere bazı temel ortak kodlar da oluşturursunuz.

```csharp
public class Program 
{
    static async Task Main()
    {
        await SynthesizeAudioAsync();
    }

    static async Task SynthesizeAudioAsync() 
    {
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    }
}
```

## <a name="synthesize-speech-to-a-file"></a>Konuşmayı bir dosyaya sentezleme

Ardından, konuşmacılara, dosyalara veya diğer çıktı akışlarına metin den konuşmaya dönüşümleri ve çıktıları yürüten bir [`SpeechSynthesizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesizer?view=azure-dotnet) nesne oluşturursunuz. Önceki [`SpeechSynthesizer`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesizer?view=azure-dotnet) adımda oluşturulan [`SpeechConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet) nesneyi ve çıktı sonuçlarının [`AudioConfig`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audio.audioconfig?view=azure-dotnet) nasıl işleneceğini belirten bir nesneyi param olarak kabul eder.

Başlatmak `AudioConfig` için, `.wav` `FromWavFileOutput()` işlevi kullanarak çıktıyı bir dosyaya otomatik olarak yazmak için bir `using` oluştur ve bir deyimle anında hazırlanın. Bu `using` bağlamdaki bir deyim, yönetilmeyen kaynakları otomatik olarak elden çıkarır ve nesnenin imha dan sonra kapsam dışına çıkmasına neden olur.

```csharp
static async Task SynthesizeAudioAsync() 
{
    var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    using var audioConfig = AudioConfig.FromWavFileOutput("path/to/write/file.wav");
}
```

Sonra, başka `using` bir `SpeechSynthesizer` ifade ile bir instantiate. Nesnenizi `config` ve `audioConfig` nesneyi param olarak geçirin. Daha sonra, konuşma sentezini yürütmek ve bir `SpeakTextAsync()` dosyaya yazma, bir metin dizisiyle çalışmak kadar basittir.

```csharp
static async Task SynthesizeAudioAsync() 
{
    var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    using var audioConfig = AudioConfig.FromWavFileOutput("path/to/write/file.wav");
    using var synthesizer = new SpeechSynthesizer(config, audioConfig);
    await synthesizer.SpeakTextAsync("A simple test to write to a file.");
}
```

Programı çalıştırın ve belirttiğiniz `.wav` konuma sentezlenmiş bir dosya yazılır. Bu, en temel kullanıma iyi bir örnektir, ancak daha sonra çıktıyı özelleştirmeye ve çıktı yanıtını özel senaryolarla çalışmak için bellek içi bir akış olarak işlemeye bakabilirsiniz.

## <a name="synthesize-to-speaker-output"></a>Hoparlör çıkışına sentezleme

Bazı durumlarda, sentezlenmiş konuşmayı doğrudan bir konuşmacıya doğrudan çıktınız isteyebilirsiniz. Bunu yapmak için, yukarıdaki `AudioConfig` `SpeechSynthesizer` örnekte param oluştururken sadece param atlayın. Bu, geçerli etkin çıktı aygıtına çıkar.

```csharp
static async Task SynthesizeAudioAsync() 
{
    var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    using var synthesizer = new SpeechSynthesizer(config);
    await synthesizer.SpeakTextAsync("Synthesizing directly to speaker output.");
}
```

## <a name="get-result-as-an-in-memory-stream"></a>Bellek içi akış olarak sonuç alma

Konuşma uygulaması geliştirmedeki birçok senaryo için, büyük olasılıkla bir dosyaya doğrudan yazmak yerine, ortaya çıkan ses verilerine bellek içi akış olarak gereksinim duymanız gerekir. Bu, şunları içeren özel davranış oluşturmanıza olanak sağlar:

* Ortaya çıkan bayt dizisini özel downstream hizmetleri için aranabilir bir akış olarak özetleyin.
* Sonucu diğer API'ler veya hizmetlerle tümleştirin.
* Ses verilerini değiştirin, `.wav` özel üstbilgiler yazın, vb.

Önceki örnekten bu değişikliği yapmak kolaydır. İlk olarak, `AudioConfig` daha fazla denetim için çıktı davranışını bu noktadan itibaren el ile yöneteceğiniz için bloğu kaldırın. Sonra `null` `SpeechSynthesizer` yapıcı `AudioConfig` için geçmek. 

> [!NOTE]
> `null` Yukarıdaki hoparlör `AudioConfig`çıktısı örneğinde olduğu gibi atlayarak yerine, geçerli etkin çıkış aygıtında varsayılan olarak sesi oynatmaz.

Bu kez, sonucu bir [`SpeechSynthesisResult`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesisresult?view=azure-dotnet) değişkene kaydedin. Özellik `AudioData` çıktı `byte []` verilerinden birini içerir. Bununla `byte []` el ile çalışabilir veya [`AudioDataStream`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audiodatastream?view=azure-dotnet) bellek içi akışı yönetmek için sınıfı kullanabilirsiniz. Bu örnekte, `AudioDataStream.FromResult()` sonuçtan bir akış almak için statik işlevi kullanırsınız.

```csharp
static async Task SynthesizeAudioAsync() 
{
    var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    using var synthesizer = new SpeechSynthesizer(config, null);
    
    var result = await synthesizer.SpeakTextAsync("Getting the response as an in-memory stream.");
    using var stream = AudioDataStream.FromResult(result);
}
```

Buradan, ortaya `stream` çıkan nesneyi kullanarak herhangi bir özel davranış uygulayabilirsiniz.

## <a name="customize-audio-format"></a>Ses biçimini özelleştirme

Aşağıdaki bölümde, aşağıdakiler de dahil olmak üzere ses çıktısı özniteliklerinin nasıl özelleştirilen gösterilmektedir:

* Ses dosyası türü
* Örnek oranı
* Bit derinliği

Ses biçimini değiştirmek için nesneüzerindeki `SetSpeechSynthesisOutputFormat()` `SpeechConfig` işlevi kullanırsınız. Bu işlev, `enum` çıktı [`SpeechSynthesisOutputFormat`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-dotnet)biçimini seçmek için kullandığınız bir tür bekler. Kullanılabilen ses biçimleri listesi için başvuru [dokümanlarına](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-dotnet) bakın.

Gereksinimlerinize bağlı olarak farklı dosya türleri için çeşitli seçenekler vardır. Tanım olarak, gibi `Raw24Khz16BitMonoPcm` ham biçimlerin ses üstbilgilerini içermediğini unutmayın. Ham biçimleri yalnızca alt akış uygulamanızın ham bit akışını çözebileceğini bildiğinizde veya bit derinliğine, örnek hızına, kanal sayısına vb. göre el ile üstbilgi oluşturmayı planlıyorsanız kullanın.

Bu örnekte, `Riff24Khz16BitMonoPcm` `SpeechSynthesisOutputFormat` `SpeechConfig` nesne üzerinde ayarlayarak yüksek doğruluklu BIR RIFF biçimi belirtirsiniz. Önceki bölümdeki örneğe benzer şekilde, sonucun bellek içi akışını almak ve sonra bir dosyaya yazmak için kullanırsınız. [`AudioDataStream`](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.audiodatastream?view=azure-dotnet)

```csharp
static async Task SynthesizeAudioAsync() 
{
    var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    config.SetSpeechSynthesisOutputFormat(SpeechSynthesisOutputFormat.Riff24Khz16BitMonoPcm);

    using var synthesizer = new SpeechSynthesizer(config, null);
    var result = await synthesizer.SpeakTextAsync("Customizing audio output format.");

    using var stream = AudioDataStream.FromResult(result);
    await stream.SaveToWaveFileAsync("path/to/write/file.wav");
}
```

Programınızı yeniden çalıştırmak, `.wav` belirtilen yola bir dosya yazar.

## <a name="use-ssml-to-customize-speech-characteristics"></a>Konuşma özelliklerini özelleştirmek için SSML'yi kullanın

Konuşma Sentezi Biçimlendirme Dili (SSML), bir XML şemasından isteklerinizi göndererek adım, telaffuz, konuşma hızı, ses düzeyi ve daha fazla metin-konuşma çıktısı ince ayar yapmanızı sağlar. Bu bölümde birkaç pratik kullanım örneği gösterilmektedir, ancak daha ayrıntılı bir kılavuz için [SSML nasıl yapılacağını makaleye](../../../speech-synthesis-markup.md)bakın.

Özelleştirme için SSML kullanmaya başlamak için, sesi değiştiren basit bir değişiklik yaparsınız.
İlk olarak, bu örnekte, `ssml.xml`kök proje dizininizde SSML config için yeni bir XML dosyası oluşturun. Kök öğe her `<speak>`zaman ve bir `<voice>` öğedeki metni sarma `name` param kullanarak sesi değiştirmenize olanak sağlar. Bu örnek, sesi erkek bir İngilizce (İngiltere) sesine değiştirir. Bu sesin, **nöral** seslere göre farklı fiyatlandırma ve kullanılabilirlik olan **standart** bir ses olduğunu unutmayın. Desteklenen **standart** seslerin [tam listesine](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#standard-voices) bakın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    When you're on the motorway, it's a good idea to use a sat-nav.
  </voice>
</speak>
```

Ardından, XML dosyanıza başvurmak için konuşma sentezi isteğini değiştirmeniz gerekir. İstek çoğunlukla aynıdır, ancak `SpeakTextAsync()` işlevi kullanmak yerine. `SpeakSsmlAsync()` Bu işlev bir XML dizesi bekler, bu nedenle ilk `File.ReadAllText()`olarak SSML config'inizi bir dize olarak yüklersiniz. Buradan, sonuç nesnesi önceki örneklerle tam olarak aynıdır.

> [!NOTE]
> Visual Studio kullanıyorsanız, yapı config büyük olasılıkla varsayılan olarak XML dosyanızı bulamaz. Bunu düzeltmek için XML dosyasını sağ tıklatın ve **Özellikler'i**seçin. **Yapı Eylemi'ni** *İçerime*değiştirin ve Kopyayı Her *Zaman Kopyalamak* **için Çıktı Dizini olarak** değiştirin.

```csharp
public static async Task SynthesizeAudioAsync() 
{
    var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
    using var synthesizer = new SpeechSynthesizer(config, null);
    
    var ssml = File.ReadAllText("./ssml.xml");
    var result = await synthesizer.SpeakSsmlAsync(ssml);

    using var stream = AudioDataStream.FromResult(result);
    await stream.SaveToWaveFileAsync("path/to/write/file.wav");
}
```

Çıktı çalışır, ancak daha doğal ses yardımcı olmak için yapabileceğiniz birkaç basit ek değişiklikler vardır. Genel konuşma hızı biraz fazla hızlı, bu yüzden `<prosody>` bir etiket ekleyeceğiz ve varsayılan hızın% **90** hızı azaltmak. Ayrıca, cümledeki virgülden sonra duraklama biraz fazla kısa ve doğal olmayan bir sondajdır. Bu sorunu gidermek için, konuşmayı geciktirmek için bir `<break>` etiket ekleyin ve zaman paramini **200ms**olarak ayarlayın. Bu özelleştirmelerin çıktıyı nasıl etkilediğini görmek için sentezi yeniden çalıştırın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    <prosody rate="0.9">
      When you're on the motorway,<break time="200ms"/> it's a good idea to use a sat-nav.
    </prosody>
  </voice>
</speak>
```

## <a name="neural-voices"></a>Sinirsel sesler

Sinirsel sesler derin sinir ağları tarafından desteklenen konuşma sentezi algoritmalarıdır. Sinirsel bir ses kullanırken, sentezlenmiş konuşma insan kayıtlarından neredeyse ayırt edilemez. İnsana benzer doğal prosody ve kelimelerin net eklemleme ile, nöral sesler önemli ölçüde kullanıcıların AI sistemleri ile etkileşim dinleme yorgunluğunu azaltır.

Nöral bir sese `name` geçmek için, [nöral ses seçeneklerinden](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices)birine değiştirin. Ardından, için `mstts`bir XML ad alanı ekleyin `<mstts:express-as>` ve metninizi etikete sarın. Konuşma `style` stilini özelleştirmek için param'ı kullanın. Bu örnek, `cheerful`ancak ayarlamayı `customerservice` `chat` veya konuşma stilindeki farkı görmeyi deneyin.

> [!IMPORTANT]
> Nöral sesler **sadece** *Doğu ABD,* *Güney Doğu Asya*ve Batı *Avrupa* bölgelerinde oluşturulan Konuşma kaynakları için desteklenir.

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="en-US-AriaNeural">
    <mstts:express-as style="cheerful">
      This is awesome!
    </mstts:express-as>
  </voice>
</speak>
```
