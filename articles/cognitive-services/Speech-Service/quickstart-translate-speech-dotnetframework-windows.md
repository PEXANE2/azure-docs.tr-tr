---
title: 'Hızlı Başlangıç: Konuşmayı çevir, C# (.NET Framework Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Kullanıcı konuşmayı yakalamak, başka bir dile çevirmek ve metni komut satırına çıkarmak için basit bir .NET Framework uygulaması oluşturacaksınız. Bu kılavuz Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: e62756ab428135ca3d5d9107f37e61813097d21f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553463"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-framework"></a>Hızlı Başlangıç: .NET Framework için konuşma SDK 'Sı ile konuşmayı çevirin

Hızlı başlangıç, [konuşmadan metne](quickstart-csharp-dotnet-windows.md) ve [metinden konuşmaya](quickstart-text-to-speech-dotnet-windows.md)yönelik olarak da kullanılabilir.

Bu hızlı başlangıçta, bilgisayarınızın mikrofonunuzdan Kullanıcı konuşmayı yakalayan, konuşmayı çeviren ve çevrilmiş metni gerçek zamanlı olarak komut satırına seçtiğiniz basit bir .NET Framework uygulaması oluşturacaksınız. Bu uygulama, 64 bit Windows üzerinde çalışmak üzere tasarlanmıştır ve [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 ile oluşturulmuştur.

Konuşma çevirisi için kullanılabilen dillerin tüm listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. `Program.cs` dosyasını açın ve tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Konuşma, konuşma hizmetine iletilir, çevrilmiş ve aynı pencerede görüntülenen metne dönüştürülür.

    ![Başarılı çeviri sonrasında konsol çıktısının ekran görüntüsü](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png "Başarılı çeviri sonrasında konsol çıkışı")

## <a name="next-steps"></a>Sonraki adımlar

Ses dosyasından konuşmayı okuma ve metin ile çevrilmiş bir metnin sentezleştirilmiş konuşma olarak nasıl okunacağını gösteren ek örnekler, GitHub ' da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
