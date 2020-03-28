---
title: 'Quickstart: Konuşma-konuşma, C# (UWP) - Konuşma hizmeti çevirme'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 12/09/2019
ms.author: erhopf
ms.topic: include
ms.openlocfilehash: a0f62db319d54c2db71a86f621985a304dbbb4d2
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78924752"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=uwp)

## <a name="add-sample-code"></a>Örnek kod ekleme

Şimdi uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve C# kodu arkasını uygulama ekleyin.

1. **Çözüm Gezgini'nde**, açık `MainPage.xaml`.

1. Tasarımcının XAML görünümünde, **Grid** etiketine aşağıdaki XAML snippet'i `</Grid>`ekleyin (arasında ve arasında): `<Grid>`

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml#StackPanel)]

1. **Çözüm Gezgini'nde,** kod arkasındaki `MainPage.xaml.cs`kaynak dosyayı açın. (Bu altında `MainPage.xaml`gruplandı .)

1. Aşağıdaki parçacıkla tüm kodu değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml.cs#code)]

1. Bu `SpeechTranslationFromMicrophone_ButtonClicked` dosyadaki işleyicide dizeyi `YourSubscriptionKey`bulun ve abonelik anahtarınızla değiştirin.

1. İşleyicide `SpeechTranslationFromMicrophone_ButtonClicked` dizeyi `YourServiceRegion`bulun ve aboneliğinizle ilişkili [bölgeyle](~/articles/cognitive-services/Speech-Service/regions.md) değiştirin. (Örneğin, ücretsiz `westus` deneme aboneliği için kullanın.)

1. Menü çubuğundan, değişikliklerinizi kaydetmek için**Tümlerini Kaydet'i** **seçin.** > 

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı oluşturmaya ve test etmeye hazırsınız.

1. Menü çubuğundan, uygulamayı oluşturmak için **Yapı** > **Çözümü'nü** seçin. Kodun artık hatasız derlenmesi gerekir.

1. Uygulamayı başlatmak için **Hata Ayıklama** > **Başlatma Hata Ayıklama'yı** (veya **F5**tuşuna) seçin. **Helloworld** penceresi görüntülenir.

   ![C# örnek UWP çeviri uygulaması - quickstart](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. **Mikrofonu Etkinleştir'i**seçin ve erişim izni isteği geldiğinde **Evet'i**seçin.

   ![Mikrofon erişim izni isteği](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. **Mikrofon girişinden konuşmayı Çevir'i**seçin ve cihazınızın mikrofonuna İngilizce bir tümcecik veya cümle konuşun. Uygulama konuşmanızı Konuşma hizmetine iletir ve konuşmanızı başka bir dilde metne çevirir (bu durumda Almanca). Konuşma hizmeti çevrilen metni pencerede çeviren uygulamaya geri gönderir.

   ![Konuşma çevirisi kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
