---
title: Önceden yapılmış modeller - LUIS
titleSuffix: Azure Cognitive Services
description: Önceden oluşturulmuş modeller etki alanları, niyetler, söyleyişler ve varlıklar sağlar. Uygulamanızı önceden oluşturulmuş bir etki alanıyla başlatabilir veya daha sonra uygulamanıza alakalı bir etki alanı ekleyebilirsiniz.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74280833"
---
# <a name="prebuilt-models"></a>Önceden oluşturulmuş modeller

Önceden oluşturulmuş modeller etki alanları, niyetler, söyleyişler ve varlıklar sağlar. Uygulamanızı önceden oluşturulmuş bir modelle başlatabilir veya daha sonra uygulamanıza alakalı bir model ekleyebilirsiniz. 

## <a name="types-of-prebuilt-models"></a>Önceden oluşturulmuş model türleri

LUIS önceden oluşturulmuş üç model sunar. Her model herhangi bir zamanda uygulamanız için eklenebilir. 

|Model türü|Dahil Eder|
|--|--|
|[Domain](luis-reference-prebuilt-domains.md)|Niyetler, söyleyişler, varlıklar|
|Hedefler|Niyetler, söyleyişler|
|[Varlıklar](luis-reference-prebuilt-entities.md)|Yalnızca varlıklar| 

## <a name="prebuilt-domains"></a>Önceden oluşturulmuş etki alanları

Dil Anlama (LUIS), etki alanları veya ortak istemci uygulamaları kategorileri için birlikte çalışan [niyet](luis-how-to-add-intents.md) ve [varlıkların](luis-concept-entity-types.md) önceden eğitilmiş modelleri olan *önceden oluşturulmuş etki alanlarını*sağlar. 

Önceden oluşturulmuş etki alanları eğitimlidir ve LUIS uygulamanıza eklemeye hazırdır. Önceden oluşturulmuş bir etki alanının amaçları ve varlıkları, bunları uygulamanız için ekledikten sonra tamamen özelleştirilebilir. 

> [!TIP]
> Önceden oluşturulmuş bir etki alanında bulunan niyet ler ve varlıklar en iyi şekilde birlikte çalışır. Mümkün olduğunda aynı etki alanından gelen amaçları ve varlıkları birleştirmek daha iyidir.
> Yardımcı Programlar önceden oluşturulmuş etki alanı, herhangi bir etki alanında kullanılmak üzere özelleştirebileceğiniz amaçları vardır. Örneğin, uygulamanıza `Utilities.Repeat` ekleyebilir ve kullanıcının uygulamanızda yinelemek isteyebileceği eylemleri tanımasını sağlayabilirsiniz. 

### <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Önceden oluşturulmuş etki alanı amacının davranışını değiştirme

Önceden oluşturulmuş bir etki alanının, LUIS uygulamanızda olmasını istediğiniz ama farklı davranmasını istediğiniz bir amaca benzer bir niyet içerdiğini görebilirsiniz. Örneğin, Önceden oluşturulmuş **Yerler** etki `MakeReservation` alanı bir restoran rezervasyonu yapmak için bir niyet sağlar, ancak uygulamanızın bu amacı otel rezervasyonu yapmak için kullanmasını istersiniz. Bu durumda, otel rezervasyonları yapma niyetine örnek sözler ekleyerek bu niyetin davranışını değiştirebilir ve uygulamayı yeniden eğitebilirsiniz. 

[Önceden oluşturulmuş etki alanlarının](./luis-reference-prebuilt-domains.md)tam listesini önceden oluşturulmuş etki alanları referansında bulabilirsiniz.

## <a name="prebuilt-intents"></a>Önceden yapılmış niyetler

LUIS, önceden oluşturulmuş etki alanlarının her biri için önceden oluşturulmuş niyetler ve onların söyleyişlerini sağlar. Tüm etki alanı eklenmeden niyetler eklenebilir. Bir niyet eklemek, uygulamanıza bir niyet ve onun söyleyişlerini ekleme işlemidir. Hem niyet adı hem de söyleyiş listesi değiştirilebilir.  

## <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar

LUIS, tarihler, saatler, sayılar, ölçümler ve para birimi gibi yaygın bilgi türlerini tanımak için önceden oluşturulmuş varlıklar kümesi içerir. Önceden oluşturulmuş varlık desteği, LUIS uygulamanızın kültürüne göre değişir. Luis'in desteklediği, kültür desteği de dahil olmak üzere önceden oluşturulmuş varlıkların tam listesi için [önceden oluşturulmuş varlık başvurusuna](./luis-reference-prebuilt-entities.md)bakın.

Önceden oluşturulmuş bir varlık uygulamanıza dahil edildiğinde, tahminleri yayınlanmış uygulamanıza dahil edilir. Önceden oluşturulmuş varlıkların davranışı önceden eğitilmişve **değiştirilemez.** 

> [!NOTE]
> **builtin.datetime** amortismana hazırdır. Bu, tarih ve saat aralıklarının tanınmasını ve belirsiz tarih ve saatlerin daha iyi tanınmasını sağlayan [**builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md)ile değiştirilir.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanıza [önceden oluşturulmuş varlıkları](luis-prebuilt-entities.md) nasıl ekleyeceğinizi öğrenin.
