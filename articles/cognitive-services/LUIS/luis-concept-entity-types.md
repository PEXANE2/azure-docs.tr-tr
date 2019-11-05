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
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 9dc26e50e1c0f43e816e422f0fee91a246ea04a9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73487592"
---
# <a name="entities-and-their-purpose-in-luis"></a>ISIS 'de varlıklar ve amaçları

Varlıkların birincil amacı, istemci uygulamaya tahmin edilebilir veri ayıklama hakkı vermektir. _İsteğe bağlı_, ikincil amaç, tanımlayıcılarla amaç tahminini artırmak için kullanılır. 

İki tür varlık vardır: 

* makine tarafından öğrenilen-bağlam
* makine tarafından öğrenilmeyen-tam metin eşleşmeleri için

En geniş veri ayıklama seçeneklerini sağladığından, her zaman makine tarafından öğrenilen bir varlıkla başlayın.

## <a name="entity-compared-to-intent"></a>Amaca kıyasla varlık

Varlık, ayıklanarak ayıklanmasını istediğiniz bir veri kavramını temsil eder. 

Aşağıdaki 3 gözetlerini göz önünde bulundurun:

|İfade|Ayıklanan veriler|Açıklama|
|--|--|--|
|`Help`|-|Ayıklanacak bir şey yok.|
|`Send Bob a present`|Bob, mevcut|Emre, görevi tamamlamak için kesinlikle önemlidir. Mevcut yeterli bilgi olabilir veya bot 'ın, bir takip eden soruda mevcut olduğunu açıklığa kavuşturmanız gerekebilir.|
|`Send Bob a box of chocolates.`|İki önemli veri parçası olan Bob ve Box çikolata, kullanıcının isteğini tamamlamak için önemlidir.|

Bir söylenişi birçok varlık içerebilir veya hiç yok. Bir istemci uygulamasının görevini gerçekleştirmesi için varlığa ihtiyacı _olabilir_ . 

Karşılaştırmayla, bir söylenişi için amaç tahmini _gereklidir_ ve tüm söylik 'i temsil eder. LU, örnek söyleyeni bir amaç içinde bulundurmaktır. İstemci uygulama için söylenişi 'in birincil amacı önemli değilse, tüm söyleyeni yok 'a ekleyin. 

Daha sonra uygulama yaşam döngüsünde daha sonra bulursanız, bunları kolayca yapabilirsiniz. Bu, yazarken yazılanların düzenlenmesi veya istemci uygulamasında tahmin edilen amaç kullanılması olabilir. 

İstemci uygulamasında tahmin edilen amacı kullanma gereksinimi yoktur, ancak tahmin uç noktası yanıtının bir parçası olarak döndürülür.

## <a name="entities-represent-data"></a>Varlıklar verileri temsil eder

Varlıklar, utterance 'ten çekmek istediğiniz veri. Bu bir ad, tarih, ürün adı veya herhangi bir sözcük grubu olabilir. 

|İfade|Varlık|Veriler|
|--|--|--|
|New York 'a 3 bilet satın alın|Önceden oluşturulmuş sayı<br>Location. Destination|3<br>New York|
|Yeni York 'tan Istanbul 'a 5 Mart 'ta bir bilet satın alın|Location. Origin<br>Location. Destination<br>Önceden oluşturulmuş datetimeV2|New York<br>Londra<br>5 Mart 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Varlıklar isteğe bağlıdır, ancak önemle önerilir

Amaçlar gerekir, varlıklar isteğe bağlıdır. Uygulamanızdaki her kavram için varlık oluşturmanız gerekmez, ancak yalnızca istemci uygulamasının işlem yapması için gerekli olanlar için. 

Aradıklarınızın ayrıntıları yoksa, gerek duyunuzun devam etmesi gerekir, bunları eklemeniz gerekmez. Uygulamanız geliştikçe, bunları daha sonra ekleyebilirsiniz. 

Bilgileri nasıl kullanacağınız konusunda emin değilseniz, [datetimeV2](luis-reference-prebuilt-datetimev2.md), [Ordinal](luis-reference-prebuilt-ordinal.md), [e-posta](luis-reference-prebuilt-email.md)ve [telefon numarası](luis-reference-prebuilt-phonenumber.md)gibi yaygın olarak önceden oluşturulmuş birkaç varlık ekleyin.

## <a name="design-entities-for-decomposition"></a>Ayrıştırma için varlıkları tasarlama

Makine tarafından öğrenilen bir varlıkla varlık tasarımınıza başlayın. Bu sayede, varlığınızın zaman içindeki kolay tasarım büyümesi ve değişiklikleri vardır. Varlık tasarımını tamamlamaya yönelik **kısıtlamalar** ve **tanımlayıcılar** içeren alt **bileşenleri** (alt varlıklar) ekleyin. 

