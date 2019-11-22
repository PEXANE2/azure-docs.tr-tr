---
title: LUSıS uygulamanızı oluşturmaya yönelik en iyi uygulamalar
titleSuffix: Azure Cognitive Services
description: LUSıS uygulamanızın modelinden en iyi sonuçları elde etmek için en iyi uygulamaları öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: b4be79338db71ad83204fae971da0b77885a8070
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280918"
---
# <a name="best-practices-for-building-a-language-understanding-luis-app"></a>Dil anlama (LUSıS) uygulaması oluşturmak için en iyi uygulamalar
LUSıS uygulamanızı derlemek için uygulama yazma işlemini kullanın: 

* Yapı dil modelleri (amaçlar ve varlıklar)
* Birkaç eğitim örneği ekleyin (amaç başına 15-30)
* Uç noktada Yayımla
* Uç noktasından test 

Uygulamanız [yayımlandıktan](luis-how-to-publish-app.md)sonra, uç noktadan özellikler, yayımlama ve test için geliştirme yaşam döngüsünü kullanın. Daha fazla örnek ekleyerek bir sonraki yazma döngüsüne başlamamayın çünkü bu, LUSıS 'ın gerçek dünyada Kullanıcı utcileri ile modelinizi öğrenmesinin izin vermez. 

Her iki örneğin ve uç nokta çeşidinin geçerli kümesi, bir bütün olarak, yüksek tahmin puanlarının döndürülünceye kadar bu noktaları genişletmeyin. [Etkin öğrenimi](luis-concept-review-endpoint-utterances.md)kullanarak puanları geliştirir. 




## <a name="do-and-dont"></a>Bunu yapmak ve yok
Aşağıdaki listede LUIS uygulamaları için en iyi uygulamaları içerir:

