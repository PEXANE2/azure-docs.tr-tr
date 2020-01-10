---
title: 'Hızlı başlangıç: konuşmayı, amaçları ve varlıkları tanıma, Java-konuşma hizmeti'
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
ms.openlocfilehash: 7654d952469e35c3b45af252e910cb0e36693029
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772888"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* Bu, ilk Java (JRE) projem ise, <a href="~/articles/cognitive-services/Speech-Service/quickstarts/create-project.md?tabs=jre" target="_blank">boş bir örnek proje oluşturmak</a>için bu kılavuzu kullanın.
* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md?tabs=jre" target="_blank">Geliştirme ortamınız Için konuşma SDK 'Sını yükler</a>.

## <a name="create-a-luis-app-for-intent-recognition"></a>Amaç tanıma için bir LUSıS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project"></a>Projenizi açın

1. Tercih ettiğiniz IDE 'yi açın.
2. Projenizi yükleyin ve `Main.java`açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-20,69-76)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesnesi başlatabilmeniz için, LUSıS tahmin kaynağınız için anahtar ve konum kullanan bir yapılandırma oluşturmanız gerekir.  

Bu kodu `main()`try/catch bloğuna ekleyin. Bu değerleri güncelleştirdiğinizden emin olun:

* `"YourLanguageUnderstandingSubscriptionKey"` değerini LUSıS tahmin anahtarınızla değiştirin. 
* `"YourLanguageUnderstandingServiceRegion"` değerini LUSıS konumunuz ile değiştirin. 

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=27)]

Bu örnek, `SpeechConfig`oluşturmak için `FromSubscription()` yöntemini kullanır. Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="initialize-an-intentrecognizer"></a>Bir Yoğunlumtanıyıcıyı başlatma

Şimdi bir `IntentRecognizer`oluşturalım. Konuşma yapılandırmanızın hemen altına bu kodu ekleyin.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=30)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>LanguageUnderstandingModel ve amaçlar ekleyin

Bir `LanguageUnderstandingModel`, amaç tanıyıcı ile ilişkilendirmeniz ve tanınan hedefleri eklemeniz gerekir. Ana otomasyon için önceden oluşturulmuş etki alanındaki amaçları kullanacağız. 

Bu kodu `IntentRecognizer`altına ekleyin. `"YourLanguageUnderstandingAppId"`, LUSıS uygulama KIMLIĞINIZ ile değiştirdiğinizden emin olun. 

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa bkz. [Amaç tanıma için BIR lusıs uygulaması oluşturma](#create-a-luis-app-for-intent-recognition).

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=33-36)]

## <a name="recognize-an-intent"></a>Amacı tanıma

`IntentRecognizer` nesnesinden `recognizeOnceAsync()` yöntemini çağıracağız. Bu yöntem, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin konuşmayı tanımayı durdur olarak belirlenmesinin ardından olduğunu bilmesini sağlar.

Bu kodu modelinizin altına ekleyin:

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=41)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüleme

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutmaya ve sonucu konsola yazdıracağız.

`recognizeOnceAsync()`çağrın altına bu kodu ekleyin: [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=44-65)]

## <a name="release-resources"></a>Yayın kaynakları

Bunları kullanarak işiniz bittiğinde konuşma kaynaklarını serbest bırakmanız önemlidir. Try/catch bloğunun sonuna bu kodu ekleyin:

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=67-68)]

## <a name="check-your-code"></a>Kodunuzu denetleyin

Bu noktada, kodunuzun şöyle görünmesi gerekir:  

> [!NOTE]
> Bu sürüme bazı açıklamalar ekledik.

[!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/intent-recognition/src/speechsdk/quickstart/Main.java?range=6-76)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

F11 tuşuna basın veya **Çalıştır** > **Hata Ayıkla** seçeneğini belirleyin.
Mikrofonunuzdan yapılan sonraki 15 saniyelik konuşma girişi tanınır ve konsol penceresinde günlüğe kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]