---
title: 'Hızlı Başlangıç: Konuşmayı sentezleştirme, .NET Framework (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Windows için .NET Framework ve konuşma SDK 'Sı kullanarak bir metin okuma konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metinden sentezleştirme ve konuşmacının gerçek zamanlı olarak dinlenmesini sağlayabilirsiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: 3c925e0160a4c57f7e43c3463fc1db7098a6e606
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553608"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Hızlı Başlangıç: .NET Framework için konuşma SDK 'Sı ile konuşmayı sentezleştirme (Windows)

Hızlı başlangıç, [konuşma tanıma](quickstart-csharp-dotnet-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-dotnetframework-windows.md)için de kullanılabilir.

Windows için .NET Framework ve konuşma SDK 'Sı kullanarak bir metin okuma konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metinden sentezleştirme ve konuşmacının gerçek zamanlı olarak dinlenmesini sağlayabilirsiniz.

Hızlı bir gösterim için (aşağıda gösterildiği gibi, Visual Studio projesini kendiniz oluşturmadan):

GitHub 'dan en yeni bilişsel [Hizmetler konuşma SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-speech-sdk) alın.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
* Konuşmacı (veya kulaklık) kullanılabilir.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `Program.cs` dosyasını açın otomatik olarak oluşturulan kodu bu örnekle değiştirin:

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Dizeyi `YourSubscriptionKey` bulun ve konuşma Hizmetleri abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz denemeyi kullanıyorsanız bölge `westus` olur.

1. Projedeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-08-build.png "Başarılı derleme")

1. Uygulamayı başlatmak için menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnet-windows-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir konsol penceresi görünür ve sizden bir metin yazmanız istenir. Birkaç kelime veya tümce yazın. Yazdığınız metin konuşma hizmetlerine iletilir ve konuşmacının oynadığı konuşmayı sentezleştirilmiştir.

    ![Başarılı tanımadan sonra konsol çıktısının ekran görüntüsü](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Başarılı tanımadan sonra konsol çıktısı")

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
