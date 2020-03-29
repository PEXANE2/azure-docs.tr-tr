---
title: Varlık türleri - LUIS
titleSuffix: Azure Cognitive Services
description: 'Varlıklar söyleyişten veri ayıklar. Varlık türleri, verilerin öngörülebilir ayıklama sağlar. İki tür varlık vardır: makinede öğrenilen ve makineden öğrenilmemiş. Hangi varlık türüyle birlikte çalıştığınızı bilmek önemlidir.'
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79221034"
---
# <a name="entities-and-their-purpose-in-luis"></a>LUIS'teki varlıklar ve amaçları

Varlıkların birincil amacı, istemci uygulamasına öngörülebilir veri ayıklama vermektir. _İsteğe bağlı,_ ikincil amaç, tanımlayıcıları olan amacın veya diğer varlıkların tahminini artırmaktır.

İki tür varlık vardır:

* makine öğrenilen - bağlamdan
* makinede öğrenilmemiş - tam metin eşleşmeleri, desen eşleşmeleri veya önceden oluşturulmuş varlıklar tarafından algılama için

Makinede öğrenilen varlıklar en geniş veri ayıklama seçeneklerini sağlar. Makinede öğrenilen olmayan varlıklar metin eşleştirerek çalışır ve makinede öğrenilen bir varlık üzerinde bağımsız olarak veya [kısıtlama](#design-entities-for-decomposition) olarak kullanılabilir.

## <a name="entities-represent-data"></a>Varlıklar verileri temsil ediyor

Varlıklar, adlar, tarihler, ürün adları veya herhangi bir önemli sözcük grubu gibi sözcük ten ibareden çıkarmak istediğiniz verilerdir. Bir söyleyiş birçok varlığı içerebilir veya hiç bir şey içermez. İstemci uygulaması, görevini gerçekleştirmek için verilere ihtiyaç _duyabilir._

Varlıkların bir modeldeki her amaç için tüm eğitim sözcüklerinde tutarlı bir şekilde etiketlenmeleri gerekir.

 Kendi varlıklarınızı tanımlayabilir veya [datetimeV2](luis-reference-prebuilt-datetimev2.md), [ordinal,](luis-reference-prebuilt-ordinal.md) [e-posta](luis-reference-prebuilt-email.md)ve [telefon numarası](luis-reference-prebuilt-phonenumber.md)gibi yaygın kavramlar için zaman kazanmak için önceden oluşturulmuş varlıkları kullanabilirsiniz.

|İfade|Varlık|Veri|
|--|--|--|
|New York'a 3 bilet satın alın|Önceden oluşturulmuş sayı<br>Yer.Hedef|3<br>New York|
|5 Mart'ta New York'tan Londra'ya bilet alın|Yer.Origin<br>Yer.Hedef<br>Önceden oluşturulmuş datetimeV2|New York<br>Londra<br>5 Mart 2018|

### <a name="entities-are-optional"></a>Varlıklar isteğe bağlıdır

Niyetler gerekli olmakla birlikte, varlıklar isteğe bağlıdır. Uygulamanızdaki her kavram için varlıklar oluşturmanız gerekmez, ancak yalnızca istemci uygulamasının harekete geçmesi için gerekli olan varlıklar için.

Söyleyişleriniz istemci uygulamasının gerektirdiği verilere sahip değilse, varlık eklemeniz gerekmez. Uygulamanız geliştikçe ve yeni bir veri gereksinimi tanımlandıkça, luis modelinize daha sonra uygun varlıklar ekleyebilirsiniz.

## <a name="entity-compared-to-intent"></a>Niyete göre varlık

Varlık, ayıklanması istediğiniz söyleyincin içindeki bir veri kavramını temsil eder.

Bir söyleyiş isteğe bağlı olarak varlıkları içerebilir. Buna karşılık, bir söyleyiş için niyet tahmini _gereklidir_ ve tüm söyleyerek temsil eder. LUIS örnek söyleyişbir niyet içinde yer alan gerektirir.

Aşağıdaki 4 söyleyiş göz önünde bulundurun:

|İfade|Öngörülen niyet|Çıkarılan varlıklar|Açıklama|
|--|--|--|--|
|Yardım|Yardım|-|Çıkarılacak bir şey yok.|
|Bir şey gönder|sendSomething|-|Çıkarılacak bir şey yok. Model bu bağlamda ayıklamak `something` için eğitilmedi ve alıcı da yok.|
|Bob'a hediye gönder|sendSomething|`Bob`, `present`|Model, adını `Bob`ayıklayan [personName](luis-reference-prebuilt-person.md) önceden oluşturulmuş varlık ile eğitilmiştir. Ayıklamak `present`için makineden öğrenilen bir varlık kullanılmıştır.|
|Bob'a bir kutu çikolata gönder.|sendSomething|`Bob`, `box of chocolates`|İki önemli veri parçası `Bob` ve `box of chocolates`varlıklar tarafından ayıklanmıştır.|

## <a name="design-entities-for-decomposition"></a>Ayrıştırma için tasarım varlıkları

Üst düzey varlığınızı makineden öğrenilen bir varlık yapmak iyi bir varlık tasarımıdır. Bu, zaman içinde varlık tasarımınızda ve **alt bileşenlerin** (alt varlıklar) kullanımına, isteğe bağlı olarak **kısıtlamalar** ve **tanımlayıcılarla,** üst düzey varlığı istemci uygulamasının gerektirdiği parçalara ayrıştırmaya olanak tanır.

Ayrıştırma için tasarım, LUIS'in istemci uygulamanıza derin bir varlık çözünürlüğü geri dönmesini sağlar. Bu, istemci uygulamanızın iş kurallarına odaklanmasına ve veri çözünürlüğünü LUIS'e bırakmasına olanak tanır.

### <a name="machine-learned-entities-are-primary-data-collections"></a>Makinede öğrenilen varlıklar birincil veri koleksiyonlarıdır

[**Makinede öğrenilen varlıklar**](tutorial-machine-learned-entity.md) üst düzey veri birimidir. Alt bileşenler, makinede öğrenilen varlıkların alt varlıklarıdır.

Makinede öğrenilen bir varlık, eğitim sözlemeleri yoluyla öğrenilen bağlamı temel alan tetikleyiciler. **Kısıtlamalar,** [liste](reference-entity-list.md) veya [Regex](reference-entity-regular-expression.md)gibi makinede öğrenilmemiş bir varlığın tam metin eşleştirme tanımına dayalı olarak tetiklenen daha fazla kısıtlamanın yanı sıra makine tarafından öğrenilen bir varlığa uygulanan isteğe bağlı kurallardır. Örneğin, makinede `size` öğrenilen bir varlık, `sizeList` `size` varlığı yalnızca varlık içinde bulunan değerlerle karşılaşıldığında tetiklemeye zorlayan bir liste varlığı kısıtlamasına `sizeList` sahip olabilir.

[**Tanımlayıcılar,**](luis-concept-feature.md) tahmin için sözcüklerin veya tümceciklerin alaka düzeyini artırmak için uygulanan özelliklerdir. Bir niyeti veya varlığı *tanımlamak* için kullanıldıkları için *tanımlayıcı* olarak adlandırılırlar. Tanımlayıcılar, LUIS'in gözlemlediği ve öğrendiği önemli sözcükler veya tümcecikler gibi verilerin ayırt edici özelliklerini veya özniteliklerini tanımlar.

LUIS uygulamanızda bir ifade listesi özelliği oluşturduğunuzda, varsayılan olarak genel olarak etkinleştirilir ve tüm niyet ve kuruluşlararasında eşit olarak uygulanır. Ancak, tümcecik listesini makinede öğrenilen bir varlığın (veya *modelin)* tanımlayıcısı (özelliği) olarak uygularsanız, kapsamı yalnızca bu modele uygulanacak şekilde azalır ve artık diğer tüm modellerde kullanılmaz. Bir modeliçin tanımlayıcı olarak bir ifade listesi kullanmak, uygulandığı modelin doğruluğuna yardımcı olarak ayrışmaya yardımcı olur.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Varlık türleri

Varlığı, verilerin nasıl ayıklanması gerektiğine ve çıkarıldıktan sonra nasıl temsil edilmesi gerektiğine göre seçin.

|Varlık türü|Amaç|
|--|--|
|[**Makineden öğrenilen**](tutorial-machine-learned-entity.md)|Makinede öğrenilen varlıklar, söyleyiş bağlamından öğrenirler. Varlık türüne bakılmaksızın varlıkların üst gruplandırması. Bu, örnek sözlemelerde yerleşim varyasyonu önemli hale getirir. |
|[**Liste**](reference-entity-list.md)|**Tam metin eşleşmi**ile ayıklanan öğelerin ve eşanlamlılarının listesi.|
|[**Desen.herhangi bir**](reference-entity-pattern-any.md)|Varlığın sonunun belirlenmesinin zor olduğu varlık. |
|[**Önceden oluşturulmuş**](luis-reference-prebuilt-entities.md)|URL veya e-posta gibi belirli türde verileri ayıklamak için zaten eğitilmiş. Bu önceden oluşturulmuş varlıkların bazıları açık kaynak [Tanırlar-Metin](https://github.com/Microsoft/Recognizers-Text) projesinde tanımlanır. Belirli kültürünüz veya varlığınız şu anda desteklenmiyorsa, projeye katkıda bulunun.|
|[**Normal ifade**](reference-entity-regular-expression.md)|Tam metin **eşleşmesi**için düzenli ifade kullanır.|

## <a name="extracting-contextually-related-data"></a>Bağlamsal olarak ilişkili verileri ayıklama

Bir söyleyiş, verilerin anlamının söyleyiş içindeki içeriğe dayandığı bir varlığın iki veya daha fazla olayını içerebilir. Bir örnek, menşei ve varış noktası olmak üzere iki konumu olan bir uçuş rezervasyonu için bir sözdür.

`Book a flight from Seattle to Cairo`

Bir `location` varlığın iki örneğinin ayıklanması gerekir. İstemci başvurusunun bilet satın alımını tamamlamak için her birinin konum türünü bilmesi gerekir.

Bağlamsal olarak ilişkili verileri ayıklamak için iki teknik vardır:

 * Varlık `location` makineden öğrenilen bir varlıktır ve iki alt `origin` bileşen `destination` varlığı kullanır ve (tercih edilen)
 * Varlık `location` iki **rol** `origin` kullanır ve`destination`

Birden çok varlık bir sözcükte bulunabilir ve kullanıldıkları bağlamın bir önemi yoksa ayrışma veya roller kullanılmadan ayıklanabilir. Örneğin, söyleyiş konumların bir listesini `I want to travel to Seattle, Cairo, and London.`içeriyorsa, bu liste her öğenin ek bir anlamı olmadığı bir listedir.

### <a name="using-subcomponent-entities-of-a-machine-learned-entity-to-define-context"></a>Bağlamı tanımlamak için makinede öğrenilen bir varlığın alt bileşen varlıklarını kullanma

Bir uçuş rezervasyonu eylemini açıklayan verileri ayıklamak ve ardından üst düzey varlığı istemci uygulamasının gerektirdiği ayrı parçalara ayırmak için [**makineden öğrenilen**](tutorial-machine-learned-entity.md) bir varlık kullanabilirsiniz.

Bu örnekte, `Book a flight from Seattle to Cairo`üst düzey varlık `travelAction` ayıklamak `flight from Seattle to Cairo`için etiketlenmiş ve olabilir. Sonra iki alt bileşen varlıkları `origin` oluşturulur, denir ve `destination`, her `geographyV2` ikisi de önceden oluşturulmuş varlığın bir kısıtlama uygulanmıştır. Eğitim de, `origin` ve `destination` uygun olarak etiketlenir.

### <a name="using-entity-role-to-define-context"></a>Bağlamı tanımlamak için Varlık rolünü kullanma

Rol, bir varlığın söyleyiş içindeki içeriğe dayalı diğer adıdır. Bir rol önceden oluşturulmuş veya özel varlık türüyle kullanılabilir ve hem örnek söz hem de desenlerde kullanılabilir. Bu örnekte, `location` varlığın iki `origin` `destination` rolüne ve her ikisinin de örnek söyleyişlerinde işaretlemesi gerekir.

LUIS rolü `location` bulur ancak belirleyemezse, konum varlığı yine de döndürülür. İstemci uygulaması, kullanıcının hangi konum türünü kastettiğini belirlemek için bir soruyla birlikte takip etmek gerekir.


## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Maksimum varlık sayısından daha fazlasına ihtiyacınız varsa

Sınırdan daha fazlasına ihtiyacınız varsa desteğe başvurun. Bunu yapmak için, sisteminiz hakkında ayrıntılı bilgi toplamak, [LUIS](luis-reference-regions.md#luis-website) web sitesine gidin ve sonra **Destek**seçin. Azure aboneliğinizde destek hizmetleri varsa, [Azure teknik desteğine](https://azure.microsoft.com/support/options/)başvurun.

## <a name="entity-prediction-status"></a>Varlık tahmin durumu

LUIS portalı, bir örnek söyleyişde varlığın seçtiğiniz varlıktan farklı bir varlık tahminine sahip olduğunu gösterir. Bu farklı puan, geçerli eğitilmiş modele dayanır.

## <a name="next-steps"></a>Sonraki adımlar

İyi [söyleyişlerle](luis-concept-utterance.md)ilgili kavramları öğrenin.

Bkz. LUIS uygulamanıza nasıl varlık ekleyeceğiniz hakkında daha fazla bilgi edinmek için [taraf ekle'](luis-how-to-add-entities.md) ye bakın.

Bkz. Öğretici: Makinede öğrenilen varlığı kullanarak bir söyleyişten yapılandırılmış veri çıkarmayı öğrenmek için [Dil Anlama (LUIS)'deki makine öğrenilen varlıklarla kullanıcı dilinden yapılandırılmış verileri ayıklayın.](tutorial-machine-learned-entity.md)
 
