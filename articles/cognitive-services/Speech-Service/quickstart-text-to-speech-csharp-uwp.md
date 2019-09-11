---
title: 'Hızlı Başlangıç: Sentezleştirme konuşma C# , (UWP)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu makalede, oluşturduğunuz bir C# Bilişsel hizmetler konuşma SDK'sı kullanarak evrensel Windows Platformu (UWP) uygulama. Konuşmayı gerçek zamanlı olarak cihazınızın hoparlörünü sentezleştir. Uygulama, konuşma SDK 'Sı NuGet paketiyle oluşturulmuştur ve 2019 Microsoft Visual Studio.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/19/2019
ms.author: yinhew
ms.openlocfilehash: 65b65c9af377b6a9951f9f328e0732850d3b9c1d
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382196"
---
# <a name="quickstart-synthesize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını kullanarak UWP uygulamasında konuşmayı sentezleştirme

Hızlı başlangıçlara [konuşma tanıma](quickstart-csharp-uwp.md), [konuşma çevirisi](quickstart-translate-speech-uwp.md)ve [ses-ilk Sanal Yardımcısı](quickstart-virtual-assistant-csharp-uwp.md)için de erişilebilir.

Bu makalede bilişsel Hizmetler C# [konuşma SDK 'sını](speech-sdk.md)kullanarak bir Evrensel Windows platformu (UWP) uygulaması geliştirirsiniz. Program, konuşmayı gerçek zamanlı olarak cihazınızın hoparlörünü sentezleştirir. Uygulamayı, [konuşma SDK 'Sı NuGet paketini](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2019 (herhangi bir sürümü) kullanarak oluşturursunuz.

> [!NOTE]
> Evrensel Windows Platformu; PC, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 destekleyen tüm cihazlarda çalışan uygulamalar geliştirmenize olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

Şimdi uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve arka plan C# kod uygulamasını ekleyin.

1. **Çözüm Gezgini**' de, `MainPage.xaml`öğesini açın.

1. Tasarımcının xaml görünümünde, **kılavuz** etiketine aşağıdaki xaml kod parçacığını ekleyin (ve `<Grid>` `</Grid>`arasında):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. **Çözüm Gezgini**' de, arka plan kod kaynak dosyasını `MainPage.xaml.cs`açın. (Altında `MainPage.xaml`gruplandırılır.)

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Kaynak dosyanın `Speak_ButtonClicked` işleyicisinde, dizeyi `YourSubscriptionKey`bulun ve abonelik anahtarınızla değiştirin.

1. İşleyicisinde, dizeyi `YourServiceRegion`bulun ve aboneliğinizle ilişkili bölge ile değiştirin. [](regions.md) `Speak_ButtonClicked` (Örneğin, ücretsiz deneme `westus` aboneliği için kullanın.)

1. Değişikliklerinizi kaydetmek için menü çubuğundan **Dosya** > **Tümünü Kaydet** ' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı derlemek ve test etmek için hazırsınız.

1. Menü çubuğundan uygulamayı derlemek için derleme**Build Solution** ' **ı seçin.**  >  Kodun artık hatasız derlenmesi gerekir.

1. Uygulamayı başlatmak için hata**ayıklamayı Başlat** ' **ı seçin (** veya F5 tuşuna basın). >  **HelloWorld** penceresi görüntülenir.

   ![Örnek UWP konuşma sen, Hızlı Başlangıç C# uygulaması](media/sdk/qs-text-to-speech-uwp-helloworld-window.png)

1. Metin kutusuna bir metin girin ve **konuş**' a tıklayın. Metniniz konuşma hizmetlerine iletilir ve konuşmacının oynadığı konuşmayı sentezleştirilmiştir.

    ![Konuşma birleştirme Kullanıcı arabirimi](media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Özel ses modelleri oluşturma ve kullanma](how-to-custom-voice-create-voice.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
