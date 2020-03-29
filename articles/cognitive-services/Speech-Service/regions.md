---
title: Bölgeler - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti için konuşma-metin, metinden konuşmaya ve konuşma çevirisi de dahil olmak üzere kullanılabilir bölgelerin ve uç noktaların listesi.
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 560575ca7f51218e472abecb4319f4a3db69b1ff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220473"
---
# <a name="speech-service-supported-regions"></a>Konuşma hizmeti desteklenen bölgeler

Konuşma hizmeti, uygulamanızın sesi metne dönüştürmesine, konuşma çevirisi yapmasına ve gizli metni konuşmaya dönüştürmesine olanak tanır. Hizmet, Konuşma SDK ve REST API'leri için benzersiz uç noktaları olan birden çok bölgede kullanılabilir.

Tüm bölgeler için konuşma deneyiminize özel yapılandırmalar gerçekleştirmek için Konuşma portalına buradan ulaşabilirsiniz:https://speech.microsoft.com

Konuşma hizmetinizin çağrıları için, aramanın aboneliğiniz için bölgeyle eşleştiğinden emin olun.

## <a name="speech-sdk"></a>Konuşma SDK'sı

Konuşma [SDK'](speech-sdk.md)da bölgeler bir dize olarak belirtilir (örneğin, C#için Konuşma `SpeechConfig.FromSubscription` SDK'sında bir parametre olarak).

### <a name="speech-to-text-text-to-speech-and-translation"></a>Konuşmadan metne, metinden konuşmaya ve çeviriye

Konuşma özelleştirme portalına buradan ulaşabilirsiniz:https://speech.microsoft.com

Konuşma hizmeti konuşma **tanıma,** **metin-konuşma**ve **çeviri**için bu bölgelerde mevcuttur:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

[Konuşma SDK](speech-sdk.md)kullanıyorsanız , bölgeler **Bölge tanımlayıcısı** tarafından belirtilir (örneğin, bir parametre `SpeechConfig.FromSubscription`olarak). Bölgenin aboneliğinizin bölgesiyle eşleştidiğinden emin olun.

### <a name="intent-recognition"></a>Amaç tanıma

Konuşma SDK üzerinden **niyet tanıma** için kullanılabilir bölgeler şunlardır:

| Küresel bölge | Bölge           | Bölge tanımlayıcısı |
| ------------- | ---------------- | -------------------- |
| Asya          | Doğu Asya        | `eastasia`           |
| Asya          | Güneydoğu Asya   | `southeastasia`      |
| Avustralya     | Doğu Avustralya   | `australiaeast`      |
| Avrupa        | Kuzey Avrupa     | `northeurope`        |
| Avrupa        | Batı Avrupa      | `westeurope`         |
| Kuzey Amerika | Doğu ABD          | `eastus`             |
| Kuzey Amerika | Doğu ABD 2        | `eastus2`            |
| Kuzey Amerika | Orta Güney ABD | `southcentralus`     |
| Kuzey Amerika | Orta Batı ABD  | `westcentralus`      |
| Kuzey Amerika | Batı ABD          | `westus`             |
| Kuzey Amerika | Batı ABD 2        | `westus2`            |
| Güney Amerika | Güney Brezilya     | `brazilsouth`        |

Bu, [Dil Anlama hizmeti (LUIS)](/azure/cognitive-services/luis/luis-reference-regions)tarafından desteklenen yayımlama bölgelerinin bir alt kümesidir.

### <a name="voice-assistants"></a>Ses yardımcıları

[Konuşma SDK](speech-sdk.md) bu bölgelerde **ses asistanı** yeteneklerini destekler:

| Bölge         | Bölge tanımlayıcısı |
| -------------- | -------------------- |
| Batı ABD        | `westus`             |
| Batı ABD 2      | `westus2`            |
| Doğu ABD        | `eastus`             |
| Doğu ABD 2      | `eastus2`            |
| Batı Avrupa    | `westeurope`         |
| Kuzey Avrupa   | `northeurope`        |
| Güneydoğu Asya | `southeastasia`      |

## <a name="rest-apis"></a>REST API'leri

Konuşma hizmeti ayrıca konuşma-metin ve metin-konuşma istekleri için REST uç noktaları ortaya çıkarır.

### <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Konuşmadan metne başvuru belgeleri için, [konuşmadan metne REST API'ye](rest-speech-to-text.md)bakın.

REST API için bitiş noktası şu biçime sahiptir:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

Aboneliğinizin bulunduğu bölgeye uyan tanımlayıcıyı bu tablodan değiştirin: `<REGION_IDENTIFIER>`

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> 4xx HTTP hatası almamak için dil parametresi URL'ye eklenmelidir. Örneğin, Batı ABD bitiş noktasını kullanarak ABD İngilizcesi olarak ayarlanan dil: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.

### <a name="text-to-speech"></a>Metin okuma

Metinden konuşmaya başvuru belgeleri için [metinden konuşmaya REST API'ye](rest-text-to-speech.md)bakın.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
