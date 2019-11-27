---
title: Önceden oluşturulmuş modeller-LUSıS
titleSuffix: Azure Cognitive Services
description: Önceden oluşturulmuş modelleri, etki alanları, amacı, konuşma ve varlıklar sağlar. Önceden oluşturulmuş bir etki alanı ile uygulamanızı başlatabilir veya ilgili bir etki alanı daha sonra uygulamanıza ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: diberry
ms.openlocfilehash: e0b5b95a7524b60a7c3367035a15a7158fa7908a
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280833"
---
# <a name="prebuilt-models"></a>Önceden oluşturulmuş modeller

Önceden oluşturulmuş modelleri, etki alanları, amacı, konuşma ve varlıklar sağlar. Uygulamanızı önceden oluşturulmuş bir modelle başlatabilir veya uygulamanıza daha sonra ilgili bir model ekleyebilirsiniz. 

## <a name="types-of-prebuilt-models"></a>Önceden oluşturulmuş modelleri türü

LUSıS, üç tür önceden oluşturulmuş model sağlar. Her model, herhangi bir zamanda uygulamanıza eklenebilir. 

|Model türü|İçerir|
|--|--|
|[Alanını](luis-reference-prebuilt-domains.md)|Hedefleri, konuşma, varlıklar|
|Hedefler|Hedefleri, konuşma|
|[Varlıklar](luis-reference-prebuilt-entities.md)|Yalnızca varlıklar| 

## <a name="prebuilt-domains"></a>Önceden oluşturulmuş etki alanları

Language Understanding (LUU), etki alanları ya da istemci uygulamalarının ortak kategorileri için birlikte çalışan, önceden eğitilen [amaçlar](luis-how-to-add-intents.md) ve [varlık](luis-concept-entity-types.md) modelleri olan önceden *oluşturulmuş etki alanları*sağlar. 

Önceden oluşturulmuş etki alanları, eğitilmiş ve LUIS uygulamanıza eklemek hazır. Önceden oluşturulmuş bir etki alanının amaçları ve varlıkları uygulamanıza eklendikten sonra tamamen özelleştirilebilir. 

> [!TIP]
> Amaç ve varlıkları önceden oluşturulmuş bir etki alanında en iyi birlikte çalışır. Hedefleri ve mümkün olduğunda aynı etki alanındaki varlık birleştirerek daha iyidir.
> Yardımcı programları önceden oluşturulmuş etki alanının herhangi bir etki alanı kullanmak için özelleştirebileceğiniz amacı vardır. Örneğin, uygulamanıza `Utilities.Repeat` ekleyebilir ve bu uygulamayı, kullanıcının uygulamanızda yinelemek isteyebileceğiniz her türlü eylemi tanımasını sağlayabilirsiniz. 

### <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Önceden oluşturulmuş etki alanına amacını davranışını değiştirme

LUIS uygulamanızı olmasını istediğiniz bir hedefi benzer bir amaç önceden oluşturulmuş bir etki alanı içerir ancak farklı davranır istediğiniz bulabilirsiniz. Örneğin, önceden oluşturulmuş olan etki **alanı, Restoran** rezervasyonu yapmak için bir `MakeReservation` amacı sağlar, ancak uygulamanızın bu amacı, otel rezervasyonları yapmak için kullanmasını istiyorsunuz. Bu durumda, otel ayırmaları yapma ve sonra uygulamayı yeniden eğitme hakkında amaç ekleyerek bu amaca ilişkin davranışı değiştirebilirsiniz. 

Önceden [oluşturulmuş etki alanları başvurusunda](./luis-reference-prebuilt-domains.md)önceden oluşturulmuş etki alanlarının tam listesini bulabilirsiniz.

## <a name="prebuilt-intents"></a>Önceden oluşturulmuş hedefleri

LUO, önceden oluşturulmuş amaçlar ve bunların kendilerine ait yerleşik etki alanlarının her biri için onlara yönelik bir işlem sağlar. Tüm etki alanına eklemeden ıntents eklenebilir. Amaç eklemek, uygulamanıza amaç ve bu nesnelerin bir hedefini ekleme işlemidir. Hedefi adı hem de utterance liste değiştirilebilir.  

## <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar

LUIS, tarihler, saatler, sayılar, Ölçümler ve para birimi gibi bilgileri genel türleri tanıma için önceden oluşturulmuş varlıklar kümesi içerir. Önceden oluşturulmuş varlık destek LUIS uygulamanızı kültüre göre değişir. LUID 'nin desteklediği önceden oluşturulmuş varlıkların tam listesi için, kültüre göre destek dahil olmak üzere, [önceden oluşturulmuş varlık başvurusuna](./luis-reference-prebuilt-entities.md)bakın.

Önceden oluşturulmuş bir varlık, uygulamanızda eklendiğinde, Öngörüler, yayımlanan uygulamayı dahil edilir. Önceden oluşturulmuş varlıkların davranışı önceden eğitilmiş **ve değiştirilemez** . 

> [!NOTE]
> **Builtin. DateTime** kullanım dışıdır. Tarih ve saat aralıklarının tanınabilmesi ve belirsiz tarih ve saatlerin daha iyi tanınmasını sağlayan [**Builtin. datetimeV2**](luis-reference-prebuilt-datetimev2.md)ile değiştirilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanıza [önceden oluşturulmuş varlıklar eklemeyi](luis-prebuilt-entities.md) öğrenin.
