---
title: 'Hızlı başlangıç: konuşmayı, amaçları ve varlıkları tanıma, C# -konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: a54d01aa78ad77c9328f79f31d21da570e7f5676
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73503825"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Bir LUSıS uygulaması oluşturma ve bir uç nokta anahtarı edinme](../../../../quickstarts/create-luis.md)
> * [Geliştirme ortamınızı kurma](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Boş bir örnek proje oluşturun](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio 'da açın

İlk adım, projenizin Visual Studio 'da açık olduğundan emin olmak.

1. Visual Studio 2019 ' i başlatın.
2. Projenizi yükleyin ve `Program.cs`açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim. `RecognizeIntentAsync()`adlı zaman uyumsuz bir yöntem oluşturduğunuzdan emin olun.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=7-17,77-86)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesnesi başlatabilmeniz için, LUSıS Endpoing anahtarınızı ve bölgenizi kullanan bir yapılandırma oluşturmanız gerekir. `RecognizeIntentAsync()` yöntemine bu kodu ekleyin.

Bu örnek, `SpeechConfig`oluşturmak için `FromSubscription()` yöntemini kullanır. Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

> [!NOTE]
> Yalnızca uç nokta anahtarı konuşma amacı tanıma için geçerli olduğundan, HALME uç noktası anahtarını kullanmak önemlidir. Doğru anahtar alma hakkında yönergeler için bkz. [lusıs uygulaması oluşturma ve bir uç nokta anahtarı alma](~/articles/cognitive-services/Speech-Service/quickstarts/create-luis.md) .

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=26)]

## <a name="initialize-a-intentrecognizer"></a>Bir Yoğunlumtanıyıcıyı başlatma

Şimdi bir `IntentRecognizer`oluşturalım. Yönetilmeyen kaynakların doğru şekilde yayınlanmasıyla emin olmak için bir using ifadesinin içinde bu nesne oluşturulur. Bu kodu, konuşma yapılandırmanızın hemen altına `RecognizeIntentAsync()` yöntemine ekleyin.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=28-30,76)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>LanguageUnderstandingModel ve amaçlar ekleyin

Artık bir `LanguageUnderstandingModel` amaç tanıyıcı ile ilişkilendirmeniz ve tanınan hedefleri eklemeniz gerekir.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=31-35)]

## <a name="recognize-an-intent"></a>Amacı tanıma

`IntentRecognizer` nesnesinden `RecognizeOnceAsync()` yöntemini çağıracağız. Bu yöntem, konuşma hizmeti 'nin tanıma için tek bir tümcecik gönderdiğini ve tümcecik reconizing konuşmayı durdurma için tanımlandıktan sonra olduğunu bilmesini sağlar.

Using ifadesinin içinde şu kodu ekleyin: [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=46)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüleme

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutmaya ve sonucu konsola yazdıracağız.

Using ifadesinin içinde, `RecognizeOnceAsync()`aşağıdaki kodu ekleyin: [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=48-75)]

## <a name="check-your-code"></a>Kodunuzu denetleyin

Bu noktada, kodunuz şuna benzemelidir: (Bu sürüme bazı açıklamalar ekledik) [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/intent-recognition/helloworld/Program.cs?range=5-86)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

1. **Kodu derleyin** -Visual Studio menü çubuğundan **derleme** > **Build Solution**' ı seçin.
2. **Uygulamanızı başlatın** -menü çubuğundan **Hata Ayıkla** > hata **ayıklamayı Başlat** ' ı seçin veya **F5**tuşuna basın.
3. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
