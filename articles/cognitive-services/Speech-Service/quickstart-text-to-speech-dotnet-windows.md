---
title: 'Hızlı Başlangıç: Konuşmayı sentezleştirme, .NET Framework (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Windows için .NET Framework ve konuşma SDK 'Sı kullanarak bir metin okuma konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metinden sentezleştirme ve konuşmacının gerçek zamanlı olarak dinlenmesini sağlayabilirsiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/27/2019
ms.author: yinhew
ms.openlocfilehash: 1a411455e4a6dea22e092cdfc8e70ee23b656435
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327443"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Hızlı Başlangıç: .NET Framework için konuşma SDK 'Sı ile konuşmayı sentezleştirme (Windows)

Hızlı başlangıç, [konuşma tanıma](quickstart-csharp-dotnet-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-dotnetframework-windows.md)için de kullanılabilir.

Windows için .NET Framework ve konuşma SDK 'Sı kullanarak bir metin okuma konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metinden sentezleştirme ve konuşmacının gerçek zamanlı olarak dinlenmesini sağlayabilirsiniz.

Hızlı bir gösterim için (Bu makalede açıklandığı gibi Visual Studio projesini kendiniz oluşturmadan), GitHub 'dan en son bilişsel [Hizmetler konuşma SDK 'sını](https://github.com/Azure-Samples/cognitive-services-speech-sdk) alın.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
* Konuşmacı (veya kulaklık) kullanılabilir.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. **Program.cs** açın ve otomatik olarak oluşturulan kodu şu örnekle değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. @No__t-0 dizesini bulun ve konuşma Hizmetleri abonelik anahtarınızla değiştirin.

1. @No__t-0 dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz deneme aboneliği kullanıyorsanız bölge `westus` ' dır.

1. Menü çubuğundan **dosya** > **Tümünü Kaydet**' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Uygulama derlemek için menü **çubuğundan @no__t-** 1**derleme çözümü** oluştur ' u seçin. Kodun artık hatasız derlenmesi gerekir.

1. **HelloWorld** uygulamasını başlatmak Için **Hata Ayıkla** > **hata ayıklamayı Başlat** (veya **F5**' i seçin) seçeneğini belirleyin.

1. Ingilizce bir tümcecik veya tümce girin. Uygulama, metninizi, konuşmacının oynamasını sağlamak üzere uygulamaya sentezleştirilmiş konuşma gönderen konuşma hizmetlerine iletir.

   ![Konuşma birleştirme Kullanıcı arabirimi](media/sdk/qs-tts-csharp-dotnet-windows-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Özel bir ses oluşturun](how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini Kaydet](record-custom-voice-samples.md)
