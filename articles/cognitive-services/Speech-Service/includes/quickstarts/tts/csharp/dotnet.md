---
title: 'Quickstart: Synthesize konuşma, C# (Windows) - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Windows için .NET çerçevesini ve Konuşma SDK'sını kullanarak metinden konuşmaya konsol uygulaması oluşturmak için bu kılavuzu kullanın. Bittiğinde, metni sentezleyebilir ve konuşmanızı konuşmacınızdaki konuşmayı gerçek zamanlı olarak duyabilirsiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/27/2019
ms.author: yinhew
ms.openlocfilehash: b615ba8085650e9aa686fb4a229d9752c4f6e2ce
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78925478"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları unutmayın:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=dotnet)

## <a name="add-sample-code"></a>Örnek kod ekleme

1. **Program.cs** açın ve otomatik olarak oluşturulan kodu bu örnekle değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp/dotnet/text-to-speech/helloworld/Program.cs#code)]

1. Dizeyi `YourSubscriptionKey`bulun ve Konuşma hizmeti abonelik anahtarınızla değiştirin.

1. Dizeyi `YourServiceRegion`bulun ve aboneliğinizle ilişkili [bölgeyle](~/articles/cognitive-services/Speech-Service/regions.md) değiştirin. Örneğin, ücretsiz deneme aboneliğikullanıyorsanız, bölge `westus`.

1. Menü çubuğundan **Dosya** > **Yı Tümle'yi kaydet'i**seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan, uygulamayı oluşturmak için **Yapı** > **Çözüm'üne** bakın' ı seçin. Kodun artık hatasız derlenmesi gerekir.

1. **Helloworld** uygulamasını başlatmak için **Hata** > **Ayıklama Başlatma Hata Ayıklama'yı** (veya **F5'i**seçin) seçeneğini belirleyin.

1. İngilizce bir tümcecik veya cümle girin. Uygulama, metninizi konuşma hizmetine iletir ve bu da sözcünüzde çalacak şekilde uygulamaya sentezlenmiş konuşma gönderir.

   ![Konuşma sentezi kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Ses Oluşturma](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini kaydetme](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
