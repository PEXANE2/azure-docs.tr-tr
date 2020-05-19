---
title: Model sözdizimi başvurusu-LUSıS
description: Language Understanding (LUSıS) uygulamalarındaki Kullanıcı dıklarından anahtar verileri ayıklamak için varlıklar oluşturun. Ayıklanan veriler, istemci uygulaması tarafından kullanılır.
ms.topic: reference
ms.date: 04/14/2020
ms.author: diberry
ms.openlocfilehash: 1665f1ef8a868b011e9e4de8562aeda9edef5ce2
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585581"
---
# <a name="pattern-syntax"></a>Desen söz dizimi

Desen sözdizimi, utterance için bir şablondur. Şablon, eşleştirmek istediğiniz sözcükler ve varlıkların yanı sıra yoksaymak istediğiniz sözcüklerin ve [noktalama işaretlerini](luis-reference-application-settings.md#punctuation-normalization) içermelidir. Normal bir ifade **değil** .

> [!CAUTION]
> Desenler, alt varlıklar değil yalnızca makine tarafından öğrenilen varlık üst öğelerini içerir.

Desenlerdeki varlıklar, küme ayraçları ile çevrelenmiş `{}` . Desenler varlıkları ve rolleri olan varlıkları içerebilir. [Desen. any](luis-concept-entity-types.md#patternany-entity) yalnızca desenlerinde kullanılan bir varlıktır.

Model sözdizimi aşağıdaki sözdizimini destekler:

|İşlev|Söz dizimi|İç içe geçme düzeyi|Örnek|
|--|--|--|--|
|varlık| {}-süslü ayraçlar|2|{Entity-Name} formu nerede?|
|isteğe bağlı|[]-köşeli parantezler<BR><BR>Herhangi bir isteğe bağlı ve gruplandırma birleşiminin iç içe geçme düzeylerinde 3 sınırı vardır |2|Soru işareti isteğe bağlıdır [?]|
|gruplandırma|()-parantez|2|-( \| b)|
|veya| \|-dikey çubuk (kanal)<br><br>Bir gruptaki dikey çubuklar (veya) üzerinde 2 sınırı vardır |-|Burada form ({form-adı-Short} &#x7c; {form-adı-Long} &#x7c; {form-Number})|
|söylenişi başlangıcı ve/veya bitişi|^-şapka işareti|-|^ söylenişi 'a başla<br>söylenişi tamamlandı ^<br>{Number} varlık ^ ile tüm söylenişi ile tam sabit değer eşleşmesi ^|

## <a name="nesting-syntax-in-patterns"></a>Desenlerdeki iç içe sözdizimi

Köşeli parantezler ile **isteğe bağlı** sözdizimi, iç içe iki düzey olabilir. Örneğin: `[[this]is] a new form`. Bu örnek aşağıdaki söyleylerini sağlar:

|İç içe isteğe bağlı söylenişi örneği|Açıklama|
|--|--|
|Bu yeni bir formdur|Düzendeki tüm kelimeleri eşleştirir|
|Yeni bir form|desenli dış isteğe bağlı sözcük ve isteğe bağlı olmayan sözcüklerden eşleşir|
|Yeni bir form|yalnızca gerekli kelimeleri eşleştirir|

**Gruplama** sözdizimi, parantez ile iç içe iki düzey olabilir. Örneğin: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )`. Bu özellik üç varlığın eşleştirilmesini sağlar.

Entity1, Origin (Seattle) ve hedef (Cairo) gibi rollere sahip bir konum ise ve varlık 2 bir liste varlığındaki bilinen bir bina adı (Redbatı-C) ise, aşağıdaki utterlikler bu düzene eşlenir:

|İç içe gruplandırma söylenişi örneği|Açıklama|
|--|--|
|Redbatı-C|Dış gruplandırma varlığıyla eşleşir|
|Seattle|iç gruplandırma varlıklarından biriyle eşleşir|
|Cairo|iç gruplandırma varlıklarından biriyle eşleşir|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>İsteğe bağlı sözdizimi olan gruplar için iç içe sınırları

**İsteğe bağlı** sözdizimine sahip **gruplama** birleşimi 3 iç içe geçme düzeyi sınırına sahiptir.

|İzin Verildi|Örnek|
|--|--|
|Yes|([(test1 &#x7c; test2)] &#x7c; test3)|
|Hayır|([([test1] &#x7c; test2)] &#x7c; test3)|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>Ya da tabanlı sözdizimi olan gruplar için iç içe sınırları

**OR-ing** sözdizimine sahip **gruplama** birleşimi 2 dikey çubuk sınırına sahiptir.

|İzin Verildi|Örnek|
|--|--|
|Yes|(test1 &#x7c; test2 &#x7c; (test3 &#x7c; test4))|
|Hayır|(test1 &#x7c; test2 &#x7c; test3 &#x7c; (test4 &#x7c; test5)) |

## <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Bir model şablonuna varlık eklemek için sözdizimi
Model şablonuna bir varlık eklemek için, varlık adını gibi küme ayraçları ile çevreleyin `Who does {Employee} manage?` .

|Varlıkla birlikte|
|--|
|`Who does {Employee} manage?`|

## <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Bir model şablonuna varlık ve rol eklemek için sözdizimi
Bir varlık rolü, `{entity:role}` varlık adının ardından iki nokta üst üste, ardından rol adı ile birlikte gösterilir. Model şablonuna bir rol içeren bir varlık eklemek için varlık adı ve rol adını, gibi küme ayraçları ile çevreleyin `Book a ticket from {Location:Origin} to {Location:Destination}` .

|Varlık rollerine sahip model|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

## <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Bir model eklemek için sözdizimi. herhangi bir model şablonu
Model. herhangi bir varlık, düzene farklı uzunlukta bir varlık eklemenize olanak tanır. Model şablonu izlenmedikçe, model. any herhangi bir uzunlukta olabilir.

Bir **örüntü. herhangi** bir varlık, model eklemek için, model çevreleyin. gibi küme ayraçları olan tüm `How much does {Booktitle} cost and what format is it available in?` varlıklar.

|Desenli model. any varlığı|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Düzendeki kitap başlıkları|
|--|
|**Bu kitabın** maliyeti ne kadar sürer ve hangi biçimi kullanabilir?|
|Ne kadar çok **sorun** var ve hangi biçimi kullanabilir?|
|Ne kadar büyük **bir köpek, gece ve ne zaman maliyetinde bir Köpeyı** merak ediyor?|

Book 'ın, kitap başlığının, bu modele bağlı olarak, kitap başlığının nerede bittiğini öğrendiği için, kitap başlığının kelimeleri LUO 'ya kafa karıştırıcı değildir.

## <a name="explicit-lists"></a>Açık listeler

Şunu yaparken özel duruma izin vermek için yazma API 'SI aracılığıyla [açık bir liste](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) oluşturun:

* Düzeniniz bir model içeriyor [. any](luis-concept-entity-types.md#patternany-entity)
* Bu desen söz dizimi, utterance temelinde yanlış bir varlık ayıklama olasılığının kullanılmasına izin verir.

Örneğin, hem isteğe bağlı sözdizimi, hem de varlık söz dizimi içeren bir örüntükiz olduğunu `[]` ve `{}` verileri hatalı bir şekilde ayıkladığınızı varsayalım.

[{Person}] ' kaynağından {Subject} [hakkında [bul] e-postasını göz önünde bulundurun.

Aşağıdaki söyleyenlerdeki **Konu** ve **kişi** varlığı doğru ve yanlış bir şekilde ayıklanır:

|İfade|Varlık|Doğru ayıklama|
|--|--|:--:|
|Chris 'ten gelen köpekler hakkında e-posta|Konu = köpekler<br>kişi = Chris|✔|
|La Mancha 'den adam hakkında e-posta|Konu = Man<br>kişi = La Mancha|X|

Yukarıdaki tabloda, konu `the man from La Mancha` (kitap başlığı) olmalıdır, ancak konu isteğe bağlı kelimeyi içerdiğinden `from` , başlık yanlış tahmin edilir.

Bu özel durumu düzende onarmak için, `the man from la mancha` [Açık liste IÇIN yazma API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8)'sini kullanarak {Subject} varlığı için açık bir liste eşleşmesi olarak ekleyin.

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Bir şablonda isteğe bağlı metni işaretlemek için sözdizimi
Normal ifade köşeli ayraç söz dizimini kullanarak el ile, yazı içinde isteğe bağlı metni işaretleyin `[]` . İsteğe bağlı metin, köşeli ayraçları yalnızca iki köşeli ayraç içine alabilir.

|İsteğe bağlı metin içeren desenler|Anlamı|
|--|--|
|`[find] email about {subject} [from {person}]`|`find`ve `from {person}` isteğe bağlıdır|
|' Bana [?] yardımcı olabilir|Noktalama işareti isteğe bağlıdır|

Noktalama işaretleri ( `?` , `!` , `.` ) göz ardı edilmelidir ve desenlerdeki köşeli ayraç söz dizimini kullanarak bunları yok saymanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Desenler hakkında daha fazla bilgi edinin:

* [Desenler ekleme](luis-how-to-model-intent-pattern.md)
* [Nasıl eklenir. herhangi bir varlık](luis-how-to-add-entities.md#add-a-patternany-entity)
* [Desenler kavramları](luis-concept-patterns.md)

. JSON yanıtında [yaklaşım nasıl döndürüleceğini](luis-reference-prebuilt-sentiment.md) anlayın.