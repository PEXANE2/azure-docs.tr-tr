---
title: 'Hızlı Başlangıç: Konuşmayı tanıma, .NET Framework (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Windows için .NET Framework ve Konuşma Tanıma SDK'sını kullanarak bir konuşmayı metne dönüştürme konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: wolfma
ms.openlocfilehash: cb140647394858fbc0a9a00ea125365d5b7a08d5
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327037"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-net-framework-windows"></a>Hızlı Başlangıç: .NET Framework için konuşma SDK 'Sı ile konuşmayı tanıma (Windows)

Hızlı başlangıç, [konuşma senşisi](quickstart-text-to-speech-dotnet-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-dotnetframework-windows.md)için de kullanılabilir.

İsterseniz, farklı bir programlama dili ve/veya ortamı seçin:<br/>
[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Windows için .NET Framework ve Konuşma Tanıma SDK'sını kullanarak bir konuşmayı metne dönüştürme konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.

Hızlı bir gösterim için (Bu makalede açıklandığı gibi Visual Studio projesini kendiniz oluşturmadan), GitHub 'dan en son bilişsel [Hizmetler konuşma SDK 'sını](https://github.com/Azure-Samples/cognitive-services-speech-sdk) alın.

## <a name="prerequisites"></a>Önkoşullar

Bu projeyi tamamlamak için şunlar gerekir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Konuşma hizmeti için bir abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).
* Bilgisayarınızın mikrofonuna erişin.

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. **Program.cs** açın ve otomatik olarak oluşturulan kodu şu örnekle değiştirin:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. @No__t-0 dizesini bulun ve konuşma Hizmetleri abonelik anahtarınızla değiştirin.

1. @No__t-0 dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz deneme aboneliği kullanıyorsanız bölge `westus` ' dır.

1. Menü çubuğundan **dosya** > **Tümünü Kaydet**' i seçin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulama derlemek için menü **çubuğundan @no__t-** 1**derleme çözümü** oluştur ' u seçin. Kodun artık hatasız derlenmesi gerekir.

1. **HelloWorld** uygulamasını başlatmak Için **Hata Ayıkla** > **hata ayıklamayı Başlat** (veya **F5**' i seçin) seçeneğini belirleyin.

1. Cihazınızın mikrofonuna Ingilizce bir tümcecik veya tümce konuşun. Uygulama, konuşmanızı görüntü için uygulamaya geri gönderen konuşma hizmetlerine iletir.

   ![Konuşma tanıma kullanıcı arabirimi](media/sdk/qs-csharp-dotnet-windows-10-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Konuşma Tanıma için model eğitme](how-to-custom-speech-train-model.md)
