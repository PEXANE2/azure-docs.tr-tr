---
title: Önceden oluşturulmuş modeller-LUSıS
titleSuffix: Azure Cognitive Services
description: Önceden oluşturulmuş modeller, etki alanları, amaçlar, utbotslar ve varlıklar sağlar. Uygulamanızı önceden oluşturulmuş bir etki alanıyla başlatabilir veya uygulamanıza daha sonra ilgili bir etki alanı ekleyebilirsiniz.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: b88801ded3dea7c7514ff117361feba3e95444ed
ms.sourcegitcommit: b4665f444dcafccd74415fb6cc3d3b65746a1a31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72264394"
---
# <a name="prebuilt-domain-intent-and-entity-models"></a>Önceden oluşturulmuş etki alanı, amaç ve varlık modelleri

Önceden oluşturulmuş modeller, etki alanları, amaçlar, utbotslar ve varlıklar sağlar. Uygulamanızı önceden oluşturulmuş bir etki alanıyla başlatabilir veya uygulamanıza daha sonra ilgili bir etki alanı ekleyebilirsiniz. 

## <a name="types-of-prebuilt-models"></a>Önceden oluşturulmuş modellerin türleri

LUSıS 'in sağladığı 3 tür önceden oluşturulmuş model vardır. Her bir model uygulamanıza dilediğiniz zaman eklenebilir. 

|Model türü|Şunları İçerir|
|--|--|
|Domain|Amaçlar, utterslar, varlıklar|
|Hedefler|Amaçlar, utbotslar|
|Varlıklar|Yalnızca varlıklar| 

## <a name="prebuilt-domains"></a>Önceden oluşturulmuş etki alanları

Language Understanding (LUU), etki alanları veya ortak istemci uygulamaları kategorileri için birlikte çalışan önceden oluşturulmuş [Amaç](luis-how-to-add-intents.md) ve [varlık](luis-concept-entity-types.md) kümeleri olan *önceden oluşturulmuş etki alanları*sağlar. 

Önceden oluşturulmuş etki alanları, LUSıS Uygulamanıza eklenmeye hazır hale gelir. Önceden oluşturulmuş bir etki alanındaki amaçlar ve varlıklar, uygulamanıza eklendikten sonra tamamen özelleştirilebilir. 

Önceden oluşturulmuş bir etki alanının tamamını özelleştirmeden başlatırsanız, uygulamanızın kullanması gerekmeyen amaçları ve varlıkları silin. Önceden oluşturulan etki alanının zaten sağladığı kümesine bazı amaçlar veya varlıklar da ekleyebilirsiniz. Örneğin, bir spor olay uygulaması için önceden oluşturulmuş **olayları** kullanıyorsanız, spor ekipleri için varlıklar ekleyebilirsiniz. LUSıS 'e yönelik [utterlerin sağlamaya](luis-how-to-add-example-utterances.md) başladığınızda, uygulamanıza özgü terimleri dahil edin. LU, bunları tanımak ve önceden oluşturulan etki alanının amaçlarını ve varlıklarını uygulamanızın ihtiyaçlarına göre uyarlar. 

> [!TIP]
> Önceden oluşturulmuş bir etki alanındaki amaçlar ve varlıklar en iyi şekilde çalışır. Mümkün olduğunda aynı etki alanındaki amaçları ve varlıkları birleştirmek daha iyidir.
> Yardımcı programlar önceden oluşturulmuş etki alanı, herhangi bir etki alanında kullanım için özelleştirebileceğiniz amaçlar içerir. Örneğin, uygulamanıza `Utilities.Repeat` ekleyebilir ve Kullanıcı uygulamanızda yinelemek isteyebileceğiniz herhangi bir eylemi tanımasını sağlayabilirsiniz. 

### <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Önceden oluşturulmuş bir etki alanı hedefinin davranışını değiştirme

Önceden oluşturulmuş bir etki alanının, LUIN uygulamanızda olmasını istediğiniz bir amaca benzer bir amaç içerdiğini, ancak farklı davranması gerektiğini fark edebilirsiniz. Örneğin, önceden oluşturulmuş olan etki **alanı, Restoran** rezervasyonu yapmak için `MakeReservation` amacını sağlar, ancak uygulamanızın otel rezervasyonları yapmak için bu amacı kullanmasını istersiniz. Bu durumda, VASIS 'e `MakeReservation` hedefini kullanarak otel rezervasyonları yapıp bunları etiketleyerek bu amaca yönelik davranışı değiştirebilirsiniz. bu nedenle, bir otelin kitap oluşturma isteğindeki `MakeReservation` amacını tanımak için LUDA geri alınabilir.

Önceden [oluşturulmuş etki alanları başvurusunda](./luis-reference-prebuilt-domains.md)önceden oluşturulmuş etki alanlarının tam listesini bulabilirsiniz.

## <a name="prebuilt-intents"></a>Önceden oluşturulmuş amaçlar

LUO, önceden oluşturulmuş amaçlar ve bunların utslarını sağlar. Tüm etki alanı eklenmeksizin amaçlar eklenebilir. Amaç ekleme işlemi, bir amaç ekleme ve bunların utansları. Amaç adı ve söylenişi listesinin her ikisi de değiştirilebilir.  

## <a name="prebuilt-entities"></a>Önceden oluşturulmuş varlıklar

LUO, tarihler, saatler, sayılar, ölçümler ve para birimi gibi ortak bilgi türlerini tanınması için önceden oluşturulmuş varlıklar kümesi içerir. Önceden oluşturulmuş varlık desteği, LUSıS uygulamanızın kültürüne göre farklılık gösterir. LUID 'nin desteklediği önceden oluşturulmuş varlıkların tam listesi için, kültüre göre destek dahil olmak üzere, [önceden oluşturulmuş varlık başvurusuna](./luis-reference-prebuilt-entities.md)bakın.

Uygulamanıza önceden oluşturulmuş bir varlık dahil edildiğinde, tahminleri yayımlanmış uygulamanıza dahil edilir. Önceden oluşturulmuş varlıkların davranışı önceden eğitilmiş **ve değiştirilemez** . 

> [!NOTE]
> **Builtin. DateTime** kullanım dışıdır. Tarih ve saat aralıklarının tanınabilmesi ve belirsiz tarih ve saatlerin daha iyi tanınmasını sağlayan [**Builtin. datetimeV2**](luis-reference-prebuilt-datetimev2.md)ile değiştirilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanıza [önceden oluşturulmuş varlıklar eklemeyi](luis-prebuilt-entities.md) öğrenin.
