---
title: 'Hızlı Başlangıç: Konuşmayı tanıma, C++ (MacOS)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak macOS C++ 'ta konuşmayı tanımayı öğrenin
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: d7d35c251848af5cfd5852080d01831085401227
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68554122"
---
# <a name="quickstart-recognize-speech-in-c-on-macos-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını C++ kullanarak MacOS 'ta konuşmayı tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, macOS 10,13 ve C++ üzeri için bir konsol uygulaması oluşturacaksınız. Mac 'in mikrofonunuzdan gerçek zamanlı olarak konuşmayı metne eklemek için bilişsel Hizmetler [konuşma SDK 'sını](speech-sdk.md) kullanın. Uygulama, [macOS Için konuşma SDK 'sı](https://aka.ms/csspeech/macosbinary) ve Mac 'in varsayılan C++ derleyicisi ile oluşturulmuştur (örneğin, `g++`).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir konuşma Hizmetleri abonelik anahtarına ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [konuşma hizmetlerini ücretsiz deneyin](get-started.md) .

## <a name="install-speech-sdk"></a>Konuşma SDK'sını yükleme

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.6.0`.

MacOS için konuşma SDK 'Sı, öğesinden https://aka.ms/csspeech/macosbinary sıkıştırılmış bir çerçeve paketi olarak indirilebilir.

SDK'yı aşağıda gösterildiği gibi indirin ve yükleyin:

1. Konuşma SDK'sı dosyalarının ayıklanacağı bir dizin seçin ve `SPEECHSDK_ROOT` ortam değişkenini bu dizine işaret edecek şekilde ayarlayın. Bu değişken, ileride komutlarda bu dizine başvurmayı kolaylaştırır. Örneğin, giriş dizininizdeki `speechsdk` dizinine başvurmak istiyorsanız, şunun gibi bir komut kullanın:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa dizini oluşturun.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Konuşma SDK çerçevesini içeren `.zip` Arşivi indirin ve ayıklayın:

   ```sh
   wget -O SpeechSDK-macOS.zip https://aka.ms/csspeech/macosbinary
   unzip SpeechSDK-macOS.zip -d "$SPEECHSDK_ROOT"
   ```

1. Açılan paketin en üst düzey dizininin içeriğini doğrulayın:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Dizin listesi, üçüncü taraf bildirim ve lisans dosyalarının yanı sıra bir `MicrosoftCognitiveServicesSpeech.framework` dizin içermelidir.

## <a name="add-sample-code"></a>Örnek kodu ekleme

1. `helloworld.cpp` adlı bir C++ kaynak dosyası oluşturun ve aşağıdaki kodu dosyaya yapıştırın.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-macos/helloworld.cpp#code)]

1. Bu yeni dosyada, dizeyi `YourSubscriptionKey` konuşma Hizmetleri abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

## <a name="build-the-app"></a>Uygulama oluşturma

> [!NOTE]
> Aşağıdaki komutları _tek bir komut satırı_ olarak girdiğinizden emin olun. Bunu yapmanın en kolay yolu, komutu her komutun yanındaki **Kopyala** düğmesini kullanarak kopyalayıp kabuk isteminize yapıştırmaktır.

* Uygulamayı derlemek için aşağıdaki komutu çalıştırın.

  ```sh
  g++ helloworld.cpp -o helloworld --std=c++14 -F${SPEECHSDK_ROOT} -framework MicrosoftCognitiveServicesSpeech
  ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Yükleyicinin kitaplık yolunu Konuşma SDK'sı kitaplığına işaret edecek şekilde yapılandırın.

    ```sh
    export DYLD_FRAMEWORK_PATH="$DYLD_FRAMEWORK_PATH:$SPEECHSDK_ROOT"
    ```

1. Uygulamayı çalıştırın.

   ```sh
   ./helloworld
   ```

1. Konsol penceresinde bir istem görünerek bir şey söylemenizi ister. İngilizce bir deyim ya da cümle söyleyin. Konuşma, konuşma hizmetlerine iletilir ve aynı pencerede görüntülenen metne gönderilir.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C++ 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
