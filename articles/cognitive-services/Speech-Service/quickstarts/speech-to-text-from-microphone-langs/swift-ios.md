---
title: 'Quickstart: Bir mikrofon, Swift konuşma tanıma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: SDK Konuşmasını kullanarak iOS'ta Swift'teki konuşmayı nasıl tanıyın
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/23/2019
ms.author: chlandsi
ms.openlocfilehash: 31f5339c70d52019400ca5f1fe873de4790a3bd6
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75380533"
---
# <a name="quickstart-recognize-speech-in-swift-on-ios-by-using-the-speech-sdk"></a>Quickstart: Konuşma SDK kullanarak iOS swift konuşma tanıyın

Quickstarts konuşma [sentezi](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/swift-ios.md)için de kullanılabilir.

Bu makalede, mikrofondan metne kaydedilen konuşmayı transkripsiyonu yapmak için Azure Bilişsel Hizmetler Konuşma SDK'sını kullanarak Swift'te nasıl bir iOS uygulaması oluşturabileceğinizi öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapmanız gerekir:

* Konuşma hizmeti için [bir abonelik anahtarı.](~/articles/cognitive-services/Speech-Service/get-started.md)
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya daha sonra ve [CocoaPods](https://cocoapods.org/) yüklü bir macOS makinesi.

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Bu öğretici, 1.6.0'dan önce SDK'nın bir sürümüyle çalışmaz.

iOS için Bilişsel Hizmetler Konuşma SDK bir çerçeve paketi olarak dağıtılır. Xcode projelerinde [CocoaPod](https://cocoapods.org/) olarak kullanılabilir veya el https://aka.ms/csspeech/iosbinary ile indirilebilir ve bağlanabilir. Bu makalede bir CocoaPod kullanır.

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluşturma

Xcode'u başlatın ve **Dosya** > **Yeni** > **Projesi'ni**seçerek yeni bir proje başlatın.
Şablon seçimi iletişim kutusunda **iOS Tek Görünüm Uygulaması** şablonu'nu seçin.

Bunu izleyen iletişim kutularında aşağıdaki seçimleri yapın.

1. Proje **Seçenekleri** iletişim kutusunda:
    1. Quickstart uygulaması için bir ad girin, örneğin, *helloworld*.
    1. Zaten bir Apple geliştirici hesabınız varsa uygun bir kuruluş adı ve bir kuruluş tanımlayıcısı girin. Test amacıyla, *testorg*gibi bir ad kullanın. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız var. Daha fazla bilgi için [Apple geliştirici sitesine](https://developer.apple.com/)bakın.
    1. **Swift'in** projenin dili olarak seçildiğinden emin olun.
    1. Film şeridi kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı kaldırın. Örnek uygulama için basit kullanıcı arabirimi programlı olarak oluşturulur.
    1. Testler ve temel veriler için tüm onay kutularını temizleyin.
1. Bir proje dizini seçin:
    1. Projeyi koymak için bir dizin seçin. Bu adım, Xcode projesiiçin tüm dosyaları içeren seçilen dizinde bir helloworld dizini oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Uygulamanın ayrıca dosyadaki mikrofonun kullanımını `Info.plist` bildirmesi gerekir. Genel bakışta dosyayı seçin ve *konuşma tanıma için Mikrofon*gibi bir değere sahip Gizlilik - Mikrofon Kullanımı **Açıklaması** anahtarını ekleyin.

    ![Info.plist'teki Ayarlar](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-ios-info-plist.png)

1. Xcode projesini kapatın. CocoaPods'u kurduktan sonra farklı bir örneğini kullanırsınız.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Helloworld projesinin içinde helloworld dizinine adını `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` içeren yeni bir başlık dosyası yerleştirin. Aşağıdaki kodu yapıştırın:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]

1. **Objective-C** `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` Köprü Başlığı alanındaki helloworld hedefi için Swift proje ayarlarına köprü başlığına göreli yolu ekleyin.

   ![Üstbilgi özellikleri](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-ios-bridging-header.png)

1. Otomatik oluşturulan `AppDelegate.swift` dosyanın içeriğini aşağıdaki kodla değiştirin:

   [!code-swift[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/AppDelegate.swift#code)]
1. Otomatik oluşturulan `ViewController.swift` dosyanın içeriğini aşağıdaki kodla değiştirin:

   [!code-swift[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/ViewController.swift#code)]
1. In `ViewController.swift`, dizeyi `YourSubscriptionKey` abonelik anahtarınızla değiştirin.
1. Dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölgeyle](~/articles/cognitive-services/Speech-Service/regions.md) değiştirin. Örneğin, ücretsiz `westus` deneme aboneliği için kullanın.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK'yı CocoaPod olarak yükleyin

1. CocoaPod bağımlılık yöneticisini [yükleme yönergelerinde](https://guides.cocoapods.org/using/getting-started.html)açıklandığı gibi yükleyin.
1. Helloworld olan örnek uygulamanızın dizinine gidin. Bu dizine *Podfile* adını ve aşağıdaki içeriği içeren bir metin dosyası yerleştirin:

   [!code-ruby[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/Podfile)]
1. Bir terminaldeki helloworld dizinine gidin ve `pod install`komutu çalıştırın. Bu komut, `helloworld.xcworkspace` hem örnek uygulamayı hem de Konuşma SDK'sını bir bağımlılık olarak içeren bir Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki adımlarda kullanılır.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Xcode'da `helloworld.xcworkspace` çalışma alanını açın.
1. Hata Ayıklama **View** > **Alanını** > **Etkinleştir konsolu**seçerek hata ayıklama çıktısını görünür hale getirin.
1. **Ürün** > **Hedefi** menüsündeki listeden uygulamanın hedefi olarak iOS simülatörü veya geliştirme makinenize bağlı bir iOS aygıtını seçin.
1. Menüden **Ürün** > **Çalıştır'ı** seçerek iOS simülatöründe örnek kodu oluşturun ve çalıştırın. **Ayrıca Oynat** düğmesini de seçebilirsiniz.
1. Uygulamadaki **Tanı** düğmesini seçtikten ve birkaç kelime söyledikten sonra, ekranalt kısmında konuştuğunuz metni görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneklerimizi GitHub'da keşfedin](https://aka.ms/csspeech/samples)
