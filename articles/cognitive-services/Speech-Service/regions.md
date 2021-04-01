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
ms.date: 08/20/2020
ms.author: panosper
ms.custom: seodec18,references_regions
ms.openlocfilehash: 646d29e72b91cd6afcde8e70ad8fd8715442b88e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "98786804"
---
# <a name="speech-service-supported-regions"></a>Konuşma hizmeti tarafından desteklenen bölgeler

Konuşma hizmeti, uygulamanızın sesi metne dönüştürmesini, konuşma çevirisi gerçekleştirmesini ve metni konuşmaya dönüştürmeyi sağlar. Hizmet, konuşma SDK 'Sı ve REST API 'Leri için benzersiz uç noktalara sahip birden çok bölgede kullanılabilir.

Tüm bölgeler için konuşma deneyiminize özel yapılandırma gerçekleştirmeye yönelik konuşma portalı buradan edinilebilir: https://speech.microsoft.com

Bölgeleri düşünürken aşağıdaki noktaları göz önünde bulundurun:

* Uygulamanız bir [konuşma SDK 'sı](speech-sdk.md)kullanıyorsa, `westus` bir konuşma yapılandırması oluştururken gibi bölge tanımlayıcısı sağlarsınız.
* Uygulamanız konuşma hizmeti 'nin [REST API 'lerinden](./overview.md#reference-docs)birini kullanıyorsa bölge, istek yaparken kullandığınız uç nokta URI 'sinin bir parçasıdır.
* Bölge için oluşturulan anahtarlar yalnızca o bölgede geçerlidir. Bunları diğer bölgelerle birlikte kullanmaya çalışmak, kimlik doğrulama hatalarına neden olur.

## <a name="speech-sdk"></a>Konuşma SDK'sı

[Konuşma SDK 'sında](speech-sdk.md), bölgeler bir dize olarak belirtilir (örneğin, `SpeechConfig.FromSubscription` C# IÇIN konuşma SDK 'sında öğesine bir parametre olarak).

### <a name="speech-to-text-text-to-speech-and-translation"></a>Konuşmadan metne, metinden konuşmaya ve çeviri

Konuşma özelleştirme portalı şurada bulunabilir: https://speech.microsoft.com

Konuşma hizmeti, **konuşma tanıma**, **metinden konuşmaya** ve **çeviri** için şu bölgelerde kullanılabilir:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

[Konuşma SDK 'sını](speech-sdk.md)kullanıyorsanız, bölgeler **bölge tanımlayıcısı** tarafından belirtilir (örneğin, öğesine parametresi olarak `SpeechConfig.FromSubscription` ). Bölgenin aboneliğinizin bölgesiyle aynı olduğundan emin olun.

Özel bir modeli ses verileriyle eğitme planlıyorsanız, daha hızlı eğitim için [adanmış donanım olan bölgelerden](custom-speech-overview.md#set-up-your-azure-account) birini kullanın. Tam eğitilen modeli daha sonra başka bir bölgeye kopyalamak için [REST API](https://centralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/CopyModelToSubscription) kullanabilirsiniz.

### <a name="intent-recognition"></a>Amaç tanıma

Konuşma SDK 'Sı aracılığıyla **Amaç tanıma** için kullanılabilir bölgeler şunlardır:

| Genel bölge | Region           | Bölge tanımlayıcısı |
| ------------- | ---------------- | -------------------- |
| Asya          | Doğu Asya        | `eastasia`           |
| Asya          | Güneydoğu Asya   | `southeastasia`      |
| Avustralya     | Doğu Avustralya   | `australiaeast`      |
| Avrupa        | Kuzey Avrupa     | `northeurope`        |
| Avrupa        | West Europe      | `westeurope`         |
| Kuzey Amerika | Doğu ABD          | `eastus`             |
| Kuzey Amerika | Doğu ABD 2        | `eastus2`            |
| Kuzey Amerika | Orta Güney ABD | `southcentralus`     |
| Kuzey Amerika | Orta Batı ABD  | `westcentralus`      |
| Kuzey Amerika | Batı ABD          | `westus`             |
| Kuzey Amerika | Batı ABD 2        | `westus2`            |
| Güney Amerika | Güney Brezilya     | `brazilsouth`        |

Bu, [Language Understanding hizmeti (Luo)](../luis/luis-reference-regions.md)tarafından desteklenen yayımlama bölgelerinin bir alt kümesidir.

### <a name="voice-assistants"></a>Ses yardımcıları

[Konuşma SDK 'sı](speech-sdk.md) , bu bölgelerde [doğrudan hat konuşmayı](./direct-line-speech.md) kullanarak **sesli yardımcı** yeteneklerini destekler:

| Genel bölge | Region           | Bölge tanımlayıcısı    |
| ------------- | ---------------- | -------------------- |
| Kuzey Amerika | Batı ABD          | `westus`             |
| Kuzey Amerika | Batı ABD 2        | `westus2`            |
| Kuzey Amerika | Doğu ABD          | `eastus`             |
| Kuzey Amerika | Doğu ABD 2        | `eastus2`            |
| Kuzey Amerika | Orta Batı ABD  | `westcentralus`      |
| Kuzey Amerika | Orta Güney ABD | `southcentralus`     |
| Avrupa        | West Europe      | `westeurope`         |
| Avrupa        | Kuzey Avrupa     | `northeurope`        |
| Asya          | Doğu Asya        | `eastasia`           |
| Asya          | Güneydoğu Asya   | `southeastasia`      |
| Hindistan         | Orta Hindistan    | `centralindia`       |

### <a name="speaker-recognition"></a>Konuşmacı Tanıma

Konuşmacı tanıma özelliği şu anda yalnızca bölgede kullanılabilir `westus` .

## <a name="rest-apis"></a>REST API'leri

Konuşma hizmeti, konuşmadan metne ve metinden konuşmaya istekleri için REST uç noktalarını da kullanıma sunar.

### <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Konuşmadan metne başvuru belgeleri için bkz. [konuşmayı metne dönüştürme REST API](rest-speech-to-text.md).

REST API uç noktası şu biçimdedir:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

`<REGION_IDENTIFIER>`Bu tablodaki aboneliğinizin bölgesiyle eşleşen tanımlayıcıyla değiştirin:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> Bir 4xx HTTP hatası almamak için dil parametresi URL 'ye eklenmelidir. Örneğin, Batı ABD uç noktası kullanılarak dil ABD Ingilizcesi olarak ayarlanmıştır: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US` .

### <a name="text-to-speech"></a>Metin okuma

Metinden konuşmaya başvuru belgeleri için bkz. [metin okuma REST API](rest-text-to-speech.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]