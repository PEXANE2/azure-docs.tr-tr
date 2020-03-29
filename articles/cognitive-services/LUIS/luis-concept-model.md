---
title: Modellerle tasarım - LUIS
titleSuffix: Azure Cognitive Services
description: Dil anlama modelleri çeşitli sağlar. Bazı modeller birden fazla şekilde kullanılabilir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: d721ceb25b3ce2408563a0bed16457d05affe7b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220000"
---
# <a name="design-with-intent-and-entity-models"></a>Amaç ve varlık modelleri ile tasarım 

Dil anlama modelleri çeşitli sağlar. Bazı modeller birden fazla şekilde kullanılabilir. 

## <a name="v3-authoring-uses-machine-teaching"></a>V3 Yazma makine öğretimkullanır

LUIS insanların kolayca bir makineye kavramları öğretmek için izin verir. Makine daha sonra akıllı uygulamalara güç sağlamak için kullanılabilecek modeller (sınıflandırıcılar ve çıkarıcılar gibi kavramların işlevsel yaklaşımları) oluşturabilir. LUIS makine öğrenimi ile desteklenirken, makine öğrenimini anlamak için gerekli değildir. Bunun yerine, makine öğretmenleri kavramın olumlu ve olumsuz örneklerini göstererek ve bir kavramın diğer ilgili kavramlar kullanılarak nasıl modelleneceğini açıklayarak kavramları LUIS'e iletirler. Öğretmenler ayrıca öngörü hatalarını belirleyerek ve düzelterek LUIS'in modelini etkileşimli olarak geliştirebilirler. 

## <a name="v3-authoring-model-decomposition"></a>V3 Yazma modeli ayrıştırma

LUIS, V3 yazma API'leri ile _model ayrışmasını_ destekler ve modeli daha küçük parçalara ayırılır. Bu, çeşitli parçaların nasıl oluşturulduklarına ve tahmin edildiğine güvenle modellerinizi oluşturmanıza olanak tanır.

Model ayrıştırma aşağıdaki parçalara sahiptir:

