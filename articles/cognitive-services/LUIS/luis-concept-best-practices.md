---
title: LUIS uygulamanızı oluşturmak için en iyi uygulamalar
titleSuffix: Azure Cognitive Services
description: LUIS uygulamanızın modelinden en iyi sonuçları almak için en iyi uygulamaları öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74280918"
---
# <a name="best-practices-for-building-a-language-understanding-luis-app"></a>Dil anlama (LUIS) uygulaması oluşturmak için en iyi uygulamalar
LUIS uygulamanızı oluşturmak için uygulama yazma işlemini kullanın: 

* Dil modelleri oluşturma (niyet ler ve varlıklar)
* Birkaç eğitim örneği söyleyiş ekleyin (niyet başına 15-30)
* Bitiş noktasına yayımlama
* Uç noktadan test 

Uygulamanız [yayımlandıktan](luis-how-to-publish-app.md)sonra, son noktadan özellikler eklemek, yayımlamak ve test etmek için geliştirme yaşam döngüsünü kullanın. Luis'in modelinizi gerçek dünyadaki kullanıcı söyleyerek öğrenmesine izin vermediği için bir sonraki yazma döngüsüne daha fazla örnek söyleyerek başlamayın. 

Hem örnek hem de bitiş noktası sözcüklerinin geçerli kümesi kendinden emin, yüksek tahmin puanları dönene kadar söyleyemeleri genişletmeyin. Aktif öğrenmeyi kullanarak puanları [artırın.](luis-concept-review-endpoint-utterances.md) 




## <a name="do-and-dont"></a>Yap ve yapma
Aşağıdaki liste LUIS uygulamaları için en iyi uygulamaları içerir:

