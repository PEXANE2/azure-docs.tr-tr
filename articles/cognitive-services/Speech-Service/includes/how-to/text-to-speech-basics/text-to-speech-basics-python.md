---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2020
ms.author: trbye
ms.openlocfilehash: be60a2f371148fabf73fc7fcdce114295775d71c
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80986257"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede, bir Azure hesabınız ve Konuşma hizmeti aboneliğiniz olduğu varsayar. Hesabınız ve aboneliğiniz yoksa, [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-the-speech-sdk"></a>Konuşma SDK'yı yükleyin

Herhangi bir şey yapmadan önce Konuşma SDK'sını yüklemeniz gerekir.

```Python
pip install azure-cognitiveservices-speech
```

macOS'taysanız ve yükleme sorunlarıyla karşılaştıysanız, önce bu komutu çalıştırmanız gerekebilir.

```Python
python3 -m pip install --upgrade pip
```

Konuşma SDK yüklendikten sonra, komut dosyanızın en üstüne aşağıdaki alma deyimleri ekleyin.

```Python
from azure.cognitiveservices.speech import AudioDataStream, SpeechConfig, SpeechSynthesizer, SpeechSynthesisOutputFormat
from azure.cognitiveservices.speech.audio import AudioOutputConfig
```

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK'sını kullanarak Konuşma hizmetini aramak için [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)bir . Bu sınıf, anahtarınız ve ilişkili bölgeniz, bitiş noktanız, ana bilgisayar veya yetkilendirme belirteciniz gibi aboneliğiniz hakkında bilgiler içerir.

> [!NOTE]
> Konuşma tanıma, konuşma sentezi, çeviri veya niyet tanıma gerçekleştirin, her zaman bir yapılandırma oluşturursunuz.

Bir baş harfe başlatmanın birkaç [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)yolu vardır:

* Abonelikle: bir anahtar ve ilişkili bölgede geçirin.
* Bir bitiş noktası ile: Konuşma hizmeti bitiş noktası geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir ana bilgisayar adresi geçmek. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci yle: yetkilendirme jetonundan ve ilişkili bölgeye geçin.

