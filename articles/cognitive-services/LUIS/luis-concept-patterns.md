---
title: Desenler yardım tahmini-LUSıS
titleSuffix: Azure Cognitive Services
description: Bir desen çok daha fazla konuşma sağlamadan bir amaç için daha yüksek doğruluk derecesi elde etmek sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: acbcaa7e5588c0fecf2c20751e69442e1373cbb5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563994"
---
# <a name="patterns-improve-prediction-accuracy"></a>Desenlerini tahmin doğruluğunu artırmak
Desenler, çeşitli konuşma çok benzer olduğunda doğruluğunu artırmak için tasarlanmıştır.  Bir desen çok daha fazla konuşma sağlamadan bir amaç için daha yüksek doğruluk derecesi elde etmek sağlar. 

## <a name="patterns-solve-low-intent-confidence"></a>Düşük hedefi güvenle desenleri çözün
Kuruluş şeması ile ilgili olarak çalışan bir rapor bir insan kaynakları uygulamasında göz önünde bulundurun. Bir çalışanın adı ve ilişki LUIS çalışanlar ilgili döndürür. Bir çalışan, bir yönetici adı çiğdem ve adında bir AST ekibi olan bir çalışanı göz önünde bulundurun: Michael, Rebecca ve Carl.

![Kuruluş Şeması görüntüsü](./media/luis-concept-patterns/org-chart.png)

|Konuşmalar|Tahmin hedefi|Intent puanı|
|--|--|--|
|Tom'ın alt kimdir?|GetOrgChart|.30|
|Tom alt kimdir?|GetOrgChart|.30|

LUIS, bir uygulamanın farklı uzunluktaki cümle, farklı sözcük sırasını ve hatta farklı sözcük (eş anlamlılar "alt", "manage", "rapor") ile 10 ile 20 konuşma arasında varsa, düşük güvenilirlik puanı döndürebilir. LUSıS 'in sözcük sırasının önemini anlamalarına yardımcı olmak için bir düzen oluşturun. 

Desen aşağıdaki durumlarda çözer: 

* Amaç puanı düşük
* Doğru amaç, en üst puan değildir ancak en üst puana çok yakın değildir. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Desenler hedefi garantisi değildir.
Desenlerini tahmin teknolojilerinin bir karışımını kullanın. Bir desende bir amaç için bir şablon utterance ayarı hedefi tahmin garantisi değil ancak güçlü bir sinyaldir. 

<a name="patterns-do-not-improve-entity-detection"/></a>

## <a name="patterns-do-not-improve-machine-learned-entity-detection"></a>Desenler makine tarafından öğrenilen varlık algılamayı iyileştirmez

Bir model öncelikle amaç ve rollerin tahminine yardımcı olmak için tasarlanmıştır. Model. herhangi bir varlık, serbest biçimli varlıkları ayıklamak için kullanılır. Desenler varlıkları kullanırken, bir desen makine tarafından öğrenilen bir varlığı algılamaya yardımcı olmaz.  

Birden çok kuralı tek bir düzende daraltarak, geliştirilmiş varlık tahminini görmeyi beklememeniz gerekmez. Basit varlıkların tetiklenmesi için, bu tür varlıkları eklemeniz veya liste varlıklarını kullanmanız gerekir, ancak sizin düzeniniz tetikleymeyecektir.

## <a name="patterns-use-entity-roles"></a>Varlık rolleri desenleri kullanın
İki veya daha fazla varlık desenindeki bağlamsal ilişkiliyse, varlık desenleri kullanın [rolleri](luis-concept-roles.md) varlıklarla ilgili bağlamsal bilgi ayıklamak için.  

## <a name="prediction-scores-with-and-without-patterns"></a>Desenler olmadan ve tahmin puanları
Yeterli örnek konuşma göz önünde bulundurulduğunda, LUIS tahmin olasılık desensiz imkanımız olacaktır. Desen çok konuşma sağlamaya gerek kalmadan güvenilirlik puanı artırın.  

## <a name="pattern-matching"></a>Desen eşleştirme
Bir düzeni desen içinde varlıkları ilk algılama ve ardından bir kelimelerin rest ve sözcük sırasını deseninin doğrulama göre eşleştirilir. Varlıklar, deseni eşleştirmek için bir desen için gereklidir. Desen karakter düzeyinde değil belirteci düzeyinde uygulanır. 

