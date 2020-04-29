---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: db87f31f4702d3487a11a6d833755105455d74c8
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400759"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=windows&pivots=programming-language-cpp)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

## <a name="source-code"></a>Kaynak kod

*HelloWorld. cpp*adlı bir C++ kaynak dosyası oluşturun ve içine aşağıdaki kodu yapıştırın.

[!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp/windows/from-microphone/helloworld/helloworld.cpp#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Kod açıklaması

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Uygulama oluşturma ve çalıştırma

1. Menü çubuğundan uygulamayı derlemek için derleme**Build Solution** ' **ı seçin.** >  Kodun artık hatasız derlenmesi gerekir.

1.  >  **HelloWorld** uygulamasını başlatmak için hata**ayıklamayı Başlat** ' **ı seçin (** veya <kbd>F5</kbd>tuşuna basın).

1. İngilizce bir deyim ya da cümle söyleyin. Uygulama, konuşmanızı metne aktarır ve bunları görüntülenmek üzere uygulamaya geri gönderir.

   ![Başarılı tanıma sonrasında konsol çıkışı](~/articles/cognitive-services/Speech-Service/media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]