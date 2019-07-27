---
title: Veri dönüştürme-LUSıS
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) Öngörüler önce Konuşma nasıl değiştirilebilir öğrenin
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: diberry
ms.openlocfilehash: bdce1a49ce6c6531ce344de5aa157717fe72c609
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560803"
---
# <a name="convert-data-format-of-utterances"></a>Konuşma veri biçimini Dönüştür
LUIS, Bilişsel hizmetler konuşma hizmeti konuşma metin konuşma tahmin önce konuşulan konuşma dönüştürmek için kullanır. 

## <a name="speech-to-intent-conversion-concepts"></a>Konuşma niyetini dönüştürme kavramları
LUIS metinde konuşma dönüştürülmesi konuşulan konuşma bir uç noktasına göndermesi ve LUIS tahmini yanıt almanızı sağlar. Bir tümleştirme işlemidir [konuşma](https://docs.microsoft.com/azure/cognitive-services/Speech) LUIS ile hizmet. Bir [öğreticide](../speech-service/how-to-recognize-intents-from-speech-csharp.md)konuşma amacı hakkında daha fazla bilgi edinin.

### <a name="key-requirements"></a>Önemli gereksinimler
Oluşturma gerekmez bir **Bing konuşma API'si** Bu tümleştirme için anahtar. A **Language Understanding** Azure portalında oluşturulan anahtarı bu tümleştirme için çalışır. Ludo başlangıç anahtarını kullanmayın.

### <a name="pricing-tier"></a>Fiyatlandırma Katmanı
Bu tümleştirme, her zamanki Language Understanding fiyatlandırma katmanlarından farklı bir [fiyatlandırma](luis-boundaries.md#key-limits) modeli kullanır. 

### <a name="quota-usage"></a>Kota kullanımı
Bkz: [anahtar sınırları](luis-boundaries.md#key-limits) bilgi. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Veri ayıklanıyor](luis-concept-data-extraction.md)

