---
title: 'Hızlı başlangıç: konuşmayı, amaçları ve varlıkları tanıma, C++ -konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 01/02/2020
ms.topic: include
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: d421de69f856790b89c866a6c8b7221e9214aef4
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772986"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* Bu ilk C++ projeniz ise, <a href="~/articles/cognitive-services/Speech-Service/quickstarts/create-project.md?tabs=windows" target="_blank">boş bir örnek proje oluşturmak</a>için bu kılavuzu kullanın.
* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=windows" target="_blank">Geliştirme ortamınız Için konuşma SDK 'Sını yükler</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Amaç tanıma için bir LUSıS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio'da açın.

Ardından, projenizi Visual Studio 'da açın.

1. Visual Studio 2019 ' i başlatın.
2. Projenizi yükleyin ve `helloworld.cpp`açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim. `recognizeIntent()`adlı zaman uyumsuz bir yöntem oluşturduğunuzdan emin olun.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-16,73-81)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesnesi başlatabilmeniz için, LUSıS tahmin kaynağınız için anahtar ve konum kullanan bir yapılandırma oluşturmanız gerekir. 

> [!IMPORTANT]
> Başlangıç anahtarınız ve yazma anahtarlarınız çalışmayacak. Daha önce oluşturduğunuz tahmin anahtarınızı ve konumunuzu kullanmanız gerekir. Daha fazla bilgi için bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition). 

`recognizeIntent()` yöntemine bu kodu ekleyin. Bu değerleri güncelleştirdiğinizden emin olun:

* `"YourLanguageUnderstandingSubscriptionKey"` değerini LUSıS tahmin anahtarınızla değiştirin. 
* `"YourLanguageUnderstandingServiceRegion"` değerini LUSıS konumunuz ile değiştirin. 

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=25)]

Bu örnek, `SpeechConfig`oluşturmak için `FromSubscription()` yöntemini kullanır. Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](https://docs.microsoft.com/cpp/cognitive-services/speech/speechconfig).

Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="initialize-an-intentrecognizer"></a>Bir Yoğunlumtanıyıcıyı başlatma

Şimdi bir `IntentRecognizer`oluşturalım. Bu kodu, konuşma yapılandırmanızın hemen altına `recognizeIntent()` yöntemine ekleyin.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=28)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>LanguageUnderstandingModel ve amaçlar ekleyin

Bir `LanguageUnderstandingModel`, amaç tanıyıcı ile ilişkilendirmeniz ve tanınan hedefleri eklemeniz gerekir. Ana otomasyon için önceden oluşturulmuş etki alanındaki amaçları kullanacağız. 

Bu kodu `IntentRecognizer`altına ekleyin. `"YourLanguageUnderstandingAppId"`, LUSıS uygulama KIMLIĞINIZ ile değiştirdiğinizden emin olun. 

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=31-34)]

## <a name="recognize-an-intent"></a>Amacı tanıma

`IntentRecognizer` nesnesinden `RecognizeOnceAsync()` yöntemini çağıracağız. Bu yöntem, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin konuşmayı tanımayı durdur olarak belirlenmesinin ardından olduğunu bilmesini sağlar. Basitlik sağlamak için gelecekte işlemin tamamlanmasını bekliyoruz.

Bu kodu modelinizin altına ekleyin:

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=44)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüleme

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutmaya ve sonucu konsola yazdıracağız.

Aşağıdaki kodu `auto result = recognizer->RecognizeOnceAsync().get();`ekleyin:

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=47-72)]

## <a name="check-your-code"></a>Kodunuzu denetleyin

Bu noktada, kodunuzun şöyle görünmesi gerekir:  

> [!NOTE]
> Bu sürüme bazı açıklamalar ekledik.

[!code-cpp[](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/intent-recognition/helloworld/helloworld.cpp?range=6-81)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

1. **Kodu derleyin** -Visual Studio menü çubuğundan **derleme** > **Build Solution**' ı seçin.
2. **Uygulamanızı başlatın** -menü çubuğundan **Hata Ayıkla** > hata **ayıklamayı Başlat** ' ı seçin veya **F5**tuşuna basın.
3. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]