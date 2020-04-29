---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: c35f30e1f403baf5fa01d450f77b77120a5b28cf
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400798"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=macos&pivots=programming-language-cpp)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

## <a name="source-code"></a>Kaynak kod

*HelloWorld. cpp*adlı bir C++ kaynak dosyası oluşturun ve içine aşağıdaki kodu yapıştırın.

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/macos/from-microphone/helloworld.cpp#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Kod açıklaması

[!INCLUDE [code explanation](../code-explanation.md)]

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

1. Konsol penceresinde bir istem görünerek bir şey söylemenizi ister. İngilizce bir deyim ya da cümle söyleyin. Söyledikleriniz Konuşma hizmetine aktarılır ve metne dönüştürülür; metin aynı pencerede görünür.

   ```text
   Say something...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
