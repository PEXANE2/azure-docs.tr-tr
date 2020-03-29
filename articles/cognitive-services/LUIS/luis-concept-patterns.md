---
title: Desenler tahmin yardımcı olur - LUIS
titleSuffix: Azure Cognitive Services
description: Bir desen çok daha fazla söyleyiş sağlamadan bir niyet için daha fazla doğruluk elde etmenizi sağlar.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 11/11/2019
ms.author: diberry
ms.openlocfilehash: 6c1b548de25369c162b4a08dfa20fce62c17f99f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75890280"
---
# <a name="patterns-improve-prediction-accuracy"></a>Desenler tahmin doğruluğunu artırır
Desenler, birkaç söyleyiş çok benzer olduğunda doğruluğu artırmak için tasarlanmıştır.  Bir desen çok daha fazla söyleyiş sağlamadan bir niyet için daha fazla doğruluk elde etmenizi sağlar. 

## <a name="patterns-solve-low-intent-confidence"></a>Desenler düşük niyet güvenini çözüyor
Bir çalışanla ilgili olarak kuruluş grafiğinde rapor veren bir İnsan Kaynakları uygulaması düşünün. Bir çalışanın adı ve ilişkisi göz önüne alındığında, LUIS ilgili çalışanları döndürür. Bir yönetici adı Alice ile bir çalışan, Tom düşünün ve astları bir ekip adlı: Michael, Rebecca ve Carl.

![Organizasyon grafiğinin görüntüsü](./media/luis-concept-patterns/org-chart.png)

|Konuşmalar|Öngörülen niyet|Niyet puanı|
|--|--|--|
|Tom'un astı kim?|GetorgChart|.30|
|Tom'un astı kim?|GetorgChart|.30|

Bir uygulamanın farklı cümle uzunluklarına, farklı sözcük sırasına ve hatta farklı sözcüklere ("ast", "yönet", "rapor" eşanlamlısı) sahip 10 ila 20 kelimesi varsa, LUIS düşük bir güven puanı döndürebilir. LUIS'in sipariş sözcüğünün önemini anlamasına yardımcı olacak bir desen oluşturun. 

Desenler aşağıdaki durumları çözer: 

* Niyet puanı düşük
* Doğru niyet en yüksek puan değil, en yüksek skora çok yakındır. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Desenler niyet garantisi değildir
Desenler tahmin teknolojilerinin bir karışımını kullanır. Bir desen deseni bir şablon söyleyerek bir niyet ayarlama niyet tahmini bir garanti değil, güçlü bir sinyaldir. 

<a name="patterns-do-not-improve-entity-detection"/></a>

## <a name="patterns-do-not-improve-machine-learned-entity-detection"></a>Desenler, makinede öğrenilen varlık algılamayı iyileştirmez

Bir desen öncelikle niyet ve rollerin tahmin yardımcı olmak içindir. _Desen.herhangi bir_ varlık serbest formdaki varlıkları ayıklamak için kullanılır. Desenler varlıkları kullanırken, desen makinede öğrenilen bir varlığı algılamaya yardımcı olmaz.  

Birden çok sözcüğü tek bir desene daraltmanız durumunda gelişmiş varlık tahmini görmeyi beklemeyin. Basit varlıkların ateşlemesi için, kalıpları eklemeniz veya liste varlıklarını kullanmanız gerekir.

## <a name="patterns-use-entity-roles"></a>Desenler varlık rollerini kullanır
Bir desendeki iki veya daha fazla varlık bağlamsal olarak ilişkiliyse, desenler varlıklar hakkında bağlamsal bilgi ayıklamak için varlık [rollerini](luis-concept-roles.md) kullanır.  

## <a name="prediction-scores-with-and-without-patterns"></a>Desenli ve desenlersiz tahmin puanları
Yeterli örnek söyleyiş göz önüne alındığında, LUIS desenler olmadan tahmin güvenini artırmak mümkün olacaktır. Desenler kadar çok söyleyiş sağlamak zorunda kalmadan güven puanı artırmak.  

## <a name="pattern-matching"></a>Desen eşleştirme
Desen, önce desen içindeki varlıkları algılayarak, daha sonra desenin geri kalanını ve sözcük sırasını doğrulayarak eşleşir. Bir desen eşleşecek şekilde desende varlıklar gereklidir. Desen, karakter düzeyinde değil, belirteç düzeyinde uygulanır. 

## <a name="pattern-only-apps"></a>Yalnızca desenli uygulamalar
Her amaç için bir desen olduğu sürece, örnek söyleyişleri olmayan kasıtlı bir uygulama oluşturabilirsiniz. Yalnızca desenli bir uygulama için desen, örnek sözlemeler gerektirdiğinden, makineden öğrenilen varlıklar içermemelidir. 

## <a name="best-practices"></a>En iyi uygulamalar
[En iyi uygulamaları](luis-concept-best-practices.md)öğrenin.

## <a name="pattern-syntax"></a>Desen söz dizimi

[Desen sözdizimi başvurudan desen sözdizimini](reference-pattern-syntax.md)öğrenin. 

## <a name="next-steps"></a>Sonraki adımlar

Desenler hakkında daha fazla bilgi edinin:

* [Desen ekleme](luis-how-to-model-intent-pattern.md)
* [Desen ekleme nasıl.herhangi bir varlık](luis-how-to-add-entities.md#add-a-patternany-entity)
* [Desenler Sözdizimi](reference-pattern-syntax.md)

> [!div class="nextstepaction"]
> [Bu öğreticide desenleri nasıl uygulayacağınızı öğrenin](luis-tutorial-pattern.md)
