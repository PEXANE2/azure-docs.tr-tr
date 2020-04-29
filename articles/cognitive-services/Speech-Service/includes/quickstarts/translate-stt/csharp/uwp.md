---
title: 'Hızlı başlangıç: konuşmayı metne dönüştürme, C# (UWP)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.author: erhopf
ms.topic: include
ms.openlocfilehash: 62993b2e553630edd228228b4faa82de44997063
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80671775"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)

## <a name="add-sample-code"></a>Örnek kod ekleme

Şimdi uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve C# arka plan kod uygulamasını ekleyin.

1. **Çözüm Gezgini**' de, `MainPage.xaml`öğesini açın.

1. Tasarımcının XAML görünümünde, **kılavuz** ETIKETINE aşağıdaki xaml kod parçacığını ekleyin (ve `<Grid>` `</Grid>`arasında):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml#StackPanel)]

1. **Çözüm Gezgini**' de, arka plan kod kaynak dosyasını `MainPage.xaml.cs`açın. (Altında `MainPage.xaml`gruplandırılır.)

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml.cs#code)]

1. Bu dosyadaki `SpeechTranslationFromMicrophone_ButtonClicked` İşleyicide, dizeyi `YourSubscriptionKey`bulun ve abonelik anahtarınızla değiştirin.

1. `SpeechTranslationFromMicrophone_ButtonClicked` İşleyicisinde, dizeyi `YourServiceRegion`bulun ve aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin. (Örneğin, ücretsiz deneme `westus` aboneliği için kullanın.)

1. Değişikliklerinizi kaydetmek için menü çubuğundan **Dosya** > **Tümünü Kaydet** ' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı derlemek ve test etmek için hazırsınız.

1. Menü çubuğundan uygulamayı derlemek için derleme**Build Solution** ' **ı seçin.** >  Kodun artık hatasız derlenmesi gerekir.

1. Uygulamayı başlatmak için hata**ayıklamayı Başlat** ' **ı seçin (** veya F5 tuşuna basın). **F5** >  **HelloWorld** penceresi görüntülenir.

   ![C# ' de örnek UWP çeviri uygulaması-hızlı başlangıç](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. **Mikrofonu etkinleştir**' i seçin ve erişim izni Isteği açıldığında **Evet**' i seçin.

   ![Mikrofon erişimi izin isteği](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. **Mikrofon girişinden konuşmayı çevir**' i seçin ve cihazınızın mikrofonuna İngilizce bir tümcecik veya cümle konuşun. Uygulama konuşmanızı konuşma hizmetine iletir ve bu da konuşmayı başka bir dildeki (Bu örnekte, Almanca) metne dönüştürür. Konuşma hizmeti, çevrilmiş metni, penceredeki çeviriyi görüntüleyen uygulamaya geri gönderir.

   ![Konuşma çevirisi Kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
