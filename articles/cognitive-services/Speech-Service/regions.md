---
title: Bölgeler-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetinin bölgeleri için başvuru.
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 469dab093ed7a62171d232695af3258cc874b5f3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73481310"
---
# <a name="speech-service-supported-regions"></a>Konuşma hizmeti tarafından desteklenen bölgeler

Konuşma hizmeti, uygulamanızın sesi metne dönüştürmesini, konuşma çevirisi gerçekleştirmesini ve metni konuşmaya dönüştürmeyi sağlar. Hizmet, konuşma SDK 'Sı ve REST API 'Leri için benzersiz uç noktalara sahip birden çok bölgede kullanılabilir.

Aboneliğinizin bölgesiyle eşleşen uç noktayı kullandığınızdan emin olun.

## <a name="speech-sdk"></a>Konuşma SDK'sı

[Konuşma SDK 'sında](speech-sdk.md), bölgeler bir dize olarak belirtilir (örneğin, için C#konuşma SDK 'sında `SpeechConfig.FromSubscription` parametresi olarak).

### <a name="speech-to-text-text-to-speech-and-translation"></a>Konuşmadan metne, metinden konuşmaya ve çeviri

Konuşma SDK 'Sı, **konuşma tanıma**, **metinden konuşmaya**ve **çeviri**için şu bölgelerde kullanılabilir:

  Bölge | Konuşma SDK parametresi | Konuşma özelleştirme portalı
 ------|-------|--------
 Batı ABD | `westus` | https://westus.cris.ai
 Batı ABD 2 | `westus2` | https://westus2.cris.ai
 Doğu ABD | `eastus` | https://eastus.cris.ai
 Doğu ABD 2 | `eastus2` | https://eastus2.cris.ai
 Orta ABD | `centralus` | https://centralus.cris.ai
 Orta Kuzey ABD | `northcentralus` | https://northcentralus.cris.ai
 Orta Güney ABD | `southcentralus` | https://southcentralus.cris.ai
 Orta Hindistan | `centralindia` | https://centralindia.cris.ai
 Doğu Asya | `eastasia` | https://eastasia.cris.ai
 Güneydoğu Asya | `southeastasia` | https://southeastasia.cris.ai
 Doğu Japonya | `japaneast` | https://japaneast.cris.ai
 Kore Orta | `koreacentral` | https://koreacentral.cris.ai
 Avustralya Doğu | `australiaeast` | https://australiaeast.cris.ai
 Kanada Orta | `canadacentral` | https://canadacentral.cris.ai
 Kuzey Avrupa | `northeurope` | https://northeurope.cris.ai
 Batı Avrupa | `westeurope` | https://westeurope.cris.ai
 Birleşik Krallık Güney | `uksouth` | https://uksouth.cris.ai
 Fransa Orta | `francecentral` | https://francecentral.cris.ai

### <a name="intent-recognition"></a>Amaç tanıma

Konuşma SDK 'Sı aracılığıyla **Amaç tanıma** için kullanılabilir bölgeler şunlardır:

 Genel bölge | Bölge | Konuşma SDK parametresi
 ------|-------|--------
 Asya | Doğu Asya | `eastasia`
 Asya | Güneydoğu Asya | `southeastasia`
 Avustralya | Avustralya Doğu | `australiaeast`
 Avrupa | Kuzey Avrupa | `northeurope`
 Avrupa | Batı Avrupa | `westeurope`
 Kuzey Amerika | Doğu ABD | `eastus`
 Kuzey Amerika | Doğu ABD 2 | `eastus2`
 Kuzey Amerika | Orta Güney ABD | `southcentralus`
 Kuzey Amerika | Batı Orta ABD | `westcentralus`
 Kuzey Amerika | Batı ABD | `westus`
 Kuzey Amerika | Batı ABD 2 | `westus2`
 Güney Amerika | Güney Brezilya | `brazilsouth`

Bu, [Language Understanding hizmeti (Luo)](/azure/cognitive-services/luis/luis-reference-regions)tarafından desteklenen yayımlama bölgelerinin bir alt kümesidir.

### <a name="voice-assistants"></a>Ses yardımcıları

[Konuşma SDK 'sı](speech-sdk.md) bu bölgelerde **sesli yardımcı** yeteneklerini destekler:

Bölge | Konuşma SDK parametresi
-------|---------------------
Batı ABD | `westus`
Batı ABD 2 | `westus2`
Doğu ABD | `eastus`
Doğu ABD 2 | `eastus2`
Batı Avrupa | `westeurope`
Kuzey Avrupa | `northeurope`
Güneydoğu Asya | `southeastasia`

## <a name="rest-apis"></a>REST API'leri

Konuşma hizmeti, konuşmadan metne ve metinden konuşmaya istekleri için REST uç noktalarını da kullanıma sunar.

### <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Konuşmadan metne başvuru belgeleri için bkz. [konuşmayı metne dönüştürme REST API](rest-speech-to-text.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Metin okuma

Metinden konuşmaya başvuru belgeleri için bkz. [metin okuma REST API](rest-text-to-speech.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]