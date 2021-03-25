---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 10/07/2020
ms.author: v-jawe
ms.custom: references_regions
ms.openlocfilehash: 3953e7182d90cb1737a2083e2315612b07f2eb84
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105105204"
---
Bu hızlı başlangıçta, konuşma SDK 'sını kullanarak konuşmacı tanıma için temel tasarım düzenlerini öğrenirsiniz; örneğin:

* Metne bağımlı ve metinden bağımsız doğrulama
* Sesler grubu arasında bir ses örneği tanımlamak için konuşmacı tanımlaması
* Ses profillerini silme

Konuşma tanıma kavramlarının üst düzey bir görünümü için bkz. [genel bakış](../../../speaker-recognition-overview.md) makalesi.

## <a name="skip-to-samples-on-github"></a>GitHub 'da örneklere atlayın

Örnek koda doğrudan atlamak istiyorsanız GitHub 'da [JavaScript hızlı başlangıç örneklerine](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/fa6428a0837779cbeae172688e0286625e340942/quickstart/javascript/node/speaker-recognition) bakın.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

> [!IMPORTANT]
> Konuşmacı tanıma özelliği şu anda *yalnızca* bölgede oluşturulan Azure konuşma kaynaklarında desteklenmektedir `westus` .

## <a name="install-the-speech-sdk"></a>Konuşma SDK 'sını yükler

Herhangi bir şey yapabilmeniz <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">için önce JavaScript Için konuşma SDK 'sını </a>yüklemeniz gerekir. Platformunuza bağlı olarak, aşağıdaki yönergeleri kullanın:

- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs#get-the-speech-sdk" target="_blank">Node.js <span 
class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="/azure/cognitive-services/speech-service/speech-sdk?tabs=browser#get-the-speech-sdk" target="_blank">Web tarayıcısı </a>

Ayrıca, hedef ortama bağlı olarak, aşağıdakilerden birini kullanın:

# <a name="script"></a>[SCRIPT](#tab/script)

JavaScript *microsoft.cognitiveservices.speech.sdk.bundle.js* dosyası <a href="https://aka.ms/csspeech/jsbrowserpackage" target="_blank">için konuşma SDK 'sını</a> indirip ayıklayın ve HTML dosyanıza erişilebilen bir klasöre yerleştirin.

```html
<script src="microsoft.cognitiveservices.speech.sdk.bundle.js"></script>;
```

> [!TIP]
> Bir Web tarayıcısını hedefliyorsanız ve etiketini kullanıyorsanız, `<script>` `sdk` ön ek gerekli değildir. `sdk`Ön ek, modülü adlandırmak için kullanılan bir diğer addır `require` .

# <a name="import"></a>[aktarmaya](#tab/import)

```javascript
import * from "microsoft-cognitiveservices-speech-sdk";
```

Hakkında daha fazla bilgi için `import` bkz. <a href="https://javascript.info/import-export" target="_blank">dışa ve içeri aktarma </a>.

# <a name="require"></a>[gerektirir](#tab/require)

```javascript
const sdk = require("microsoft-cognitiveservices-speech-sdk");
```

Hakkında daha fazla bilgi için `require` bkz. <a href="https://nodejs.org/en/knowledge/getting-started/what-is-require/" target="_blank">ne gerektiriyor? </a>.

---

## <a name="import-dependencies"></a>Bağımlılıkları içeri aktar

Bu makaledeki örnekleri çalıştırmak için,. js dosyanızın en üstüne aşağıdaki deyimleri ekleyin.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="dependencies":::

Bu deyimler gereken kitaplıkları içeri aktarır ve konuşma hizmeti abonelik anahtarınızı ve bölgenizi ortam değişkeninizden alır. Ayrıca, aşağıdaki görevlerde kullanacağınız ses dosyalarının yollarını da belirtir.

## <a name="create-helper-function"></a>Yardımcı işlevi oluştur

Ses dosyalarını konuşma hizmeti tarafından kullanılmak üzere akışlara okumak için aşağıdaki yardımcı işlevi ekleyin.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="helpers":::

