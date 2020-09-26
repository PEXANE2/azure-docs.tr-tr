---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2020
ms.author: trbye
ms.openlocfilehash: f169e45b126f064c5bae315b9dcd30a39c830fdd
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91332480"
---
Bu hızlı başlangıçta, konuşma SDK 'sını kullanarak metinden konuşmaya senşlerini kullanmaya yönelik yaygın tasarım düzenlerini öğrenirsiniz. Temel yapılandırma ve birleştirme işlemleri gerçekleştirerek başlar ve aşağıdakiler de dahil olmak üzere özel uygulama geliştirme için daha gelişmiş örneklere geçin:

* Bellek içi akışlar olarak yanıt alma
* Çıkış örneği hızını ve bit hızını özelleştirme
* SSML kullanarak senssıs istekleri gönderme (konuşma birleştirme biçimlendirme dili)
* Sinir seslerini kullanma

## <a name="skip-to-samples-on-github"></a>GitHub 'da örneklere atlayın

Örnek koda doğrudan atlamak istiyorsanız GitHub 'daki [Python hızlı başlangıç örneklerine](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/python/text-to-speech) bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir.

```Python
pip install azure-cognitiveservices-speech
```

MacOS 'ta çalışıyorsanız ve yüklemeyi sorunları yaşıyorsanız, önce bu komutu çalıştırmanız gerekebilir.

```Python
python3 -m pip install --upgrade pip
```

Konuşma SDK 'Sı yüklendikten sonra, betiğinizin en üstüne aşağıdaki içeri aktarma deyimlerini ekleyin.

```Python
from azure.cognitiveservices.speech import AudioDataStream, SpeechConfig, SpeechSynthesizer, SpeechSynthesisOutputFormat
from azure.cognitiveservices.speech.audio import AudioOutputConfig
```

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir oluşturmanız gerekir [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python&preserve-view=true) . Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir.

> [!NOTE]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

Şunları başlatabilmeniz için birkaç yol vardır [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python&preserve-view=true) :

* Abonelik ile: bir anahtarı ve ilişkili bölgeyi geçirin.
* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

Bu örnekte, bir [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python&preserve-view=true) abonelik anahtarı ve bölgesi kullanarak bir oluşturun. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```python
speech_config = SpeechConfig(subscription="YourSubscriptionKey", region="YourServiceRegion")
```

## <a name="synthesize-speech-to-a-file"></a>Konuşmayı bir dosyaya sentezleştirme

