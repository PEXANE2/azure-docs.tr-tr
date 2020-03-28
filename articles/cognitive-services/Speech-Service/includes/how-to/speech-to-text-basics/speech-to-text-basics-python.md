---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: dapine
ms.openlocfilehash: c8354a67d26210035355f4947cb5f17cee934508
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79372826"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede varsayar:

* Azure hesabınız ve Konuşma hizmeti aboneliğiniz var. Hesabınız ve hesabınız yoksa -- [Konuşma hizmetini ücretsiz olarak deneyin.](../../../get-started.md)

## <a name="install-and-import-the-speech-sdk"></a>Konuşma SDK'yı yükleme ve alma

Herhangi bir şey yapmadan önce Konuşma SDK'sını yüklemeniz gerekir.

```Python
pip install azure-cognitiveservices-speech
```

macOS'taysanız ve yükleme sorunlarıyla karşılaştıysanız, önce bu komutu çalıştırmanız gerekebilir.

```Python
python3 -m pip install --upgrade pip
```

Speech SDK yüklendikten sonra, bu bildirimle Python projenize aktarın.

```Python
import azure.cognitiveservices.speech as speechsdk
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

Bir anahtar ve bölge kullanılarak [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) nasıl oluşturulduğuna bir göz atalım.

```Python
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
```

## <a name="initialize-a-recognizer"></a>Tanıyıcıyı başlatma

Bir , bir [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)sonraki adım oluşturduktan sonra [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python)bir . Bir [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python)harfini başharflediğinizde, bunu geçirmeniz `speech_config`gerekir. Bu, konuşma hizmetinin isteğinizi doğrulamak için gerektirdiği kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu kullanarak konuşmayı fark ediyorsanız, aşağıdaki [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python) gibi görünmelidir:

```Python
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config)
```

Ses giriş aygıtını belirtmek istiyorsanız, ''ınızı ilke [`AudioConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audioconfig?view=azure-python) olarak alırken `audio_config` bir parametre [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python)oluşturmanız ve parametreyi sağlamanız gerekir.

> [!TIP]
> [Ses giriş aygıtınız için aygıt kimliğini nasıl alacağınızı öğrenin.](../../../how-to-select-audio-input-devices.md)

```Python
audio_config = AudioConfig(device_name="<device id>");
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_config)
```

Mikrofon kullanmak yerine ses dosyası sağlamak istiyorsanız, yine de `audio_config`bir . Ancak, bir [`AudioConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audioconfig?view=azure-python), yerine sağlamak `device_name`için bir , `filename` parametre kullanırsınız oluşturun.

```Python
audio_filename = "whatstheweatherlike.wav"
audio_input = speechsdk.AudioConfig(filename=audio_filename)
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_input)
```

## <a name="recognize-speech"></a>Konuşma tanıma

Python için Konuşma SDK'sının [Tanıyan sınıfı,](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python) konuşma tanıma için kullanabileceğiniz birkaç yöntemi ortaya çıkarır.

* Tek çekim tanıma (eşitleme) - Engelleme (senkron) modunda tanıma gerçekleştirir. Tek bir söyleyiş tanındıktan sonra döner. Tek bir söyleyninin sonu, sonunda sessizlik dinleyerek veya en fazla 15 saniyelik ses işlenene kadar belirlenir. Görev sonucu olarak tanıma metnini döndürür.
* Tek çekim tanıma (async) - Engellemesiz (asynchronous) modda tanıma gerçekleştirir. Bu tek bir söyleyiş tanıyacaktır. Tek bir söyleyninin sonu, sonunda sessizlik dinleyerek veya en fazla 15 saniyelik ses işlenene kadar belirlenir.
* Sürekli tanıma (eşitleme) - Senkronize olarak sürekli tanıma yı başlatır. Tanıma sonuçları almak `EventSignal` için istemcinin bağlanması gerekir. Tanımayı durdurmak için [stop_continuous_recognition()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition--)numaralı telefonu arayın.
* Sürekli tanıma (async) - Asynchronously sürekli tanıma işlemi başlatır. Kullanıcı tanıma sonuçları almak için EventSignal'e bağlanmak zorundadır. Eşzamanlı sürekli tanımayı durdurmak için [stop_continuous_recognition()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition-async--)numaralı telefonu arayın.

> [!NOTE]
> [Konuşma tanıma modunu](../../../how-to-choose-recognition-mode.md)nasıl seçeceğiniz hakkında daha fazla bilgi edinin.

### <a name="single-shot-recognition"></a>Tek çekim tanıma

Burada senkron tek çekim tanıma bir örnek [`recognize_once()`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognize-once)kullanarak:

```Python
result = speech_recognizer.recognize_once()
```

Burada senkron tek çekim tanıma bir örnek [`recognize_once_async()`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognize-once-async------azure-cognitiveservices-speech-resultfuture)kullanarak:

```Python
result = speech_recognizer.recognize_once_async()
```

Senkron veya eşzamanlı yöntemi kullanıp kullanmadığınıza bakılmaksızın, sonuç aracılığıyla yinelemek için bazı kodlar yazmanız gerekir. Bu örnek şu [`result.reason`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.resultreason?view=azure-python)şekilde değerlendirilir:

* Tanıma sonucunu yazdırır:`speechsdk.ResultReason.RecognizedSpeech`
* Tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin:`speechsdk.ResultReason.NoMatch `
* Bir hatayla karşılaşılırsa, hata iletisini yazdırın:`speechsdk.ResultReason.Canceled`

```Python
if result.reason == speechsdk.ResultReason.RecognizedSpeech:
    print("Recognized: {}".format(result.text))
