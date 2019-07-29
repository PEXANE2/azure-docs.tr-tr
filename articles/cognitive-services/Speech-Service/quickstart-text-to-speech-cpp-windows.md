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
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: fdc5e540dd7e84d47aa139768fbfc2614f38b376
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607721"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını kullanarak C++ Windows 'da konuşmayı sentezleştirme

Hızlı başlangıç, [konuşma tanıma](quickstart-cpp-windows.md) ve [konuşma çevirisi](quickstart-translate-speech-cpp-windows.md)için de kullanılabilir.

Bu makalede Windows için bir C++ konsol uygulaması oluşturacaksınız. Bilişsel Hizmetler [konuşma SDK 'sını](speech-sdk.md) kullanarak konuşmayı gerçek zamanlı olarak sentezleştirme ve konuşmayı bilgisayarınızın hoparlörünüz üzerinde oynatacak şekilde kullanıyorsunuz. Uygulama, [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 veya üzeri (herhangi bir sürüm) ile oluşturulmuştur.

Bu makalede açıklanan özellik, [konuşma SDK 1.5.0](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/1.5.0)' den edinilebilir.

Konuşma birleştirme için kullanılabilen dillerin/seslerin tüm listesi için bkz. [dil desteği](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir konuşma Hizmetleri abonelik anahtarına ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [konuşma hizmetlerini ücretsiz deneyin](get-started.md) .

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. *helloworld.cpp* kaynak dosyasını açın. İlk dahil etme deyiminin (`#include "stdafx.h"` veya `#include "pch.h"`) altındaki tüm kodu aşağıdakiyle değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız olarak derlenmesi gerekir.

   ![Visual Studio uygulamasının, Çözümü Derle seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-06-build.png)

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

   ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Bir konsol penceresi görünür ve bir metin yazmanızı ister. Birkaç kelime veya tümce yazın. Yazdığınız metin konuşma hizmetlerine iletilir ve konuşmacının oynadığı konuşmayı sentezleştirilmiştir.

   ![Başarılı birleştirme sonrasında konsol çıktısının ekran görüntüsü](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Konuşmayı bir ses dosyasına kaydetme gibi ek örnekler, GitHub 'da bulunabilir.

> [!div class="nextstepaction"]
> [GitHub C++ 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
