---
title: 'Hızlı Başlangıç: Sentezleştirme konuşma C++ , (Linux)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Linux 'ta C++ konuşmayı nasıl sentezleyeceğinizi öğrenin
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: dbc9c2adc2663d3ba0ec3f9ea17b758ca2723441
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553709"
---
# <a name="quickstart-synthesize-speech-in-c-on-linux-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK 'sını kullanarak C++ Linux 'ta konuşmayı sentezleştirme

Hızlı başlangıç, [konuşma tanıma](quickstart-cpp-linux.md)için de kullanılabilir.

Bu makalede, Linux (Ubuntu 16,04, Ubuntu 18,04, de, 9) için bir C++ konsol uygulaması oluşturacaksınız. Bilişsel Hizmetler [konuşma SDK 'sını](speech-sdk.md) kullanarak konuşmayı gerçek zamanlı olarak sentezleştirme ve konuşmayı bilgisayarınızın hoparlörünüz üzerinde oynatacak şekilde kullanıyorsunuz. Uygulama [Linux için Konuşma SDK'sı](https://aka.ms/csspeech/linuxbinary) ve Linux dağıtımınızın C++ derleyicisi (örneğin `g++`) ile oluşturulur.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir konuşma Hizmetleri abonelik anahtarına ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Ayrıntılar için bkz. [konuşma hizmetlerini ücretsiz deneyin](get-started.md) .

## <a name="install-speech-sdk"></a>Konuşma SDK'sını yükleme

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.6.0`.

Linux için Konuşma SDK'sı, gerek 64 bit gerekse 32 bit uygulamalar derlemek için kullanılabilir. Gerekli kitaplıklar ve üst bilgi dosyaları, öğesinden https://aka.ms/csspeech/linuxbinary bir bataklık dosyası olarak indirilebilir.

SDK'yı aşağıda gösterildiği gibi indirin ve yükleyin:

1. SDK'nın bağımlılık dosyalarının yüklü olduğundan emin olun.

   * Ubuntu 'da:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.0 libasound2 wget
     ```

   * On yıl 9 ' da:

     ```sh
     sudo apt-get update
     sudo apt-get install build-essential libssl1.0.2 libasound2 wget
     ```

1. Konuşma SDK'sı dosyalarının ayıklanacağı bir dizin seçin ve `SPEECHSDK_ROOT` ortam değişkenini bu dizine işaret edecek şekilde ayarlayın. Bu değişken, ileride komutlarda bu dizine başvurmayı kolaylaştırır. Örneğin, giriş dizininizdeki `speechsdk` dizinine başvurmak istiyorsanız, şunun gibi bir komut kullanın:

   ```sh
   export SPEECHSDK_ROOT="$HOME/speechsdk"
   ```

1. Henüz yoksa dizini oluşturun.

   ```sh
   mkdir -p "$SPEECHSDK_ROOT"
   ```

1. Konuşma SDK'sı ikili dosyalarını içeren `.tar.gz` arşivini indirin ve açın:

   ```sh
   wget -O SpeechSDK-Linux.tar.gz https://aka.ms/csspeech/linuxbinary
   tar --strip 1 -xzf SpeechSDK-Linux.tar.gz -C "$SPEECHSDK_ROOT"
   ```

1. Açılan paketin en üst düzey dizininin içeriğini doğrulayın:

   ```sh
   ls -l "$SPEECHSDK_ROOT"
   ```

   Dizinde üçüncü taraf bildirim ve lisans dosyaları, ayrıca üstbilgi (`.h`) dosyalarını içeren bir `include` dizini ve kitaplıkları içeren bir `lib` dizini olmalıdır.

   [!INCLUDE [Linux Binary Archive Content](../../../includes/cognitive-services-speech-service-linuxbinary-content.md)]

## <a name="add-sample-code"></a>Örnek kodu ekleyin

1. `helloworld.cpp` adlı bir C++ kaynak dosyası oluşturun ve aşağıdaki kodu dosyaya yapıştırın.

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-linux/helloworld.cpp#code)]

1. Bu yeni dosyada, dizeyi `YourSubscriptionKey` konuşma Hizmetleri abonelik anahtarınızla değiştirin.

1. `YourServiceRegion` dizesini aboneliğinizle ilişkili [bölge](regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

## <a name="build-the-app"></a>Uygulama oluşturma

> [!NOTE]
> Aşağıdaki komutları _tek bir komut satırı_ olarak girdiğinizden emin olun. Bunu yapmanın en kolay yolu, komutu her komutun yanındaki **Kopyala** düğmesini kullanarak kopyalayıp kabuk isteminize yapıştırmaktır.

* Uygulamayı derlemek için **x64**  (64 bit) bir sistemde aşağıdaki komutu çalıştırın.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x64" -l:libasound.so.2
  ```

* Uygulamayı derlemek için **x86** (32 bit) bir sistemde aşağıdaki komutu çalıştırın.

  ```sh
  g++ helloworld.cpp -o helloworld -I "$SPEECHSDK_ROOT/include/cxx_api" -I "$SPEECHSDK_ROOT/include/c_api" --std=c++14 -lpthread -lMicrosoft.CognitiveServices.Speech.core -L "$SPEECHSDK_ROOT/lib/x86" -l:libasound.so.2
  ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Yükleyicinin kitaplık yolunu Konuşma SDK'sı kitaplığına işaret edecek şekilde yapılandırın.

   * **x64** (64 bit) bir sistemde, aşağıdaki komutu girin.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x64"
     ```

   * **x86** (32 bit) bir sistemde, aşağıdaki komutu girin.

     ```sh
     export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SPEECHSDK_ROOT/lib/x86"
     ```

1. Uygulamayı çalıştırın.

   ```sh
   ./helloworld
   ```

1. Konsol penceresinde bir istem belirir ve bir metin yazmanızı ister. Birkaç kelime veya tümce yazın. Yazdığınız metin konuşma hizmetlerine iletilir ve konuşmacının oynadığı konuşmayı sentezleştirilmiştir.

   ```text
   Type some text that you want to speak...
   > hello
   Speech synthesized to speaker for text [hello]
   Press enter to exit...
   ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub C++ 'daki örnekleri keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Ses yazı tiplerini özelleştirme](how-to-customize-voice-font.md)
- [Ses örneklerini Kaydet](record-custom-voice-samples.md)
