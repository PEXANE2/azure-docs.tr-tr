---
title: Metinden konuşmaya hızlı başlangıç-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Metinden konuşmaya dönüştürmek için konuşma SDK 'sını nasıl kullanacağınızı öğrenin. Bu hızlı başlangıçta, nesne oluşturma ve tasarım desenleri, desteklenen ses çıkış biçimleri, konuşma CLı ve konuşma birleştirme için özel yapılandırma seçenekleri hakkında bilgi edineceksiniz.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/01/2020
ms.author: trbye
ms.custom: devx-track-python, devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
zone_pivot_groups: programming-languages-set-twenty-four
keywords: Metinden konuşmaya
ms.openlocfilehash: 7a41c4d9c1074b376da3de556caf63ced0bc84ec
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102428239"
---
# <a name="get-started-with-text-to-speech"></a>Metin okumayı kullanmaya başlama

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

::: zone pivot="programming-languages-objectivec-swift"
[!INCLUDE [Objective-C and Swift Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-objectivec-swift.md)]
::: zone-end

::: zone pivot="programming-language-python"
[!INCLUDE [Python Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-python.md)]
::: zone-end

::: zone pivot="programming-language-curl"
[!INCLUDE [REST include](includes/how-to/text-to-speech-basics/text-to-speech-basics-curl.md)]
::: zone-end

::: zone pivot="programmer-tool-spx"
[!INCLUDE [CLI Basics include](includes/how-to/text-to-speech-basics/text-to-speech-basics-cli.md)]
::: zone-end

## <a name="get-position-information"></a>Konum bilgilerini al

Bu zamanlamaya göre belirli bir işlem yapabilmesi için, projeniz konuşmayı metne göre konuşularak bir sözcüğün ne zaman konuşmaya çalıştığını bilmeniz gerekebilir. Örnek olarak, kelimeleri konuşulan sözcükleri vurgulamak isterseniz, neyin vurgulanmasını, ne zaman vurgulanmasını ve ne kadar vurgulanmasını istediğinizi bilmeniz gerekir.

Bunu, `WordBoundary` içinde bulunan olayını kullanarak gerçekleştirebilirsiniz `SpeechSynthesizer` . Bu olay, her yeni konuşulan kelimenin başlangıcında ortaya çıkar ve söylenen akış içinde bir saat ve giriş isteminde bir metin uzaklığında bir zaman boşluğu sağlar.

* `AudioOffset` Çıktının başlangıcı ve sonraki sözcüğün başlangıcı arasındaki çıktı sesinin geçen süreyi bildirir. Bu, yüz-nanosaniyelik birim (HNS) ile 10.000 HNS ile 1 milisaniyeye denk ölçülür.
* `WordOffset` Giriş dizesindeki karakter konumunu (özgün metin veya [SSML](speech-synthesis-markup.md)), konuşmadan önce gelen kelimeden hemen önce bildirir.

> [!NOTE]
> `WordBoundary` çıkış ses verileri kullanılabilir hale geldiği için olaylar tetiklenir, bu da çıkış cihazına kayıttan yürütmeyi daha hızlı olacaktır. Akış zamanlamasını "gerçek zamanlı" olarak uygun şekilde eşitleme, çağıran tarafından yapılmalıdır.

`WordBoundary`GitHub 'da [metin okuma örneklerinde](https://aka.ms/csspeech/samples) kullanma örneklerini bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel Ses’i kullanmaya başlama](how-to-custom-voice.md)
* [SSML ile sentezi geliştirme](speech-synthesis-markup.md)
* Kitap ve haber makaleleri gibi büyük metin örnekleri için [uzun ses API](long-audio-api.md) 'sini nasıl kullanacağınızı öğrenin
* GitHub 'daki [hızlı başlangıç örneklerine](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart) bakın
