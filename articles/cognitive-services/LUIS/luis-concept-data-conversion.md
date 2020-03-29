---
title: Veri dönüştürme - LUIS
titleSuffix: Azure Cognitive Services
description: Dil Anlama (LUIS) tahminleri önce söyleyiş nasıl değiştirilebilir öğrenin
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: aeb9094db83b14af988f70485788934a7854200c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68619795"
---
# <a name="convert-data-format-of-utterances"></a>Silenlerin veri biçimini dönüştürme
LUIS, tahminden önce kullanıcı sözcüklerinin aşağıdaki dönüşümlerini sağlar"

* Bilişsel Hizmetler Konuşma hizmetini kullanarak metne [konuşma.](../Speech-Service/overview.md) 

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Metin konuşma luis ile bir entegrasyon olarak sağlanır. 

### <a name="intent-conversion-concepts"></a>Amaç dönüştürme kavramları
KONUŞMAnın LUIS'te metne dönüştürülmesi, konuşulan konuşmaları bitiş noktasına göndermenize ve LUIS tahmin yanıtı almanıza olanak tanır. Bu süreç, [Konuşma](https://docs.microsoft.com/azure/cognitive-services/Speech) hizmetinin LUIS ile bütünleştirilmesidir. Bir [öğreticiyle](../speech-service/how-to-recognize-intents-from-speech-csharp.md)Niyete Konuşma hakkında daha fazla bilgi edinin.

### <a name="key-requirements"></a>Önemli gereksinimler
Bu tümleştirme için bir **Bing Konuşma API** anahtarı oluşturmanız gerekmez. Azure portalında oluşturulan **Bir Dil Anlama** anahtarı bu tümleştirme için çalışır. LUIS başlangıç anahtarını kullanmayın.

### <a name="pricing-tier"></a>Fiyatlandırma Katmanı
Bu tümleştirme, normal Dil Bilgisi fiyatlandırma katmanlarından farklı bir [fiyatlandırma](luis-boundaries.md#key-limits) modeli kullanır. 

### <a name="quota-usage"></a>Kota kullanımı
Bilgi için [Anahtar sınırlarına](luis-boundaries.md#key-limits) bakın. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Verileri ayıklama](luis-concept-data-extraction.md)

