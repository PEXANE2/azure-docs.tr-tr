---
title: 'Quickstart: Bir mikrofon, C# (.NET) konuşma tanıma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/17/2019
ms.author: erhopf
ms.openlocfilehash: c969b5e5daa4c4cfd84695fef70f0a2a5c50ce02
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78924918"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

## <a name="open-your-project-in-visual-studio"></a>Görsel Stüdyo'da projenizi açın

İlk adım, visual studio'da projenizin açık olduğundan emin olmaktır.

1. Görsel Stüdyo 2019'u başlatın.
2. Projenizi yükleyin `Program.cs`ve açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim. 'li bir `RecognizeSpeechAsync()`async yöntemi oluşturduğunuza dikkat edin.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=5-15,43-52)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `SpeechRecognizer` nesneyi başlatmadan önce, abonelik anahtarınızı ve abonelik bölgenizi kullanan bir yapılandırma oluşturmanız gerekir [(bölgeden](https://aka.ms/speech/sdkregion) **Bölge tanımlayıcısını** seçin. Yönteme `RecognizeSpeechAsync()` bu kodu ekleyin.

> [!NOTE]
> Bu örnek, `FromSubscription()` `SpeechConfig`'yi oluşturmak için yöntemi kullanır. Kullanılabilir yöntemlerin tam listesi için [SpeechConfig Class'a](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet)bakın.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=16)]
> Konuşma SDK dil için en-us kullanarak tanıma varsayılan olacaktır, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../../how-to-specify-source-language.md) bakın.

## <a name="initialize-a-speechrecognizer"></a>Bir SpeechRecognizer'ı Başlatma

Şimdi, bir. `SpeechRecognizer` Bu nesne, yönetilmeyen kaynakların doğru şekilde serbest bırakılmasını sağlamak için kullanılan bir deyimin içinde oluşturulur. Bu kodu yönteme, `RecognizeSpeechAsync()` Konuşma yapılandırmanızın hemen altına ekleyin.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=17-19,42)]

## <a name="recognize-a-phrase"></a>Bir tümceciği tanıma

Nesneden, `SpeechRecognizer` `RecognizeOnceAsync()` yöntemi aramalısınız. Bu yöntem, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar.

Using deyiminin içine bu kodu ekleyin.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=20)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hataları) görüntüleme

Tanıma sonucu Konuşma hizmeti tarafından döndürüldüğünde, bununla ilgili bir şey yapmak istersiniz. Basit tutacağız ve sonucu konsola yazdıracağız.

Aşağıdaki kullanarak deyimi `RecognizeOnceAsync()`içinde, bu kodu ekleyin.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs?range=22-41)]

## <a name="check-your-code"></a>Kodunuzu kontrol edin

Bu noktada, kodunuz bu gibi görünmelidir.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/from-microphone/helloworld/Program.cs)]

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Artık uygulamanızı oluşturmaya ve Konuşma hizmetini kullanarak konuşma tanımamızı test etmeye hazırsınız.

1. **Kodu derle** - Visual Studio'nun menü çubuğundan **Build** > **Build Solution'ı**seçin.
2. **Uygulamanızı başlatın** - Menü çubuğundan **Hata** > **Ayıklama'yı** seçin veya **F5 tuşuna**basın.
3. **Tanımaya başlayın** - İngilizce bir ifade konuşmanızı ister. Konuşmanız Konuşma hizmetine gönderilir, metin olarak yazılır ve konsolda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
