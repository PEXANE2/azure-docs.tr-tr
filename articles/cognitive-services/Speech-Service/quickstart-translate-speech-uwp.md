---
title: 'Hızlı Başlangıç: Konuşmayı çevir, C# (UWP)-konuşma Hizmetleri'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Kullanıcı konuşmayı yakalamak, başka bir dile çevirmek ve metni komut satırına çıkarmak için basit bir Evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız. Bu kılavuz Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 07/23/2019
ms.author: erhopf
ms.openlocfilehash: dafcbd6bbe7f542d4eabc6b7cf858ac4871caae8
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68404895"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Hızlı Başlangıç: Konuşmayı konuşma SDK 'Sı C# (UWP) ile çevirin

Hızlı başlangıç, [konuşmadan metne](quickstart-csharp-uwp.md), [metinden konuşmaya](quickstart-text-to-speech-csharp-uwp.md) ve [ses-ilk Sanal Yardımcısı](quickstart-virtual-assistant-csharp-uwp.md)için de kullanılabilir.

Bu hızlı başlangıçta, bilgisayarınızın mikrofonunuzdan Kullanıcı konuşmayı yakalayan, konuşmayı çeviren ve çevrilmiş metni gerçek zamanlı olarak komut satırına seçtiğiniz basit bir Evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız. Bu uygulama, 64 bit Windows üzerinde çalışmak üzere tasarlanmıştır ve [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 ile oluşturulmuştur.

Konuşma çevirisi için kullanılabilen dillerin tüm listesi için bkz. [dil desteği](language-support.md).

> [!NOTE]
> UWP, bilgisayarlar, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 ' u destekleyen herhangi bir cihazda çalışan uygulamalar geliştirmenize olanak sağlar.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. Uygulamanın kullanıcı arabirimini XAML kullanılarak tanımlanır. `MainPage.xaml` dosyasını Çözüm Gezgini'nde açın. Tasarımcı xaml görünümünde, ve `<Grid>` `</Grid>`arasında aşağıdaki xaml kod parçacığını ekleyin.

    [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. Arka plan kod kaynak dosyasını açın `MainPage.xaml.cs` (`MainPage.xaml` altında gruplandırılmış olarak bulabilirsiniz). İçindeki tüm kodu aşağıdakiyle değiştirin.

    [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. Bu dosyadaki `SpeechTranslationFromMicrophone_ButtonClicked` işleyicisinde `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `SpeechTranslationFromMicrophone_ButtonClicked` işleyicisinde `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Değişiklikleri projeye kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun artık hatasız derlenmesi gerekir.

    ![Visual Studio uygulamasının, Çözümü Derle seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-08-build.png "Başarılı derleme")

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

    ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneği vurgulanmış olarak ekran görüntüsü](media/sdk/qs-csharp-uwp-09-start-debugging.png "Uygulamayı hata ayıklamada başlatma")

1. Bir pencere açılır. **Mikrofonu Etkinleştir**’i seçin ve çıkan izin isteğini kabul edin.

    ![İzin isteğinin ekran görüntüsü](media/sdk/qs-csharp-uwp-10-access-prompt.png "Uygulamayı hata ayıklamada başlat")

1. **Mikrofon girdisi ile konuşma tanıma**’yı seçin ve cihazınızın mikrofonuna İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin pencerede görünür.

    ![Konuşma tanıma kullanıcı arabiriminin ekran görüntüsü](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
