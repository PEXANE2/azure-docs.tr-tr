---
title: 'Hızlı Başlangıç: Sentezleştirme konuşma C++ , (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Windows Masaüstü 'nde C++ konuşmayı nasıl sentezleyeceğinizi öğrenin
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/24/2019
ms.author: yinhew
ms.openlocfilehash: 686b21d3e02266af77687778c32f0d1ca6d55154
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383091"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını kullanarak C++ Windows 'da konuşmayı sentezleştirme

Hızlı başlangıç, [konuşma tanıma](quickstart-cpp-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-cpp-windows.md)için de kullanılabilir.

Bu makalede Windows için bir C++ konsol uygulaması oluşturacaksınız. Bilişsel Hizmetler [konuşma SDK 'sını](speech-sdk.md) kullanarak konuşmayı gerçek zamanlı olarak sentezleştirme ve konuşmayı bilgisayarınızın hoparlörünüz üzerinde oynatacak şekilde kullanıyorsunuz. Uygulama, [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2019 (herhangi bir sürüm) ile oluşturulmuştur.

Konuşma birleştirme için kullanılabilen dillerin/seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir konuşma Hizmetleri abonelik anahtarına ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [konuşma hizmetlerini ücretsiz deneyin](get-started.md) .

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. **helloworld.cpp** kaynak dosyasını açın.

1. Tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Menü çubuğundan **Dosya** > **Tümünü Kaydet**' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan uygulamayı derlemek için derleme**Build Solution** ' **ı seçin.**  >  Kodun artık hatasız derlenmesi gerekir.

1. HelloWorld uygulamasını başlatmak için hata ayıklamayı Başlat ' ı seçin (veya F5 tuşuna basın). > 

1. Ingilizce bir tümcecik veya cümle yazın. Uygulama, metninizi, konuşmacının oynamasını sağlamak üzere uygulamaya sentezleştirilmiş konuşma gönderen konuşma hizmetlerine iletir.

   ![Başarılı konuşma Birleştirmeden sonra konsol çıkışı](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Konuşmayı bir ses dosyasına kaydetme gibi ek örnekler, GitHub 'da bulunabilir.

> [!div class="nextstepaction"]
> [GitHub C++ 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Özel bir ses oluşturun](how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini Kaydet](record-custom-voice-samples.md)