Sonra, [`SpeechSynthesizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesizer?view=azure-python&preserve-view=true) metin okuma dönüştürmeleri yürüten ve hoparlörlerde, dosyalarda veya diğer çıkış akışlarına giden çıktıları yürüten bir nesne oluşturun. , [`SpeechSynthesizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesizer?view=azure-python&preserve-view=true) [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python&preserve-view=true) Önceki adımda oluşturulan nesneyi params olarak kabul eder ve [`AudioOutputConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audiooutputconfig?view=azure-python&preserve-view=true) Çıkış sonuçlarının nasıl işleneceğini belirten bir nesnedir.

Başlamak için, `AudioOutputConfig` `.wav` Oluşturucu param ' ı kullanarak çıktıyı otomatik olarak bir dosyaya yazmak üzere bir oluşturun `filename` .

```python
audio_config = AudioOutputConfig(filename="path/to/write/file.wav")
```

Sonra, `SpeechSynthesizer` `speech_config` nesneyi ve nesneyi params olarak geçirerek bir örneğini oluşturun `audio_config` . Daha sonra, konuşma birleştirmenin yürütülmesi ve bir dosyaya yazılması, `speak_text_async()` bir metin dizesiyle çalışırken basittir.

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=audio_config)
synthesizer.speak_text_async("A simple test to write to a file.")
```

Programı çalıştırın ve belirttiğiniz konuma bir sentezlenmiş `.wav` dosya yazılır. Bu, en temel kullanımın iyi bir örneğidir, ancak sonraki bir deyişle, özel senaryolarla çalışmaya yönelik bellek içi akış olarak çıktıyı özelleştirmeye ve çıkış yanıtını işlemeye baktığınızın ardından.

## <a name="synthesize-to-speaker-output"></a>Konuşmacı çıktısına sentezleştirme

Bazı durumlarda, doğrudan sentezlenmiş konuşmayı doğrudan bir konuşmacının çıktısını almak isteyebilirsiniz. Bunu yapmak için, önceki bölümdeki örneği kullanın, ancak `AudioOutputConfig` `filename` param ' ı kaldırarak öğesini değiştirin ve ayarlayın `use_default_speaker=True` . Bu, geçerli etkin çıkış cihazına çıkış verir.

```python
audio_config = AudioOutputConfig(use_default_speaker=True)
```

## <a name="get-result-as-an-in-memory-stream"></a>Bellek içi akış olarak sonuç alın

Konuşma uygulaması geliştirmede birçok senaryo için, büyük olasılıkla bir dosyaya doğrudan yazmak yerine, sonuçta elde edilen ses verilerinin bellek içi akış olarak olması gerekir. Bu, aşağıdakiler dahil olmak üzere özel davranış oluşturmanıza izin verir:

* Özel aşağı akış Hizmetleri için arama yapabilecek bir akış olarak ortaya çıkan bayt dizisinin soyut olması.
* Sonucu diğer API 'leri veya hizmetleriyle tümleştirin.
* Ses verilerini değiştirin, özel üstbilgiler yazın `.wav` vb.

Önceki örnekte bu değişikliği yapmak basittir. İlk `AudioConfig` olarak, daha fazla denetim için çıkış davranışını bu noktadan el ile yöneteceği için öğesini kaldırın. Sonra `None` `AudioConfig` Oluşturucu içinde öğesine geçirin `SpeechSynthesizer` . 

> [!NOTE]
> `None` `AudioConfig` Yukarıdaki konuşmacı çıktısı örneğinde olduğu gibi değil, için geçirme, geçerli etkin çıkış cihazında varsayılan olarak sesi oynamaz.

Bu kez, sonucu bir [`SpeechSynthesisResult`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisresult?view=azure-python&preserve-view=true) değişkene kaydedersiniz. `audio_data`Özelliği, `bytes` Çıkış verilerinin bir nesnesini içerir. Bu nesneyle el ile çalışabilirsiniz veya [`AudioDataStream`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audiodatastream?view=azure-python&preserve-view=true) bellek içi akışı yönetmek için sınıfını kullanabilirsiniz. Bu örnekte, `AudioDataStream` sonuçtan bir akış almak için oluşturucuyu kullanırsınız.

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)
result = synthesizer.speak_text_async("Getting the response as an in-memory stream.").get()
stream = AudioDataStream(result)
```

Buradan, sonuçta elde edilen nesneyi kullanarak özel davranışları uygulayabilirsiniz `stream` .

## <a name="customize-audio-format"></a>Ses biçimini Özelleştir

Aşağıdaki bölümde aşağıdakiler dahil olmak üzere ses çıkış özniteliklerinin nasıl özelleştirileceği gösterilmektedir:

* Ses dosyası türü
* Örnek hız
* Bit derinliği

Ses biçimini değiştirmek için `set_speech_synthesis_output_format()` nesnesi üzerinde işlevini kullanın `SpeechConfig` . Bu işlev `enum` [`SpeechSynthesisOutputFormat`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-python&preserve-view=true) , çıkış biçimini seçmek için kullandığınız türünde bir tür bekliyor. Kullanılabilir [Ses biçimlerinin listesi](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-python&preserve-view=true) için başvuru belgelerine bakın.

Gereksinimlerinize bağlı olarak farklı dosya türleri için çeşitli seçenekler vardır. Tanımına göre, gibi ham biçimlerin `Raw24Khz16BitMonoPcm` Ses üst bilgilerini içermediğini unutmayın. Ham biçimleri yalnızca aşağı akış uygulamanızın bir ham Bitstream kodunu çözemediğini veya bit derinliğine, örnek hızına, kanal sayısına ve vb. göre el ile üstbilgileri oluşturmayı planlıyorsanız kullanın.

Bu örnekte, `Riff24Khz16BitMonoPcm` nesnesi üzerinde ayarını yaparak Yüksek uygunluğa sahip bir biçim belirtirsiniz `SpeechSynthesisOutputFormat` `SpeechConfig` . Önceki bölümdeki örneğe benzer şekilde, [`AudioDataStream`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audiodatastream?view=azure-python&preserve-view=true) sonucun bellek içi akışını elde etmek ve ardından bir dosyaya yazmak için öğesini kullanırsınız.


```python
speech_config.set_speech_synthesis_output_format(SpeechSynthesisOutputFormat["Riff24Khz16BitMonoPcm"])
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)