|Yapın|Yapmayın|
|--|--|
|[Farklı hedefleri tanımlayın](#do-define-distinct-intents)<br>[Amaçlar 'a tanımlayıcı ekleme](#do-add-descriptors-to-intents) |[Amaçlar için birçok örnek Konuşma ekleme](#dont-add-many-example-utterances-to-intents)<br>[Birkaç veya basit varlık kullanın](#dont-use-few-or-simple-entities) |
|[Her amaç için çok geneldir ve çok belirli arasında bir tatlı nokta bulmak](#do-find-sweet-spot-for-intents)|[LUIS eğitim platformu olarak kullanın](#dont-use-luis-as-a-training-platform)|
|[Uygulamanızı sürümleriyle tekrarlayarak oluşturun](#do-build-your-app-iteratively-with-versions)<br>[Model ayrıştırma için varlık oluşturma](#do-build-for-model-decomposition)|[Diğer biçimlere yoksayılıyor aynı biçimde, birçok örnek Konuşma ekleme](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Sonraki yinelemelerde desenler ekleme](#do-add-patterns-in-later-iterations)|[Hedefleri ve varlıkların tanımı karışımı](#dont-mix-the-definition-of-intents-and-entities)|
|Hiçbiri amacı dışında [tüm amaçlar genelinde utbotlarınızı dengeleyin](#balance-your-utterances-across-all-intents) .<br>[Hedefi hiçbiri için örnek Konuşma ekleme](#do-add-example-utterances-to-none-intent)|[Tüm olası değerlerle tanımlayıcı oluştur](#dont-create-descriptors-with-all-the-possible-values)|
|[Etkin öğrenme için Öner özelliğinden yararlanın](#do-leverage-the-suggest-feature-for-active-learning)|[Çok fazla desen ekleme](#dont-add-many-patterns)|
|[Toplu test ile uygulamanızın performansını izleyin](#do-monitor-the-performance-of-your-app)|[Eğitin ve yayımlayın her tek örnek utterance eklendi](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Farklı hedefleri tanımlayın
Sözlük her hedefi için yalnızca bu amaç için ve farklı bir hedefi ile çakışan olduğundan emin olun. Örneğin, hava yolu fışıkları ve oteller gibi seyahat düzenlemelerini işleyen bir uygulamaya sahip olmak istiyorsanız, bu konu alanlarının farklı amaçlar halinde veya aynı amaca göre, utterance içindeki belirli veriler için varlıklar kullanabilirsiniz.

Sözlük iki amaçları arasında aynı ise, amacı birleştirin ve varlıkları kullanın. 

Aşağıdaki örnek konuşma göz önünde bulundurun:

|Örnek konuşmalar|
|--|
|Kitap bir uçuş|
|Kitap bir otel|

`Book a flight` ve `Book a hotel` aynı `book a `sözlüğünü kullanır. Bu biçim aynı şekilde aynıdır, bu nedenle `flight` farklı kelimeleri ve ayıklanan varlıklar olarak `hotel` aynı amaç olmalıdır. 

## <a name="do-add-descriptors-to-intents"></a>Amaçlar 'a tanımlayıcı ekleme

Tanımlayıcılar, bir amaç için özellikleri tanımlamaya yardımcı olur. Bir tanımlayıcı, bu amaç için önemli olan sözcüklerin bir ifade listesi veya bu amaç için önemli bir varlık olabilir. 

## <a name="do-find-sweet-spot-for-intents"></a>Tatlı nokta hedefleri için Bul
LUIS verilerden öngörü, hedefleri örtüşmez belirlemek için kullanın. Çakışan amaçlar LUSıS 'yi şaşırtır. Hedefi Puanlama en yakın olduğunu sonucudur başka bir amaç. LUIS her zaman eğitim verilerde aynı tam yolunu kullanmadığından, ilk veya ikinci eğitim olma şansı çakışan bir amacı vardır. Bu çevir/flop gerçekleşmemesi için her bir amaç için utterance 'in puanına sahip olmasını istiyorsunuz. İyi ayrım hedefleri için beklenen üst amaca her seferinde neden olur. 
 
<a name="#do-build-the-app-iteratively"></a>

## <a name="do-build-your-app-iteratively-with-versions"></a>Uygulamanızı sürümleriyle tekrarlayarak oluşturma

Her yazma döngüsünün, var olan sürümden klonlanan yeni bir [Sürüm](luis-concept-version.md)içinde olması gerekir. 

## <a name="do-build-for-model-decomposition"></a>Model ayrıştırma için derleme yap

Model ayrıştırma işleminin tipik bir süreci vardır:

* istemci uygulamanın kullanıcı amaçları temelinde **Amaç** oluşturun
* gerçek dünya Kullanıcı girişine göre 15-30 örnek ekleme
* örnek utde üst düzey veri kavramını etiketle
* veri kavramını alt bileşenlere Böl
* alt bileşenlere tanımlayıcı (Özellikler) ekleme
* amaca tanımlayıcı (Özellikler) ekleme 

Amacınızı oluşturduktan sonra, örnek bir şekilde, aşağıdaki örnekte varlık ayrıştırma açıklanmaktadır. 

Bir utterde çıkarmak istediğiniz tüm veri kavramlarını tanımlayarak başlayın. Bu, makineniz tarafından öğrenilen varlıktır. Ardından, tümceciğini parçalarını parçalara ayırın. Bu, alt bileşenleri (varlıklar olarak), tanımlayıcılar ve kısıtlamalarla birlikte tanımlamayı içerir. 

Örneğin, bir adresi ayıklamak istiyorsanız, en iyi makine tarafından öğrenilen varlık `Address`olarak çağrılabilir. Adresi oluştururken, sokak adresi, şehir, eyalet ve posta kodu gibi bazı alt bileşenlerinden bazılarını yapın. 

Posta kodunu normal bir **ifadeye geçirerek bu** öğeleri oluşturmaya devam edin. Cadde adresini bir cadde numarası (önceden oluşturulmuş bir sayı kullanarak), sokak adı ve sokak türü bölümlerine ayırın. Cadde türü, Avenue, Circle, Road ve Lane gibi bir **tanımlayıcı** listesi ile açıklanabilir.

V3 yazma API 'SI, model ayrıştırma için izin verir. 

## <a name="do-add-patterns-in-later-iterations"></a>Daha sonraki yinelemelerde desenler ekleme

Desenler eklemeden önce uygulamanın nasıl davrandığını anlamanız gerekir [, çünkü desenler](luis-concept-patterns.md) daha fazla örneğe göre daha fazla ağırlıklı ve güvenle eğrilticektir. 

Uygulamanızın nasıl davranacağını anladıktan sonra, uygulamanıza uygulanan desenler ekleyin. Bunları her [yineleme](luis-concept-app-iteration.md)ile eklemeniz gerekmez. 

Model tasarımınızın başına ekleme konusu yoktur, ancak model, modelin bu şekilde test edildikten sonra modeli nasıl değiştirdiğinin daha kolay bir şekilde görülebilir. 
 
<!--

### Phrase lists

[Phrase lists](luis-concept-feature.md) allow you to define dictionaries of words related to your app domain. Seed your phrase list with a few words then use the suggest feature so LUIS knows about more words in the vocabulary specific to your app. A Phrase List improves intent detection and entity classification by boosting the signal associated with words or phrases that are significant to your app. 

Don't add every word to the vocabulary since the phrase list isn't an exact match. 

For more information:
* Concept: [Phrase list features in your LUIS app](luis-concept-feature.md)
* How-to: [Use phrase lists to boost signal of word list](luis-how-to-add-features.md)



### Patterns

Real user utterances from the endpoint, very similar to each other, may reveal patterns of word choice and placement. The [pattern](luis-concept-patterns.md) feature takes this word choice and placement along with regular expressions to improve your prediction accuracy. A regular expression in the pattern allows for words and punctuation you intend to ignore while still matching the pattern. 

Use pattern's [optional syntax](luis-concept-patterns.md) for punctuation so punctuation can be ignored. Use the [explicit list](luis-concept-patterns.md#explicit-lists) to compensate for pattern.any syntax issues. 

For more information:
* Concept: [Patterns improve prediction accuracy](luis-concept-patterns.md)
* How-to: [How to add Patterns to improve prediction accuracy](luis-how-to-model-intent-pattern.md)
-->

<a name="balance-your-utterances-across-all-intents"></a>

## <a name="do-balance-your-utterances-across-all-intents"></a>Tüm Amaçlar genelinde deterlerinizi dengeleyin

LULAR tahminlerinin doğru olması için, her bir amaç içindeki (hiçbiri amacı hariç) örnek miktarları görece eşit olmalıdır. 

100 örnek ile ilgili bir amaç varsa ve 20 örnek elde ediyorsanız, 100-utterance amacına daha yüksek bir tahmin ücreti sunulacaktır.  

## <a name="do-add-example-utterances-to-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme

Bu amaç, uygulamanızın dışındaki her şeyi gösteren geri dönüş amacını gösterir. Bir örnek utterance hiçbiri LUIS uygulamanızı geri kalanında her 10 örnek konuşma için hedefi ekleyin.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Etkin öğrenme için Öner özelliğinden yararlanın

Kullanım [etkin olarak öğrenmeye](luis-how-to-review-endpoint-utterances.md)'s **gözden geçirin, konuşma uç noktası** hedefleri için daha fazla örnek konuşma eklemek yerine düzenli olarak. Uygulamayı sürekli olarak konuşma uç noktası almak için bu listeyi artan ve değiştirme.

## <a name="do-monitor-the-performance-of-your-app"></a>Uygulamanızın performansını izleyin

Bir [Batch test](luis-concept-batch-test.md) kümesi kullanarak tahmin doğruluğunu izleyin. 

[Örnek](luis-concept-utterance.md) Aralık veya uç nokta utaları olarak kullanılmayan ayrı bir Aralık kümesini saklayın. Uygulamayı test kümeniz için geliştirilir. Gerçek kullanıcı konuşma yansıtmak için test uyarlayın. Uygulamanın her bir yinelemesini veya sürümünü değerlendirmek için bu test kümesini kullanın. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Birçok örnek konuşma için hedefleri ekleme

Uygulama yayımlandıktan sonra, yalnızca geliştirme yaşam döngüsü sürecinde etkin öğrenmeden gelen utbotları ekleyin. Konuşma çok benzer bir desen ekleyin. 

## <a name="dont-use-few-or-simple-entities"></a>Birkaç veya basit varlık kullanmayın

Varlıklar, veri ayıklama ve tahmin için oluşturulmuştur. Her bir amaç, amaca göre verileri tanımlayan makine tarafından öğrenilen varlıkların olması önemlidir. Bu, istemci uygulamanızın ayıklanan varlığı kullanması gerekmiyorsa bile, LUTO 'NıN amacı tahmin etmesine yardımcı olur. 

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

Hava yolu fışıklarından kitap oluşturacak bir bot için, bir **muhasebeci** amaç oluşturun. Her Havayolu ya da her hedef için bir hedefi oluşturmayın. Bu parçaları veri kullanan [varlıkları](luis-concept-entity-types.md) ve bunları örnek konuşma işaretleyin. 

## <a name="dont-create-descriptors-with-all-the-possible-values"></a>Tüm olası değerlerle tanımlayıcı oluşturma

Tanımlayıcı [tümceciği listelerinde](luis-concept-feature.md) birkaç örnek sağlayın, ancak her sözcüğe değil. LUIS genelleştirir ve bağlam dikkate alır. 

## <a name="dont-add-many-patterns"></a>Birçok desen ekleme

Ekleme çok fazla [desenleri](luis-concept-patterns.md). LUIS, daha az sayıda örnek ile hızlı bir şekilde öğrenmek için tasarlanmıştır. Sistem gereksiz yere aşırı yükleme yapmaz.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Yoksa, eğitmek ve her tek örnek utterance ile yayımlama

10 veya 15 konuşma, eğitim ve yayımlamadan önce ekleyin. Tahmin doğruluğunu etkisini görmenize olanak sağlar. Tek bir utterance ekleme puanına göre görünür bir etkisi olmayabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi edinmek için nasıl [uygulamanızı planlama](luis-how-plan-your-app.md) LUIS uygulamanızda.
