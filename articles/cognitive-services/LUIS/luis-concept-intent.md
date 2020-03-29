---
title: Niyet ler ve varlıklar - LUIS
titleSuffix: Azure Cognitive Services
description: Tek bir amaç, kullanıcının gerçekleştirmek istediği bir görevi veya eylemi temsil eder. Bu, kullanıcının ifadesinde ortaya konan hedeftir. Kullanıcıların uygulamanızda yapmak istediği eylemlere karşılık gelen bir niyet kümesi tanımlayın.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220979"
---
# <a name="intents-in-your-luis-app"></a>LUIS uygulamanızdaki niyetler

Bir niyet, kullanıcının gerçekleştirmek istediği bir görevi veya eylemi temsil eder. Bu bir amaç veya hedef bir kullanıcının [söyleyiş](luis-concept-utterance.md)ifade edilir.

Kullanıcıların uygulamanızda yapmak istediği eylemlere karşılık gelen bir niyet kümesi tanımlayın. Örneğin, bir seyahat uygulaması birkaç amacı tanımlar:

Seyahat uygulaması amaçları   |   Örnek konuşmalar   | 
------|------|
 BookFlight     |   "Gelecek hafta Rio'ya bir uçuş ayarla" <br/> "Beni 24'te Rio'ya uçur" <br/> "Önümüzdeki Pazar Rio de Janeiro'ya uçak bileti istiyorum."    |
 Karşılama     |   "Merhaba" <br/>"Merhaba" <br/>"Günaydın"  |
 CheckWeather | "Boston'da hava nasıl?" <br/> "Bana bu hafta sonu için hava tahminini göster" |
 None         | "Bana bir kurabiye tarifi getirin"<br>"Lakers kazandı mı?" |

Tüm uygulamalar önceden tanımlanmış bir niyetle gelir, "[Yok](#none-intent)", geri dönüş niyeti. 

## <a name="prebuilt-domains-provide-intents"></a>Önceden oluşturulmuş etki alanları, niyet sağlar
Tanımladığınız niyetlere ek olarak, [önceden oluşturulmuş etki alanlarından](luis-how-to-use-prebuilt-domains.md)birinden önceden oluşturulmuş amaçları da kullanabilirsiniz. 

## <a name="return-all-intents-scores"></a>Tüm niyet puanları döndürme
Tek bir niyete bir söz atıyorsun. LUIS, varsayılan olarak bitiş noktasında bir söz aldığında, bu söyleyiş için en yüksek niyeti döndürür. 

Tüm sözcüklerin puanlarını sözcük için istiyorsanız, tahmin API'sının sorgu dizelerinde bir bayrak sağlayabilirsiniz. 

|Tahmin API sürümü|Bayrak|
|--|--|
|V2|`verbose=true`|
|V3|`show-all-intents=true`|

## <a name="intent-compared-to-entity"></a>Varlıkla karşılaştırıldığında niyet
Amaç, uygulamanın kullanıcı için yapması gereken eylemi temsil eder ve tüm söyleyiş üzerine kuruludur. Bir söyleyiş yalnızca bir üst puanlama niyeti olabilir ama birçok varlıklar olabilir. 

<a name="how-do-intents-relate-to-entities"></a>

Kullanıcının _niyeti_ istemci uygulamanızda checkweather() işlevine yapılan bir çağrı gibi bir eylemi tetiklediğinde bir niyet oluşturun. Ardından, eylemi yürütmek için gereken parametreleri temsil edecek varlıklar oluşturun. 

|Amaç   | Varlık | Örnek konuşma   | 
|------------------|------------------------------|------------------------------|
| CheckWeather | { "türü": "konum", "varlık": "seattle" }<br>{ "type": "builtin.datetimeV2.date","entity": "tomorrow","resolution":"2018-05-23" } | Hava `Seattle` `tomorrow`nasıl? |
| CheckWeather | { "type": "date_range", "entity": "bu hafta sonu" } | Bana tahmin göster`this weekend` | 
||||

## <a name="prebuilt-domain-intents"></a>Önceden oluşturulmuş etki alanı amaçları

[Önceden oluşturulmuş etki alanları,](luis-how-to-use-prebuilt-domains.md) kasıtlı olarak söyleyiş ler sağlar. 

## <a name="none-intent"></a>Amaç yok

**Yok** niyeti oluşturulur, ancak bilerek boş bırakılır. **None** amacı gerekli bir amaçtır ve silinemez veya yeniden adlandırılamaz. Etki alanınızın dışındaki söyleyişlerle doldurun.

