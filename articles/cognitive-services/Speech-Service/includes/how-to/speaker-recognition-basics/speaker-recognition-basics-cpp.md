---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 09/28/2020
ms.author: v-jawe
ms.custom: references_regions
ms.openlocfilehash: 489514068d83f7b2953732415ba066a2d4555df8
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95015514"
---
Bu hızlı başlangıçta, konuşma SDK 'sını kullanarak konuşmacı tanıma için temel tasarım düzenlerini öğrenirsiniz; örneğin:

* Metne bağımlı ve metinden bağımsız doğrulama
* Sesler grubu arasında bir ses örneği tanımlamak için konuşmacı tanımlaması
* Ses profillerini silme

Konuşma tanıma kavramlarının üst düzey bir görünümü için bkz. [genel bakış](../../../speaker-recognition-overview.md) makalesi.

## <a name="skip-to-samples-on-github"></a>GitHub 'da örneklere atlayın

Örnek koda doğrudan atlamak istiyorsanız GitHub 'daki [C++ hızlı başlangıç örneklerine](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp/windows) bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

> [!IMPORTANT]
> Konuşmacı tanıma özelliği şu anda *yalnızca* bölgede oluşturulan Azure konuşma kaynaklarında desteklenmektedir `westus` .

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz için önce konuşma SDK 'sını yüklemeniz gerekir. Platformunuza bağlı olarak, aşağıdaki yönergeleri kullanın:

* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=linux&pivots=programming-language-cpp" target="_blank">'Un <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=macos&pivots=programming-language-cpp" target="_blank">macOS <span class="docon docon-navigate-external x-hidden-focus"></span></a>
* <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/setup-platform?tabs=windows&pivots=programming-language-cpp" target="_blank">Pencerelerin <span class="docon docon-navigate-external x-hidden-focus"></span></a>

## <a name="import-dependencies"></a>Bağımlılıkları içeri aktar

Bu makaledeki örnekleri çalıştırmak için,. cpp dosyanızın en üstüne aşağıdaki deyimleri ekleyin.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="dependencies":::

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Konuşma SDK 'sını kullanarak konuşma hizmetini çağırmak için bir oluşturmanız gerekir [`SpeechConfig`](/cpp/cognitive-services/speech/speechconfig) . Bu sınıf, uygulamanız hakkında, anahtarınız ve ilgili bölge, uç nokta, ana bilgisayar veya yetkilendirme belirteci gibi bilgileri içerir.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="get_speech_config":::

## <a name="text-dependent-verification"></a>Metne bağımlı doğrulama

Konuşmacı Doğrulama, konuşmacının bilinen veya **kayıtlı** bir sesle eşleştiğini onaylama işlemidir. İlk adım, bir ses profilini **kaydetmek** ve bu sayede hizmetin gelecekteki sesli örnekleri karşılaştırmak için bir şey olması gerekir. Bu örnekte, profili, hem kayıt hem de doğrulama için kullanılmak üzere belirli bir parola gerektiren **metne bağlı** bir strateji kullanarak kaydedeolursunuz. Desteklenen parola listesi için [başvuru belgelerine](/rest/api/speakerrecognition/) bakın.

### <a name="textdependentverification-function"></a>Textdependentdoğrulama işlevi

İşlevi oluşturarak başlayın `TextDependentVerification` .

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="text_dependent_verification":::