Bu örnekte, bir [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) abonelik anahtarı ve bölge kullanarak oluşturun. Bölge tanımlayıcınızı bulmak için [bölge destek](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```python
speech_config = SpeechConfig(subscription="YourSubscriptionKey", region="YourServiceRegion")
```

## <a name="synthesize-speech-to-a-file"></a>Konuşmayı bir dosyaya sentezleme

Ardından, konuşmacılara, dosyalara veya diğer çıktı akışlarına metin den konuşmaya dönüşümleri ve çıktıları yürüten bir [`SpeechSynthesizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesizer?view=azure-python) nesne oluşturursunuz. Önceki [`SpeechSynthesizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesizer?view=azure-python) adımda oluşturulan [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) nesneyi ve çıktı sonuçlarının [`AudioOutputConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audiooutputconfig?view=azure-python) nasıl işleneceğini belirten bir nesneyi param olarak kabul eder.

Başlatmak için, `AudioOutputConfig` `filename` oluşturucu param kullanarak `.wav` çıktıyı otomatik olarak bir dosyaya yazmak için bir oluştur.

```python
audio_config = AudioOutputConfig(filename="path/to/write/file.wav")
```

Ardından, nesnenizi `SpeechSynthesizer` `speech_config` ve nesneyi param `audio_config` olarak geçirerek anında bir annızı edin. Daha sonra, konuşma sentezini yürütmek ve bir `speak_text_async()` dosyaya yazma, bir metin dizisiyle çalışmak kadar basittir.

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=audio_config)
synthesizer.speak_text_async("A simple test to write to a file.")
```

Programı çalıştırın ve belirttiğiniz `.wav` konuma sentezlenmiş bir dosya yazılır. Bu, en temel kullanıma iyi bir örnektir, ancak daha sonra çıktıyı özelleştirmeye ve çıktı yanıtını özel senaryolarla çalışmak için bellek içi bir akış olarak işlemeye bakabilirsiniz.

## <a name="synthesize-to-speaker-output"></a>Hoparlör çıkışına sentezleme

Bazı durumlarda, sentezlenmiş konuşmayı doğrudan bir konuşmacıya doğrudan çıktınız isteyebilirsiniz. Bunu yapmak için, önceki bölümdeki örneği kullanın, ancak `filename` paramı kaldırarak `use_default_speaker=True`değiştirin `AudioOutputConfig` ve ayarlayın. Bu, geçerli etkin çıktı aygıtına çıkar.

```python
audio_config = AudioOutputConfig(use_default_speaker=True)
```

## <a name="get-result-as-an-in-memory-stream"></a>Bellek içi akış olarak sonuç alma

Konuşma uygulaması geliştirmedeki birçok senaryo için, büyük olasılıkla bir dosyaya doğrudan yazmak yerine, ortaya çıkan ses verilerine bellek içi akış olarak gereksinim duymanız gerekir. Bu, şunları içeren özel davranış oluşturmanıza olanak sağlar:

* Ortaya çıkan bayt dizisini özel downstream hizmetleri için aranabilir bir akış olarak özetleyin.
* Sonucu diğer API'ler veya hizmetlerle tümleştirin.
* Ses verilerini değiştirin, `.wav` özel üstbilgiler yazın, vb.

Önceki örnekten bu değişikliği yapmak kolaydır. İlk olarak, `AudioConfig`daha fazla denetim için çıktı davranışını bu noktadan itibaren el ile yöneteceğiniz için kaldırın. Sonra `None` `SpeechSynthesizer` yapıcı `AudioConfig` için geçmek. 

> [!NOTE]
> `None` Yukarıdaki hoparlör `AudioConfig`çıktısı örneğinde olduğu gibi atlayarak yerine, geçerli etkin çıkış aygıtında varsayılan olarak sesi oynatmaz.

Bu kez, sonucu bir [`SpeechSynthesisResult`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisresult?view=azure-python) değişkene kaydedin. Özellik `audio_data` çıktı `bytes` verilerinin bir nesnesini içerir. Bu nesneyle el ile çalışabilir veya [`AudioDataStream`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audiodatastream?view=azure-python) bellek içi akışı yönetmek için sınıfı kullanabilirsiniz. Bu örnekte, `AudioDataStream` sonuçtan bir akış almak için oluşturucuyu kullanırsınız.

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)
result = synthesizer.speak_text_async("Getting the response as an in-memory stream.").get()
stream = AudioDataStream(result)
```

Buradan, ortaya `stream` çıkan nesneyi kullanarak herhangi bir özel davranış uygulayabilirsiniz.

## <a name="customize-audio-format"></a>Ses biçimini özelleştirme

Aşağıdaki bölümde, aşağıdakiler de dahil olmak üzere ses çıktısı özniteliklerinin nasıl özelleştirilen gösterilmektedir:

* Ses dosyası türü
* Örnek oranı
* Bit derinliği

Ses biçimini değiştirmek için nesneüzerindeki `set_speech_synthesis_output_format()` `SpeechConfig` işlevi kullanırsınız. Bu işlev, `enum` çıktı [`SpeechSynthesisOutputFormat`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-python)biçimini seçmek için kullandığınız bir tür bekler. Kullanılabilen ses biçimleri listesi için başvuru [dokümanlarına](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-python) bakın.

Gereksinimlerinize bağlı olarak farklı dosya türleri için çeşitli seçenekler vardır. Tanım olarak, gibi `Raw24Khz16BitMonoPcm` ham biçimlerin ses üstbilgilerini içermediğini unutmayın. Ham biçimleri yalnızca alt akış uygulamanızın ham bit akışını çözebileceğini bildiğinizde veya bit derinliğine, örnek hızına, kanal sayısına vb. göre el ile üstbilgi oluşturmayı planlıyorsanız kullanın.

Bu örnekte, `Riff24Khz16BitMonoPcm` `SpeechSynthesisOutputFormat` `SpeechConfig` nesne üzerinde ayarlayarak yüksek doğruluklu BIR RIFF biçimi belirtirsiniz. Önceki bölümdeki örneğe benzer şekilde, sonucun bellek içi akışını almak ve sonra bir dosyaya yazmak için kullanırsınız. [`AudioDataStream`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audiodatastream?view=azure-python)


```python
speech_config.set_speech_synthesis_output_format(SpeechSynthesisOutputFormat["Riff24Khz16BitMonoPcm"])
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)

result = synthesizer.speak_text_async("Customizing audio output format.").get()
stream = AudioDataStream(result)
stream.save_to_wav_file("path/to/write/file.wav")
```

Programınızı yeniden çalıştırmak, belirtilen `.wav` yola özelleştirilmiş bir dosya yazar.

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

Ardından, XML dosyanıza başvurmak için konuşma sentezi isteğini değiştirmeniz gerekir. İstek çoğunlukla aynıdır, ancak `speak_text_async()` işlevi kullanmak yerine. `speak_ssml_async()` Bu işlev bir XML dizesi bekler, bu nedenle ilk olarak SSML config'inizi dize olarak okuyun. Buradan, sonuç nesnesi önceki örneklerle tam olarak aynıdır.

> [!NOTE]
> `ssml_string` Dize `ï»¿` başında içerir, eğer BOM biçimini şerit gerekir veya hizmet bir hata döndürecek. Bunu parametreyi `encoding` aşağıdaki gibi ayarlayarak yaparsınız: `open("ssml.xml", "r", encoding="utf-8-sig")`.

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)

ssml_string = open("ssml.xml", "r").read()
result = synthesizer.speak_ssml_async(ssml_string).get()

stream = AudioDataStream(result)
stream.save_to_wav_file("path/to/write/file.wav")
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