**None** niyet geri dönüş niyeti, her uygulamada önemli ve toplam söyleyiş% 10 olmalıdır. Bu uygulama etki alanı (konu alanı) önemli olmayan LUIS söyleyiş öğretmek için kullanılır. **Yok** niyeti için herhangi bir söz eklemezseniz, LUIS etki alanı dışındaki bir söyleyiyi etki alanı niyetlerinden birine zorlar. Bu, LUIS'e söyleyiş için yanlış niyeti öğreterek tahmin puanlarını çarpıtacaktır. 

Bir söyleyiş Yok amacı olarak tahmin edildiğinde, istemci uygulaması daha fazla soru sorabilir veya kullanıcıyı geçerli seçimlere yönlendirmek için bir menü sağlayabilir. 

## <a name="negative-intentions"></a>Olumsuz niyetler 
"Araba **istiyorum"** ve "Araba **istemiyorum"** gibi olumsuz ve olumlu niyetleri belirlemek istiyorsanız, iki niyet (bir pozitif ve bir negatif) oluşturabilir ve her biri için uygun söyleyişler ekleyebilirsiniz. Veya tek bir niyet oluşturabilir ve iki farklı pozitif ve negatif terimleri bir varlık olarak işaretleyebilirsiniz.  

## <a name="intents-and-patterns"></a>Niyetler ve desenler

Düzenli bir ifade olarak kısmen veya tamamen tanımlanabilen örnek ifadeleriniz varsa, [bir desenle](luis-concept-patterns.md)eşleştirilmiş [normal ifade varlığını](luis-concept-entity-types.md#regular-expression-entity) kullanmayı düşünün. 

Normal bir ifade varlığı kullanmak, desenin eşleşebilmeleri için veri ayıklamayı garanti eder. Desen eşleştirmesi, tam bir niyetin döndürülemesini garanti eder. 

## <a name="intent-balance"></a>Niyet dengesi
Uygulama etki alanı amaçları, her amaç boyunca bir sözcük dengesine sahip olmalıdır. 10, 500 sözle başka bir niyet inanmayın. Bu dengeli değil. Bu durum varsa, 500 söyleyerek niyetin çoğunun bir [desen](luis-concept-patterns.md)halinde yeniden düzenlenip düzenlenmeyeceğini görmek için niyeti gözden geçirin. 

**Yok** niyeti bakiyeye dahil edilmez. Bu niyet, uygulamadaki toplam deyişlerin %10'una sahip olmalıdır.

## <a name="intent-limits"></a>Niyet sınırları
Bir modele kaç niyet ekleyebileceğinizi anlamak için [sınırları](luis-boundaries.md#model-boundaries) gözden geçirin. 

### <a name="if-you-need-more-than-the-maximum-number-of-intents"></a>Maksimum niyet sayısından daha fazlasına ihtiyacınız varsa 
İlk olarak, sisteminizin çok fazla niyet kullanıp kullanmadığını göz önünde bulundurun. 

### <a name="can-multiple-intents-be-combined-into-single-intent-with-entities"></a>Birden çok niyet, varlıklarla tek bir niyetle birleştirilebilir mi? 
Çok benzer olan niyetler, LUIS'in aralarında ayrım yapmalarını zorlaştırabilir. Niyetler, kullanıcının istediği ana görevleri yakalamak için yeterince çeşitli olmalıdır, ancak kodunuzu izlediği her yolu yakalamaları gerekmez. Örneğin, BookFlight ve FlightCustomerService bir seyahat uygulamasında ayrı niyetler olabilir, ancak BookInternationalFlight ve BookDomesticFlight çok benzerdir. Sisteminizin bunları ayırt etmesi gerekiyorsa, niyetler yerine varlıkları veya başka bir mantığı kullanın. 

### <a name="dispatcher-model"></a>Dispatcher modeli
LUIS ve QnA maker uygulamalarını gönderme [modeliyle](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps)birleştirme hakkında daha fazla bilgi edinin. 

### <a name="request-help-for-apps-with-significant-number-of-intents"></a>Önemli sayıda amacı olan uygulamalar için yardım isteyin
Niyet sayısını azaltmak veya niyetinizi birden fazla uygulamaya bölmek sizin için işe yaramıyorsa desteğe başvurun. Azure aboneliğinizde destek hizmetleri varsa, [Azure teknik desteğine](https://azure.microsoft.com/support/options/)başvurun. 

## <a name="next-steps"></a>Sonraki adımlar

* Niyetlerle ilgili önemli kelimeler olan [varlıklar](luis-concept-entity-types.md)hakkında daha fazla bilgi edinin
* LUIS uygulamanızda [niyet eklemeyi ve nasıl yönetleyeceğinizi](luis-how-to-add-intents.md) öğrenin.
* Niyet [en iyi uygulamaları](luis-concept-best-practices.md) gözden geçirin
