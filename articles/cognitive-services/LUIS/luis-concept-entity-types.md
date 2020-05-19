---
title: Varlık türleri-LUSıS
description: Bir varlık, tahmin çalışma zamanında bir Kullanıcı noktasından verileri ayıklar. _İsteğe bağlı_, ikincil amaç, varlığı bir özellik olarak kullanarak amaç veya diğer varlıkların tahminini de artırır.
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: 9d8afd5a660b3af5556256835486e984d7d657bc
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83585649"
---
# <a name="extract-data-with-entities"></a>Varlıkları olan verileri ayıklama

Bir varlık, tahmin çalışma zamanında bir Kullanıcı noktasından verileri ayıklar. _İsteğe bağlı_, ikincil amaç, varlığı bir özellik olarak kullanarak amaç veya diğer varlıkların tahminini de artırır.

Birçok varlık türü vardır:

* [Makine tarafından öğrenilen varlık](reference-entity-machine-learned-entity.md)
* Makine tarafından öğrenilmeyen, tam metin eşleşmeleri, model eşleşmeleri veya önceden oluşturulmuş varlıklara göre algılama için gerekli bir [özellik](luis-concept-feature.md) olarak kullanılır
* Bir [düzendeki](reference-entity-pattern-any.md) kitap başlıkları gibi serbest biçimli metinleri ayıklamak için [.](#patternany-entity)

Makine tarafından öğrenilen varlıklar, en geniş veri ayıklama seçeneği yelpazesi sağlar. Makine tarafından öğrenilen varlıklar, metin eşleştirme ile çalışır ve makine tarafından öğrenilen bir varlık veya amaç için [gerekli bir özellik](#design-entities-for-decomposition) olarak kullanılır.

## <a name="entities-represent-data"></a>Varlıklar verileri temsil eder

Varlıklar, adlar, tarihler, ürün adları veya herhangi bir önemli sözcük grubu gibi, utterklerden çekmek istediğiniz verilerden oluşur. Bir söylenişi birçok varlık içerebilir veya hiç yok. İstemci uygulaması, görevini gerçekleştirmek için verilere gereksinim duyuyor _olabilir_ .

Varlıkların, bir modeldeki her amaç için tüm eğitimlere sürekli olarak etiketlenmesi gerekir.

 [DatetimeV2](luis-reference-prebuilt-datetimev2.md), [Ordinal](luis-reference-prebuilt-ordinal.md), [e-posta](luis-reference-prebuilt-email.md)ve [telefon numarası](luis-reference-prebuilt-phonenumber.md)gibi yaygın kavramlara zaman kazanmak için kendi varlıklarınızı tanımlayabilir veya önceden oluşturulmuş varlıkları kullanabilirsiniz.

|İfade|Varlık|Veriler|
|--|--|--|
|New York 'a 3 bilet satın alın|Önceden oluşturulmuş sayı<br>Hedef|3<br>New York|


### <a name="entities-are-optional-but-recommended"></a>Varlıklar isteğe bağlıdır ancak önerilir

[Amaçlar](luis-concept-intent.md) gerekir, varlıklar isteğe bağlıdır. Uygulamanızdaki her kavram için varlık oluşturmanız gerekmez, ancak yalnızca istemci uygulamanın verileri ihtiyacı olan veya varlığın başka bir varlığa veya amaca bir ipucu veya sinyal görevi gördüğü durumlar için.

Uygulamanız geliştirdiğinden ve veriler için yeni bir gereksinim belirlendiğinde, daha sonra LUO modelinize uygun varlıkları ekleyebilirsiniz.

## <a name="entity-compared-to-intent"></a>Amaca kıyasla varlık

Varlık _, söylenişi içindeki_bir veri kavramını temsil eder. Amaç, _Tüm söyliği_sınıflandırır.

Aşağıdaki dört kuralı göz önünde bulundurun:

|İfade|Amaç tahmin edildi|Ayıklanan varlıklar|Açıklama|
|--|--|--|--|
|Yardım|Yardım|-|Ayıklanacak bir şey yok.|
|Bir şey gönder|Sendbir şey|-|Ayıklanacak bir şey yok. Modelin bu bağlamda Ayıklanacak gerekli bir özelliği yok `something` ve belirtilen alıcı yok.|
|Emre 'yi mevcut gönder|Sendbir şey|`Bob`, `present`|Model, `Bob` önceden oluşturulmuş varlığın gerekli bir özelliğini ekleyerek ayıklar `personName` . Ayıklamak için makine tarafından öğrenilen bir varlık kullanıldı `present` .|
|Emre 'nin bir çikolata kutusunu Gönder|Sendbir şey|`Bob`, `box of chocolates`|İki önemli veri parçası `Bob` ve, `box of chocolates` makine tarafından öğrenilen varlıklar tarafından ayıklandı.|

## <a name="design-entities-for-decomposition"></a>Ayrıştırma için varlıkları tasarlama

Makine ile öğrenilen varlıklar, uygulama şemanızı, büyük bir kavramı alt varlıklara bölmek için, ayrıştırma için tasarlamanıza olanak sağlar.

Ayrıştırma için tasarlamak, LUSıS 'nin istemci uygulamanıza derin bir varlık çözümlemesi döndürmesini sağlar. Bu, istemci uygulamanızın iş kurallarına odaklanılmasını ve veri çözünürlüğünü LUO 'ya bırakmasını sağlar.

Makine tarafından öğrenilen bir varlık, örnek söylemeler aracılığıyla öğrenilen bağlamı temel alır.

[**Makine tarafından öğrenilen varlıklar**](tutorial-machine-learned-entity.md) en üst düzey ayıklayıcıları. Alt varlıklar, makine tarafından öğrenilen varlıkların alt varlıklarıdır.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Varlık türleri

Üst öğe için bir alt varlık, makine tarafından öğrenilen bir varlık olmalıdır. Alt varlık, makine tarafından öğrenilen bir varlığı [özellik](luis-concept-feature.md)olarak kullanabilir.

Verilerin ayıklanabilmesi ve ayıklandıktan sonra nasıl temsil edilebilmesi gerektiğine bağlı olarak varlığı seçin.

|Varlık türü|Amaç|
|--|--|
|[**Makine tarafından öğrenilen**](tutorial-machine-learned-entity.md)|Etiketlenmiş örneklerden öğrenilen iç içe geçmiş karmaşık verileri ayıklayın. |
|[**Listele**](reference-entity-list.md)|Öğelerin listesi ve **tam metin eşleşmesi**ile ayıklanan eş anlamlılar.|
|[**Model. any**](#patternany-entity)|Varlık sonunu bulmanın zor olduğu varlık, varlığın serbest biçimli olduğundan belirlenir. Yalnızca [desenlerle](luis-concept-patterns.md)kullanılabilir.|
|[**Önceden oluşturulmuş**](luis-reference-prebuilt-entities.md)|URL veya e-posta gibi belirli tür verileri ayıklamak zaten eğitildi. Bu önceden oluşturulmuş varlıkların bazıları açık kaynaklı [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text) projesinde tanımlanmıştır. Belirli bir kültür veya varlığınız Şu anda desteklenmiyorsa, projeye katkıda bulunun.|
|[**Normal ifade**](reference-entity-regular-expression.md)|**Tam metin eşleşmesi**için normal ifade kullanır.|

## <a name="extracting-contextually-related-data"></a>Bağlamsal olarak ile ilgili veriler ayıklanıyor

Söylenişi, verilerin anlamı, söylenişi içindeki bağlamı temel alan bir varlığın iki veya daha fazla örneğini içerebilir. İki coğrafi konumu, kaynağı ve hedefi olan bir uçuş sağlayan bir örnek, bir örnektir.

`Book a flight from Seattle to Cairo`

İki konumun, Bilet satın almanın tamamlanabilmesi için her konumun türünü bilmesi için, istemci uygulamanın her bir konumun türünü bilmesi için ayıklanmaları gerekir.

Kaynak ve hedefi ayıklamak için, Bilet siparişi makine tarafından öğrenilen varlığın bir parçası olarak iki alt varlık oluşturun. Alt varlıkların her biri için, geographyV2 kullanan gerekli bir özellik oluşturun.

<a name="using-component-constraints-to-help-define-entity"></a>
<a name="using-subentity-constraints-to-help-define-entity"></a>

### <a name="using-required-features-to-constrain-entities"></a>Varlıkları kısıtlamak için gerekli özellikleri kullanma

[Gerekli özellikler](luis-concept-feature.md) hakkında daha fazla bilgi edinin

## <a name="patternany-entity"></a>Pattern.any varlığı

Bir model. any yalnızca bir [düzende](luis-concept-patterns.md)mevcuttur.

<a name="if-you-need-more-than-the-maximum-number-of-entities"></a>
## <a name="exceeding-app-limits-for-entities"></a>Varlıklar için uygulama sınırlarını aşma

[Sınırdan](luis-limits.md#model-limits)daha fazlasına ihtiyacınız varsa desteğe başvurun. Bunu yapmak için sisteminizle ilgili ayrıntılı bilgiler toplayın, [Luo](luis-reference-regions.md#luis-website) Web sitesine gidin ve ardından **destek**' i seçin. Azure aboneliğiniz destek hizmetleri içeriyorsa, [Azure teknik desteği](https://azure.microsoft.com/support/options/)'ne başvurun.

## <a name="entity-prediction-status"></a>Varlık tahmin durumu

Lua portalı, varlığın örnek bir değer için seçtiğiniz varlıktan farklı bir varlık tahmini ne zaman olduğunu gösterir. Bu farklı puan, geçerli eğitilen modele dayalıdır. Aşağıdakilerden birini veya birkaçını kullanarak eğitim hatalarını çözmek için bu bilgileri kullanın:
* Varlığın kavramını belirlemesine yardımcı olmak için varlık için bir [özellik](luis-concept-feature.md) oluşturun
* Varlıkla daha fazla [örnek](luis-concept-utterance.md) ekleme ve etiket ekleyin
* Tahmin uç noktasında alınan her türlü, varlığın kavramını belirlemenize yardımcı olabilecek, [etkin öğrenme önerilerini gözden geçirin](luis-concept-review-endpoint-utterances.md) .

## <a name="next-steps"></a>Sonraki adımlar

[Tebrikler ilgili](luis-concept-utterance.md)kavramları öğrenin.

LUSıS uygulamanıza varlık ekleme hakkında daha fazla bilgi edinmek için bkz. [varlık ekleme](luis-how-to-add-entities.md) .

Bkz. Öğretici: makine tarafından öğrenilen varlığı kullanarak bir noktadan yapılandırılmış verilerin nasıl ayıklanacağını öğrenmek için [Language Understanding (LUA) içindeki makine tarafından öğrenilen varlıklara sahip kullanıcı aracılığıyla yapılandırılmış verileri ayıklama](tutorial-machine-learned-entity.md) .

