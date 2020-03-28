---
title: 'Quickstart: Synthesize konuşma, Objective-C - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Speech SDK'yı kullanarak macOS'ta Objective-C'de konuşmanın nasıl sentezleneceğini öğrenin
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: yulili
ms.openlocfilehash: 37eed03ed839411f1acf5d963d4118a3c6d2c379
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975933"
---
# <a name="quickstart-synthesize-speech-in-objective-c-on-macos-using-the-speech-sdk"></a>Quickstart: Konuşma SDK kullanarak macOS'ta Objective-C'de konuşma sentezleme

Bu makalede, metinden konuşma sentezlemek ve varsayılan ses çıkışı ile oynatmak için Bilişsel Hizmetler Konuşma SDK kullanarak Objective-C'de bir macOS uygulaması oluşturmayı öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, ön koşulların bir listesi aşağıda veda edebilirsiniz:

* Konuşma hizmeti için [bir abonelik anahtarı](~/articles/cognitive-services/Speech-Service/get-started.md)
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya sonraki ve macOS 10.13 veya sonraki macOS makinesi

## <a name="get-the-speech-sdk-for-macos"></a>macOS için Konuşma SDK alın

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

Bu öğreticinin SDK'nın 1.7.0'dan önceki sürümüyle çalışmayacağını unutmayın.

Mac için Bilişsel Hizmetler Konuşma SDK bir çerçeve paketi olarak dağıtılır.
Bir [CocoaPod](https://cocoapods.org/)olarak Xcode projelerinde kullanılabilir , https://aka.ms/csspeech/macosbinary ya da indirilen ve manuel olarak bağlı. Bu kılavuzda Bir CocoaPod kullanır.

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluşturma

Xcode'u başlatın ve**Yeni** >  **Proje dosyasına** > tıklayarak yeni bir proje başlatın.**Project**
Şablon seçimi iletişim kutusunda "Kakao Uygulaması" şablonu'nu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Zaten bir Apple geliştirici hesabınız varsa, uygun bir kuruluş adı ve bir kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız var. Ayrıntılar için [Apple geliştirici sitesine](https://developer.apple.com/) bakın.
    1. Proje dili olarak Objective-C seçildiğinden emin olun.
    1. Film şeridi kullanmak ve belge tabanlı bir uygulama oluşturmak için onay kutularını devre dışı kaldırın. Örnek uygulama için basit kullanıcı arabirimi programlı olarak oluşturulur.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
    ![Proje Ayarları](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-project-settings.png)
1. Proje dizini seçin
    1. Projeyi koymak için bir dizin seçin. Bu, ev `helloworld` dizininizde Xcode projesinin tüm dosyalarını içeren bir dizin oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
1. Ağ erişimi için yetkileri ayarlayın. Uygulama yapılandırmasına ulaşmak için soldaki genel bakışta ilk satırdaki uygulama adını tıklatın ve ardından "Yetenekler" sekmesini seçin.
    1. Uygulama için "Uygulama ve Kasa" ayarını etkinleştirin.
    1. "Giden Bağlantılar" erişimi için onay kutularını etkinleştirin.
    ![Sandbox Ayarları](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-macos-sandbox-tts.png)
1. Xcode projesini kapatın. Daha sonra CocoaPods kurduktan sonra bunun farklı bir örnek kullanacaktır.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK'yı CocoaPod olarak yükleyin

1. CocoaPod bağımlılık yöneticisini [yükleme yönergelerinde](https://guides.cocoapods.org/using/getting-started.html)açıklandığı gibi yükleyin.
1. Örnek uygulamanızın dizinine gidin`helloworld`( ). Bu dizine ad `Podfile` ve aşağıdaki içeriği içeren bir metin dosyası yerleştirin:  
   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/text-to-speech/helloworld/Podfile)]
1. Bir terminaldeki dizine `helloworld` gidin ve `pod install`komutu çalıştırın. Bu, hem `helloworld.xcworkspace` örnek uygulamayı hem de Konuşma SDK'sını bir bağımlılık olarak içeren bir Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki lerde kullanılacaktır.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Xcode'da `helloworld.xcworkspace` çalışma alanını açın.
1. Aşağıdaki şekilde otomatik oluşturulan `AppDelegate.m` dosyasının içeriğini değiştirin:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/macos/text-to-speech/helloworld/helloworld/AppDelegate.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Hata ayıklama çıktısını görünür hale getirin (**Hata Ayıklama Alanını** > **Etkinleştir konsolu****görüntüleyin** > ).
1. Menüden **Ürün** -> **Çalıştır'ı** seçerek veya **Oynat** düğmesini tıklatarak örnek kodu oluşturun ve çalıştırın.
1. Bazı metinler girdikten ve uygulamadaki düğmeye tıkladıktan sonra, sentezlenen sesin çolduğunu duymalısınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub'da Objective-C örneklerini keşfedin](https://aka.ms/csspeech/samples)

