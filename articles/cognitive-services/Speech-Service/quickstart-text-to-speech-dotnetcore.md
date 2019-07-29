---
title: 'Hızlı Başlangıç: Sentezleştirme konuşma C# , (.NET Core)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Windows üzerinde .NET C# Core altında konuşmayı nasıl sentezleyeceğinizi öğrenin
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/24/2019
ms.author: yinhew
ms.openlocfilehash: ccb6ec5b6f29c57a6136c571558d4126d0607710
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607685"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-core"></a>Hızlı Başlangıç: .NET Core için konuşma SDK 'Sı ile konuşmayı sentezleştirme

Hızlı başlangıç, [konuşma tanıma](quickstart-csharp-dotnetcore-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-dotnetcore-windows.md)için de kullanılabilir.

Bu makalede Bilişsel Hizmetler[Konuşma SDK'sı](speech-sdk.md)'nı kullanarak Windows'da .NET Core için bir C# konsol uygulaması oluşturacaksınız. Konuşmayı, BILGISAYARıNıZıN hoparlörünü gerçek zamanlı olarak gerçek zamanlı olarak sentezlendir. Uygulama, [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 veya üzeri (herhangi bir sürüm) ile oluşturulmuştur.

> [!NOTE]
> .NET Core, [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) teknik özelliklerini gerçekleştiren açık kaynaklı, platformlar arası bir .NET platformudur.

Bu hızlı başlangıcı tamamlayabilmeniz için bir konuşma Hizmetleri abonelik anahtarına ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [konuşma hizmetlerini ücretsiz deneyin](get-started.md) .

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [.NET Core SDK](https://dotnet.microsoft.com/download)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) veya üzeri
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-dotnetcore-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. `Program.cs` dosyasını açın ve tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnetcore/helloworld/Program.cs#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir konsol penceresi görünür ve bir metin yazmanızı ister. Birkaç kelime veya tümce yazın. Yazdığınız metin konuşma hizmetlerine iletilir ve konuşmacının oynadığı konuşmayı sentezleştirilmiştir.

    ![Başarılı birleştirme sonrasında konsol çıktısının ekran görüntüsü](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png "Başarılı birleştirme sonrasında konsol çıkışı")

## <a name="next-steps"></a>Sonraki adımlar

Konuşmayı bir ses dosyasına birleştirme gibi ek örnekler, GitHub 'da bulunabilir.

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
