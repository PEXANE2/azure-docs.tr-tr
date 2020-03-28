---
title: 'Quickstart: Synthesize konuşma, Swift - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'sını kullanarak macOS'ta Swift'te konuşmayı nasıl sentezleyin
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: ea0f61ddd2c60d2806af0f6dcf97c7d2388335d0
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975882"
---
# <a name="quickstart-synthesize-speech-in-swift-on-macos-using-the-speech-sdk"></a>Quickstart: Konuşma SDK kullanarak macOS Swift synthesize konuşma

Bu makalede, metinden konuşma sentezlemek ve varsayılan ses çıkışı ile oynatmak için Bilişsel Hizmetler Konuşma SDK kullanarak Swift'te bir macOS uygulaması oluşturmayı öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, ön koşulların bir listesi aşağıda veda edebilirsiniz:

* Konuşma hizmeti için [bir abonelik anahtarı.](~/articles/cognitive-services/Speech-Service/get-started.md)
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya daha sonra ve [CocoaPods](https://cocoapods.org/) yüklü bir macOS makinesi.

## <a name="get-the-speech-sdk-for-macos"></a>macOS için Konuşma SDK alın

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

macOS için Bilişsel Hizmetler Konuşma SDK bir çerçeve paketi olarak dağıtılır.
Bir [CocoaPod](https://cocoapods.org/)olarak Xcode projelerinde kullanılabilir , https://aka.ms/csspeech/macosbinary ya da indirilen ve manuel olarak bağlı. Bu kılavuzda Bir CocoaPod kullanır.

> [!NOTE] 
> Bu öğretici, 1.7.0'dan önce SDK sürümleriile çalışmaz.

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluşturma

Xcode'u başlatın ve**Yeni** >  **Proje dosyasına** > tıklayarak yeni bir proje başlatın.**Project**
Şablon seçimi iletişim kutusunda "Kakao Uygulaması" şablonu'nu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Zaten bir Apple geliştirici hesabınız varsa, uygun bir kuruluş adı ve bir kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız var. Ayrıntılar için [Apple geliştirici sitesine](https://developer.apple.com/) bakın.
    1. Swift'in projenin dili olarak seçildiğinden emin olun.
    1. Film şeridi kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı kaldırın. Örnek uygulama için basit kullanıcı arabirimi programlı olarak oluşturulur.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
1. Proje dizini seçin
    1. Projeyi koymak için bir dizin seçin. Bu, seçilen `helloworld` dizinde Xcode projesinin tüm dosyalarını içeren bir dizin oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Xcode projesini kapatın. Daha sonra CocoaPods kurduktan sonra bunun farklı bir örnek kullanacaktır.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Helloworld projesinin içindeki `MicrosoftCognitiveServicesSpeech-Bridging-Header.h` `helloworld` dizine adı içeren yeni bir üstbilgi dosyası yerleştirin ve aşağıdaki kodu içine yapıştırın:

   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h#code)]
1. `helloworld/MicrosoftCognitiveServicesSpeech-Bridging-Header.h` *Objective-C Köprü Üstbilgisi* üstbilgisi ![özelliklerindeki helloworld hedefi için Swift proje ayarlarına köprü leme üstbilgisine göreli yolu ekleyin](~/articles/cognitive-services/Speech-Service/media/sdk/qs-swift-macos-bridging-header.png)
1. Aşağıdaki şekilde otomatik oluşturulan `AppDelegate.swift` dosyasının içeriğini değiştirin:

   [!code-swift[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/helloworld/AppDelegate.swift#code)]
1. In `AppDelegate.swift`, dizeyi `YourSubscriptionKey` abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili bölge ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK'yı CocoaPod olarak yükleyin

1. CocoaPod bağımlılık yöneticisini [yükleme yönergelerinde](https://guides.cocoapods.org/using/getting-started.html)açıklandığı gibi yükleyin.
1. Örnek uygulamanızın dizinine gidin`helloworld`( ). Bu dizine ad `Podfile` ve aşağıdaki içeriği içeren bir metin dosyası yerleştirin:

   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/swift/macos/text-to-speech/helloworld/Podfile)]
1. Bir terminaldeki dizine `helloworld` gidin ve `pod install`komutu çalıştırın. Bu, hem `helloworld.xcworkspace` örnek uygulamayı hem de Konuşma SDK'sını bir bağımlılık olarak içeren bir Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki lerde kullanılacaktır.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Xcode'da `helloworld.xcworkspace` çalışma alanını açın.
1. Hata ayıklama çıktısını görünür hale getirin (**Hata Ayıklama Alanını** > **Etkinleştir konsolu****görüntüleyin** > ).
1. Menüden **Ürün** > **Çalıştır'ı** seçerek veya **Oynat** düğmesini tıklatarak örnek kodu oluşturun ve çalıştırın.
1. Bazı metinler girdikten ve uygulamadaki düğmeye tıkladıktan sonra, sentezlenen sesin çolduğunu duymalısınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneklerimizi GitHub'da keşfedin](https://aka.ms/csspeech/samples)

