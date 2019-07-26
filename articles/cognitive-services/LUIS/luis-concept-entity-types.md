---
title: Varlık türleri
titleSuffix: Language Understanding - Azure Cognitive Services
description: "Varlıklar, utterance 'ten veri ayıklar. Varlık türleri, verilerin tahmin edilebilir bir şekilde ayıklanmasını sağlar. İki tür varlık vardır: makine tarafından öğrenilen ve makine tarafından öğrenilmemiş. Ne tür bir varlık ile birlikte çalıştığınızı bilmeniz önemlidir."
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: a5a3ba8c25107317e7c47ee358f9a6ebe7d4556f
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479108"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>LUSıS 'de varlık türleri ve amaçları

Varlıklar, utterance 'ten veri ayıklar. Varlık türleri, verilerin tahmin edilebilir bir şekilde ayıklanmasını sağlar. İki tür varlık vardır: makine tarafından öğrenilen ve makine tarafından öğrenilmemiş. Ne tür bir varlık ile birlikte çalıştığınızı bilmeniz önemlidir. 

## <a name="entity-compared-to-intent"></a>Intent'e karşılaştırıldığında varlık

Varlık, bir sözcük veya tümcecik ayıklanan istediğiniz utterance içinde temsil eder. Bir utterance birçok varlığın veya hiçbiri hiç içerebilir. Bir istemci uygulamasının görevini gerçekleştirmesi veya kullanıcıya sunmak için birkaç seçenek kılavuzu olarak kullanması gerekebilir. 

Bir varlık:

* Benzer nesnelerin (Yerlerim, şeyler, kişiler, olaylar veya kavramlar) bir koleksiyonunu içeren bir sınıfı temsil eder. 
* Amaç ile ilgili bilgileri açıklar


Örneğin, bir haber arama uygulaması "konu", "kaynak", "anahtar" ve "yayımlama tarihi" Haberler için arama anahtar veri olan gibi varlıklar içerebilir. Seyahat kayıt uygulamasında, "konum", "Tarih", "Airline", "seyahat sınıfı" ve "Bilet", uçuş kaydı için önemli bilgiler ("Book uçuş" hedefi ile ilgilidir).

Buna karşılık olarak amaç tüm utterance tahminini temsil eder. 

## <a name="entities-help-with-data-extraction-only"></a>Varlıklar yalnızca veri ayıklama ile ilgili yardım

Varlıkları yalnızca varlık ayıklama amacına göre etiketleyebilir veya işaretlersiniz, amaç tahminiyle ilgili değildir.

## <a name="entities-represent-data"></a>Varlık verilerini temsil eder.

Utterance çekmek için istediğiniz verilerin varlıklardır. Bu, bir ad, tarih, ürün adı veya herhangi bir kelimelerin grubu olabilir. 

|İfade|Varlık|Veriler|
|--|--|--|
|New York 3 bilet satın alma|Önceden oluşturulmuş numarası<br>Location.Destination|3<br>New York|
|5 Mart Londra New York'tan bilet satın alma|Location.Origin<br>Location.Destination<br>Önceden oluşturulmuş datetimeV2|New York<br>Londra<br>5 Mart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Varlıkları isteğe bağlıdır ancak uygulanması önemle önerilir.

Varlıklar, amacı gerekli olsa da, isteğe bağlıdır. Uygulamanızdaki her kavram için varlık oluşturmanız gerekmez, ancak yalnızca istemci uygulamasının işlem yapması için gerekli olanlar için. 

Botunuzun devam etmesi için gerekli ayrıntıları, konuşma yoksa, bunları eklemeniz gerekmez. Uygulamanız geliştikçe daha sonra ekleyebilirsiniz. 

Bilgileri nasıl kullanacağınız konusunda emin değilseniz, [datetimeV2](luis-reference-prebuilt-datetimev2.md), [Ordinal](luis-reference-prebuilt-ordinal.md), [e-posta](luis-reference-prebuilt-email.md)ve [telefon numarası](luis-reference-prebuilt-phonenumber.md)gibi yaygın olarak önceden oluşturulmuş birkaç varlık ekleyin.

