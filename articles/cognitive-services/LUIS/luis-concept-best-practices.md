---
title: En iyi yöntemler-LUSıS
titleSuffix: Azure Cognitive Services
description: LUIS uygulama modelinden en iyi sonuçları almak için LUIS en iyi adımları öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 7a2802bd4daa1a009c610688120c9a56583b054f
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639282"
---
# <a name="best-practices-for-building-a-language-understanding-app-with-cognitive-services"></a>Bilişsel hizmetler dil anlama uygulamayla oluşturmaya yönelik en iyi uygulamalar
LUSıS uygulamanızı derlemek için uygulama yazma işlemini kullanın: 

* Dil modeli oluşturun
* Birkaç eğitim örnek konuşma (10-15 hedefi başına) ekleme
* Yayımlama 
* Uç noktasından test 
* Özellik ekleme

Uygulamanız [yayımlandıktan](luis-how-to-publish-app.md)sonra, uç noktadan özellik eklemek, yayımlamak ve test etmek için yazma döngüsünü kullanın. Sonraki geliştirme döngüsünü daha fazla örnek konuşma ekleyerek başlamaz. Modelinizi gerçek kullanıcı konuşma öğrenin LUIS izin vermez. 

Geçerli örnek hem uç nokta konuşma döndürüyor kadar başarılara, yüksek tahmin puanları LUIS öğrenme işini etkili olması için sırada konuşma genişletmeyin. [Etkin öğrenme](luis-concept-review-endpoint-utterances.md), [desenler](luis-concept-patterns.md)ve [tümcecik listelerini](luis-concept-feature.md)kullanarak puanları geliştirir. 

## <a name="do-and-dont"></a>Bunu yapmak ve yok
Aşağıdaki listede LUIS uygulamaları için en iyi uygulamaları içerir:

