---
title: 'Quickstart: Bir mikrofon, Objective-C konuşma tanıma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Speech SDK'yı kullanarak iOS'ta Objective-C'deki konuşmayı nasıl tanıyın
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/23/2019
ms.author: chlandsi
ms.openlocfilehash: c1246b19670a18f8dadc0c5e1c64dd5af4c1b210
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75380788"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-by-using-the-speech-sdk"></a>Hızlı başlangıç: Speech SDK kullanarak iOS'ta Objective-C'deki konuşmayı tanıma

Quickstarts konuşma [sentezi](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/objectivec-ios.md)için de kullanılabilir.

Bu makalede, Ses'ten veya kaydedilmiş ses içeren bir dosyadan metni metine çevirmek için Azure Bilişsel Hizmetler Konuşma SDK'sını kullanarak Objective-C'de bir iOS uygulaması oluşturmayı öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yapmanız gerekir:

* Konuşma hizmeti için [bir abonelik anahtarı.](~/articles/cognitive-services/Speech-Service/get-started.md)
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya sonraki bir macOS makinesi.
* Hedef iOS sürüm 9.3 veya daha sonra ayarlanır.

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

iOS için Bilişsel Hizmetler Konuşma SDK şu anda bir Kakao çerçeve olarak dağıtılır.
[Bu web sitesinden](https://aka.ms/csspeech/iosbinary)indirilebilir. Dosyayı giriş dizininize indirin.

## <a name="create-an-xcode-project"></a>Bir Xcode projesi oluşturma

Xcode'u başlatın ve **Dosya** > **Yeni** > **Projesi'ni**seçerek yeni bir proje başlatın.
Şablon seçimi iletişim kutusunda **iOS Tek Görünüm Uygulaması** şablonu'nu seçin.

Bunu izleyen iletişim kutularında aşağıdaki seçimleri yapın.

1. Proje **Seçenekleri** iletişim kutusunda:
    1. Quickstart uygulaması için bir ad girin, örneğin, *helloworld*.
    1. Zaten bir Apple geliştirici hesabınız varsa uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, *testorg*gibi bir ad kullanın. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız var. Daha fazla bilgi için [Apple geliştirici sitesine](https://developer.apple.com/)bakın.
    1. Project'in dili olarak **Objective-C'nin** seçildiğinden emin olun.
    1. Testler ve temel veriler için tüm onay kutularını temizleyin.

    ![Proje ayarları](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-project-settings.png)

1. Bir proje dizini seçin:
   1. Projeyi yerleştirmek için giriş dizininizi seçin. Bu adım, xcode projesi için tüm dosyaları içeren ev dizini bir helloworld dizini oluşturur.
   1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
   1. Proje ayarları ekranındaki Yolları SDK'ya ayarlayın.
      1. **Gömülü İkili** üstbilginin altındaki **Genel** sekmesinde, **Katıştırılmış ikiliekle** > **diğerini**ekle'yi seçerek Çerçeve Olarak SDK kitaplığını ekleyin. Ev dizininize gidin ve dosyayı `MicrosoftCognitiveServicesSpeech.framework`seçin. Bu eylem, SDK kitaplığını üstbilgi **Bağlantılı Çerçeve ve Kitaplıklara** otomatik olarak ekler.
         ![Eklenen çerçeve](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-framework.png)
      1. **Ayarlar'ı Oluştur** sekmesine gidin ve **Tüm** ayarını seçin.
      1. $(SRCROOT)/... dizini ekleyin... **Arama Yolları** başlığı altında Çerçeve **Arama Yolları'na.**

      ![Çerçeve Arama Yolları ayarı](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Kullanıcı arabirimini ayarlama

Örnek uygulama çok basit bir kullanıcı arama b.iI vardır. Dosyadan veya mikrofon girişinden konuşma tanımayı başlatmak için iki düğmesi ve sonucu görüntülemek için bir metin etiketi vardır. Kullanıcı arabirimi projenin `Main.storyboard` bölümünde ayarlanır. Proje `Main.storyboard` ağacının girişini sağ tıklayarak ve Kaynak**Kodu**Olarak >  **Aç'ı**seçerek film şeridinin XML görünümünü açın.

Otomatik oluşturulan XML'yi bu kodla değiştirin:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/from-microphone/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Bağlantıyı sağ tıklayarak ve hedefi kaydet'i seçerek [örnek wav dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) **indirin.**
   Wav dosyasını bir Bulucu penceresinden Proje görünümünün kök düzeyine sürükleyerek projeye bir kaynak olarak ekleyin.
   Ayarları değiştirmeden aşağıdaki iletişim kutusunda **Bitir'i** seçin.
1. Otomatik oluşturulan `ViewController.m` dosyanın içeriğini aşağıdaki kodla değiştirin:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/from-microphone/helloworld/helloworld/ViewController.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. Dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölgeyle](~/articles/cognitive-services/Speech-Service/regions.md) değiştirin. Örneğin, ücretsiz `westus` deneme aboneliği için kullanın.
1. Mikrofon erişimi için isteği ekleyin. Proje ağacının `Info.plist` girişini sağ tıklatın ve Kaynak > **Kodu** **Olarak Aç'ı**seçin. `<dict>` Bölüme aşağıdaki satırları ekleyin ve sonra dosyayı kaydedin.

    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Hata Ayıklama **View** > **Alanını** > **Etkinleştir konsolu**seçerek hata ayıklama çıktısını görünür hale getirin.
1. **Ürün** > **Hedefi** menüsündeki listeden uygulamanın hedefi olarak iOS simülatörü veya geliştirme makinenize bağlı bir iOS aygıtını seçin.
1. Menüden **Ürün** > **Çalıştır'ı** seçerek iOS simülatöründe örnek kodu oluşturun ve çalıştırın. **Ayrıca Oynat** düğmesini de seçebilirsiniz.
1. Uygulamadaki **Tanı (Dosya)** düğmesini seçtikten sonra ses dosyasının içeriğini görmeniz gerekir: "Hava nasıldır?" ses dosyasının içeriklerini görmeniz gerekir.

   ![Simüle iOS uygulaması](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-simulated-app.png)

1. Uygulamadaki **Tanı (Mikrofon)** düğmesini seçtikten ve birkaç kelime söyledikten sonra, ekranalt kısmında konuştuğunuz metni görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub'da Objective-C örneklerini keşfedin](https://aka.ms/csspeech/samples)