Bu işlevde, [audioconfig](/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig) nesnesi oluşturmak Için [audioınputstream. createpushstream](/javascript/api/microsoft-cognitiveservices-speech-sdk/audioinputstream#createpushstream-audiostreamformat-) ve [Audioconfig. fromstreaminput](/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig#fromstreaminput-audioinputstream---pullaudioinputstreamcallback-) yöntemlerini kullanırsınız. Bu `AudioConfig` nesne bir ses akışını temsil eder. `AudioConfig`Aşağıdaki görevler sırasında, bu nesnelerden birkaçını kullanacaksınız.

## <a name="text-dependent-verification"></a>Metne bağımlı doğrulama

Konuşmacı Doğrulama, konuşmacının bilinen veya **kayıtlı** bir sesle eşleştiğini onaylama işlemidir. İlk adım, bir ses profilini **kaydetmek** ve bu sayede hizmetin gelecekteki sesli örnekleri karşılaştırmak için bir şey olması gerekir. Bu örnekte, profili, hem kayıt hem de doğrulama için kullanılmak üzere belirli bir parola gerektiren **metne bağlı** bir strateji kullanarak kaydedeolursunuz. Desteklenen parola listesi için [başvuru belgelerine](/rest/api/speakerrecognition/) bakın.

### <a name="textdependentverification-function"></a>Textdependentdoğrulama işlevi

İşlevi oluşturarak başlayın `TextDependentVerification` .

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="text_dependent_verification":::

Bu işlev, [Voiceprofileclient. createProfileAsync](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient#createprofileasync-voiceprofiletype--string---e--voiceprofile-----void---e--string-----void-) yöntemiyle bir [voiceprofile](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofile) nesnesi oluşturur. Üç [tür](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofiletype) vardır `VoiceProfile` :

- TextIndependentIdentification
- Textdependentdoğrulaması
- Textindependentdoğrulaması

Bu durumda, ' `VoiceProfileType.TextDependentVerification` ye geçitirsiniz `VoiceProfileClient.createProfileAsync` .

Sonra, ileri ' yi ve sonra tanımlayacaksınız iki yardımcı işlevi `AddEnrollmentsToTextDependentProfile` çağırabilirsiniz `SpeakerVerify` . Son olarak, profili kaldırmak için [Voiceprofileclient. deleteProfileAsync](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient#deleteprofileasync-voiceprofile---response--voiceprofileresult-----void---e--string-----void-) ' i çağırın.

### <a name="addenrollmentstotextdependentprofile-function"></a>AddEnrollmentsToTextDependentProfile işlevi

Bir ses profilini kaydetmek için aşağıdaki işlevi tanımlayın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="add_enrollments_dependent":::

Bu işlevde, `GetAudioConfigFromFile` daha önce tanımladığınız işlevi, `AudioConfig` Ses örneklerinden nesneler oluşturmak için çağırır. Bu ses örnekleri "My Voice My Passport, beni Doğrula" gibi bir parola içerir. Daha sonra bu ses örneklerini [Voiceprofileclient. Kayıtprofileasync](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient#enrollprofileasync-voiceprofile--audioconfig---e--voiceprofileenrollmentresult-----void---e--string-----void-) yöntemini kullanarak kaydedin.

### <a name="speakerverify-function"></a>Hoparlörkerverify işlevi

`SpeakerVerify`Aşağıdaki gibi tanımlayın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="speaker_verify":::

Bu işlevde, daha önce oluşturduğunuz [Voiceprofile](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofile) nesnesini geçirerek [Speakerlıtemicationmodel. fromprofile](/javascript/api/microsoft-cognitiveservices-speech-sdk/speakerverificationmodel#fromprofile-voiceprofile-) yöntemiyle bir [hoparlörkerdoğrulamaları icationmodel](/javascript/api/microsoft-cognitiveservices-speech-sdk/speakerverificationmodel) nesnesi oluşturacaksınız.

Ardından, daha önce kaydettiğiniz ses örnekleriyle aynı parolayı içeren bir ses örneğini doğrulamak için [SpeechRecognizer. recognizeOnceAsync](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#recognizeonceasync--e--speechrecognitionresult-----void---e--string-----void-) yöntemini çağırın. `SpeechRecognizer.recognizeOnceAsync` özelliği 0,0-1,0 ' y i değişen bir benzerlik puanı içeren bir [Hoparlörkerrecognitionresult](/javascript/api/microsoft-cognitiveservices-speech-sdk/speakerrecognitionresult) nesnesi döndürür `score` . `SpeakerRecognitionResult`Nesnesi `reason` [resultreason](/javascript/api/microsoft-cognitiveservices-speech-sdk/resultreason)türünde bir özellik de içerir. Doğrulama başarılı olduysa, `reason` özelliğin değeri olmalıdır `RecognizedSpeaker` .

## <a name="text-independent-verification"></a>Metnin bağımsız doğrulaması

**Metne bağımlı** doğrulamanın aksine, **metinden bağımsız** doğrulama:

* Belirli bir parolanın söyulmasına gerek yoktur, her şey söylenebilir
* Üç ses *örneği gerektirmez, ancak 20* saniye Toplam ses gerektirir

### <a name="textindependentverification-function"></a>Textindependentdoğrulama işlevi

İşlevi oluşturarak başlayın `TextIndependentVerification` .

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="text_independent_verification":::

İşlevi gibi `TextDependentVerification` , bu işlev, [Voiceprofileclient. createProfileAsync](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient#createprofileasync-voiceprofiletype--string---e--voiceprofile-----void---e--string-----void-) yöntemiyle bir [voiceprofile](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofile) nesnesi oluşturur.

Bu durumda, ' `VoiceProfileType.TextIndependentVerification` ye geçitirsiniz `createProfileAsync` .

Ardından, daha sonra `AddEnrollmentsToTextIndependentProfile` tanımladığınız ve daha önce tanımladığınız iki yardımcı işlevi çağırabilirsiniz: `SpeakerVerify` Son olarak, profili kaldırmak için [Voiceprofileclient. deleteProfileAsync](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient#deleteprofileasync-voiceprofile---response--voiceprofileresult-----void---e--string-----void-) ' i çağırın.

### <a name="addenrollmentstotextindependentprofile"></a>AddEnrollmentsToTextIndependentProfile

Bir ses profilini kaydetmek için aşağıdaki işlevi tanımlayın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="add_enrollments_independent":::

Bu işlevde, `GetAudioConfigFromFile` daha önce tanımladığınız işlevi, `AudioConfig` Ses örneklerinden nesneler oluşturmak için çağırır. Daha sonra bu ses örneklerini [Voiceprofileclient. Kayıtprofileasync](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient#enrollprofileasync-voiceprofile--audioconfig---e--voiceprofileenrollmentresult-----void---e--string-----void-) yöntemini kullanarak kaydedin.

## <a name="speaker-identification"></a>Konuşmacı belirleme

Konuşmacı kimliği, belirli bir kayıtlı ses grubundan **kimin** konuşduğunu tespit etmek için kullanılır. Bu işlem, tek bir profille doğrulanması yerine, aynı anda birden çok ses profiline karşı bir kez kimlik doğrulaması yapabilmekle birlikte, **metnin bağımsız doğrulamasına** benzer.

### <a name="textindependentidentification-function"></a>TextIndependentIdentification işlevi

İşlevi oluşturarak başlayın `TextIndependentIdentification` .

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="text_independent_indentification":::

`TextDependentVerification`Ve işlevleri gibi `TextIndependentVerification` , bu Işlev, [Voiceprofileclient. createProfileAsync](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient#createprofileasync-voiceprofiletype--string---e--voiceprofile-----void---e--string-----void-) yöntemiyle bir [voiceprofile](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofile) nesnesi oluşturur.

Bu durumda, ' `VoiceProfileType.TextIndependentIdentification` ye geçitirsiniz `VoiceProfileClient.createProfileAsync` .

Daha sonra, daha `AddEnrollmentsToTextIndependentProfile` önce tanımladığınız ve daha sonra tanımlayacağımız iki yardımcı işlevi çağırabilirsiniz: `SpeakerIdentify` Son olarak, profili kaldırmak için [Voiceprofileclient. deleteProfileAsync](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient#deleteprofileasync-voiceprofile---response--voiceprofileresult-----void---e--string-----void-) ' i çağırın.

### <a name="speakeridentify-function"></a>Hoparlörkerbelirlenmesi işlevi

`SpeakerIdentify`İşlevi aşağıdaki gibi tanımlayın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="speaker_identify":::

Bu işlevde, daha önce oluşturduğunuz [Voiceprofile](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofile) nesnesini geçirerek [Speakerıdencertificate. fromprofiles](/javascript/api/microsoft-cognitiveservices-speech-sdk/speakeridentificationmodel#fromprofiles-voiceprofile---) yöntemiyle birlikte bir [hoparlörkerationcertificate](/javascript/api/microsoft-cognitiveservices-speech-sdk/speakeridentificationmodel) oluşturacaksınız.

Ardından, [SpeechRecognizer. recognizeOnceAsync](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechrecognizer#recognizeonceasync--e--speechrecognitionresult-----void---e--string-----void-) yöntemini çağırır ve bir ses örneğine geçitirsiniz.
`SpeechRecognizer.recognizeOnceAsync` oluşturmak için kullandığınız nesnelere göre bu ses örneğinin sesini belirlemeyi dener `VoiceProfile` `SpeakerIdentificationModel` . Özelliği eşleştirmeyi tanımlayan bir [Speakerrecognıtionresult](/javascript/api/microsoft-cognitiveservices-speech-sdk/speakerrecognitionresult) nesnesi döndürür; `profileId` `VoiceProfile` ancak özellik, 0,0-1,0 ' y i `score` değişen bir benzerlik puanı içerir.

## <a name="main-function"></a>Main işlevi

Son olarak, `main` işlevi aşağıdaki gibi tanımlayın.

:::code language="javascript" source="~/cognitive-services-quickstart-code/javascript/speech/speaker-recognition.js" id="main":::

Bu işlev, Ses profilleri oluşturmak, kaydetmek ve silmek için kullanılan bir [Voiceprofileclient](/javascript/api/microsoft-cognitiveservices-speech-sdk/voiceprofileclient) nesnesi oluşturur. Ardından daha önce tanımladığınız işlevleri çağırır.