|Yapın|Yapmayın|
|--|--|
|[Farklı hedefleri tanımlayın](#do-define-distinct-intents) |[Amaçlar için birçok örnek Konuşma ekleme](#dont-add-many-example-utterances-to-intents) |
|[Her amaç için çok geneldir ve çok belirli arasında bir tatlı nokta bulmak](#do-find-sweet-spot-for-intents)|[LUIS eğitim platformu olarak kullanın](#dont-use-luis-as-a-training-platform)|
|[Yinelemeli olarak uygulamanızı oluşturun](#do-build-the-app-iteratively)|[Diğer biçimlere yoksayılıyor aynı biçimde, birçok örnek Konuşma ekleme](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Sonraki yinelemelerde tümcecik listeler ve düzenleri ekleyin](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Hedefleri ve varlıkların tanımı karışımı](#dont-mix-the-definition-of-intents-and-entities)|
|Hiçbiri amacı dışında [tüm amaçlar genelinde utbotlarınızı dengeleyin](#balance-your-utterances-across-all-intents) .<br>[Hedefi hiçbiri için örnek Konuşma ekleme](#do-add-example-utterances-to-none-intent)|[Olası tüm değerlerle ifade listeleri oluşturma](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Etkin öğrenme için Öner özelliğinden yararlanın](#do-leverage-the-suggest-feature-for-active-learning)|[Çok fazla desen ekleme](#dont-add-many-patterns)|
|[Uygulamanızın performansını izleyin](#do-monitor-the-performance-of-your-app)|[Eğitin ve yayımlayın her tek örnek utterance eklendi](#dont-train-and-publish-with-every-single-example-utterance)|
|[Her uygulama yinelemesi için sürümleri kullanın](#do-use-versions-for-each-app-iteration)||

## <a name="do-define-distinct-intents"></a>Farklı hedefleri tanımlayın
Sözlük her hedefi için yalnızca bu amaç için ve farklı bir hedefi ile çakışan olduğundan emin olun. Örneğin, hava yolu fışıkları ve oteller gibi seyahat düzenlemelerini işleyen bir uygulamaya sahip olmak istiyorsanız, bu konu alanlarının farklı amaçlar halinde veya aynı amaca göre, utterance içindeki belirli veriler için varlıklar kullanabilirsiniz.

Sözlük iki amaçları arasında aynı ise, amacı birleştirin ve varlıkları kullanın. 

Aşağıdaki örnek konuşma göz önünde bulundurun:

|Örnek konuşmalar|
|--|
|Kitap bir uçuş|
|Kitap bir otel|

"Bir kitap" ve "bir otel kitap" aynı sözlüğünü kullanın "kitap bir". Bu biçim aynı olduğu için aynı amaç, ayıklanan varlıklar olarak farklı Uçuş ve otel kelimeleri ile aynı amaç olmalıdır. 

Daha fazla bilgi için:
* Prototip [LUSıS uygulamanızdaki amaçlar hakkındaki kavramlar](luis-concept-intent.md)
* Öğretici: [Kullanıcı amaçları 'nı öğrenmek için LUSıS uygulaması oluşturun](luis-quickstart-intents-only.md)
* Nasıl yapılır: [Kullanıcı amaçlarını belirleme amacını ekleyin](luis-how-to-add-intents.md)


## <a name="do-find-sweet-spot-for-intents"></a>Tatlı nokta hedefleri için Bul
LUIS verilerden öngörü, hedefleri örtüşmez belirlemek için kullanın. Çakışan amaçlar LUSıS 'yi şaşırtır. Hedefi Puanlama en yakın olduğunu sonucudur başka bir amaç. LUIS her zaman eğitim verilerde aynı tam yolunu kullanmadığından, ilk veya ikinci eğitim olma şansı çakışan bir amacı vardır. Bu çevir/flop gerçekleşmemesi için her bir amaç için utterance 'in puanına sahip olmasını istiyorsunuz. İyi ayrım hedefleri için beklenen üst amaca her seferinde neden olur. 
 
## <a name="do-build-the-app-iteratively"></a>Uygulama çalıştırmalarınızı derleme
[Örnek](luis-concept-utterance.md) Aralık veya uç nokta utaları olarak kullanılmayan ayrı bir Aralık kümesini saklayın. Uygulamayı test kümeniz için geliştirilir. Gerçek kullanıcı konuşma yansıtmak için test uyarlayın. Uygulamanın her bir yinelemesini veya sürümünü değerlendirmek için bu test kümesini kullanın. 

Geliştiriciler, üç veri kümesi olması gerekir. Model oluşturmak için örnek konuşma davranıştır. Uç nokta modeli test etmek için saniyedir. Üçüncü blind test kullanılan verileri olduğu [toplu test](luis-how-to-batch-test.md). Bu son küme, uygulamayı eğitmek veya uç noktada göndermek için kullanılmaz.  

Daha fazla bilgi için:
* Prototip [LUSıS uygulamanız için yazma çevrimi](luis-concept-app-iteration.md)

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>İfade listeleri ve desenler sonraki yinelemelerde ekleme

Uygulamanız sınanmadan önce bu uygulamaların uygulanmamaları en iyi uygulamadır. Bu özellikler daha fazla değerden daha büyük bir şekilde ağırlıklı olduğundan ve güvenilirliği eğeceğinden, [ifade listeleri](luis-concept-feature.md) ve [desenler](luis-concept-patterns.md) eklemeden önce uygulamanın nasıl davranacağını anlamalısınız. 

Uygulamanızın bu olmadan nasıl davranacağını anladıktan sonra, bu özelliklerin her birini uygulamanıza uygulanan şekilde ekleyin. Her [yineleme](luis-concept-app-iteration.md) ile bu özellikleri eklemeniz veya özellikleri her bir sürümle değiştirmeniz gerekmez. 

Model tasarımınızın başına ekleme konusu yoktur, ancak model, her bir özelliğin, model, bir yandan da test edildikten sonra nasıl değiştiği hakkında daha kolay bir şekilde görülebilir. 

[Etkin öğrenmesinin](luis-concept-review-endpoint-utterances.md)sağladığı avantajdan yararlanmak için [uç nokta](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) aracılığıyla test etmeniz en iyi uygulamadır. [Etkileşimli test bölmesi](luis-interactive-test.md) aynı zamanda geçerli bir test yöntemlidir. 
 

### <a name="phrase-lists"></a>Tümcecik listeleri

[Tümcecik listeleri](luis-concept-feature.md) sözlükleri bir kelimelerin uygulama etki alanınızla ilişkili tanımlamanızı sağlar. Tümcecik listenizi birkaç sözcükle çekirdek yapın ve ardından öner özelliğini kullanarak lu, uygulamanıza özgü Sözlük içinde daha fazla sözcük hakkında daha fazla bilgi sahibi olur. Tümcecik listesi, uygulamanız için önemli olan sözcüklerle veya tümceciklerle ilişkili sinyali artırarak amaç algılamayı ve varlık sınıflandırmasını geliştirir. 

Her sözcük, tümcecik listesi değil tam bir eşleşme olduğundan sözlüğü eklemeyin. 

Daha fazla bilgi için:
* Prototip [LUSıS uygulamanızdaki ifade listesi özellikleri](luis-concept-feature.md)
* Nasıl yapılır: [Sözcük listesinin sinyalini artırmak için tümcecik listelerini kullanma](luis-how-to-add-features.md)

### <a name="patterns"></a>Desenler

Gerçek kullanıcı konuşma birbirine çok benzer uç noktasından word seçim ve yerleştirme düzenlerini gösterilmesine neden olabilir. [Deseni](luis-concept-patterns.md) özelliği bu sözcük seçimi ve normal ifadeler birlikte yerleştirme, tahmin doğruluğunu artırmak için alır. Bir normal ifade deseninde sözcükler ve noktalama işaretleri hala desen eşleştirme sırasında yok saymak için istediğinize sağlar. 

Noktalama işaretleri için deseninin [isteğe bağlı sözdizimini](luis-concept-patterns.md) kullanın, böylece noktalama işareti yoksayılabilir. Kalıbı dengelemek için [Açık listeyi](luis-concept-patterns.md#explicit-lists) kullanın. herhangi bir sözdizimi sorunu. 

Daha fazla bilgi için:
* Prototip [Desenler tahmin doğruluğunu geliştirir](luis-concept-patterns.md)
* Nasıl yapılır: [Tahmin doğruluğunu artırmak için desenler ekleme](luis-how-to-model-intent-pattern.md)

## <a name="balance-your-utterances-across-all-intents"></a>Tüm Amaçlar genelinde dıklarınızı dengelemek

LULAR tahminlerinin doğru olması için, her bir amaç içindeki (hiçbiri amacı hariç) örnek miktarları görece eşit olmalıdır. 

100 örnek ile ilgili bir amaç varsa ve 20 örnek elde ediyorsanız, 100-utterance amacına daha yüksek bir tahmin ücreti sunulacaktır.  

## <a name="do-add-example-utterances-to-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme

Bu amaç, uygulamanızın dışındaki her şeyi belirtilen geri dönüş amacını gösterir. Bir örnek utterance hiçbiri LUIS uygulamanızı geri kalanında her 10 örnek konuşma için hedefi ekleyin.

Daha fazla bilgi için:
* Prototip [LUSıS uygulamanız için nelerin iyi olduğunu anlayın](luis-concept-utterance.md)

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Etkin öğrenme için Öner özelliğinden yararlanın

Kullanım [etkin olarak öğrenmeye](luis-how-to-review-endpoint-utterances.md)'s **gözden geçirin, konuşma uç noktası** hedefleri için daha fazla örnek konuşma eklemek yerine düzenli olarak. Uygulamayı sürekli olarak konuşma uç noktası almak için bu listeyi artan ve değiştirme.

Daha fazla bilgi için:
* Prototip [Uç nokta uttlerini inceleyerek etkin öğrenmeyi etkinleştirme kavramları](luis-concept-review-endpoint-utterances.md)
* Öğretici: [Öğretici: Uç nokta dıklarını inceleyerek, hariç tahminleri düzeltir](luis-tutorial-review-endpoint-utterances.md)
* Nasıl yapılır: [LUSıS portalındaki uç nokta utslerini gözden geçirme](luis-how-to-review-endpoint-utterances.md)

## <a name="do-monitor-the-performance-of-your-app"></a>Uygulamanızın performansını izleyin

Bir [Batch test](luis-concept-batch-test.md) kümesi kullanarak tahmin doğruluğunu izleyin. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Birçok örnek konuşma için hedefleri ekleme

Yalnızca uygulama yayımlandıktan sonra konuşma bir süreçtir, etkin öğrenim ekleyin. Konuşma çok benzer bir desen ekleyin. 

## <a name="dont-use-luis-as-a-training-platform"></a>LUIS eğitim platformu olarak kullanmayın

LUIS, bir dil modeli etki özeldir. Genel bir doğal dil eğitimi platformu olarak çalışması amaçlıyordu. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Diğer biçimlere yoksayılıyor aynı biçimde, birçok örnek Konuşma ekleme

LUIS, içinde bir amaç'ın konuşma çeşitlemeleri bekliyor. Konuşma, aynı genel anlamı yaparken farklılık gösterebilir. Değişimleri utterance uzunluğu, word seçeneği ve word yerleştirme içerebilir. 

|Aynı biçimi kullanma|Değişen biçimini kullanın|
|--|--|
|Seattle bilet satın alma<br>Paris bilet satın alma<br>Orlando bilet satın alma|Seattle 1 bilet satın alma<br>Ayırma paris'e sonraki Pazartesi kırmızı göz üzerindeki iki bilgisayar lisansı<br>3 biletleri için spring sonu tarihleri arasında Orlando için kitap istiyorum|

Farklı miktarlarını, ikinci sütun kullanan farklı fiiller (satın alma, ayrılmış, kitabı) (1, iki, 3), ve sözcükler ancak tüm farklı düzenlenişlerini uçak bileti seyahat satın bağdaştırıcılar aynı amaca sahip. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Hedefleri ve varlıkların tanımı bir arada kullanmayın

Botunuzun sürecek bir hedefi için herhangi bir işlem oluşturun. Varlıklar, bu eylem mümkün kılan parametreleri olarak kullanma. 

Havayolu uçuşlar kitap bir sohbet Robotu için oluşturun bir **BookFlight** hedefi. Her Havayolu ya da her hedef için bir hedefi oluşturmayın. Bu parçaları veri kullanan [varlıkları](luis-concept-entity-types.md) ve bunları örnek konuşma işaretleyin. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>Olası tüm değerlerle ifade listeleri oluşturma

Bazı örneklerde sağlamak [tümcecik listeleri](luis-concept-feature.md) ancak her sözcük. LUIS genelleştirir ve bağlam dikkate alır. 

## <a name="dont-add-many-patterns"></a>Birçok desen ekleme

Ekleme çok fazla [desenleri](luis-concept-patterns.md). LUIS, daha az sayıda örnek ile hızlı bir şekilde öğrenmek için tasarlanmıştır. Sistem gereksiz yere aşırı yükleme yapmaz.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Yoksa, eğitmek ve her tek örnek utterance ile yayımlama

10 veya 15 konuşma, eğitim ve yayımlamadan önce ekleyin. Tahmin doğruluğunu etkisini görmenize olanak sağlar. Tek bir utterance ekleme puanına göre görünür bir etkisi olmayabilir. 

## <a name="do-use-versions-for-each-app-iteration"></a>Her uygulama yinelemesi için sürümleri kullanın

Her yazma döngüsünün, var olan sürümden klonlanan yeni bir [Sürüm](luis-concept-version.md)içinde olması gerekir. LUIN sürümleri için sınır yoktur. Bir sürüm adı, API yolunun bir parçası olarak kullanılır, bu nedenle bir URL 'de izin verilen karakterlerin yanı sıra bir sürüm için 10 karakter sayısı içinde tutulması önemlidir. Sürümlerinizin düzenlenmesini sağlamak için bir sürüm adı Stratejisi geliştirin. 

Daha fazla bilgi için:
* Prototip [Bir LUSıS sürümünün nasıl ve ne zaman kullanılacağını anlayın](luis-concept-version.md)
* Nasıl yapılır: [Hazırlama veya üretim uygulamalarını etkilemeden düzenlemek ve test etmek için sürümleri kullanın](luis-how-to-manage-versions.md)


## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [uygulamanızı planlama](luis-how-plan-your-app.md) LUIS uygulamanızda.
