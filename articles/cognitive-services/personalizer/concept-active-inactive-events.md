---
title: Etkin ve etkin olmayan olaylar-kişiselleştirici
description: Bu makalede, kişiselleştirici hizmeti içinde etkin ve etkin olmayan olayların kullanımı ele alınmaktadır.
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 7d1044d02ceba1f3d0996b1fe1c8a9a44b31049b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91253658"
---
# <a name="active-and-inactive-events"></a>Etkin olan ve olmayan olaylar

**Etkin** bir olay, müşterinin sonucunu göstereceğimizi bildiğiniz ve yeniden puanı belirleyebildiğiniz bir dereceye yapılan çağrıdır. Bu, varsayılan davranıştır.

**Etkin olmayan** bir olay, kullanıcının iş mantığı nedeniyle önerilen eylemi görüp görmediğinden emin olduğunuz bir derece çağrıdır. Bu, kişiselleştirici varsayılan Reward ile eğitilmemesi için olayı atbırakmanıza olanak tanır. Etkin olmayan olaylar, Reward API 'sini çağırmamalıdır.

Öğrenme döngüsünün gerçek olay türünü öğrenmesi önemlidir. Etkin olmayan bir olay, bir yeniden çağrıya sahip olmayacaktır. Etkin bir olayın bir yeniden çağrısı olmalıdır ancak API çağrısı hiçbir şekilde yapılmadıysa, varsayılan ödül puanı uygulanır. Kullanıcı deneyimini etkileyeceğini öğrendiğinizde, bir olayın durumunu devre dışı iken etkin olarak değiştirin.

## <a name="typical-active-events-scenario"></a>Tipik etkin olaylar senaryosu

Uygulamanız, derecelendirme API 'sini çağırdığında, uygulamanın **Rewarterctionıd** alanında gösterilmesi gereken eylemi alırsınız.  Bu andan itibaren, kişiselleştirici aynı EventID 'ye sahip bir ödül çağrısı bekler. Ödül puanı, modeli gelecekteki derece çağrılar için eğitebilmek üzere kullanılır. EventID için bir ödül çağrısı alınmıyorsa, varsayılan bir yeniden eşleşme uygulanır. [Varsayılan yeniden ödüller](how-to-settings.md#configure-rewards-for-the-feedback-loop) Azure Portal, kişiselleştirici kaynağınız üzerinde ayarlanır.

## <a name="other-event-type-scenarios"></a>Diğer olay türü senaryoları

Bazı senaryolarda, uygulamanın, sonucun ne şekilde kullanılacağını veya kullanıcıya görüntülendiğini bilse bile, uygulamanın sırasıyla derece çağrısı yapması gerekebilir. Bu durum, örneğin, yükseltilen içeriğin sayfa işleme bir pazarlama kampanyası tarafından üzerine yazılmasından kaynaklanıyor olabilir. Sıra çağrısının sonucu hiç kullanılmadıysa ve Kullanıcı onu hiç görmüyorsa, karşılık gelen bir ödül çağrısı göndermeyin.

Genellikle, şu durumlarda bu senaryolar meydana gelir:

* Kullanıcının görebileceğini veya görmeyebilirsiniz prerendering kullanıcı ARABIRIMI.
* Uygulamanız, çok sayıda gerçek zamanlı bağlamla derecelendirme çağrılarının yapıldığı ve uygulamanın çıktıyı kullanmasının veya kullanmayabilir.

Bu durumlarda, etkinliğin _devre dışı_ olmasını isteyen derece çağrısı yapmak Için kişiselleştirici kullanın. Kişiselleştirici bu olay için bir ödül beklemez ve varsayılan bir ödül uygulamaz.

İş mantığınızdaki daha sonra uygulama, sıralama çağrısından bilgileri kullanıyorsa, olayı _etkinleştirmeniz_ yeterlidir. Etkinlik etkin olduğunda, kişiselleştirici bir olay yeniden bekliyor. Reward API 'sine açık bir çağrı yapılmıyorsa, kişiselleştirici varsayılan bir ödül uygular.

## <a name="inactive-events"></a>Etkin olmayan olaylar

Bir olaya yönelik eğitimi devre dışı bırakmak için kullanarak derecesi çağırın `learningEnabled = False` .

Etkin olmayan bir olay için, EventID için bir ödül gönderirseniz veya `activate` Bu EventID için API 'yi çağırdığınızda öğrenme, örtülü olarak etkinleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Ödül Puanını ve hangi verilerin dikkate alınacağını nasıl belirleyebileceğinizi](concept-rewards.md)öğrenin.
