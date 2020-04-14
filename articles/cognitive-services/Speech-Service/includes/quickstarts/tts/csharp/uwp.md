---
title: 'Quickstart: Synthesize konuşma, C# (UWP) - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede Bilişsel HizmetlerKonuşma SDK'sı'nı kullanarak bir C# Evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız. Konuşmayı metinden gerçek zamanlı olarak cihazınızın hoparlörüne sentezlersiniz. Uygulama Speech SDK NuGet Paketi ve Microsoft Visual Studio 2019 ile oluşturulmuştır.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: yinhew
ms.openlocfilehash: 89fc5fddffb291942b8f3a4db3dfdf4ccd6cf46a
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275602"
---
> [!NOTE]
> Evrensel Windows Platformu; PC, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 destekleyen tüm cihazlarda çalışan uygulamalar geliştirmenize olanak tanır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)

## <a name="add-sample-code"></a>Örnek kod ekleme

Şimdi uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve C# kodu arkasını uygulama ekleyin.

1. **Çözüm Gezgini'nde**, açık `MainPage.xaml`.

1. Tasarımcının XAML görünümünde, **Grid** etiketine aşağıdaki XAML snippet'i `</Grid>`ekleyin (arasında ve arasında): `<Grid>`

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/text-to-speech/helloworld/MainPage.xaml#StackPanel)]

1. **Çözüm Gezgini'nde,** kod arkasındaki `MainPage.xaml.cs`kaynak dosyayı açın. (Bu altında `MainPage.xaml`gruplandı .)

1. Aşağıdaki parçacıkla tüm kodu değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/text-to-speech/helloworld/MainPage.xaml.cs#code)]

1. Kaynak dosyanın `Speak_ButtonClicked` işleyicisinde dizeyi `YourSubscriptionKey`bulun ve abonelik anahtarınızla değiştirin.

1. İşleyicide `Speak_ButtonClicked` dizeyi `YourServiceRegion`bulun ve aboneliğinizle ilişkili [bölgeyle](~/articles/cognitive-services/Speech-Service/regions.md) değiştirin. (Örneğin, ücretsiz `westus` deneme aboneliği için kullanın.)

1. Menü çubuğundan, değişikliklerinizi kaydetmek için**Tümlerini Kaydet'i** **seçin.** > 

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı oluşturmaya ve test etmeye hazırsınız.

1. Menü çubuğundan, uygulamayı oluşturmak için **Yapı** > **Çözüm'üne** bakın' ı seçin. Kodun artık hatasız derlenmesi gerekir.

1. Uygulamayı başlatmak için **Hata Ayıklama** > **Başlatma Hata Ayıklama'yı** (veya **F5**tuşuna) seçin. **Helloworld** penceresi görüntülenir.

   ![C# örnek UWP konuşma sentezi uygulaması - quickstart](~/articles/cognitive-services/Speech-Service/media/sdk/qs-text-to-speech-uwp-helloworld-window.png)

1. Textbox'a bazı metinler girin ve **Konuş'u**tıklatın. Metniniz Konuşma hizmetine aktarılır ve hoparlörünüzün üzerinde çalan konuşmayla sentezlenir.

    ![Konuşma sentezi kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Ses Oluşturma](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini kaydetme](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
