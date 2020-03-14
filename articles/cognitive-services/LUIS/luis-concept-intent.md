---
title: Amaçlar ve varlıklar-LUSıS
titleSuffix: Azure Cognitive Services
description: Tek bir amaç, kullanıcının gerçekleştirmek istediği bir görevi veya eylemi temsil eder. Bir amaç veya hedef kullanıcının utterance ifade edilen olduğundan. Kullanıcıların uygulamanızda almak istediğiniz eylemlerine karşılık gelen bir ıntents kümesi tanımlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: diberry
ms.openlocfilehash: 309a2592dbac2918aeb532fbe91e33d296f4e5a5
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79220979"
---
# <a name="intents-in-your-luis-app"></a>LUSıS uygulamanızdaki amaçlar

Kullanıcı eylem gerçekleştirmek istediği ya da bir amacı bir görevi gösterir. Bu, kullanıcının [utterliği](luis-concept-utterance.md)içinde ifade edilen bir amaç veya hedeftir.

Kullanıcıların uygulamanızda almak istediğiniz eylemlerine karşılık gelen bir ıntents kümesi tanımlar. Örneğin, bir seyahat uygulaması, çeşitli hedefleri tanımlar:

Uygulama amaçları seyahat   |   Örnek konuşmalar   | 
------|------|
 BookFlight     |   "Bana bir uçuş RIO için sonraki hafta kitap" <br/> "Benim için RIO üzerinde 24 uçarak" <br/> "Uçak bileti Rio de Janeiro sonraki Sunday ihtiyacım"    |
 Karşılama     |   "Hi" <br/>"Hello" <br/>"Günaydın"  |
 CheckWeather | "Gibi de Boston hava nedir?" <br/> "Bu hafta için hava durumu tahminini Göster" |
 Yok         | "Bana bir tanımlama bilgisi tarif Al"<br>"Lakers win?" |

Tüm uygulamalar, geri dönüş amacı olan önceden tanımlanmış "[none](#none-intent)" hedefi ile gelir. 

## <a name="prebuilt-domains-provide-intents"></a>Önceden oluşturulmuş etki alanları hedefleri belirtin
Tanımladığınız amaçlardan ek olarak, önceden oluşturulan hedefleri [önceden oluşturulmuş etki alanlarından](luis-how-to-use-prebuilt-domains.md)birini kullanarak kullanabilirsiniz. 

## <a name="return-all-intents-scores"></a>Tüm hedefleri puanlarını döndürür
Tek bir hedefi için bir utterance atarsınız. Luara, uç noktada bir söylenişi aldığında, varsayılan olarak o zaman için en üst amacı döndürür. 

Utterance için tüm amaçlar için puanları istiyorsanız, tahmin API 'sinin sorgu dizesinde bir bayrak sağlayabilirsiniz. 

|Tahmin API sürümü|Bayrağı|
|--|--|
|V2|`verbose=true`|
|V3|`show-all-intents=true`|

## <a name="intent-compared-to-entity"></a>Varlığa karşılaştırıldığında hedefi
Amaç, uygulamanın kullanıcı için yapması gereken eylemi temsil eder ve tüm söylik 'i temel alır. Bir utterance hedefi Puanlama yalnızca bir üste sahip olabilir ancak birçok varlık sahip olabilir. 

<a name="how-do-intents-relate-to-entities"></a>

Kullanıcının _amacı_ , checkhava durumu () işlevine yapılan bir çağrı gibi istemci uygulamanızda bir eylem tetikleyeceğinden bir amaç oluşturun. Ardından, eylemi yürütmek için gereken parametreleri temsil eden varlıklar oluşturun. 

|Amaç   | Varlık | Örnek konuşma   | 
|------------------|------------------------------|------------------------------|
| CheckWeather | {"type": "Konum", "varlık": "seattle"}<br>{"type": "builtin.datetimeV2.date","entity": "yarın", "Çözüm": "2018-05-23"} | `Seattle` `tomorrow`Hava durumu nedir? |
| CheckWeather | {"type": "date_range", "varlık": "Bu hafta sonu"} | `this weekend` tahminini göster | 
||||

## <a name="prebuilt-domain-intents"></a>Önceden oluşturulmuş etki alanı hedefleri

[Önceden oluşturulan etki alanları](luis-how-to-use-prebuilt-domains.md) , utterlerle amaçlar sağlar. 

## <a name="none-intent"></a>Amaç yok

**Hiçbiri** amacı oluşturulur ancak boş bırakılır. **Hiçbiri** amacı gerekli bir amaç değildir ve silinemez veya yeniden adlandırılamaz. Etki alanı dışındaki bir konuşma ile doldurun.

