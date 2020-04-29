---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: 3613d190ef079d0e477d42b426a224d8e4dda7e6
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81400796"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

## <a name="source-code"></a>Kaynak kod

*QuickStart.py* adlı bir dosya oluşturun ve Içine aşağıdaki python kodunu yapıştırın.

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/from-microphone/quickstart.py#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Kod açıklaması

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Uygulama oluşturma ve çalıştırma

Şimdi uygulamayı test etmeye hazırsınız ve konuşma tanıma işlevini konuşma hizmetini kullanarak doğrulayacağız.

1. **Uygulamanızı başlatın** -komut satırından şunu yazın:
    ```bash
    python quickstart.py
    ```
2. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

