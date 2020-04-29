---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 58f771120c5c81e27751af66b64e146c79bb6508
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81422178"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=jre&pivots=programming-language-java" target="_blank">Geliştirme ortamınız Için konuşma SDK 'Sını yükleyip boş bir örnek proje<span class="docon docon-navigate-external x-hidden-focus"></span>oluşturun</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Amaç tanıma için bir LUSıS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project"></a>Projenizi açın

1. Tercih ettiğiniz IDE 'yi açın.
2. Projenizi yükleyin ve açın `Main.java`.

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-20,68-75)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesneyi başlatabilmeniz IÇIN, lusıs tahmin kaynağınız için anahtar ve konum kullanan bir yapılandırma oluşturmanız gerekir.  

Bu kodu içindeki `main()`try/catch bloğuna ekleyin. Bu değerleri güncelleştirdiğinizden emin olun:

* LUSıS tahmin anahtarınızla değiştirin `"YourLanguageUnderstandingSubscriptionKey"` .
* LUSıS konumunuz ile değiştirin `"YourLanguageUnderstandingServiceRegion"` . Bölgeden **bölge tanımlayıcısı** kullan [region](https://aka.ms/speech/sdkregion)

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=27)]

Bu örnek, `SpeechConfig`oluşturmak `FromSubscription()` için yöntemini kullanır. Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="initialize-an-intentrecognizer"></a>Bir Yoğunlumtanıyıcıyı başlatma

Şimdi bir `IntentRecognizer`oluşturalım. Konuşma yapılandırmanızın hemen altına bu kodu ekleyin.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=30)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>LanguageUnderstandingModel ve amaçlar ekleyin

Bir `LanguageUnderstandingModel` öğesini amaç tanıyıcı ile ilişkilendirmeniz ve tanınan hedefleri eklemeniz gerekir. Ana otomasyon için önceden oluşturulmuş etki alanındaki amaçları kullanacağız.

Buraya bu kodu ekleyin `IntentRecognizer`. LUSıS uygulama KIMLIĞINIZ ile `"YourLanguageUnderstandingAppId"` değiştirdiğinizden emin olun.

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=33-35)]

## <a name="recognize-an-intent"></a>Amacı tanıma

`IntentRecognizer` Nesnesinden `recognizeOnceAsync()` yöntemi çağıracağız. Bu yöntem, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin konuşmayı tanımayı durdur olarak belirlenmesinin ardından olduğunu bilmesini sağlar.

Bu kodu modelinizin altına ekleyin:

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=40)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüleme

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutmaya ve sonucu konsola yazdıracağız.

Çağrın altına bu kodu ekleyin `recognizeOnceAsync()`.

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

<kbd>F11</kbd>tuşuna basın veya**hata ayıklamayı** **Çalıştır** > ' ı seçin.
Mikrofonunuzdan yapılan sonraki 15 saniyelik konuşma girişi tanınır ve konsol penceresinde günlüğe kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
