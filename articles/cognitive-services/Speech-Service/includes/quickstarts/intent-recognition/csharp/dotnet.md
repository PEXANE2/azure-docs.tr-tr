---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 54b5e3f0ed2fd10fa04305fdefbec725143f62c5
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81422194"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp" target="_blank">Geliştirme ortamınız Için konuşma SDK 'sını, Create ve Empty örnek projesi<span class="docon docon-navigate-external x-hidden-focus"></span>olarak yükler</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Amaç tanıma için bir LUSıS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio 'da açın

Ardından, projenizi Visual Studio 'da açın.

1. Visual Studio 2019 ' i başlatın.
2. Projenizi yükleyin ve açın `Program.cs`.

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim. Adlı `RecognizeIntentAsync()`bir zaman uyumsuz yöntem oluşturduğunuza dikkat edin.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=7-17,77-86)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesneyi başlatabilmeniz IÇIN, lusıs tahmin kaynağınız için anahtar ve konum kullanan bir yapılandırma oluşturmanız gerekir.

> [!IMPORTANT]
> Başlangıç anahtarınız ve yazma anahtarlarınız çalışmayacak. Daha önce oluşturduğunuz tahmin anahtarınızı ve konumunuzu kullanmanız gerekir. Daha fazla bilgi için bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

Bu kodu `RecognizeIntentAsync()` yöntemine ekleyin. Bu değerleri güncelleştirdiğinizden emin olun:

* LUSıS tahmin anahtarınızla değiştirin `"YourLanguageUnderstandingSubscriptionKey"` .
* LUSıS konumunuz ile değiştirin `"YourLanguageUnderstandingServiceRegion"` . Bölgeden **bölge tanımlayıcısı** kullanın [.](https://aka.ms/speech/sdkregion)

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=26)]

Bu örnek, `SpeechConfig`oluşturmak `FromSubscription()` için yöntemini kullanır. Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="initialize-an-intentrecognizer"></a>Bir Yoğunlumtanıyıcıyı başlatma

Şimdi bir `IntentRecognizer`oluşturalım. Yönetilmeyen kaynakların doğru şekilde yayınlanmasıyla emin olmak için bir using ifadesinin içinde bu nesne oluşturulur. Bu kodu, konuşma yapılandırmanızın `RecognizeIntentAsync()` hemen altına, yöntemine ekleyin.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=29-30,76)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>LanguageUnderstandingModel ve amaçlar ekleyin

Bir `LanguageUnderstandingModel` öğesini amaç tanıyıcı ile ilişkilendirmeli ve tanınan amaçları eklemelisiniz. Ana otomasyon için önceden oluşturulmuş etki alanındaki amaçları kullanacağız. Önceki bölümden using ifadesine bu kodu ekleyin. LUSıS uygulama KIMLIĞINIZ ile `"YourLanguageUnderstandingAppId"` değiştirdiğinizden emin olun.

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=33-35)]

## <a name="recognize-an-intent"></a>Amacı tanıma

`IntentRecognizer` Nesnesinden `RecognizeOnceAsync()` yöntemi çağıracağız. Bu yöntem, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin konuşmayı tanımayı durdur olarak belirlenmesinin ardından olduğunu bilmesini sağlar.

Using ifadesinin içinde, bu kodu modelinizin altına ekleyin.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=46)]

## <a name="display-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüle

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutacağız ve sonuçları konsola yazdıracağız.

Using ifadesinin içinde, aşağıdaki `RecognizeOnceAsync()`kodu ekleyin:

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=49-75)]

## <a name="check-your-code"></a>Kodunuzu denetleyin

Bu noktada, kodunuzun şöyle görünmesi gerekir:

> [!NOTE]
> Bu sürüme bazı açıklamalar ekledik.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=7-86)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

1. **Kodu derleyin** -Visual Studio menü çubuğundan derleme**Build Solution**' **ı seçin.** > 
2. **Uygulamanızı başlatın** -menü çubuğundan hata**ayıklamayı Başlat hata** **Ayıkla** > ' yı seçin veya <kbd>F5</kbd>tuşuna basın.
3. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
