---
title: 'Hızlı başlangıç: Sentezleştirme konuşma, C# (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Windows için .NET Framework ve konuşma SDK 'Sı kullanarak bir metin okuma konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metinden sentezleştirme ve konuşmacının gerçek zamanlı olarak dinlenmesini sağlayabilirsiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: yinhew
ms.openlocfilehash: 89f491aae91787ec6b67b9aa2daee262f3bab62e
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81275128"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet&pivots=programming-language-csharp)

## <a name="add-sample-code"></a>Örnek kod ekleme

1. **Program.cs** açın ve otomatik olarak oluşturulan kodu şu örnekle değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/text-to-speech/helloworld/Program.cs#code)]

1. Dizeyi `YourSubscriptionKey`bulun ve konuşma hizmeti abonelik anahtarınızla değiştirin.

1. Dizeyi `YourServiceRegion`bulun ve aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin. Örneğin, ücretsiz deneme aboneliği kullanıyorsanız, bölgesi olur `westus`.

1. Menü çubuğundan **Dosya** > **Tümünü Kaydet**' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan uygulamayı derlemek için derleme**Build Solution** ' **ı seçin.** >  Kodun artık hatasız derlenmesi gerekir.

1.  >  **HelloWorld** uygulamasını başlatmak için hata**ayıklamayı Başlat** ' **ı seçin (** veya **F5**' i seçin).

1. Ingilizce bir tümcecik veya tümce girin. Uygulama, metninizi, konuşmacının oynamasını sağlamak üzere uygulamaya sentezleştirilmiş konuşma gönderen konuşma hizmetine iletir.

   ![Konuşma birleştirme Kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel bir ses oluşturun](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini Kaydet](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
