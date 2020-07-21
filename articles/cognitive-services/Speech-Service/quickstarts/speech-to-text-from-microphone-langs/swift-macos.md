---
title: 'Hızlı başlangıç: konuşmayı tanıma, Swift-konuşma hizmeti (macOS)'
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler konuşma SDK 'sını kullanarak macOS bilgisayarı için Swift 'ta konuşmayı tanımak üzere bir uygulama oluşturmayı öğrenin.
services: cognitive-services
author: cbasoglu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/25/2020
ms.author: cbasoglu
ms.openlocfilehash: 68947446d18ce1632b63ac3ba27311f648f3deea
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86524123"
---
# <a name="quickstart-recognize-speech-in-swift-on-macos-using-the-speech-sdk"></a>Hızlı başlangıç: konuşma SDK 'sını kullanarak macOS 'ta Swift 'ta konuşmayı tanıma

Hızlı başlangıçlara [konuşma birleştirme](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/swift-macos.md)için de erişilebilir.

Bu makalede, bir mikrofondan metne kaydedilen konuşmayı oluşturmak için bilişsel hizmetler konuşma SDK 'sını kullanarak Swift 'ta macOS uygulaması oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, önkoşulların bir listesi aşağıda verilmiştir:

* Konuşma hizmeti için bir [abonelik anahtarı](~/articles/cognitive-services/Speech-Service/get-started.md) .
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri ve [Cocoapods](https://cocoapods.org/) yüklü bir MacOS makinesi.

## <a name="get-the-speech-sdk-for-macos"></a>MacOS için konuşma SDK 'sını alın

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Bu öğreticinin SDK sürümü 1.6.0 ' den önceki sürümüyle çalışmadığını unutmayın.

MacOS için bilişsel hizmetler konuşma SDK 'Sı bir çerçeve paketi olarak dağıtılır.
Xcode projelerinde bir [CocoaPod](https://cocoapods.org/)olarak veya https://aka.ms/csspeech/macosbinary ile indirilen ve el ile bağlanmış bir şekilde kullanılabilir. Bu kılavuzda bir CocoaPod kullanılır.

## <a name="create-an-xcode-project"></a>Xcode projesi oluşturma

Xcode ' u başlatın ve **Dosya**  >  **Yeni**  >  **Proje**' ye tıklayarak yeni bir proje başlatın.
Şablon Seçimi iletişim kutusunda, "Cocoa uygulaması" şablonunu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Zaten bir Apple geliştirici hesabınız varsa, uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız vardır. Ayrıntılar için [Apple Developer sitesine](https://developer.apple.com/) bakın.
    1. Swift 'un proje için dil olarak seçildiğinden emin olun.
    1. Görsel taslakları kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı bırakın. Örnek uygulama için basit kullanıcı arabirimi programlı olarak oluşturulacaktır.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
1. Proje dizini seçin
    1. Projeyi içine koymak için bir dizin seçin. Bu, `helloworld` Seçili dizinde Xcode projesi için tüm dosyaları içeren bir dizin oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Ağ ve mikrofon erişimi için yetkilendirmeleri ayarlayın. Uygulama yapılandırmasına ulaşmak için sol taraftaki genel bakışın ilk satırındaki uygulama adına tıklayın ve ardından "yetenekler" sekmesini seçin.
    1. Uygulama için "uygulama korumalı alanı" ayarını etkinleştirin.
    1. "Giden bağlantılar" ve "mikrofon" erişimi için onay kutularını etkinleştirin.
    ![Korumalı alan ayarları](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-macos-sandbox.png)
1. Uygulamanın Ayrıca dosyada mikrofonun kullanımını bildirmesi gerekir `Info.plist` . Genel bakışta dosyaya tıklayın ve "Gizlilik-mikrofon kullanım açıklaması" anahtarını "konuşma tanıma için mikrofon gereklidir" gibi bir değere ekleyin.
    ![Info. plist dosyasındaki ayarlar](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-macos-info-plist.png)
1. Xcode projesini kapatın. Daha sonra CocoaPods ayarladıktan sonra farklı bir örneğini kullanacaksınız.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. HelloWorld projesinin içindeki dizine yeni bir üst bilgi dosyası yerleştirin `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` `helloworld` ve aşağıdaki kodu buna yapıştırın:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/from-microphone/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]
1. Hedef `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` *-C köprüleme üst* bilgi alanı ![ üst bilgisi özelliklerindeki HelloWorld hedefine ait Swift proje ayarlarına köprü üstbilgisine göreli yolu ekleyin](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-macos-bridging-header.png)
1. Aşağıdaki şekilde otomatik oluşturulan `AppDelegate.swift` dosyasının içeriğini değiştirin:

   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/from-microphone/helloworld/helloworld/AppDelegate.swift#code)]
1. İçinde `AppDelegate.swift` , dizesini `YourSubscriptionKey` abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili bölge ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK 'Yı bir CocoaPod olarak yükler

1. [Yükleme yönergeleri](https://guides.cocoapods.org/using/getting-started.html)bölümünde açıklandığı gibi CocoaPod Dependency Manager 'ı yükleme.
1. Örnek uygulamanızın dizinine gidin ( `helloworld` ). `Podfile`Bu dizine, adı ve aşağıdaki içeriğe sahip bir metin dosyası yerleştirin:

   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/from-microphone/helloworld/Podfile)]
1. `helloworld`Terminaldeki dizine gidin ve komutunu çalıştırın `pod install` . Bu işlem `helloworld.xcworkspace` , bağımlılık olarak hem örnek uygulamayı hem de konuşma SDK 'sını içeren bir Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki şekilde kullanılacaktır.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. `helloworld.xcworkspace`Çalışma alanını Xcode 'da açın.
1. Hata ayıklama çıkışını görünür hale getirme **(**  >  **hata ayıklama alanını**  >  **etkinleştirme konsolu**).
1. Menüden **ürün**  >  **Çalıştır** ' ı seçerek veya **oynat** düğmesine tıklayarak örnek kodu derleyin ve çalıştırın.
1. Uygulamada "tanıma" düğmesine tıkladıktan sonra birkaç sözcük söyledikten sonra, uygulama penceresinin alt bölümünde konuşulan metni görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklerimizi keşfet](https://aka.ms/csspeech/samples)

