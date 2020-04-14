---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 04/03/2020
ms.author: dapine
ms.openlocfilehash: ce694641ef51ae2a1afd33a6220bfb467799896a
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81275469"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices" target="_blank">Azure Konuşma kaynağı oluşturma<span class="docon docon-navigate-external x-hidden-focus"></span></a>
> * [Geliştirme ortamınızı kurup boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?pivots=programming-language-python)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

## <a name="source-code"></a>Kaynak kod

*quickstart.py* adlı bir dosya oluşturun ve aşağıdaki Python kodunu yapıştırın.

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/from-microphone/quickstart.py#code)]

[!INCLUDE [replace key and region](../replace-key-and-region.md)]

## <a name="code-explanation"></a>Kod açıklaması

[!INCLUDE [code explanation](../code-explanation.md)]

## <a name="build-and-run-app"></a>Uygulama oluşturma ve çalıştırma

Artık uygulamayı sınamaya ve Konuşma hizmetini kullanarak konuşma tanıma işlevini doğrulamaya hazırsınız.

1. **Uygulamanızı başlatın** - Komut satırından yazın:
    ```bash
    python quickstart.py
    ```
2. **Tanımayı başlatın** - İngilizce bir ifade konuşmanızı ister. Konuşmanız Konuşma hizmetine gönderilir, metin olarak yazılır ve konsolda işlenir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]

