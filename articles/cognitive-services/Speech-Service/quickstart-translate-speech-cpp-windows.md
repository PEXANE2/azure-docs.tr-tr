---
title: 'Hızlı Başlangıç: Konuşmayı çevir, C++ (Windows)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, Kullanıcı konuşmayı yakalamak, C++ başka bir dile çevirmek ve metni komut satırına çıkarmak için bir uygulama oluşturacaksınız. Bu kılavuz Windows kullanıcıları için tasarlanmıştır.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/24/2019
ms.author: erhopf
ms.openlocfilehash: 6ba4e44efc7ff24aa48f9f16840b2248423f7241
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382669"
---
# <a name="quickstart-translate-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını C++ kullanarak Windows 'da konuşmayı dönüştürme

Hızlı başlangıçlara [konuşma tanıma](quickstart-cpp-windows.md) ve [konuşma senşü](quickstart-text-to-speech-cpp-windows.md)için de erişilebilir.

Bu hızlı başlangıçta, bilgisayarınızın mikrofonunuzdan Kullanıcı C++ konuşmayı yakalayan, konuşmayı çeviren ve çevrilmiş metni gerçek zamanlı olarak komut satırına seçtiğiniz bir uygulama oluşturacaksınız. Bu uygulama, [konuşma SDK 'Sı NuGet paketi](https://aka.ms/csspeech/nuget) ve Microsoft Visual Studio 2019 (herhangi bir sürüm) ile oluşturulmuştur.

Konuşma çevirisi için kullanılabilen dillerin tüm listesi için bkz. [dil desteği](language-support.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir konuşma Hizmetleri abonelik anahtarına ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [konuşma hizmetlerini ücretsiz deneyin](get-started.md) .

## <a name="create-a-visual-studio-project"></a>Visual Studio projesi oluşturma

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. **helloworld.cpp** kaynak dosyasını açın.

1. Tüm kodu aşağıdaki kod parçacığıyla değiştirin:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/cpp-windows/helloworld/helloworld.cpp?range=2-#code)]

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

1. Menü çubuğundan **Dosya** > **Tümünü Kaydet**' i seçin.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

1. Menü çubuğundan uygulamayı derlemek için derleme**Build Solution** ' **ı seçin.**  >  Kodun artık hatasız derlenmesi gerekir.

1. HelloWorld uygulamasını başlatmak için hata ayıklamayı Başlat ' ı seçin (veya F5 tuşuna basın). > 

1. İngilizce bir deyim ya da cümle söyleyin. Uygulama, konuşmanızı, metne (Bu örnekte, Fransızca ve Almanca 'ya) çeviren ve yeniden veren konuşma hizmetlerine iletir. Konuşma hizmetleri daha sonra metni görüntülenmek üzere uygulamaya geri gönderir.

   ![Başarılı konuşma çevirisi sonrasında konsol çıkışı](media/sdk/qs-translate-cpp-windows-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Ses dosyasından konuşmayı okuma veya çevrilmiş metni sentezleştirilmiş konuşmaya dönüştürme gibi ek örnekler, GitHub 'da bulunabilir.

> [!div class="nextstepaction"]
> [GitHub C++ 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Özel Konuşma Tanıma için model eğitme](how-to-custom-speech-train-model.md)
