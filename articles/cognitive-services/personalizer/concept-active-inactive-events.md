---
title: Aktif ve aktif olmayan olaylar - Personalizer
description: Bu makalede, Personalizer hizmeti içinde etkin ve etkin olmayan olayların kullanımı açıklenmektedir.
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: a8f27542208965e2b820b9fc45cfcc5353a7f193
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624258"
---
# <a name="active-and-inactive-events"></a>Etkin olan ve olmayan olaylar

**Etkin** bir olay, sonucu müşteriye göstereceğiniz ve ödül puanını belirleyeceğiniz Rank'a yapılan herhangi bir çağrıdır. Bu varsayılan davranıştır.

**Etkin olmayan** bir olay, iş mantığı nedeniyle kullanıcının önerilen eylemi görüp görmeyeceğiniz sıralamaya yapılan bir çağrıdır. Bu, Personalizer'ın varsayılan ödülle eğitilememesine izin verir. Etkin olmayan olaylar Ödül API'sini aramamalıdır.

Öğrenme döngüsünün olayın gerçek türünü bilmesi önemlidir. Etkin olmayan bir olayın ödül çağrısı olmaz. Etkin bir olay bir Ödül çağrısı na sahip olmalıdır, ancak API çağrısı hiç yapılmazsa, varsayılan ödül puanı uygulanır. Bir olayın durumunu kullanıcı deneyimini etkileyeceğini bildiğiniz anda etkin olmayandan etkin e duruma değiştirin.

## <a name="typical-active-events-scenario"></a>Tipik etkin olaylar senaryosu

Uygulamanız Rank API'yi aradığında, uygulamanın **ödülActionId** alanında göstermesi gereken eylemi alırsınız.  O andan itibaren, Personalizer aynı eventId'e sahip bir ödül puanı içeren bir Ödül çağrısı bekler. Ödül puanı gelecekteki Rank aramaları için modeli eğitmek için kullanılır. EventId için Ödül çağrısı alınmazsa, varsayılan ödül uygulanır. [Varsayılan ödüller](how-to-settings.md#configure-rewards-for-the-feedback-loop) Azure portalındaki Personalizer kaynağınızda ayarlanır.

## <a name="other-event-type-scenarios"></a>Diğer olay türü senaryoları

Bazı senaryolarda, sonucun kullanIlip kullanılmayacağını veya kullanıcıya görüntüleneceğini bile bilmeden uygulamanın Rank'ı araması gerekebilir. Bu durum, örneğin, tanıtılan içeriğin sayfa oluşturmanın bir pazarlama kampanyası tarafından üzerine yazıldığı durumlarda gerçekleşebilir. Rank çağrısının sonucu hiç kullanılmadıysa ve kullanıcı bunu hiç görmediyse, ilgili bir Ödül çağrısı göndermeyin.

Genellikle, bu senaryolar şu durumlarda gerçekleşir:

* Kullanıcının görebileceği veya göremeyen kullanıcı kullanıcısını önceden oluşturmanız.
* Uygulamanız, Rank çağrılarının çok az gerçek zamanlı bağlamla yapıldığı ve uygulamanın çıktıyı kullanıp kullanmayabileceği veya kullanamayabileceği tahmine dayalı kişiselleştirme yapıyor.

Bu gibi durumlarda, Etkinliğin _etkin_olmasını isteyerek Rank'ı aramak için Personalizer'ı kullanın. Personalizer bu etkinlik için bir ödül beklemez ve varsayılan bir ödül uygulamaz.

Daha sonra iş mantığınızda, uygulama Rank çağrısındaki bilgileri kullanıyorsa, olayı _etkinleştirmeniz_ için etkinleştirmeniz gerekmektedir. Etkinlik aktif olur olmaz, Personalizer bir etkinlik ödülü bekler. Ödül API'sine açık bir arama yapılmazsa, Personalizer varsayılan ödül uygular.

## <a name="inactive-events"></a>Etkin olmayan olaylar

Bir etkinlik için eğitimi devre dışı kalarak Rank'ı kullanarak rank'ı `learningEnabled = False`arayın.

Etkin olmayan bir olay için, eventId için bir ödül gönderirseniz `activate` veya bu eventId için API'yi ararsanız öğrenme dolaylı olarak etkinleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Ödül puanını nasıl belirleyip hangi verileri göz önünde bulundurmanızı](concept-rewards.md)öğrenin.
