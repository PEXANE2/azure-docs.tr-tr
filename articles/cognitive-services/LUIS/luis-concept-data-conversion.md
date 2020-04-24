---
title: Veri dönüştürme-LUSıS
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUSıS) ' de tahmine göre nasıl değiştirilebileceğinizi öğrenin
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: b2455df87c8eae1a48cb6c8b1381dad85d304bf4
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82099249"
---
# <a name="convert-data-format-of-utterances"></a>Söyleyenlerdeki veri biçimlerini Dönüştür
LUO, tahmine göre bir kullanıcı için aşağıdaki dönüştürmeleri sağlar "

* Bilişsel [Hizmetler konuşma](../Speech-Service/overview.md) hizmetini kullanarak konuşmayı metne dönüştürme.

## <a name="speech-to-text"></a>Konuşmayı metne dönüştürme

Konuşmadan metne, LUSıS ile tümleştirme olarak sunulmaktadır.

### <a name="intent-conversion-concepts"></a>Amaç dönüştürme kavramları
Luo 'da konuşmayı metne dönüştürme, bir uç noktaya konuşmalar göndermenizi ve bir LUSıS tahmini yanıtı almanızı sağlar. İşlem, LUSıS ile [konuşma](https://docs.microsoft.com/azure/cognitive-services/Speech) hizmetinin bir Tümleştirmesidir. Bir [öğreticide](../speech-service/how-to-recognize-intents-from-speech-csharp.md)konuşma amacı hakkında daha fazla bilgi edinin.

### <a name="key-requirements"></a>Önemli gereksinimler
Bu tümleştirme için bir **Bing Konuşma API'si** anahtarı oluşturmanız gerekmez. Azure portal oluşturulan **Language Understanding** anahtarı bu tümleştirme için geçerlidir. Ludo başlangıç anahtarını kullanmayın.

### <a name="pricing-tier"></a>Fiyatlandırma Katmanı
Bu tümleştirme, her zamanki Language Understanding fiyatlandırma katmanlarından farklı bir [fiyatlandırma](luis-limits.md#key-limits) modeli kullanır.

### <a name="quota-usage"></a>Kota kullanımı
Bilgi için bkz. [anahtar sınırları](luis-limits.md#key-limits) .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Verileri ayıklama](luis-concept-data-extraction.md)

