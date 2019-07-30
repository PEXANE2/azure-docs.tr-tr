---
title: Uygulamanızı planlayın-LUSıS
titleSuffix: Azure Cognitive Services
description: İlgili uygulamayı amaç ve varlıkları anahat ve uygulama planlarınızı Language Understanding Intelligent Services (LUIS içinde) oluşturun.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: dc648b30dc1236080be06044f510557ae0ce9476
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638304"
---
# <a name="plan-your-luis-app-with-subject-domain-intents-and-entities"></a>LUSıS uygulamanızı konu etki alanı, amaçlar ve varlıklar ile planlayın

Uygulamanızı planlamak için, konu alanı etki alanınızı belirlemeniz gerekir. Bu, uygulamanızla ilgili olası amaçları ve varlıkları içerir.  

## <a name="identify-your-domain"></a>Etki alanınızı tanımlayın

Bir LUIS uygulaması bir etki alanına özgü konu ortalanır.  Örneğin, kayıt biletleri, uçuşlar, Oteller ve kiralama otomobiller gerçekleştiren bir seyahat uygulaması olabilir. Başka bir uygulama, uygulama, uygunluk çalışmalarını izlemeyi ve hedeflerini ayarlama ilgili içerik sağlayabilir. Etki alanını tanıtan sözcük ve tümcecikleri etki alanınız için önemli olan bulmanıza yardımcı olur.

> [!TIP]
> LUIS sunar [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) birçok yaygın senaryo için.
> Önceden oluşturulmuş bir etki alanı bir başlangıç noktası olarak uygulamanız için kullanıp kullanamayacağını denetleyin.

## <a name="identify-your-intents"></a>Hedefleri tanımlayın

Düşünün [hedefleri](luis-concept-intent.md) uygulamanızın görev için önemli. Bir kitap ve kullanıcının hedefteki hava durumunu denetlemek için işlevleri ile bir seyahat uygulaması örneği ele alalım. Bu eylemler için "BookFlight" ve "GetWeather" ıntents tanımlayabilirsiniz. Daha karmaşık bir daha fazla işlev uygulaması, daha fazla amacı sahiptir ve bunları özenle çok belirli olmaması için tanımlamanız gerekir. Örneğin, "BookFlight" ve "BookHotel" ayrı ıntents olması gerekebilir, ancak "BookInternationalFlight" ve "BookDomesticFlight" çok benzer olabilir.

> [!NOTE]
> Yalnızca kullanmak için en iyi bir uygulamadır uygulamanızın işlevleri gerçekleştirmek gerektiği kadar amacı. Çok fazla ıntents tanımlarsanız, konuşma doğru sınıflandırmak LUIS için daha zor hale gelir. Çok tanımlarsanız birkaç, bunlar olabilir çakıştığı için farklı şekilde genel.

## <a name="create-example-utterances-for-each-intent"></a>Her bir amaç için örnek konuşma oluşturma

Hedefleri belirledikten sonra her bir amaç için 15 ila 30 örnek oluşturma oluşturun. Kullanmaya başlamak için bu sayıdan daha az sahip değildir veya her bir amaç için çok fazla sayıda utterme oluşturun. Her utterance önceki utterance farklı olmalıdır. Konuşma iyi çeşitli genel sözcük sayısı, sözcük seçimi, fiil şimdiki ve noktalama işaretleri içerir. 

Daha fazla [bilgi için,](luis-concept-utterance.md) bu bilgileri gözden geçirin.

## <a name="identify-your-entities"></a>Varlıklarınızı tanımlayın

Ayıklanan istediğiniz varlıkları örnek konuşma belirleyin. Bir uçuş sağlamak için hedef, tarih, Airline, Bilet kategorisi ve seyahat sınıfı gibi bilgilere ihtiyacınız vardır. Bu veri türleri için varlıklar oluşturun ve ardından, bir amacı elde etmek için önemli olduklarından, örnek türlerdeki [varlıkları](luis-concept-entity-types.md) işaretleyin. 

Uygulamanızda kullanmak için hangi varlıkları belirlerken, farklı nesne türleri arasındaki ilişkileri yakalamak için varlıkların olduğunu aklınızda bulundurun. [LUIS varlıklarda](luis-concept-entity-types.md) farklı türleri hakkında daha fazla ayrıntı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanız, yayımlanan eğitildi ve konuşma uç noktası alır sonra tahmin geliştirmelerle uygulamak planlama [etkin olarak öğrenmeye](luis-how-to-review-endpoint-utterances.md), [tümcecik listeleri](luis-concept-feature.md), ve [desenleri](luis-concept-patterns.md). 


* Bkz: [Language Understanding Intelligent Services (LUIS) ilk uygulamanızı oluşturma](luis-get-started-create-app.md) LUIS uygulaması oluşturma Hızlı Kılavuz.
