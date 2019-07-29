---
title: 'Hızlı Başlangıç: Konuşmayı tanıma, C# (UWP)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, oluşturduğunuz bir C# Bilişsel hizmetler konuşma SDK'sı kullanarak evrensel Windows Platformu (UWP) uygulama. Cihazınızın mikrofonundan gerçek zamanda konuşmayı metne dönüştüreceksiniz. Uygulama, konuşma SDK'sı NuGet paketi ve Microsoft Visual Studio 2017 ile oluşturulmuştur.
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/23/2019
ms.author: lisaweixu
ms.custom: seodec18
ms.openlocfilehash: b4c3c0979f3fea491ad8b04285973517423eaeeb
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607795"
---
# <a name="quickstart-recognize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını kullanarak UWP uygulamasındaki konuşmayı tanıma

Hızlı başlangıç, [metinden konuşmaya](quickstart-text-to-speech-csharp-uwp.md), [konuşma çevirisi](quickstart-translate-speech-uwp.md) ve [ses-ilk Sanal Yardımcısı](quickstart-virtual-assistant-csharp-uwp.md)için de kullanılabilir.

İsterseniz, farklı bir programlama dili ve/veya ortamı seçin:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede bir C# Evrensel Windows platformu geliştirirsiniz (UWP; Bilişsel Hizmetler [konuşma SDK 'sını](speech-sdk.md)kullanarak Windows sürüm 1709 sonrası) uygulaması. Program, cihazınızın mikrofonunuzdan gerçek zamanlı olarak konuşmayı metne dönüştürme. Uygulama, [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 veya üzeri (herhangi bir sürüm) ile oluşturulmuştur.

> [!NOTE]
> Evrensel Windows Platformu; PC, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 destekleyen tüm cihazlarda çalışan uygulamalar geliştirmenize olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) veya üzeri
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. Uygulamanın kullanıcı arabirimini XAML kullanılarak tanımlanır. `MainPage.xaml` dosyasını Çözüm Gezgini'nde açın. Tasarımcının XAML görünümünde, aşağıdaki XAML kod parçacığını Kılavuz etiketine ekleyin (`<Grid>` ve `</Grid>` arasında).

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Arka plan kod kaynak dosyasını açın `MainPage.xaml.cs` (`MainPage.xaml` altında gruplandırılmış olarak bulabilirsiniz). İçindeki tüm kodu aşağıdakiyle değiştirin.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Bu dosyadaki `SpeechRecognitionFromMicrophone_ButtonClicked` işleyicisinde `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `SpeechRecognitionFromMicrophone_ButtonClicked` işleyicisinde `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-08-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir pencere açılır. **Mikrofonu Etkinleştir**’i seçin ve çıkan izin isteğini kabul edin.

    ![İzin isteğinin ekran görüntüsü](media/sdk/qs-csharp-uwp-10-access-prompt.png "Uygulamayı hata ayıklamada başlat")

1. **Mikrofon girdisi ile konuşma tanıma**’yı seçin ve cihazınızın mikrofonuna İngilizce bir deyim ya da cümle söyleyin. Konuşma, konuşma hizmetlerine iletilir ve pencerede görüntülenen metne gönderilir.

    ![Konuşma tanıma kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-csharp-uwp-11-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Konuşmayı çevirme](how-to-translate-speech-csharp.md)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
