---
title: 'Hızlı başlangıç: konuşmayı metne çevirme, C# (UWP)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 12/09/2019
ms.author: erhopf
ms.topic: include
ms.openlocfilehash: 70db4e23aebe2a986c9aa4c85c28ac6529ac3202
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "76156049"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurma](../../../../quickstarts/setup-platform.md?tabs=uwp)
> * [Boş bir örnek proje oluşturun](../../../../quickstarts/create-project.md?tabs=uwp)

## <a name="add-sample-code"></a>Örnek kodu ekleyin

Şimdi uygulamanın kullanıcı arabirimini tanımlayan XAML kodunu ekleyin ve arka plan C# kod uygulamasını ekleyin.

1. **Çözüm Gezgini**' de `MainPage.xaml`açın.

1. Tasarımcı XAML görünümünde, **kılavuz** ETIKETINE aşağıdaki xaml kod parçacığını ekleyin (`<Grid>` ve `</Grid>`arasında):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml#StackPanel)]

1. **Çözüm Gezgini**, arka plan kod kaynak dosyası `MainPage.xaml.cs`açın. (`MainPage.xaml`altında gruplandırılır.)

1. İçindeki tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/uwp/translate-speech-to-text/helloworld/MainPage.xaml.cs#code)]

1. Bu dosyadaki `SpeechTranslationFromMicrophone_ButtonClicked` işleyicisinde, `YourSubscriptionKey`dizesini bulun ve abonelik anahtarınızla değiştirin.

1. `SpeechTranslationFromMicrophone_ButtonClicked` işleyicisinde, `YourServiceRegion`dizesini bulun ve aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin. (Örneğin, ücretsiz deneme aboneliği için `westus` kullanın.)

1. Değişikliklerinizi kaydetmek için menü çubuğundan **dosya** > **Tümünü Kaydet** ' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Artık uygulamanızı derlemek ve test etmek için hazırsınız.

1. Menü çubuğundan, uygulamayı derlemek için **build** > **Build Solution** öğesini seçin. Kodun artık hatasız derlenmesi gerekir.

1. Uygulamayı başlatmak için hata **ayıklamayı başlatın** > **Hata Ayıkla** ' yı seçin (veya **F5**tuşuna basın). **HelloWorld** penceresi görüntülenir.

   ![Hızlı başlangıçta örnek UWP çeviri C# uygulaması](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. **Mikrofonu etkinleştir**' i seçin ve erişim izni Isteği açıldığında **Evet**' i seçin.

   ![Mikrofon erişimi izin isteği](~/articles/cognitive-services/Speech-Service/media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. **Mikrofon girişinden konuşmayı çevir**' i seçin ve cihazınızın mikrofonuna İngilizce bir tümcecik veya cümle konuşun. Uygulama konuşmanızı konuşma hizmetine iletir ve bu da konuşmayı başka bir dildeki (Bu örnekte, Almanca) metne dönüştürür. Konuşma hizmeti, çevrilmiş metni, penceredeki çeviriyi görüntüleyen uygulamaya geri gönderir.

   ![Konuşma çevirisi Kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
