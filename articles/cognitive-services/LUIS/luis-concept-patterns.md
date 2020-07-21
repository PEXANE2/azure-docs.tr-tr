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
ms.openlocfilehash: 1ecdbaa28cad633817497f4af2091b9e38ba7db2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86538063"
---
# <a name="patterns-improve-prediction-accuracy"></a>Desenler tahmin doğruluğunu geliştirir
Desenler, birkaç söylenme çok benzer olduğunda doğruluğu artırmak için tasarlanmıştır.  Bir desen, çok daha fazla sayıda daha fazla bilgi sağlamadan bir amaç için daha fazla doğruluk elde etmenizi sağlar.

## <a name="patterns-solve-low-intent-confidence"></a>Desenler düşük amaçlı güvenilirliği çözüyor
Bir çalışanla ilgili olarak kurumsal grafik üzerinde rapor veren bir Insan kaynakları uygulaması düşünün. Bir çalışanın adı ve ilişkisi verildiğinde, LUSıS ilgili çalışanları döndürür. Bir çalışan, Tom, bir yönetici adı ile bir çalışanı ve: Michael, Rebecca ve Carl adlı bir AST ekibi göz önünde bulundurun.

![Kuruluş grafiğinin görüntüsü](./media/luis-concept-patterns/org-chart.png)

|İfadeler|Amaç tahmin edildi|Amaç puanı|
|--|--|--|
|Tom 'ın astı kim?|GetOrgChart|.30|
|Tom 'ın astı kim?|GetOrgChart|.30|

Bir uygulama, farklı uzunluklara, farklı sözcük sıralarına ve hatta farklı sözcüklere ("AST", "Yönet", "Report") sahip 10 ile 20 ' lik arasında bir değer içeriyorsa, luya düşük Güvenirlik puanı döndürebilir. LUSıS 'in sözcük sırasının önemini anlamalarına yardımcı olmak için bir düzen oluşturun.

Desenler aşağıdaki durumları çözerek:

* Amaç puanı düşük
* Doğru amaç, en üst puan değildir ancak en üst puana çok yakın değildir.

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Desenler amaç garantisi değildir
Desenler bir tahmin teknolojileri karışımı kullanır. Bir düzende bir şablon için amaç ayarlamak, amaç tahminine dair garanti değildir ancak güçlü bir sinyaldir.

<a name="patterns-do-not-improve-entity-detection"></a>

## <a name="patterns-do-not-improve-machine-learning-entity-detection"></a>Desenler makine öğrenimi varlığı algılamayı iyileştirmez

Bir model öncelikle amaç ve rollerin tahminine yardımcı olmak için tasarlanmıştır. _Model. herhangi bir_ varlık, serbest biçimli varlıkları ayıklamak için kullanılır. Desenler varlıkları kullanırken, bir model bir makine öğrenimi varlığını algılamaya yardımcı olmaz.

Birden çok kuralı tek bir düzende daraltarak, geliştirilmiş varlık tahminini görmeyi beklememeniz gerekmez. Basit varlıkların tetiklenmesi için, bu tür varlıkları eklemeniz veya liste varlıklarını kullanmanız gerekir, ancak sizin düzeniniz tetikleymeyecektir.

## <a name="patterns-use-entity-roles"></a>Desenler varlık rollerini kullanır
Bir Desendeki iki veya daha fazla varlık bağlamsal olarak ilişkiliyse, desenler hakkında bağlamsal bilgileri ayıklamak için, desenler varlık [rollerini](luis-concept-roles.md) kullanır.

## <a name="prediction-scores-with-and-without-patterns"></a>Desenler içeren ve olmayan tahmin puanları
Yeterli sayıda örnek utterde verilince, LUSıS, desenler olmadan tahmin güvenini artırabilir. Desenler, çok sayıda söyleyme sağlamak zorunda kalmadan güvenirlik Puanını artırır.

## <a name="pattern-matching"></a>Desen eşleştirme
Bir düzen, öncelikle düzen içindeki varlıkların algılanması ve ardından düzenin sözcüklerin ve sözcük sırasının geri kalanının doğrulanması ile eşleştirilir. Bir düzenin eşleşmesi için düzende varlıklar gereklidir. Bu model, karakter düzeyinde değil, belirteç düzeyinde uygulanır.

## <a name="pattern-only-apps"></a>Yalnızca örüntü uygulamalar
Her bir amaç için bir desen olduğu sürece, örnekleri olmayan amaçlar içeren bir uygulama oluşturabilirsiniz. Tek bir uygulama için, bu, örnek söylemeye ihtiyaç duyduğu için desen makine öğrenimi varlıkları içermemelidir.

## <a name="patternany-entity"></a>Pattern.any varlığı

[!INCLUDE [Pattern.any entity - concepts](./includes/pattern-any-entity.md)]

## <a name="best-practices"></a>Önerilen uygulamalar
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