* [Niyet](#intents-classify-utterances)
    * özellikleri tarafından sağlanan [tanımlayıcılar](#descriptors-are-features)
* [makine de öğrenilen varlıklar](#machine-learned-entities)
    * [alt bileşenler](#entity-subcomponents-help-extract-data) (ayrıca makineden öğrenilen varlıklar)
        * özellikleri tarafından sağlanan [tanımlayıcılar](#descriptors-are-features) 
        * düzenli ifadeler ve listeler gibi makine de öğrenilmemiş varlıklar tarafından sağlanan [kısıtlamalar](#constraints-are-text-rules)

## <a name="v2-authoring-models"></a>V2 Yazma modelleri

LUIS, V2 yazma API'leri ile bileşik varlıkları destekler. Bu benzer model ayrıştırma sağlar, ancak V3 modeli ayrışması ile aynı değildir. Önerilen model mimarisi, V3 yazma API'lerinde ayrışmayı modellemektir. 

## <a name="intents-classify-utterances"></a>Niyetler, söyleyiyi sınıflandırmak

Bir niyet, luis'e niyeti öğretmek için örnek söyleyişleri sınıfa eder. Bir niyet içindeki örnek söz, söyleyiş olumlu örnekleri olarak kullanılır. Bu aynı söyleyiş diğer tüm niyetlerde olumsuz örnek olarak kullanılır.

Bir kullanıcının kitap sipariş etme niyetini belirlemesi gereken bir uygulamayı ve müşterinin gönderim adresine ihtiyaç duyduğu bir uygulamayı düşünün. Bu uygulamanın iki `OrderBook` amacı `ShippingLocation`vardır: ve .

Aşağıdaki söyleyiş `OrderBook` niyet için olumlu bir **örnek** ve ve `ShippingLocation` `None` niyetler için olumsuz bir **örnektir:** 

`Buy the top-rated book on bot architecture.`

İyi tasarlanmış niyetlerin sonucu, onların örnek söyleyişleriyle, yüksek niyet tahminidir. 

## <a name="entities-extract-data"></a>Varlıklar veri ayıklama

Bir varlık, sözcükten ayıklanması istediğiniz bir veri birimini temsil eder. 

### <a name="machine-learned-entities"></a>Makine de öğrenilen varlıklar

Makineden öğrenilen varlık, makineden öğrenilen varlıklar olan alt bileşenler içeren üst düzey bir varlıktır. 

**Makinede öğrenilen bir varlık kullanın:**

* alt bileşenler istemci uygulaması tarafından gerekli olduğunda
* makine öğrenme algoritmasının varlıkları ayrıştırması

Her alt bileşen şunları yapabilir:

* alt bileşenler
* kısıtlamalar (normal ifade varlığı veya liste varlığı)
* tanımlayıcılar (ifade listesi gibi özellikler) 

Makinede öğrenilen bir varlık örneği, uçak bileti siparişidir. Kavramsal olarak bu tarih, saat, koltuk miktarı, birinci sınıf veya koç, menşe konumu, hedef konumu ve yemek seçimi gibi koltuk türü gibi birçok küçük veri birimleri ile tek bir işlemdir.


### <a name="entity-subcomponents-help-extract-data"></a>Varlık alt bileşenleri veri ayıklama yardımcı olur

Alt bileşen, makineden öğrenilen bir üst varlık içinde makinede öğrenilen bir alt varlıktır. 

**Alt bileşeni şu şekilde kullanın:**

* makinede öğrenilen varlığın (ana varlık) parçalarını ayrıştırın.

Aşağıdaki, tüm bu ayrı veri parçalarıyla makineden öğrenilen bir varlığı temsil eder:

* TravelOrder (makineden öğrenilen varlık)
    * DateTime (önceden oluşturulmuş datetimeV2)
    * Konum (makinede öğrenilen varlık)
        * Origin (rol gibi `from`bağlam içinde bulundu)
        * Hedef (rol gibi `to`bağlam içinde bulunan)
    * Oturma (makine de öğrenilen varlık)
        * Miktar (önceden oluşturulmuş sayı)
        * Kalite (ifade listesinin tanımlayıcısı ile makine öğrenilen varlık)
    * Yemekler (gıda seçenekleri olarak liste varlık kısıtlaması ile makine öğrenilen varlık)

Bu verilerin bazıları, örneğin kaynak konumu ve hedef konumu, belki de bu tür ifadeler `from` ile, `to`söyleyiş bağlamında öğrenilmelidir. Verilerin diğer bölümleri tam dize eşleşmeleri`Vegan`( ) veya önceden oluşturulmuş `Seattle` varlıklar `Cairo`(geographyV2 ve) ile ayıklanabilir. 

Verilerin hangi modelleri seçtiğinize göre nasıl eşleştirilip ayıklandığınızı ve bunları nasıl yapılandırdığınızı tasarlarsınız.

### <a name="constraints-are-text-rules"></a>Kısıtlamalar metin kurallarıdır

Kısıtlama, normal ifade varlığı veya liste varlığı gibi makinede öğrenilmemiş bir varlık tarafından sağlanan metin eşleştirme kuralıdır. Kısıtlama, öngörüyü sınırlamak ve istemci uygulaması tarafından gereken varlık çözümlünü sağlamak için tahmin zamanında uygulanır. Alt bileşeni yazarken bu kuralları tanımlarsınız. 

**Bir kısıtlama kullanın:**
* ayıklamak için tam metin bildiğinizde.

Kısıtlamalar şunlardır:

* [düzenli ifade](reference-entity-regular-expression.md) varlıkları
* [liste](reference-entity-list.md) varlıkları 
* [önceden oluşturulmuş](luis-reference-prebuilt-entities.md) varlıklar

Uçak bileti örneğine devam eden havaalanı kodları, tam metin eşleşmeleri için liste varlığında olabilir. 

Bir havaalanı listesi için, Seattle için liste `Seattle` girişi şehir adıdır ve Seattle ile eşanlamlı olan çevre kasaba ve şehirlerle birlikte Seattle havaalanı kodunu içerir:

|`Seattle`Varlık eşanlamlılarını listele|
|--|
|`Sea`|
|`seatac`|
|`Bellevue`|

Havaalanı kodları için yalnızca 3 harf kodlarını tanımak istiyorsanız, kısıtlama olarak normal bir ifade kullanın. 

`/^[A-Z]{3}$/`

## <a name="intents-versus-entities"></a>Varlıklara karşı niyetler

Varlıklar, söyleyişten çıkarılan veri parçaları iken, niyet _tüm_ söyleyişin istenen sonucudur. Genellikle niyetler istemci uygulamasının gerçekleştirmesi gereken eylemlere bağlıdır ve varlıklar bu eylemi gerçekleştirmek için gereken bilgilerdir. Programlama açısından bakıldığında, bir amaç bir yöntem çağrısını tetikler ve varlıklar bu yöntem çağrısıiçin parametre olarak kullanılır.

Bu söyleyiş bir amacı _olmalıdır_ ve varlıklar _olabilir:_

`Buy an airline ticket from Seattle to Cairo`

Bu söyleyiş tek bir amacı vardır:

* Uçak bileti satın alma

Bu _söyleyiş_ birkaç tüzel kişilikolabilir:

* Seattle (menşeli) ve Kahire'nin konumları (hedef)
* Tek bir biletin miktarı

## <a name="descriptors-are-features"></a>Tanımlayıcılar özelliklerdir

Tanımlayıcı, ifade listeleri ve varlıkları da dahil olmak üzere, eğitim zamanında bir modele uygulanan bir özelliktir. 

**İstediğince bir tanımlayıcı kullanın:**

* tanımlayıcı tarafından tanımlanan sözcük ve tümceciklerin önemini artırmak
* LUIS tanımlayıcı için tavsiye etmek için yeni metin veya ifadeler tavsiye var
* eğitim verilerindeki bir hatayı düzeltme

## <a name="next-steps"></a>Sonraki adımlar

* [Niyetleri](luis-concept-intent.md) ve [varlıkları](luis-concept-entity-types.md)anlayın. 