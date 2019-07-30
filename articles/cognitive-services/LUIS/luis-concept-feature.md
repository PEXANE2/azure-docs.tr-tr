---
title: Özellikler-LUSıS
titleSuffix: Azure Cognitive Services
description: Bir dil modeli, ipuçlarını, etiket veya sınıflandırmak istediğiniz giriş anlamayı hakkında sağlamaya özellikleri ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: e85f9a90af778a7e68bf761d8d2159e808cf122d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639217"
---
# <a name="phrase-list-features-in-your-luis-app"></a>LUSıS uygulamanızdaki ifade listesi özellikleri

Machine learning'de bir *özellik* bir ayırt edici nitelik veya sisteminizin gözlemler veri özniteliği. 

Bir dil modeli, ipuçlarını, etiket veya sınıflandırmak istediğiniz giriş anlamayı hakkında sağlamaya özellikleri ekleyin. Özellikleri, hedefleri ve varlıkları hem LUIS yardımcı olur, ancak özellikleri, hedefleri ve varlıkları kendilerini değildir. Bunun yerine, özellikler, ilgili koşulları örnekleri sağlayabilir.  

## <a name="what-is-a-phrase-list-feature"></a>Bir ifade listesi özelliği nedir?
Tümcecik listesi, uygulamanız için önemli olan sözcüklerin veya deyimlerin listesidir ve diğer sözcüklerden daha fazlasını yapın. Bir tümcecik listesi, bu sözcüklerin yaklaşık olarak LUO 'ya ek bir sinyal olarak uygulama etki alanının sözlüğüne ekler. LUIS bunları biri hakkında ne öğrenir otomatik olarak başkaları için de uygulanır. Bu liste, tam metin eşleştirmelerinin kapalı bir [liste varlığı](luis-concept-entity-types.md#types-of-entities) değildir.

Tümcecik listeleri, sözcük ve tümcecik için çok sayıda sözcük ve tümcecik kullanan söylenişi örnekleri eklemeniz gerekir.

## <a name="phrase-lists-help-all-models"></a>Tümcecik listeleri tüm modellere yardımcı olur

Tümcecik listeleri belirli bir amaca veya varlığa bağlı değildir, ancak tüm amaçlar ve varlıklara önemli bir artırma olarak eklenmiştir. Amacı, amaç algılamayı ve varlık sınıflandırmasını geliştirmaktır.

## <a name="how-to-use-phrase-lists"></a>İfade listeleri kullanma

Uygulamanızda uygulama için önemli sözcükler veya tümcecikler olduğunda [bir tümcecik listesi oluşturun](luis-how-to-add-features.md) , örneğin:

* sektör koşulları
* Argo
* kısaltmaları
* şirkete özgü dil
* başka bir dilden, ancak uygulamanızda sık kullanılan dil
* örneğinizdeki önemli sözcükler ve tümcecikler

Birkaç sözcük veya tümcecik girdikten sonra, ilgili değerleri bulmak için **öner** özelliğini kullanın. İfade listesi değerlerinize eklemeden önce ilgili değerleri gözden geçirin.

|Liste türü|Amaç|
|--|--|
|Değiştirilebilir|Listedeki başka bir sözcüğe değiştirildiğinde, aynı amaca ve varlık ayıklamasını içeren eş anlamlılar veya sözcükler.|
|Çapraz olmayan|Uygulamanıza özgü uygulama sözlüğü, genellikle bu dildeki diğer sözcüklerden daha fazla.|

### <a name="interchangeable-lists"></a>Değiştirilebilir listeler

Bir *değiştirilebilir* ifade listesi, eş anlamlı olan değerler içindir. Örneğin, tüm su gövdelerini, ve gibi örnek bir örnek elde etmek istiyorsanız: 

* Harika Lakes 'e hangi şehirler yakınlıyor? 
* Gölü Havasu hangi yolda çalışır?
* Nve bitişi nerede? 

Her söylük, su gövdesinden bağımsız olarak hem amaç hem de varlıklar için belirtilmelidir: 

* [Bodyofsu] için hangi şehirler yakın?
* [Bodyofsu] üzerinde hangi yol çalıştırılır?
* [Bodyofsu] başlangıcı ve bitişi nerede? 

Su gövdesi için sözcükler veya ifadeler eş anlamlı olduğundan ve söyleyenlerde birbirinin yerine kullanılabileceği için, tümcecik listesindeki bir **değiştirilebilir** ayarını kullanın. 

### <a name="non-interchangeable-lists"></a>Çapraz olmayan listeler

Çapraz olmayan bir tümcecik listesi, LUSıS 'de algılama yapan bir sinyaldir. Tümcecik listesi, diğer sözcüklerin daha önemli olduğu kelimeleri veya tümceleri gösterir. Bu, hem amacı hem de varlık algılamayı belirlemede yardımcı olur. Örneğin, genel (kültürler genelinde, ancak hala tek bir dilde) seyahat gibi bir konu etki alanınız olduğunu varsayalım. Uygulama için önemli olan ancak eş anlamlı olmayan sözcükler ve deyimler vardır. 

