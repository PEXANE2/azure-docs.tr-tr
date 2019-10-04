---
title: Özellikler-LUSıS
titleSuffix: Azure Cognitive Services
description: Etiketlemek veya sınıflandırmak istediğiniz girişin nasıl tanınılacağı hakkında ipuçları sağlamak için bir dil modeline özellikler ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: dab4b4c6f41a95623a40e5d3fd859f9613afac27
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949610"
---
# <a name="phrase-list-features-in-your-luis-app"></a>LUSıS uygulamanızdaki ifade listesi özellikleri

Machine Learning 'de, bir *özellik* sistem engelinizdeki verilerin bir ayırt edici nitelik veya özniteliğidir. 

Etiketlemek veya sınıflandırmak istediğiniz girişin nasıl tanınılacağı hakkında ipuçları sağlamak için bir dil modeline özellikler ekleyin. Özellikler LUıN hem amaçları hem de varlıkları tanımasına karşın özellikler, amaç veya varlıkların kendileri değildir. Bunun yerine, Özellikler ilgili koşullara örnek verebilir.  

## <a name="what-is-a-phrase-list-feature"></a>Tümcecik listesi özelliği nedir?
Tümcecik listesi, uygulamanız için önemli olan sözcüklerin veya deyimlerin listesidir ve diğer sözcüklerden daha fazlasını yapın. Bir tümcecik listesi, bu sözcüklerin yaklaşık olarak LUO 'ya ek bir sinyal olarak uygulama etki alanının sözlüğüne ekler. Bunlardan biri hakkında ne tür bir saldırgan, diğerlerine de otomatik olarak uygulanır. Bu liste, tam metin eşleştirmelerinin kapalı bir [liste varlığı](luis-concept-entity-types.md#types-of-entities) değildir.

Tümcecik listeleri, sözcük ve tümcecik için çok sayıda sözcük ve tümcecik kullanan söylenişi örnekleri eklemeniz gerekir.

## <a name="phrase-lists-help-all-models"></a>Tümcecik listeleri tüm modellere yardımcı olur

Tümcecik listeleri belirli bir amaca veya varlığa bağlı değildir, ancak tüm amaçlar ve varlıklara önemli bir artırma olarak eklenmiştir. Amacı, amaç algılamayı ve varlık sınıflandırmasını geliştirmaktır.

## <a name="how-to-use-phrase-lists"></a>Tümcecik listelerini kullanma

Uygulamanızda uygulama için önemli sözcükler veya tümcecikler olduğunda [bir tümcecik listesi oluşturun](luis-how-to-add-features.md) , örneğin:

* sektör koşulları
* Argo
* kısaltmaları
* şirkete özgü dil
* başka bir dilden, ancak uygulamanızda sık kullanılan dil
* örneğinizdeki önemli sözcükler ve tümcecikler

Birkaç sözcük veya tümcecik girdikten sonra, ilgili değerleri bulmak için **öner** özelliğini kullanın. İfade listesi değerlerinize eklemeden önce ilgili değerleri gözden geçirin.

Tümcecik listesi, eş anlamlı olan değerler içindir. Örneğin, tüm su gövdelerini, ve gibi örnek bir örnek elde etmek istiyorsanız: 

* Harika Lakes 'e hangi şehirler yakınlıyor? 
* Gölü Havasu hangi yolda çalışır?
* Nve bitişi nerede? 

Her söylük, su gövdesinden bağımsız olarak hem amaç hem de varlıklar için belirtilmelidir: 

* [Bodyofsu] için hangi şehirler yakın?
* [Bodyofsu] üzerinde hangi yol çalıştırılır?
* [Bodyofsu] başlangıcı ve bitişi nerede? 