elif result.reason == speechsdk.ResultReason.NoMatch:
    print("No speech could be recognized: {}".format(result.no_match_details))
elif result.reason == speechsdk.ResultReason.Canceled:
    cancellation_details = result.cancellation_details
    print("Speech Recognition canceled: {}".format(cancellation_details.reason))
    if cancellation_details.reason == speechsdk.CancellationReason.Error:
        print("Error details: {}".format(cancellation_details.error_details))
```

### <a name="continuous-recognition"></a>Sürekli tanıma

Sürekli tanıma, tek çekimtanımadan biraz daha fazla ilgilidir. Tanıma sonuçlarını almak `EventSignal` için bağlanmanızı gerektirir ve tanımayı durdurmak için [stop_continuous_recognition()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition--) veya [stop_continuous_recognition()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition-async--)aramanız gerekir. Aşağıda, ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirildiğine ilişkin bir örnek verilmiştir.

Girişi tanımlayarak ve bir [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python)baş harfe başlayalım:

```Python
audio_config = speechsdk.audio.AudioConfig(filename=weatherfilename)
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_config)
```

Sonra, konuşma tanıma durumunu yönetmek için bir değişken oluşturalım. Başlangıç olarak, bunu, `False`tanımanın başlangıcından beri güvenli bir şekilde bitmediğini varsayabiliriz.

```Python
done = False
```

Şimdi, bir geri `evt` arama oluşturacağız. Akılda tutulması gereken birkaç şey var.

* Bir `evt` ileti alındığı `evt` zaman, ileti yazdırılır.
* Bir `evt` alındıktan sonra, [stop_continuous_recognition()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition--) tanımayı durdurmak için çağrılır.
* Tanıma durumu ' `True`olarak değiştirilir.

```Python
def stop_cb(evt):
    print('CLOSING on {}'.format(evt))
    speech_recognizer.stop_continuous_recognition()
    nonlocal done
    done = True
