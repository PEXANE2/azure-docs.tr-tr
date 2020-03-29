---
title: V3 makine öğrenilen varlığa geçirin
titleSuffix: Azure Cognitive Services
description: V3 yazarlığı, makinede öğrenilen varlık ve uygulamanın diğer varlıklarına veya özelliklerine ilişki ekleme olanağının yanı sıra yeni bir varlık türü, makineden öğrenilen varlık sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: diberry
ms.openlocfilehash: b5dbcd9033d9a41e43ea907d043e0c0486b236db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75563839"
---
# <a name="migrate-to-v3-authoring-entity"></a>V3 Yazarlık varlığına geçirin

V3 yazarlığı, makinede öğrenilen varlık ve uygulamanın diğer varlıklarına veya özelliklerine ilişki ekleme olanağının yanı sıra yeni bir varlık türü, makineden öğrenilen varlık sağlar.

## <a name="entities-are-decomposable-in-v3"></a>Varlıklar V3'te decomposable

[ApI'ler](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview) kullanarak veya [önizleme portalı](https://preview.luis.ai/)yla V3 yazma API'leri ile oluşturulan varlıklar, üst ve alt öğeiçeren katmanlı bir varlık modeli oluşturmanıza olanak sağlar. Üst makine **öğrenilen varlık** olarak bilinir ve çocuklar makine öğrenilen varlığın **alt bileşenleri** olarak bilinir.

Her alt bileşen aynı zamanda makineden öğrenilen bir varlıktır, ancak kısıtlamalar ve tanımlayıcıların ek yapılandırma seçenekleriyle birliktedir.

* **Kısıtlamalar,** bir varlığın bir kuralla eşleştiğinde ayıklanmasına engel olan tam metin eşleştirme kurallarıdır. Kural, şu anda tam bir metin eşleşen varlık tarafından tanımlanır: önceden oluşturulmuş bir [varlık,](luis-reference-prebuilt-entities.md)normal bir [ifade varlığı](reference-entity-regular-expression.md)veya [liste varlık](reference-entity-list.md).
* **Tanımlayıcılar,** varlığı güçlü bir şekilde belirtmek için kullanılan ifade listeleri veya varlıklar gibi [özelliklerdir.](luis-concept-feature.md)

V3 yazarlığı, makinede öğrenilen varlık ve uygulamanın diğer varlıklarına veya özelliklerine ilişki ekleme olanağının yanı sıra yeni bir varlık türü, makineden öğrenilen varlık sağlar.

## <a name="how-do-these-new-relationships-compare-to-v2-authoring"></a>Bu yeni ilişkiler V2 yazarlıkla karşılaştırıldığında nasıl

V2 yazarlık, aynı görevi gerçekleştirmek için rolleri ve özellikleri ile birlikte hiyerarşik ve bileşik varlıklar sağladı. Varlıklar, özellikler ve roller açıkça birbiriyle ilişkili olmadığından, LUIS'in tahmin sırasında ilişkileri nasıl ima ettiği anlamak zordu.

V3 ile ilişki açık ve uygulama yazarları tarafından tasarlanmıştır. Bu, uygulama yazarı olarak şunları yapmanızı sağlar:

* Luis'in bu ilişkileri nasıl öngördüğünü görsel olarak görün, örnek sözlerle
* [Etkileşimli test bölmesiyle](luis-interactive-test.md) veya bitiş noktasında bu ilişkiler için test
* Bu ilişkileri istemci uygulamasında, iyi yapılandırılmış, adlandırılmış, iç içe geçen [bir .json nesnesi](reference-entity-machine-learned-entity.md) aracılığıyla kullanın

## <a name="planning"></a>Planlama

Geçiş yaparken, geçiş planınızda aşağıdakileri göz önünde bulundurun:

* LUIS uygulamanızı yedekleve geçişi ayrı bir uygulamada gerçekleştirin. V2 ve V3 uygulamasının aynı anda kullanılabilmesi, gerekli değişiklikleri ve tahmin sonuçları üzerindeki etkisini doğrulamanızı sağlar.
* Geçerli tahmin başarı ölçümlerini yakalama
* Geçerli pano bilgilerini uygulama durumunun anlık görüntüsü olarak yakalayın
* Varolan amaçları, varlıkları, ifade listelerini, desenleri ve toplu iş testlerini gözden geçirin
* Aşağıdaki öğeler **değişiklik olmadan**geçirilebilir:
    * Hedefler
    * Varlıklar
        * Normal ifade varlığı
        * Liste varlığı
    * Özellikler
        * Tümcecik listesi
* Aşağıdaki öğelerin **değişikliklerle**geçirilmeleri gerekir:
    * Varlıklar
        * Hiyerarşik varlık
        * Bileşik varlık
    * Roller - roller yalnızca makinede öğrenilen (üst) bir varlığa uygulanabilir. Roller alt bileşenlere uygulanamaz
    * Hiyerarşik ve bileşik varlıkları kullanan toplu testler ve desenler

Geçiş planınızı tasarlarken, tüm hiyerarşik ve bileşik varlıklar geçirildikten sonra, makinede öğrenilen son varlıkları gözden geçirmek için zaman bırakın. Düz bir geçiş işe yarayacak olsa da, değişikliği yaptıktan ve toplu test sonuçlarınızı gözden geçirdikten ve JSON'u tahmin ettikten sonra, istemci tarafındaki uygulamaya teslim edilen son bilgilerin farklı şekilde düzenlenmesi için daha birleşik JSON değişiklikler yapmanıza yol açabilir. Bu, kod yeniden düzenlemeye benzer ve kuruluşunuzun yerinde olan gözden geçirme işlemiyle birlikte ele alınmalıdır.

V2 modeliniz için toplu testler insanız ve toplu iş testlerini geçişin bir parçası olarak V3 modeline geçirin, geçişin uç nokta tahmin sonuçlarını nasıl etkileyeceğini doğrulayamazsınız.

## <a name="migrating-from-v2-entities"></a>V2 varlıklarından geçiş

V3 yazma modeline geçmeye başladığınızda, makinede öğrenilen varlığa ve kısıtlamalar ve tanımlayıcılar da dahil olmak üzere alt bileşenlerine nasıl geçeceğinizi düşünmelisiniz.

Aşağıdaki tablo, hangi varlıkların V2'den V3 varlık tasarımına geçiş ilerler.

|V2 yazarlık varlık türü|V3 yazarlık varlık türü|Örnek|
|--|--|--|
|Bileşik varlık|Makine öğrenilen varlık|[Daha fazla bilgi edinin](#migrate-v2-composite-entity)|
|Hiyerarşik varlık|Makinede öğrenilen varlığın rolü|[Daha fazla bilgi edinin](#migrate-v2-hierarchical-entity)|

## <a name="migrate-v2-composite-entity"></a>V2 Bileşik varlığı geçir

V2 kompozitinin her bir alt çocuğu V3 makine den öğrenilen varlığın bir alt bileşeniile temsil edilmelidir. Bileşik alt önceden oluşturulmuş, düzenli bir ifade veya bir liste varlığı ise, bu alt bileşeni alt bileşeni alt bileşeni üzerinde bir **kısıtlama** olarak uygulanmalıdır.

Bileşik bir varlığı makineden öğrenilen bir varlığa geçirmeyi planlarken göz önünde bulundurulması gerekenler:
* Alt varlıklar desenlerde kullanılamaz
* Alt varlıklar artık paylaşılmaz
* Çocuk varlıklar, eskiden makineden öğrenilmemiş sayılsalar etiketlenmeli

### <a name="existing-descriptors"></a>Varolan tanımlayıcılar

Bileşik varlıktaki sözcükleri artırmak için kullanılan tüm ifadeler listesi, makinede öğrenilen (üst) varlık, alt bileşen (alt) varlık veya niyete (ifade listesi yalnızca bir amaç için geçerliyse) tanımlayıcı olarak uygulanmalıdır. Tanımlayıcıyı en önemli şekilde artırması gereken varlığa eklemeyi planlayın. Bir alt bileşenin (alt) tahminini en önemli şekilde artıracaksa, tanımlayıcıyı genel olarak makinede öğrenilen (üst) varlığa eklemeyin.

### <a name="new-descriptors"></a>Yeni tanımlayıcılar

V3 yazarken, varlıkları tüm varlıklar ve niyetler için olası tanımlayıcılar olarak değerlendirmek için bir planlama adımı ekleyin.

### <a name="example-entity"></a>Örnek varlık

Bu varlık yalnızca bir örnektir. Kendi varlık geçişiniz başka hususlar gerektirebilir.

Kullanan bir pizzayı `order` değiştirmek için bir V2 kompoziti düşünün:
* teslimat süresi için önceden oluşturulmuş datetimeV2
* pizza, pasta, kabuk ve tepesi gibi belirli kelimeleri artırmak için ifade listesi
* mantar, zeytin, pepperoni gibi topingleri tespit etmek için varlık listeleyin.

Bu varlık için örnek bir söz:

`Change the toppings on my pie to mushrooms and delivery it 30 minutes later`

Aşağıdaki tablo geçişi gösterir:

|V2 modelleri|V3 modelleri|
|--|--|
|Üst - Bileşen varlık adlı`Order`|Üst - Makine öğrenilen varlık adlı`Order`|
|Çocuk - Önceden oluşturulmuş datetimeV2|* Önceden oluşturulmuş varlığı yeni uygulamaya geçirin.<br>* Önceden oluşturulmuş datetimeV2 için üst kısıtlama ekleyin.|
|Alt - topingler için varlık liste|* Liste varlığını yeni uygulamaya geçirin.<br>* Sonra liste varlığı için üst bir kısıtlama ekleyin.|


## <a name="migrate-v2-hierarchical-entity"></a>V2 Hiyerarşik varlığı geçir

V2 yazarlıkta, LUIS'te var olan rollerden önce hiyerarşik bir varlık sağlanmıştır. Her ikisi de bağlam kullanımına dayalı varlıkları ayıklamak için aynı amaca hizmet etti. Hiyerarşik varlıklarınız varsa, bunları rolleri olan basit varlıklar olarak düşünebilirsiniz.

V3 yazarlık olarak:
* Bir rol, makinede öğrenilen (üst) varlık ta uygulanabilir.
* Bir rol hiçbir alt bileşene uygulanamaz.

Bu varlık yalnızca bir örnektir. Kendi varlık geçişiniz başka hususlar gerektirebilir.

Bir pizza `order`değiştirmek için bir V2 hiyerarşik varlık düşünün:
* her çocuğun ya orijinal bir tepesi ya da son tepesi belirler

Bu varlık için örnek bir söz:

`Change the topping from mushrooms to olives`

Aşağıdaki tablo geçişi gösterir:

|V2 modelleri|V3 modelleri|
|--|--|
|Üst - Bileşen varlık adlı`Order`|Üst - Makine öğrenilen varlık adlı`Order`|
|Çocuk - Orijinal ve son pizza tepesi ile hiyerarşik varlık|* Her `Order` tepesi için rol ekleyin.|

## <a name="next-steps"></a>Sonraki adımlar

* [Geliştirici kaynakları](developer-reference-resource.md)
