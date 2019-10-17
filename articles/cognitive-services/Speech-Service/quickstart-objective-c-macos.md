---
title: 'Hızlı başlangıç: konuşmayı tanıma, amaç-C-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: MacOS 'ta konuşma SDK 'sını kullanarak konuşmayı nasıl anlayacağınızı öğrenin
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 6955fc5dd98b65d2eb94914ea39685f1f69a46ac
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72327789"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-by-using-the-speech-sdk"></a>Hızlı başlangıç: konuşma SDK 'sını kullanarak macOS 'ta hedef-C içindeki konuşmayı tanıma

Hızlı başlangıçlara [konuşma birleştirme](quickstart-text-to-speech-objectivec-macos.md)için de erişilebilir.

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, bir mikrofondan metne kaydedilen konuşmayı oluşturmak için Azure bilişsel hizmetler konuşma SDK 'sını kullanarak amaç-C ' d e macOS uygulaması oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapmanız gerekir:

* Konuşma hizmeti için bir [abonelik anahtarı](get-started.md) .
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri ve macos 10,13 veya üzeri bir MacOS makinesi.

## <a name="get-the-speech-sdk-for-macos"></a>MacOS için konuşma SDK 'sını alın

[!INCLUDE [License notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Mac için bilişsel hizmetler konuşma SDK 'Sı, çerçeve paketi olarak dağıtılır. Xcode projelerinde bir [CocoaPod](https://cocoapods.org/) olarak veya https://aka.ms/csspeech/macosbinary ' den indirilen ve el ile bağlanmış bir şekilde kullanılabilir. Bu makalede bir CocoaPod kullanılmıştır.

## <a name="create-an-xcode-project"></a>Xcode projesi oluşturma

Xcode ' u başlatın ve **dosya** > **Yeni** > **Proje**' yi seçerek yeni bir proje başlatın. Şablon Seçimi iletişim kutusunda, **Cocoa uygulama** şablonunu seçin.

İzleyen iletişim kutularında aşağıdaki seçimleri yapın.

1. **Proje seçenekleri** iletişim kutusunda:
    1. Hızlı başlangıç uygulaması için *HelloWorld*gibi bir ad girin.
    1. Zaten bir Apple geliştirici hesabınız varsa, uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, *testorg*gibi bir ad kullanın. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız vardır. Daha fazla bilgi için [Apple geliştirici sitesine](https://developer.apple.com/)bakın.
    1. **Hedef-C** ' nin proje için dil olarak seçildiğinden emin olun.
    1. Görsel taslakları kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını temizleyin. Örnek uygulama için basit kullanıcı arabirimi programlı bir şekilde oluşturulur.
    1. Testler ve temel veriler için tüm onay kutularını temizleyin.

    ![Proje ayarları](media/sdk/qs-objectivec-macos-project-settings.png)

1. Bir proje dizini seçin:
    1. Projeyi içine koymak için bir dizin seçin. Bu adım, giriş dizininizde Xcode projesi için tüm dosyaları içeren bir HelloWorld dizini oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Ağ ve mikrofon erişimi için yetkilendirmeleri ayarlayın. Uygulama yapılandırmasına ulaşmak için sol taraftaki genel bakışın ilk satırında uygulama adını seçin. Ardından **yetenekler** sekmesini seçin.
    1. Uygulama için **uygulama korumalı alanı** ayarını etkinleştirin.
    1. **Giden bağlantılar** ve **mikrofon** erişimi için onay kutularını seçin.

    ![Korumalı alan ayarları](media/sdk/qs-objectivec-macos-sandbox.png)

1. Uygulamanın Ayrıca `Info.plist` dosyasında mikrofonun kullanımını bildirmesi gerekir. Genel bakışta dosyayı seçin ve *konuşma tanıma için, Microphone*gibi bir değere sahip **Gizlilik-mikrofon kullanım açıklaması** anahtarını ekleyin.

    ![Info. plist dosyasındaki ayarlar](media/sdk/qs-objectivec-macos-info-plist.png)

1. Xcode projesini kapatın. Daha sonra CocoaPods ayarladıktan sonra farklı bir örneğini kullanırsınız.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK 'Yı bir CocoaPod olarak yükler

1. [Yükleme yönergeleri](https://guides.cocoapods.org/using/getting-started.html)bölümünde açıklandığı gibi CocoaPod Dependency Manager 'ı yükleme.
1. Örnek uygulamanızın dizinine gidin, HelloWorld. *Pod dosyası* adında bir metin dosyası ve bu dizine aşağıdaki içeriği yerleştirin:

   [!code-ruby[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/Podfile)]
1. Terminalde HelloWorld dizinine gidin ve `pod install` komutunu çalıştırın. Bu komut, bağımlılık olarak hem örnek uygulamayı hem de konuşma SDK 'sını içeren `helloworld.xcworkspace` Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki adımlarda kullanılır.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Xcode 'da-0 @no__t çalışma alanını açın.
1. Otomatik olarak oluşturulan `AppDelegate.m` dosyasının içeriğini aşağıdaki kodla değiştirin:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/helloworld/AppDelegate.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. @No__t-0 dizesini aboneliğinizle ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz deneme aboneliği için `westus` kullanın.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. @No__t-1**hata ayıklama alanını** **görüntüle**' ye tıklayarak hata ayıklama çıktısını görünür hale getirin  > **konsolu etkinleştir**.
1. Menüden **ürün** > **Çalıştır** ' i seçerek örnek kodu derleyin ve çalıştırın. Ayrıca **oynat**' ı seçebilirsiniz.
1. Düğmeyi seçip birkaç sözcükten sonra, ekranın alt bölümünde söylenen metni görmeniz gerekir. Uygulamayı ilk kez çalıştırdığınızda, uygulamanın bilgisayarınızın mikrofonuna erişmesini sağlamanız istenir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da hedef-C örneklerini keşfet](https://aka.ms/csspeech/samples)
