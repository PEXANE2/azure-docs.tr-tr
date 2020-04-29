---
title: Desenler yardım tahmini-LUSıS
titleSuffix: Azure Cognitive Services
description: Bir desen, çok daha fazla sayıda daha fazla bilgi sağlamadan bir amaç için daha fazla doğruluk elde etmenizi sağlar.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75890280"
---
# <a name="patterns-improve-prediction-accuracy"></a>Desenler tahmin doğruluğunu geliştirir
Desenler, birkaç söylenme çok benzer olduğunda doğruluğu artırmak için tasarlanmıştır.  Bir desen, çok daha fazla sayıda daha fazla bilgi sağlamadan bir amaç için daha fazla doğruluk elde etmenizi sağlar. 

## <a name="patterns-solve-low-intent-confidence"></a>Desenler düşük amaçlı güvenilirliği çözüyor
Bir çalışanla ilgili olarak kurumsal grafik üzerinde rapor veren bir Insan kaynakları uygulaması düşünün. Bir çalışanın adı ve ilişkisi verildiğinde, LUSıS ilgili çalışanları döndürür. Bir çalışan, Tom, bir yönetici adı ile bir çalışanı ve: Michael, Rebecca ve Carl adlı bir AST ekibi göz önünde bulundurun.

![Kuruluş grafiğinin görüntüsü](./media/luis-concept-patterns/org-chart.png)

|Konuşmalar|Amaç tahmin edildi|Amaç puanı|
|--|--|--|
|Tom 'ın astı kim?|GetOrgChart|.30|
|Tom 'ın astı kim?|GetOrgChart|.30|

Bir uygulama, farklı uzunluklara, farklı sözcük sıralarına ve hatta farklı sözcüklere ("AST", "Yönet", "Report") sahip 10 ile 20 ' lik arasında bir değer içeriyorsa, luya düşük Güvenirlik puanı döndürebilir. LUSıS 'in sözcük sırasının önemini anlamalarına yardımcı olmak için bir düzen oluşturun. 

Desenler aşağıdaki durumları çözerek: 

* Amaç puanı düşük
* Doğru amaç, en üst puan değildir ancak en üst puana çok yakın değildir. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Desenler amaç garantisi değildir
Desenler bir tahmin teknolojileri karışımı kullanır. Bir düzende bir şablon için amaç ayarlamak, amaç tahminine dair garanti değildir ancak güçlü bir sinyaldir. 

<a name="patterns-do-not-improve-entity-detection"/></a>

## <a name="patterns-do-not-improve-machine-learned-entity-detection"></a>Desenler makine tarafından öğrenilen varlık algılamayı iyileştirmez

Bir model öncelikle amaç ve rollerin tahminine yardımcı olmak için tasarlanmıştır. _Model. herhangi bir_ varlık, serbest biçimli varlıkları ayıklamak için kullanılır. Desenler varlıkları kullanırken, bir desen makine tarafından öğrenilen bir varlığı algılamaya yardımcı olmaz.  

Birden çok kuralı tek bir düzende daraltarak, geliştirilmiş varlık tahminini görmeyi beklememeniz gerekmez. Basit varlıkların tetiklenmesi için, bu tür varlıkları eklemeniz veya liste varlıklarını kullanmanız gerekir, ancak sizin düzeniniz tetikleymeyecektir.

## <a name="patterns-use-entity-roles"></a>Desenler varlık rollerini kullanır
Bir Desendeki iki veya daha fazla varlık bağlamsal olarak ilişkiliyse, desenler hakkında bağlamsal bilgileri ayıklamak için, desenler varlık [rollerini](luis-concept-roles.md) kullanır.  

## <a name="prediction-scores-with-and-without-patterns"></a>Desenler içeren ve olmayan tahmin puanları
Yeterli sayıda örnek utterde verilince, LUSıS, desenler olmadan tahmin güvenini artırabilir. Desenler, çok sayıda söyleyme sağlamak zorunda kalmadan güvenirlik Puanını artırır.  

## <a name="pattern-matching"></a>Desen eşleştirme
Bir düzen, öncelikle düzen içindeki varlıkların algılanması ve ardından düzenin sözcüklerin ve sözcük sırasının geri kalanının doğrulanması ile eşleştirilir. Bir düzenin eşleşmesi için düzende varlıklar gereklidir. Bu model, karakter düzeyinde değil, belirteç düzeyinde uygulanır. 

## <a name="pattern-only-apps"></a>Yalnızca örüntü uygulamalar
Her bir amaç için bir desen olduğu sürece, örnekleri olmayan amaçlar içeren bir uygulama oluşturabilirsiniz. Tek bir uygulama için, bu, örnek bir varlık gerektirdiğinden, bu model makine tarafından öğrenilen varlıklar içermemelidir. 

## <a name="best-practices"></a>En iyi uygulamalar
[En iyi yöntemleri](luis-concept-best-practices.md)öğrenin.

## <a name="pattern-syntax"></a>Desen söz dizimi

[Model sözdizimi başvurusundan](reference-pattern-syntax.md)bir model sözdizimi öğrenin. 

## <a name="next-steps"></a>Sonraki adımlar

Desenler hakkında daha fazla bilgi edinin:

* [Desenler ekleme](luis-how-to-model-intent-pattern.md)
* [Nasıl eklenir. herhangi bir varlık](luis-how-to-add-entities.md#add-a-patternany-entity)
* [Desenler sözdizimi](reference-pattern-syntax.md)

> [!div class="nextstepaction"]
> [Bu öğreticide desenleri uygulamayı öğrenin](luis-tutorial-pattern.md)
