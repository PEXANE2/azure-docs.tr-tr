---
title: 'Hızlı Başlangıç: Konuşmayı ve Swift-konuşma hizmetini tanıma'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak iOS 'ta Swift 'ta konuşmayı tanımayı öğrenin
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: chlandsi
ms.openlocfilehash: e9d17b0bdeb89fc03c0f089b84a89a5203c39566
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68717419"
---
# <a name="quickstart-recognize-speech-in-swift-on-ios-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını kullanarak iOS 'ta Swift 'ta konuşmayı tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, bir mikrofondan metne kaydedilen konuşmayı oluşturmak için bilişsel hizmetler konuşma SDK 'sını kullanarak Swift 'ta bir iOS uygulaması oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce önkoşullarının listesi aşağıda verilmiştir:

* Konuşma hizmeti için bir [abonelik anahtarı](get-started.md) .
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri ve [Cocoapods](https://cocoapods.org/) yüklü bir MacOS makinesi.

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.6.0`. Bu öğreticinin, SDK 'nın daha önceki bir sürümünde değişiklik yapılmadan çalışmadığına unutmayın.

İOS için bilişsel hizmetler konuşma SDK 'Sı, bir çerçeve paketi olarak dağıtılır.
Xcode projelerinde bir [CocoaPod](https://cocoapods.org/)olarak veya ile indirilen https://aka.ms/csspeech/macosbinary ve el ile bağlanmış bir şekilde kullanılabilir. Bu kılavuzda bir CocoaPod kullanılır.

## <a name="create-an-xcode-project"></a>Xcode projesi oluşturma

Xcode’u başlatın ve **Dosya** > **Yeni** > **Proje** seçeneklerine tıklayarak yeni bir proje başlatın.
Şablon seçimi iletişim kutusunda, “iOS Tek Görünüm Uygulaması” şablonunu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Zaten bir Apple geliştirici hesabınız varsa, uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız vardır. Ayrıntılar için [Apple Developer sitesine](https://developer.apple.com/) bakın.
    1. Swift 'un proje için dil olarak seçildiğinden emin olun.
    1. Görsel taslakları kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı bırakın. Örnek uygulama için basit kullanıcı arabirimi programlı olarak oluşturulacaktır.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
1. Proje dizini seçin
    1. Projeyi içine koymak için bir dizin seçin. Bu, seçili `helloworld` dizinde Xcode projesi için tüm dosyaları içeren bir dizin oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Uygulamanın Ayrıca `Info.plist` dosyada mikrofonun kullanımını bildirmesi gerekir. Genel bakışta dosyaya tıklayın ve "Gizlilik-mikrofon kullanım açıklaması" anahtarını "konuşma tanıma için mikrofon gereklidir" gibi bir değere ekleyin.
    ![Info. plist dosyasındaki ayarlar](media/sdk/qs-swift-ios-info-plist.png)
1. Xcode projesini kapatın. Daha sonra CocoaPods ayarladıktan sonra farklı bir örneğini kullanacaksınız.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. HelloWorld projesinin içindeki `helloworld` dizine yeni bir üst bilgi `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` dosyası yerleştirin ve aşağıdaki kodu buna yapıştırın:  
   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]
1. Hedef `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` *-C köprüleme üst* bilgi alanı ![üst bilgisi özelliklerindeki HelloWorld hedefine ait Swift proje ayarlarına köprü üstbilgisine göreli yolu ekleyin](media/sdk/qs-swift-ios-bridging-header.png)
1. Aşağıdaki şekilde otomatik oluşturulan `AppDelegate.swift` dosyasının içeriğini değiştirin:  
   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/AppDelegate.swift#code)]
1. Aşağıdaki şekilde otomatik oluşturulan `ViewController.swift` dosyasının içeriğini değiştirin:  
   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/helloworld/ViewController.swift#code)]
1. İçinde `ViewController.swift`, dizesini `YourSubscriptionKey` abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK 'Yı bir CocoaPod olarak yükler

1. [Yükleme yönergeleri](https://guides.cocoapods.org/using/getting-started.html)bölümünde açıklandığı gibi CocoaPod Dependency Manager 'ı yükleme.
1. Örnek uygulamanızın dizinine gidin (`helloworld`). Bu dizine, adı `Podfile` ve aşağıdaki içeriğe sahip bir metin dosyası yerleştirin:  
   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift-ios/helloworld/Podfile)]
1. Terminaldeki `pod install`dizine gidin ve komutunu çalıştırın. `helloworld` Bu işlem, bağımlılık `helloworld.xcworkspace` olarak hem örnek uygulamayı hem de konuşma SDK 'sını içeren bir Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki şekilde kullanılacaktır.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. `helloworld.xcworkspace` Çalışma alanını Xcode 'da açın.
1. Hata ayıklama çıkışını görünür hale getirin (**Görünüm** > **Hata Ayıklama Alanı** > **Konsolu Etkinleştir**).
1. **Ürün** > **hedefi** menüsündeki listeden uygulama hedefi olarak geliştirme makinenize bağlı iOS simülatörü veya iOS cihazını seçin.
1. Menüden **Ürün** > **Çalıştır** seçeneklerini belirleyerek veya **Oynat** düğmesine tıklayarak iOS simülatöründe örnek kodu derleyin ve çalıştırın.
1. Uygulamada "tanıma" düğmesine tıkladıktan sonra birkaç sözcük söyledikten sonra, ekranın alt bölümünde söylenen metni görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklerimizi keşfet](https://aka.ms/csspeech/samples)
