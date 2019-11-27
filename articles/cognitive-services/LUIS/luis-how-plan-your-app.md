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
ms.date: 11/20/2019
ms.author: diberry
ms.openlocfilehash: 6a155f4c43da03ccdc40d289742918973aa6da7b
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326786"
---
# <a name="plan-your-luis-app-schema-with-subject-domain-and-data-extraction"></a>LUSıS uygulama şemanızı konu etki alanı ve veri ayıklama ile planlayın

Bir LUSıS uygulama şeması, konu [etki alanınız](luis-glossary.md#domain)ile ilgili [amaçları](luis-glossary.md#intent) ve [varlıkları](luis-glossary.md#entity) içerir. Amaç, Kullanıcı [araslarını](luis-glossary.md#utterance)sınıflandırır ve varlıklar, Kullanıcı dıklarındaki verileri ayıklar.

## <a name="identify-your-domain"></a>Etki alanınızı tanımlayın

Bir LUSıS uygulaması, konu etki alanı etrafında ortalanır. Örneğin, Bilet, fışıklar, otel ve Kiralama otomobillerini işleyen bir seyahat uygulamasına sahip olabilirsiniz. Başka bir uygulama, uygulama, uygunluk çalışmalarını izlemeyi ve hedeflerini ayarlama ilgili içerik sağlayabilir. Etki alanını tanımlamak, etki alanınız için uygun olan kelimeleri veya tümceleri bulmanıza yardımcı olur.

> [!TIP]
> LUSıS birçok yaygın senaryo için [önceden oluşturulmuş etki alanları](luis-how-to-use-prebuilt-domains.md) sunar. Önceden oluşturulmuş bir etki alanı bir başlangıç noktası olarak uygulamanız için kullanıp kullanamayacağını denetleyin.

## <a name="identify-your-intents"></a>Hedefleri tanımlayın

Uygulamanızın görevi için önemli olan [amaçları](luis-concept-intent.md) göz önünde bulundurun.

Bir kitap ve kullanıcının hedefteki hava durumunu denetlemek için işlevleri ile bir seyahat uygulaması örneği ele alalım. Bu eylemler için `BookFlight` ve `GetWeather` amaçlarını tanımlayabilirsiniz.

Daha fazla işlev içeren daha karmaşık bir uygulamada, daha fazla amaç vardır ve bu amaçları dikkatle tanımlamanız gerekir, böylelikle amaçlar çok özgü değildir. Örneğin, `BookFlight` ve `BookHotel` ayrı amaçlar olması gerekebilir, ancak `BookInternationalFlight` ve `BookDomesticFlight` çok benzer olabilir.

> [!NOTE]
> Yalnızca kullanmak için en iyi bir uygulamadır uygulamanızın işlevleri gerçekleştirmek gerektiği kadar amacı. Çok fazla ıntents tanımlarsanız, konuşma doğru sınıflandırmak LUIS için daha zor hale gelir. Çok az sayıda tanımlarsanız, bu durum örtüşeceğinden genel olabilir.

Genel Kullanıcı amacını belirlemeniz gerekmiyorsa, tüm örnek Kullanıcı araslarını `None` amacına ekleyin. Uygulamanız daha fazla amaç gerektiren bir şekilde büyürse, daha sonra oluşturabilirsiniz.

## <a name="create-example-utterances-for-each-intent"></a>Her bir amaç için örnek konuşma oluşturma

Kullanmaya başlamak için, her amaç için çok fazla sayıda konuşma oluşturmaktan kaçının. Hedefleri belirledikten sonra, amaç başına 15 ila 30 örnek oluşturma oluşturun. Her söylük, daha önce sağlandıklarından farklı olmalıdır. Genel sözcük sayısı, kelime seçimi, fiil zaman hali ve noktalama işaretleri de iyi bir şekilde bulunur.

Daha fazla bilgi için bkz. [lusıs uygulamaları için iyi BIR değer anlama](luis-concept-utterance.md).

## <a name="identify-your-entities"></a>Varlıklarınızı tanımlayın

Ayıklanan istediğiniz varlıkları örnek konuşma belirleyin. Bir uçuş sağlamak için hedef, tarih, Airline, Bilet kategorisi ve seyahat sınıfı gibi bilgilere ihtiyacınız vardır. Bu veri türleri için varlıklar oluşturun ve sonra örnek söyleylerdeki [varlıkları](luis-concept-entity-types.md) işaretleyin. Varlıklar, amacı yerine getirmeye önem taşır.

Uygulamanızda hangi varlıkların kullanılacağını belirlerken, nesne türleri arasındaki ilişkileri yakalamak için farklı türde varlıkların olduğunu aklınızda bulundurun. [Luve Içindeki varlıklar](luis-concept-entity-types.md) , farklı türler hakkında daha fazla ayrıntı sağlar.

> [!TIP]
> LUSıS, yaygın, konuşma Kullanıcı senaryolarında [önceden oluşturulmuş varlıklar](luis-prebuilt-entities.md) sunmaktadır. Önceden oluşturulmuş varlıkları, uygulama geliştirme için bir başlangıç noktası olarak kullanmayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUSıS geliştirme lifecylce öğrenme](luis-concept-app-iteration.md)

