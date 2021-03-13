---
title: Konuşma SDK-konuşma hizmeti ile sıkıştırılmış ses codec bileşeni
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'Sı ile konuşma hizmetine sıkıştırılmış ses akışını nasıl sağlayacağınızı öğrenin. Linux için C++, C# ve Java, Android 'de Java ve amaç-C için kullanılabilir.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-two
ms.openlocfilehash: 1e08203076de2073e39c5b5f5eb40b66c88490d7
ms.sourcegitcommit: df1930c9fa3d8f6592f812c42ec611043e817b3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103417752"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile codec ile sıkıştırılmış ses girişi kullanma

Konuşma hizmeti SDK 'Sı **sıkıştırılmış ses girişi akış** API 'si, veya kullanarak konuşma hizmetine sıkıştırılmış ses akışını sağlamanın bir yolunu sağlar `PullStream` `PushStream` .

Platform | Diller | Desteklenen GStreamer sürümü
| :--- | ---: | :---:
Windows (UWP hariç)  | C++, C#, Java, Python | [1.18.3](https://gstreamer.freedesktop.org/data/pkg/windows/1.18.3/)
Linux  | C++, C#, Java, Python | [desteklenen Linux dağıtımları ve hedef mimarileri](~/articles/cognitive-services/speech-service/speech-sdk.md)
Android  | Java | [1.18.3](https://gstreamer.freedesktop.org/data/pkg/android/1.18.3/)

## <a name="speech-sdk-version-required-for-compressed-audio-input"></a>Sıkıştırılmış ses girişi için konuşma SDK sürümü gerekli
* RHEL 8 ve CentOS 8 için konuşma SDK sürümü 1.10.0 veya üzeri gereklidir
* Windows için konuşma SDK sürümü 1.11.0 veya üzeri gereklidir.
* Windows ve Android 'de en son GStreamer için konuşma SDK 'Sı sürüm 1.16.0 veya üzeri.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="gstreamer-required-to-handle-compressed-audio"></a>Sıkıştırılmış sesi işlemek için GStreamer gerekiyor

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Codec ile sıkıştırılmış ses girişini kullanan örnek kod

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/python/examples.md)]
::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmayı tanımayı öğrenin](./get-started-speech-to-text.md)