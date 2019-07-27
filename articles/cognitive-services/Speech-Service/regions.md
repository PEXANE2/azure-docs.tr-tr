---
title: Bölgeler-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti bölgeleri için başvuru.
services: cognitive-services
author: mahilleb-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: c0414277b4851891911908ba4f42e92abedc86e4
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68553214"
---
# <a name="speech-service-supported-regions"></a>Konuşma hizmeti desteklenen bölgeler

Konuşma hizmeti, uygulamanızın sesi metne dönüştürün, gizli metin okuma ve konuşma çevirisi gerçekleştirin sağlar. Hizmet, REST API'leri ve Speech SDK'sı için benzersiz uç noktaları ile birden fazla bölgede kullanılabilir.

Aboneliğiniz için bölge eşleşen uç nokta kullandığınızdan emin olun.

## <a name="speech-sdk"></a>Konuşma SDK'sı

[Konuşma SDK 'sında](speech-sdk.md), bölgeler bir dize olarak belirtilir (örneğin, için `SpeechConfig.FromSubscription` C#konuşma SDK 'sında bir parametresi olarak).

### <a name="speech-to-text-text-to-speech-and-translation"></a>Konuşmadan metne, metinden konuşmaya ve çeviri

Konuşma SDK 'Sı, **konuşma tanıma**, **metinden konuşmaya**ve **çeviri**için şu bölgelerde kullanılabilir:

  Bölge | Konuşma SDK parametresi | Konuşma özelleştirme portalı
 ------|-------|--------
 Batı ABD | `westus` | https://westus.cris.ai
 Batı ABD 2 | `westus2` | https://westus2.cris.ai
 East US | `eastus` | https://eastus.cris.ai
 Doğu ABD 2 | `eastus2` | https://eastus2.cris.ai
 Orta ABD | `centralus` | https://centralus.cris.ai
 Orta Kuzey ABD | `northcentralus` | https://northcentralus.cris.ai
 Orta Güney ABD | `southcentralus` | https://southcentralus.cris.ai
 Orta Hindistan | `centralindia` | https://centralindia.cris.ai
 Doğu Asya | `eastasia` | https://eastasia.cris.ai
 Güneydoğu Asya | `southeastasia` | https://southeastasia.cris.ai
 Japonya Doğu | `japaneast` | https://japaneast.cris.ai
 Kore Orta | `koreacentral` | https://koreacentral.cris.ai
 Avustralya Doğu | `australiaeast` | https://australiaeast.cris.ai
 Orta Kanada | `canadacentral` | https://canadacentral.cris.ai
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
 Kuzey Amerika | East US | `eastus`
 Kuzey Amerika | Doğu ABD 2 | `eastus2`
 Kuzey Amerika | Orta Güney ABD | `southcentralus`
 Kuzey Amerika | Batı Orta ABD | `westcentralus`
 Kuzey Amerika | Batı ABD | `westus`
 Kuzey Amerika | Batı ABD 2 | `westus2`
 Güney Amerika | Güney Brezilya | `brazilsouth`

Bu, [Language Understanding hizmeti (Luo)](/azure/cognitive-services/luis/luis-reference-regions)tarafından desteklenen yayımlama bölgelerinin bir alt kümesidir.

### <a name="voice-first-virtual-assistants"></a>Ses-ilk sanal yardımcılar

[Konuşma SDK 'sı](speech-sdk.md) , bu bölgelerdeki **ilk ses Sanal Yardımcısı** yeteneklerini destekler:

Bölge | Konuşma SDK parametresi
-------|---------------------
Batı ABD | `westus`
Batı ABD 2 | `westus2`
East US | `eastus`
Doğu ABD 2 | `eastus2`
Batı Avrupa | `westeurope`
Kuzey Avrupa | `northeurope`
Güneydoğu Asya | `southeastasia`

## <a name="rest-apis"></a>REST API'leri

Konuşma hizmeti de konuşma metin ve metin okuma istekleri için REST uç noktalarını kullanıma sunar.

### <a name="speech-to-text"></a>Konuşmayı Metne Dönüştürme

Konuşmadan metne başvuru belgeleri için bkz. [konuşmayı metne dönüştürme REST API](rest-speech-to-text.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Metin okuma

Metinden konuşmaya başvuru belgeleri için bkz. [metin okuma REST API](rest-text-to-speech.md).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]