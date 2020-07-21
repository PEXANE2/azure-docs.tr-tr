---
title: 'Hızlı başlangıç: konuşmayı tanıma, amaç-C-konuşma hizmeti (iOS)'
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler konuşma SDK 'sını kullanarak bir iOS cihazı için hedef-C ' de konuşmayı tanımak üzere bir uygulama oluşturmayı öğrenin.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 06/25/2020
ms.author: chlandsi
ms.openlocfilehash: ba12f258ce7b0db60d3e7343bf12ffc28e8548ab
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86524293"
---
# <a name="quickstart-recognize-speech-in-objective-c-on-ios-by-using-the-speech-sdk"></a>Hızlı başlangıç: konuşma SDK 'sını kullanarak iOS 'ta hedef C 'de konuşmayı tanıma

Hızlı başlangıçlara [konuşma birleştirme](~/articles/cognitive-services/Speech-Service/quickstarts/text-to-speech-langs/objectivec-ios.md)için de erişilebilir.

Bu makalede, Azure bilişsel hizmetler konuşma SDK 'sını kullanarak bir mikrofondan veya kaydedilmiş ses içeren bir dosyadan konuşmaya bir şekilde konuşma yapmak için, amaç-C ' d e bir iOS uygulaması oluşturmayı öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yapmanız gerekir:

* Konuşma hizmeti için bir [abonelik anahtarı](~/articles/cognitive-services/Speech-Service/get-started.md) .
* [Xcode 9.4.1](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12) veya üzeri bir MacOS makinesi.
* Hedef, iOS sürüm 9,3 veya üzeri olarak ayarlandı.

## <a name="get-the-speech-sdk-for-ios"></a>iOS için Konuşma SDK’sını alın

[!INCLUDE [License notice](~/includes/cognitive-services-speech-service-license-notice.md)]

İOS için bilişsel hizmetler konuşma SDK 'Sı Şu anda bir Cocoa çerçevesi olarak dağıtılır.
[Bu Web sitesinden](https://aka.ms/csspeech/iosbinary)indirilebilir. Dosyayı giriş dizininize indirin.

## <a name="create-an-xcode-project"></a>Xcode projesi oluşturma

Xcode ' u başlatın ve **Dosya**  >  **Yeni**  >  **Proje**' yi seçerek yeni bir proje başlatın.
Şablon Seçimi iletişim kutusunda **IOS tek görünüm uygulaması** şablonunu seçin.

İzleyen iletişim kutularında aşağıdaki seçimleri yapın.

1. **Proje seçenekleri** iletişim kutusunda:
    1. Hızlı başlangıç uygulaması için *HelloWorld*gibi bir ad girin.
    1. Zaten bir Apple geliştirici hesabınız varsa, uygun bir kuruluş adı ve kuruluş tanımlayıcısı girin. Test amacıyla, *testorg*gibi bir ad kullanın. Uygulamayı imzalamak için uygun bir sağlama profiline ihtiyacınız vardır. Daha fazla bilgi için [Apple geliştirici sitesine](https://developer.apple.com/)bakın.
    1. **Hedef-C** ' nin proje için dil olarak seçildiğinden emin olun.
    1. Testler ve temel veriler için tüm onay kutularını temizleyin.

    ![Proje ayarları](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-project-settings.png)

1. Bir proje dizini seçin:
   1. Projeyi yerleştirmek için giriş dizininizi seçin. Bu adım, giriş dizininizde Xcode projesi için tüm dosyaları içeren bir HelloWorld dizini oluşturur.
   1. Bu örnek için Git deposu oluşturmayı devre dışı bırakın.
   1. Proje ayarları ekranındaki SDK 'nın yollarını ayarlayın.
      1. **Katıştırılmış** ikililer üstbilgisinin altındaki **genel** sekmesinde, **katıştırılmış ikililer**Ekle başka ' i seçerek SDK kitaplığını bir çerçeve olarak ekleyin  >  **Add other**. Giriş dizininize gidin ve dosyayı seçin `MicrosoftCognitiveServicesSpeech.framework` . Bu eylem SDK kitaplığını otomatik olarak üst bilgi **bağlantılı çerçeveye ve kitaplıklara** ekler.
         ![Eklenen çerçeve](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-framework.png)
      1. **Derleme ayarları** sekmesine gidin ve **Tüm** ayarı seçin.
      1. $ (SRCROOT)/dizinini ekleyin. **arama yolları** başlığı altındaki **çerçeveye arama yolları** .

      ![Çerçeve arama yolları ayarı](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-framework-search-paths.png)

## <a name="set-up-the-ui"></a>Kullanıcı arabirimini ayarlama

Örnek uygulamanın çok basit bir kullanıcı arabirimi vardır. Konuşma tanımayı dosyadan veya mikrofon girişinden ya da sonucu görüntüleyecek bir metin etiketiyle başlatmak için iki düğme vardır. Kullanıcı arabirimi projenin `Main.storyboard` bölümünde ayarlanır. `Main.storyboard`Proje ağacının girişine sağ tıklayıp **Open As**  >  **kaynak kodu**aç ' ı seçerek görsel taslağın XML görünümünü açın.

Otomatik olarak oluşturulan XML 'i bu kodla değiştirin:

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/from-microphone/helloworld/helloworld/Base.lproj/Main.storyboard)]