Ayrıştırma için tasarlamak, LUSıS 'nin istemci uygulamanıza derin bir varlık çözümlemesi döndürmesini sağlar. Bu, istemci uygulamanızın iş kurallarına odaklanılmasını ve veri çözünürlüğünü LUO 'ya bırakmasını sağlar.

### <a name="machine-learned-entities-are-primary-data-collections"></a>Makine tarafından öğrenilen varlıklar birincil veri koleksiyonlarıdır

Makine tarafından öğrenilen varlıklar, en üst düzey veri birimidir. Alt bileşenler, makine tarafından öğrenilen varlıkların alt varlıklarıdır. 

**Kısıtlamalar** , verileri tanımlamak ve ayıklamak için kurallar uygulayan tam metin eşleştirme varlıklarıdır. **Tanımlayıcılar** , tahmine yönelik sözcüklerin veya tümceciklerin uygunluğunu artırmak için uygulanan özelliklerdir.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Varlık türleri

Verilerin ayıklanabilmesi ve ayıklandıktan sonra nasıl temsil edilebilmesi gerektiğine bağlı olarak varlığı seçin.

|varlık türü|Amaç|
|--|--|
|[**Makine tarafından öğrenilen**](#composite-entity)|Varlık türünden bağımsız olarak varlıkların üst gruplandırması. Makine tarafından öğrenilen varlıklar, utterde bağlamdaki içerikten öğrenilir. Bu, yerleştirme Çeşitlemelerinde önemli bir değer sağlar. |
|[**Listele**](#list-entity)|Öğelerin listesi ve **tam metin eşleşmesi**ile ayıklanan eş anlamlılar.|
|[**Model. any**](#patternany-entity)|Varlık sonunun belirlenmesi zor olan varlık. |
|[**Önceden oluşturulmuş**](#prebuilt-entity)|URL veya e-posta gibi belirli tür verileri ayıklamak zaten eğitildi. Bu önceden oluşturulmuş varlıkların bazıları açık kaynaklı [Tanıyıcılar-metin](https://github.com/Microsoft/Recognizers-Text) projesinde tanımlanmıştır. Belirli bir kültür veya varlığınız Şu anda desteklenmiyorsa, projeye katkıda bulunun.|
|[**Normal Ifade**](#regular-expression-entity)|**Tam metin eşleşmesi**için normal ifade kullanır.|

### <a name="entity-role-defines-context"></a>Varlık rolü bağlamı tanımlıyor

Bir varlığın rolü, utterance içindeki bağlama göre adlandırılmış diğer addır. İki konum, kaynak ve hedef olan bir uçuşmanın bir örneği olan bir örnek bir örnektir.

`Book a flight from Seattle to Cairo`

`location` varlığın iki örneği ayıklanmalıdır. Bilet satın alma işleminin tamamlanabilmesi için istemci uygulamanın konumun türünü bilmesi gerekir. `location` varlığın iki `origin` ve `destination` olması gerekir ve her ikisi de örnek utenlerde işaretlenmelidir. 

LUSıS `location` bulursa ve rolü tespit leyemiyorsa, konum varlığı yine de döndürülür. Kullanıcının ne tür bir konum olduğunu belirleyebilmek için istemci uygulamanın bir soru ile izlenmesi gerekir. 

Birden çok varlık bir utterlik içinde bulunabilir ve roller kullanılmadan ayıklanabilir. Tümce bağlamı varlık değerini gösteriyorsa, bir rolün kullanılması gerekir.

Söylenişi bir konum listesi içeriyorsa `I want to travel to Seattle, Cairo, and London.`, her öğenin ek anlamı olmadığı bir listesidir. 

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>En fazla varlık sayısından daha fazlasına ihtiyacınız varsa 

Sınırdan daha fazlasına ihtiyacınız varsa desteğe başvurun. Bunu yapmak için sisteminizle ilgili ayrıntılı bilgiler toplayın, [Luo](luis-reference-regions.md#luis-website) Web sitesine gidin ve ardından **destek**' i seçin. Azure aboneliğiniz destek hizmetleri içeriyorsa, [Azure teknik desteği](https://azure.microsoft.com/support/options/)'ne başvurun. 

## <a name="entity-prediction-status"></a>Varlık tahmin durumu

LUU portalı, bir örnekte varlığın seçtiğiniz varlıktan farklı bir varlık tahminiyle ne zaman olduğunu gösterir. Bu farklı puan, geçerli eğitilen modele dayalıdır. 

## <a name="next-steps"></a>Sonraki adımlar

[Tebrikler ilgili](luis-concept-utterance.md)kavramları öğrenin. 

LUSıS uygulamanıza varlık ekleme hakkında daha fazla bilgi edinmek için bkz. [varlık ekleme](luis-how-to-add-entities.md) .
