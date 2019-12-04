---
title: 'Hızlı başlangıç: bir mikrofondan konuşmayı tanıma C# , (.net)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: 106a11959bdaf34f44faa0c6a2b031ccba537835
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74796111"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurma](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Boş bir örnek proje oluşturun](../../../../quickstarts/create-project.md?tabs=dotnet)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio 'da açın

İlk adım, projenizin Visual Studio 'da açık olduğundan emin olmak.

1. Visual Studio 2019 ' i başlatın.
2. Projenizi yükleyin ve `Program.cs`açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim. `RecognizeSpeechAsync()`adlı zaman uyumsuz bir yöntem oluşturduğunuzdan emin olun.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=5-15,43-52)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `SpeechRecognizer` nesnesi başlatabilmeniz için önce abonelik anahtarınızı ve abonelik bölgenizi kullanan bir yapılandırma oluşturmanız gerekir. `RecognizeSpeechAsync()` yöntemine bu kodu ekleyin.

> [!NOTE]
> Bu örnek, `SpeechConfig`oluşturmak için `FromSubscription()` yöntemini kullanır. Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=16)]
> Konuşma SDK 'Sı, dil için en-US kullanarak varsayılan olarak tanıma yapılır, kaynak dili seçme hakkında bilgi için bkz. [konuşmayı için kaynak dilini belirtme](../../../../how-to-specify-source-language.md) .

## <a name="initialize-a-speechrecognizer"></a>SpeechRecognizer başlatma

Şimdi bir `SpeechRecognizer`oluşturalım. Yönetilmeyen kaynakların doğru şekilde yayınlanmasıyla emin olmak için bir using ifadesinin içinde bu nesne oluşturulur. Bu kodu, konuşma yapılandırmanızın hemen altına `RecognizeSpeechAsync()` yöntemine ekleyin.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=17-19,42)]

## <a name="recognize-a-phrase"></a>Bir tümceciği tanıma

`SpeechRecognizer` nesnesinden `RecognizeOnceAsync()` yöntemini çağıracağız. Bu yöntem, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin konuşmayı tanımayı durdur olarak belirlenmesinin ardından olduğunu bilmesini sağlar.

Using ifadesinin içinde şu kodu ekleyin: [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=20)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüleme

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutmaya ve sonucu konsola yazdıracağız.

Using ifadesinin içinde, `RecognizeOnceAsync()`aşağıdaki kodu ekleyin: [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=22-41)]

## <a name="check-your-code"></a>Kodunuzu denetleyin

Bu noktada, kodunuzun şöyle görünmesi gerekir: [!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs)]

## <a name="build-and-run-your-app"></a>Uygulamanızı derleyin ve çalıştırın

Artık uygulamanızı oluşturmaya ve konuşma tanıma özelliğini kullanarak konuşma tanıma 'yı test etmeye hazır olursunuz.

1. **Kodu derleyin** -Visual Studio menü çubuğundan **derleme** > **Build Solution**' ı seçin.
2. **Uygulamanızı başlatın** -menü çubuğundan **Hata Ayıkla** > hata **ayıklamayı Başlat** ' ı seçin veya **F5**tuşuna basın.
3. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
