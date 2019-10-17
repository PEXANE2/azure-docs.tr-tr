---
title: 'Hızlı başlangıç: Sentezleştirme konuşma, amaç-C-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak iOS 'ta bir konuşmayı hedefe nasıl sentezleyeceğinizi öğrenin
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: 099a2f707431827996aa304aa706a9efbe54f589
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72438829"
---
# <a name="quickstart-synthesize-speech-in-objective-c-on-ios-using-the-speech-sdk"></a>Hızlı başlangıç: konuşma SDK 'sını kullanarak iOS 'ta hedef için konuşmayı Sentezleştirme

Hızlı başlangıç, [konuşma tanıma](quickstart-objectivec-ios.md)için de kullanılabilir.

Bu makalede, metinden konuşmayı sentezleştirmek için bilişsel hizmetler konuşma SDK 'sını kullanarak amaç-C ' d e bir iOS uygulaması oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, önkoşulların bir listesi aşağıda verilmiştir:

* Konuşma hizmeti için bir [abonelik anahtarı](get-started.md)
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri bir MacOS makinesi
* Hedef, iOS sürüm 9,3 veya üzeri olarak ayarlandı

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bu öğreticinin SDK sürümü 1.7.0 ' den önceki sürümüyle çalışmadığını unutmayın.

İOS için bilişsel hizmetler konuşma SDK 'Sı Şu anda bir Cocoa çerçevesi olarak dağıtılır.
Xcode projelerinde [CocoaPod](https://cocoapods.org/)olarak veya https://aka.ms/csspeech/iosbinary ' den indirilen ve el ile bağlanmış bir şekilde kullanılabilir. Bu kılavuzda bir CocoaPod kullanılır.

## <a name="create-an-xcode-project"></a>Xcode Projesi oluşturma

Xcode’u başlatın ve **Dosya** > **Yeni** > **Proje** seçeneklerine tıklayarak yeni bir proje başlatın.
Şablon seçimi iletişim kutusunda, “iOS Tek Görünüm Uygulaması” şablonunu seçin.

Takip eden iletişim kutularında, aşağıdaki seçimleri yapın:

1. Proje Seçenekleri İletişim Kutusu
    1. Hızlı başlangıç uygulaması için bir ad girin, örneğin `helloworld`.
    1. Zaten bir Apple geliştirici hesabınız varsa uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, `testorg` gibi herhangi bir ad seçebilirsiniz. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız vardır. Ayrıntılar için [Apple Developer sitesine](https://developer.apple.com/) bakın.
    1. Proje dili olarak Objective-C seçildiğinden emin olun.
    1. Testler ve temel veriler için tüm onay kutularını devre dışı bırakın.
    ![Proje Ayarları](media/sdk/qs-objectivec-project-settings.png)
1. Proje dizini seçin
    1. Projeyi yerleştirmek için giriş dizininizi seçin. Bu, giriş dizininizde Xcode projesi için tüm dosyaları içeren bir `helloworld` dizini oluşturur.
    1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.

## <a name="install-the-sdk-as-a-cocoapod"></a>SDK 'Yı bir CocoaPod olarak yükler

1. [Yükleme yönergeleri](https://guides.cocoapods.org/using/getting-started.html)bölümünde açıklandığı gibi CocoaPod Dependency Manager 'ı yükleme.
1. Örnek uygulamanızın dizinine gidin (`helloworld`). @No__t-0 adlı bir metin dosyası ve bu dizine aşağıdaki içeriği koyun:  
   [!code-ruby[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/objectivec-ios/helloworld/Podfile)]
1. Terminalde `helloworld` dizinine gidin ve `pod install` komutunu çalıştırın. Bu işlem, bağımlılık olarak hem örnek uygulamayı hem de konuşma SDK 'sını içeren `helloworld.xcworkspace` Xcode çalışma alanı oluşturur. Bu çalışma alanı aşağıdaki şekilde kullanılacaktır.

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Xcode 'da `helloworld.xcworkspace` çalışma alanını açın.
1. Aşağıdaki şekilde otomatik oluşturulan `AppDelegate.m` dosyasının içeriğini değiştirin:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/objectivec-ios/helloworld/helloworld/AppDelegate.m#code)]
1. Aşağıdaki şekilde otomatik oluşturulan `ViewController.m` dosyasının içeriğini değiştirin:  
   [!code-objectivec[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/objectivec-ios/helloworld/helloworld/ViewController.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. `YourServiceRegion` dizesini, aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin, ücretsiz deneme aboneliği için `westus`) değiştirin.

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Hata ayıklama çıkışını görünür hale getirin (**Görünüm** > **Hata Ayıklama Alanı** > **Konsolu Etkinleştir**).
1. **Ürün** > **hedef** menüsündeki listeden uygulama hedefi olarak, geliştirme makinenize bağlı iOS simülatörü veya iOS cihazını seçin.
1. Menüden **Ürün** > **Çalıştır** seçeneklerini belirleyerek veya **Oynat** düğmesine tıklayarak iOS simülatöründe örnek kodu derleyin ve çalıştırın.

   ![iOS Uygulaması Simülasyonu](media/sdk/qs-objectivec-simulated-app-tts.png)

1. Bazı metinleri girdikten ve uygulamadaki düğmeye tıkladıktan sonra, birleştirilmiş sesin çalındığını duymalısınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da hedef-C örneklerini keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
