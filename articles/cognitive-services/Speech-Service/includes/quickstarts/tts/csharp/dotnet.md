---
title: 'Hızlı başlangıç: Sentezleştirme C# konuşma, (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Windows için .NET Framework ve konuşma SDK 'Sı kullanarak bir metin okuma konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metinden sentezleştirme ve konuşmacının gerçek zamanlı olarak dinlenmesini sağlayabilirsiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/27/2019
ms.author: yinhew
ms.openlocfilehash: 38dac73d253245fce86ac419c7a4eb3a106df3f0
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818327"
---
## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurma](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Boş bir örnek proje oluşturun](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="add-sample-code"></a>Örnek kod ekleme

1. **Program.cs** açın ve otomatik olarak oluşturulan kodu şu örnekle değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/text-to-speech/helloworld/Program.cs#code)]

1. `YourSubscriptionKey`dize bulun ve konuşma hizmeti abonelik anahtarınızla değiştirin.

1. `YourServiceRegion`dize bulun ve aboneliğiniz ile ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin. Örneğin, ücretsiz deneme aboneliği kullanıyorsanız bölge `westus`.

1. Menü çubuğundan **dosya** > **Tümünü Kaydet**' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Uygulama derlemek için menü **çubuğundan** > **derleme çözümü** oluştur ' u seçin. Kodun artık hatasız derlenmesi gerekir.

1. **HelloWorld** uygulamasını başlatmak Için hata **ayıklamayı Başlat** > **Hata Ayıkla** ' yı seçin (veya **F5**' i seçin).

1. Ingilizce bir tümcecik veya tümce girin. Uygulama, metninizi, konuşmacının oynamasını sağlamak üzere uygulamaya sentezleştirilmiş konuşma gönderen konuşma hizmetine iletir.

   ![Konuşma birleştirme Kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel bir ses oluşturun](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini Kaydet](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