Diğer bir örnek için, nadir, özel ve yabancı sözcükler için, çakışmayan bir tümcecik listesi kullanın. LUIS yabancı kelimeler (dışında uygulamanın kültür) yanı sıra, nadir ve özel sözcük tanıyamadı olabilir. Nadir bir sözcükler kümesini LUIS tanımayı öğrenin bir sınıf oluşturur, ancak eş anlamlılar olmayan değiştirilebilir olmayan ayarını gösterir veya birbirleri ile değiştirilebilir.

Bir tümcecik listesine her bir olası sözcüğü veya tümceciği eklemeyin, bir seferde birkaç sözcük veya tümce ekleyin, sonra yeniden eğitme ve yayımlayın. 

Tümcecik listesi zaman içinde büyüdükçe, bazı terimlerin birçok formu (eş anlamlı) olduğunu fark edebilirsiniz. Bunları, değiştirilebilecek başka bir tümcecik listesine bölün. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="phrase-lists-help-identify-simple-interchangeable-entities"></a>Tümcecik listeleri basit bir değiştirilebilir varlıkların belirlenmesine yardımcı olur
Birbirinin yerine ifade listeleri LUIS uygulamanızın performansını ayarlamak için iyi bir yoludur. Uygulamanızın doğru amaç konuşma tahmin etme veya varlık tanıma sorun varsa, konuşma olağan dışı bir sözcük veya anlamları belirsiz olabilir sözcükler içeren hakkında düşünün. Bu sözcükler tümcecik listesinde içermek için iyi adaylar değildir.

## <a name="phrase-lists-help-identify-intents-by-better-understanding-context"></a>Tümcecik listeler Yardım hedefleri daha iyi anlama bağlamdan tanımlayın
Bir ifade listesi LUIS sıkı eşleştirme işlemi yapmak veya her zaman deyim listesindeki tüm koşulları tamamen aynı etiket için bir yönerge değil. Bu, yalnızca bir ipucu verir. Örneğin, "Patti" ve "Selma" adların olup olmadığını belirten bir ifade listesi olabilir, ancak LUIS kullanmaya devam edebilirsiniz bağlamsal bilgiler farklı bir şey anlamına olduğunu bilmek "2 için bir ayırma sırasında Patti'nın Diner Akşam olun" ve "bana sürüş bulma yönlendirmeler Selma, Gürcistan". 

Bir ifade listesine eklenmesi, daha fazla örnek konuşma eklemeye yönelik bir amacı bir alternatiftir. 

## <a name="when-to-use-phrase-lists-versus-list-entities"></a>Ne zaman tümceciğini kullanın varlıklar listesi listeler
Her iki ifade listesi ve [liste varlığı](reference-entity-list.md) , her türlü amaç genelinde bir şekilde etkilenirken, bu, her biri farklı bir şekilde olur. Hedefi tahmin puanı etkilemek için bir ifade listesini kullanın. Bir liste varlığı, bir tam metin eşleşmesi için varlık ayıklama etkilemek için kullanın. 

### <a name="use-a-phrase-list"></a>Bir ifade listesini kullanın
Bir ifade listesiyle LUIS hala bağlam dikkate alın ve benzer öğeleri, ancak tam bir eşleşme yok, listedeki öğeleri tanımlamak için generalize. LUIS uygulamanızı generalize ve bir kategorideki yeni öğeleri tanımlamak için gerekiyorsa, bir ifade listesini kullanın. 

Yeni kişilerin adlarını tanıması gereken bir toplantı Zamanlayıcısı veya yeni ürünleri tanıması gereken bir envanter uygulaması gibi bir varlığın yeni örneklerini tanıyabilmek istediğinizde, basit bir varlık gibi başka bir makine tarafından öğrenilen varlık türü kullanın. Ardından, başka bir deyişle varlığa benzer Bul LUIS yardımcı olan bir ifade listesini sözcük ve tümcecikleri oluşturun. Bu liste değeri olarak sözcükleri ek anlam ekleyerek varlık örnekleri tanımak için LUIS size yol gösterir. 

Hedefleri ve varlıkları anlama kalitesini geliştirme ile yardımcı etki alanına özel sözlük ifade listeleri gibidir. İfade listesinin ortak kullanım Şehir adları gibi uygun isimleri ' dir. Bir şehir adı kısa çizgi veya kesme işaretleri dahil olmak üzere birkaç sözcük olabilir.
 
### <a name="dont-use-a-phrase-list"></a>Bir ifade listesi kullanma 
Bir liste varlık varlığın alabilir ve yalnızca tam olarak aynı değerleri tanımlar. her bir değeri açıkça tanımlar. Bir liste varlık varlığın tüm örneklerini'nın bilinen ve genellikle değişmez bir uygulama için uygun olabilir. Seyrek değişen bir restoran menüsü Gıda öğelerde verilebilir. Bir varlığın bir tam metin eşleşmesi gerekiyorsa, ifade listesi kullanmayın. 

## <a name="best-practices"></a>En iyi uygulamalar
Bilgi [en iyi uygulamalar](luis-concept-best-practices.md).

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [Özellik Ekle](luis-how-to-add-features.md) LUIS uygulamanızı özellikleri ekleme hakkında daha fazla bilgi için.