result = synthesizer.speak_text_async("Customizing audio output format.").get()
stream = AudioDataStream(result)
stream.save_to_wav_file("path/to/write/file.wav")
```

Programınızı yeniden çalıştırmak, belirtilen yola özelleştirilmiş bir `.wav` Dosya yazacak.

## <a name="use-ssml-to-customize-speech-characteristics"></a>SSML 'yi kullanarak konuşma özelliklerini özelleştirme

Konuşma sen, biçimlendirme dili (SSML), isteklerinizi bir XML şemasından göndererek, metin okuma çıktılarından oluşan sıklık, Söyleniş, konuşma oranı, hacim ve daha fazlasını ayarlamanıza olanak sağlar. Bu bölümde, birkaç pratik kullanım örneği gösterilmektedir, ancak daha ayrıntılı bir kılavuz için bkz. [SSML nasıl yapılır makalesi](../../../speech-synthesis-markup.md).

Özelleştirme için SSML 'yi kullanmaya başlamak için, sese geçiş yapan basit bir değişiklik yaparsınız.
İlk olarak, bu örnekte, kök proje dizininizde SSML yapılandırması için yeni bir XML dosyası oluşturun `ssml.xml` . Kök öğesi her zaman `<speak>` ve metni bir öğe içinde sarmalama, `<voice>` param kullanarak sesi değiştirmenize olanak sağlar `name` . Bu örnek, sesi erkek Ingilizce (UK) sesine dönüştürür. Bu sesin, **sinir** sesinden farklı fiyatlandırmaya ve kullanılabilirliğine sahip **Standart** bir ses olduğunu unutmayın. Desteklenen **Standart** seslerin [tam listesine](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#standard-voices) bakın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    When you're on the motorway, it's a good idea to use a sat-nav.
  </voice>
</speak>
```

Ardından, XML dosyanıza başvurmak için konuşma sensıs isteğini değiştirmeniz gerekir. İstek genellikle aynıdır, ancak işlevini kullanmak yerine `speak_text_async()` kullanın `speak_ssml_async()` . Bu işlev bir XML dizesi bekliyor, bu nedenle ilk olarak SSML yapılandırmasını bir dize olarak okumalısınız. Buradan, sonuç nesnesi önceki örneklerle tamamen aynıdır.

> [!NOTE]
> `ssml_string` `ï»¿` Dizenin başlangıcında IÇERIYORSA, bom biçimini çıkarmanız gerekir, aksi takdirde hizmet bir hata döndürür. Bunu, `encoding` parametresini aşağıdaki gibi ayarlayarak yapabilirsiniz: `open("ssml.xml", "r", encoding="utf-8-sig")` .

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)

ssml_string = open("ssml.xml", "r").read()
result = synthesizer.speak_ssml_async(ssml_string).get()

stream = AudioDataStream(result)
stream.save_to_wav_file("path/to/write/file.wav")
```

Çıktı işe yarar, ancak daha doğal bir şekilde ses duymayı sağlayacak birkaç basit ek değişiklik vardır. Genel konuşma hızı çok hızlıdır, bu nedenle bir `<prosody>` etiket ekleyeceğiz ve varsayılan orandaki **%90** ' a kadar olan hızı azaltabilirsiniz. Ayrıca, tümcede virgülden sonraki duraklama, çok kısa ve doğal olmayan bir soun. Bu sorunu onarmak için `<break>` konuşmayı geciktirmek üzere bir etiket ekleyin ve zaman param ' ı **200ms**olarak ayarlayın. Bu özelleştirmelerin çıktıyı nasıl etkilediğine bakmak için senmu yeniden çalıştırın.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    <prosody rate="0.9">
      When you're on the motorway,<break time="200ms"/> it's a good idea to use a sat-nav.
    </prosody>
  </voice>
</speak>
```

## <a name="neural-voices"></a>Sinir sesleri

Sinir sesleri, derin sinir Networks tarafından desteklenen konuşma senime algoritmalardır. Bir sinir sesi kullanırken, sentezlenmiş konuşma, insan kayıtlarından neredeyse ayırt edilemez. İnsan benzeri doğal Prosody ve sözcüklerin bir kısmını temizleyerek, sinir seslerde kullanıcılar AI sistemleriyle etkileşim kurarken büyük ölçüde dinleme performansını önemli ölçüde azaltır.

Bir sinir sesinize geçiş yapmak için, `name` [sinir Voice seçeneklerinden](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices)birini değiştirin. Ardından, için bir XML ad alanı ekleyin `mstts` ve metninizi etikete sarın `<mstts:express-as>` . `style`Konuşma stilini özelleştirmek için param 'ı kullanın. Bu örnek kullanır `cheerful` , ancak `customerservice` `chat` konuşma stilindeki farkı görmek için veya olarak ayarlamayı deneyin.

> [!IMPORTANT]
> Sinir sesleri **yalnızca** *Doğu ABD*, *Güney Doğu Asya*ve *Batı Avrupa* bölgelerinde oluşturulan konuşma kaynakları için desteklenir.

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="en-US-AriaNeural">
    <mstts:express-as style="cheerful">
      This is awesome!
    </mstts:express-as>
  </voice>
</speak>
```
