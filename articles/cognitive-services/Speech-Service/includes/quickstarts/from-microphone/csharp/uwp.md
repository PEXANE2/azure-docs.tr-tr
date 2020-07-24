---
title: 'Hızlı başlangıç: bir mikrofondan konuşmayı tanıma, C# (UWP)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/02/2020
ms.author: erhopf
ms.openlocfilehash: 1068f715eb80eb5b7991a5b25c71c81d26d9f3c8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103074"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

Bunu zaten yaptıysanız harika. Şimdi devam edelim.

## <a name="open-your-project-in-visual-studio"></a>Projenizi Visual Studio 'da açın

İlk adım, projenizin Visual Studio 'da açık olduğundan emin olmak.

## <a name="start-with-some-boilerplate-code"></a>Bazı demirbaş kodla başlayın

Projemiz için bir çatı olarak çalışacak bir kod ekleyelim.

1. **Çözüm Gezgini**' de, öğesini açın `MainPage.xaml` .

2. Tasarımcının XAML görünümünde, **kılavuz** ETIKETINE aşağıdaki xaml kod parçacığını ekleyin ( `<Grid>` ve arasında `</Grid>` ):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml#StackPanel)]

3. **Çözüm Gezgini**' de, arka plan kod kaynak dosyasını açın `MainPage.xaml.cs` . (Altında gruplandırılır `MainPage.xaml` .)

4. Kodu aşağıdaki temel kodla değiştirin:

   [!code-csharp[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=6-50,55-56,94-154)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir nesneyi başlatabilmeniz `SpeechRecognizer` için önce abonelik anahtarınızı ve abonelik bölgenizi kullanan bir yapılandırma oluşturmanız gerekir. Bu kodu `SpeechRecognitionFromMicrophone_ButtonClicked()` yöntemine ekleyin.

> [!NOTE]
> Bu örnek `FromSubscription()` , oluşturmak için yöntemini kullanır `SpeechConfig` . Kullanılabilir yöntemlerin tam listesi için bkz. [SpeechConfig Class](https://docs.microsoft.com/dotnet/api/)[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=51-53)]

## <a name="initialize-a-speechrecognizer"></a>SpeechRecognizer başlatma

Şimdi bir oluşturalım `SpeechRecognizer` . Yönetilmeyen kaynakların doğru şekilde yayınlanmasıyla emin olmak için bir using ifadesinin içinde bu nesne oluşturulur. Bu kodu `SpeechRecognitionFromMicrophone_ButtonClicked()` , konuşma yapılandırmanızın hemen altına, yöntemine ekleyin.
[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=58,59,93)]

## <a name="recognize-a-phrase"></a>Bir tümceciği tanıma

`SpeechRecognizer`Nesnesinden `RecognizeOnceAsync()` yöntemini çağırın. Bu yöntem, konuşma hizmetinin tanıma için tek bir tümcecik gönderdiğini ve bu ifadenin, konuşmayı tanımayı durdurmak için bir kez tanımlandığını bilmesini sağlar.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=66)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hatalarını) görüntüleme

Tanınma sonucu konuşma hizmeti tarafından döndürüldüğünde, onunla ilgili bir şey yapmak isteyeceksiniz. Bu uygulamayı basit tutacağız ve sonucu durum paneline yazdıracağız.

[!code-csharp[](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/from-microphone/helloworld/MainPage.xaml.cs?range=68-93)]

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı derlemek ve test etmek için hazırsınız.

1. Menü çubuğundan **Build**  >  uygulamayı derlemek için derleme**Build Solution** ' ı seçin. Kodun artık hatasız derlenmesi gerekir.

1. **Debug**  >  Uygulamayı başlatmak için hata**ayıklamayı Başlat** ' ı seçin (veya **F5**tuşuna basın). **HelloWorld** penceresi görüntülenir.

   ![C# ' de örnek UWP konuşma tanıma uygulaması-hızlı başlangıç](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-helloworld-window.png)

1. **Mikrofonu etkinleştir**' i seçin ve erişim izni Isteği açıldığında **Evet**' i seçin.

   ![Mikrofon erişimi izin isteği](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. **Mikrofon girdisi ile konuşma tanıma**’yı seçin ve cihazınızın mikrofonuna İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin pencerede görünür.

   ![Konuşma tanıma kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

