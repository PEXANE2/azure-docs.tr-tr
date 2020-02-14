---
title: Bölgeler-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne, metinden konuşmaya ve konuşma çevirisi de dahil olmak üzere konuşma hizmeti için kullanılabilir bölgelerin ve uç noktaların listesi.
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: f1f425a6e3c66a72c7632cfea16d78ea6cc0d319
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201161"
---
# <a name="speech-service-supported-regions"></a>Konuşma hizmeti tarafından desteklenen bölgeler

Konuşma hizmeti, uygulamanızın sesi metne dönüştürün, gizli metin okuma ve konuşma çevirisi gerçekleştirin sağlar. Hizmet, REST API'leri ve Speech SDK'sı için benzersiz uç noktaları ile birden fazla bölgede kullanılabilir.

Tüm bölgeler için konuşma deneyiminize özel yapılandırma gerçekleştirmeye yönelik konuşma portalı şurada bulunabilir: https://speech.microsoft.com

Konuşma hizmetinizin etkinleştirmeleri için, çağrının aboneliğinizin bölgesiyle eşleştiğinden emin olun.

## <a name="speech-sdk"></a>Konuşma SDK'sı

[Konuşma SDK 'sında](speech-sdk.md), bölgeler bir dize olarak belirtilir (örneğin, için C#konuşma SDK 'sında `SpeechConfig.FromSubscription` parametresi olarak).

### <a name="speech-to-text-text-to-speech-and-translation"></a>Konuşmadan metne, metinden konuşmaya ve çeviri

Konuşma özelleştirme portalı şurada bulunabilir: https://speech.microsoft.com

Konuşma hizmeti, **konuşma tanıma**, **metinden konuşmaya**ve **çeviri**için şu bölgelerde kullanılabilir:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

[Konuşma SDK 'sını](speech-sdk.md)kullanıyorsanız, bölgeler **bölge tanımlayıcısı** tarafından belirtilir (örneğin, `SpeechConfig.FromSubscription`parametresi olarak). Bölgenin aboneliğinizin bölgesiyle aynı olduğundan emin olun.

### <a name="intent-recognition"></a>Amaç tanıma

Konuşma SDK 'Sı aracılığıyla **Amaç tanıma** için kullanılabilir bölgeler şunlardır:

| Genel bölge | Bölge           | Konuşma SDK parametresi |
| ------------- | ---------------- | -------------------- |
| Asya          | Doğu Asya        | `eastasia`           |
| Asya          | Güneydoğu Asya   | `southeastasia`      |
| Avustralya     | Avustralya Doğu   | `australiaeast`      |
| Avrupa        | Kuzey Avrupa     | `northeurope`        |
| Avrupa        | Batı Avrupa      | `westeurope`         |
| Kuzey Amerika | Doğu ABD          | `eastus`             |
| Kuzey Amerika | Doğu ABD 2        | `eastus2`            |
| Kuzey Amerika | Orta Güney ABD | `southcentralus`     |
| Kuzey Amerika | Orta Batı ABD  | `westcentralus`      |
| Kuzey Amerika | Batı ABD          | `westus`             |
| Kuzey Amerika | Batı ABD 2        | `westus2`            |
| Güney Amerika | Brezilya Güney     | `brazilsouth`        |

Bu, [Language Understanding hizmeti (Luo)](/azure/cognitive-services/luis/luis-reference-regions)tarafından desteklenen yayımlama bölgelerinin bir alt kümesidir.

### <a name="voice-assistants"></a>Ses yardımcıları

[Konuşma SDK 'sı](speech-sdk.md) bu bölgelerde **sesli yardımcı** yeteneklerini destekler:

| Bölge         | Konuşma SDK parametresi |
| -------------- | -------------------- |
| Batı ABD        | `westus`             |
| Batı ABD 2      | `westus2`            |
| Doğu ABD        | `eastus`             |
| Doğu ABD 2      | `eastus2`            |
| Batı Avrupa    | `westeurope`         |
| Kuzey Avrupa   | `northeurope`        |
| Güneydoğu Asya | `southeastasia`      |

## <a name="rest-apis"></a>REST API'leri

Konuşma hizmeti de konuşma metin ve metin okuma istekleri için REST uç noktalarını kullanıma sunar.

### <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Konuşmadan metne başvuru belgeleri için bkz. [konuşmayı metne dönüştürme REST API](rest-speech-to-text.md).

REST API uç noktası şu biçimdedir:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

`<REGION_IDENTIFIER>`, bu tablodaki aboneliğinizin bölgesiyle eşleşen tanımlayıcıyla değiştirin:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> Bir 4xx HTTP hatası almamak için dil parametresi URL 'ye eklenmelidir. Örneğin, Batı ABD uç noktası kullanılarak ABD Ingilizcesi olarak ayarlanan dil: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.

### <a name="text-to-speech"></a>Metin okuma

Metinden konuşmaya başvuru belgeleri için bkz. [metin okuma REST API](rest-text-to-speech.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
