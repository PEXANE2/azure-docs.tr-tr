---
title: V3 makine tarafından öğrenilen varlığa geçiş
titleSuffix: Azure Cognitive Services
description: V3 yazma, makine tarafından öğrenilen varlık ve uygulamanın diğer varlıklarına ya da özelliklerine ilişki ekleyebilme olanağı sunan bir yeni varlık türü (makine tarafından öğrenilen varlık) sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/30/2019
ms.author: diberry
ms.openlocfilehash: b5dbcd9033d9a41e43ea907d043e0c0486b236db
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75563839"
---
# <a name="migrate-to-v3-authoring-entity"></a>V3 yazma varlığına geçiş

V3 yazma, makine tarafından öğrenilen varlık ve uygulamanın diğer varlıklarına ya da özelliklerine ilişki ekleyebilme olanağı sunan bir yeni varlık türü (makine tarafından öğrenilen varlık) sağlar.

## <a name="entities-are-decomposable-in-v3"></a>Varlıklar v3 'de parçalançıkarıldı

[API 'leri](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview) ya da [önizleme portalını](https://preview.luis.ai/)kullanarak v3 yazma API 'leri ile oluşturulan varlıklar, üst ve alt öğeleri olan katmanlı bir varlık modeli oluşturmanıza olanak sağlar. Üst öğe, **makine tarafından öğrenilen varlık** olarak bilinir ve alt öğeler makine tarafından öğrenilen varlığın alt **bileşenleri** olarak bilinir.

Her alt bileşen ayrıca makine tarafından öğrenilen bir varlıktır, ancak kısıtlamaların ve tanımlayıcılarının yapılandırma seçenekleri eklenmiştir.

* **Kısıtlamalar** , bir varlığın bir kuralla eşleştiğinde ayıklandığını garanti eden tam metin eşleştirme kurallarıdır. Kural, bir tam metin ile eşleşen varlık, şu anda [önceden oluşturulmuş bir varlık](luis-reference-prebuilt-entities.md), bir [normal ifade varlığı](reference-entity-regular-expression.md)veya [liste varlığı](reference-entity-list.md)tarafından tanımlanır.
* **Tanımlayıcılar** , varlığı kesin olarak belirtmek için kullanılan tümcecik listeleri veya varlıklar gibi [özelliklerdir](luis-concept-feature.md).

V3 yazma, makine tarafından öğrenilen varlık ve uygulamanın diğer varlıklarına ya da özelliklerine ilişki ekleyebilme olanağı sunan bir yeni varlık türü (makine tarafından öğrenilen varlık) sağlar.

## <a name="how-do-these-new-relationships-compare-to-v2-authoring"></a>Bu yeni ilişkiler v2 yazma ile nasıl karşılaştırılır

V2 yazma, hiyerarşik ve bileşik varlıkların yanı sıra bu aynı görevi yerine getirmek için roller ve özelliklerle birlikte verilmiştir. Varlıklar, Özellikler ve roller birbirleriyle açık bir şekilde ilişkili olmadığından, YAĞMANıN tahmin sırasında ilişkilerin nasıl ima olduğunu anlamak zordur.

V3 ile, ilişki uygulama yazarları tarafından açık ve tasarlanmıştır. Bu, uygulama yazarı olarak şunları yapmanıza olanak sağlar:

* Bu ilişkilerin bu ilişkileri nasıl tahmin ediyor olduğunu görsel olarak görme
* [Etkileşimli test bölmesi](luis-interactive-test.md) veya uç noktada bu ilişkiler için test yapın
* İstemci uygulamasında, iyi yapılandırılmış, adlandırılmış, iç içe geçmiş bir [. JSON nesnesi](reference-entity-machine-learned-entity.md) aracılığıyla bu ilişkileri kullanın

## <a name="planning"></a>Planlama

' Yi geçirdiğinizde, geçiş planınızda aşağıdakileri göz önünde bulundurun:

* LUSıS uygulamanızı yedekleyin ve geçişi ayrı bir uygulamada gerçekleştirin. Bir v2 ve v3 uygulamasının aynı anda kullanılabilir olması, gerekli değişiklikleri ve tahmin sonuçları üzerindeki etkiyi doğrulamanızı sağlar.
* Geçerli tahmini başarı ölçümlerini yakala
* Geçerli Pano bilgilerini uygulama durumunun bir anlık görüntüsü olarak yakala
* Mevcut hedefleri, varlıkları, tümcecik listelerini, desenleri ve toplu testleri gözden geçirin
* Aşağıdaki öğeler **değişiklik yapılmadan**geçirilebilir:
    * Hedefler
    * Varlıklar
        * Normal ifade varlığı
        * Liste varlığı
    * Özellikler
        * Tümcecik listesi
* Aşağıdaki öğelerin **değişikliklerle**geçirilmesi gerekir:
    * Varlıklar
        * Hiyerarşik varlık
        * Bileşik varlık
    * Roller-roller yalnızca, makine tarafından öğrenilen (üst) varlığa uygulanabilir. Roller alt bileşenlere uygulanamıyor
    * Hiyerarşik ve bileşik varlıkları kullanan toplu testler ve desenler

Geçiş planınızı tasarlarken, tüm hiyerarşik ve bileşik varlıklar geçirildikten sonra, son makineye öğrenilen varlıkları gözden geçirmek için zaman bırakın. Düz bir geçiş çalışmaya başladıktan sonra, değişikliği yaptıktan ve Batch test sonuçlarınızı ve tahmin JSON 'larınızı gözden geçirdikten sonra, daha fazla birleştirilmiş JSON, istemci tarafı uygulamasına teslim edilen son bilgilerin farklı şekilde organize edilmesi için sizi değişiklik yapmaya yol açabilir. Bu kod yeniden düzenlemesi ile benzerdir ve kuruluşunuzun yerinde olduğu gözden geçirme işlemiyle birlikte değerlendirilmelidir.

V2 modelinize yönelik Batch testleriniz yoksa ve geçişin bir parçası olarak Batch testlerini v3 modeline geçirirseniz, geçişin Endpoint tahmin sonuçlarını nasıl etkileyeceğini doğrulayamazsınız.

## <a name="migrating-from-v2-entities"></a>V2 varlıklarından geçiş

V3 yazma modeline taşımaya başladığınızda, makine tarafından öğrenilen varlığa ve bunların alt bileşenlerinden kısıtlamalar ve tanımlayıcılar dahil olmak üzere nasıl taşınacağını göz önünde bulundurmanız gerekir.

Aşağıdaki tabloda, hangi varlıkların bir v2 'den v3 varlık tasarımına geçirilmesi gerektiğini not edin.

|V2 yazma varlık türü|V3 yazarlık varlık türü|Örnek|
|--|--|--|
|Bileşik varlık|Makine tarafından öğrenilen varlık|[Daha fazla bilgi edinin](#migrate-v2-composite-entity)|
|Hiyerarşik varlık|Makine tarafından öğrenilen varlığın rolü|[Daha fazla bilgi edinin](#migrate-v2-hierarchical-entity)|

## <a name="migrate-v2-composite-entity"></a>V2 bileşik varlığını geçirme

V2 Composite 'un her bir alt öğesi v3 makine tarafından öğrenilen varlığın bir alt bileşeni ile temsil edilmelidir. Bileşik alt öğe önceden oluşturulmuş bir, normal ifade veya bir liste varlığı ise, bu, alt bileşeni temsil eden alt bileşen üzerinde bir **kısıtlama** olarak uygulanmalıdır.

Bileşik bir varlığı makineye öğrenilen bir varlığa geçirmeyi planlarken dikkat edilecek noktalar:
* Alt varlıklar desenlerinde kullanılamaz
* Alt varlıklar artık paylaşılmıyor
* Makine öğrenildikleri takdirde alt varlıkların etiketlenmesi gerekir

### <a name="existing-descriptors"></a>Mevcut tanımlayıcılar

Bileşik varlıktaki kelimeleri artırmak için kullanılan tümcecik listesi, makine tarafından öğrenilen (üst) varlığa, alt bileşen (alt) varlığına veya amaca (tümcecik listesi yalnızca bir amaca geçerliyse) bir tanımlayıcı olarak uygulanmalıdır. Tanımlayıcıyı, en önemli düzeyde iyileştirmeli varlığa eklemeyi planlayın. Bir alt bileşen (alt) tahminini en önemli ölçüde iyileştirdiği takdirde, tanımlayıcıyı makine öğrenmiş (üst) varlığına eklemeyin.

### <a name="new-descriptors"></a>Yeni tanımlayıcılar

V3 yazma bölümünde varlıkları tüm varlıklar ve amaçlar için olası tanımlayıcılar olarak değerlendirmek üzere bir planlama adımı ekleyin.

### <a name="example-entity"></a>Örnek varlık

Bu varlık yalnızca bir örnektir. Kendi varlık geçişiniz, başka hususlar gerektirebilir.

Şunu kullanan bir pizza `order` değiştirmek için bir v2 bileşimini düşünün:
* teslim süresi için önceden oluşturulmuş datetimeV2
* Pizza, pasta, Crust ve tografik gibi belirli kelimeleri artırmak için tümcecik listesi
* mushodalar, zeytin, pepperoni gibi toppings algılamak için varlık listeleyin.

Bu varlık için bir örnek söylenişi şunlardır:

`Change the toppings on my pie to mushrooms and delivery it 30 minutes later`

Aşağıdaki tabloda geçiş gösterilmektedir:

|V2 modelleri|V3 modelleri|
|--|--|
|`Order` adlı üst bileşen varlık|Ana makine tarafından öğrenilen `Order` adlı varlık|
|Alt-önceden oluşturulmuş datetimeV2|* Önceden oluşturulmuş varlığı yeni uygulamaya geçirin.<br>* Önceden oluşturulmuş datetimeV2 için üst öğede kısıtlama ekleyin.|
|Toppings için alt liste varlığı|* Liste varlığını yeni uygulamaya geçirin.<br>* Ardından liste varlığı için üst öğeye bir kısıtlama ekleyin.|


## <a name="migrate-v2-hierarchical-entity"></a>V2 hiyerarşik varlığını geçirme

V2 yazma sürümünde, LUSıS 'de mevcut rollerden önce hiyerarşik bir varlık sağlandı. Her ikisi de bağlam kullanımına göre varlıkların ayıklanması için de aynı amaca hizmet. Hiyerarşik varlıklarınız varsa, bunları rollerle basit varlıklar olarak düşünebilirsiniz.

V3 yazma:
* Makine öğrenilmiş (üst) varlığa bir rol uygulanabilir.
* Bir rol, herhangi bir alt bileşenlere uygulanamaz.

Bu varlık yalnızca bir örnektir. Kendi varlık geçişiniz, başka hususlar gerektirebilir.

Bir pizza `order`değiştirmek için bir v2 hiyerarşik varlığı düşünün:
* Her çocuğun orijinal veya son oluşturma

Bu varlık için bir örnek söylenişi şunlardır:

`Change the topping from mushrooms to olives`

Aşağıdaki tabloda geçiş gösterilmektedir:

|V2 modelleri|V3 modelleri|
|--|--|
|`Order` adlı üst bileşen varlık|Ana makine tarafından öğrenilen `Order` adlı varlık|
|Orijinal ve son pizza ile alt hiyerarşik varlık|* Her oluşturma için `Order` rol ekleyin.|

## <a name="next-steps"></a>Sonraki adımlar

* [Geliştirici kaynakları](developer-reference-resource.md)