## <a name="label-for-word-meaning"></a>Word anlamı etiketi

Word choice veya word düzenleme aynıdır, ancak aynı şeyi anlamına gelmez, bu varlıkla etiket değil. 

Aşağıdaki konuşma, word `fair` olan bir eş sesli sözcük. Aynı yazıldığından, ancak farklı bir anlama sahiptir:

|İfade|
|--|
|Ne tür bir ilçe fairs yapıldığını Seattle alanında bu yaz?|
|Geçerli derecelendirme Seattle gözden geçirilmek üzere adil mi?|

Tüm olay verileri bulmak için bir olay varlık istediyseniz, word etiket `fair` ilk utterance, ancak ikinci içinde değil.

## <a name="entities-are-shared-across-intents"></a>Varlıkları amaçları arasında paylaşılır.

Varlıkları amaçları arasında paylaşılır. Bunlar herhangi bir tek ıntent'e ait değil. Amaçlar ve varlıklar anlam ile ilişkilendirilebilir, ancak özel bir ilişki değildir.

"İstanbul için bir bilet bileti" başlığı altında "Paris", konuma başvuran bir varlıktır. Kullanıcının utterliği içinde bahsedilen varlıkları öğrenerek lu, istemci uygulamanızın, kullanıcının isteğini yerine getirmek için gerçekleştirilecek belirli eylemleri seçmesini sağlar.

## <a name="mark-entities-in-none-intent"></a>Varlıkları hiçbiri amaç halinde işaretle

**Hiçbiri** amacı dahil tüm amaçlar, mümkün olduğunda varlık olarak işaretlenmiş olmalıdır. Bu varlıkların içinde konuşma nerede ve ne varlıkları sözcüklerdir hakkında daha fazla LUIS yardımcı olur. 

## <a name="entity-status-for-predictions"></a>Tahminler elde etmek için varlık durumu

LUıS portalı, bir örnekteki varlık işaretli varlıktan farklı olduğunda ya da başka bir varlığa ne kadar yakın olduğunu ve bu nedenle belirsiz olduğunu bildirir. Bu, örnekte yer aldığı kırmızı alt çizgiyle belirtilir. 