## <a name="add-the-sample-code"></a>Örnek kod ekleme

1. Bağlantıya sağ tıklayıp **hedefi farklı kaydet**' i seçerek [örnek wav dosyasını](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-speech-sdk/f9807b1079f3a85f07cbb6d762c6b5449d536027/samples/cpp/windows/console/samples/whatstheweatherlike.wav) indirin.
   Wav dosyasını bir Bulucu penceresinden Proje görünümünün kök düzeyine sürükleyerek projeye bir kaynak olarak ekleyin.
   Ayarları değiştirmeden aşağıdaki iletişim kutusunda **son** ' u seçin.
1. Otomatik olarak oluşturulan `ViewController.m` dosyanın içeriğini aşağıdaki kodla değiştirin:

   [!code-objectivec[Quickstart code](~/samples-cognitive-services-speech-sdk/quickstart/objectivec/ios/from-microphone/helloworld/helloworld/ViewController.m#code)]
1. `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.
1. Dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile değiştirin. Örneğin, `westus` ücretsiz deneme aboneliği için kullanın.
1. Mikrofon erişimi için isteği ekleyin. `Info.plist`Proje ağacının girişine sağ tıklayın ve kaynak kodu **olarak aç**' ı seçin  >  **Source Code**. Bölümüne aşağıdaki satırları ekleyin `<dict>` ve dosyayı kaydedin.

    ```xml
    <key>NSMicrophoneUsageDescription</key>
    <string>Need microphone access for speech recognition from microphone.</string>
    ```

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Hata ayıklama alanını **görüntüle**  >  **Debug Area**  >  **konsolunu etkinleştir**' i seçerek hata ayıklama çıktısını görünür yapın.
1. **Ürün**  >  **hedefi** menüsündeki listeden uygulama hedefi olarak geliştirme makinenize bağlı iOS simülatörü veya iOS cihazını seçin.
1. Menüden **ürün**çalıştırması ' nı seçerek iOS benzeticisinde örnek kodu derleyin ve çalıştırın  >  **Run** . Ayrıca **oynat** düğmesini de seçebilirsiniz.
1. Uygulamadaki **tanı (dosya)** düğmesini seçtikten sonra ses dosyasının içeriğini "" Hava durumu nedir? "şeklinde görmeniz gerekir. ses dosyasının içeriklerini görmeniz gerekir.

   ![Sanal iOS uygulaması](~/articles/cognitive-services/Speech-Service/media/sdk/qs-objectivec-simulated-app.png)

1. Uygulamada **tanı (mikrofon)** düğmesini seçip birkaç kelime söyledikten sonra, ekranın alt bölümünde söylenen metni görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da hedef-C örneklerini keşfet](https://aka.ms/csspeech/samples)
