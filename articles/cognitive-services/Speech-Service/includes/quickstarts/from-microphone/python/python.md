---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: trbye
ms.openlocfilehash: f26f0ab6da398dcdee307f89b27cca780d08af85
ms.sourcegitcommit: 374d1533ea2f2d9d3f8b6e6a8e65c6a5cd4aea47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85839026"
---
## <a name="prerequisites"></a>Önkoşullar

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

Şimdi konuşma hizmetini kullanarak konuşma tanımayı test etmeye hazırsınız. 

Bunu macOS üzerinde çalıştırıyorsanız ve bir mikrofon kullanan oluşturduğunuz ilk Python uygulaması ise, büyük olasılıkla mikrofona Terminal erişimi sağlamanız gerekir. **Sistem ayarları** ' nı açın ve **güvenlik & gizlilik**' i seçin. Sonra **Gizlilik** ' i seçin ve listeden **mikrofonu** bulun. Son olarak, **Terminal** ve Kaydet ' i seçin. 

1. **Uygulamanızı başlatın** -komut satırından şunu yazın:
    ```bash
    python quickstart.py
    ```
2. **Tanımayı Başlat** -bu, İngilizce bir tümceciği konuşarak ister. Konuşma konuşma hizmetine gönderilir, metin olarak yeniden oluşturulur ve konsolunda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