Daha fazla bilgi için bkz. [varlık durumu tahminleri](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Varlık türleri

LUSıS birçok varlık türü sunar. Verilerin ayıklanabilmesi ve ayıklandıktan sonra nasıl temsil edilebilmesi gerektiğine bağlı olarak varlığı seçin.

Varlık öğrenimi ile, bu varlık, varlığın utterde nasıl göründüğünü öğrenmeye devam etmesine olanak tanır. Varlıklar, tam metin veya normal bir ifadeyle eşleşen makine öğrenimi olmadan ayıklanabilir. Desenlerdeki varlıklar karışık bir uygulamayla ayıklanabilir. 

Varlık ayıklandıktan sonra, varlık verileri tek bir bilgi birimi olarak temsil edilebilir veya istemci uygulamanın kullanabileceği bir bilgi birimi oluşturmak için diğer varlıklarla birlikte birleştirilebilir.

|Makine tarafından öğrenilen|Işaret edebilir|Öğretici|Örnek<br>Yanıt|Varlık türü|Amaç|
|--|--|--|--|--|--|
|✔|✔|[✔](luis-tutorial-composite-entity.md)|[✔](luis-concept-data-extraction.md#composite-entity-data)|[**Bileşik**](#composite-entity)|Varlık türünden bağımsız olarak varlıkların gruplandırılması.|
|||[✔](luis-quickstart-intent-and-list-entity.md)|[✔](luis-concept-data-extraction.md#list-entity-data)|[**Listele**](#list-entity)|Öğelerin listesi ve tam metin eşleşmesi ile ayıklanan eş anlamlılar.|
|Karma||[✔](luis-tutorial-pattern.md)|[✔](luis-concept-data-extraction.md#patternany-entity-data)|[**Model. any**](#patternany-entity)|Varlık sonunun belirlenmesi zor olan varlık.|
|||[✔](luis-tutorial-prebuilt-intents-entities.md)|[✔](luis-concept-data-extraction.md#prebuilt-entity-data)|[**Önceden oluşturulmuş**](#prebuilt-entity)|Çeşitli veri türlerini ayıklamak için zaten eğitildi.|
|||[✔](luis-quickstart-intents-regex-entity.md)|[✔](luis-concept-data-extraction.md#regular-expression-entity-data)|[**Normal Ifade**](#regular-expression-entity)|Metinle eşleştirmek için normal ifade kullanır.|
|✔|✔|[✔](luis-quickstart-primary-and-secondary-data.md)|[✔](luis-concept-data-extraction.md#simple-entity-data)|[**MPLE**](#simple-entity)|Sözcük veya tümcecik içinde tek bir kavram içerir.|

Yalnızca makine tarafından öğrenilen varlıkların örnek utslarında işaretlenmesi gerekir. Makine tarafından öğrenilen varlıklar, [uç nokta sorgularıyla](luis-concept-test.md#endpoint-testing) test edildiğinde ve [uç nokta](luis-how-to-review-endoint-utt.md)gelişlerini inceleyerek en iyi şekilde çalışır. 

Model. tüm varlıkların, amaç Kullanıcı örnekleri değil, [model](luis-how-to-model-intent-pattern.md) şablonu örneklerinde işaretlenmesi gerekir. 

Karışık varlıklar, varlık algılama yöntemlerinin bir birleşimini kullanır.

## <a name="machine-learned-entities-use-context"></a>Makine tarafından öğrenilen varlıklar bağlam kullanır

Makine tarafından öğrenilen varlıklar, utterde bağlamdaki içerikten öğrenilir. Bu, yerleştirme Çeşitlemelerinde önemli bir değer sağlar. 

## <a name="non-machine-learned-entities-dont-use-context"></a>Makine tarafından öğrenilen varlıklar bağlam kullanmaz

Aşağıdaki makine olmayan öğrenilmiş varlıklar, varlıkları eşleştirirken hesaba terance bağlamı almaz: 

* [Önceden oluşturulmuş varlıklar](#prebuilt-entity)
* [Regex varlıkları](#regular-expression-entity)
* [Varlıklar listesi](#list-entity) 

Bu varlıklar, etiketleme veya modeli eğitme gerektirmez. Varlığı ekledikten veya yapılandırdıktan sonra, varlıklar ayıklanır. Zorunluluğunu getirir, bu varlıkların aşırı eşleştirilebileceği, burada bağlamın hesapta alınması durumunda eşleşme yapılmamalıdır. 

Bu durum, yeni modellerdeki liste varlıklarıyla sık sık gerçekleşir. Modelinizi bir liste varlığıyla oluşturup test edersiniz, ancak modelinizi yayımladığınızda ve uç noktadan sorgular aldığınızda, bağlam eksikliği nedeniyle modelinizin aşırı eşleşme olduğunu fark etmiş olursunuz. 

Sözcükleri veya tümceleri eşleştirmek ve hesabı hesaba çekmek istiyorsanız iki seçeneğiniz vardır. Birincisi, bir tümcecik listesiyle eşleştirilmiş basit bir varlık kullanmaktır. Tümcecik listesi eşleştirme için kullanılmayacak ancak bunun yerine görece benzer sözcüklerin (değiştirilebilir liste) sinyaline yardımcı olacaktır. Tümcecik listesinin çeşitlemeleri yerine tam bir eşleşmelidir, aşağıda açıklandığı gibi bir rol içeren bir liste varlığı kullanın.

### <a name="context-with-non-machine-learned-entities"></a>Makine tarafından öğrenilen varlıklar ile bağlam

Makine tarafından öğrenilen varlıkların yerine, bu bağlamın bağlamını kullanmak istiyorsanız, [rolleri](luis-concept-roles.md)kullanmanız gerekir.

[Önceden oluşturulmuş varlıklar](#prebuilt-entity), [Regex](#regular-expression-entity) varlıkları veya [liste](#list-entity) varlıkları gibi makine tarafından öğrenilen bir varlığınız varsa, istediğiniz örnekten daha fazla eşleşen bir varlık oluşturmayı düşünün. Bir rol aradığınız şeyi yakalar ve bir rol ne aradıklarınızı yakalayacaktır. Her iki sürümün de örnek söylenecek şekilde etiketlenmesi gerekir.  

## <a name="composite-entity"></a>Bileşik varlık

[Bileşik bir varlık](reference-entity-composite.md) , önceden oluşturulmuş varlıklar, basit, normal ifade ve liste varlıkları gibi diğer varlıklardan oluşur. Ayrı varlıklar, tam bir varlık oluşturur. 

## <a name="list-entity"></a>Liste varlığı

[Liste varlıkları](reference-entity-list.md) , sabit ve kapalı bir ilgili sözcük kümesini eş anlamlılarıyla birlikte temsil eder. LUIS, liste varlıkları için ek değerler bulmaz. Kullanım **önerilir** yeni sözcükleri sunabileceği önerileri görmek için özellik geçerli listede bağlı. Birden fazla liste varlığı ile aynı değeri varsa, her varlık uç nokta sorguda döndürülür. 

## <a name="patternany-entity"></a>Pattern.Any varlık

[Desen. any](reference-entity-pattern-any.md) , varlığın nerede başladığını ve bittiğini işaretlemek için yalnızca bir desen şablonunda kullanılan değişken uzunluklu bir yer tutucudur.  

## <a name="prebuilt-entity"></a>Önceden oluşturulmuş varlık

Önceden oluşturulmuş varlıklar, e-posta, URL ve telefon numarası gibi yaygın kavramları temsil eden yerleşik türlerdir. Önceden oluşturulmuş varlık adları ayrılmıştır. Uygulamaya eklenen [tüm önceden oluşturulmuş varlıklar](luis-prebuilt-entities.md) , bir noktada bulunduklarında Endpoint tahmin sorgusuna döndürülür. 

Varlık, şu durumlarda iyi bir uyum:

* Veriler, dil külveriniz için önceden oluşturulmuş varlıklar tarafından desteklenen ortak kullanım durumuyla eşleşir. 

Önceden oluşturulmuş varlıklar dilediğiniz zaman eklenebilir ve kaldırılabilir.

![Sayı önceden oluşturulmuş varlık](./media/luis-concept-entities/number-entity.png)

[Öğretici](luis-tutorial-prebuilt-intents-entities.md)<br>
[Varlık için örnek JSON yanıtı](luis-concept-data-extraction.md#prebuilt-entity-data)

Bu önceden oluşturulmuş varlıkların bazıları açık kaynaklı [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text) projesinde tanımlanmıştır. Belirli bir kültürün veya varlık şu anda desteklenmemektedir, projeye katkıda bulunur. 

### <a name="troubleshooting-prebuilt-entities"></a>Önceden oluşturulmuş varlıkların sorunlarını giderme

Lub portalında, özel varlığınızın yerine önceden oluşturulmuş bir varlık etiketlenmişse, bunun nasıl düzeltileceğini gösteren birkaç seçeneğiniz vardır.

Uygulamaya eklenen önceden oluşturulmuş varlıklar, aynı metin için özel varlıkları ayıklamalıdır olsa bile _her zaman_ döndürülür. 

#### <a name="change-tagged-entity-in-example-utterance"></a>Etiketli varlığı örnek utde değiştirme

Önceden oluşturulmuş varlık özel varlıkla aynı metin veya belirteçlere sahip olursa, örnekteki metinde bulunan metni seçin ve etiketli utteryi değiştirin. 

Önceden oluşturulmuş varlık özel varlıkınıza göre daha fazla metin veya belirtece etiketlediyseniz, bunun nasıl düzeltileceğini gösteren birkaç seçeneğiniz vardır:

* [Örnek söylenişi yöntemini kaldır](#remove-example-utterance-to-fix-tagging)
* [Önceden oluşturulmuş varlık yöntemini kaldır](#remove-prebuilt-entity-to-fix-tagging)

#### <a name="remove-example-utterance-to-fix-tagging"></a>Etiketlemesini onarmak için örnek söylenişi 'yi kaldırın 

İlk seçiminiz, örnek utüterliği ortadan kaldırandır. 

1. Örnek utterliği silin.
1. Uygulamayı yeniden eğitme. 
1. Tam bir örnek olarak, önceden oluşturulmuş bir varlık olarak işaretlenen varlık olan sözcüğü veya tümceciği geri ekleyin. Sözcüğe veya ifadeye hala önceden oluşturulmuş varlık de işaretlenmiş olur. 
1. **Amaç** sayfasında bulunan örnekteki varlık ' ı seçin ve özel varlığınızla değiştirin ve yeniden eğitme yapın. Bu, LUSıS 'in bu metni kullanan herhangi bir örnek için bu tam metni önceden oluşturulmuş varlık olarak işaretlemesini önlemelidir. 
1. Özgün örnek tamamını amacına geri ekleyin. Özel varlık önceden oluşturulmuş varlık yerine işaretlenmeye devam etmelidir. Özel varlık işaretli değilse, söz konusu metne daha fazla örnek eklemeniz gerekir.

#### <a name="remove-prebuilt-entity-to-fix-tagging"></a>Etiketlemeyi onarmak için önceden oluşturulmuş varlığı kaldırma

1. Uygulamadan önceden oluşturulmuş varlığı kaldırın. 
1. **Amaç** sayfasında, örnekte yer alan özel varlığı işaretleyin.
1. Uygulamayı eğitin.
1. Önceden oluşturulmuş varlığı uygulamaya geri ekleyin ve uygulamayı eğitme. Bu düzeltilme, önceden oluşturulmuş varlığın bir bileşik varlığın parçası olmadığını varsayar.

## <a name="regular-expression-entity"></a>Normal ifade varlığı 

[Normal ifade varlığı](reference-entity-regular-expression.md) , sağladığınız normal ifade düzenine göre bir varlığı ayıklar.

## <a name="simple-entity"></a>Basit varlık

A [varlığın](reference-entity-simple.md) bir makine öğrenilen değerdir. Bir sözcük veya tümcecik olabilir.
## <a name="entity-limits"></a>Varlık sınırları

Gözden geçirme [sınırları](luis-boundaries.md#model-boundaries) anlamak için bir model ekleyebilirsiniz kaç her varlık türü.

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Varlıklar, en fazla sayısından daha ihtiyacınız varsa 

Birleşik varlıkları varlık rolleriyle birlikte kullanmanız gerekebilir.

Bileşik varlık, bir bütün parçalarını temsil eder. Örneğin, PlaneTicketOrder adlı bileşik bir varlık alt varlıklar Havayolu, hedef DepartureCity DepartureDate ve PlaneTicketClass olabilir.

Luda, makine tarafından öğrenilen, ancak LUSıS uygulamanızın sabit bir değer listesi belirlemesine izin veren liste varlık türünü de sağlar. Bkz: [LUIS sınırları](luis-boundaries.md) sınırları varlık türleri gözden geçirmek için başvuru. 

Bu varlıkları kabul ediyorsanız ve sınırdan daha fazlasına ihtiyaç duyuyorsanız, desteğe başvurun. Bunu yapmak için sisteminizin hakkında ayrıntılı bilgi toplamak, Git [LUIS](luis-reference-regions.md#luis-website) Web sitesine gidin ve ardından **Destek**. Destek Hizmetleri Azure aboneliğinize dahildir, başvurun [Azure teknik desteğine](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Sonraki adımlar

Kavramları iyi hakkında bilgi edinin [konuşma](luis-concept-utterance.md). 

Bkz: [varlık Ekle](luis-how-to-add-entities.md) LUIS uygulamanızı varlıklar ekleme hakkında daha fazla bilgi için.
