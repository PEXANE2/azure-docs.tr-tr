---
title: 'Quickstart: Bir mikrofon, Objective-C konuşma tanıma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: SDK Konuşma'yı kullanarak macOS'taki Objective-C'deki konuşmayı nasıl tanıyın
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/23/2019
ms.author: chlandsi
ms.openlocfilehash: c2f0fbe66b26c6eca6e0c0b2530efacba9bae958
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75380601"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-macos-by-using-the-speech-sdk"></a>Hızlı başlatma: SDK Konuşmasını kullanarak macOS'ta Objective-C'deki konuşmayı tanıma

Quickstarts konuşma [sentezi](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/objectivec-macos.md)için de kullanılabilir.

Bu makalede, mikrofondan metne kaydedilen konuşmayı transkripsiyonu yapmak için Azure Bilişsel Hizmetler Konuşma SDK'sını kullanarak Objective-C'de macOS uygulaması oluşturmayı öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapmanız gerekir:

* Konuşma hizmeti için [bir abonelik anahtarı.](~/articles/cognitive-services/Speech-Service/get-started.md)
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya sonraki ve macOS 10.13 veya daha yeni bir macOS makinesi.

## <a name="get-the-speech-sdk-for-macos"></a>macOS için Konuşma SDK alın

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Mac için Bilişsel Hizmetler Konuşma SDK bir çerçeve paketi olarak dağıtılır. Xcode projelerinde [CocoaPod](https://cocoapods.org/) olarak kullanılabilir veya el https://aka.ms/csspeech/macosbinary ile indirilebilir ve bağlanabilir. Bu makalede bir CocoaPod kullanır.

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluşturma

Xcode'u başlatın ve **Dosya** > **Yeni** > **Projesi'ni**seçerek yeni bir proje başlatın. Şablon seçimi iletişim kutusunda Kakao **Uygulaması** şablonu'nu seçin.

Bunu izleyen iletişim kutularında aşağıdaki seçimleri yapın.

1. Proje **Seçenekleri** iletişim kutusunda:
    1. Quickstart uygulaması için bir ad girin, örneğin, *helloworld*.
    1. Zaten bir Apple geliştirici hesabınız varsa uygun bir kuruluş adı ve bir kuruluş tanımlayıcısı girin. Test amacıyla, *testorg*gibi bir ad kullanın. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız var. Daha fazla bilgi için [Apple geliştirici sitesine](https://developer.apple.com/)bakın.
    1. Project'in dili olarak **Objective-C'nin** seçildiğinden emin olun.
    1. Film şeridi kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını temizleyin. Örnek uygulama için basit kullanıcı arabirimi programlı olarak oluşturulur.
    1. Testler ve temel veriler için tüm onay kutularını temizleyin.

    ![Proje ayarları](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-project-settings.png)

1. Bir proje dizini seçin:
    1. Projeyi koymak için bir dizin seçin. Bu adım, xcode projesi için tüm dosyaları içeren ev dizini bir helloworld dizini oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Ağ ve mikrofon erişimi için yetkileri ayarlayın. Uygulama yapılandırmasına ulaşmak için soldaki genel bakışta ilk satırdaki uygulama adını seçin. Ardından **Yetenekler** sekmesini seçin.
    1. Uygulama için **Uygulama ve Kasa** ayarını etkinleştirin.
    1. **Giden Bağlantılar** ve **Mikrofon** erişimi için onay kutularını seçin.

    ![Sandbox ayarları](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-sandbox.png)

1. Uygulamanın ayrıca dosyadaki mikrofonun kullanımını `Info.plist` bildirmesi gerekir. Genel bakışta dosyayı seçin ve *konuşma tanıma için Mikrofon*gibi bir değere sahip Gizlilik - Mikrofon Kullanımı **Açıklaması** anahtarını ekleyin.

    ![Info.plist'teki Ayarlar](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-info-plist.png)

1. Xcode projesini kapatın. CocoaPods'u kurduktan sonra farklı bir örneğini kullanırsınız.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK'yı CocoaPod olarak yükleyin

1. CocoaPod bağımlılık yöneticisini [yükleme yönergelerinde](https://guides.cocoapods.org/using/getting-started.html)açıklandığı gibi yükleyin.
1. Helloworld olan örnek uygulamanızın dizinine gidin. Bu dizine *Podfile* adını ve aşağıdaki içeriği içeren bir metin dosyası yerleştirin:

   [!code-ruby[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/from-microphone/helloworld/Podfile)]
1. Bir terminaldeki helloworld dizinine gidin ve `pod install`komutu çalıştırın. Bu komut, `helloworld.xcworkspace` hem örnek uygulamayı hem de Konuşma SDK'sını bir bağımlılık olarak içeren bir Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki adımlarda kullanılır.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Xcode'da `helloworld.xcworkspace` çalışma alanını açın.
1. Otomatik oluşturulan `AppDelegate.m` dosyanın içeriğini aşağıdaki kodla değiştirin:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/from-microphone/helloworld/helloworld/AppDelegate.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. Dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölgeyle](~/articles/cognitive-services/Speech-Service/regions.md) değiştirin. Örneğin, ücretsiz `westus` deneme aboneliği için kullanın.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Hata Ayıklama **View** > **Alanını** > **Etkinleştir konsolu**seçerek hata ayıklama çıktısını görünür hale getirin.
1. Menüden **Ürün** > **Çalıştır'ı** seçerek örnek kodu oluşturun ve çalıştırın. **Ayrıca Oynat'ı**da seçebilirsiniz.
1. Düğmeyi seçip birkaç kelime söyledikten sonra, ekranın alt kısmında konuştuğunuz metni görmeniz gerekir. Uygulamayı ilk kez çalıştırdığınızda, uygulamanın bilgisayarınızın mikrofonuna erişmesi istenir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub'da Objective-C örneklerini keşfedin](https://aka.ms/csspeech/samples)
