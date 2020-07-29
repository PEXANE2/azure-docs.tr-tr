---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 26b3b3bd061d3ec7c3efd5c613c5beec33867022
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87298890"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=jre&pivots=programming-language-java" target="_blank">Geliştirme ortamınız Için konuşma SDK 'Sını yükleyip boş bir örnek proje <span class="docon docon-navigate-external x-hidden-focus"></span> oluşturun</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Amaç tanıma için bir LUSıS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project"></a>Projenizi açın

1. Tercih ettiğiniz IDE 'yi açın.
2. Projenizi yükleyin ve açın `Main.java` .

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-20,68-75)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir nesneyi başlatabilmeniz için `IntentRecognizer` , lusıs tahmin kaynağınız için anahtar ve konum kullanan bir yapılandırma oluşturmanız gerekir.  

Bu kodu içindeki try/catch bloğuna ekleyin `main()` . Bu değerleri güncelleştirdiğinizden emin olun:

* `"YourLanguageUnderstandingSubscriptionKey"`Lusıs tahmin anahtarınızla değiştirin.
* `"YourLanguageUnderstandingServiceRegion"`Lusıs konumunuz ile değiştirin. Bölgeden **bölge tanımlayıcısı** kullan [region](https://aka.ms/speech/sdkregion)

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=27)]

Bu örnek `FromSubscription()` , oluşturmak için yöntemini kullanır `SpeechConfig` . Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="initialize-an-intentrecognizer"></a>Bir Yoğunlumtanıyıcıyı başlatma

Şimdi bir oluşturalım `IntentRecognizer` . Konuşma yapılandırmanızın hemen altına bu kodu ekleyin.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=30)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>LanguageUnderstandingModel ve amaçlar ekleyin

Bir `LanguageUnderstandingModel` öğesini amaç tanıyıcı ile ilişkilendirmeniz ve tanınan hedefleri eklemeniz gerekir. Ana otomasyon için önceden oluşturulmuş etki alanındaki amaçları kullanacağız.

Buraya bu kodu ekleyin `IntentRecognizer` . `"YourLanguageUnderstandingAppId"`Lusıs uygulama kimliğiniz ile değiştirdiğinizden emin olun.

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=33-35)]

Bu örnek, `addIntent()` tek tek amaçları eklemek için işlevini kullanır. Bir modelden tüm amaçları eklemek istiyorsanız `addAllIntents(model)` modeli kullanın ve geçirin.

## <a name="recognize-an-intent"></a>Amacı tanıma

`IntentRecognizer`Nesnesinden yöntemi çağıracağız `recognizeOnceAsync()` . Bu yöntem, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin konuşmayı tanımayı durdur olarak belirlenmesinin ardından olduğunu bilmesini sağlar.

Bu kodu modelinizin altına ekleyin:

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=40)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüleme

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutmaya ve sonucu konsola yazdıracağız.

Çağrın altına bu kodu ekleyin `recognizeOnceAsync()` .

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=43-64)]

## <a name="release-resources"></a>Yayın kaynakları

Bunları kullanarak işiniz bittiğinde konuşma kaynaklarını serbest bırakmanız önemlidir. Try/catch bloğunun sonuna bu kodu ekleyin:

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=66-67)]

## <a name="check-your-code"></a>Kodunuzu denetleyin

Bu noktada, kodunuzun şöyle görünmesi gerekir:

> [!NOTE]
> Bu sürüme bazı açıklamalar ekledik.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-75)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

<kbd>F11</kbd>tuşuna basın veya **Run**  >  **hata ayıklamayı**Çalıştır ' ı seçin.
Mikrofonunuzdan yapılan sonraki 15 saniyelik konuşma girişi tanınır ve konsol penceresinde günlüğe kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
