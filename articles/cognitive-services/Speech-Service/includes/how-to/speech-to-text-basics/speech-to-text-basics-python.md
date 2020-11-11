---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/11/2020
ms.author: trbye
ms.openlocfilehash: 44980977f366bc40ceff9c7b5751d5657c79ccc2
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94482674"
---
Konuşma hizmetinin temel özelliklerinden biri de insan konuşmanızı tanıyabilme ve (genellikle konuşma-metin olarak adlandırılır). Bu hızlı başlangıçta, uygulama ve ürünlerinize yönelik konuşma SDK 'sını kullanarak yüksek kaliteli bir konuşmayı metne dönüştürme işlemini nasıl gerçekleştireceğinizi öğreneceksiniz.

## <a name="skip-to-samples-on-github"></a>GitHub 'da örneklere atlayın

Örnek koda doğrudan atlamak istiyorsanız GitHub 'daki [Python hızlı başlangıç örneklerine](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/python) bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede şu varsayılmaktadır:

* Azure hesabınız ve konuşma hizmeti aboneliğiniz var. Hesabınız ve aboneliğiniz yoksa-- [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

## <a name="install-and-import-the-speech-sdk"></a>Konuşma SDK 'sını yükleyip içeri aktarma

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir.

```Python
pip install azure-cognitiveservices-speech
```

MacOS 'ta çalışıyorsanız ve yüklemeyi sorunları yaşıyorsanız, önce bu komutu çalıştırmanız gerekebilir.

```Python
python3 -m pip install --upgrade pip
```

Konuşma SDK 'Sı yüklendikten sonra Python projenize aktarın.

```Python
import azure.cognitiveservices.speech as speechsdk
```

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir oluşturmanız gerekir [`SpeechConfig`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) . Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir. [`SpeechConfig`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)Anahtarınızı ve bölgenizi kullanarak bir oluşturun. Anahtar-bölge çiftini bulmak için [anahtarlar ve bölge bulma](../../../overview.md#find-keys-and-region) sayfasına bakın.

```Python
speech_config = speechsdk.SpeechConfig(subscription="<paste-your-subscription-key>", region="<paste-your-region>")
```

Şunları başlatabilmeniz için birkaç farklı yol vardır [`SpeechConfig`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) :

* Uç nokta ile: bir konuşma hizmeti uç noktasında geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Bir ana bilgisayar ile: bir konak adresini geçirin. Anahtar veya yetkilendirme belirteci isteğe bağlıdır.
* Yetkilendirme belirteci ile: bir yetkilendirme belirtecini ve ilişkili bölgeyi geçirin.

> [!NOTE]
> Konuşma tanıma, konuşma birleştirme, çeviri veya amaç tanıma işlemlerini gerçekleştirmekten bağımsız olarak her zaman bir yapılandırma oluşturacaksınız.

## <a name="recognize-from-microphone"></a>Mikrofondan tanı

Cihaz mikrofonunuzu kullanarak konuşmayı tanımak için `SpeechRecognizer` `AudioConfig` , bir ve iletmeksizin bir oluşturun `speech_config` .

```Python
import azure.cognitiveservices.speech as speechsdk

def from_mic():
    speech_config = speechsdk.SpeechConfig(subscription="<paste-your-subscription-key>", region="<paste-your-region>")
    speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config)
    
    print("Speak into your microphone.")
    result = speech_recognizer.recognize_once_async().get()
    print(result.text)

from_mic()
```

*Belirli* bir ses giriş cihazını kullanmak istiyorsanız, cihaz kimliğini bir içinde belirtmeniz `AudioConfig` ve oluşturucunun param öğesine geçirmeniz gerekir `SpeechRecognizer` `audio_config` . Ses giriş cihazınız için [CIHAZ kimliğini nasıl alabileceğinizi](../../../how-to-select-audio-input-devices.md) öğrenin.

## <a name="recognize-from-file"></a>Dosyadan tanı

Mikrofon kullanmak yerine bir ses dosyasından konuşmayı tanımak istiyorsanız, oluşturun [`AudioConfig`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audioconfig?preserve-view=true&view=azure-python) ve `filename` parametresini kullanın.

```Python
import azure.cognitiveservices.speech as speechsdk

def from_file():
    speech_config = speechsdk.SpeechConfig(subscription="<paste-your-subscription-key>", region="<paste-your-region>")
    audio_input = speechsdk.AudioConfig(filename="your_file_name.wav")
    speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_input)
    
    result = speech_recognizer.recognize_once_async().get()
    print(result.text)

from_file()
```

## <a name="error-handling"></a>Hata işleme

Önceki örneklerde, ' den tanınan metin alınır `result.text` , ancak hataları ve diğer yanıtları işlemek için, sonucu işlemek üzere bazı kodlar yazmanız gerekir. Aşağıdaki kod, özelliğini değerlendirir [`result.reason`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.resultreason?view=azure-python) ve:

* Tanınma sonucunu yazdırır: `speechsdk.ResultReason.RecognizedSpeech`
* Bir tanıma eşleşmesi yoksa, kullanıcıyı bilgilendirin: `speechsdk.ResultReason.NoMatch `
* Bir hata ile karşılaşırsanız, hata iletisini yazdırın: `speechsdk.ResultReason.Canceled`

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

## <a name="continuous-recognition"></a>Sürekli tanıma

Önceki örneklerde tek bir söylik tanınabilmesi için tek kararlı tanıma kullanılır. Tek bir utterüance 'in sonunda, sonda sessizlik dinlemesi veya en fazla 15 saniyelik ses işlenene kadar belirlenir.

Bunun aksine, sürekli tanıma, tanımanın ne zaman durdurulacağını **denetlemek** istediğinizde kullanılır. `EventSignal`Tanıma sonuçlarını almak için ' a bağlanmanızı gerektirir ve ' de, tanımayı durdurmak için [stop_continuous_recognition ()](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition--) veya [stop_continuous_recognition ()](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition-async--)çağırmanız gerekir. İşte, bir ses giriş dosyasında sürekli tanımanın nasıl gerçekleştirilebileceğini gösteren bir örnek.

Girişi tanımlayarak ve şunu başlatarak başlayalım [`SpeechRecognizer`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechrecognizer?preserve-view=true&view=azure-python) :

```Python
audio_config = speechsdk.audio.AudioConfig(filename=weatherfilename)
speech_recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_config)
```

Sonra, konuşma tanımanın durumunu yönetmek için bir değişken oluşturalım. Başlamak için bunu olarak ayarlayacağız, çünkü `False` tanımanın başlangıcında bitmediğini güvenli bir şekilde varsayacağız.

```Python
done = False
```

Şimdi, alındığında sürekli tanımayı durdurmak için bir geri çağırma oluşturacağız `evt` . Göz önünde bulundurmanız gereken birkaç nokta vardır.

* Bir `evt` alındığında `evt` ileti yazdırılır.
* Bir bir alındıktan sonra `evt` , tanımayı durdurmak için [stop_continuous_recognition ()](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#stop-continuous-recognition--) çağırılır.
* Tanınma durumu olarak değiştirilir `True` .

```Python
def stop_cb(evt):
    print('CLOSING on {}'.format(evt))
    speech_recognizer.stop_continuous_recognition()
    nonlocal done
    done = True
```

Bu kod örneği, ' den gönderilen olaylara geri çağırmaları bağlamayı gösterir [`SpeechRecognizer`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#start-continuous-recognition--) .

* [`recognizing`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognizing): Ara tanıma sonuçları içeren olaylar için sinyal.
* [`recognized`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#recognized): Son tanıma sonuçlarını içeren olaylar için sinyal (başarılı bir tanıma denemesi olduğunu gösterir).
* [`session_started`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-started): Bir tanıma oturumunun başlangıcını (işlem) gösteren olaylar için sinyal.
* [`session_stopped`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-stopped): Bir tanıma oturumunun (işlem) sonunu gösteren olaylar için sinyal.
* [`canceled`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#canceled): İptal edilen tanıma sonuçlarını içeren olaylar için sinyal (sonuç veya doğrudan iptal isteği olarak iptal edilen bir tanıma girişimi veya ya da bir aktarım ya da protokol arızası).

```Python
speech_recognizer.recognizing.connect(lambda evt: print('RECOGNIZING: {}'.format(evt)))
speech_recognizer.recognized.connect(lambda evt: print('RECOGNIZED: {}'.format(evt)))
speech_recognizer.session_started.connect(lambda evt: print('SESSION STARTED: {}'.format(evt)))
speech_recognizer.session_stopped.connect(lambda evt: print('SESSION STOPPED {}'.format(evt)))
speech_recognizer.canceled.connect(lambda evt: print('CANCELED {}'.format(evt)))

speech_recognizer.session_stopped.connect(stop_cb)
speech_recognizer.canceled.connect(stop_cb)
```

Her şey ayarlandığında [start_continuous_recognition ()](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.recognizer?view=azure-python#session-stopped)çağırabiliriz.

```Python
speech_recognizer.start_continuous_recognition()
while not done:
    time.sleep(.5)
```

### <a name="dictation-mode"></a>Dikte etme modu

Sürekli tanıma kullanırken, ilgili "dikte etmeyi etkinleştir" işlevini kullanarak dikte işlemini etkinleştirebilirsiniz. Bu mod, konuşma yapılandırma örneğinin noktalama gibi tümce yapılarının sözcük açıklamalarını yorumlamasını sağlar. Örneğin, "kasadaki gerçek zamanlı olarak", "kasadaki canlı mısınız?" metni olarak yorumlanabilir.

Dikte modunu etkinleştirmek için, içindeki [`enable_dictation()`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#enable-dictation--) yöntemi kullanın [`SpeechConfig`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?preserve-view=true&view=azure-python) .

```Python 
SpeechConfig.enable_dictation()
```

## <a name="change-source-language"></a>Kaynak dilini değiştir

Konuşma tanıma için ortak bir görev, giriş (veya kaynak) dilini belirtmektir. Giriş dilini Almanca olarak nasıl değiştirebileceğinizi göz atalım. Kodunuzda SpeechConfig bulun ve ardından bu satırı hemen altına ekleyin.

```Python
speech_config.speech_recognition_language="de-DE"
```

[`speech_recognition_language`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python#speech-recognition-language) bağımsız değişken olarak bir dize alan parametredir. Desteklenen [yerel ayarlar/diller](../../../language-support.md)listesinde herhangi bir değer sağlayabilirsiniz.

## <a name="improve-recognition-accuracy"></a>Tanıma doğruluğunu geliştirme

Konuşma SDK 'Sı ile tanıma doğruluğunu geliştirmenin birkaç yolu vardır. Tümcecik listelerine göz atalım. Tümcecik listeleri, bir kişinin adı veya belirli bir konum gibi, ses verilerinde bilinen tümcecikleri belirlemek için kullanılır. Tek sözcükler veya bütün ifadeler, bir tümcecik listesine eklenebilir. Tanıma sırasında, tüm tümcecik için tam eşleşme sese dahil olursa tümcecik listesindeki bir giriş kullanılır. Tümcecikle tam eşleşme bulunamazsa, tanıma yardımlı değildir.

> [!IMPORTANT]
> Tümcecik listesi özelliği yalnızca Ingilizce olarak kullanılabilir.

Bir tümcecik listesi kullanmak için, önce bir [`PhraseListGrammar`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?preserve-view=true&view=azure-python) nesne oluşturun, ardından ile belirli sözcükler ve deyimler ekleyin [`addPhrase`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?view=azure-python#addphrase-phrase--str-) .

Herhangi bir değişiklik [`PhraseListGrammar`](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.phraselistgrammar?preserve-view=true&view=azure-python) , bir sonraki tanıma göre veya konuşma hizmetine yeniden bağlanmaya sonra devreye girer.

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