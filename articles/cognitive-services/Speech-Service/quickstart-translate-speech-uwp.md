---
title: 'Hızlı Başlangıç: Konuşmayı çevir, C# (UWP)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Kullanıcı konuşmayı yakalamak, başka bir dile çevirmek ve metni komut satırına çıkarmak için bir Evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız. Bu kılavuz Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 08/19/2019
ms.author: erhopf
ms.topic: quickstart
ms.openlocfilehash: e513cbbc615965ef196a830351aab8ac241c3f20
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382642"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Hızlı Başlangıç: Konuşmayı konuşma SDK 'Sı C# (UWP) ile çevirin

Hızlı başlangıçlara [konuşma tanıma](quickstart-csharp-uwp.md), [konuşma sensimi](quickstart-text-to-speech-csharp-uwp.md)ve [ses-ilk Sanal Yardımcısı](quickstart-virtual-assistant-csharp-uwp.md)için de erişilebilir.

Bu hızlı başlangıçta, bilgisayarınızın mikrofonunuzdan Kullanıcı konuşmayı yakalayan, konuşmayı çeviren ve çevrilmiş metni gerçek zamanlı olarak komut satırına bes bir Evrensel Windows Platformu (UWP) uygulaması oluşturacaksınız. Bu uygulama, 64 bit Windows üzerinde çalışmak üzere tasarlanmıştır ve [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2019 ile oluşturulmuştur.

Konuşma çevirisi için kullanılabilen dillerin tüm listesi için bkz. [dil desteği](language-support.md).

> [!NOTE]
> UWP, bilgisayarlar, Xbox, Surface Hub ve diğer cihazlar dahil olmak üzere Windows 10 ' u destekleyen herhangi bir cihazda çalışan uygulamalar geliştirmenize olanak sağlar.

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

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. **Çözüm Gezgini**' de, arka plan kod kaynak dosyasını `MainPage.xaml.cs`açın. (Altında `MainPage.xaml`gruplandırılır.)

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. `YourSubscriptionKey`Bu dosyadaki `SpeechTranslationFromMicrophone_ButtonClicked` İşleyicide, dizeyi bulun ve abonelik anahtarınızla değiştirin.

1. İşleyicisinde, dizeyi `YourServiceRegion`bulun ve aboneliğinizle ilişkili bölge ile değiştirin. [](regions.md) `SpeechTranslationFromMicrophone_ButtonClicked` (Örneğin, ücretsiz deneme `westus` aboneliği için kullanın.)

1. Değişikliklerinizi kaydetmek için menü çubuğundan **Dosya** > **Tümünü Kaydet** ' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı derlemek ve test etmek için hazırsınız.

1. Menü çubuğundan uygulamayı derlemek için derleme**Build Solution** ' **ı seçin.**  >  Kodun artık hatasız derlenmesi gerekir.

1. Uygulamayı başlatmak için hata**ayıklamayı Başlat** ' **ı seçin (** veya F5 tuşuna basın). >  **HelloWorld** penceresi görüntülenir.

   ![Hızlı başlangıçta örnek UWP çeviri C# uygulaması](media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. **Mikrofonu etkinleştir**' i seçin ve erişim izni Isteği açıldığında **Evet**' i seçin.

   ![Mikrofon erişimi izin isteği](media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. **Mikrofon girişinden konuşmayı çevir**' i seçin ve cihazınızın mikrofonuna İngilizce bir tümcecik veya cümle konuşun. Uygulama konuşmanızı konuşma hizmetine iletir ve bu da konuşmayı başka bir dildeki (Bu örnekte, Almanca) metne dönüştürür. Konuşma hizmeti, çevrilmiş metni, penceredeki çeviriyi görüntüleyen uygulamaya geri gönderir.

   ![Konuşma çevirisi Kullanıcı arabirimi](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Konuşma Tanıma için model eğitme](how-to-custom-speech-train-model.md)
