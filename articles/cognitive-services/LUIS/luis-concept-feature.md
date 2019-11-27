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
ms.date: 11/03/2019
ms.author: diberry
ms.openlocfilehash: b151043babd6c67a17f704bb671a415fcc3ba7b2
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280906"
---
# <a name="machine-learned-features"></a>Makine tarafından öğrenilen Özellikler 

Machine Learning 'de, bir _özellik_ , sistem engellerinizin öğrendiği & öğrendiği verilerin bir ayırt edici nitelik veya özniteliğidir. Language Understanding (LUSıS) ' de bir özellik, amaç ve varlıklarınız hakkında ne kadar önemli olduğunu açıklar ve açıklar.

[ÖNIZLEME luın portalında](https://preview.luis.ai)özellikler, amacı veya varlığı _anlatmak_ için kullanıldıklarından _tanımlayıcılardır_ .  

## <a name="features-_descriptors_-in-language-understanding"></a>Language Understanding Özellikler (_tanımlayıcılar_)

Tanımlayıcılar olarak da bilinen özellikler, örnek söylerinizi belirlemek Language Understanding yardım için ipuçlarına açıklama sağlar. Şu özellikler mevcuttur: 

* Amaçlar veya varlıklar için özellik olarak ifade listesi
* Amaçlar veya varlıklara özellikler olarak varlıklar

Özellikler, model ayrıştırma için şemanızın gerekli bir parçası olarak düşünülmelidir. 

## <a name="what-is-a-phrase-list"></a>Tümcecik listesi nedir?

Tümcecik listesi, belirlemeye çalıştığınız kavramı belirlemeye yardımcı olan sözcüklerin, deyimlerin, sayıların veya diğer karakterlerin bir listesidir. Liste büyük/küçük harfe duyarlıdır. 

## <a name="when-to-use-a-phrase-list"></a>Tümcecik listesi ne zaman kullanılır?

Bir tümcecik listesi ile, LUSıS, bağlamı ve genelleştirir, ancak tam metin eşleşmesi değil, öğesine benzer öğeleri belirler. LUSıS uygulamanızın yeni öğeleri genelleştirabilmesi ve belirleyebilmesi gerekiyorsa, bir tümcecik listesi kullanın. 

Yeni kişilerin adlarını tanıması gereken bir toplantı Zamanlayıcısı veya yeni ürünleri tanıması gereken bir envanter uygulaması gibi yeni örnekleri tanıyabilmek istediğinizde, makine tarafından öğrenilen bir varlıkla başlayın. Ardından, LUO 'NUN benzer anlamlara sahip sözcüklerin bulmasına yardımcı olan bir tümcecik listesi oluşturun. Bu tümcecik listesi, LUSıS 'in bu sözcüklerin değerine ek anlam ekleyerek örnekleri tanımasını sağlar. 

Hedefleri ve varlıkları anlama kalitesini geliştirme ile yardımcı etki alanına özel sözlük ifade listeleri gibidir. 

## <a name="considerations-when-using-a-phrase-list"></a>Tümcecik listesi kullanırken dikkat edilecek noktalar

Bir tümcecik listesi, varsayılan olarak uygulamadaki tüm modellere uygulanır. Bu, tüm amaçları ve varlıkları çapraz bir şekilde ifade eden tümcecik listelerinde çalışacaktır. Ölülebilirlik için, yalnızca ilgili olduğu modellere bir tümcecik listesi uygulamanız gerekir. 

Bir tümcecik listesi oluşturur (varsayılan olarak genel olarak oluşturulur), daha sonra bunu belirli bir modele tanımlayıcı (özellik) olarak uygularsanız, diğer modellerden kaldırılır. Bu kaldırma, uygulandığı model için tümcecik listesine ilgi ekler ve modelin sağladığı doğruluğu artırmaya yardımcı olur. 

`enabledForAllModels` bayrağı, bu model kapsamını API 'de denetler. 

<a name="how-to-use-phrase-lists"></a>

### <a name="how-to-use-a-phrase-list"></a>Tümcecik listesi kullanma

Amaç veya varlığınızda şu gibi önemli sözcükler ya da tümcecikler olduğunda [bir tümcecik listesi oluşturun](luis-how-to-add-features.md) :

* sektör koşulları
* Argo
* kısaltmaları
* şirkete özgü dil
* başka bir dilden, ancak uygulamanızda sık kullanılan dil
* örneğinizdeki önemli sözcükler ve tümcecikler

Olası her sözcük veya **tümceciği eklemeyin.** Bunun yerine, bir kerede birkaç sözcük veya tümce ekleyin, sonra yeniden eğitme ve yayımlama. Liste zamanla büyüdükçe, bazı terimlerin birçok formu (eş anlamlı) olduğunu fark edebilirsiniz. Bunları başka bir listeye bölün. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="when-to-use-an-entity-as-a-feature"></a>Bir varlık özellik olarak ne zaman kullanılır? 

Bir varlık, amaç veya varlık düzeyinde bir özellik olarak eklenebilir. 

### <a name="entity-as-a-feature-to-an-intent"></a>Amaç özelliği olarak varlık

Varlığın algılanması amaç için önemli olduğunda bir amaca tanımlayıcı (özellik) olarak bir varlık ekleyin.

Örneğin, amaç bir uçuş sağlamak için ise ve varlık bilet bilgileri (örneğin, bilgisayar sayısı, kaynak ve hedef) ise, Bilet bilgileri varlığının bulunması, kitap kolu hedefinin tahminiyle ilgili ağırlık eklemesi gerekir. 

### <a name="entity-as-a-feature-to-another-entity"></a>Başka bir varlığa özellik olarak varlık

Varlık (a), varlık (B) için önemli olduğunda bu varlığın (A) algılanabilmesi durumunda başka bir varlığa (B) bir özellik olarak eklenmelidir.

Örneğin, sokak adresi varlığı (A) algılanırsa, sokak adresini bulma (A), sevkiyat adresi varlığı için tahmine ağırlık ekler (B). 

## <a name="best-practices"></a>En iyi uygulamalar
[En iyi yöntemleri](luis-concept-best-practices.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

LUSıS uygulamanıza özellikler ekleme hakkında daha fazla bilgi edinmek için bkz. [özellik ekleme](luis-how-to-add-features.md) .