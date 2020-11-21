---
author: v-jaswel
ms.service: cognitive-services
ms.topic: include
ms.date: 09/28/2020
ms.author: v-jawe
ms.custom: references_regions
ms.openlocfilehash: 3a32946e10e8a81394b54fc44e4c92e8625e7ad6
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95015500"
---
Bu hızlı başlangıçta, konuşma SDK 'sını kullanarak konuşmacı tanıma için temel tasarım düzenlerini öğrenirsiniz; örneğin:

* Metne bağımlı ve metinden bağımsız doğrulama
* Sesler grubu arasında bir ses örneği tanımlamak için konuşmacı tanımlaması
* Ses profillerini silme

Konuşma tanıma kavramlarının üst düzey bir görünümü için bkz. [genel bakış](../../../speaker-recognition-overview.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede bir Azure hesabınız ve konuşma hizmeti aboneliğiniz olduğunu varsaymaktadır. Hesabınız ve aboneliğiniz yoksa [konuşma hizmetini ücretsiz deneyin](../../../overview.md#try-the-speech-service-for-free).

> [!IMPORTANT]
> Konuşmacı tanıma özelliği şu anda *yalnızca* bölgede oluşturulan Azure konuşma kaynaklarında desteklenmektedir `westus` .

## <a name="text-dependent-verification"></a>Metne bağımlı doğrulama

Konuşmacı Doğrulama, konuşmacının bilinen veya **kayıtlı** bir sesle eşleştiğini onaylama işlemidir. İlk adım, bir ses profilini **kaydetmek** ve bu sayede hizmetin gelecekteki sesli örnekleri karşılaştırmak için bir şey olması gerekir. Bu örnekte, profili, hem kayıt hem de doğrulama için kullanılmak üzere belirli bir parola gerektiren **metne bağlı** bir strateji kullanarak kaydedeolursunuz. Desteklenen parola listesi için [başvuru belgelerine](/rest/api/speakerrecognition/) bakın.

[Bir ses profili oluşturarak](/rest/api/speakerrecognition/verification/textdependent/createprofile)başlayın. Bu makaledeki her bir kıvrımlı komuta konuşma hizmeti abonelik anahtarınızı ve bölgenizi eklemeniz gerekir.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tdv_create_profile":::

Üç tür ses profili vardır:

- Metne bağımlı doğrulama
- Metnin bağımsız doğrulaması
- Metnin bağımsız tanımlayıcısı

Bu durumda, metne bağlı bir doğrulama Ses profili oluşturursunuz. Aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tdv_create_profile_response":::

Ardından, [ses profilini kaydedin](/rest/api/speakerrecognition/verification/textdependent/createenrollment). `--data-binary`Parametre değeri için, bilgisayarınızda desteklenen parolaları içeren bir ses dosyası belirtin, örneğin "My Voice My Passport sunucum, beni Doğrula" gibi. Bu tür bir ses dosyasını [Windows Voice Kaydedicisi](https://www.microsoft.com/p/windows-voice-recorder/9wzdncrfhwkn?activetab=pivot:overviewtab)gibi bir uygulamayla kaydedebilir veya [metin okuma](../../../index-text-to-speech.yml)kullanarak oluşturabilirsiniz.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tdv_enroll":::

Aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tdv_enroll_response_1":::

Bu yanıt, iki ses örneği kaydetmeniz gerektiğini söyler.

Toplam üç ses örneğini kaydettikten sonra, aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tdv_enroll_response_2":::

Artık [bir ses örneğini ses profiline karşı doğrulamaya](/rest/api/speakerrecognition/verification/textdependent/verifyprofile)hazırsınız. Bu ses örneği, ses profilini kaydetmek için kullandığınız örneklerle aynı parolayı içermelidir.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tdv_verify":::

Aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tdv_verify_response":::

, `Accept` Parolanın eşleştiği ve doğrulamanın başarılı olduğu anlamına gelir. Yanıt, 0,0-1,0 ' y i değişen bir benzerlik puanı da içerir.

Son olarak, [ses profilini silin](/rest/api/speakerrecognition/verification/textdependent/deleteprofile).

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tdv_delete_profile":::

Yanıt yok.

## <a name="text-independent-verification"></a>Metnin bağımsız doğrulaması

**Metne bağımlı** doğrulamanın aksine, **metinden bağımsız** doğrulama:

* Belirli bir parolanın söyulmasına gerek yoktur, her şey söylenebilir
* Üç ses *örneği gerektirmez, ancak 20* saniye Toplam ses gerektirir

[Bir metin bağımsız doğrulama profili oluşturarak](/rest/api/speakerrecognition/verification/textindependent/createprofile)başlayın.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tiv_create_profile":::

Aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tiv_create_profile_response":::

Ardından, [ses profilini kaydedin](/rest/api/speakerrecognition/verification/textindependent/createenrollment). Bu, üç ses örneği göndermek yerine, toplam 20 saniyelik ses içeren ses örnekleri göndermeniz gerekir.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tiv_enroll":::

Yeterli ses örneği gönderdikten sonra, aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tiv_enroll_response":::

Artık [bir ses örneğini ses profiline karşı doğrulamaya](/rest/api/speakerrecognition/verification/textindependent/verifyprofile)hazırsınız. Bu ses örneğinin bir parola içermesi gerekmez. En az dört saniyelik ses içerdiği sürece herhangi bir konuşma içerebilir.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tiv_verify":::

Aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tiv_verify_response":::

, `Accept` Doğrulamanın başarılı olduğu anlamına gelir. Yanıt, 0,0-1,0 ' y i değişen bir benzerlik puanı da içerir.

Son olarak, [ses profilini silin](/rest/api/speakerrecognition/verification/textindependent/deleteprofile).

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tiv_delete_profile":::

Yanıt yok.

## <a name="speaker-identification"></a>Konuşmacı belirleme

Konuşmacı kimliği, belirli bir kayıtlı ses grubundan **kimin** konuşduğunu tespit etmek için kullanılır. Bu işlem, tek bir profille doğrulanması yerine, aynı anda birden çok ses profiline karşı bir kez kimlik doğrulaması yapabilmekle birlikte, **metnin bağımsız doğrulamasına** benzer.

[Metin bağımsız bir kimlik profili oluşturarak](/rest/api/speakerrecognition/identification/textindependent/createprofile)başlayın.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tii_create_profile":::

Aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tii_create_profile_response":::

Ardından, [ses profilini kaydedin](/rest/api/speakerrecognition/identification/textindependent/createenrollment). Yine, toplam 20 saniyelik ses içeren ses örnekleri göndermeniz gerekir. Bu örneklerin bir parola içermesi gerekmez.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tii_enroll":::

Yeterli ses örneği gönderdikten sonra, aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tii_enroll_response_2":::

Artık ses [profilini kullanarak bir ses örneğini belirlemek](/rest/api/speakerrecognition/identification/textindependent/identifysinglespeaker)için hazırsınız. Tanımla komutu, olası ses profili kimliklerinin virgülle ayrılmış bir listesini kabul eder. Bu durumda, yalnızca daha önce oluşturduğunuz ses profilinin KIMLIĞINI geçireceğiz. Bununla birlikte, isterseniz, her bir ses profilinin farklı bir sesten ses örneklerine kaydolduğu birden fazla ses profili kimliği geçirebilirsiniz.

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tii_identify":::

Aşağıdaki yanıtı almalısınız.

:::code language="json" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tii_identify_response":::

Yanıt, gönderdiğiniz ses örneği ile en yakından eşleşen ses profilinin KIMLIĞINI içerir. Ayrıca, benzerlik sırasına göre derecelendirilen aday ses profillerinin bir listesini içerir.

Son olarak, [ses profilini silin](/rest/api/speakerrecognition/identification/textindependent/deleteprofile).

:::code language="curl" source="~/cognitive-services-quickstart-code/curl/speech/speaker-recognition.sh" id="tii_delete_profile":::

Yanıt yok.