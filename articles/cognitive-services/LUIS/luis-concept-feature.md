---
title: Özellikler - LUIS
titleSuffix: Azure Cognitive Services
description: Etiketlemek veya sınıflandırmak istediğiniz girişi nasıl tanıyacağınız hakkında ipuçları sağlamak için bir dil modeline özellikler ekleyin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79221001"
---
# <a name="machine-learned-features"></a>Makinede öğrenilen özellikler 

Makine öğreniminde bir _özellik,_ sisteminizin gözlemlediği & öğrendiği verilerin ayırt edici bir özelliği veya niteliğidir. Dil Anlama (LUIS) 'de bir özellik, amacınız ve varlıklarınız hakkında neyin önemli olduğunu açıklar ve açıklar.

Önizleme [LUIS portalında,](https://preview.luis.ai)özellikleri _tanımlayıcıdır,_ çünkü amaç veya varlığı _tanımlamak_ için kullanılırlar.  

## <a name="features-_descriptors_-in-language-understanding"></a>Dil Anlamada Özellikler (_tanımlayıcılar)_

Tanımlayıcı olarak da bilinen özellikler, Dil Anlama'nın örnek sözcüklerini tanımlamasına yardımcı olacak ipuçlarını açıklar. Şu özellikler mevcuttur: 

* Niyetveya tüzel kişiliklere özellik olarak ifade listesi
* Niyetlere veya varlıklara özellik olarak varlıklar

Özellikler, model ayrıştırma için şemanızın gerekli bir parçası olarak düşünülmelidir. 

## <a name="what-is-a-phrase-list"></a>İfade listesi nedir

Tümcecik listesi, tanımlamaya çalıştığınız kavramı tanımlamaya yardımcı olan sözcüklerin, tümceciklerin, sayıların veya diğer karakterlerin listesidir. Liste büyük/küçük harf duyarsız. 

## <a name="when-to-use-a-phrase-list"></a>Tümcecik listesi ne zaman kullanılır?

Bir tümcecik listesiyle, LUIS bağlamı dikkate alır ve tam bir metin eşleşmesine benzer, ancak tam olmayan öğeleri tanımlamak için genelleştirir. Yeni öğeleri genelleştirebilmek ve tanımlayabilmek için LUIS uygulamanıza ihtiyacınız varsa, bir ifade listesi kullanın. 

Yeni kişilerin adlarını tanıması gereken bir toplantı zamanlayıcısı veya yeni ürünleri tanıması gereken bir envanter uygulaması gibi yeni örnekleri tanıyabilmek istediğinizde, makinede öğrenilen bir varlıkla başlayın. Ardından, LUIS'in benzer anlamdaki sözcükleri bulmasına yardımcı olan bir ifade listesi oluşturun. Bu ifade listesi, luis'in bu sözcüklerin değerine ek önem ekleyerek örnekleri tanımasını yönlendirir. 

Tümcecik listeleri, hem amaçların hem de varlıkların anlama kalitesini artırmaya yardımcı olan etki alanına özgü kelime dağarcığı gibidir. 

## <a name="considerations-when-using-a-phrase-list"></a>İfade listesi kullanırken dikkat edilmesi gerekenler

Varsayılan olarak uygulamadaki tüm modellere bir ifade listesi uygulanır. Bu, tüm niyet ve varlıkları geçebilecek tümcecik listeleri için çalışır. Decomposability için, yalnızca ilgili modellere bir tümcecik listesi uygulamanız gerekir. 

Bir tümcecik listesi oluşturursanız (varsayılan olarak genel olarak oluşturulur), daha sonra belirli bir modele tanımlayıcı (özellik) olarak uygularsanız, diğer modellerden kaldırılır. Bu kaldırma, uygulandığı modeliçin ifade listesine alaka ekler ve modelde sağladığı doğruluğu artırmaya yardımcı olur. 

Bayrak `enabledForAllModels` API'deki bu model kapsamını denetler. 

<a name="how-to-use-phrase-lists"></a>

### <a name="how-to-use-a-phrase-list"></a>İfade listesi nasıl kullanılır?

Amacınız veya tüzel kişiliğiniz gibi önemli sözcük veya tümceciklere sahipolduğunda [bir ifade listesi oluşturun:](luis-how-to-add-features.md)

* endüstri terimleri
* Argo
* Kısaltma
* şirkete özel dil
* başka bir dilden gelen ancak uygulamanızda sık kullanılan dil
* örnek sözlerindeki anahtar sözcükler ve ifadeler

Mümkün olan her kelimeyi veya tümceciği **eklemeyin.** Bunun yerine, aynı anda birkaç sözcük veya tümcecik ekleyin, ardından yeniden eğitin ve yayımlayın. Liste zamanla büyüdükçe, bazı terimlerin birçok biçimi (eşanlamlısı) olduğunu görebilirsiniz. Bunları başka bir listeye ayır. 

<a name="phrase-lists-help-identify-simple-exchangeable-entities"></a>

## <a name="when-to-use-an-entity-as-a-feature"></a>Bir varlığın özellik olarak ne zaman kullanılacağı 

Bir varlık, amaç veya varlık düzeyinde bir özellik olarak eklenebilir. 

### <a name="entity-as-a-feature-to-an-intent"></a>Bir amacın özelliği olarak varlık

Bu varlığın algılanması amaç için önemli olduğunda, bir amaca tanımlayıcı (özellik) olarak bir varlık ekleyin.

Örneğin, amaç bir uçuş rezervasyonuysa ve varlık bilet bilgileriyse (koltuk sayısı, menşei ve varış noktası gibi), bilet bilgileri varlığını bulmak kitap uçuş niyetinin tahminine ağırlık katmalıdır. 

### <a name="entity-as-a-feature-to-another-entity"></a>Başka bir varlığın özelliği olarak varlık

Bir varlık (A), varlığın (B) tahmini için önemli olduğunda, bir varlık (B) özelliği olarak başka bir varlığa (B) eklenmelidir.

Örneğin, sokak adresi varlığı (A) algılanırsa, (A) sokak adresini bulmak, sevkiyat adresi varlığı (B) için tahmine ağırlık katar. 

## <a name="best-practices"></a>En iyi uygulamalar
[En iyi uygulamaları](luis-concept-best-practices.md)öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

LUIS uygulamanıza nasıl özellikler ekleyeceğiniz hakkında daha fazla bilgi edinmek için [Özellikler Ekle'ye](luis-how-to-add-features.md) bakın.