---
title: Uygulamanızı planlayın - LUIS
description: İlgili uygulama niyetlerini ve varlıklarını anahatlatın ve ardından Dil Anlama Akıllı Hizmetleri'nde (LUIS) uygulama planlarınızı oluşturun.
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: dfed27a05973a2ea2e9a97eaa1c233b847b33d87
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81382301"
---
# <a name="plan-your-luis-app-schema-with-subject-domain-and-data-extraction"></a>Luis uygulama şemanızı konu etki alanı ve veri çıkarma ile planlayın

Bir LUIS uygulama şeması, konu [alanadınız](luis-glossary.md#domain)ile ilgili niyet ve [tüzel](luis-glossary.md#entity) [kişilikleri](luis-glossary.md#intent) içerir. Niyetler kullanıcı [sözcüklerini](luis-glossary.md#utterance)sınıfa sınıfa ve varlıklar kullanıcı sözcüklerinden veri ayıklar.

## <a name="identify-your-domain"></a>Etki alanınızı tanımlayın

Luis uygulaması bir konu etki alanı etrafında ortalanır. Örneğin, bilet, uçuş, otel ve kiralık araba rezervasyonu yapan bir seyahat uygulamanız olabilir. Başka bir uygulama egzersiz, fitness çabalarını izleme ve hedefler belirleme ile ilgili içerik sağlayabilir. Etki alanını tanımlamak, etki alanınızın izleilgili sözcüklerini veya tümceciklerini bulmanıza yardımcı olur.

> [!TIP]
> LUIS, birçok yaygın senaryo için [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) sunar. Uygulamanız için başlangıç noktası olarak önceden oluşturulmuş bir etki alanını kullanıp kullanamayacağınızı kontrol edin.

## <a name="identify-your-intents"></a>Niyetlerinizi belirleyin

Uygulamanızın görevi için önemli olan [amaçları](luis-concept-intent.md) düşünün.

Bir uçuş rezervasyonu yapmak ve kullanıcının varış noktasındaki hava durumunu kontrol etmek için işlevleri olan bir seyahat uygulaması örneğini ele alalım. Bu eylemlerin `BookFlight` amaçlarını ve `GetWeather` niyetlerini tanımlayabilirsiniz.

Daha fazla işleve sahip daha karmaşık bir uygulamada, daha fazla amacınız vardır ve bunları dikkatli bir şekilde tanımlamanız gerekir, böylece niyetler çok özel değildir. Örneğin, `BookFlight` ve `BookHotel` ayrı niyetler olması gerekebilir, ancak `BookInternationalFlight` çok `BookDomesticFlight` benzer olabilir.

> [!NOTE]
> Uygulamanızın işlevlerini gerçekleştirmek için yalnızca istediğiniz kadar niyet kullanmak en iyi yöntemdir. Çok fazla niyet tanımlarsanız, LUIS'in söyleyileri doğru sınıflandırması zorlaşır. Çok az tanımlarsanız, çakışacak kadar genel olabilirler.

Genel kullanıcı niyetini belirlemeniz gerekmiyorsa, tüm örnek kullanıcı söyleyicilerini niyete `None` ekleyin. Uygulamanız daha fazla niyete ihtiyaç duyarsa, bunları daha sonra oluşturabilirsiniz.

## <a name="create-example-utterances-for-each-intent"></a>Her amaç için örnek söyleyişler oluşturma

Öncelikle, her amaç için çok fazla söyleyiş oluşturmaktan kaçının. Niyetleri belirledikten sonra, niyet başına 15-30 örnek sözcük oluşturun. Her söyleyiş daha önce sağlanan söyleyişlerden farklı olmalıdır. Genel sözcük sayısı, sözcük seçimi, fiil zaman ve [noktalama işaretleri](luis-reference-application-settings.md#punctuation-normalization)arasında iyi bir çeşitlilik vardır.

Daha fazla bilgi için, [LUIS uygulamaları için iyi söyleyiş anlama](luis-concept-utterance.md)bakın.

## <a name="identify-your-entities"></a>Varlıklarınızı tanımlayın

Örnek deyişle, ayıklanmalarını istediğiniz varlıkları tanımlayın. Bir uçuş rezervasyonu için varış noktası, tarih, havayolu, bilet kategorisi ve seyahat sınıfı gibi bilgilere ihtiyacınız vardır. Bu veri türleri için varlıklar oluşturun [entities](luis-concept-entity-types.md) ve sonra varlıkları örnek sözlerle işaretleyin. Varlıklar bir amacı gerçekleştirmek için önemlidir.

Uygulamanızda hangi varlıkların kullanılacağını belirlerken, nesne türleri arasındaki ilişkileri yakalamak için farklı türde varlıklar olduğunu unutmayın. [LUIS'teki varlıklar](luis-concept-entity-types.md) farklı türler hakkında daha fazla ayrıntı sağlar.

> [!TIP]
> LUIS, ortak, konuşmalı kullanıcı senaryoları için [önceden oluşturulmuş varlıklar](luis-prebuilt-entities.md) sunar. Uygulama geliştirme için başlangıç noktası olarak önceden oluşturulmuş varlıkları kullanmayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS geliştirme lifecylce öğrenme](luis-concept-app-iteration.md)

