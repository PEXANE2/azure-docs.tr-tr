---
title: Konuşma birleştirme temelleri-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Metinden konuşmaya dönüştürmek için konuşma SDK 'sını nasıl kullanacağınızı öğrenin. Bu makalede, nesne oluşturma, desteklenen ses çıkış biçimleri ve konuşma birleştirme için özel yapılandırma seçenekleri hakkında bilgi edineceksiniz.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 04/14/2020
ms.author: trbye
ms.custom: tracking-python
zone_pivot_groups: programming-languages-set-two-with-js
ms.openlocfilehash: ddcfeaad70e6552f94f9c87b6e9cf24ed15bfba8
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85611484"
---
# <a name="learn-the-basics-of-speech-synthesis"></a>Konuşma sensören'in temellerini öğrenin

Bu makalede, konuşma SDK 'sını kullanarak metinden konuşmaya senşlerini kullanmaya yönelik yaygın tasarım düzenlerini öğrenirsiniz. Temel yapılandırma ve birleştirme işlemleri gerçekleştirerek başlar ve aşağıdakiler de dahil olmak üzere özel uygulama geliştirme için daha gelişmiş örneklere geçin:

* Bellek içi akışlar olarak yanıt alma
* Çıkış örneği hızını ve bit hızını özelleştirme
* SSML kullanarak senssıs istekleri gönderme (konuşma birleştirme biçimlendirme dili)
* Sinir seslerini kullanma

> [!TIP]
> Hızlı başlangıçlarımızın birini tamamlamaya yönelik bir şansınız yoksa, alana 'yi başlatmanıza ve metni kendinize konuşarak deneyebilirsiniz.
> * [Konuşmayı hoparlöre sentezleme](quickstarts/text-to-speech.md)

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
