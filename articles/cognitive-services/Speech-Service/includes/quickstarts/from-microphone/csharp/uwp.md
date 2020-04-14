---
title: 'Quickstart: Bir mikrofon, C# (UWP) - Konuşma hizmeti konuşma tanıma'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/02/2020
ms.author: erhopf
ms.openlocfilehash: 1c631f4dea3b182c97f11f3892dff834c7681507
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275391"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

Bunu zaten yaptıysan, harika. Devam edelim.

## <a name="open-your-project-in-visual-studio"></a>Görsel Stüdyo'da projenizi açın

İlk adım, visual studio'da projenizin açık olduğundan emin olmaktır.

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim.

1. **Çözüm Gezgini'nde**, açık `MainPage.xaml`.

2. Tasarımcının XAML görünümünde, **Grid** etiketine aşağıdaki XAML snippet'i `</Grid>`ekleyin (arasında ve arasında): `<Grid>`

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml#StackPanel)]

3. **Çözüm Gezgini'nde,** kod arkasındaki `MainPage.xaml.cs`kaynak dosyayı açın. (Bu altında `MainPage.xaml`gruplandı .)

4. Kodu aşağıdaki temel kodla değiştirin:

   [!code-csharp[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=6-50,55-56,94-154)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `SpeechRecognizer` nesneyi başlatmadan önce, abonelik anahtarınızı ve abonelik bölgenizi kullanan bir yapılandırma oluşturmanız gerekir. Yönteme `RecognizeSpeechAsync()` bu kodu ekleyin.

> [!NOTE]
> Bu örnek, `FromSubscription()` `SpeechConfig`'yi oluşturmak için yöntemi kullanır. Kullanılabilir yöntemlerin tam listesi için [SpeechConfig Class'a](https://docs.microsoft.com/dotnet/api/) bakın[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=51-53)]

## <a name="initialize-a-speechrecognizer"></a>Bir SpeechRecognizer'ı Başlatma

Şimdi, bir. `SpeechRecognizer` Bu nesne, yönetilmeyen kaynakların doğru şekilde serbest bırakılmasını sağlamak için kullanılan bir deyimin içinde oluşturulur. Bu kodu yönteme, `RecognizeSpeechAsync()` Konuşma yapılandırmanızın hemen altına ekleyin.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=58,59,93)]

## <a name="recognize-a-phrase"></a>Bir tümceciği tanıma

Nesneden, `SpeechRecognizer` `RecognizeOnceAsync()` yöntemi aramalısınız. Bu yöntem, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar.

Using deyiminin içine bu kodu ekleyin.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=66)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hataları) görüntüleme

Tanıma sonucu Konuşma hizmeti tarafından döndürüldüğünde, bununla ilgili bir şey yapmak istersiniz. Basit tutacağız ve sonucu durum paneline yazdıracağız.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=68-93)]

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı oluşturmaya ve test etmeye hazırsınız.

1. Menü çubuğundan, uygulamayı oluşturmak için **Yapı** > **Çözüm'üne** bakın' ı seçin. Kodun artık hatasız derlenmesi gerekir.

1. Uygulamayı başlatmak için **Hata Ayıklama** > **Başlatma Hata Ayıklama'yı** (veya **F5**tuşuna) seçin. **Helloworld** penceresi görüntülenir.

   ![C# örnek UWP konuşma tanıma uygulaması - quickstart](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-helloworld-window.png)

1. **Mikrofonu Etkinleştir'i**seçin ve erişim izni isteği geldiğinde **Evet'i**seçin.

   ![Mikrofon erişim izni isteği](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. **Mikrofon girdisi ile konuşma tanıma**’yı seçin ve cihazınızın mikrofonuna İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin pencerede görünür.

   ![Konuşma tanıma kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