```

Bu kod örneği, geri aramaları ' dan [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#start-continuous-recognition--)gönderilen olaylara nasıl bağlayabilirsiniz' i gösterir.

* [`recognizing`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognizing): Ara tanıma sonuçlarını içeren olaylar için sinyal.
* [`recognized`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognized): Nihai tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi gösteren).
* [`session_started`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-started): Tanıma oturumunun (operasyonun) başlangıcını gösteren olaylar için sinyal.
* [`session_stopped`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-stopped): Tanıma oturumunun (operasyonun) sona erini gösteren olaylar için sinyal.
* [`canceled`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#canceled): İptal edilmiş tanıma sonuçlarını içeren olaylar için sinyal (sonuç olarak iptal edilen bir tanıma girişimi veya doğrudan iptal isteği veya alternatif olarak bir aktarım veya protokol hatası) gösterir.

```Python
speech_recognizer.recognizing.connect(lambda evt: print('RECOGNIZING: {}'.format(evt)))
speech_recognizer.recognized.connect(lambda evt: print('RECOGNIZED: {}'.format(evt)))
speech_recognizer.session_started.connect(lambda evt: print('SESSION STARTED: {}'.format(evt)))
speech_recognizer.session_stopped.connect(lambda evt: print('SESSION STOPPED {}'.format(evt)))
speech_recognizer.canceled.connect(lambda evt: print('CANCELED {}'.format(evt)))

speech_recognizer.session_stopped.connect(stop_cb)
speech_recognizer.canceled.connect(stop_cb)
```

Her şey ayarlandı, biz [start_continuous_recognition()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-stopped)arayabilirsiniz.

```Python
speech_recognizer.start_continuous_recognition()
while not done:
    time.sleep(.5)
```

### <a name="dictation-mode"></a>Dikte modu

Sürekli tanıma kullanırken, ilgili "dikte işlemini etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, noktalama işaretleri gibi tümce yapılarının sözcük açıklamalarını yorumlamak için konuşma config örneğine neden olur. Örneğin, "Şehirde mi yaşıyorsunuz soru işareti" sözü "Şehirde mi yaşıyorsunuz?" şeklinde yorumlanır.

Dikte modunu etkinleştirmek için, [`enable_dictation()`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#enable-dictation--) yöntemi [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)nikullanarak.

```Python 
SpeechConfig.enable_dictation()
```

## <a name="change-source-language"></a>Kaynak dilini değiştirme

Konuşma tanıma için ortak bir görev giriş (veya kaynak) dilini belirtmektir. Giriş dilini Almanca'ya nasıl değiştireceğinize bir göz atalım. Kodunuzda SpeechConfig'inizi bulun ve bu satırı doğrudan altına ekleyin.

```Python
speech_config.speech_recognition_language="de-DE"
```

[`speech_recognition_language`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#speech-recognition-language)bir bağımsız değişken olarak bir dize alan bir parametredir. Desteklenen [yerel halk/dil](../../../language-support.md)listesinde herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu artırın

Konuşma SDK ile tanıma doğruluğunu artırmanın birkaç yolu vardır. İfade Listeleri'ne bir göz atalım. Tümcecik Listeleri, ses verilerindeki bilinen ifadeleri tanımlamak için kullanılır, örneğin bir kişinin adı veya belirli bir konum. Tümcecik Listesine tek sözcük veya tam tümcecik eklenebilir. Tanıma sırasında, sese tüm tümcecik için tam bir eşleşme varsa, bir ifade listesindeki bir giriş kullanılır. İfadeyle tam eşleşme bulunamazsa, tanıma yardımcı olmaz.

> [!IMPORTANT]
> İfade Listesi özelliği yalnızca İngilizce olarak kullanılabilir.

Bir tümcecik listesini kullanmak [`PhraseListGrammar`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?view=azure-python) için önce bir nesne oluşturun, sonra [`addPhrase`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?view=azure-python#addphrase-phrase--str-)belirli sözcükler ve tümcecikler ekleyin.

Bir sonraki [`PhraseListGrammar`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?view=azure-python) tanıma da veya Konuşma hizmetine yeniden bağlantı dan sonra etkili olacak değişiklikler.

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Supercalifragilisticexpialidocious")
```

İfade listenizi temizlemeniz gerekiyorsa: 

```Python
phrase_list_grammar.clear()
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../how-to-custom-speech.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)