---
title: Varlıklar için roller-LUSıS
titleSuffix: Azure Cognitive Services
description: Adlandırılmış ve bağlamsal subtypes yalnızca desenlerinde kullanılan bir varlığın rolleridir. Örneğin utterance içinde `buy a ticket from New York to London`, New York hem London olan şehirleri ancak her bir tümce içinde farklı bir anlama sahiptir. New York kaynak Şehir ve Londra hedef şehir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: b4bd61ea74055a04718d8a9d8d5ccd42671af2ac
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638342"
---
# <a name="entity-roles-for-contextual-subtypes"></a>Bağlamsal alt türler için varlık rolleri

Roller, varlıkların adlandırılmış alt türleri olmasını sağlar. Bir rol, önceden oluşturulmuş veya özel varlık türü ile kullanılabilir ve her iki örnek de ve desenlerinde kullanılabilir. 

<a name="example-role-for-entities"></a>
<a name="roles-with-prebuilt-entities"></a>

## <a name="machine-learned-entity-example-of-roles"></a>Makinenin makine tarafından öğrenilen varlık örneği

" **New York** 'dan **Londra**'ya bilet satın alma" başlığı altında, hem New York hem de Londra şehirlerdir, ancak her birinin cümlede farklı bir anlamı vardır. New York kaynak Şehir ve Londra hedef şehir. 

```
buy a ticket from New York to London
```

Rolleri, bu fark için bir ad verin:

|Varlık türü|Varlık adı|Role|Amaç|
|--|--|--|--|
|Basit|Location|kaynak|Burada gelen düzlemi bırakır.|
|Basit|Location|Hedef|Uçağın nerede gölünüzdeki|

## <a name="non-machine-learned-entity-example-of-roles"></a>Makinenin öğrenilen varlık olmayan varlık örneği

"Toplantıyı 8 ' den 9 ' a kadar zamanla", her iki sayı de bir zaman gösterir, ancak her seferinde utterde farklı bir anlamı vardır. Roller, farkların adını sağlar. 

```
Schedule the meeting from 8 to 9
```

|Varlık türü|Rol adı|Value|
|--|--|--|
|Önceden oluşturulmuş datetimeV2|StartTime|8|
|Önceden oluşturulmuş datetimeV2|EndTime|9|

## <a name="are-multiple-entities-in-an-utterance-the-same-thing-as-roles"></a>Birden çok varlık, rollerle aynı şeyi gerçekleştirsin mi? 

Birden çok varlık bir utterlik içinde bulunabilir ve roller kullanılmadan ayıklanabilir. Tümce bağlamı, varlığın hangi sürümünün bir değere sahip olduğunu gösteriyorsa, bir rolün kullanılması gerekir. 

### <a name="dont-use-roles-for-duplicates-without-meaning"></a>Bir anlamı olmayan yinelemeler için roller kullanmayın

Söylenişi bir konum `I want to travel to Seattle, Cairo, and London.`listesi içeriyorsa, bu, her öğenin ek anlamı olmadığı bir listesidir. 

### <a name="use-roles-if-duplicates-indicate-meaning"></a>Yinelemeler anlamı gösteriyorsa rolleri kullanın

Söylenişi anlamı `I want to travel from Seattle, with a layover in Londen, landing in Cairo.`olan konumların bir listesini içeriyorsa, Origin, layover ve Destination 'in bu anlamı rollerle yakalanmalıdır.

### <a name="roles-can-indicate-order"></a>Roller sırayı belirtebilir

Söylenişi, ayıklamak `I want to first start with Seattle, second London, then third Cairo`istediğiniz sırayı belirtmek için değiştiyse, birkaç yolla ayıklayabilirsiniz. ,,, Rolünü `first start with` `second`gösteren belirteçleri etiketlendirebilirsiniz. `third` Ayrıca, sipariş ve yerleştirme fikrini yakalamak için önceden oluşturulmuş varlık **sıra sayısını** ve bileşik bir varlıkta **GeographyV2** önceden oluşturulmuş varlığı de kullanabilirsiniz. 

## <a name="how-are-roles-used-in-example-utterances"></a>Rol örnek olarak nasıl kullanılır?

Bir varlık bir role sahip olduğunda ve varlık örnek bir şekilde işaretlenmişse, yalnızca varlığı seçme veya varlığı ve rolü seçme seçeneğiniz vardır. 

Aşağıdaki örnek, varlıkları ve rolleri kullanır:

|Belirteç görünümü|Varlık görünümü|
|--|--|
|**Seattle** hakkında daha fazla bilgi edinmek için ilginç mıyım|{Location} hakkında daha fazla bilgi edinmek istiyorum|
|Seattle 'dan New York 'a bilet satın alın|{Location: Origin} öğesinden {location: Destination} öğesine bir bilet satın alın|

## <a name="how-are-roles-used-in-patterns"></a>Rolleri, modelleri nasıl kullanılır?
Bir desenin şablon utterance içinde rolleri içinde utterance kullanılır: 

|Varlık rolleriyle deseni|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Rol sözdizimi desenleri
Varlık ve rol parantez içinde içine alınmış `{}`. Varlık ve rol virgül ile ayrılır. 

## <a name="entity-roles-versus-collaborator-roles"></a>Varlık rolleri ve ortak çalışan rolleri

Varlık rolleri, LUSıS uygulamasının veri modeli için geçerlidir. [Ortak](luis-concept-collaborator.md) çalışan rolleri, yazma erişimi düzeyleri için geçerlidir. 

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Makine tarafından öğrenilen varlıklarla varlık rollerini kullanarak [uygulamalı bir öğretici](tutorial-entity-roles.md) kullanın
* Eklemeyi öğrenin [rolleri](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity)
