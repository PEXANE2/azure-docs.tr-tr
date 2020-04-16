---
title: Konuşma sentezi temelleri - Konuşma servisi
titleSuffix: Azure Cognitive Services
description: Metinden konuşmaya dönüştürmek için Konuşma SDK'sını nasıl kullanacağınızı öğrenin. Bu makalede, nesne yapısı, desteklenen ses çıktıbiçimleri ve konuşma sentezi için özel yapılandırma seçenekleri hakkında bilgi edineceksiniz.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: trbye
zone_pivot_groups: programming-languages-set-sixteen
ms.openlocfilehash: 74f762dd0004bf72b703d0f611a16762b1ff028d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399530"
---
# <a name="learn-the-basics-of-speech-synthesis"></a>Konuşma sentezinin temellerini öğrenin

Bu makalede, Konuşma SDK kullanarak metin-konuşma sentezi yapmak için ortak tasarım desenleri öğrenirler. Temel yapılandırma ve sentez yaparak başlarsınız ve aşağıdakiler dahil olmak üzere özel uygulama geliştirme için daha gelişmiş örneklere geçersiniz:

* Yanıtları bellek içi akış olarak alma
* Çıktı örnek oranını ve bit oranını özelleştirme
* SSML kullanarak sentez istekleri gönderme (konuşma sentezi biçimlendirme dili)
* Nöral sesler kullanma

> [!TIP]
> Hızlı başlangıçlarımızdan birini tamamlama şansınız olmadıysa, lastikleri tekmelemenizi ve konuşma tanımayı kendiniz denemenizi öneririz.
> * [Mikrofondan konuşmayı tanıma](quickstarts/text-to-speech.md)

::: zone pivot="programming-language-csharp"
[!INCLUDE [C# Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-csharp.md)]
::: zone-end

::: zone pivot="programming-language-cpp"
[!INCLUDE [C++ Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-cpp.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [Java Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-java.md)]
::: zone-end

::: zone pivot="programming-language-javascript"
[!INCLUDE [JavaScript Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-javascript.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-python.md)]
::: zone-end

::: zone pivot="programming-language-more"
[!INCLUDE [More languages include](./includes/how-to/speech-to-text-basics/more.md)]
::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Ses’i kullanmaya başlama](how-to-custom-voice.md)
* [SSML ile sentezi geliştirme](speech-synthesis-markup.md)