Bu işlev [Createprofileasync](/cpp/cognitive-services/speech/voiceprofileclient#createprofileasync) yöntemiyle bir [voiceprofile](/cpp/cognitive-services/speech/voiceprofile) nesnesi oluşturur. Üç [tür](/cpp/cognitive-services/speech/microsoft-cognitiveservices-speech-namespace#enum-voiceprofiletype) vardır `VoiceProfile` :

- TextIndependentIdentification
- Textdependentdoğrulaması
- Textindependentdoğrulaması

Bu durumda, ' ye geçitirsiniz `VoiceProfileType::TextDependentVerification` `CreateProfileAsync` .

Sonra, ileri ' yi ve sonra tanımlayacaksınız iki yardımcı işlevi `AddEnrollmentsToTextDependentProfile` çağırabilirsiniz `SpeakerVerify` . Son olarak, profili temizlemek için [Deleteprofileasync](/cpp/cognitive-services/speech/voiceprofileclient#deleteprofileasync) çağrısı yapın.

### <a name="addenrollmentstotextdependentprofile-function"></a>AddEnrollmentsToTextDependentProfile işlevi

Bir ses profilini kaydetmek için aşağıdaki işlevi tanımlayın.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="add_enrollments_dependent":::

Bu işlevde, ses örneklerini, `while` kalan ve kayıt için gereken örnek sayısını izleyen bir döngüye kaydeder. Her yinelemede, [Kayıtprofileasync](/cpp/cognitive-services/speech/voiceprofileclient#enrollprofileasync) parolayı mikrofona konuşarak ister ve örneği ses profiline ekler.

### <a name="speakerverify-function"></a>Hoparlörkerverify işlevi

`SpeakerVerify`Aşağıdaki gibi tanımlayın.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="speaker_verify":::

Bu işlevde, daha önce oluşturduğunuz [Voiceprofile](/cpp/cognitive-services/speech/voiceprofile) nesnesini geçirerek [speakerlıtemicationmodel:: fromprofile](/cpp/cognitive-services/speech/speakerverificationmodel#fromprofile) yöntemiyle bir [hoparlörkerdoğrulamaları icationmodel](/cpp/cognitive-services/speech/speakerverificationmodel) nesnesi oluşturacaksınız.

Sonra, [SpeechRecognizer:: RecognizeOnceAsync](/cpp/cognitive-services/speech/speechrecognizer#recognizeonceasync) , parolayı yeniden konuşmanıza neden olur, ancak bu kez ses profilinize göre doğrular ve 0.0-1,0 arasında bir benzerlik puanı döndürür. [Speakerrecognitionresult](/cpp/cognitive-services/speech/speakerrecognitionresult) nesnesi ayrıca `Accept` `Reject` , parolanın eşleşip eşleşmediğini göz önünde bulundurularak veya ' i döndürür.

## <a name="text-independent-verification"></a>Metnin bağımsız doğrulaması

**Metne bağımlı** doğrulamanın aksine, **metinden bağımsız** doğrulama:

* Belirli bir parolanın söyulmasına gerek yoktur, her şey söylenebilir
* Üç ses *örneği gerektirmez, ancak 20* saniye Toplam ses gerektirir

### <a name="textindependentverification-function"></a>Textindependentdoğrulama işlevi

İşlevi oluşturarak başlayın `TextIndependentVerification` .

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="text_independent_verification":::

İşlevi gibi `TextDependentVerification` , bu Işlev [Createprofileasync](/cpp/cognitive-services/speech/voiceprofileclient#createprofileasync) yöntemiyle bir [voiceprofile](/cpp/cognitive-services/speech/voiceprofile) nesnesi oluşturur.

Bu durumda, ' ye geçitirsiniz `VoiceProfileType::TextIndependentVerification` `CreateProfileAsync` .

Ardından, daha sonra `AddEnrollmentsToTextIndependentProfile` tanımladığınız ve daha önce tanımladığınız iki yardımcı işlevi çağırabilirsiniz: `SpeakerVerify` Son olarak, profili temizlemek için [Deleteprofileasync](/cpp/cognitive-services/speech/voiceprofileclient#deleteprofileasync) çağrısı yapın.

### <a name="addenrollmentstotextindependentprofile"></a>AddEnrollmentsToTextIndependentProfile

Bir ses profilini kaydetmek için aşağıdaki işlevi tanımlayın.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="add_enrollments_independent":::

Bu işlevde, ses örneklerini, `while` kalan ses ve kayıt için gerekli olan sesin saniye sayısını izleyen bir döngüye kaydeder. Her yinelemede, [Kayıtprofileasync](/cpp/cognitive-services/speech/voiceprofileclient#enrollprofileasync) sizi mikrofona konuşuyor ve örneği ses profiline ekliyor.

## <a name="speaker-identification"></a>Konuşmacı belirleme

Konuşmacı kimliği, belirli bir kayıtlı ses grubundan **kimin** konuşduğunu tespit etmek için kullanılır. Bu işlem, tek bir profile karşı doğrulama yerine, aynı anda birden çok ses profiline karşı, ana fark ile **metin bağımsız doğrulamaya** çok benzer.

### <a name="textindependentidentification-function"></a>TextIndependentIdentification işlevi

İşlevi oluşturarak başlayın `TextIndependentIdentification` .

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="text_independent_indentification":::

`TextDependentVerification`Ve işlevleri gibi `TextIndependentVerification` , bu Işlev [Createprofileasync](/cpp/cognitive-services/speech/voiceprofileclient#createprofileasync) yöntemiyle bir [voiceprofile](/cpp/cognitive-services/speech/voiceprofile) nesnesi oluşturur.

Bu durumda, ' ye geçitirsiniz `VoiceProfileType::TextIndependentIdentification` `CreateProfileAsync` .

Daha sonra, daha `AddEnrollmentsToTextIndependentProfile` önce tanımladığınız ve daha sonra tanımlayacağımız iki yardımcı işlevi çağırabilirsiniz: `SpeakerIdentify` Son olarak, profili temizlemek için [Deleteprofileasync](/cpp/cognitive-services/speech/voiceprofileclient#deleteprofileasync) çağrısı yapın.

### <a name="speakeridentify-function"></a>Hoparlörkerbelirlenmesi işlevi

`SpeakerIdentify`İşlevi aşağıdaki gibi tanımlayın.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="speaker_identify":::

Bu işlevde, [hoparlörkertemationmodel:: FromProfiles](/cpp/cognitive-services/speech/speakeridentificationmodel#fromprofiles) yöntemiyle bir [hoparlörkerıcertificate](/cpp/cognitive-services/speech/speakeridentificationmodel) nesnesi oluşturacaksınız. `SpeakerIdentificationModel::FromProfiles`[Voiceprofile](/cpp/cognitive-services/speech/voiceprofile) nesnelerinin bir listesini kabul eder. Bu durumda, yalnızca `VoiceProfile` daha önce oluşturduğunuz nesneyi geçireceğiz. Ancak isterseniz, `VoiceProfile` her biri farklı bir sesten ses örneklerine kayıtlı birden çok nesneyi geçirebilirsiniz.

Sonra, [SpeechRecognizer:: RecognizeOnceAsync](/cpp/cognitive-services/speech/speechrecognizer#recognizeonceasync) , tekrar konuşmak isteyip istemediğinizi sorar. Bu süre, sesinizi kayıtlı ses profilleriyle karşılaştırır ve en benzer ses profilini döndürür.

## <a name="main-function"></a>Main işlevi

Son olarak, `main` işlevi aşağıdaki gibi tanımlayın.

:::code language="cpp" source="~/cognitive-services-quickstart-code/cpp/speech/speaker-recognition.cpp" id="main":::

Bu işlev, yalnızca daha önce tanımladığınız işlevleri çağırır. Ancak ilk olarak, bir [Voiceprofileclient](/cpp/cognitive-services/speech/voiceprofileclient) nesnesi ve [hoparlörkertanıyıcı](/cpp/cognitive-services/speech/speakerrecognizer) nesnesi oluşturur.

```
auto speech_config = GetSpeechConfig();
auto client = VoiceProfileClient::FromConfig(speech_config);
auto recognizer = SpeakerRecognizer::FromConfig(speech_config, audio_config);
```

, `VoiceProfileClient` Ses profilleri oluşturmak, kaydetmek ve silmek için kullanılır. , `SpeakerRecognizer` Konuşma örneklerini bir veya daha fazla kayıtlı ses profiline karşı doğrulamak için kullanılır.

## <a name="changing-audio-input-type"></a>Ses giriş türünü değiştirme

Bu makaledeki örneklerde, ses örnekleri için varsayılan cihaz mikrofonu giriş olarak kullanılır. Ancak, mikrofon girişi yerine ses dosyalarını kullanmanız gereken senaryolarda aşağıdaki satırı değiştirmeniz yeterlidir:

```
auto audio_config = Audio::AudioConfig::FromDefaultMicrophoneInput();
```

Yeni değer:

```
auto audio_config = Audio::AudioConfig::FromWavFileInput(path/to/your/file.wav);
```

Ya da herhangi bir kullanımını `audio_config` [sesle:: audioconfig:: Fromwavfileınput](/cpp/cognitive-services/speech/audio-audioconfig#fromwavfileinput)ile değiştirin. Ayrıca, örneğin, kayıt ve dosyalar için bir mikrofon kullanarak, örneğin, doğrulama için bir mikrofon kullanarak karma girdilere sahip olabilirsiniz.