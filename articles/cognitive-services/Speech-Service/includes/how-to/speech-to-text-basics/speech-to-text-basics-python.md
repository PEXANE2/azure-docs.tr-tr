---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: 3ee78560818d3651d5822c551ba2e0ccee3376e6
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82204509"
---
## <a name="prerequisites"></a>Ön koşullar

Bu makalede şu varsayılmaktadır:

* Azure hesabınız ve konuşma hizmeti aboneliğiniz var. Hesabınız ve aboneliğiniz yoksa-- [konuşma hizmetini ücretsiz deneyin](../../../get-started.md).

## <a name="install-and-import-the-speech-sdk"></a>Konuşma SDK 'sını yükleyip içeri aktarma

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir.

```Python
pip install azure-cognitiveservices-speech
```

MacOS 'ta çalışıyorsanız ve yüklemeyi sorunları yaşıyorsanız, önce bu komutu çalıştırmanız gerekebilir.

```Python
python3 -m pip install --upgrade pip
```

Konuşma SDK 'Sı yüklendikten sonra, bu bildirimde bulunan Python projenize aktarın.

```Python
import azure.cognitiveservices.speech as speechsdk
```

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)oluşturmanız gerekir. Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir.

> [!NOTE]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

[`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)Şunları başlatabilmeniz için birkaç yol vardır:

* Abonelik ile: bir anahtarı ve ilişkili bölgeyi geçirin.
* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

Bir [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) anahtar ve bölge kullanarak nasıl oluşturulduğuna göz atalım. Bölge tanımlarınızı bulmak için [bölge desteği](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) sayfasına bakın.

```Python
speech_key, service_region = "YourSubscriptionKey", "YourServiceRegion"
speech_config = speechsdk.SpeechConfig(subscription=speech_key, region=service_region)
```

## <a name="initialize-a-recognizer"></a>Tanıyıcı başlatma

Bir [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)oluşturduktan sonra, bir sonraki adım bir [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python)' ı başlatmaktır. Bir [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python)başlattığınızda, bunu geçirmeniz gerekir `speech_config`. Bu, konuşma hizmetinin isteğinizi doğrulamak için ihtiyaç duyduğu kimlik bilgilerini sağlar.

Cihazınızın varsayılan mikrofonunu [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python) kullanarak konuşmayı tanıyor olmanız halinde şöyle görünmelidir:

```Python
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config)
```

Ses giriş cihazını belirtmek isterseniz, oluşturmanız [`AudioConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audioconfig?view=azure-python) ve ' ı başlatırken `audio_config` parametresini sağlamanız gerekir. [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python)

> [!TIP]
> [Ses giriş cihazınız için CIHAZ kimliğini nasıl alabileceğinizi öğrenin](../../../how-to-select-audio-input-devices.md).

```Python
audio_config = AudioConfig(device_name="<device id>");
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_config)
```

Mikrofon kullanmak yerine bir ses dosyası sağlamak istiyorsanız, yine de sağlamanız gerekir `audio_config`. Ancak, ' yi sağlamak [`AudioConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audioconfig?view=azure-python) `device_name`yerine bir oluşturduğunuzda, `filename` parametresini kullanacaksınız.

```Python
audio_filename = "whatstheweatherlike.wav"
audio_input = speechsdk.AudioConfig(filename=audio_filename)
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_input)
```

## <a name="recognize-speech"></a>Konuşma tanıma

Python için konuşma SDK 'Sı için [tanıyıcı sınıfı](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python) , konuşma tanıma için kullanabileceğiniz birkaç yöntem sunar.

