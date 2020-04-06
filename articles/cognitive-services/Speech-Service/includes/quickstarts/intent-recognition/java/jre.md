---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: dapine
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 247b2a75af675c95745153e20b923d004a2f2f79
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80671607"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=jre&pivots=programming-language-java" target="_blank">Geliştirme ortamınız için<span class="docon docon-navigate-external x-hidden-focus"></span>Konuşma SDK'sını yükleyin ve boş bir örnek proje oluşturun.</a>

## <a name="create-a-luis-app-for-intent-recognition"></a>Niyet tanıma için bir LUIS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project"></a>Projenizi açın

1. Tercih ettiğiniz IDE'yi açın.
2. Projenizi yükleyin `Main.java`ve açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-20,68-75)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesneyi başlatmadan önce, LUIS tahmin kaynağınızın anahtarını ve konumunu kullanan bir yapılandırma oluşturmanız gerekir.  

Bu kodu try / catch bloğuna `main()`ekleyin. Bu değerleri güncelleştirdiğinden emin olun:

* LUIS `"YourLanguageUnderstandingSubscriptionKey"` tahmin anahtarınızla değiştirin.
* LUIS `"YourLanguageUnderstandingServiceRegion"` konumunuzla değiştirin. [Bölgeden](https://aka.ms/speech/sdkregion) **Bölge tanımlayıcısı** kullanma

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa, [bkz.](#create-a-luis-app-for-intent-recognition)

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=27)]

Bu örnek, `FromSubscription()` `SpeechConfig`'yi oluşturmak için yöntemi kullanır. Kullanılabilir yöntemlerin tam listesi için [SpeechConfig Class'a](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)bakın.

Konuşma SDK dil için en-us kullanarak tanıma varsayılan olacaktır, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../../how-to-specify-source-language.md) bakın.

## <a name="initialize-an-intentrecognizer"></a>Bir IntentRecognizer'ı Başlatma

Şimdi, bir. `IntentRecognizer` Bu kodu Konuşma yapılandırmanızın hemen altına ekleyin.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=30)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Bir LanguageUnderstandingModel ve Niyetler ekleyin

A'yı `LanguageUnderstandingModel` niyet tanıyan kişiyle ilişkilendirmeniz ve tanınmak istediğiniz amaçları eklemeniz gerekir. Ev otomasyonu için önceden oluşturulmuş alan dan gelen niyetleri kullanacağız.

Bu kodu aşağıdaki `IntentRecognizer`kodu ekleyin. LUIS uygulama kimliğinizle değiştirdiğinizden `"YourLanguageUnderstandingAppId"` emin olun.

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa, [bkz.](#create-a-luis-app-for-intent-recognition)

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=33-35)]

## <a name="recognize-an-intent"></a>Bir niyeti tanıma

Nesneden, `IntentRecognizer` `recognizeOnceAsync()` yöntemi aramalısınız. Bu yöntem, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar.

Bu kodu modelinizin altına ekleyin:

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=40)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hataları) görüntüleme

Tanıma sonucu Konuşma hizmeti tarafından döndürüldüğünde, bununla ilgili bir şey yapmak istersiniz. Basit tutacağız ve sonucu konsola yazdıracağız.

Bu kodu çağrınızın altına `recognizeOnceAsync()`ekle.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=43-64)]

## <a name="release-resources"></a>Yayın Kaynakları

Bunları kullanmayı bitirdiğinizde konuşma kaynaklarını serbest bırakmak önemlidir. Bu kodu try / catch bloğunun sonuna ekleyin:

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=66-67)]

## <a name="check-your-code"></a>Kodunuzu kontrol edin

Bu noktada, kodunuz şu şekilde görünmelidir:

> [!NOTE]
> Bu sürüme bazı yorumlar ekledik.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-75)]

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

<kbd>F11</kbd>tuşuna basın veya**Hata Ayıklama'yı** **çalıştır'ı** > seçin.
Mikrofonunuzdan yapılan sonraki 15 saniyelik konuşma girişi tanınır ve konsol penceresinde günlüğe kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
