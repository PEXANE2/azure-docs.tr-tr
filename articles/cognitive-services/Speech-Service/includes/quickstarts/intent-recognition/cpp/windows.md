---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 08679bac77121ee2d276f3d2854e0b119c769582
ms.sourcegitcommit: 48e5379c373f8bd98bc6de439482248cd07ae883
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98109259"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=windows&pivots=programming-language-cpp" target="_blank">Geliştirme ortamınız Için konuşma SDK 'Sını yükleyip boş bir örnek proje <span class="docon docon-navigate-external x-hidden-focus"></span> oluşturun</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Amaç tanıma için bir LUSıS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio 'da açın

Ardından, projenizi Visual Studio 'da açın.

1. Visual Studio 2019 ' i başlatın.
2. Projenizi yükleyin ve açın `helloworld.cpp` .

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim. Adlı bir zaman uyumsuz yöntem oluşturduğunuza dikkat edin `recognizeIntent()` .

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-16,72-80)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir nesneyi başlatabilmeniz için `IntentRecognizer` , lusıs tahmin kaynağınız için anahtar ve konum kullanan bir yapılandırma oluşturmanız gerekir.

> [!IMPORTANT]
> Başlangıç anahtarınız ve yazma anahtarlarınız çalışmayacak. Daha önce oluşturduğunuz tahmin anahtarınızı ve konumunuzu kullanmanız gerekir. Daha fazla bilgi için bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

Bu kodu `recognizeIntent()` yöntemine ekleyin. Bu değerleri güncelleştirdiğinizden emin olun:

* `"YourLanguageUnderstandingSubscriptionKey"`Lusıs tahmin anahtarınızla değiştirin.
* `"YourLanguageUnderstandingServiceRegion"`Lusıs konumunuz ile değiştirin.  Bölgeden **bölge tanımlayıcısı** kullanın [.](../../../../regions.md)

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=25)]

Bu örnek `FromSubscription()` , oluşturmak için yöntemini kullanır `SpeechConfig` . Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](/cpp/cognitive-services/speech/speechconfig).

Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="initialize-an-intentrecognizer"></a>Bir Yoğunlumtanıyıcıyı başlatma

Şimdi bir oluşturalım `IntentRecognizer` . Bu kodu `recognizeIntent()` , konuşma yapılandırmanızın hemen altına, yöntemine ekleyin.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=28)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>LanguageUnderstandingModel ve amaçlar ekleyin

Bir `LanguageUnderstandingModel` öğesini amaç tanıyıcı ile ilişkilendirmeniz ve tanınan hedefleri eklemeniz gerekir. Ana otomasyon için önceden oluşturulmuş etki alanındaki amaçları kullanacağız.

Buraya bu kodu ekleyin `IntentRecognizer` . `"YourLanguageUnderstandingAppId"`Lusıs uygulama kimliğiniz ile değiştirdiğinizden emin olun.

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=31-33)]

Bu örnek, `AddIntent()` tek tek amaçları eklemek için işlevini kullanır. Bir modelden tüm amaçları eklemek istiyorsanız `AddAllIntents(model)` modeli kullanın ve geçirin.

> [!NOTE]
> Konuşma SDK 'Sı yalnızca LUG v 2.0 uç noktalarını destekler.
> Bir v 2.0 URL 'SI deseninin kullanılması için örnek sorgu alanında bulunan v 3.0 uç nokta URL 'sini el ile değiştirmeniz gerekir.
> LUSıS v 2.0 uç noktaları şu iki desenden birini her zaman izler:
> * `https://{AzureResourceName}.cognitiveservices.azure.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`
> * `https://{Region}.api.cognitive.microsoft.com/luis/v2.0/apps/{app-id}?subscription-key={subkey}&verbose=true&q=`

## <a name="recognize-an-intent"></a>Amacı tanıma

`IntentRecognizer`Nesnesinden yöntemi çağıracağız `RecognizeOnceAsync()` . Bu yöntem, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin konuşmayı tanımayı durdur olarak belirlenmesinin ardından olduğunu bilmesini sağlar. Basitlik sağlamak için gelecekte işlemin tamamlanmasını bekliyoruz.

Bu kodu modelinizin altına ekleyin:

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=43)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüleme

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutmaya ve sonucu konsola yazdıracağız.

Aşağıdaki kodu ekleyin `auto result = recognizer->RecognizeOnceAsync().get();` :

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=46-71)]

## <a name="check-your-code"></a>Kodunuzu denetleyin

Bu noktada, kodunuzun şöyle görünmesi gerekir:

> [!NOTE]
> Bu sürüme bazı açıklamalar ekledik.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-79)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

1. **Kodu derleyin** -Visual Studio menü **çubuğundan derleme**  >  **Build Solution**' ı seçin.
2. **Uygulamanızı başlatın** -menü çubuğundan hata   >  **ayıklamayı Başlat hata** Ayıkla ' yı seçin veya <kbd>F5</kbd>tuşuna basın.
3. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]