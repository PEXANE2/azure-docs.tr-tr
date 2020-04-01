---
title: Konuşma SDK ile codec sıkıştırılmış ses akışı - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma SDK ile sıkıştırılmış sesin Konuşma hizmetine nasıl aktarılabildiğini öğrenin. Linux için C++, C#ve Java, Android'de Java ve iOS'ta Objective-C için kullanılabilir.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: amishu
zone_pivot_groups: programming-languages-set-twelve
ms.openlocfilehash: 6aabd27ae38cac0bb9effad2adcadc4935a28c6e
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80409691"
---
# <a name="use-codec-compressed-audio-input-with-the-speech-sdk"></a>Speech SDK ile codec sıkıştırılmış ses girişi kullanın

Konuşma hizmeti SDK **Sıkıştırılmış Ses Girişi Akışı** API ya kullanarak Konuşma hizmetine `PullStream` sıkıştırılmış ses akışı için bir yol sağlar ya da `PushStream`.

> [!IMPORTANT]
> Sıkıştırılmış giriş sesi akışı şu anda Linux'ta C#, C++, Java için desteklenir (Ubuntu 16.04, Ubuntu 18.04, Debian 9, RHEL 8, CentOS 8). Ayrıca Android'de Java ve iOS platformunda Objective-C için desteklenir.
> Konuşma SDK sürüm 1.7.0 veya daha yüksek (sürüm 1.10.0 veya daha yüksek RHEL 8, CentOS 8 için) gereklidir.

[!INCLUDE [supported-audio-formats](includes/supported-audio-formats.md)]

## <a name="prerequisites"></a>Ön koşullar

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/prerequisites.md)]
::: zone-end

::: zone pivot="programming-language-objectivec"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/objectivec/prerequisites.md)]
::: zone-end

## <a name="example-code-using-codec-compressed-audio-input"></a>Codec sıkıştırılmış ses girişi kullanarak örnek kod

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/cpp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/java/examples.md)]
::: zone-end

::: zone pivot="programming-language-objectivec"
[!INCLUDE [prerequisites](includes/how-to/compressed-audio-input/objectivec/examples.md)]
::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmayı nasıl tanıyacağınızı öğrenin](quickstarts/speech-to-text-from-microphone.md)
