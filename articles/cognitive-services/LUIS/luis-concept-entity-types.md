---
title: Varlık türleri-LUSıS
titleSuffix: Azure Cognitive Services
description: "Varlıklar, utterance 'ten veri ayıklar. Varlık türleri, verilerin tahmin edilebilir bir şekilde ayıklanmasını sağlar. İki tür varlık vardır: makine tarafından öğrenilen ve makine tarafından öğrenilmemiş. Ne tür bir varlık ile birlikte çalıştığınızı bilmeniz önemlidir."
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: diberry
ms.openlocfilehash: 6ee156efb5512c92d86ba05513b6a2b91df4eae8
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78393862"
---
# <a name="entities-and-their-purpose-in-luis"></a>ISIS 'de varlıklar ve amaçları

Varlıkların birincil amacı, istemci uygulamaya tahmin edilebilir veri ayıklama hakkı vermektir. _İsteğe bağlı_, ikincil amaç, tanımlayıcı içeren amaç veya diğer varlıkların tahminini artırmak için kullanılır.

İki tür varlık vardır:

* makine tarafından öğrenilen-bağlam
* makine tarafından öğrenilmeyen-tam metin eşleşmeleri, model eşleşmeleri veya önceden oluşturulmuş varlıklar tarafından algılama için

Makine tarafından öğrenilen varlıklar, en geniş veri ayıklama seçeneği yelpazesi sağlar. Makine tarafından öğrenilen varlıklar, metin eşleştirme ile çalışır ve makine tarafından öğrenilen bir varlıkta bağımsız olarak veya [kısıtlama](#design-entities-for-decomposition) olarak kullanılabilir.

## <a name="entities-represent-data"></a>Varlık verilerini temsil eder.

Varlıklar, adlar, tarihler, ürün adları veya herhangi bir önemli sözcük grubu gibi, utterklerden çekmek istediğiniz verilerden oluşur. Bir utterance birçok varlığın veya hiçbiri hiç içerebilir. İstemci uygulaması, görevini gerçekleştirmek için verilere gereksinim duyuyor _olabilir_ .

Varlıkların, bir modeldeki her amaç için tüm eğitimlere sürekli olarak etiketlenmesi gerekir.

 [DatetimeV2](luis-reference-prebuilt-datetimev2.md), [Ordinal](luis-reference-prebuilt-ordinal.md), [e-posta](luis-reference-prebuilt-email.md)ve [telefon numarası](luis-reference-prebuilt-phonenumber.md)gibi yaygın kavramlara zaman kazanmak için kendi varlıklarınızı tanımlayabilir veya önceden oluşturulmuş varlıkları kullanabilirsiniz.

|İfade|Varlık|Veriler|
|--|--|--|
|New York 3 bilet satın alma|Önceden oluşturulmuş numarası<br>Location.Destination|3<br>New York|
|5 Mart Londra New York'tan bilet satın alma|Location.Origin<br>Location.Destination<br>Önceden oluşturulmuş datetimeV2|New York<br>Londra<br>5 Mart 2018|

### <a name="entities-are-optional"></a>Varlıklar isteğe bağlıdır

Varlıklar, amacı gerekli olsa da, isteğe bağlıdır. Uygulamanızdaki her kavram için varlık oluşturmanız gerekmez, ancak yalnızca istemci uygulamasının işlem yapması için gerekli olanlar için.

Utterklerinizin istemci uygulaması için gereken verileri yoksa, varlık eklemeniz gerekmez. Uygulamanız geliştirdiğinden ve veriler için yeni bir gereksinim belirlendiğinde, daha sonra LUO modelinize uygun varlıkları ekleyebilirsiniz.

## <a name="entity-compared-to-intent"></a>Intent'e karşılaştırıldığında varlık

Varlık, ayıklanarak ayıklanmasını istediğiniz bir veri kavramını temsil eder.

Bir söylenişi, isteğe bağlı olarak varlıklar içerebilir. Karşılaştırmayla, bir söylenişi için amaç tahmini _gereklidir_ ve tüm söylik 'i temsil eder. LU, örnek söyleyeni bir amaç içinde bulundurmaktır.

Aşağıdaki 4 göz önünde bulundurun:

|İfade|Tahmin hedefi|Ayıklanan varlıklar|Açıklama|
|--|--|--|--|
|Yardım|yardım|-|Ayıklanacak bir şey yok.|
|Bir şey gönder|Sendbir şey|-|Ayıklanacak bir şey yok. Model, bu bağlamda `something` ayıklamak üzere eğitilmedi ve herhangi bir alıcı yok.|
|Emre 'yi mevcut gönder|Sendbir şey|`Bob`, `present`|Model, `Bob`adı ayıklanmış olan [PersonName](luis-reference-prebuilt-person.md) önceden oluşturulmuş varlıkla eğitildi. `present`ayıklamak için makine tarafından öğrenilen bir varlık kullanıldı.|
|Emre 'nin bir çikolata kutusunu Gönder|Sendbir şey|`Bob`, `box of chocolates`|`Bob` ve `box of chocolates`iki önemli veri parçası varlıklar tarafından ayıklandı.|

## <a name="design-entities-for-decomposition"></a>Ayrıştırma için varlıkları tasarlama

En üst düzey varlığınızı makine tarafından öğrenilen bir varlık haline getirmek için iyi bir varlık tasarımdır. Bu, en üst düzey varlığı istemci uygulaması için gereken parçalara ayırmak için zaman içinde varlık tasarımınızda, isteğe bağlı olarak **kısıtlamalar** ve **tanımlayıcılarla**yapılan değişikliklere izin verir.

Ayrıştırma için tasarlamak, LUSıS 'nin istemci uygulamanıza derin bir varlık çözümlemesi döndürmesini sağlar. Bu, istemci uygulamanızın iş kurallarına odaklanılmasını ve veri çözünürlüğünü LUO 'ya bırakmasını sağlar.

### <a name="machine-learned-entities-are-primary-data-collections"></a>Makine tarafından öğrenilen varlıklar birincil veri koleksiyonlarıdır

[**Makine tarafından öğrenilen varlıklar**](tutorial-machine-learned-entity.md) , en üst düzey veri birimidir. Alt bileşenler, makine tarafından öğrenilen varlıkların alt varlıklarıdır.

Makine tarafından öğrenilen bir varlık, eğitim dıkları aracılığıyla öğrenilmiş içeriğe göre tetiklenir. **Kısıtlamalar** , bir [liste](reference-entity-list.md) veya [Regex](reference-entity-regular-expression.md)gibi makine tarafından öğrenilen bir varlığın tam metin eşleştirme tanımına bağlı olarak, tetiklemenin daha fazla kısıtlanacak makine tarafından öğrenilen bir varlığa uygulanan isteğe bağlı kurallardır. Örneğin, `size` makine tarafından öğrenilen bir varlık, `size` varlığını yalnızca `sizeList` varlığı içinde yer alan değerler ile karşılaşıldığında tetiklemek üzere kısıtlayan bir `sizeList` listesi varlığının kısıtlamasına sahip olabilir.

[**Tanımlayıcılar**](luis-concept-feature.md) , tahmine yönelik sözcüklerin veya tümceciklerin uygunluğunu artırmak için uygulanan özelliklerdir. Bu değerler, amacı veya varlığı *tanımlamakta* kullanıldıklarından *tanımlayıcı* olarak adlandırılırlar. Tanımlayıcılar, önemli kelimeler veya veri özniteliklerini (örneğin, LUVE öğrendiği önemli sözcükler veya ifadeler) ayırt eder.

LUSıS uygulamanızda bir ifade listesi özelliği oluşturduğunuzda, varsayılan olarak genel olarak etkinleştirilir ve tüm amaçlar ve varlıklar arasında eşit olarak uygulanır. Ancak, tümcecik listesini makine tarafından öğrenilen bir varlığın (veya *modelinin*) tanımlayıcı (özellik) olarak uygularsanız, kapsamı yalnızca o modele uygulanabilir ve artık diğer tüm modellerle kullanılmaz. Bir model için tanımlayıcı olarak bir ifade listesi kullanmak, uygulandığı modelin doğruluğunu ele alarak ayrışmaya yardımcı olur.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Varlık türleri

Verilerin ayıklanabilmesi ve ayıklandıktan sonra nasıl temsil edilebilmesi gerektiğine bağlı olarak varlığı seçin.

|Varlık türü|Amaç|
|--|--|
|[**Makine tarafından öğrenilen**](tutorial-machine-learned-entity.md)|Makine tarafından öğrenilen varlıklar, utterde bağlamdaki içerikten öğrenilir. Varlık türünden bağımsız olarak varlıkların üst gruplandırması. Bu, yerleştirme Çeşitlemelerinde önemli bir değer sağlar. |
|[**Listele**](reference-entity-list.md)|Öğelerin listesi ve **tam metin eşleşmesi**ile ayıklanan eş anlamlılar.|
|[**Model. any**](reference-entity-pattern-any.md)|Varlık sonunun belirlenmesi zor olan varlık. |
|[**Önceden oluşturulmuş**](luis-reference-prebuilt-entities.md)|URL veya e-posta gibi belirli tür verileri ayıklamak zaten eğitildi. Bu önceden oluşturulmuş varlıkların bazıları açık kaynaklı [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text) projesinde tanımlanmıştır. Belirli bir kültürün veya varlık şu anda desteklenmemektedir, projeye katkıda bulunur.|
|[**Normal Ifade**](reference-entity-regular-expression.md)|**Tam metin eşleşmesi**için normal ifade kullanır.|

## <a name="extracting-contextually-related-data"></a>Bağlamsal olarak ile ilgili veriler ayıklanıyor

Söylenişi, verilerin anlamı, söylenişi içindeki bağlamı temel alan bir varlığın iki veya daha fazla örneğini içerebilir. İki konum, kaynak ve hedef olan bir uçuşmanın bir örneği olan bir örnek bir örnektir.

`Book a flight from Seattle to Cairo`

`location` varlığın iki örneği ayıklanmalıdır. Bilet satın alma işleminin tamamlanabilmesi için istemci uygulamanın konumun türünü bilmesi gerekir.

Bağlamsal olarak ile ilgili verileri ayıklamak için iki teknik vardır:

 * `location` varlık, makine tarafından öğrenilen bir varlıktır ve `origin` ve `destination` yakalamak için iki alt bileşen varlığı kullanır (tercih edilen)
 * `location` varlık `origin` ve `destination` iki **rolünü** kullanır

Birden çok varlık bir söylenişi içinde bulunabilir ve kullanıldıkları bağlamın hiç önemi yoksa, ayrıştırma veya roller kullanılmadan ayıklanabilir. Örneğin, söylenişi bir konum listesi içeriyorsa `I want to travel to Seattle, Cairo, and London.`, her öğenin ek anlamı olmadığı bir listesidir.

### <a name="using-subcomponent-entities-of-a-machine-learned-entity-to-define-context"></a>Bağlam tanımlamak için makine tarafından öğrenilen bir varlığın alt bileşen varlıklarını kullanma

Bir uçuşın kayıt işlemini açıklayan verileri ayıklamak ve ardından üst düzey varlığı istemci uygulaması için gereken ayrı parçalara çıkarmak için [**makine tarafından öğrenilen bir varlık**](tutorial-machine-learned-entity.md) kullanabilirsiniz.

Bu örnekte `Book a flight from Seattle to Cairo`, üst düzey varlık `travelAction` ve `flight from Seattle to Cairo`ayıklamak için etiketlenebilir. Ardından, `origin` ve `destination`olarak adlandırılan iki alt bileşen varlığı, önceden oluşturulmuş `geographyV2` varlığına uygulanan bir kısıtlama ile oluşturulur. Eğitim arasları 'nda `origin` ve `destination` uygun şekilde etiketlidir.

### <a name="using-entity-role-to-define-context"></a>Bağlamı tanımlamak için varlık rolü kullanma

Rol, bir varlık için, utterance içindeki bağlamı temel alan adlandırılmış diğer addır. Bir rol, önceden oluşturulmuş veya özel varlık türü ile kullanılabilir ve her iki örnek de ve desenlerinde kullanılabilir. Bu örnekte `location` varlığının iki `origin` ve `destination` olması gerekir ve her ikisi de örnek söylerde işaretlenmelidir.

LUSıS `location` bulursa ve rolü tespit leyemiyorsa, konum varlığı yine de döndürülür. Kullanıcının ne tür bir konum olduğunu belirleyebilmek için istemci uygulamanın bir soru ile izlenmesi gerekir.


## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Varlıklar, en fazla sayısından daha ihtiyacınız varsa

Sınırdan daha fazlasına ihtiyacınız varsa desteğe başvurun. Bunu yapmak için sisteminizle ilgili ayrıntılı bilgiler toplayın, [Luo](luis-reference-regions.md#luis-website) Web sitesine gidin ve ardından **destek**' i seçin. Azure aboneliğiniz destek hizmetleri içeriyorsa, [Azure teknik desteği](https://azure.microsoft.com/support/options/)'ne başvurun.

## <a name="entity-prediction-status"></a>Varlık tahmin durumu

LUU portalı, bir örnekte varlığın seçtiğiniz varlıktan farklı bir varlık tahminiyle ne zaman olduğunu gösterir. Bu farklı puan, geçerli eğitilen modele dayalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Tebrikler ilgili](luis-concept-utterance.md)kavramları öğrenin.

LUSıS uygulamanıza varlık ekleme hakkında daha fazla bilgi edinmek için bkz. [varlık ekleme](luis-how-to-add-entities.md) .

Bkz. Öğretici: makine tarafından öğrenilen varlığı kullanarak bir noktadan yapılandırılmış verilerin nasıl ayıklanacağını öğrenmek için [Language Understanding (LUA) içindeki makine tarafından öğrenilen varlıklara sahip kullanıcı aracılığıyla yapılandırılmış verileri ayıklama](tutorial-machine-learned-entity.md) .
 
