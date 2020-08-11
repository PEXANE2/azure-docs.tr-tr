---
title: 'Hızlı başlangıç: konuşmayı konuşmaya çevirme, C# (UWP)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 04/04/2020
ms.author: jhakulin
ms.topic: include
ms.openlocfilehash: 2af0d0eb2411c5ffe9f80498a3e93d48fe0a769b
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88057710"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=uwp&pivots=programming-language-csharp)

## <a name="add-sample-code"></a>Örnek kod ekleme

Şimdi uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve C# arka plan kod uygulamasını ekleyin.

1. **Çözüm Gezgini**' de, öğesini açın `MainPage.xaml` .

1. Tasarımcının XAML görünümünde, **kılavuz** ETIKETINE aşağıdaki xaml kod parçacığını ekleyin ( `<Grid>` ve arasında `</Grid>` ):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml#StackPanel)]

1. **Çözüm Gezgini**' de, arka plan kod kaynak dosyasını açın `MainPage.xaml.cs` . (Altında gruplandırılır `MainPage.xaml` .)

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml.cs#code)]

1. `SpeechTranslationFromMicrophone_ButtonClicked`Bu dosyadaki İşleyicide, dizeyi bulun `YourSubscriptionKey` ve abonelik anahtarınızla değiştirin.

1. İşleyicisinde, `SpeechTranslationFromMicrophone_ButtonClicked` dizeyi bulun `YourServiceRegion` ve aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin. (Örneğin, `westus` ücretsiz deneme aboneliği için kullanın.)

1. Değişikliklerinizi kaydetmek için menü çubuğundan **Dosya**  >  **Tümünü Kaydet** ' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı derlemek ve test etmek için hazırsınız.

1. Menü çubuğundan **Build**  >  uygulamayı derlemek için derleme**Build Solution** ' ı seçin. Kodun artık hatasız derlenmesi gerekir.

1. **Debug**  >  Uygulamayı başlatmak için hata**ayıklamayı Başlat** ' ı seçin (veya **F5**tuşuna basın). **HelloWorld** penceresi görüntülenir.

   ![C# ' de örnek UWP çeviri uygulaması-hızlı başlangıç](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. **Mikrofonu etkinleştir**' i seçin ve erişim izni Isteği açıldığında **Evet**' i seçin.

   ![Mikrofon erişimi izin isteği](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. **Mikrofon girişinden konuşmayı çevir**' i seçin ve cihazınızın mikrofonuna İngilizce bir tümcecik veya cümle konuşun. Uygulama konuşmanızı konuşma hizmetine iletir ve bu da konuşmayı başka bir dildeki (Bu örnekte, Almanca) metne dönüştürür. Konuşma hizmeti, çevrilmiş metni, penceredeki çeviriyi görüntüleyen uygulamaya geri gönderir.

   ![Konuşma çevirisi Kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
