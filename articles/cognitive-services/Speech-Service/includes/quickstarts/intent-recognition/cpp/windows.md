---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.topic: include
ms.author: dapine
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: abd69a49659ef745f81a2186ea97440cf35f20df
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80671675"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=windows&pivots=programming-language-cpp" target="_blank">Geliştirme ortamınız için<span class="docon docon-navigate-external x-hidden-focus"></span>Konuşma SDK'sını yükleyin ve boş bir örnek proje oluşturun.</a>

## <a name="create-a-luis-app-for-intent-recognition"></a>Niyet tanıma için bir LUIS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project-in-visual-studio"></a>Görsel Stüdyo'da projenizi açın

Ardından, Visual Studio'da projenizi açın.

1. Görsel Stüdyo 2019'u başlatın.
2. Projenizi yükleyin `helloworld.cpp`ve açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim. 'li bir `recognizeIntent()`async yöntemi oluşturduğunuza dikkat edin.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-16,72-80)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesneyi başlatmadan önce, LUIS tahmin kaynağınızın anahtarını ve konumunu kullanan bir yapılandırma oluşturmanız gerekir.

> [!IMPORTANT]
> Başlangıç anahtarınız ve yazma tuşlarınız çalışmaz. Daha önce oluşturduğunuz tahmin anahtarınızı ve konumunuzu kullanmanız gerekir. Daha fazla bilgi için [bkz.](#create-a-luis-app-for-intent-recognition)

Yönteme `recognizeIntent()` bu kodu ekleyin. Bu değerleri güncelleştirdiğinden emin olun:

* LUIS `"YourLanguageUnderstandingSubscriptionKey"` tahmin anahtarınızla değiştirin.
* LUIS `"YourLanguageUnderstandingServiceRegion"` konumunuzla değiştirin.  [Bölgeden](https://aka.ms/speech/sdkregion) **Bölge tanımlayıcısı** kullanın.

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa, [bkz.](#create-a-luis-app-for-intent-recognition)

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=25)]

Bu örnek, `FromSubscription()` `SpeechConfig`'yi oluşturmak için yöntemi kullanır. Kullanılabilir yöntemlerin tam listesi için [SpeechConfig Class'a](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig)bakın.

Konuşma SDK dil için en-us kullanarak tanıma varsayılan olacaktır, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../../how-to-specify-source-language.md) bakın.

## <a name="initialize-an-intentrecognizer"></a>Bir IntentRecognizer'ı Başlatma

Şimdi, bir. `IntentRecognizer` Bu kodu yönteme, `recognizeIntent()` Konuşma yapılandırmanızın hemen altına ekleyin.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=28)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Bir LanguageUnderstandingModel ve Niyetler ekleyin

A'yı `LanguageUnderstandingModel` niyet tanıyan kişiyle ilişkilendirmeniz ve tanınmak istediğiniz amaçları eklemeniz gerekir. Ev otomasyonu için önceden oluşturulmuş alan dan gelen niyetleri kullanacağız.

Bu kodu aşağıdaki `IntentRecognizer`kodu ekleyin. LUIS uygulama kimliğinizle değiştirdiğinizden `"YourLanguageUnderstandingAppId"` emin olun.

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa, [bkz.](#create-a-luis-app-for-intent-recognition)

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=31-33)]

## <a name="recognize-an-intent"></a>Bir niyeti tanıma

Nesneden, `IntentRecognizer` `RecognizeOnceAsync()` yöntemi aramalısınız. Bu yöntem, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar. Basitlik için geleceğin tamamlanmasını bekleyeceğiz.

Bu kodu modelinizin altına ekleyin:

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=43)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hataları) görüntüleme

Tanıma sonucu Konuşma hizmeti tarafından döndürüldüğünde, bununla ilgili bir şey yapmak istersiniz. Basit tutacağız ve sonucu konsola yazdıracağız.

Bu kodu aşağıdaki `auto result = recognizer->RecognizeOnceAsync().get();`ekle:

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=46-71)]

## <a name="check-your-code"></a>Kodunuzu kontrol edin

Bu noktada, kodunuz şu şekilde görünmelidir:

> [!NOTE]
> Bu sürüme bazı yorumlar ekledik.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-79)]

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Artık uygulamanızı oluşturmaya ve Konuşma hizmetini kullanarak konuşma tanımamızı test etmeye hazırsınız.

1. **Kodu derle** - Visual Studio'nun menü çubuğundan **Build** > **Build Solution'ı**seçin.
2. **Uygulamanızı başlatın** - Menü çubuğundan **Hata** > **Ayıklama'yı** seçin veya <kbd>F5 tuşuna</kbd>basın.
3. **Tanımaya başlayın** - İngilizce bir ifade konuşmanızı ister. Konuşmanız Konuşma hizmetine gönderilir, metin olarak yazılır ve konsolda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
