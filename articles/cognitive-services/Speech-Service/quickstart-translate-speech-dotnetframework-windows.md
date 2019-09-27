---
title: 'Hızlı Başlangıç: Konuşmayı çevir, C# (.NET Framework Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Kullanıcı konuşmayı yakalamak, başka bir dile çevirmek ve metni komut satırına çıkarmak için bir .NET Framework uygulaması oluşturacaksınız. Bu kılavuz Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: erhopf
ms.openlocfilehash: bb710a3e3adb13aa3999c13043c8bb93f6b885f1
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327355"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-framework-windows"></a>Hızlı Başlangıç: .NET Framework için konuşma SDK 'Sı ile konuşmayı çevirme (Windows)

Hızlı başlangıçlara [konuşma tanıma](quickstart-csharp-dotnet-windows.md) ve [konuşma senşü](quickstart-text-to-speech-dotnet-windows.md)için de erişilebilir.

Bu hızlı başlangıçta, bilgisayarınızın mikrofonunuzdan Kullanıcı konuşmayı yakalayan, konuşmayı çeviren ve çevrilmiş metni gerçek zamanlı olarak komut satırına bes bir .NET Framework uygulaması oluşturacaksınız. Bu uygulama 32 bit veya 64 bit Windows üzerinde çalışabilir ve [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2019 ile oluşturulmuştur.

Konuşma çevirisi için kullanılabilen dillerin tüm listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. **Program.cs**açın ve içindeki tüm kodu aşağıdaki kodla değiştirin.

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-dotnet-windows/helloworld/Program.cs#code)]

1. @No__t-0 dizesini bulun ve abonelik anahtarınızla değiştirin.

1. @No__t-0 dizesini bulun ve aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz deneme aboneliği kullanıyorsanız bölge `westus` ' dır.

1. Menü çubuğundan **dosya** > **Tümünü Kaydet**' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Uygulama derlemek için menü **çubuğundan @no__t-** 1**derleme çözümü** oluştur ' u seçin. Kodun artık hatasız derlenmesi gerekir.

1. **HelloWorld** uygulamasını başlatmak Için **Hata Ayıkla** > **hata ayıklamayı Başlat** (veya **F5**' i seçin) seçeneğini belirleyin.

1. Cihazınızın mikrofonuna Ingilizce bir tümcecik veya tümce konuşun. Uygulama konuşmanızı konuşma hizmetine iletir ve bu da konuşmayı başka bir dildeki (Bu örnekte, Almanca) metne dönüştürür. Konuşma hizmeti, çevrilmiş metni, penceredeki çeviriyi görüntüleyen uygulamaya geri gönderir.

   ![Konuşma çevirisi Kullanıcı arabirimi](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Ses dosyasından konuşmayı okuma ve metin ile çevrilmiş bir metnin sentezleştirilmiş konuşma olarak nasıl okunacağını gösteren ek örnekler, GitHub ' da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub C# 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Konuşma Tanıma için model eğitme](how-to-custom-speech-train-model.md)
