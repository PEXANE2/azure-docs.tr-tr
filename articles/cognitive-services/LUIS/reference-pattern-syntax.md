---
title: Desen sözdizimi başvurusu - LUIS
titleSuffix: Azure Cognitive Services
description: Dil Anlama (LUIS) uygulamalarında kullanıcı söyleyişlerinden önemli verileri ayıklamak için varlıklar oluşturun. Çıkarılan veriler istemci uygulaması tarafından kullanılır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 12/09/2019
ms.author: diberry
ms.openlocfilehash: 696f4bdc22bed01a4b5be8bff63ade482a8dbe0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219736"
---
# <a name="pattern-syntax"></a>Desen söz dizimi

Desen sözdizimi bir sözcük için bir şablondur. Şablon, eşleştirmek istediğiniz sözcükleri ve varlıkların yanı sıra göz ardı etmek istediğiniz sözcükleri ve noktalama işaretlerini içermelidir. Bu normal bir ifade **değildir.**

> [!CAUTION]
> Desenler yalnızca makinede öğrenilen varlık ebeveynlerini içerir, alt bileşenleri değil.

Desenlerdeki varlıklar kıvırcık köşeli `{}`ayraçlarla çevrilidir. Desenler, varlıkları ve rolleri olan varlıkları içerebilir. [Desen.any](luis-concept-entity-types.md#patternany-entity) sadece desenlerde kullanılan bir varlıktır.

Desen sözdizimi aşağıdaki sözdizimini destekler:

|İşlev|Sözdizimi|İç içe geçme düzeyi|Örnek|
|--|--|--|--|
|varlık| {}- kıvırcık braketler|2|Form {entity-name} nerede?|
|isteğe bağlı|[] - kare köşeli ayraçlar<BR><BR>İsteğe bağlı ve gruplandırmanın herhangi bir kombinasyonunun iç içe geçme düzeylerinde 3'ün sınırı vardır |2|Soru işareti isteğe bağlıdır [?]|
|gruplandırma|() - parantez|2|(a \| b)|
|or| \|- dikey çubuk (boru)<br><br>Bir grupta dikey çubuklarda (Veya) 2 sınırı vardır |-|Form nerede ({form-name-kısa} &#x7c; {form-name-long} &#x7c; {form-number})|
|söyleyiş başlangıcı ve/veya sonu|^ - caret|-|^söyleyiş başlar<br>söyleyiş yapılır^<br>^tüm söyleyyiş ile {sayı} varlığı^|

## <a name="nesting-syntax-in-patterns"></a>Sözdizimini desenlere yerleştirme

Kare paranteziçeren **isteğe bağlı** sözdizimi iki düzeyiçlenebilir. Örneğin: `[[this]is] a new form`. Bu örnek, aşağıdaki ifadelere izin verir:

|İç içe isteğe bağlı söyleyiş örneği|Açıklama|
|--|--|
|Bu yeni bir formdur|desendeki tüm sözcükleri eşler|
|yeni bir formdur|desende dış isteğe bağlı sözcük ve isteğe bağlı olmayan sözcüklerle eşleşir|
|yeni bir form|sadece gerekli sözcükleri eşler|

Parantez içeren **gruplandırma** sözdizimi iki düzey iç içe olabilir. Örneğin: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )`. Bu özellik, üç varlıktan herhangi birinin eşleşmesine izin verir.

Entity1, kaynak (Seattle) ve hedef (Kahire) ve Entity 2 gibi rolleri olan bir konumsa, bir liste varlığından (RedWest-C) bilinen bir yapı adıysa, aşağıdaki sözcükler bu desenle eşleşir:

|İç içe gruplandırma söyleyiş örneği|Açıklama|
|--|--|
|RedWest-C|dış gruplandırma varlığıyla eşleşir|
|Seattle|iç gruplandırma varlıklarından biriyle eşleşir|
|Cairo|iç gruplandırma varlıklarından biriyle eşleşir|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>İsteğe bağlı sözdizimi olan gruplar için iç içe geçme sınırları

**İsteğe bağlı** sözdizimi ile **gruplandırma** nın bir birleşimi, 3 iç içe geçme düzeyi sınırına sahiptir.

|İzin Verildi|Örnek|
|--|--|
|Evet|( [ ( test1 &#x7c; test2 ) ] &#x7c; test3 )|
|Hayır|( [ [ test1 ] &#x7c; test2 ] &#x7c; test3 )|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>Veya sözdizimi olan gruplar için iç içe geçme sınırları

**Or-ing** sözdizimi ile **gruplandırma** nın birleşimi 2 dikey çubuk sınırına sahiptir.

|İzin Verildi|Örnek|
|--|--|
|Evet|( test1 &#x7c; test2 &#x7c; ( test3 &#x7c; test4 ) )|
|Hayır|( test1 &#x7c; test2 &#x7c; test3 &#x7c; ( test4 &#x7c; test5 ) |

## <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Desen şablonuna varlık eklemek için sözdizimi
Desen şablonuna bir varlık eklemek için varlık adını kıvırcık `Who does {Employee} manage?`ayraçlarla çevreleyin.

|Varlıklı desen|
|--|
|`Who does {Employee} manage?`|

## <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Desen şablonuna varlık ve rol eklemek için sözdizimi
Varlık rolü varlık adı `{entity:role}` ile bir üst üste sonra rol adı tarafından gösterilir. Desen şablonuna rolü olan bir varlık eklemek için, varlık adını ve rol `Book a ticket from {Location:Origin} to {Location:Destination}`adını kıvırcık ayraçlarla çevreleyin.

|Varlık rolleri ile desen|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

## <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Desen şablonuna desen eklemek için sözdizimi
Desen.herhangi bir varlık desene değişen uzunlukta bir varlık eklemenize izin verir. Desen şablonu izli olduğu sürece, desen.herhangi bir uzunluk olabilir.

Desen şablonuna bir **Desen.herhangi** bir varlık eklemek için Desen'i çevreleyin.herhangi bir varlığı kıvırcık ayraçlarla çevreleyin, örneğin `How much does {Booktitle} cost and what format is it available in?`.

|Desen ile Desen.herhangi bir varlık|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Desendeki kitap başlıkları|
|--|
|Ne kadar **bu kitap** maliyeti çalmak ve ne biçimde mevcuttur?|
|**Maliyet** ne kadar sorulması ve hangi biçimde kullanılabilir?|
|**Gece-Zaman** maliyeti köpek Meraklı Olay ne kadar ve ne formatta mevcuttur?|

Kitap başlığının kelimeleri LUIS için kafa karıştırıcı değildir çünkü LUIS kitap başlığının nerede bittiğini bilir.

## <a name="explicit-lists"></a>Açık listeler

aşağıdaki halde özel durumlara izin vermek için yazma API'si aracılığıyla açık bir [liste](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) oluşturun:

* Desenin bir [Desen içerir.herhangi](luis-concept-entity-types.md#patternany-entity) bir
* Ve bu desen sözdizimi, söyleyiş dayalı yanlış bir varlık çıkarma olasılığı sağlar.

Örneğin, hem isteğe bağlı sözdizimi `[]`hem de varlık `{}`sözdizimini içeren bir deseniniz olduğunu varsayalım.

{subject} [from {person}]' hakkında '[bul] e-postasını düşünün.

Aşağıdaki ifadelerde, **özne** ve **kişi** varlığı doğru ve yanlış olarak ayıklanır:

|İfade|Varlık|Doğru çıkarma|
|--|--|:--:|
|Chris köpekler hakkında e-posta|özne=köpekler<br>kişi=Chris|✔|
|La Mancha gelen adam hakkında e-posta|özne=adam<br>kişi=La Mancha|X|

Önceki tabloda, konu (bir `the man from La Mancha` kitap başlığı) olmalıdır, ancak konu `from`isteğe bağlı kelime içerdiğinden, başlık yanlış tahmin edilir.

Desenin bu özel durumu `the man from la mancha` düzeltmek [için, açık liste için yazma API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8)kullanarak {konu} varlığı için açık bir liste eşleşmesi olarak ekleyin.

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>İsteğe bağlı metni şablon sözcükte işaretlemek için sözdizimi
Normal ifade kare svesi sözdizimini kullanarak `[]`isteğe bağlı metni sözcükte işaretleyin. İsteğe bağlı metin, yalnızca iki paranteze kadar olan kare köşeli ayraçları iç içe alabilir.

|İsteğe bağlı metiniçeren desen|Anlamı|
|--|--|
|`[find] email about {subject} [from {person}]`|`find`ve `from {person}` isteğe bağlı|
|'Bana yardım edebilir misin?]|Noktalama işareti isteğe bağlıdır|

Noktalama işaretleri (`?` `!`, `.`, ) göz ardı edilmelidir ve desenler kare ayraç sözdizimi kullanarak bunları yoksaymanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Desenler hakkında daha fazla bilgi edinin:

* [Desen ekleme](luis-how-to-model-intent-pattern.md)
* [Desen ekleme nasıl.herhangi bir varlık](luis-how-to-add-entities.md#add-a-patternany-entity)
* [Örüntü Kavramları](luis-concept-patterns.md)

.json yanıtında [duyguların](luis-reference-prebuilt-sentiment.md) nasıl geri döndüğünü anlayın.