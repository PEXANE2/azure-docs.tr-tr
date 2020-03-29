---
title: Konuşma hizmeti ile konuşma çevirisi
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti, uygulamalarınız, araçlarınız ve aygıtlarınıza uçtan uca, gerçek zamanlı, çok dilli konuşma çevirisini eklemenize olanak tanır. Aynı API hem konuşmayı konuşmaya hem de konuşmayı metne çevirmek için kullanılabilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: erhopf
ms.openlocfilehash: f51288da6af3580ba7592950cde4f17d7adad529
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80052612"
---
# <a name="what-is-speech-translation"></a>Konuşma çevirisi nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Konuşma hizmetinden konuşma çevirisi, ses akışlarının gerçek zamanlı, çok dilli konuşma-konuşma ve konuşma-metin çevirisini sağlar. Speech SDK ile uygulamalarınız, araçlarınız ve cihazlarınız sağlanan ses için kaynak transkripsiyonlarına ve çeviri çıktılarına erişebilir. Konuşma algılandıkça ara transkripsiyon ve çeviri sonuçları döndürülür ve final sonuçları sentezlenmiş konuşmaya dönüştürülebilir.

Microsoft'un çeviri motoru iki farklı yaklaşımla desteklenmektedir: istatistiksel makine çevirisi (SMT) ve nöral makine çevirisi (NMT). SMT, birkaç kelimenin bağlamı göz önüne alındığında mümkün olan en iyi çevirileri tahmin etmek için gelişmiş istatistiksel analiz kullanır. NMT ile sinir ağları, sözcükleri çevirmek için cümlelerin tam bağlamını kullanarak daha doğru, doğal görünen çeviriler sağlamak için kullanılır.

Bugün, Microsoft en popüler dillere çeviri için NMT kullanır. [Konuşma-konuşma çevirisi için kullanılabilen](language-support.md#speech-translation) tüm diller NMT tarafından desteklenmektedir. Konuşma-metin çevirisi, dil çiftine bağlı olarak SMT veya NMT'yi kullanabilir. Hedef dil NMT tarafından desteklendiğinde, tam çeviri NMT desteklidir. Hedef dil NMT tarafından desteklenmediği zaman, çeviri, İngilizceyi iki dil arasında "pivot" olarak kullanarak NMT ve SMT'nin bir melezidir.

## <a name="core-features"></a>Temel özellikler

Burada Konuşma SDK ve REST API'ler aracılığıyla mevcut özellikleri şunlardır:

| Kullanım örneği | SDK | REST |
|----------|-----|------|
| Tanıma sonuçlarıyla konuşma-metin çevirisi. | Evet | Hayır |
| Konuşma-konuşma çevirisi. | Evet | Hayır |
| Ara tanıma ve çeviri sonuçları. | Evet | Hayır |

## <a name="get-started-with-speech-translation"></a>Konuşma çevirisi ile başlayın

Kod çalıştırmanız için 10 dakikadan kısa bir süre içinde tasarlanmış hızlı başlangıçlar sunuyoruz. Bu tablo, dile göre düzenlenen konuşma çevirisi nin listesini içerir.

| Hızlı Başlangıç | Platform | API başvurusu |
|------------|----------|---------------|
| [C#, .NET Çekirdek](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=dotnetcore) | Windows | [Gözat](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Çerçeve](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=dotnet) | Windows | [Gözat](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-csharp&tabs=uwp) | Windows | [Gözat](https://aka.ms/csspeech/csharpref) |
| [C++](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-cpp&tabs=windows) | Windows | [Gözat](https://aka.ms/csspeech/cppref)|
| [Java](~/articles/cognitive-services/Speech-Service/quickstarts/translate-speech-to-text.md?pivots=programming-language-java&tabs=jre) | Windows, Linux, macOS | [Gözat](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Örnek kod

Konuşma SDK için örnek kod GitHub mevcuttur. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek çekim tanıma/çeviri ve özel modeller ile çalışma gibi yaygın senaryoları kapsar.

* [Konuşma-metin ve çeviri örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Geçiş kılavuzları

Uygulamalarınız, araçlarınız veya ürünleriniz [Çevirmen Konuşma API'sini](https://docs.microsoft.com/azure/cognitive-services/translator-speech/overview)kullanıyorsa, Konuşma hizmetine geçiş inizi yardımcı olacak kılavuzlar oluşturduk.

* [Çevirmen Konuşma API'sinden Konuşma hizmetine geçiş](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Referans dokümanları

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma Cihazları SDK’sı](speech-devices-sdk.md)
* [REST API: Konuşma-metin](rest-speech-to-text.md)
* [REST API: Metinden konuşmaya](rest-text-to-speech.md)
* [REST API: Toplu transkripsiyon ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)
* [Konuşma SDK alın](speech-sdk.md)