**Hiçbiri** amacı her uygulama için önemli olan geri dönüş amacı ve toplam dikkat sayısının %10 ' u olmalıdır. LUIS uygulama etki alanında (konu alanı), önemli değildir. Konuşma öğretmek için kullanılır. **Hiçbiri** amacı için herhangi bir söylik eksiz DEĞILSENIZ, Lua, etki alanı dışındaki bir şekilde etki alanı amaçlardakilerden birine zorlar. LUIS utterance yanlış hedefini eğitiminde tarafından bu tahmin puanları eğme. 

Bir söylenişi, None amacı olarak tahmin edildiğinde, istemci uygulaması daha fazla soru sorabilir veya kullanıcıyı geçerli seçeneklere yönlendirecek bir menü sağlayabilir. 

## <a name="negative-intentions"></a>Negatif amaçları 
" **Bir otomobil istiyorum"** ve " **araba istemiyorum"** gibi negatif ve pozitif amaçları tespit etmek istiyorsanız, iki amaç oluşturabilir (bir pozitif ve bir negatif) ve her biri için uygun bir Aralık ekleyebilirsiniz. Tek bir hedefi oluşturma ve iki farklı pozitif ve negatif koşulları bir varlık olarak işaretleyin.  

## <a name="intents-and-patterns"></a>Amaçlar ve desenler

Bir normal ifade olarak kısmen veya bütün olarak tanımlanabilen örnek utsunuz varsa, bir [desen](luis-concept-patterns.md)ile eşleştirilmiş [normal ifade varlığını](luis-concept-entity-types.md#regular-expression-entity) kullanmayı düşünün. 

Bir normal ifade varlığı kullanmak, düzenin eşleşmesi için veri ayıklamasını garanti eder. Bu eşleştirme, tam bir amacı garanti eder. 

## <a name="intent-balance"></a>Intent bakiyesi
Uygulama etki alanı hedefleri konuşma dengesi her hedefi arasında olmalıdır. 10 Konuşma ile bir hedefi ve 500 Konuşma ile başka bir amacı yoktur. Bu dengeli değil. Bu durumda, amaçlarının çoğunun bir [düzende](luis-concept-patterns.md)yeniden düzenlenebilmeme olup olmadığını görmek için 500 utterler ile amacı gözden geçirin. 

**Hiçbir** amaç bakiyeye dahil değildir. Bu hedefi % uygulamasında toplam konuşma 10 içermelidir.

## <a name="intent-limits"></a>Intent sınırları
Bir modele kaç amaç ekleyebileceğiniz hakkında bilgi edinmek için [limitleri](luis-boundaries.md#model-boundaries) gözden geçirin. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>Intents en fazla sayısından fazlasını gerekiyorsa 
İlk olarak, sisteminize çok fazla ıntents kullanıp kullanmadığını göz önünde bulundurun. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Birden çok hedefleri tek amacı, varlıklarla birleştirilebilir 
Çok benzer bir ıntents LUIS bunları ayırt daha zor yapabilirsiniz. Intents olması için anın ancak alan, kodunuzun her yolunu yakalamak üzere gerekmeyen ana görevleri yakalamak için yeterli değiştirilen. Örneğin, bir seyahat uygulamasında ayrı ıntents BookFlight ve FlightCustomerService olabilir, ancak BookInternationalFlight ve BookDomesticFlight çok benzerdir. Bunları ayırt etmek sisteminizi gerekiyorsa, varlıklar veya diğer mantıksal yerine hedefleri kullanın. 

### <a name="dispatcher-model"></a>Dağıtıcı modeli
LUVE QnA Maker uygulamalarını [dağıtma modeliyle](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps)birleştirme hakkında daha fazla bilgi edinin. 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Önemli sayıda hedefleri olan uygulamalar için yardım iste
Intents sayısını azaltmayı ya da birden çok uygulamalarda, hedefleri bölme sizin için işe yaramazsa, desteğe başvurun. Azure aboneliğiniz destek hizmetleri içeriyorsa, [Azure teknik desteği](https://azure.microsoft.com/support/options/)'ne başvurun. 

## <a name="next-steps"></a>Sonraki adımlar

* Amaçlar ile ilgili önemli sözcükler olan [varlıklar](luis-concept-entity-types.md)hakkında daha fazla bilgi edinin
* LUSıS uygulamanızda [amaçları ekleme ve yönetme](luis-how-to-add-intents.md) hakkında bilgi edinin.
* Hedef [en iyi uygulamaları](luis-concept-best-practices.md) gözden geçirme
