---
title: 'Hızlı başlangıç: bir mikrofondan konuşmayı tanıma, Swift konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak iOS 'ta Swift 'ta konuşmayı tanımayı öğrenin
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/25/2020
ms.author: chlandsi
ms.openlocfilehash: 95c69aad24ff10b49bdc09411553e6ed43bfdccd
ms.sourcegitcommit: b56226271541e1393a4b85d23c07fd495a4f644d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85391443"
---
# <a name="quickstart-recognize-speech-in-swift-on-ios-by-using-the-speech-sdk"></a>Hızlı başlangıç: konuşma SDK 'sını kullanarak iOS 'ta Swift 'ta konuşmayı tanıma

Hızlı başlangıçlara [konuşma birleştirme](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/swift-ios.md)için de erişilebilir.

Bu makalede, bir mikrofondan metne kaydedilen konuşmayı oluşturmak için Azure bilişsel hizmetler konuşma SDK 'sını kullanarak Swift 'ta bir iOS uygulaması oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapmanız gerekir:

* Konuşma hizmeti için bir [abonelik anahtarı](~/articles/cognitive-services/Speech-Service/get-started.md) .
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri ve [Cocoapods](https://cocoapods.org/) yüklü bir MacOS makinesi.

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Bu öğretici, 1.6.0 ' den önceki SDK sürümü ile çalışmaz.

İOS için bilişsel hizmetler konuşma SDK 'Sı, bir çerçeve paketi olarak dağıtılır. Xcode projelerinde bir [CocoaPod](https://cocoapods.org/) olarak veya tarafından indirilen https://aka.ms/csspeech/iosbinary ve el ile bağlanmış bir şekilde kullanılabilir. Bu makalede bir CocoaPod kullanılmıştır.

## <a name="create-an-xcode-project"></a>Xcode projesi oluşturma

Xcode ' u başlatın ve **Dosya**  >  **Yeni**  >  **Proje**' yi seçerek yeni bir proje başlatın.
Şablon Seçimi iletişim kutusunda **IOS tek görünüm uygulaması** şablonunu seçin.

İzleyen iletişim kutularında aşağıdaki seçimleri yapın.

1. **Proje seçenekleri** iletişim kutusunda:
    1. Hızlı başlangıç uygulaması için *HelloWorld*gibi bir ad girin.
    1. Zaten bir Apple geliştirici hesabınız varsa, uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, *testorg*gibi bir ad kullanın. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız vardır. Daha fazla bilgi için [Apple geliştirici sitesine](https://developer.apple.com/)bakın.
    1. **Swift** 'un proje için dil olarak seçildiğinden emin olun.
    1. Görsel taslakları kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı bırakın. Örnek uygulama için basit kullanıcı arabirimi programlı bir şekilde oluşturulur.
    1. Testler ve temel veriler için tüm onay kutularını temizleyin.
1. Bir proje dizini seçin:
    1. Projeyi içine koymak için bir dizin seçin. Bu adım, seçili dizinde Xcode projesi için tüm dosyaları içeren bir HelloWorld dizini oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Uygulamanın Ayrıca dosyada mikrofonun kullanımını bildirmesi gerekir `Info.plist` . Genel bakışta dosyayı seçin ve *konuşma tanıma için, Microphone*gibi bir değere sahip **Gizlilik-mikrofon kullanım açıklaması** anahtarını ekleyin.

    ![Info. plist dosyasındaki ayarlar](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-ios-info-plist.png)

1. Xcode projesini kapatın. Daha sonra CocoaPods ayarladıktan sonra farklı bir örneğini kullanırsınız.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. HelloWorld projesi içindeki HelloWorld dizinine adlı yeni bir üst bilgi dosyası yerleştirin `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` . Aşağıdaki kodu içine yapıştırın:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]

1. Hedef `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` **-C köprü oluşturma üst bilgisi** alanındaki HelloWorld hedefine ait Swift proje ayarlarına köprü üst bilgisinin göreli yolunu ekleyin.

   ![Üst bilgi özellikleri](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-ios-bridging-header.png)

1. Otomatik olarak oluşturulan `AppDelegate.swift` dosyanın içeriğini aşağıdaki kodla değiştirin:

   [!code-swift[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/AppDelegate.swift#code)]
1. Otomatik olarak oluşturulan `ViewController.swift` dosyanın içeriğini aşağıdaki kodla değiştirin:

   [!code-swift[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/helloworld/ViewController.swift#code)]
1. İçinde `ViewController.swift` , dizesini `YourSubscriptionKey` abonelik anahtarınızla değiştirin.
1. Dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin. Örneğin, `westus` ücretsiz deneme aboneliği için kullanın.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK 'Yı bir CocoaPod olarak yükler

1. [Yükleme yönergeleri](https://guides.cocoapods.org/using/getting-started.html)bölümünde açıklandığı gibi CocoaPod Dependency Manager 'ı yükleme.
1. Örnek uygulamanızın dizinine gidin, HelloWorld. *Pod dosyası* adında bir metin dosyası ve bu dizine aşağıdaki içeriği yerleştirin:

   [!code-ruby[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/swift/ios/from-microphone/helloworld/Podfile)]
1. Terminalde HelloWorld dizinine gidin ve komutunu çalıştırın `pod install` . Bu komut `helloworld.xcworkspace` , bağımlılık olarak hem örnek uygulamayı hem de konuşma SDK 'sını içeren bir Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki adımlarda kullanılır.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Çalışma alanını `helloworld.xcworkspace` Xcode 'da açın.
1. Hata ayıklama alanını **görüntüle**  >  **Debug Area**  >  **konsolunu etkinleştir**' i seçerek hata ayıklama çıktısını görünür yapın.
1. **Ürün**  >  **hedefi** menüsündeki listeden uygulama hedefi olarak geliştirme makinenize bağlı iOS simülatörü veya iOS cihazını seçin.
1. Menüden **ürün**çalıştırması ' nı seçerek iOS benzeticisinde örnek kodu derleyin ve çalıştırın  >  **Run** . Ayrıca **oynat** düğmesini de seçebilirsiniz.
1. Uygulamadaki **tanıma** düğmesini seçip birkaç sözcükten sonra, ekranın alt bölümünde söylenen metni görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklerimizi keşfet](https://aka.ms/csspeech/samples)
