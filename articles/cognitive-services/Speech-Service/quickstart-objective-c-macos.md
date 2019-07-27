---
title: 'Hızlı Başlangıç: Konuşmayı tanıma, amaç-C-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak macOS 'ta hedef C 'de konuşmayı tanımayı öğrenin
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: 45cd1210ee6af3c456171a427729f6e16caf2d58
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559356"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Hızlı Başlangıç: MacOS 'ta konuşma SDK 'sını kullanarak bir konuşmayı tanıt

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, bir mikrofondan metne kaydedilen konuşmayı oluşturmak için bilişsel hizmetler konuşma SDK 'sını kullanarak amaç-C ' d e macOS uygulaması oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce önkoşullarının listesi aşağıda verilmiştir:

* A [abonelik anahtarı](get-started.md) konuşma hizmeti
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri ve macos 10,13 veya üzeri bir MacOS makinesi

## <a name="get-the-speech-sdk-for-macos"></a>MacOS için konuşma SDK 'sını alın

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.6.0`.

Mac için bilişsel hizmetler konuşma SDK 'Sı, çerçeve paketi olarak dağıtılır.
Xcode projelerinde bir [CocoaPod](https://cocoapods.org/)olarak veya ile indirilen https://aka.ms/csspeech/macosbinary ve el ile bağlanmış bir şekilde kullanılabilir. Bu kılavuzda bir CocoaPod kullanılır.

## <a name="create-an-xcode-project"></a>Xcode projesi oluşturma

Xcode’u başlatın ve **Dosya** > **Yeni** > **Proje** seçeneklerine tıklayarak yeni bir proje başlatın.
Şablon Seçimi iletişim kutusunda, "Cocoa uygulaması" şablonunu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Zaten bir Apple geliştirici hesabınız varsa, uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız vardır. Ayrıntılar için [Apple Developer sitesine](https://developer.apple.com/) bakın.
    1. Proje dili olarak Objective-C seçildiğinden emin olun.
    1. Görsel taslakları kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı bırakın. Örnek uygulama için basit kullanıcı arabirimi programlı olarak oluşturulacaktır.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
    ![Proje Ayarları](media/sdk/qs-objectivec-macos-project-settings.png)
1. Proje dizini seçin
    1. Projeyi içine koymak için bir dizin seçin. Bu, giriş `helloworld` dizininizde Xcode projesi için tüm dosyaları içeren bir dizin oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Ağ ve mikrofon erişimi için yetkilendirmeleri ayarlayın. Uygulama yapılandırmasına ulaşmak için sol taraftaki genel bakışın ilk satırındaki uygulama adına tıklayın ve ardından "yetenekler" sekmesini seçin.
    1. Uygulama için "uygulama korumalı alanı" ayarını etkinleştirin.
    1. "Giden bağlantılar" ve "mikrofon" erişimi için onay kutularını etkinleştirin.
    ![Korumalı alan ayarları](media/sdk/qs-objectivec-macos-sandbox.png)
1. Uygulamanın Ayrıca `Info.plist` dosyada mikrofonun kullanımını bildirmesi gerekir. Genel bakışta dosyaya tıklayın ve "Gizlilik-mikrofon kullanım açıklaması" anahtarını "konuşma tanıma için mikrofon gereklidir" gibi bir değere ekleyin.
    ![Info. plist dosyasındaki ayarlar](media/sdk/qs-objectivec-macos-info-plist.png)
1. Xcode projesini kapatın. Daha sonra CocoaPods ayarladıktan sonra farklı bir örneğini kullanacaksınız.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK 'Yı bir CocoaPod olarak yükler

1. [Yükleme yönergeleri](https://guides.cocoapods.org/using/getting-started.html)bölümünde açıklandığı gibi CocoaPod Dependency Manager 'ı yükleme.
1. Örnek uygulamanızın dizinine gidin (`helloworld`). Bu dizine, adı `Podfile` ve aşağıdaki içeriğe sahip bir metin dosyası yerleştirin:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/Podfile)]
1. Terminaldeki `pod install`dizine gidin ve komutunu çalıştırın. `helloworld` Bu işlem, bağımlılık `helloworld.xcworkspace` olarak hem örnek uygulamayı hem de konuşma SDK 'sını içeren bir Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki şekilde kullanılacaktır.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. `helloworld.xcworkspace` Çalışma alanını Xcode 'da açın.
1. Aşağıdaki şekilde otomatik oluşturulan `AppDelegate.m` dosyasının içeriğini değiştirin:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec-macos/helloworld/helloworld/AppDelegate.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Hata ayıklama çıkışını görünür hale getirin (**Görünüm** > **Hata Ayıklama Alanı** > **Konsolu Etkinleştir**).
1. Menüden **ürün** > **Çalıştır** ' ı seçerek veya **oynat** düğmesine tıklayarak örnek kodu derleyin ve çalıştırın.
1. Düğmeye tıkladıktan ve birkaç sözcükten sonra, ekranın alt bölümünde söylenen metni görmeniz gerekir. Uygulamayı ilk kez çalıştırdığınızda, uygulamanın bilgisayarınızın mikrofonuna erişmesini sağlamanız istenir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da hedef-C örneklerini keşfet](https://aka.ms/csspeech/samples)