Su gövdesi için sözcükler veya ifadeler eşanlamlı olduğundan ve söylerde birbirinin yerine kullanılabilir. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Tümcecik listeleri basit bir değiştirilebilir varlıkların belirlenmesine yardımcı olur
Değiştirilebilir tümcecik listeleri, LUSıS uygulamanızın performansını ayarlamak için iyi bir yoldur. Uygulamanız, doğru amaca göre tahmin etmeye veya varlıkları tanıma karşı bir sorunla karşılaşmışsa, en fazla alışılmadık sözcükler içerip içermediğini veya anlamı anlamlı olabilecek kelimeleri düşünün. Bu sözcükler, bir tümcecik listesine dahil etmek için iyi adaylardır.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Tümcecik listeleri, bağlamı daha iyi anlamak için amaçları belirlemesine yardımcı olur
Tümcecik listesi, bir LUSıS için bir yönerge değildir ve her zaman tümcecik listesindeki tüm terimleri tamamen aynı şekilde etiketleyebilir. Yalnızca bir ipucu. Örneğin, "PATTI" ve "Selma" adlarının olduğunu gösteren bir tümcecik listeniz olabilir, ancak LUP, "Şunun için PATTI için" bir ayırma, akşam yemeği için de Selma 'nın yönleri, Georgia ". 

Tümcecik listesi eklemek, bir amaca daha fazla örnek eklemek için bir alternatiftir. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Tümcecik listeleri ve liste varlıkları ne zaman kullanılır?
Her iki ifade listesi ve [liste varlığı](reference-entity-list.md) , her türlü amaç genelinde bir şekilde etkilenirken, bu, her biri farklı bir şekilde olur. Amaç tahmin Puanını etkilemek için bir tümcecik listesi kullanın. Tam metin eşleşmesi için varlık ayıklamayı etkilemek üzere bir liste varlığı kullanın. 

### <a name="use-a-phrase-list"></a>Tümcecik listesi kullanma
Bir ifade listesi ile, LUSıS, bir listedeki öğeler olarak tam bir eşleşmeye benzer ancak tam eşleşme olmayan öğeleri belirlemek için yine de hesap ve genelleştirilebilen bir içerik alabilir. LUSıS uygulamanızın bir kategorideki yeni öğeleri genelleştirabilmesi ve belirleyebilmesi gerekiyorsa, bir tümcecik listesi kullanın. 

Yeni kişilerin adlarını tanıması gereken bir toplantı Zamanlayıcısı veya yeni ürünleri tanıması gereken bir envanter uygulaması gibi bir varlığın yeni örneklerini tanıyabilmek istediğinizde, basit bir varlık gibi başka bir makine tarafından öğrenilen varlık türü kullanın. Ardından, LUTO 'nın varlığa benzer diğer sözcükleri bulmasına yardımcı olan sözcüklerin ve tümceciklerin bir ifade listesi oluşturun. Bu liste, bu sözcüklerin değerine daha fazla anlam ekleyerek, bu kelimelerin örneklerini tanımak için LUSıS 'e kılavuzluk eder. 

Tümcecik listeleri, hem amaçları hem de varlıkların anlaşılmasına yardımcı olan, etki alanına özgü sözlük gibidir. Bir tümcecik listesinin yaygın kullanımları, şehir adları gibi uygun adlarla aynıdır. Şehir adı, kısa çizgiler veya kesme işareti dahil olmak üzere birkaç sözcük olabilir.
 
### <a name="dont-use-a-phrase-list"></a>Tümcecik listesi kullanma 
Bir liste varlığı bir varlığın aldığı her değeri açıkça tanımlar ve yalnızca tam olarak eşleşen değerleri tanımlar. Bir liste varlığı, bir varlığın tüm örneklerinin bilinen ve sık değişmediği bir uygulama için uygun olabilir. Örnek olarak, bir restoran menüsünde seyrek olarak değişen yiyecek öğeleri verilebilir. Bir varlıkla tam metin eşleşmesi gerekiyorsa, tümcecik listesi kullanmayın. 

## <a name="best-practices"></a>Önerilen uygulamalar
[En iyi yöntemleri](luis-concept-best-practices.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

LUSıS uygulamanıza özellikler ekleme hakkında daha fazla bilgi edinmek için bkz. [özellik ekleme](luis-how-to-add-features.md) .