|Yapın|Yapmayın|
|--|--|
|[Farklı amaçları tanımlama](#do-define-distinct-intents)<br>[Niyetlere tanımlayıcıekleme](#do-add-descriptors-to-intents) |[Niyetlere birçok örnek söyleyiş ekleme](#dont-add-many-example-utterances-to-intents)<br>[Az sayıda veya basit varlık kullanma](#dont-use-few-or-simple-entities) |
|[Her amaç için çok genel ve çok özel arasında tatlı bir nokta bulun](#do-find-sweet-spot-for-intents)|[LUIS'i eğitim platformu olarak kullanın](#dont-use-luis-as-a-training-platform)|
|[Uygulamanızı sürümlerle yinelemeli olarak oluşturun](#do-build-your-app-iteratively-with-versions)<br>[Model ayrıştırma için varlıklar oluşturma](#do-build-for-model-decomposition)|[Diğer biçimleri yoksayarak, aynı biçimde birçok örnek sözcük ekleme](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Sonraki yinelemelerde desen ekleme](#do-add-patterns-in-later-iterations)|[Niyet ve varlıkların tanımını karıştırın](#dont-mix-the-definition-of-intents-and-entities)|
|Yok niyet dışında [tüm niyetler arasında söyleyiş dengeleyin.](#balance-your-utterances-across-all-intents)<br>[None amacına örnek söz ekleme](#do-add-example-utterances-to-none-intent)|[Mümkün olan tüm değerlere sahip tanımlayıcılar oluşturma](#dont-create-descriptors-with-all-the-possible-values)|
|[Etkin öğrenme için önerme özelliğinden yararlanın](#do-leverage-the-suggest-feature-for-active-learning)|[Çok fazla desen ekleme](#dont-add-many-patterns)|
|[Toplu testile uygulamanızın performansını izleme](#do-monitor-the-performance-of-your-app)|[Tren ve her bir örnek söyleyiş ile yayınlamak eklendi](#dont-train-and-publish-with-every-single-example-utterance)|

## <a name="do-define-distinct-intents"></a>Farklı niyetler tanımla
Her amaç için kelime sadece bu amaç için ve farklı bir niyet ile örtüşen olmadığından emin olun. Örneğin, havayolu uçuşları ve oteller gibi seyahat düzenlemelerini ele alan bir uygulamanız olmasını istiyorsanız, bu konu alanlarını ayrı niyetler olarak veya söz konusu veri için varlıklarla aynı niyette olmayı seçebilirsiniz.

İki amaç arasındaki sözcük aynıysa, amacı birleştirin ve varlıkları kullanın. 

Aşağıdaki örnek söyleyerek göz önünde bulundurun:

|Örnek konuşmalar|
|--|
|Bir uçuş rezervasyonu yapın|
|Otel rezervasyonu yapın|

`Book a flight`ve `Book a hotel` aynı kelime `book a `kullanmak . Bu biçim aynıdır, bu nedenle çıkarılan varlıkların farklı `flight` sözcükleri ile `hotel` aynı niyetolmalıdır. 

## <a name="do-add-descriptors-to-intents"></a>Niyetlere tanımlayıcılar ekleme

Tanımlayıcılar, bir amaç için özellikleri açıklamaya yardımcı olur. Tanımlayıcı, bu amaç için önemli olan sözcüklerin veya bu amaç için önemli olan bir varlığın bir ifade listesi olabilir. 

## <a name="do-find-sweet-spot-for-intents"></a>Niyetler için tatlı bir nokta bulmak mı
Niyetlerinizin çakışıp örtüşmeyecek olmadığını belirlemek için LUIS'in tahmin verilerini kullanın. Örtüşen niyetler LUIS'in kafasını karıştırır. Sonuç, en yüksek puanlama niyetinin başka bir amacla çok yakın olmasıdır. LUIS her eğitim için veriler arasında aynı yolu kullanmadığından, çakışan bir niyetin eğitimde birinci veya ikinci olma şansı vardır. Her niyet için söyleyiş puanının birbirinden daha uzak olmasını istiyorsunuz, böylece bu flip/flop gerçekleşmez. Niyetler için iyi bir ayrım her zaman beklenen üst niyet neden olmalıdır. 
 
<a name="#do-build-the-app-iteratively"></a>

## <a name="do-build-your-app-iteratively-with-versions"></a>Uygulamanızı sürümlerle yinelemeli olarak oluşturun

Her yazma döngüsü yeni bir [sürüm](luis-concept-version.md)içinde olmalıdır , varolan bir sürümden klonlanmış. 

## <a name="do-build-for-model-decomposition"></a>Model ayrıştırma için yapı oluşturma

Model ayrıştırma tipik bir işlem vardır:

* istemci uygulamasının kullanıcı amaçlarına göre **Niyet** oluşturma
* gerçek kullanıcı girişine dayalı 15-30 örnek söz eklemek
* örnek söyleyiş etiket üst düzey veri kavramı
* veri kavramını alt bileşenlere ayırma
* alt bileşenlere tanımlayıcı (özellikler) ekleme
* niyete tanımlayıcılar (özellikler) eklemek 

Bir kez niyet oluşturduktan ve örnek ifadeler ekledikten sonra, aşağıdaki örnek varlık ayrışma açıklar. 

Bir söyleyerek ayıklamak istediğiniz tüm veri kavramlarını tanımlayarak başlayın. Bu senin makineden öğrenilen varlığın. Sonra ifadeyi kendi parçalarına ayrıştırın. Bu, alt bileşenleri (varlık olarak) tanımlamanın yanı sıra tanımlayıcılar ve kısıtlamaları içerir. 

Örneğin bir adres ayıklamak istiyorsanız, makinede öğrenilen en `Address`üst varlık çağrılabilir. Adresi oluştururken, sokak adresi, şehir, eyalet ve posta kodu gibi bazı alt bileşenlerini tanımlayın. 

Posta kodunu normal bir ifadeyle **kısıtlayarak** bu öğeleri çürütmeye devam edin. Sokak adresini bir sokak numarasının (önceden oluşturulmuş bir numara kullanarak), bir sokak adının ve sokak türüne ayırın. Sokak türü, cadde, daire, yol ve şerit gibi **tanımlayıcı** bir listeyle tanımlanabilir.

V3 yazma API modeli ayrıştırma sağlar. 

## <a name="do-add-patterns-in-later-iterations"></a>Sonraki yinelemelerde desen ekleme

Desenler örnek söyleyişlerden daha ağır ağırlıklı olduğundan ve güveni çarpıtacağı [için, uygulamanın desen](luis-concept-patterns.md) eklemeden önce nasıl bir şekilde nasıl hissettiğini anlamalısınız. 

Uygulamanızın nasıl bir şekilde hissettiğini anladıktan sonra, uygulamanız için geçerli olan desenler ekleyin. Her [yineleme](luis-concept-app-iteration.md)ile bunları eklemeniz gerekmez. 

Model tasarımınızın başında bunları eklemenin bir sakıncası yoktur, ancak model söyleyerek test edildikten sonra her desenin modeli nasıl değiştirdiğini görmek daha kolaydır. 
 
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

## <a name="do-balance-your-utterances-across-all-intents"></a>Tüm niyetler arasında söyleyişlerinizi dengeleyin

LUIS tahminlerinin doğru olabilmesi için, her bir niyetteki örnek sözcük lerin miktarının (Yok niyeti hariç) nispeten eşit olması gerekir. 

100 örnek söz ve 20 örnek söyleyiş ile bir niyet iseniz, 100-söyleyiş niyeti tahmin daha yüksek bir oran olacaktır.  

## <a name="do-add-example-utterances-to-none-intent"></a>None amacına örnek söz ekleme

Bu amaç, uygulamanızın dışındaki her şeyi gösteren geri dönüş niyetidir. LUIS uygulamanızın geri kalanında her 10 örnek söyleyiş için None niyetine bir örnek söyleyiş ekleyin.

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Aktif öğrenme için önerme özelliğinden yararlanın

Kasıtlı olarak daha fazla örnek sözcük eklemek [yerine, etkin öğrenme](luis-how-to-review-endpoint-utterances.md)'nin **İnceleme uç nokta larını** düzenli olarak kullanın. Uygulama sürekli uç nokta söyleyerek alıyor olduğundan, bu liste büyüyor ve değişiyor.

## <a name="do-monitor-the-performance-of-your-app"></a>Uygulamanızın performansını izleyin

[Toplu iş testi](luis-concept-batch-test.md) kümesini kullanarak tahmin doğruluğunu izleyin. 

[Örnek söz](luis-concept-utterance.md) veya uç nokta söyleyiş olarak kullanılmayan ayrı bir sözcük kümesi tutun. Test setiniz için uygulamayı geliştirmeye devam edin. Test kümesini gerçek kullanıcı söyleyicilerini yansıtacak şekilde uyarla. Uygulamanın her yinelemesini veya sürümünü değerlendirmek için bu test kümesini kullanın. 

## <a name="dont-add-many-example-utterances-to-intents"></a>Niyetlere çok fazla örnek söz eklemeyin

Uygulama yayınlandıktan sonra, yalnızca geliştirme yaşam döngüsü sürecinde aktif öğrenmeden gelen sözlerle ekleyin. Söyleyişler çok benzerse, bir desen ekleyin. 

## <a name="dont-use-few-or-simple-entities"></a>Az sayıda veya basit varlık kullanmayın

Varlıklar veri ayıklama ve tahmin için oluşturulur. Her niyetin, amaçtaki verileri açıklayan makineden öğrenilen varlıklara sahip olması önemlidir. Bu, istemci uygulamanızın ayıklanan varlığı kullanması gerekmese bile, LUIS'in amacı tahmin etmesini sağlar. 

## <a name="dont-use-luis-as-a-training-platform"></a>LUIS'i eğitim platformu olarak kullanmayın

LUIS, bir dil modelinin etki alanına özgüdür. Bu genel bir doğal dil eğitim platformu olarak çalışmak anlamına gelmez. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>Diğer biçimleri yoksayarak, aynı biçimde çok sayıda örnek sözcük eklemeyin

LUIS bir niyetin söyleyişlerinde farklılıklar bekler. Aynı genel anlamı sahip iken söyleyemeler değişebilir. Varyasyonlar, sözcük uzunluğu, sözcük seçimi ve sözcük yerleşimini içerebilir. 

|Aynı biçimi kullanmayın|Farklı biçim kullanma|
|--|--|
|Seattle'a bilet al<br>Paris'e bilet al<br>Orlando'ya bilet al|Seattle için 1 bilet satın alın<br>Önümüzdeki Pazartesi Paris'e kırmızı göz üzerinde rezerv iki koltuk<br>Bahar tatili için Orlando'ya 3 bilet ayırtmak istiyorum.|

İkinci sütunda farklı fiiller (satın alma, rezerv, kitap), farklı miktarlarda (1, iki, 3) ve kelimelerin farklı düzenlemeleri kullanılır, ancak hepsi seyahat için uçak bileti satın alma niyetine sahiptir. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>Niyet ve varlıkların tanımını karıştırmayın

Botunuzun yapacağı herhangi bir eylem için bir niyet oluşturun. Varlıkları, bu eylemi mümkün kınlayan parametreler olarak kullanın. 

Havayolu uçuşları için rezervasyon yaptıracak bir bot için **BookFlight** amacı oluşturun. Her havayolu veya her varış noktası için bir niyet oluşturmayın. Bu verileri [varlık](luis-concept-entity-types.md) olarak kullanın ve bunları örnek sözlerle işaretleyin. 

## <a name="dont-create-descriptors-with-all-the-possible-values"></a>Tüm olası değerlere sahip tanımlayıcılar oluşturmayın

Açıklayıcı [tümcecik listelerinde](luis-concept-feature.md) birkaç örnek sağlayın, ancak her sözcük tespret indeğil. LUIS genelleştirir ve bağlamı dikkate alır. 

## <a name="dont-add-many-patterns"></a>Çok desen eklemeyin

Çok fazla desen [eklemeyin.](luis-concept-patterns.md) LUIS daha az örnekle hızlı bir şekilde öğrenmek içindir. Gereksiz yere sistemi aşırı yüklemeyin.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>Her bir örnekle eğitilip yayınlamayın

Eğitim ve yayıncılık önce 10 veya 15 kelime ekleyin. Bu, tahmin doğruluğu üzerindeki etkisini görmenizi sağlar. Tek bir sözcük eklemek skor üzerinde gözle görülür bir etki yaratmayabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* LUIS [uygulamanızda uygulamanızı nasıl planlayınız](luis-how-plan-your-app.md) öğrenin.
