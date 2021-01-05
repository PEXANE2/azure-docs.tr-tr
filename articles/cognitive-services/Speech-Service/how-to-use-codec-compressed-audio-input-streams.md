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
ms.openlocfilehash: 410c0942b9040a6707a51e4ff9f375b9d4728668
ms.sourcegitcommit: 28c93f364c51774e8fbde9afb5aa62f1299e649e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97821579"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Konuşma SDK 'Sı ile codec ile sıkıştırılmış ses girişi kullanma

Konuşma hizmeti SDK 'Sı **sıkıştırılmış ses girişi akış** API 'si, veya kullanarak konuşma hizmetine sıkıştırılmış ses akışını sağlamanın bir yolunu sağlar `PullStream` `PushStream` .

Platform | Diller | Desteklenen GStreamer sürümü
| :--- | ---: | :---:
Windows (UWP hariç)  | C++, C#, Java, Python | [1.15.1](https://gstreamer.freedesktop.org/releases/gstreamer/1.5.1.html)
Linux  | C++, C#, Java, Python | [desteklenen Linux dağıtımları ve hedef mimarileri](~/articles/cognitive-services/speech-service/speech-sdk.md)
Android  | Java | [1.14.4](https://gstreamer.freedesktop.org/data/pkg/android/1.14.4/)

## <a name="speech-sdk-version-required-for-compressed-audio-input"></a>Sıkıştırılmış ses girişi için konuşma SDK sürümü gerekli
* RHEL 8 ve CentOS 8 için konuşma SDK sürümü 1.10.0 veya üzeri gereklidir
* Windows için konuşma SDK sürümü 1.11.0 veya üzeri gereklidir.

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