* Tek atışı tanıma (eşitleme)-bir engelleme (zaman uyumlu) modunda tanıma gerçekleştirir. Tek bir söylenişi tanındıktan sonra döndürür. Tek bir utterüance 'in sonunda, sonda sessizlik dinlemesi veya en fazla 15 saniyelik ses işlenene kadar belirlenir. Görev, tanıma metnini sonuç olarak döndürür.
* Tek atışı tanıma (Async)-engelleyici olmayan (zaman uyumsuz) modda tanıma gerçekleştirir. Bu, tek bir utterance algılar. Tek bir utterüance 'in sonunda, sonda sessizlik dinlemesi veya en fazla 15 saniyelik ses işlenene kadar belirlenir.
* Sürekli tanıma (eşitleme)-eşzamanlı algılamayı sürekli olarak başlatır. İstemci, tanıma sonuçlarını almak `EventSignal` için öğesine bağlanmalıdır. Tanımayı durdurmak için [stop_continuous_recognition ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition--)çağırın.
* Sürekli tanıma (Async)-zaman uyumsuz olarak sürekli tanıma işlemini başlatır. Kullanıcı, tanıma sonuçlarını almak için EventSignal 'e bağlanmalıdır. Zaman uyumsuz sürekli tanımayı durdurmak için [stop_continuous_recognition ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition-async--)çağırın.

> [!NOTE]
> [Konuşma tanıma modunu seçme](../../../how-to-choose-recognition-mode.md)hakkında daha fazla bilgi edinin.

### <a name="single-shot-recognition"></a>Tek atışı tanıma

Kullanarak [`recognize_once()`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognize-once)zaman uyumlu tek kararlı tanıma örneği aşağıda verilmiştir:

```Python
result = speech_recognizer.recognize_once()
```

Şu kullanılarak [`recognize_once_async()`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognize-once-async------azure-cognitiveservices-speech-resultfuture)zaman uyumsuz tek kararlı tanıma örneği aşağıda verilmiştir:

```Python
result = speech_recognizer.recognize_once_async()
```

Zaman uyumlu veya zaman uyumsuz yöntemi kullanlemenizden bağımsız olarak, sonuç boyunca yinelemek için bazı kodlar yazmanız gerekir. Bu örnek [`result.reason`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.resultreason?view=azure-python)şunları değerlendirir:

* Tanınma sonucunu yazdırır:`speechsdk.ResultReason.RecognizedSpeech`
* Bir tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin:`speechsdk.ResultReason.NoMatch `
* Bir hata ile karşılaşırsanız, hata iletisini yazdırın:`speechsdk.ResultReason.Canceled`

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

Sürekli tanıma, tek kararlı tanıma göre biraz daha karmaşıktır. Tanıma sonuçlarını almak `EventSignal` için ' a bağlanmanızı gerektirir ve ' de, tanımayı durdurmak için [stop_continuous_recognition ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition--) veya [stop_continuous_recognition ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition-async--)çağırmanız gerekir. İşte, bir ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirilebileceğini gösteren bir örnek.

Girişi tanımlayarak ve şunu başlatarak başlayalım [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?view=azure-python):

```Python
audio_config = speechsdk.audio.AudioConfig(filename=weatherfilename)
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_config)
```

Sonra, konuşma tanımanın durumunu yönetmek için bir değişken oluşturalım. Başlamak için bunu olarak `False`ayarlayacağız, çünkü tanımanın başlangıcında bitmediğini güvenli bir şekilde varsayacağız.

```Python
done = False
```

Şimdi, alındığında sürekli tanımayı `evt` durdurmak için bir geri çağırma oluşturacağız. Göz önünde bulundurmanız gereken birkaç nokta vardır.

* Bir `evt` alındığında `evt` ileti yazdırılır.
* Bir bir `evt` alındıktan sonra, tanımayı durdurmak için [stop_continuous_recognition ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition--) çağırılır.
* Tanınma durumu olarak `True`değiştirilir.

```Python
def stop_cb(evt):
    print('CLOSING on {}'.format(evt))
    speech_recognizer.stop_continuous_recognition()
    nonlocal done
    done = True
```

