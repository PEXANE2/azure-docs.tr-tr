---
title: 'Hızlı Başlangıç: Konuşmayı çevir, C++ (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Kullanıcı konuşmayı yakalamak, başka C++ bir dile çevirmek ve metni komut satırına çıkarmak için basit bir uygulama oluşturacaksınız. Bu kılavuz Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 379087ca94eee6ce3d45bfd97b4771c5f08d6333
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607708"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c"></a>Hızlı Başlangıç: Konuşma SDK 'Sı ile konuşmayı çevirC++

Hızlı başlangıç, [konuşma tanıma](quickstart-cpp-windows.md) ve [metinden konuşmaya](quickstart-text-to-speech-cpp-windows.md)için de kullanılabilir.

Bu hızlı başlangıçta, bilgisayarınızın mikrofonunuzdan Kullanıcı konuşmayı C++ yakalayan, konuşmayı çeviren ve çevrilmiş metni gerçek zamanlı olarak komut satırına bes bir basit uygulama oluşturacaksınız. Bu uygulama, 64 bit Windows üzerinde çalışmak üzere tasarlanmıştır ve [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2017 veya üzeri sürümleriyle oluşturulmuştur.

Konuşma çevirisi için kullanılabilen dillerin tüm listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) veya üzeri
* Konuşma hizmeti için bir Azure abonelik anahtarı. [Ücretsiz bir tane alın](get-started.md).

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. *helloworld.cpp* kaynak dosyasını açın. İlk dahil etme deyiminin (`#include "stdafx.h"` veya `#include "pch.h"`) altındaki tüm kodu aşağıdakiyle değiştirin:

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/cpp-windows/helloworld/helloworld.cpp#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Proje üzerindeki değişiklikleri kaydedin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Uygulamayı derleyin. Menü çubuğundan **Derle** > **Çözümü Derle**'yi seçin. Kodun hatasız olarak derlenmesi gerekir.

   ![Visual Studio uygulamasının, Çözümü Derle seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-06-build.png)

1. Uygulamayı başlatın. Menü çubuğundan **Hata Ayıklama** > **Hata Ayıklamayı Başlat**'ı seçin veya **F5** tuşuna basın.

   ![Visual Studio uygulamasının, Hata Ayıklamayı Başlat seçeneğinin vurgulandığı ekran görüntüsü](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Bir şey söylemenizi isteyen bir konsol penceresi görünür. İngilizce bir deyim ya da cümle söyleyin. Konuşmanızı, aynı pencerede görüntülenen konuşma hizmetine iletilir ve metne dönüştürülür.

   ![Başarılı çeviri sonrasında konsol çıktısının ekran görüntüsü](media/sdk/qs-translate-cpp-windows-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Ses dosyasından konuşmayı okuma ve metin ile çevrilmiş bir metnin sentezleştirilmiş konuşma olarak nasıl okunacağını gösteren ek örnekler, GitHub ' da kullanılabilir.

> [!div class="nextstepaction"]
> [GitHub C++ 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