## <a name="pattern-syntax"></a>Desen sözdizimi
Desen, bir şablon için bir utterance sözdizimidir. Şablon, sözcük ve sözcükler yanı sıra eşleştirmek istediğiniz varlıkları ve yok saymak için istediğiniz noktalama içermelidir. Bu **değil** normal bir ifade. 

Varlıkları desenleri süslü ayraç tarafından çevrilmiş `{}`. Desenler, varlıkları ve varlıklar rolleriyle içerebilir. [Desen. any](luis-concept-entity-types.md#patternany-entity) yalnızca desenlerinde kullanılan bir varlıktır. 

Model sözdizimi aşağıdaki sözdizimini destekler:

|İşlev|Sözdizimi|İç içe geçme düzeyi|Örnek|
|--|--|--|--|
|varlık| {}-süslü ayraçlar|2|{Entity-Name} formu nerede?|
|isteğe bağlı|[]-köşeli parantezler<BR><BR>Herhangi bir isteğe bağlı ve gruplandırma birleşiminin iç içe geçme düzeylerinde 3 sınırı vardır |2|Soru işareti isteğe bağlıdır [?]|
|gruplama|()-parantez|2|-( \| b)|
|veya| \|-dikey çubuk (kanal)<br><br>Bir gruptaki dikey çubuklar (veya) üzerinde 2 sınırı vardır |-|WHERE formu ({form-adı-Short} &#x7c; {form-adı-Long} &#x7c; {form-numarası})| 
|söylenişi başlangıcı ve/veya bitişi|^-şapka işareti|-|^ söylenişi 'a başla<br>söylenişi tamamlandı ^<br>{Number} varlık ^ ile tüm söylenişi ile tam sabit değer eşleşmesi ^|

## <a name="nesting-syntax-in-patterns"></a>Desenlerdeki iç içe sözdizimi

Köşeli parantezler ile **isteğe bağlı** sözdizimi, iç içe iki düzey olabilir. Örneğin: `[[this]is] a new form` Bu örnek aşağıdaki söyleylerini sağlar: 

|İç içe isteğe bağlı söylenişi örneği|Açıklama|
|--|--|
|Bu yeni bir formdur|Düzendeki tüm kelimeleri eşleştirir|
|Yeni bir form|desenli dış isteğe bağlı sözcük ve isteğe bağlı olmayan sözcüklerden eşleşir|
|Yeni bir form|yalnızca gerekli kelimeleri eşleştirir|

**Gruplama** sözdizimi, parantez ile iç içe iki düzey olabilir. Örneğin: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )` Bu özellik üç varlığın eşleştirilmesini sağlar. 

Entity1, Origin (Seattle) ve hedef (Cairo) gibi rollere sahip bir konum ise ve varlık 2 bir liste varlığındaki bilinen bir bina adı (Redbatı-C) ise, aşağıdaki utterlikler bu düzene eşlenir:

|İç içe gruplandırma söylenişi örneği|Açıklama|
|--|--|
|Redbatı-C|Dış gruplandırma varlığıyla eşleşir|
|Seattle|iç gruplandırma varlıklarından biriyle eşleşir|
|Kahire|iç gruplandırma varlıklarından biriyle eşleşir|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>İsteğe bağlı sözdizimi olan gruplar için iç içe sınırları

**İsteğe bağlı** sözdizimine sahip **gruplama** birleşimi 3 iç içe geçme düzeyi sınırına sahiptir.

|İzin Verildi|Örnek|
|--|--|
|Evet|([(test1 &#x7c; test2)] &#x7c; test3)|
|Hayır|([([test1] &#x7c; test2)] &#x7c; test3)|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>Ya da tabanlı sözdizimi olan gruplar için iç içe sınırları

**OR-ing** sözdizimine sahip **gruplama** birleşimi 2 dikey çubuk sınırına sahiptir.

|İzin Verildi|Örnek|
|--|--|
|Evet|(test1 &#x7c; test2 &#x7c; (test3 &#x7c; test4))|
|Hayır|(test1 &#x7c; test2 &#x7c; test3 &#x7c; (test4 &#x7c; test5)) |

## <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Bir varlık için bir desen şablonu eklemek için söz dizimi
Surround gibi küme ayracı ile varlık adı deseni şablona bir varlık eklemek için `Who does {Employee} manage?`. 

|Varlık deseni|
|--|
|`Who does {Employee} manage?`|

## <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Bir varlık ve rol için bir desen şablonu eklemek için söz dizimi
Bir varlık rolü olarak gösterilir `{entity:role}` ile bir iki nokta üst üste ve ardından rol adı tarafından izlenen varlık adı. Desen şablona bir role sahip bir varlık eklemek için varlık adı ve küme ayracı ile rol adı gibi çevreleyen `Book a ticket from {Location:Origin} to {Location:Destination}`. 

|Varlık rolleriyle deseni|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

## <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Bir pattern.any deseni şablonuna eklemek için söz dizimi
Pattern.any varlık varlığın değişen uzunluktaki desen eklemenizi sağlar. Düzen şablonu ve ardından sürece pattern.any herhangi bir uzunlukta olabilir. 

Eklemek için bir **Pattern.any** deseni şablon varlığa saran ve küme ayraçlarının Pattern.any varlıkla gibi `How much does {Booktitle} cost and what format is it available in?`.  

|Pattern.any varlık deseni|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Deseninde bir kitap adları|
|--|
|Ne kadardır **kitabın çalabilir** maliyet ve hangi biçimde, kullanılabilir?|
|Ne kadardır **isteyin** maliyet ve hangi biçimde, kullanılabilir?|
|Ne kadardır **gece zamanlı Mandal, merak olay** maliyet ve hangi biçimde, kullanılabilir?| 

Book 'ın, kitap başlığının, bu modele bağlı olarak, kitap başlığının nerede bittiğini öğrendiği için, kitap başlığının kelimeleri LUO 'ya kafa karıştırıcı değildir.

## <a name="explicit-lists"></a>Açık listeler

Şunu yaparken özel duruma izin vermek için yazma API 'SI aracılığıyla [açık bir liste](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) oluşturun:

* Düzeniniz bir model içeriyor [. any](luis-concept-entity-types.md#patternany-entity)
* Bu desen söz dizimi, utterance temelinde yanlış bir varlık ayıklama olasılığının kullanılmasına izin verir. 

Örneğin, hem isteğe bağlı söz dizimi içeren bir düzeni olduğunu varsayalım `[]`ve varlık sözdizimi `{}`, birleştirilmiş bir şekilde hatalı verileri ayıklamak için.

Desen '[Bul] e-postadan hakkında {subject} [{person}]'.

Aşağıdaki konuşma içinde **konu** ve **kişi** varlık doğru ve hatalı bir şekilde ayıklanır:

|İfade|Varlık|Doğru ayıklama|
|--|--|:--:|
|Chris köpekler hakkında e-posta|Konu köpekler =<br>kişi Chris =|✔|
|ADAM La Mancha gelen e-posta|Konu man =<br>kişi La Mancha =|X|

Yukarıdaki tabloda, konu `the man from La Mancha` (kitap başlığı) olmalıdır, ancak konu isteğe bağlı kelimeyi `from`içerdiğinden, başlık yanlış tahmin edilir. 

Bu özel durumun desen için düzeltmek için ekleme `the man from la mancha` {subject} varlık kullanan bir açık listesi eşleşme olarak [açık bir listesi için API geliştirme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>İsteğe bağlı bir metin şablonu utterance olarak işaretlemek için söz dizimi
İsteğe bağlı normal ifade köşeli parantez sözdizimini kullanarak utterance metni işaretleme `[]`. İsteğe bağlı bir metin, köşeli ayraç yalnızca iki ayraçlar en fazla iç içe yerleştirebilirsiniz.

|İsteğe bağlı metin deseni|Anlamı|
|--|--|
|`[find] email about {subject} [from {person}]`|`find`ve `from {person}` isteğe bağlıdır|
|' Bana [?] yardımcı olabilir|Noktalama işareti isteğe bağlıdır|

Noktalama işaretleri (`?`, `!`, `.`) göz ardı edilmelidir ve desenlerdeki köşeli ayraç söz dizimini kullanarak bunları yok saymanız gerekir. 

## <a name="pattern-only-apps"></a>Yalnızca örüntü uygulamalar
Her bir amaç için bir desen olduğu sürece, örnekleri olmayan amaçlar içeren bir uygulama oluşturabilirsiniz. Tek bir uygulama için, bu, örnek bir varlık gerektirdiğinden, bu model makine tarafından öğrenilen varlıklar içermemelidir. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bu öğreticide desenlerini uygulama hakkında bilgi edinin](luis-tutorial-pattern.md)