Bu kod örneği, [`SpeechRecognizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#start-continuous-recognition--)' den gönderilen olaylara geri çağırmaları bağlamayı gösterir.

* [`recognizing`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognizing): Ara tanıma sonuçları içeren olaylar için sinyal.
* [`recognized`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognized): Son tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi olduğunu gösterir).
* [`session_started`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-started): Bir tanıma oturumunun başlangıcını (işlem) gösteren olaylar için sinyal.
* [`session_stopped`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-stopped): Bir tanıma oturumunun (işlem) sonunu gösteren olaylar için sinyal.
* [`canceled`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#canceled): İptal edilen tanıma sonuçlarını içeren olaylar için sinyal (sonuç veya doğrudan iptal isteği olarak iptal edilen bir tanıma girişimi veya ya da bir aktarım ya da protokol arızası).

```Python
speech_recognizer.recognizing.connect(lambda evt: print('RECOGNIZING: {}'.format(evt)))
speech_recognizer.recognized.connect(lambda evt: print('RECOGNIZED: {}'.format(evt)))
speech_recognizer.session_started.connect(lambda evt: print('SESSION STARTED: {}'.format(evt)))
speech_recognizer.session_stopped.connect(lambda evt: print('SESSION STOPPED {}'.format(evt)))
speech_recognizer.canceled.connect(lambda evt: print('CANCELED {}'.format(evt)))

speech_recognizer.session_stopped.connect(stop_cb)
speech_recognizer.canceled.connect(stop_cb)
```

Her şey ayarlandığında [start_continuous_recognition ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-stopped)çağırabiliriz.

```Python
speech_recognizer.start_continuous_recognition()
while not done:
    time.sleep(.5)
```

### <a name="dictation-mode"></a>Dikte etme modu

Sürekli tanıma kullanırken, ilgili "dikte etmeyi etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, konuşma yapılandırma örneğinin noktalama gibi tümce yapılarının sözcük açıklamalarını yorumlamasını sağlar. Örneğin, "kasadaki gerçek zamanlı olarak", "kasadaki canlı mısınız?" metni olarak yorumlanabilir.

Dikte modunu etkinleştirmek için, içindeki [`enable_dictation()`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#enable-dictation--) yöntemi kullanın. [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)

```Python 
SpeechConfig.enable_dictation()
```

## <a name="change-source-language"></a>Kaynak dilini değiştir

Konuşma tanıma için ortak bir görev, giriş (veya kaynak) dilini belirtmektir. Giriş dilini Almanca olarak nasıl değiştirebileceğinizi göz atalım. Kodunuzda SpeechConfig bulun ve ardından bu satırı hemen altına ekleyin.

```Python
speech_config.speech_recognition_language="de-DE"
```

[`speech_recognition_language`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#speech-recognition-language)bağımsız değişken olarak bir dize alan parametredir. Desteklenen [yerel ayarlar/diller](../../../language-support.md)listesinde herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu geliştirme

Konuşma SDK 'Sı ile tanıma doğruluğunu geliştirmenin birkaç yolu vardır. Tümcecik listelerine göz atalım. Tümcecik listeleri, bir kişinin adı veya belirli bir konum gibi, ses verilerinde bilinen tümcecikleri belirlemek için kullanılır. Tek sözcükler veya bütün ifadeler, bir tümcecik listesine eklenebilir. Tanıma sırasında, tüm tümcecik için tam eşleşme sese dahil olursa tümcecik listesindeki bir giriş kullanılır. Tümcecikle tam eşleşme bulunamazsa, tanıma yardımlı değildir.

> [!IMPORTANT]
> Tümcecik listesi özelliği yalnızca Ingilizce olarak kullanılabilir.

Bir tümcecik listesi kullanmak için, önce bir [`PhraseListGrammar`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?view=azure-python) nesne oluşturun, ardından ile [`addPhrase`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?view=azure-python#addphrase-phrase--str-)belirli sözcükler ve deyimler ekleyin.

Herhangi bir değişiklik [`PhraseListGrammar`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?view=azure-python) , bir sonraki tanıma göre veya konuşma hizmetine yeniden bağlanmaya sonra devreye girer.

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Supercalifragilisticexpialidocious")
```

Tümcecik listenizi temizlemeniz gerekirse: 

```Python
phrase_list_grammar.clear()
```

### <a name="other-options-to-improve-recognition-accuracy"></a>Tanıma doğruluğunu artırmak için diğer seçenekler

Tümcecik listeleri, tanıma doğruluğunu artırmak için yalnızca bir seçenektir. Aşağıdakileri de yapabilirsiniz: 

* [Özel Konuşma ile doğruluğu geliştirme](../../../how-to-custom-speech.md)
* [Kiracı modelleriyle doğruluğu geliştirme](../../../tutorial-tenant-model.md)
