---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: trbye
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 54b5e3f0ed2fd10fa04305fdefbec725143f62c5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422194"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp" target="_blank">Geliştirme ortamınız için Konuşma SDK'sını bir<span class="docon docon-navigate-external x-hidden-focus"></span>örnek proje oluştur ve boş layın.</a>

## <a name="create-a-luis-app-for-intent-recognition"></a>Niyet tanıma için bir LUIS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project-in-visual-studio"></a>Görsel Stüdyo'da projenizi açın

Ardından, Visual Studio'da projenizi açın.

1. Görsel Stüdyo 2019'u başlatın.
2. Projenizi yükleyin `Program.cs`ve açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim. 'li bir `RecognizeIntentAsync()`async yöntemi oluşturduğunuza dikkat edin.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=7-17,77-86)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesneyi başlatmadan önce, LUIS tahmin kaynağınızın anahtarını ve konumunu kullanan bir yapılandırma oluşturmanız gerekir.

> [!IMPORTANT]
> Başlangıç anahtarınız ve yazma tuşlarınız çalışmaz. Daha önce oluşturduğunuz tahmin anahtarınızı ve konumunuzu kullanmanız gerekir. Daha fazla bilgi için [bkz.](#create-a-luis-app-for-intent-recognition)

Yönteme `RecognizeIntentAsync()` bu kodu ekleyin. Bu değerleri güncelleştirdiğinden emin olun:

* LUIS `"YourLanguageUnderstandingSubscriptionKey"` tahmin anahtarınızla değiştirin.
* LUIS `"YourLanguageUnderstandingServiceRegion"` konumunuzla değiştirin. [Bölgeden](https://aka.ms/speech/sdkregion) **Bölge tanımlayıcısı** kullanın.

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa, [bkz.](#create-a-luis-app-for-intent-recognition)

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=26)]

Bu örnek, `FromSubscription()` `SpeechConfig`'yi oluşturmak için yöntemi kullanır. Kullanılabilir yöntemlerin tam listesi için [SpeechConfig Class'a](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)bakın.

Konuşma SDK dil için en-us kullanarak tanıma varsayılan olacaktır, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../../how-to-specify-source-language.md) bakın.

## <a name="initialize-an-intentrecognizer"></a>Bir IntentRecognizer'ı Başlatma

Şimdi, bir. `IntentRecognizer` Bu nesne, yönetilmeyen kaynakların doğru şekilde serbest bırakılmasını sağlamak için kullanılan bir deyimin içinde oluşturulur. Bu kodu yönteme, `RecognizeIntentAsync()` Konuşma yapılandırmanızın hemen altına ekleyin.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=29-30,76)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Bir LanguageUnderstandingModel ve niyetler ekleyin

A'yı `LanguageUnderstandingModel` niyet tanıyan kişiyle ilişkilendirmeniz ve tanınmasını istediğiniz amaçları eklemeniz gerekir. Ev otomasyonu için önceden oluşturulmuş alan dan gelen niyetleri kullanacağız. Bu kodu önceki bölümden gelen kullanarak ifadeye ekleyin. LUIS uygulama kimliğinizle değiştirdiğinizden `"YourLanguageUnderstandingAppId"` emin olun.

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa, [bkz.](#create-a-luis-app-for-intent-recognition)

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=33-35)]

## <a name="recognize-an-intent"></a>Bir niyeti tanıma

Nesneden, `IntentRecognizer` `RecognizeOnceAsync()` yöntemi aramalısınız. Bu yöntem, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar.

Kullanarak ifadenin içine, bu kodu modelinizin altına ekleyin.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=46)]

## <a name="display-recognition-results-or-errors"></a>Görüntüleme tanıma sonuçları (veya hatalar)

Tanıma sonucu Konuşma hizmeti tarafından döndürüldüğünde, bununla ilgili bir şey yapmak istersiniz. Basit tutacağız ve sonuçları konsola yazdıracağız.

Aşağıdaki kullanarak deyimi `RecognizeOnceAsync()`içinde, bu kodu ekleyin:

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=49-75)]

## <a name="check-your-code"></a>Kodunuzu kontrol edin

Bu noktada, kodunuz şu şekilde görünmelidir:

> [!NOTE]
> Bu sürüme bazı yorumlar ekledik.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=7-86)]

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Artık uygulamanızı oluşturmaya ve Konuşma hizmetini kullanarak konuşma tanımamızı test etmeye hazırsınız.

1. **Kodu derle** - Visual Studio'nun menü çubuğundan **Build** > **Build Solution'ı**seçin.
2. **Uygulamanızı başlatın** - Menü çubuğundan **Hata** > **Ayıklama'yı** seçin veya <kbd>F5 tuşuna</kbd>basın.
3. **Tanımaya başlayın** - İngilizce bir ifade konuşmanızı ister. Konuşmanız Konuşma hizmetine gönderilir, metin olarak yazılır ve konsolda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
