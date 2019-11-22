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
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280895"
---
# <a name="intents-in-your-luis-app"></a>LUSıS uygulamanızdaki amaçlar

Kullanıcı eylem gerçekleştirmek istediği ya da bir amacı bir görevi gösterir. Bir amaç veya hedef bir kullanıcının ifade olduğundan [utterance](luis-concept-utterance.md).

Kullanıcıların uygulamanızda almak istediğiniz eylemlerine karşılık gelen bir ıntents kümesi tanımlar. Örneğin, bir seyahat uygulaması, çeşitli hedefleri tanımlar:

Uygulama amaçları seyahat   |   Örnek konuşmalar   | 
------|------|
 BookFlight     |   "Bana bir uçuş RIO için sonraki hafta kitap" <br/> "Benim için RIO üzerinde 24 uçarak" <br/> "Uçak bileti Rio de Janeiro sonraki Sunday ihtiyacım"    |
 Karşılama     |   "Hi" <br/>"Hello" <br/>"Günaydın"  |
 CheckWeather | "Gibi de Boston hava nedir?" <br/> "Bu hafta için hava durumu tahminini Göster" |
 None         | "Bana bir tanımlama bilgisi tarif Al"<br>"Lakers win?" |

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
| CheckWeather | {"type": "Konum", "varlık": "seattle"}<br>{"type": "builtin.datetimeV2.date","entity": "yarın", "Çözüm": "2018-05-23"} | Hava durumu, beğendiğiniz Özellikler `Seattle` `tomorrow`? |
| CheckWeather | {"type": "date_range", "varlık": "Bu hafta sonu"} | Tahmini Göster `this weekend` | 
||||

## <a name="prebuilt-domain-intents"></a>Önceden oluşturulmuş etki alanı hedefleri

[Önceden oluşturulan etki alanları](luis-how-to-use-prebuilt-domains.md) , utterlerle amaçlar sağlar. 

## <a name="none-intent"></a>Amaç yok

**Hiçbiri** hedefi oluşturulur, ancak boş bilerek. **Hiçbiri** amacı gerekli amacı ve silinemez veya yeniden adlandırılamaz. Etki alanı dışındaki bir konuşma ile doldurun.

**Hiçbiri** amacı her uygulama için önemli olan geri dönüş amacı ve toplam dikkat sayısının %10 ' u olmalıdır. LUIS uygulama etki alanında (konu alanı), önemli değildir. Konuşma öğretmek için kullanılır. İçin herhangi bir konuşma eklemezseniz **hiçbiri** hedefi, LUIS etki alanına bir etki alanı amacı dışında bir utterance zorlar. LUIS utterance yanlış hedefini eğitiminde tarafından bu tahmin puanları eğme. 

Bir söylenişi, None amacı olarak tahmin edildiğinde, istemci uygulaması daha fazla soru sorabilir veya kullanıcıyı geçerli seçeneklere yönlendirecek bir menü sağlayabilir. 

## <a name="negative-intentions"></a>Negatif amaçları 
Negatif ve pozitif amaçları gibi belirlemek istiyorsanız "miyim **istediğiniz** bir araba" ve "ı **yoksa** bir araba istediğiniz", iki hedefleri (bir pozitif ve negatif bir) oluşturma ve için uygun Konuşma ekleme Her. Tek bir hedefi oluşturma ve iki farklı pozitif ve negatif koşulları bir varlık olarak işaretleyin.  

## <a name="intents-and-patterns"></a>Amaçlar ve desenler

Bir normal ifade olarak kısmen veya bütün olarak tanımlanabilen örnek utsunuz varsa, bir [desen](luis-concept-patterns.md)ile eşleştirilmiş [normal ifade varlığını](luis-concept-entity-types.md#regular-expression-entity) kullanmayı düşünün. 

Bir normal ifade varlığı kullanmak, düzenin eşleşmesi için veri ayıklamasını garanti eder. Bu eşleştirme, tam bir amacı garanti eder. 

## <a name="intent-balance"></a>Intent bakiyesi
Uygulama etki alanı hedefleri konuşma dengesi her hedefi arasında olmalıdır. 10 Konuşma ile bir hedefi ve 500 Konuşma ile başka bir amacı yoktur. Bu dengeli değil. Bu durum varsa, birçok hedefleri halinde yeniden, görmek için 500 konuşma amacıyla gözden geçirin. bir [deseni](luis-concept-patterns.md). 

**Hiçbiri** hedefi bakiyeye dahil değildir. Bu hedefi % uygulamasında toplam konuşma 10 içermelidir.

## <a name="intent-limits"></a>Intent sınırları
Gözden geçirme [sınırları](luis-boundaries.md#model-boundaries) kaç hedefleri öğrenmek için bir model ekleyebilirsiniz. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>Intents en fazla sayısından fazlasını gerekiyorsa 
İlk olarak, sisteminize çok fazla ıntents kullanıp kullanmadığını göz önünde bulundurun. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Birden çok hedefleri tek amacı, varlıklarla birleştirilebilir 
Çok benzer bir ıntents LUIS bunları ayırt daha zor yapabilirsiniz. Intents olması için anın ancak alan, kodunuzun her yolunu yakalamak üzere gerekmeyen ana görevleri yakalamak için yeterli değiştirilen. Örneğin, bir seyahat uygulamasında ayrı ıntents BookFlight ve FlightCustomerService olabilir, ancak BookInternationalFlight ve BookDomesticFlight çok benzerdir. Bunları ayırt etmek sisteminizi gerekiyorsa, varlıklar veya diğer mantıksal yerine hedefleri kullanın. 

### <a name="dispatcher-model"></a>Dağıtıcı modeli
LUIS ve soru-cevap Oluşturucu uygulamalarla birleştirme hakkında daha fazla bilgi [gönderme modeli](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps). 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Önemli sayıda hedefleri olan uygulamalar için yardım iste
Intents sayısını azaltmayı ya da birden çok uygulamalarda, hedefleri bölme sizin için işe yaramazsa, desteğe başvurun. Destek Hizmetleri Azure aboneliğinize dahildir, başvurun [Azure teknik desteğine](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [varlıkları](luis-concept-entity-types.md), önemli sözcükleri hedefleri için uygun olan
* Bilgi nasıl [ekleyin ve hedefleri yönetin](luis-how-to-add-intents.md) LUIS uygulamanızda.
* Gözden geçirin, amacı [en iyi uygulamalar](luis-concept-best-practices.md)
